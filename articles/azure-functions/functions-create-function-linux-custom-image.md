---
title: 在使用自訂映像 (預覽版) 的 Linux 上建立函式 | Microsoft Docs
description: 了解如何建立在自訂 Linux 映像上執行的 Azure Functions。
services: functions
keywords: ''
author: ggailey777
ms.author: glenga
ms.date: 10/19/2018
ms.topic: tutorial
ms.service: azure-functions
ms.custom: mvc
ms.devlang: azure-cli
manager: jeconnoc
ms.openlocfilehash: aa3c72c7ff2aa5e25fbff9fc38c33fd2dda34ecd
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/24/2018
ms.locfileid: "49985075"
---
# <a name="create-a-function-on-linux-using-a-custom-image-preview"></a>在使用自訂映像 (預覽版) 的 Linux 上建立函式

Azure Functions 可讓您在 Linux 的自訂容器中裝載函式。 您也可以[在預設的 Azure App Service 容器上裝載](functions-create-first-azure-function-azure-cli-linux.md)。 這項功能目前為預覽狀態並且需要 [Functions 2.0 執行階段](functions-versions.md)。

在本教學課程中，您將了解如何以自訂 Docker 映像的形式將函式部署到 Azure。 當您需要自訂內建的 App Service 容器映像時，此模式相當有用。 當您的函式需要特定的語言版本，或需要內建映像未提供的特定相依性或設定時，您可能會想使用自訂映像。

本教學課程會引導您使用 Azure Functions Core Tools 在自訂 Linux 映像中建立建立函式。 您會將此映像發佈到 Azure 中使用 Azure CLI 建立的函式應用程式。

在本教學課程中，您了解如何：

> [!div class="checklist"]
> * 使用 Core Tools 建立函式應用程式和 Dockerfile。
> * 使用 Docker 建置自訂映像。
> * 將自訂映像發佈到容器登錄中。
> * 建立 Azure 儲存體帳戶。
> * 建立 Linux App Service 方案。
> * 從 Docker Hub 部署函式應用程式。
> * 將應用程式設定加入函式應用程式。

下列步驟適用於 Mac、Windows 或 Linux 電腦。  

## <a name="prerequisites"></a>必要條件

在執行此範例之前，您必須具備下列項目︰

* 安裝 [Azure Core Tools 2.x 版](functions-run-local.md#v2)。

* 安裝 [Azure CLI]( /cli/azure/install-azure-cli)。 此文章需要 Azure CLI 2.0 版或更新版本。 執行 `az --version` 以尋找您擁有的版本。  
您也可以使用 [Azure Cloud Shell](https://shell.azure.com/bash)。

* 有效的 Azure 訂用帳戶。

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-the-local-function-app-project"></a>建立本機函式應用程式專案

從命令列執行下列命令，以在目前本機目錄的 `MyFunctionProj` 資料夾中建立函式應用程式專案。

```bash
func init MyFunctionProj --docker
```

當您加入 `--docker` 選項時，系統就會為專案產生 dockerfile。 此檔案會用來建立自訂容器以執行專案。 所用基礎映像取決於選擇的背景工作角色執行階段語言。  

當出現提示時，請從下列語言中選擇背景工作角色執行階段：

* `dotnet`：建立 .NET 類別庫專案 (.csproj)。
* `node`：建立 JavaScript 專案。

當命令執行時，您會看到如下輸出：

```output
Writing .gitignore
Writing host.json
Writing local.settings.json
Writing Dockerfile
```

使用下列命令來瀏覽至新的 `MyFunctionProj` 專案資料夾。

```bash
cd MyFunctionProj
```

[!INCLUDE [functions-create-function-core-tools](../../includes/functions-create-function-core-tools.md)]

[!INCLUDE [functions-update-function-code](../../includes/functions-update-function-code.md)]

[!INCLUDE [functions-run-function-test-local](../../includes/functions-run-function-test-local.md)]

## <a name="build-the-image-from-the-docker-file"></a>從 Docker 檔案建立映像

請看一下專案根資料夾中的 Dockerfile。 此檔案描述在 Linux 上執行函式應用程式所需的環境。 下列 Dockerfile 範例會建立容器，用來在 JavaScript (Node.js) 背景工作角色執行階段上執行函式應用程式： 

```docker
FROM mcr.microsoft.com/azure-functions/node:2.0

ENV AzureWebJobsScriptRoot=/home/site/wwwroot
COPY . /home/site/wwwroot
```

> [!NOTE]
> 在私人容器登錄中裝載映像時，您應該使用 Dockerfile 中的 **ENV** 變數將連線設定加入函式應用程式。 本教學課程無法保證您使用私人登錄，因此[在部署後使用 Azure CLI 加入](#configure-the-function-app)連線設定是最佳安全性做法。

### <a name="run-the-build-command"></a>執行 `build` 命令
在根資料夾中，執行 [docker build](https://docs.docker.com/engine/reference/commandline/build/) 命令，然後提供名稱、`mydockerimage` 和標記 (`v1.0.0`)。 將 `<docker-id>` 取代為 Docker Hub 帳戶識別碼。 此命令會建置容器的 Docker 映像。

```bash
docker build --tag <docker-id>/mydockerimage:v1.0.0 .
```

當命令執行時，您會看到如下輸出的內容，這是本案例針對 JavaScript 背景工作角色執行階段執行的結果：

```bash
Sending build context to Docker daemon  17.41kB
Step 1/3 : FROM mcr.microsoft.com/azure-functions/node:2.0
2.0: Pulling from azure-functions/node
802b00ed6f79: Pull complete
44580ea7a636: Pull complete
73eebe8d57f9: Pull complete
3d82a67477c2: Pull complete
8bd51cd50290: Pull complete
7bd755353966: Pull complete
Digest: sha256:480e969821e9befe7c61dda353f63298f2c4b109e13032df5518e92540ea1d08
Status: Downloaded newer image for mcr.microsoft.com/azure-functions/node:2.0
 ---> 7c71671b838f
Step 2/3 : ENV AzureWebJobsScriptRoot=/home/site/wwwroot
 ---> Running in ed1e5809f0b7
Removing intermediate container ed1e5809f0b7
 ---> 39d9c341368a
Step 3/3 : COPY . /home/site/wwwroot
 ---> 5e196215935a
Successfully built 5e196215935a
Successfully tagged <docker-id>/mydockerimage:v1.0.0
```

### <a name="test-the-image-locally"></a>在本機測試映像
要確認建立的映像是否正常運作的方式，是在本機容器中執行 Docker 映像。 發出 [docker run](https://docs.docker.com/engine/reference/commandline/run/) 命令，並將映像的名稱和標記傳遞給它。 請務必使用 `-p` 引數來指定連接埠。

```bash
docker run -p 8080:80 -it <docker-ID>/mydockerimage:v1.0.0
```

在本機 Docker 容器中執行自訂映像，瀏覽至 <http://localhost:8080> 以確認函式應用程式和容器是否正常運作。

![在本機測試函式應用程式。](./media/functions-create-function-linux-custom-image/run-image-local-success.png)

您可以選擇性地再次測試您的函式，這次請在本機容器中使用下列 URL:

`http://localhost:8080/api/myhttptrigger?name=<yourname>`

在容器中驗證過函式應用程式容器之後，停止執行。 現在，您可以將自訂映像推送到 Docker Hub 帳戶。

## <a name="push-the-custom-image-to-docker-hub"></a>將自訂映像推送至 Docker Hub

登錄是裝載映像並提供服務映像和容器服務的應用程式。 您必須將映像推送到登錄，才能共用您的映像。 Docker Hub 是 Docker 映像的登錄，可讓您裝載自己的公用或私人存放庫。

推送映像之前，必須先使用 [docker login](https://docs.docker.com/engine/reference/commandline/login/) 命令登入 Docker Hub。 在命令提示字元中，以您的帳戶名稱取代 `<docker-id>`，並於主控台輸入您的密碼。 如需其他 Docker Hub 密碼選項，請參閱 [docker login 命令文件](https://docs.docker.com/engine/reference/commandline/login/) \(英文\)。

```bash
docker login --username <docker-id>
```

「登入成功」訊息會確認您已登入。 登入之後，使用 [docker push](https://docs.docker.com/engine/reference/commandline/push/) 命令將映像推送到 Docker Hub。

```bash
docker push <docker-id>/mydockerimage:v1.0.0
```

檢查命令的輸出來確認已成功推送。

```bash
The push refers to a repository [docker.io/<docker-id>/mydockerimage:v1.0.0]
24d81eb139bf: Pushed
fd9e998161c9: Mounted from <docker-id>/mydockerimage
e7796c35add2: Mounted from <docker-id>/mydockerimage
ae9a05b85848: Mounted from <docker-id>/mydockerimage
45c86e20670d: Mounted from <docker-id>/mydockerimage
v1.0.0: digest: sha256:be080d80770df71234eb893fbe4d... size: 1796
```

現在，您可以使用此映像作為 Azure 中的新函式應用程式的部署來源。

[!INCLUDE [functions-create-resource-group](../../includes/functions-create-resource-group.md)]

[!INCLUDE [functions-create-storage-account](../../includes/functions-create-storage-account.md)]

## <a name="create-a-linux-app-service-plan"></a>建立 Linux App Service 方案

使用方案目前不支援以 Linux 裝載 Functions。 您必須在 Linux App Service 方案中託管 Linux 容器應用程式。 若要深入了解裝載，請參閱 [Azure Functions 裝載方案比較](functions-scale.md)。

[!INCLUDE [app-service-plan-no-h](../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

## <a name="create-and-deploy-the-custom-image"></a>建立和部署自訂映像

函式應用程式可裝載函式的執行。 使用 [az functionapp create](/cli/azure/functionapp#az-functionapp-create) 命令從 Docker Hub 映像建立函式應用程式。

在下列命令中，使用唯一函式應用程式名稱來替代您看見 `<app_name>` 預留位置的地方，並使用儲存體帳戶名稱來替代 `<storage_name>`。 `<app_name>` 會作為函式應用程式的預設 DNS 網域，所以此名稱在 Azure 的所有應用程式中都必須是唯一的名稱。 如先前所述，`<docker-id>` 是您的 Docker 帳戶名稱。

```azurecli-interactive
az functionapp create --name <app_name> --storage-account  <storage_name>  --resource-group myResourceGroup \
--plan myAppServicePlan --deployment-container-image-name <docker-id>/mydockerimage:v1.0.0
```

建立函式應用程式後，Azure CLI 會顯示類似下列範例的資訊：

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "containerSize": 1536,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "quickstart.azurewebsites.net",
  "enabled": true,
  "enabledHostNames": [
    "quickstart.azurewebsites.net",
    "quickstart.scm.azurewebsites.net"
  ],
   ....
    // Remaining output has been truncated for readability.
}
```

_deployment-container-image-name_ 參數表示裝載於 Docker Hub 上用於建立函式應用程式的映像。

## <a name="configure-the-function-app"></a>設定函式應用程式

此函式需要連接字串以連接到預設儲存體帳戶。 當您將自訂映像發佈至私人容器帳戶時，應使用 [ENV 指令](https://docs.docker.com/engine/reference/builder/#env) \(英文\) 或類似指令將 Dockerfile 中的這些應用程式設定改設為環境變數。

在本例中，`<storage_account>` 是您建立的儲存體帳戶名稱。 使用 [az storage account show-connection-string](/cli/azure/storage/account#show-connection-string) 命令取得連接字串。 使用 [az functionapp config appsettings set](/cli/azure/functionapp/config/appsettings#az-functionapp-config-appsettings-set) 命令，在函式應用程式中新增這些應用程式設定。

```azurecli-interactive
storageConnectionString=$(az storage account show-connection-string \
--resource-group myResourceGroup --name <storage_account> \
--query connectionString --output tsv)

az functionapp config appsettings set --name <function_app> \
--resource-group myResourceGroup \
--settings AzureWebJobsDashboard=$storageConnectionString \
AzureWebJobsStorage=$storageConnectionString
```

您現在可以在 Azure 中測試在 Linux 上執行的函式。

[!INCLUDE [functions-test-function-code](../../includes/functions-test-function-code.md)]

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

## <a name="next-steps"></a>後續步驟

在本教學課程中，您已了解如何：

> [!div class="checklist"]
> * 使用 Core Tools 建立函式應用程式和 Dockerfile。
> * 使用 Docker 建置自訂映像。
> * 將自訂映像發佈到容器登錄中。
> * 建立 Azure 儲存體帳戶。
> * 建立 Linux App Service 方案。
> * 從 Docker Hub 部署函式應用程式。
> * 將應用程式設定加入函式應用程式。

了解如何啟用核心 App Service 平台內建的持續整合功能。 您可以設定函式應用程式，以便當您在 Docker Hub 中更新您的映像時重新部署容器。

> [!div class="nextstepaction"] 
> [使用用於容器的 Web 應用程式進行持續部署](../app-service/containers/app-service-linux-ci-cd.md)
