---
title: 在 Azure Kubernetes Service (AKS) 中建立基本輸入控制器
description: 了解如何在 Azure Kubernetes Service (AKS) 叢集中安裝及設定基本 NGINX 輸入控制器。
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 08/30/2018
ms.author: iainfou
ms.openlocfilehash: fdce7c1f5876442a10fbf77b315e082a0a817dfd
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/25/2018
ms.locfileid: "50093380"
---
# <a name="create-an-ingress-controller-in-azure-kubernetes-service-aks"></a>在 Azure Kubernetes Service (AKS) 中建立輸入控制器

輸入控制器是一項可為 Kubernetes 服務提供反向 Proxy、可設定的流量路由和 TLS 終止的軟體。 Kubernetes 輸入資源可用來設定個別 Kubernetes 服務的輸入規則和路由。 透過輸入控制器和輸入規則，您可以使用單一 IP 位址將流量路由至 Kubernetes 叢集中的多個服務。

本文示範如何在 Azure Kubernetes Service (AKS) 叢集中部署 [NGINX 輸入控制器][nginx-ingress]。 然後，會有兩個應用程式在 AKS 叢集中執行，且均可透過單一 IP 位址來存取。

您也可以：

- [啟用 HTTP 應用程式路由附加元件][aks-http-app-routing]
- [建立使用內部私人網路和 IP 位址的輸入控制器][aks-ingress-internal]
- [建立使用自有 TLS 憑證的輸入控制器][aks-ingress-own-tls]
- 建立輸入控制器，其使用 Let's Encrypt 自動產生[具有動態公用 IP][aks-ingress-tls] 或[具有靜態公用 IP 位址][aks-ingress-static-tls]的 TLS 憑證

## <a name="before-you-begin"></a>開始之前

本文使用 Helm 來安裝 NGINX 輸入控制器、cert-manager 及範例 Web 應用程式。 您需要在 AKS 叢集內將 Helm 初始化，並使用適用於 Tiller 的服務帳戶。 如需設定及使用 Helm 的詳細資訊，請參閱[在 Azure Kubernetes Service (AKS) 中使用 Helm 安裝應用程式][use-helm]。

本文也需要您執行 Azure CLI 2.0.41 版或更新版本。 執行 `az --version` 以尋找版本。 如果您需要安裝或升級，請參閱[安裝 Azure CLI][azure-cli-install]。

## <a name="create-an-ingress-controller"></a>建立輸入控制器

若要建立輸入控制器，請使用 `Helm` 以安裝 *nginx-ingress*。 為了新增備援，您必須使用 `--set controller.replicaCount` 參數部署兩個 NGINX 輸入控制器複本。 為充分享有執行輸入控制器複本的好處，請確定 AKS 叢集中有多個節點。

> [!TIP]
> 下列範例會在 `kube-system` 命名空間中安裝輸入控制器。 您可以視需要為自己的環境指定不同的命名空間。 如果您的 AKS 叢集未啟用 RBAC，請將 `--set rbac.create=false` 新增至命令。

```console
helm install stable/nginx-ingress --namespace kube-system --set controller.replicaCount=2
```

為 NGINX 輸入控制器建立 Kubernetes 負載平衡器服務時，系統會指派動態公用 IP 位址，如下列範例輸出所示：

```
$ kubectl get service -l app=nginx-ingress --namespace kube-system

NAME                                         TYPE           CLUSTER-IP    EXTERNAL-IP   PORT(S)                      AGE
masked-otter-nginx-ingress-controller        LoadBalancer   10.0.92.99    40.117.74.8   80:31077/TCP,443:32592/TCP   7m
masked-otter-nginx-ingress-default-backend   ClusterIP      10.0.46.106   <none>        80/TCP                       7m
```

尚未建立任何輸入規則，因此，如果您瀏覽至內部 IP 位址，將會顯示 NGINX 輸入控制器的預設 404 頁面。 輸入規則會於下列步驟中進行設定。

## <a name="run-demo-applications"></a>執行示範應用程式

為了檢視運作中的輸入控制器，我們將在 AKS 叢集中執行兩個示範應用程式。 在此範例中，會使用 Helm 來部署簡單 'Hello world' 應用程式的兩個執行個體。

在您可以安裝範例 Helm 圖表之前，先將 Azure 範例存放庫新增至您的 Helm 環境，如下所示：

```console
helm repo add azure-samples https://azure-samples.github.io/helm-charts/
```

使用下列命令，從 Helm 圖表建立第一個示範應用程式：

```console
helm install azure-samples/aks-helloworld
```

現在，請安裝示範應用程式的第二個執行個體。 對第二個執行個體指定新的標題，以便在視覺上區分這兩個應用程式。 您也要指定唯一的服務名稱：

```console
helm install azure-samples/aks-helloworld --set title="AKS Ingress Demo" --set serviceName="ingress-demo"
```

## <a name="create-an-ingress-route"></a>建立輸入路由

這兩個應用程式現在都已在您的 Kubernetes 叢集上執行。 若要將流量路由至每個應用程式，請建立 Kubernetes 輸入資源。 輸入資源會設定將流量路由至這兩個應用程式之一的規則。

在下列範例中，傳至位址 `http://40.117.74.8/` 的流量會路由傳送至名為 `aks-helloworld` 的服務。 傳至位址 `http://40.117.74.8/hello-world-two` 的流量會路由至 `ingress-demo` 服務。

建立名為 `hello-world-ingress.yaml` 的檔案，並複製到下列範例 YAML 中。

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: hello-world-ingress
  annotations:
    kubernetes.io/ingress.class: nginx
    nginx.ingress.kubernetes.io/ssl-redirect: "false"
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
  - http:
      paths:
      - path: /
        backend:
          serviceName: aks-helloworld
          servicePort: 80
      - path: /hello-world-two
        backend:
          serviceName: ingress-demo
          servicePort: 80
```

使用 `kubectl apply -f hello-world-ingress.yaml` 命令建立輸入資源。

```
$ kubectl apply -f hello-world-ingress.yaml

ingress.extensions/hello-world-ingress created
```

## <a name="test-the-ingress-controller"></a>測試輸入控制器

若要測試輸入控制器的路由，請瀏覽至這兩個應用程式。 開啟網頁瀏覽器並前往您 NGINX 輸入控制器的 IP 位址，例如 *http://40.117.74.8*。 第一個示範應用程式會顯示在網頁瀏覽器中，如下列範例所示：

![執行於輸入控制器後方的第一個應用程式](media/ingress-basic/app-one.png)

現在，將 */hello-world-two* 路徑新增至 IP 位址，例如 *http://40.117.74.8/hello-world-two*。 此時會顯示具有自訂標題的第二個示範應用程式：

![執行於輸入控制器後方的第二個應用程式](media/ingress-basic/app-two.png)

## <a name="clean-up-resources"></a>清除資源

本文使用 Helm 來安裝輸入元件和範例應用程式。 部署 Helm 圖表時會建立一些 Kubernetes 資源。 這些資源包含 Pod、部署和服務。 若要清除這些資源，請先使用 `helm list` 命令列出 Helm 版本。 尋找名為nginx-ingress 和 aks-helloworld 的圖表，如下列範例輸出所示：

```
$ helm list

NAME                REVISION    UPDATED                     STATUS      CHART                   APP VERSION NAMESPACE
gilded-duck         1           Tue Oct 16 16:52:25 2018    DEPLOYED    nginx-ingress-0.22.1    0.15.0      kube-system
righteous-numbat    1           Tue Oct 16 16:53:53 2018    DEPLOYED    aks-helloworld-0.1.0                default
looming-moth        1           Tue Oct 16 16:53:59 2018    DEPLOYED    aks-helloworld-0.1.0                default
```

使用 `helm delete` 命令刪除版本。 下列範例會刪除 NGINX 輸入部署和兩個範例 AKS hello world 應用程式。

```
$ helm delete gilded-duck righteous-numbat looming-moth

release "gilded-duck" deleted
release "righteous-numbat" deleted
release "looming-moth" deleted
```

接下來，移除 AKS hello world 應用程式的 Helm 存放庫：

```console
helm repo remove azure-samples
```

最後，移除將流量導向範例應用程式的輸入路由：

```console
kubectl delete -f hello-world-ingress.yaml
```

## <a name="next-steps"></a>後續步驟

本文包含 AKS 的一些外部元件。 若要深入了解這些元件，請參閱下列專案頁面：

- [Helm CLI][helm-cli]
- [NGINX 輸入控制器][nginx-ingress]

您也可以：

- [啟用 HTTP 應用程式路由附加元件][aks-http-app-routing]
- [建立使用內部私人網路和 IP 位址的輸入控制器][aks-ingress-internal]
- [建立使用自有 TLS 憑證的輸入控制器][aks-ingress-own-tls]
- 建立輸入控制器，其使用 Let's Encrypt 自動產生[具有動態公用 IP][aks-ingress-tls] 或[具有靜態公用 IP 位址][aks-ingress-static-tls]的 TLS 憑證

<!-- LINKS - external -->
[helm-cli]: https://docs.microsoft.com/azure/aks/kubernetes-helm#install-helm-cli
[nginx-ingress]: https://github.com/kubernetes/ingress-nginx

<!-- LINKS - internal -->
[use-helm]: kubernetes-helm.md
[azure-cli-install]: /cli/azure/install-azure-cli
[aks-ingress-internal]: ingress-internal-ip.md
[aks-ingress-tls]: ingress-tls.md
[aks-ingress-static-tls]: ingress-static-ip.md
[aks-http-app-routing]: http-application-routing.md
[aks-ingress-own-tls]: ingress-own-tls.md
