---
title: 註冊您的應用程式 - 自訂決策服務
titlesuffix: Azure Cognitive Services
description: 逐步說明如何使用 Azure 自訂決策服務註冊新應用程式。
services: cognitive-services
author: slivkins
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-decision-service
ms.topic: conceptual
ms.date: 05/09/2018
ms.author: slivkins
ms.reviewer: marcozo
ms.openlocfilehash: 598300597856d858095ff7c2e2cf9e9264190a9d
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/19/2018
ms.locfileid: "46365395"
---
# <a name="register-your-application"></a>註冊您的應用程式

若要為您的應用程式使用自訂決策服務，請在入口網站上進行註冊。 本文說明執行方法。

1. 移至自訂決策服務的[首頁](https://ds.microsoft.com/)。 在功能區中，按一下 [我的入口網站]，如映像中反白顯示的部分：

    ![我的入口網站](./media/portal.png)

    如果您尚未登入，入口網站會提示您使用 [Microsoft 帳戶](https://account.microsoft.com/account)登入。 登入之後，入口網站會在頁面右上角顯示您的 Microsoft 帳戶。

2. 若要註冊您的應用程式，請按一下 [新應用程式] 按鈕。

3. 在對話方塊中，選擇您應用程式的「應用程式識別碼」。 自訂決策服務要求每個應用程式只能有一個唯一識別碼。 如果其他人已使用此識別碼，系統會要求您選取另一個識別碼。

4. 指定動作集合 API。 這項設定是 RSS 或 Atom 摘要，用於將應用程式的可用內容傳遞至自訂決策服務。 輸入摘要的名稱，然後輸入提供該摘要的 URL。 若要稍後執行此步驟，請按一下 [摘要] 按鈕，然後按一下 [新摘要] 按鈕。 稍後會說明建立 RSS 摘要的範例。

5. 若要註冊您的應用程式，請選取左下角的 [自訂應用程式] 核取方塊。 為記錄應用程式資料的 Azure 儲存體帳戶輸入[連接字串](../../storage/common/storage-configure-connection-string.md)。 如需如何建立儲存體帳戶的詳細資訊，請參閱[如何建立、管理或刪除儲存體帳戶](../../storage/common/storage-create-storage-account.md)。

### <a name="next-steps"></a>後續步驟

* 開始最佳化[網頁](custom-decision-service-get-started-browser.md)或[智慧型手機應用程式](custom-decision-service-get-started-app.md)。
* 完成[教學課程](custom-decision-service-tutorial-news.md)以了解更深入的範例。
* 請參閱 [API 參考](custom-decision-service-api-reference.md)以深入了解提供的功能。