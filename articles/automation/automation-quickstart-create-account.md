---
title: Azure 快速入門 - 建立 Azure 自動化帳戶 | Microsoft Docs
description: 了解如何建立 Azure 自動化帳戶及執行 Runbook
services: automation
author: csand-msft
ms.author: csand
ms.date: 10/18/2018
ms.topic: quickstart
ms.service: automation
ms.component: process-automation
ms.custom: mvc
ms.openlocfilehash: 6d473d8874208297e905a5d90a08ffd870ffa8a6
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/25/2018
ms.locfileid: "50091819"
---
# <a name="create-an-azure-automation-account"></a>建立 Azure 自動化帳戶

您可以透過 Azure 建立 Azure 自動化帳戶。 此方法可提供以瀏覽器為基礎的使用者介面，從而建立和設定自動化帳戶以及相關的資源。 本快速入門會逐步引導您建立自動化帳戶，並在帳戶中執行 Runbook。

如果您沒有 Azure 訂用帳戶，請在開始前建立[免費 Azure 帳戶](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)。

## <a name="sign-in-to-azure"></a>登入 Azure

在 https://portal.azure.com 登入 Azure

## <a name="create-automation-account"></a>建立自動化帳戶

1. 按一下 Azure 左上角的 [建立資源] 按鈕。

1. 選取 [管理工具]，然後選取 [自動化]。

1. 輸入帳戶資訊。 針對 [建立 Azure 執行身分帳戶] 選擇 [是]，就會自動啟用簡化對 Azure 進行驗證的構件。 請務必注意，在建立自動化帳戶時，名稱在選擇後即不可變更。 一個「自動化帳戶」可以管理所指定租用戶之所有區域和訂用帳戶的資源。 完成後，按一下 [建立] 可啟動自動化帳戶部署。

    ![在頁面中輸入您自動化帳戶的相關資訊](./media/automation-quickstart-create-account/create-automation-account-portal-blade.png)  

    > [!NOTE]
    > 如需可部署自動化帳戶之處的更新清單，請參閱[依區域提供的產品](https://azure.microsoft.com/en-us/global-infrastructure/services/?products=automation&regions=all)。

1. 在部署完成後，按一下 ** [所有服務]，選取 [自動化帳戶]，然後選取您所建立的自動化帳戶。

    ![自動化帳戶概觀](./media/automation-quickstart-create-account/automation-account-overview.png)

## <a name="run-a-runbook"></a>執行 Runbook

執行其中一個教學課程 Runbook。

1. 按一下 [程序自動化] 下的 [Runbook]。 隨即顯示 Runbook 的清單。 根據預設，會啟用帳戶中的幾個教學課程 Runbook。

    ![自動化帳戶 Runbook 清單](./media/automation-quickstart-create-account/automation-runbooks-overview.png)

1. 選取 **AzureAutomationTutorialScript** Runbook。 這個動作會開啟 [Runbook 概觀] 頁面。

    ![Runbook 概觀](./media/automation-quickstart-create-account/automation-tutorial-script-runbook-overview.png)

1. 按一下 [啟動]，然後在 [啟動 Runbook] 頁面上，按一下 [確定] 來啟動 Runbook。

    ![Runbook 作業頁面](./media/automation-quickstart-create-account/automation-tutorial-script-job.png)

1. [作業狀態] 變成 [執行] 之後，按一下 [輸出] 或 [所有記錄] 可檢視 Runbook 作業輸出。 在本教學課程的 Runbook 中，輸出就是您 Azure 資源的清單。

## <a name="clean-up-resources"></a>清除資源

若不再需要，可刪除資源群組、自動化帳戶和所有相關資源。 若要這樣做，請選取自動化帳戶的資源群組，然後按一下 [刪除]。

## <a name="next-steps"></a>後續步驟

在本快速入門中，您已部署自動化帳戶、啟動 Runbook 作業，並檢視作業結果。 若要深入了解 Azure 自動化，請繼續閱讀快速入門來建立您的第一個 Runbook。

> [!div class="nextstepaction"]
> [自動化快速入門 - 建立 Runbook](./automation-quickstart-create-runbook.md)
