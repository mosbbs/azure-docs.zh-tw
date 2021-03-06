---
author: PatAltimore
ms.service: active-directory-b2c
ms.topic: include
ms.date: 11/03/2016
ms.author: patricka
ms.openlocfilehash: 19e7c919345c0f56b274737840f8150f7d710501
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/25/2018
ms.locfileid: "50133171"
---
如果您只想在您的應用程式中啟用登入功能，您可以使用**登入**原則。 此原則描述客戶在登入期間將經歷的體驗，以及成功登入時，應用程式將收到的權杖內容。

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](active-directory-b2c-portal-navigate-b2c-service.md)]
按一下 [登入原則] 。

按一下刀鋒視窗頂端的 [新增]  。

[名稱]  決定您的應用程式所使用的登入原則名稱。 例如，輸入 **SiIn**。

按一下 [識別提供者] 並選取 [本機帳戶登入]。 (選擇性) 您也可以選取社交身分識別提供者 (如果已經設定)。 按一下 [確定]。

按一下 [應用程式宣告] 。 您在這裡選擇成功登入後，您要在權杖中傳回給應用程式的宣告。 例如，選取 [顯示名稱]、[識別提供者]、[郵遞區號] 和 [使用者的物件識別碼]。 按一下 [確定]。

按一下頁面底部的 [新增] 。 請注意，剛建立的原則會在 [登入原則] 刀鋒視窗中顯示為 **B2C_1_SiIn** (自動新增 **B2C\_1\_** 部分)。

按一下 [B2C_1_SiIn] 以開啟原則。

在 [應用程式] 下拉式清單中選取 [Contoso B2C 應用程式]，在 [回覆 URL / 重新導向 URI] 下拉式清單中選取 `https://localhost:44321/`。

按一下 [立即執行] 。 新的瀏覽器索引標籤隨即開啟，您可以開始進行取用者體驗來登入您的應用程式。

> [!NOTE]
> 建立和更新原則後，需要經過一分鐘才會生效。
>