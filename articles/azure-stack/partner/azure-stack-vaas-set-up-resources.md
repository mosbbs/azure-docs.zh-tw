---
title: 教學課程 - 設定驗證即服務的資源 | Microsoft Docs
description: 在本教學課程中，請了解如何設定驗證即服務的資源。
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/19/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: d9406032a55c0bbd73bf16ae2f0fa272dddd7698
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/23/2018
ms.locfileid: "49650869"
---
# <a name="tutorial-set-up-resources-for-validation-as-a-service"></a>教學課程：設定驗證即服務的資源

[!INCLUDE[Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

您必須建立解決方案。 驗證即服務 (VaaS) 解決方案代表特定硬體用料表的 Azure Stack 解決方案。 您可以使用此解決方案檢查您的硬體是否支援執行的 Azure Stack。 請遵循本教學課程，做好將此服務用於解決方案的準備。

在本教學課程中，您了解如何：

> [!div class="checklist"]
> * 設定 Azure AD (Azure AD) 執行個體以準備使用 VaaS。
> * 建立儲存體帳戶。

## <a name="configure-an-azure-ad-tenant"></a>設定 Azure AD 租用戶

必須具有 Azure AD 租用戶，才能使用 VaaS 進行驗證和註冊。 合作夥伴會使用租用戶的角色型存取控制 (RBAC) 功能，來管理合作夥伴組織中有誰可以使用 VaaS。

請註冊您的組織 Azure AD 租用戶目錄 (而不是用於 Azure Stack 的 Azure AD 租用戶目錄)，並建立用來管理使用者帳戶的原則。 如需詳細資訊，請參閱[管理 Azure Active Directory 租用戶](https://docs.microsoft.com/azure/active-directory/active-directory-administer)。

### <a name="create-a-tenant"></a>建立租用戶

請建立專門用於 VaaS 且具有描述性名稱 (例如 `ContosoVaaS@onmicrosoft.com`) 的租用戶。

1. 在 [Azure 入口網站](https://portal.azure.com)中建立 Azure AD 租用戶，或使用現有的租用戶。 <!-- For instructions on creating new Azure AD tenants, see [Get started with Azure AD](https://docs.microsoft.com/azure/active-directory/get-started-azure-ad). -->

2. 將您的組織成員新增至租用戶。 這些使用者將負責使用服務來檢視或排程測試。 完成註冊之後，您會定義使用者的存取層級。
 
    請指派下列其中一個角色，為您租用戶中的使用者授與在 VaaS 中執行動作的權限：

    | 角色名稱 | 說明 |
    |---------------------|------------------------------------------|
    | 擁有者 | 具有所有資源的完整存取權。 |
    | 讀取者 | 可檢視所有資源，但是無法建立或管理。 |
    | 測試參與者 | 可以建立及管理測試資源。 |

    若要在 **Azure Stack 驗證服務**應用程式中指派角色：

    1. 登入 [Azure 入口網站](https://portal.azure.com)。
    2. 在 [身分識別] 區段下，選取 [所有服務] > [Azure Active Directory]。
    3. 選取 [企業應用程式] > [Azure Stack 驗證服務] 應用程式。
    4. 選取 [使用者和群組]。 [Azure Stack 驗證服務 - 使用者和群組] 刀鋒視窗會列出具有使用應用程式權限的使用者。
    5. 選取 [+ 新增使用者]，以從您的租用戶新增使用者並指派角色。
   
    如果您想要區隔組織內不同群組間的 VaaS 資源和動作，您可以建立多個 Azure AD 租用戶目錄。

### <a name="register-your-tenant"></a>註冊您的租用戶

此程序會為您的租用戶授與 **Azure Stack 驗證服務** Azure AD 應用程式的權限。

1. 將租用戶的下列相關資訊傳送至 Microsoft：[vaashelp@microsoft.com](mailto:vaashelp@microsoft.com)。

    | 資料 | 說明 |
    |--------------------------------|---------------------------------------------------------------------------------------------|
    | 組織名稱 | 官方組織名稱。 |
    | Azure AD 租用戶目錄名稱 | 要註冊的 Azure AD 租用戶目錄名稱。 |
    | Azure AD 租用戶目錄識別碼 | 與目錄相關聯的 Azure AD 租用戶目錄 GUID。 如需如何尋找 Azure AD 租用戶目錄識別碼的詳細資訊，請參閱[取得租用戶識別碼](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal#get-tenant-id)。 |

2. 等候 Azure Stack 驗證小組向您確認您的租用戶可以使用 VaaS 入口網站。

### <a name="consent-to-the-vaas-application"></a>同意 VaaS 應用程式

以 Azure AD 管理員的身分，代表您的租用戶對 VaaS Azure AD 應用程式提供必要的權限：

1. 使用租用戶的全域管理員認證，來登入 [VaaS 入口網站](https://azurestackvalidation.com/)。 

2. 選取 [我的帳戶]。

3. 在系統提示您為 VaaS 授與所列的 Azure AD 權限時，接受條款以繼續操作。

## <a name="create-an-azure-storage-account"></a>建立 Azure 儲存體帳戶

在測試執行期間，VaaS 會將診斷記錄輸出至 Azure 儲存體帳戶。 除了測試記錄以外，儲存體帳戶也可用來上傳套件驗證工作流程的 OEM 延伸模組套件。

Azure 儲存體帳戶會裝載於 Azure 公用雲端中，而不是您的 Azure Stack 環境。

1. 在 Azure 入口網站中，選取 [所有服務] > [儲存體] > [儲存體帳戶]。 在 [儲存體帳戶] 刀鋒視窗上，選取 [新增]。

2. 選取要在其中建立儲存體帳戶的訂用帳戶。

3. 在 [資源群組] 下方，選取 [新建]。 為新的資源群組輸入名稱。

4. 輸入儲存體帳戶的名稱。 您選擇的名稱必須：
    - 在 Azure 中具有唯一性
    - 介於 3 到 24 個字元之間
    - 只包含數字和小寫字母

5. 為您的儲存體帳戶選取 [美國西部] 區域。

    若要確保儲存記錄不會產生網路費用，您可以將 Azure 儲存體帳戶設定為僅使用**美國西部**區域。 這項資料不需要使用資料複寫和經常性存取儲存層功能。 啟用任一功能都將大幅增加您的成本。

6. 將 [帳戶類型] 以外的設定保留為預設值：

    - [部署模型] 欄位依預設會設定為 [Resource Manager]。
    - [效能] 欄位預設會設定為 [標準]。
    - 在 [帳戶類型] 欄位中選取 [Blob 儲存體]。
    - [複寫] 欄位依預設會設定為 [本地備援儲存體 (LRS)]。
    - [存取層] 預設會設定為 [經常性存取層]。

7. 按一下 [檢閱 + 建立]，以檢閱您的儲存體帳戶設定並建立帳戶。

## <a name="next-steps"></a>後續步驟

如果您的環境不允許輸入連線，請遵循部署本機代理程式以對硬體執行測試的教學課程。

> [!div class="nextstepaction"]
> [部署本機代理程式](azure-stack-vaas-local-agent.md)