---
title: 包含檔案
description: 包含檔案
services: iot-edge
author: kgremban
ms.service: iot-edge
ms.topic: include
ms.date: 08/14/2018
ms.author: kgremban
ms.custom: include file
ms.openlocfilehash: 983c65ba6e8b87f1dd66fcfdb50eac088ffab5d0
ms.sourcegitcommit: 9e179a577533ab3b2c0c7a4899ae13a7a0d5252b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/23/2018
ms.locfileid: "49960184"
---
Azure IoT Edge 的主要功能之一，是能夠從雲端將模組部署到您的 IoT Edge 裝置。 IoT Edge 模組是實作為容器的可執行檔套件。 在本節中，您部署的模組會產生模擬裝置的遙測。

1. 在 Azure 入口網站中，瀏覽至您的 IoT 中樞。
1. 移至 [自動裝置管理] 下的 [IoT Edge]，然後選取您的 IoT Edge 裝置。
1. 選取 [設定模組]。
1. 在的 [新增模組] 步驟的 [部署模組] 區段中按一下 [新增]，然後選取 [IoT Edge 模組]。
1. 在 [名稱] 欄位中，輸入 `tempSensor`。
1. 在 [映像 URI] 欄位中，輸入 `mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0`。
1. 其他設定保留不變，然後選取 [儲存]。

   ![在輸入名稱和影像 URI 之後儲存 IoT Edge 模組](./media/iot-edge-deploy-module/name-image.png)

1. 回到 [新增模組] 步驟中，選取 [下一步]。
1. 在 [指定路由] 步驟中，您應該會有可將所有模組的所有訊息傳送至 IoT 中樞的預設路由。 如果沒有，請新增下列程式碼，然後選取 [下一步]。

   ```json
   {
       "routes": {
           "route": "FROM /messages/* INTO $upstream"
       }
   }
   ```

1. 在 [檢閱部署] 步驟中，選取 [提交]。
1. 返回裝置的詳細資料頁面，選取 [重新整理]。 除了您第一次啟動服務時所建立的 edgeAgent 模組以外，您應該還會看到名為 **edgeHub** 的另一個執行階段模組和 **tempSensor** 模組均列出。

   ![在已部署的模組清單中檢視 tempSensor](./media/iot-edge-deploy-module/deployed-modules.png)
