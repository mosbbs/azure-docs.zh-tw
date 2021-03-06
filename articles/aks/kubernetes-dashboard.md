---
title: 使用 Web 儀表板來管理 Azure Kubernetes Service 叢集
description: 了解如何使用內建的 Kubernetes Web UI 儀表板來管理 Azure Kubernetes Service (AKS) 叢集
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 10/08/2018
ms.author: iainfou
ms.openlocfilehash: 9d953cdb82412c07fe0ed4bef75dece4a929cad9
ms.sourcegitcommit: d372d75558fc7be78b1a4b42b4245f40f213018c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/09/2018
ms.locfileid: "51299124"
---
# <a name="access-the-kubernetes-web-dashboard-in-azure-kubernetes-service-aks"></a>存取 Azure Kubernetes Service (AKS) 中的 Kubernetes Web 儀表板

Kubernetes 包含的 Web 儀表板可用來執行基本的管理作業。 此儀表板可讓您檢視您應用程式的基本健全狀況狀態和計量、建立和部署服務，以及編輯現有的應用程式。 本文將說明如何使用 Azure CLI 來存取 Kubernetes 儀表板，然後引導您完成一些基本的儀表板作業。

如需 Kubernetes 儀表板的詳細資訊，請參閱 [Kubernetes Web UI 儀表板][kubernetes-dashboard]。

## <a name="before-you-begin"></a>開始之前

本文件中詳述的步驟假設您已建立 AKS 叢集，並建立與叢集的 `kubectl` 連線。 如果您需要建立 AKS 叢集，請參閱 [AKS 快速入門][aks-quickstart]。

您也必須安裝並設定 Azure CLI 版本 2.0.46 或更新版本。 執行  `az --version`  以尋找版本。 如果您需要安裝或升級，請參閱 [安裝 Azure CLI][install-azure-cli]。

## <a name="start-the-kubernetes-dashboard"></a>啟動 Kubernetes 儀表板

若要啟動 Kubernetes 儀表板，請使用 [az aks browse][az-aks-browse] 命令。 下列範例會在名為 myResourceGroup 的資源群組中，針對名為 myAKSCluster 的叢集開啟儀表：

```azurecli
az aks browse --resource-group myResourceGroup --name myAKSCluster
```

此命令會在您的開發系統與 Kubernetes API 之間建立 Proxy，並開啟 Kubernetes 儀表板的網頁瀏覽器。 如果 Web 瀏覽器未開啟至 Kubernetes 儀表板，請複製並貼上 Azure CLI 中所註明的 URL 位址，通常是 *http://127.0.0.1:8001*。

![Kubernetes Web 儀表板的概觀頁面](./media/kubernetes-dashboard/dashboard-overview.png)

### <a name="for-rbac-enabled-clusters"></a>對於已啟用 RBAC 的叢集

如果您的 AKS 叢集已使用 RBAC，必須先建立 ClusterRoleBinding，才能正確地存取儀表板。 根據預設，Kubernetes 儀表板會部署最低限度的讀取權限，並且會顯示 RBAC 存取錯誤。 Kubernetes 儀表板目前不支援透過使用者提供的認證，來決定存取層級，而是使用授與服務帳戶的角色。 叢集系統管理員可選擇將其他存取權授與 kubernetes-dashboard 服務帳戶，但這可能是權限提高的向量。 您也可以整合 Azure Active Directory 驗證，以提供更細微的存取層級。

若要建立繫結，請使用 [kubectl create clusterrolebinding][kubectl-create-clusterrolebinding] 命令，如下列範例所示。 

> [!WARNING]
> 此範例繫結不會套用任何額外的驗證元件，並且可能導致不安全的使用方式。 可存取此 URL 的任何人都可使用 Kubernetes 儀表板。 請不要讓 Kubernetes 儀表板可公開使用。
>
> 如需有關使用不同驗證方法的詳細資訊，請參閱 Kubernetes 儀表板 Wiki 上的[存取控制][dashboard-authentication]。

```console
kubectl create clusterrolebinding kubernetes-dashboard --clusterrole=cluster-admin --serviceaccount=kube-system:kubernetes-dashboard
```

您現在可以在啟用 RBAC 的叢集中存取 Kubernetes 儀表板。 若要啟動 Kubernetes 儀表板，請使用 [az aks browse][az-aks-browse] 命令，如前面步驟所述。

## <a name="create-an-application"></a>建立應用程式

為了了解 Kubernetes 儀表板如何降低管理工作的複雜性，我們將建立一個應用程式。 您可以從 Kubernetes 儀表板提供文字輸入、YAML 檔案或透過圖形化精靈來建立應用程式。

若要建立應用程式，請完成下列步驟：

1. 選取右上方視窗中的 [建立] 按鈕。
1. 若要使用圖形化精靈，請選擇 [建立應用程式]。
1. 為部署提供名稱，例如 *nginx*
1. 輸入要使用之容器映像的名稱，例如 *nginx:1.15.5*
1. 若要公開連接埠 80 以供 Web 流量使用，您需建立 Kubernetes 服務。 在 [服務] 底下，選取 [外部]，然後針對連接埠和目標連接埠輸入 **80**。
1. 準備就緒時，選取 [部署] 來建立應用程式。

![在 Kubernetes Web 儀表板中建立應用程式](./media/kubernetes-dashboard/create-app.png)

將公用外部 IP 位址指派給 Kubernetes 服務需要一兩分鐘的時間。 在左側的 [探索與負載平衡] 底下，選取 [服務]。 其中會列出您的應用程式 (包括 [外部端點])，如以下範例所示：

![檢視服務與端點清單](./media/kubernetes-dashboard/view-services.png)

選取端點位址以將網頁瀏覽器視窗開啟至預設的 NGINX 頁面：

![檢視所部署應用程式的預設 NGINX 頁面](./media/kubernetes-dashboard/default-nginx.png)

## <a name="view-pod-information"></a>檢視 Pod 資訊

Kubernetes 儀表板可以提供基本監視計量，以及針對資訊 (例如記錄) 進行疑難排解。

若要查看有關您應用程式 Pod 的詳細資訊，請選取左側功能表中的 [Pod]。 隨即會顯示可用的 Pod 清單。 請選擇您的 *nginx* Pod 以檢視資訊，例如資源耗用量：

![檢視 Pod 資訊](./media/kubernetes-dashboard/view-pod-info.png)

## <a name="edit-the-application"></a>編輯應用程式

除建立和檢視應用程式之外，Kubernetes 儀表板還可用來編輯和更新應用程式部署。 為了為應用程式提供額外的備援，我們將增加 NGINX 複本的數目。

編輯部署：

1. 選取左側功能表中的 [部署]，然後選擇您的 *nginx* 部署。
1. 選取右上方導覽列中的 [編輯]。
1. 找出 `spec.replica` 值 (大約在第 20 行)。 若要增加應用程式複本的數目，請將此值從 *1* 變更為 *3*。
1. 就緒後請選取 [更新]。

![編輯部署以更新複本數目](./media/kubernetes-dashboard/edit-deployment.png)

在複本集內建立新 Pod 需要一些時間。 請在左側功能表上，選擇 [複本集]，然後選擇您的 *nginx* 複本集。 Pod 清單現在會反映已更新的複本計數，如以下範例輸出所示：

![檢視複本集的相關資訊](./media/kubernetes-dashboard/view-replica-set.png)

## <a name="next-steps"></a>後續步驟

如需有關 Kubernetes 儀表板的詳細資訊，請參閱 [Kubernetes Web UI 儀表板][kubernetes-dashboard]。

<!-- LINKS - external -->
[kubernetes-dashboard]: https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/
[dashboard-authentication]: https://github.com/kubernetes/dashboard/wiki/Access-control
[kubectl-create-clusterrolebinding]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#-em-clusterrolebinding-em-
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply

<!-- LINKS - internal -->
[aks-quickstart]: ./kubernetes-walkthrough.md
[install-azure-cli]: /cli/azure/install-azure-cli
[az-aks-browse]: /cli/azure/aks#az-aks-browse
