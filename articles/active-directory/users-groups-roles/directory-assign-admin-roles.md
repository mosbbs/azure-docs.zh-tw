---
title: Azure Active Directory 中的系統管理員角色權限 | Microsoft Docs
description: 系統管理員角色可以新增使用者、指派系統管理角色、重設使用者密碼、管理使用者授權或管理網域。
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 10/26/2018
ms.author: curtand
ms.reviewer: vincesm
ms.custom: it-pro
ms.openlocfilehash: 28f06efdd990e46eaa84b1fe26ed5d8944971505
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/26/2018
ms.locfileid: "50156913"
---
# <a name="administrator-role-permissions-in-azure-active-directory"></a>Azure Active Directory 中的系統管理員角色權限

使用 Azure Active Directory (Azure AD) 時，您可以指定個別的系統管理員來執行不同的功能。 您可以在 Azure AD 入口網站中指定系統管理員以執行多種工作，例如新增或變更使用者、指派系統管理角色、重設使用者密碼、管理使用者授權，以及管理網域名稱等。

全域管理員可以存取所有系統管理功能。 註冊 Azure 訂用帳戶的人員預設會獲指派目錄的全域管理員角色。 只有全域管理員才能委派系統管理員角色。

## <a name="assign-or-remove-administrator-roles"></a>指派或移除系統管理員角色

若要了解如何將系統管理角色指派給 Azure Active Directory 中的使用者，請參閱[在 Azure Active Directory 中檢視和指派系統管理員角色](directory-manage-roles-portal.md)。

## <a name="available-roles"></a>可用的角色

可用的系統管理員角色如下：

* **[應用程式系統管理員](#application-administrator)**：此角色中的使用者可以建立和管理企業應用程式、應用程式註冊和應用程式 Proxy 設定的所有層面。 此角色也會授與能力來同意委派的權限以及 Microsoft Graph 和 Azure AD Graph 以外的應用程式權限。 建立新的應用程式註冊或企業應用程式時，不會加入此角色的成員作為擁有者。

  <b>重要</b>：此角色授與管理應用程式認證的能力。 獲指派此角色的使用者可以將認證新增至應用程式，並使用這些認證來模擬應用程式的身分識別。 如果應用程式的身分識別已授與 Azure Active Directory 的存取權，例如建立或更新使用者或其他物件的能力，則獲指派這個角色的使用者可以在模擬應用程式時執行這些動作。 模擬應用程式身分識別的這項能力，對於使用者透過 Azure AD 中角色指派所能執行的動作，是一種權限提高。 請務必了解，將應用程式系統管理員角色指派給使用者，會給予他們模擬應用程式身分識別的能力。

* **[應用程式開發人員](#application-developer)**：將「使用者可以註冊應用程式」設定設為「否」時，此角色中的使用者可以建立應用程式註冊。 將「使用者可同意應用程式代表自己存取公司資料」設定設為「否」時，此角色也允許成員代表他們自己同意。 建立新的應用程式註冊或企業應用程式時，不會加入此角色的成員作為擁有者。

* **[計費管理員](#billing-administrator)**：進行採購、管理訂用帳戶、管理支援票證，以及監控服務健全狀況。

* **[雲端應用程式系統管理員](#cloud-application-administrator)**：此角色中的使用者具有與應用程式系統管理員角色相同的權限，但不包括管理應用程式 Proxy 的能力。 此角色會授與能力來建立和管理企業應用程式和應用程式註冊的所有層面。 此角色也會授與能力來同意委派的權限以及 Microsoft Graph 和 Azure AD Graph 以外的應用程式權限。 建立新的應用程式註冊或企業應用程式時，不會加入此角色的成員作為擁有者。

  <b>重要</b>：此角色授與管理應用程式認證的能力。 獲指派此角色的使用者可以將認證新增至應用程式，並使用這些認證來模擬應用程式的身分識別。 如果應用程式的身分識別已授與 Azure Active Directory 的存取權，例如建立或更新使用者或其他物件的能力，則獲指派這個角色的使用者可以在模擬應用程式時執行這些動作。 模擬應用程式身分識別的這項能力，對於使用者透過 Azure AD 中角色指派所能執行的動作，是一種權限提高。 請務必了解，將雲端應用程式系統管理員角色指派給使用者，會給予他們模擬應用程式身分識別的能力。

* **[雲端裝置系統管理員](#cloud-device-administrator)**：此角色的使用者可以啟用、停用和刪除 Azure AD 中的裝置，並在 Azure 入口網站中讀取 Windows 10 BitLocker 金鑰 (如果有的話)。 此角色不會授與可供管理裝置上任何其他屬性的權限。

* **[規範管理員](#compliance-administrator)**：此角色的使用者擁有 Office 365 安全性與法規遵循中心和 Exchange 系統管理中心。 如需詳細資訊，請參閱 [關於 Office 365 管理員角色](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d)。

* **[條件式存取系統管理員](#conditional-access-administrator)**：具有此角色的使用者能夠管理 Azure Active Directory 條件式存取設定。
  > [!NOTE]
  > 若要在 Azure 中部署 Exchange ActiveSync 條件式存取原則，使用者也必須是全域系統管理員。
  
* **[裝置系統管理員](#device-administrators)**：此角色是只能指派為[裝置設定](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/DevicesMenuBlade/DeviceSettings/menuId/)中的其他本機系統管理員。 具有此角色的使用者，會在已加入 Azure Active Directory 的所有 Windows 10 裝置上，成為本機電腦系統管理員。 它們並沒有在 Azure Active Directory 中管理裝置物件的能力。 

* **[目錄讀取者](#directory-readers)**︰這是舊版角色，用來指派給不支援[同意架構](../develop/quickstart-v1-integrate-apps-with-azure-ad.md)的應用程式。 不應將它指派給任何使用者。

* **[目錄同步作業帳戶](#directory-synchronization-accounts)**：請勿使用。 此角色會自動指派給 Azure AD Connect 服務，不適用於也不支援任何其他用途。

* **[目錄寫入者](#directory-writers)**︰這是舊版角色，用來指派給不支援[同意架構](../develop/quickstart-v1-integrate-apps-with-azure-ad.md)的應用程式。 不應將它指派給任何使用者。

* **[Dynamics 365 管理員/CRM 管理員](#dynamics-365-administrator)**︰具備此角色的使用者在有 Microsoft Dynamics 365 Online 服務時，於該服務內具有全域權限，以及管理支援票證和監控服務健康情況的能力。 如需詳細資訊，請參閱[使用服務管理員角色管理您的租用戶](https://docs.microsoft.com/dynamics365/customer-engagement/admin/use-service-admin-role-manage-tenant)。
  > [!NOTE] 
  > 在 Microsoft Graph API、Azure AD Graph API 和 Azure AD PowerShell 中，會將此角色識別為「Dynamics 365 服務管理員」。 在 Azure 入口網站中則是「Dynamics 365 管理員」。

* **[Exchange 管理員](#exchange-administrator)**︰在有 Microsoft Exchange Online 服務時，具備此角色的使用者在該服務內會具有全域權限。 以及建立和管理所有 Office 365 群組、管理支援票證，以及監視服務健康情況的能力。 如需詳細資訊，請參閱 [關於 Office 365 管理員角色](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d)。
  > [!NOTE]
  > 在 Microsoft Graph API、Azure AD Graph API 和 Azure AD PowerShell 中，會將此角色識別為「Exchange 服務管理員」。 在 Azure 入口網站中則是「Exchange 管理員」。

* **[全域系統管理員 / 公司系統管理員](#company-administrator)**︰具有此角色的使用者可以存取 Azure Active Directory 中所有的系統管理功能，以及使用 Azure Active Directory 身分識別的服務，例如 Exchange Online、SharePoint Online 和 Skype for Business Online。 註冊 Azure Active Directory 租用戶的人員會變成全域管理員。 只有全域管理員才能指派其他系統管理員角色。 您的公司可以有多位全域管理員。 全域系統管理員可以為任何使用者和所有其他系統管理員重設密碼。

  > [!NOTE]
  > 在 Microsoft Graph API、Azure AD Graph API 及 Azure AD PowerShell 中，是將此角色識別為「公司系統管理員」。 它是 [Azure 入口網站](https://portal.azure.com)中的「全域管理員」。
  >
  >

* **[來賓邀請者](#guest-inviter)**︰當 [可邀請成員] 使用者設定設為 [否] 時，此角色中的使用者可管理 Azure Active Directory B2B 來賓使用者邀請。 在[關於 Azure AD B2B 共同作業](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b)中查看 B2B 共同作業的詳細資訊。 這不包含任何其他權限。

* **[資訊保護系統管理員](#information-protection-administrator)**：具備此角色的使用者只在「Azure 資訊保護」服務上具有使用者權限。 此角色允許設定「Azure 資訊保護」原則的標籤、管理保護範本，以及啟用保護。 此角色並未授與「Identity Protection 中心」、Privileged Identity Management、「監視 Office 365 服務健康情況」及「Office 365 安全與規範中心」中的任何權限。

* **[Intune 管理員](#intune-administrator)**︰在有 Microsoft Intune Online 服務時，具備此角色的使用者在該服務內會具有全域權限。 此外，此角色包含管理使用者和裝置的能力，可相關聯原則以及建立和管理群組。 如需詳細資訊，請參閱[將角色型系統管理控制用於 Microsoft Intune](https://docs.microsoft.com/intune/role-based-access-control)
  > [!NOTE]
  > 在 Microsoft Graph API、Azure AD Graph API 和 Azure AD PowerShell 中，會將此角色識別為「Intune 服務管理員」。 在 Azure 入口網站中則是「Intune 管理員」。

* **[授權管理員](#license-administrator)**：此角色中的使用者可新增、移除和更新使用者和 群組的授權指派 (使用群組型授權)，以及管理使用者的使用位置。 此角色不會授與購買或管理訂用帳戶、建立或管理群組，或在使用位置以外建立或管理使用者的能力。

* **[訊息中心讀取者](#message-center-reader)**：此角色中的使用者可以在 [Office 365 訊息中心](https://support.office.com/article/Message-center-in-Office-365-38FB3333-BFCC-4340-A37B-DEDA509C2093)內，為他們的組織監視所設服務 (例如 Exchange、Intune 和 Microsoft Teams) 的通知和諮詢健康情況更新。 訊息中心讀者每週會收到貼文的電子郵件摘要和更新，並且可以在 Office 365 中分享訊息中心的貼文。 在 Azure AD 中，指派至此角色的使用者只會有 Azure AD 服務的唯讀存取權，與使用者和群組一樣。 

* **[合作夥伴第 1 層支援](#partner-tier1-support)**︰請勿使用。 此角色已被取代，而且未來將從 Azure AD 中移除。 此角色僅供少數 Microsoft 轉售合作夥伴使用，不適用於一般用途。

* **[合作夥伴第 2 層支援](#partner-tier2-support)**︰請勿使用。 此角色已被取代，而且未來將從 Azure AD 中移除。 此角色僅供少數 Microsoft 轉售合作夥伴使用，不適用於一般用途。

* **[密碼管理員/技術支援中心管理員](#helpdesk-administrator)**：具備此角色的使用者可以變更密碼、讓重新整理權杖失效、管理服務要求，以及監視服務健康情況。 讓重新整理權杖失效會強制使用者重新登入。 技術支援中心系統管理員可以重設密碼，並且讓非系統管理員或下列角色成員的其他使用者重新整理的權杖失效：
  * 目錄讀取器
  * 來賓邀請者
  * 服務台系統管理員
  * 訊息中心讀取者
  * 報告讀者
  
  <b>重要</b>：具備此角色的使用者可以變更可存取機密或私人資訊或 Azure Active Directory 內外重要組態的人員密碼。 變更使用者的密碼表示可承擔該使用者身分識別和權限。 例如︰
  * 應用程式註冊和企業應用程式擁有者，他們可以管理他們自己的應用程式認證。 這些應用程式在 Azure AD 中可能有特殊權限，而在其他地方未授與技術支援中心系統管理員。 技術支援中心系統管理員可以透過此路徑承擔應用程式擁有者的身分識別，然後藉由更新應用程式的認證，進一步承擔特殊權限應用程式的身分識別。
  * Azure 訂用帳戶擁有者，他們具有機密或私人資訊或者 Azure 中重要組態的存取權。
  * 安全性群組和 Office 365 群組擁有者，他們可以管理群組成員資格。 這個群組可以存取機密或私人資訊或者 Azure AD 和其他位置中的重要組態。
  * Azure AD 外部其他服務 (例如，Exchange Online、Office 安全性與合規性中心和人力資源系統) 中的系統管理員。
  * 非系統管理員，例如主管、法律顧問和人力資源員工，他們可以存取機密或私人資訊。

  > [!NOTE]
  > 在 Microsoft Graph API、Azure AD Graph API 和 Azure AD PowerShell 中，會將此角色識別為「技術支援中心管理員」。 它是 [Azure 入口網站](https://portal.azure.com/)中的「密碼管理員」。
  >
  
* **[Power BI 管理員](#power-bi-administrator)**︰具備此角色的使用者在有 Microsoft Power BI 服務時，於該服務內具有全域權限，以及管理支援票證和監控服務健康情況的能力。 如需詳細資訊，請參閱[了解 Power BI 管理員角色](https://docs.microsoft.com/power-bi/service-admin-role)。
  > [!NOTE]
  > 在 Microsoft Graph API、Azure AD Graph API 和 Azure AD PowerShell 中，會將此角色識別為「Power BI 服務管理員」。 在 Azure 入口網站中則是「Power BI 管理員」。

* **[特殊權限角色管理員](#privileged-role-administrator)**：具備此角色的使用者可以管理 Azure Active Directory 中，以及 Azure AD Privileged Identity Management 內的角色指派。 此外，這個角色允許各個層面的 Privileged Identity Management 管理。

  <b>重要</b>：此角色會獲得授與管理所有 Azure AD 角色成員資格的能力，包括全域管理員角色。 此角色不包含 Azure AD 中的任何其他特殊權限能力，例如建立或更新使用者。 不過，指派給這個角色的使用者可以藉由指派額外的角色，來授與自己或其他人額外權限。

* **[報告讀取者](#reports-reader)**：具備此角色的使用者可以檢視 Office 365 系統管理中心內的使用情況報告資料和報告儀表板，以及 PowerBI 中的採用內容套件。 此外，此角色還可讓使用者存取 Azure AD 中的登入報告與活動，以及 Microsoft Graph 報告 API 所傳回的資料。 獲指派「報告讀者」角色的使用者只能存取相關的使用情況和採用計量。 他們並不具備任何系統管理權限，因此無法進行設定或存取產品特定的系統管理中心 (例如 Exchange)。 

* **[安全性系統管理員](#security-administrator)**︰具有此角色的使用者擁有安全性讀取者角色的所有唯讀權限，再加上管理與安全性相關的服務設定能力︰Azure Active Directory Identity Protection、Azure 資訊保護和 Office 365 安全與規範中心。 關於 Office 365 權限的詳細資訊可在 [Office 365 安全性與法規遵循中心的權限](https://support.office.com/article/Permissions-in-the-Office-365-Security-Compliance-Center-d10608af-7934-490a-818e-e68f17d0e9c1)中取得。
  
  | 在 | 可以執行 |
  | --- | --- |
  | 身分識別防護中心 |<ul><li>「安全性讀取者」角色的所有權限。<li>此外，還能夠執行除了重設密碼以外的所有 IPC 作業。 |
  | Privileged Identity Management |<ul><li>「安全性讀取者」角色的所有權限。<li>**無法**管理 Azure AD 角色成員資格或設定。 |
  | <p>監視 Office 365 服務健康狀況</p><p>Office 365 安全性與法規遵循中心 |<ul><li>「安全性讀取者」角色的所有權限。<li>可以設定「進階威脅防護」功能中的所有設定 (惡意程式碼和病毒防護、惡意 URL 組態、URL 追蹤等)。 |
  
* **[安全性讀取者](#security-reader)**︰具有此角色的使用者具有全域唯讀存取權，包括 Azure Active Directory、身分識別保護、Privileged Identity Management，以及能夠讀取 Azure Active Directory 登入報告和稽核記錄檔的所有資訊。 角色也會在 Office 365 安全性與法規遵循中心授與唯讀權限。 關於 Office 365 權限的詳細資訊可在 [Office 365 安全性與法規遵循中心的權限](https://support.office.com/article/Permissions-in-the-Office-365-Security-Compliance-Center-d10608af-7934-490a-818e-e68f17d0e9c1)中取得。

  | 在 | 可以執行 |
  | --- | --- |
  | 身分識別防護中心 |讀取安全性功能的所有安全性報告和設定資訊<ul><li>反垃圾郵件<li>加密<li>資料外洩防護<li>反惡意程式碼<li>進階威脅防護<li>防網路釣魚<li>郵件流程規則 |
  | Privileged Identity Management |<p>以唯讀方式存取 Azure AD PIM 中所顯示的一切資訊︰Azure AD 角色指派的原則和報告、安全性檢閱，以及在未來還可透過讀取來存取 Azure AD 角色指派以外案例的原則資料和報告。<p>**無法**註冊 Azure AD PIM 或對它進行任何變更。 擔任此角色的人員可以在 PIM 的入口網站中，或是透過 PowerShell，為其他角色 (例如「全域管理員」或「特殊權限角色管理員」) 的候選人啟用角色。 |
  | <p>監視 Office 365 服務健康狀況</p><p>Office 365 安全性與法規遵循中心</p> |<ul><li>讀取和管理警示<li>讀取安全性原則<li>讀取威脅情報、執行 Cloud App Discovery，以及在搜尋和調查時執行隔離<li>讀取所有報告 |

* **[服務支援系統管理員](#service-support-administrator)**︰具有此角色的使用者可以使用 Microsoft 開啟 Azure 和 Office 365 服務的支援要求，以及檢視 Azure 入口網站的服務儀表板和訊息中心以及 Office 365 系統管理入口網站。 如需詳細資訊，請參閱 [關於 Office 365 管理員角色](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d)。

* **[SharePoint 管理員](#sharepoint-administrator)**︰具備此角色的使用者在有 Microsoft SharePoint Online 服務時，於該服務內具有全域權限，以及建立和管理所有 Office 365 群組、管理支援票證和監控服務健康情況的能力。 如需詳細資訊，請參閱 [關於 Office 365 管理員角色](https://support.office.com/article/About-Office-365-admin-roles-da585eea-f576-4f55-a1e0-87090b6aaa9d)。
  > [!NOTE]
  > 在 Microsoft Graph API、Azure AD Graph API 和 Azure AD PowerShell 中，會將此角色識別為「SharePoint 服務管理員」。 在 Azure 入口網站中則是「SharePoint 管理員」。

* **[商務用 Skype / Lync 管理員](#skype-for-business-administrator)**︰在有 Microsoft 商務用 Skype 服務時，具備此角色的使用者在該服務內會具有全域權限，以及在 Azure Active Directory 中管理 Skype 特定的使用者屬性。 此外，此角色會授與管理支援票證及監視服務健康情況的能力，以及存取 Microsoft Teams 和商務用 Skype 系統管理中心的能力。 此帳戶也必須獲得 Microsoft Teams 授權，否則就無法執行 Microsoft Teams PowerShell Cmdlet。 如需詳細資訊，請參閱[關於商務用 Skype 系統管理員角色](https://support.office.com/article/about-the-skype-for-business-admin-role-aeb35bda-93fc-49b1-ac2c-c74fbeb737b5)，如需 Microsoft Teams 的授權資訊，請參閱[商務用 Skype 和 Microsoft Teams 附加授權](https://docs.microsoft.com/skypeforbusiness/skype-for-business-and-microsoft-teams-add-on-licensing/skype-for-business-and-microsoft-teams-add-on-licensing)

  > [!NOTE]
  > 在 Microsoft Graph API、Azure AD Graph API 和 Azure AD PowerShell 中，會將此角色識別為「Lync 服務管理員」。 在 [Azure 入口網站](https://portal.azure.com/)中則是「商務用 Skype 管理員」。

* **[Microsoft Teams 通訊系統管理員](#teams-communications-administrator)**：此角色的使用者可以管理 Microsoft Teams 在語音和電話語音相關工作負載的各個層面。 這包括電話號碼指派管理工具、語音和會議原則，以及呼叫分析工具組的完整存取權。

* **[Microsoft Teams 通訊支援工程師](#teams-communications-support-engineer)**：此角色的使用者可以使用 Microsoft Teams 和商務用 Skype 系統管理中心內的使用者呼叫疑難排解工具，針對 Microsoft Teams 和商務用 Skype 內的通訊問題進行疑難排解。 此角色的使用者可以檢視所有相關參與者的完整呼叫記錄資訊。

* **[Microsoft Teams 通訊支援專家](#teams-communications-support-specialist)**：此角色的使用者可以使用 Microsoft Teams 和商務用 Skype 系統管理中心內的使用者呼叫疑難排解工具，針對 Microsoft Teams 和商務用 Skype 內的通訊問題進行疑難排解。 此角色的使用者只能檢視其所查閱特定使用者的呼叫中所含有的使用者詳細資料。

* **[Microsoft Teams 管理員](#teams-administrator)**：此角色的使用者可以透過 Microsoft Teams 和商務用 Skype 系統管理中心以及個別的 PowerShell 模組，管理 Microsoft Teams 工作負載的所有層面。 這包括所有與電話語音、傳訊、會議和小組本身相關的管理工具以及其他領域。 這個角色會額外獲得授與建立和管理所有 Office 365 群組、管理支援票證，以及監視服務健康情況的能力。
  > [!NOTE]
  > 在 Microsoft Graph API、Azure AD Graph API 和 Azure AD PowerShell 中，會將此角色識別為「Microsoft Teams 服務管理員」。 在 Azure 入口網站中則是「Microsoft Teams 管理員」。

* **[使用者帳戶管理員](#user-account-administrator)**︰具有此角色的使用者可以建立使用者，以及管理具有部分限制使用者的所有層面 (如下所示)。 此外，具有此角色的使用者可以建立與管理所有群組。 此角色也包含建立和管理使用者檢視、管理支援票證，以及監視服務健康情況的能力。

  | | |
  | --- | --- |
  |一般權限|<p>建立 [使用者和群組]</p><p>建立和管理使用者檢視</p><p>建立 Office 支援票證|
  |<p>所有使用者，包括所有管理員</p>|<p>管理授權</p><p>管理使用者主體名稱以外的所有使用者屬性</p>
  |只有非管理員或者下列任何有限管理員角色的使用者：<ul><li>目錄讀取器<li>來賓邀請者<li>服務台系統管理員<li>訊息中心讀取者<li>報告讀者<li>使用者帳戶管理員|<p>刪除及還原</p><p>停用和啟用</p><p>使重新整理權杖失效</p><p>管理包含使用者主體名稱的所有使用者屬性</p><p>重設密碼</p><p>更新 (FIDO) 裝置金鑰</p>
  
  <b>重要</b>：具備此角色的使用者可以變更可存取機密或私人資訊或 Azure Active Directory 內外重要組態的人員密碼。 變更使用者的密碼表示可承擔該使用者身分識別和權限。 例如︰
  * 應用程式註冊和企業應用程式擁有者，他們可以管理他們自己的應用程式認證。 這些應用程式在 Azure AD 中可能有特殊權限，而在其他地方未授與使用者系統管理員。 使用者系統管理員可以透過此路徑承擔應用程式擁有者的身分識別，然後藉由更新應用程式的認證，進一步承擔特殊權限應用程式的身分識別。
  * Azure 訂用帳戶擁有者，他們具有機密或私人資訊或者 Azure 中重要組態的存取權。
  * 安全性群組和 Office 365 群組擁有者，他們可以管理群組成員資格。 這個群組可以存取機密或私人資訊或者 Azure AD 和其他位置中的重要組態。
  * Azure AD 外部其他服務 (例如，Exchange Online、Office 安全性與合規性中心和人力資源系統) 中的系統管理員。
  * 非系統管理員，例如主管、法律顧問和人力資源員工，他們可以存取機密或私人資訊。

下表說明 Azure Active Directory 中賦予每個角色的特定權限。 某些角色在 Azure Active Directory 以外的 Microsoft 服務中可能有額外的權限。

### <a name="application-administrator"></a>應用程式系統管理員
能夠建立及管理應用程式註冊與企業應用程式的所有層面。

  > [!NOTE]
  > 此角色會繼承目錄讀取者角色的其他權限。
  >
  >

| **動作** | **說明** |
| --- | --- |
| microsoft.aad.directory/applications/audience/update | 在 Azure Active Directory 中更新 applications.audience 屬性。 |
| microsoft.aad.directory/applications/authentication/update | 在 Azure Active Directory 中更新 applications.authentication 屬性。 |
| microsoft.aad.directory/applications/basic/update | 更新 Azure Active Directory 中 Applications 的基本屬性。 |
| microsoft.aad.directory/applications/create | 在 Azure Active Directory 中建立應用程式。 |
| microsoft.aad.directory/applications/credentials/update | 在 Azure Active Directory 中更新 applications.credentials 屬性。 |
| microsoft.aad.directory/applications/delete | 刪除 Azure Active Directory 中的應用程式。 |
| microsoft.aad.directory/applications/owners/update | 更新 Azure Active Directory 中的 applications.owners 屬性。 |
| microsoft.aad.directory/applications/permissions/update | 在 Azure Active Directory 中更新 applications.permissions 屬性。 |
| microsoft.aad.directory/applications/policies/update | 更新 Azure Active Directory 中的 applications.policies 屬性。 |
| microsoft.aad.directory/appRoleAssignments/create | 在 Azure Active Directory 中建立 appRoleAssignments。 |
| microsoft.aad.directory/appRoleAssignments/read | 讀取 Azure Active Directory 中的 appRoleAssignments。 |
| microsoft.aad.directory/appRoleAssignments/update | 更新在 Azure Active Directory 中的 appRoleAssignments。 |
| microsoft.aad.directory/appRoleAssignments/delete | 刪除 Azure Active Directory 中的 appRoleAssignments。 |
| microsoft.aad.directory/policies/applicationConfiguration/basic/read | 讀取 Azure Active Directory 中的 policies.applicationConfiguration 屬性。 |
| microsoft.aad.directory/policies/applicationConfiguration/basic/update | 更新 Azure Active Directory 中的 policies.applicationConfiguration 屬性。 |
| microsoft.aad.directory/policies/applicationConfiguration/create | 在 Azure Active Directory 中建立原則。 |
| microsoft.aad.directory/policies/applicationConfiguration/delete | 刪除 Azure Active Directory 中的原則。 |
| microsoft.aad.directory/policies/applicationConfiguration/owners/read | 讀取 Azure Active Directory 中的 policies.applicationConfiguration 屬性。 |
| microsoft.aad.directory/policies/applicationConfiguration/owners/update | 更新 Azure Active Directory 中的 policies.applicationConfiguration 屬性。 |
| microsoft.aad.directory/policies/applicationConfiguration/policyAppliedTo/read | 讀取 Azure Active Directory 中的 policies.applicationConfiguration 屬性。 |
| microsoft.aad.directory/servicePrincipals/basic/update | 更新 Azure Active Directory 中 servicePrincipals 的基本屬性。 |
| microsoft.aad.directory/servicePrincipals/create | 在 Azure Active Directory 中建立 servicePrincipals。 |
| microsoft.aad.directory/servicePrincipals/delete | 刪除 Azure Active Directory 中的 servicePrincipals。 |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/update | 更新 Azure Active Directory 中的 servicePrincipals.appRoleAssignedTo 屬性。 |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/update | 更新 Azure Active Directory 中的 servicePrincipals.appRoleAssignments 屬性。 |
| microsoft.aad.directory/servicePrincipals/owners/update | 更新 Azure Active Directory 中的 servicePrincipals.owners 屬性。 |
| microsoft.aad.directory/servicePrincipals/policies/update | 更新 Azure Active Directory 中的 servicePrincipals.policies 屬性。 |
| microsoft.aad.directory/users/assignLicense | 管理 Azure Active Directory 中的使用者授權。 |
| microsoft.aad.reports/allEntities/read | 讀取 Azure AD 報告。 |
| microsoft.azure.serviceHealth/allEntities/allTasks | 讀取及設定 Azure 服務健康情況。 |
| microsoft.azure.supportTickets/allEntities/allTasks | 建立和管理 Azure 支援票證。 |
| microsoft.office365.serviceHealth/allEntities/allTasks | 讀取及設定 Office 365 服務健康情況。 |
| microsoft.office365.supportTickets/allEntities/allTasks | 建立和管理 Office 365 支援票證。 |

### <a name="application-developer"></a>應用程式開發人員
可建立與 [使用者可註冊應用程式] 設定不相關的應用程式註冊。

  > [!NOTE]
  > 此角色會繼承目錄讀取者角色的其他權限。
  >
  >

| **動作** | **說明** |
| --- | --- |
| microsoft.aad.directory/applications/createAsOwner | 在 Azure Active Directory 中建立應用程式。 建立者會新增為第一個擁有者，而建立的物件會算在建立者的 250 個建立物件配額中。 |
| microsoft.aad.directory/appRoleAssignments/createAsOwner | 在 Azure Active Directory 中建立 appRoleAssignments。 建立者會新增為第一個擁有者，而建立的物件會算在建立者的 250 個建立物件配額中。 |
| microsoft.aad.directory/oAuth2PermissionGrants/createAsOwner | 在 Azure Active Directory 中建立 oAuth2PermissionGrants。 建立者會新增為第一個擁有者，而建立的物件會算在建立者的 250 個建立物件配額中。 |
| microsoft.aad.directory/servicePrincipals/createAsOwner | 在 Azure Active Directory 中建立 servicePrincipals。 建立者會新增為第一個擁有者，而建立的物件會算在建立者的 250 個建立物件配額中。 |

### <a name="billing-administrator"></a>計費管理員
能夠執行一般計費相關工作，例如更新付款資訊。

  > [!NOTE]
  > 此角色會繼承目錄讀取者角色的其他權限。
  >
  >

  > [!NOTE]
  > 此角色具有 Azure Active Directory 以外的其他權限。 如需詳細資訊，請參閱前述角色說明。
  >
  >

| **動作** | **說明** |
| --- | --- |
| microsoft.aad.directory/organization/basic/update | 更新 Azure Active Directory 中 organization 的基本屬性。 |
| microsoft.aad.directory/organization/trustedCAsForPasswordlessAuth/update | 更新 Azure Active Directory 中的 organization.trustedCAsForPasswordlessAuth 屬性。 |
| microsoft.azure.accessService/allEntities/allTasks | 管理 Azure 存取服務的所有層面。 |
| microsoft.azure.serviceHealth/allEntities/allTasks | 讀取及設定 Azure 服務健康情況。 |
| microsoft.azure.supportTickets/allEntities/allTasks | 建立和管理 Azure 支援票證。 |
| microsoft.commerce.billing/allEntities/allTasks | 管理 Office 365 帳單的所有層面。 |
| microsoft.office365.serviceHealth/allEntities/allTasks | 讀取及設定 Office 365 服務健康情況。 |
| microsoft.office365.supportTickets/allEntities/allTasks | 建立和管理 Office 365 支援票證。 |

### <a name="desktop-analytics-administrator"></a>電腦分析系統管理員
可存取及管理桌面管理工具與服務 (含 Intune)。

  > [!NOTE]
  > 此角色會繼承目錄讀取者角色的其他權限。
  >
  >

  > [!NOTE]
  > 此角色具有 Azure Active Directory 以外的其他權限。 如需詳細資訊，請參閱前述角色說明。
  >
  >

| **動作** | **說明** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | 管理 Azure 存取服務的所有層面。 |
| microsoft.azure.serviceHealth/allEntities/allTasks | 讀取及設定 Azure 服務健康情況。 |
| microsoft.azure.supportTickets/allEntities/allTasks | 建立和管理 Azure 支援票證。 |
| microsoft.office365.desktopAnalytics/allEntities/allTasks | 管理電腦分析的所有層面。 |
| microsoft.office365.serviceHealth/allEntities/allTasks | 讀取及設定 Office 365 服務健康情況。 |
| microsoft.office365.supportTickets/allEntities/allTasks | 建立和管理 Office 365 支援票證。 |

### <a name="cloud-application-administrator"></a>雲端應用程式系統管理員
能夠建立及管理應用程式註冊與企業應用程式的所有層面，但應用程式 Proxy 除外。

  > [!NOTE]
  > 此角色會繼承目錄讀取者角色的其他權限。
  >
  >

| **動作** | **說明** |
| --- | --- |
| microsoft.aad.directory/applications/audience/update | 在 Azure Active Directory 中更新 applications.audience 屬性。 |
| microsoft.aad.directory/applications/authentication/update | 在 Azure Active Directory 中更新 applications.authentication 屬性。 |
| microsoft.aad.directory/applications/basic/update | 更新 Azure Active Directory 中 Applications 的基本屬性。 |
| microsoft.aad.directory/applications/create | 在 Azure Active Directory 中建立應用程式。 |
| microsoft.aad.directory/applications/credentials/update | 在 Azure Active Directory 中更新 applications.credentials 屬性。 |
| microsoft.aad.directory/applications/delete | 刪除 Azure Active Directory 中的應用程式。 |
| microsoft.aad.directory/applications/owners/update | 更新 Azure Active Directory 中的 applications.owners 屬性。 |
| microsoft.aad.directory/applications/permissions/update | 在 Azure Active Directory 中更新 applications.permissions 屬性。 |
| microsoft.aad.directory/applications/policies/update | 更新 Azure Active Directory 中的 applications.policies 屬性。 |
| microsoft.aad.directory/appRoleAssignments/create | 在 Azure Active Directory 中建立 appRoleAssignments。 |
| microsoft.aad.directory/appRoleAssignments/update | 更新在 Azure Active Directory 中的 appRoleAssignments。 |
| microsoft.aad.directory/appRoleAssignments/delete | 刪除 Azure Active Directory 中的 appRoleAssignments。 |
| microsoft.aad.directory/policies/applicationConfiguration/create | 在 Azure Active Directory 中建立原則。 |
| microsoft.aad.directory/policies/applicationConfiguration/basic/read | 讀取 Azure Active Directory 中的 policies.applicationConfiguration 屬性。 |
| microsoft.aad.directory/policies/applicationConfiguration/basic/update | 更新 Azure Active Directory 中的 policies.applicationConfiguration 屬性。 |
| microsoft.aad.directory/policies/applicationConfiguration/delete | 刪除 Azure Active Directory 中的原則。 |
| microsoft.aad.directory/policies/applicationConfiguration/owners/read | 讀取 Azure Active Directory 中的 policies.applicationConfiguration 屬性。 |
| microsoft.aad.directory/policies/applicationConfiguration/owners/update | 更新 Azure Active Directory 中的 policies.applicationConfiguration 屬性。 |
| microsoft.aad.directory/policies/applicationConfiguration/policyAppliedTo/read | 讀取 Azure Active Directory 中的 policies.applicationConfiguration 屬性。 |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/update | 更新 Azure Active Directory 中的 servicePrincipals.appRoleAssignedTo 屬性。 |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/update | 更新 Azure Active Directory 中的 servicePrincipals.appRoleAssignments 屬性。 |
| microsoft.aad.directory/servicePrincipals/basic/update | 更新 Azure Active Directory 中 servicePrincipals 的基本屬性。 |
| microsoft.aad.directory/servicePrincipals/create | 在 Azure Active Directory 中建立 servicePrincipals。 |
| microsoft.aad.directory/servicePrincipals/delete | 刪除 Azure Active Directory 中的 servicePrincipals。 |
| microsoft.aad.directory/servicePrincipals/owners/update | 更新 Azure Active Directory 中的 servicePrincipals.owners 屬性。 |
| microsoft.aad.directory/servicePrincipals/policies/update | 更新 Azure Active Directory 中的 servicePrincipals.policies 屬性。 |
| microsoft.aad.directory/users/assignLicense | 管理 Azure Active Directory 中的使用者授權。 |
| microsoft.aad.reports/allEntities/read | 讀取 Azure AD 報告。 |
| microsoft.azure.serviceHealth/allEntities/allTasks | 讀取及設定 Azure 服務健康情況。 |
| microsoft.azure.supportTickets/allEntities/allTasks | 建立和管理 Azure 支援票證。 |
| microsoft.office365.serviceHealth/allEntities/allTasks | 讀取及設定 Office 365 服務健康情況。 |
| microsoft.office365.supportTickets/allEntities/allTasks | 建立和管理 Office 365 支援票證。 |

### <a name="cloud-device-administrator"></a>雲端裝置管理員
在 Azure AD 中管理裝置的完整存取。

  > [!NOTE]
  > 此角色會繼承目錄讀取者角色的其他權限。
  >
  >

| **動作** | **說明** |
| --- | --- |
| microsoft.aad.directory/devices/delete | 刪除 Azure Active Directory 中的 devices。 |
| microsoft.aad.directory/devices/disable | 停用 Azure Active Directory 中的 devices。 |
| microsoft.aad.directory/devices/enable | 啟用 Azure Active Directory 中的裝置。 |
| microsoft.aad.reports/allEntities/read | 讀取 Azure AD 報告。 |
| microsoft.azure.serviceHealth/allEntities/allTasks | 讀取及設定 Azure 服務健康情況。 |
| microsoft.office365.serviceHealth/allEntities/allTasks | 讀取及設定 Office 365 服務健康情況。 |

### <a name="company-administrator"></a>公司系統管理員
可管理使用 Azure AD 身分識別的 Azure AD 與 Microsoft 服務的所有層面。

  > [!NOTE]
  > 此角色會繼承角色的其他權限。
  >
  >

  > [!NOTE]
  > 此角色具有 Azure Active Directory 以外的其他權限。 如需詳細資訊，請參閱前述角色說明。
  >
  >

| **動作** | **說明** |
| --- | --- |
| microsoft.aad.directory/administrativeUnits/allProperties/allTasks | 建立和刪除 administrativeUnits，以及讀取與更新 Azure Active Directory 中的所有屬性。 |
| microsoft.aad.directory/applications/allProperties/allTasks | 建立和刪除應用程式，以及讀取與更新 Azure Active Directory 中的所有屬性。 |
| microsoft.aad.directory/appRoleAssignments/allProperties/allTasks | 建立和刪除 appRoleAssignments，以及讀取與更新 Azure Active Directory 中的所有屬性。 |
| microsoft.aad.directory/contacts/allProperties/allTasks | 建立和刪除合約，以及讀取與更新 Azure Active Directory 中的所有屬性。 |
| microsoft.aad.directory/contracts/allProperties/allTasks | 建立和刪除合約，以及讀取與更新 Azure Active Directory 中的所有屬性。 |
| microsoft.aad.directory/devices/allProperties/allTasks | 建立和刪除裝置，以及在 Azure Active Directory 中讀取和更新所有屬性。 |
| microsoft.aad.directory/directoryRoles/allProperties/allTasks | 建立和刪除 directoryRoles，以及在 Azure Active Directory 中讀取和更新所有屬性。 |
| microsoft.aad.directory/directoryRoleTemplates/allProperties/allTasks | 建立和刪除 directoryRoleTemplates，以及在 Azure Active Directory 中讀取和更新所有屬性。 |
| microsoft.aad.directory/domains/allProperties/allTasks | 建立和刪除 domains，以及在 Azure Active Directory 中讀取和更新所有屬性。 |
| microsoft.aad.directory/groups/allProperties/allTasks | 建立和刪除 groups，以及讀取與更新 Azure Active Directory 中的所有屬性。 |
| microsoft.aad.directory/groupSettings/allProperties/allTasks | 建立和刪除 groupSettings，以及讀取與更新 Azure Active Directory 中的所有屬性。 |
| microsoft.aad.directory/groupSettingTemplates/allProperties/allTasks | 建立和刪除 groupSettingTemplates，以及讀取與更新 Azure Active Directory 中的所有屬性。 |
| microsoft.aad.directory/loginTenantBranding/allProperties/allTasks | 建立和刪除 loginTenantBranding，以及在 Azure Active Directory 中讀取和更新所有屬性。 |
| microsoft.aad.directory/oAuth2PermissionGrants/allProperties/allTasks | 建立和刪除 oAuth2PermissionGrants，以及在 Azure Active Directory 中讀取和更新所有屬性。 |
| microsoft.aad.directory/organization/allProperties/allTasks | 建立和刪除 organization，同時讀取及更新 Azure Active Directory 中的所有屬性。 |
| microsoft.aad.directory/policies/allProperties/allTasks | 建立和刪除 policies，以及在 Azure Active Directory 中讀取和更新所有屬性。 |
| microsoft.aad.directory/roleAssignments/allProperties/allTasks | 建立與刪除 roleAssignments，以及讀取與更新 Azure Active Directory 中的所有屬性。 |
| microsoft.aad.directory/roleDefinitions/allProperties/allTasks | 建立與刪除 roleDefinitions，以及讀取與更新 Azure Active Directory 中的所有屬性。 |
| microsoft.aad.directory/scopedRoleMemberships/allProperties/allTasks | 建立和刪除 scopedRoleMemberships，以及在 Azure Active Directory 中讀取和更新所有屬性。 |
| microsoft.aad.directory/serviceAction/activateService | 可以在 Azure Active Directory 中執行 Activateservice 服務動作 |
| microsoft.aad.directory/serviceAction/disableDirectoryFeature | 可以在 Azure Active Directory 中執行 Disabledirectoryfeature 服務動作 |
| microsoft.aad.directory/serviceAction/enableDirectoryFeature | 可以在 Azure Active Directory 中執行 Enabledirectoryfeature 服務動作 |
| microsoft.aad.directory/serviceAction/getAvailableExtentionProperties | 可以在 Azure Active Directory 中執行 Getavailableextentionproperties 服務動作 |
| microsoft.aad.directory/servicePrincipals/allProperties/allTasks | 建立和刪除 servicePrincipals，以及在 Azure Active Directory 中讀取和更新所有屬性。 |
| microsoft.aad.directory/subscribedSkus/allProperties/allTasks | 建立和刪除 subscribedSkus，以及在 Azure Active Directory 中讀取和更新所有屬性。 |
| microsoft.aad.directory/users/allProperties/allTasks | 建立和刪除 users，以及在 Azure Active Directory 中讀取和更新所有屬性。 |
| microsoft.aad.directorySync/allEntities/allTasks | 執行 Azure AD Connect 中的所有動作。 |
| microsoft.aad.identityProtection/allEntities/allTasks | 建立和刪除所有資源，以及讀取和更新 microsoft.aad.identityProtection 中的標準屬性。 |
| microsoft.aad.privilegedIdentityManagement/allEntities/read | 讀取 microsoft.aad.privilegedIdentityManagement 中的所有資源。 |
| microsoft.aad.reports/allEntities/allTasks | 讀取及設定 Azure AD 報告。 |
| microsoft.azure.accessService/allEntities/allTasks | 管理 Azure 存取服務的所有層面。 |
| microsoft.azure.informationProtection/allEntities/allTasks | 管理 Azure 資訊保護的所有層面。 |
| microsoft.azure.serviceHealth/allEntities/allTasks | 讀取及設定 Azure 服務健康情況。 |
| microsoft.azure.supportTickets/allEntities/allTasks | 建立和管理 Azure 支援票證。 |
| microsoft.commerce.billing/allEntities/allTasks | 管理 Office 365 帳單的所有層面。 |
| microsoft.intune/allEntities/allTasks | 管理 Intune 的所有層面。 |
| microsoft.office365.complianceManager/allEntities/allTasks | 管理 Office 365 合規性管理員的所有層面 |
| microsoft.office365.exchange/allEntities/allTasks | 管理 Exchange Online 的所有層面。 |
| microsoft.office365.lockbox/allEntities/allTasks | 管理 Office 365 客戶加密箱的所有層面 |
| microsoft.office365.messageCenter/messages/read | 讀取 microsoft.office365.messageCenter 中的訊息。 |
| microsoft.office365.messageCenter/securityMessages/read | 讀取 microsoft.office365.messageCenter 中的 securityMessages。 |
| microsoft.powerApps.powerBI/allEntities/allTasks | 管理 Power BI 的所有層面。 |
| microsoft.office365.protectionCenter/allEntities/allTasks | 管理 Office 365 防護中心的所有層面。 |
| microsoft.office365.serviceHealth/allEntities/allTasks | 讀取及設定 Office 365 服務健康情況。 |
| microsoft.office365.sharepoint/allEntities/allTasks | 建立和刪除所有資源，以及讀取和更新 microsoft.office365.sharepoint 中的標準屬性。 |
| microsoft.office365.skypeForBusiness/allEntities/allTasks | 管理商務用 Skype Online 的所有層面。 |
| microsoft.office365.supportTickets/allEntities/allTasks | 建立和管理 Office 365 支援票證。 |
| microsoft.powerApps.dynamics365/allEntities/allTasks | 管理 Dynamics 365 的所有層面。 |

### <a name="compliance-administrator"></a>規範管理員
可讀取和管理合規性設定及 Azure AD 與 Office 365 中的報告。

  > [!NOTE]
  > 此角色會繼承目錄讀取者角色的其他權限。
  >
  >

  > [!NOTE]
  > 此角色具有 Azure Active Directory 以外的其他權限。 如需詳細資訊，請參閱前述角色說明。
  >
  >

| **動作** | **說明** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | 管理 Azure 存取服務的所有層面。 |
| microsoft.azure.serviceHealth/allEntities/allTasks | 讀取及設定 Azure 服務健康情況。 |
| microsoft.azure.supportTickets/allEntities/allTasks | 建立和管理 Azure 支援票證。 |
| microsoft.office365.complianceManager/allEntities/allTasks | 管理 Office 365 合規性管理員的所有層面 |
| microsoft.office365.exchange/allEntities/allTasks | 管理 Exchange Online 的所有層面。 |
| microsoft.office365.serviceHealth/allEntities/allTasks | 讀取及設定 Office 365 服務健康情況。 |
| microsoft.office365.sharepoint/allEntities/allTasks | 建立和刪除所有資源，以及讀取和更新 microsoft.office365.sharepoint 中的標準屬性。 |
| microsoft.office365.skypeForBusiness/allEntities/allTasks | 管理商務用 Skype Online 的所有層面。 |
| microsoft.office365.supportTickets/allEntities/allTasks | 建立和管理 Office 365 支援票證。 |

### <a name="conditional-access-administrator"></a>條件式存取系統管理員
可管理條件式存取功能。

  > [!NOTE]
  > 此角色會繼承目錄讀取者角色的其他權限。
  >
  >

| **動作** | **說明** |
| --- | --- |
| microsoft.aad.directory/policies/conditionalAccess/basic/read | 讀取 Azure Active Directory 中的 policies.conditionalAccess 屬性。 |
| microsoft.aad.directory/policies/conditionalAccess/basic/update | 更新 Azure Active Directory 中的 policies.conditionalAccess 屬性。 |
| microsoft.aad.directory/policies/conditionalAccess/create | 在 Azure Active Directory 中建立原則。 |
| microsoft.aad.directory/policies/conditionalAccess/delete | 刪除 Azure Active Directory 中的原則。 |
| microsoft.aad.directory/policies/conditionalAccess/owners/read | 讀取 Azure Active Directory 中的 policies.conditionalAccess 屬性。 |
| microsoft.aad.directory/policies/conditionalAccess/owners/update | 更新 Azure Active Directory 中的 policies.conditionalAccess 屬性。 |
| microsoft.aad.directory/policies/conditionalAccess/policiesAppliedTo/read | 讀取 Azure Active Directory 中的 policies.conditionalAccess 屬性。 |

### <a name="customer-lockbox-access-approver"></a>客戶 LockBox 存取核准者
可核准 Microsoft 支援要求，以存取客戶組織的資料。

  > [!NOTE]
  > 此角色會繼承目錄讀取者角色的其他權限。
  >
  >

  > [!NOTE]
  > 此角色具有 Azure Active Directory 以外的其他權限。 如需詳細資訊，請參閱前述角色說明。
  >
  >

| **動作** | **說明** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | 管理 Azure 存取服務的所有層面。 |
| microsoft.office365.lockbox/allEntities/allTasks | 管理 Office 365 客戶加密箱的所有層面 |

### <a name="device-administrators"></a>裝置系統管理員
此角色的成員會新增至已加入 Azure AD 的裝置上的本機系統管理員群組群組。

  > [!NOTE]
  > 此角色會繼承目錄讀取者角色的其他權限。
  >
  >

| **動作** | **說明** |
| --- | --- |
| microsoft.aad.directory/groupSettings/basic/read | 讀取 Azure Active Directory 中 groupSettings 的基本屬性。 |
| microsoft.aad.directory/groupSettingTemplates/basic/read | 讀取 Azure Active Directory 中 groupSettingTemplates 的基本屬性。 |

### <a name="directory-readers"></a>目錄讀取器
可讀取基本目錄資訊。 用來授與應用程式的存取權，不適用於使用者。

  > [!NOTE]
  > 此角色會繼承角色的其他權限。
  >
  >

| **動作** | **說明** |
| --- | --- |
| microsoft.aad.directory/administrativeUnits/basic/read | 讀取 Azure Active Directory 中 administrativeUnits 的基本屬性。 |
| microsoft.aad.directory/administrativeUnits/members/read | 讀取 Azure Active Directory 中的 administrativeUnits.members 屬性。 |
| microsoft.aad.directory/applications/audience/read | 讀取 Azure Active Directory 中的 applications.audience 屬性。 |
| microsoft.aad.directory/applications/authentication/read | 讀取 Azure Active Directory 中的 applications.authentication 屬性。 |
| microsoft.aad.directory/applications/basic/read | 讀取 Azure Active Directory 中 Applications 的基本屬性。 |
| microsoft.aad.directory/applications/credentials/read | 讀取 Azure Active Directory 中的 applications.credentials 屬性。 |
| microsoft.aad.directory/applications/owners/read | 讀取 Azure Active Directory 中的 applications.owners 屬性。 |
| microsoft.aad.directory/applications/permissions/read | 讀取 Azure Active Directory 中的 applications.permissions 屬性。 |
| microsoft.aad.directory/applications/policies/read | 讀取 Azure Active Directory 中的 applications.policies 屬性。 |
| microsoft.aad.directory/contacts/basic/read | 讀取 Azure Active Directory 中 contacts 的基本屬性。 |
| microsoft.aad.directory/contacts/memberOf/read | 讀取 Azure Active Directory 中的 contacts.memberOf 屬性。 |
| microsoft.aad.directory/contracts/basic/read | 讀取 Azure Active Directory 中 contracts 的基本屬性。 |
| microsoft.aad.directory/devices/basic/read | 讀取 Azure Active Directory 中 devices 的基本屬性。 |
| microsoft.aad.directory/devices/memberOf/read | 讀取 Azure Active Directory 中的 devices.memberOf 屬性。 |
| microsoft.aad.directory/devices/registeredOwners/read | 讀取 Azure Active Directory 中的 devices.registeredOwners 屬性。 |
| microsoft.aad.directory/devices/registeredUsers/read | 讀取 Azure Active Directory 中的 devices.registeredUsers 屬性。 |
| microsoft.aad.directory/directoryRoles/basic/read | 讀取 Azure Active Directory 中 directoryRoles 的基本屬性。 |
| microsoft.aad.directory/directoryRoles/eligibleMembers/read | 讀取 Azure Active Directory 中的 directoryRoles.eligibleMembers 屬性。 |
| microsoft.aad.directory/directoryRoles/members/read | 讀取 Azure Active Directory 中的 directoryRoles.members 屬性。 |
| microsoft.aad.directory/domains/basic/read | 讀取 Azure Active Directory 中 domain 的基本屬性。 |
| microsoft.aad.directory/groups/appRoleAssignments/read | 讀取 Azure Active Directory 中的 groups.appRoleAssignments 屬性。 |
| microsoft.aad.directory/groups/basic/read | 讀取 Azure Active Directory 中 groups 的基本屬性。 |
| microsoft.aad.directory/groups/memberOf/read | 讀取 Azure Active Directory 中的 Read groups.memberOf 屬性。 |
| microsoft.aad.directory/groups/members/read | 讀取 Azure Active Directory 中的 groups.members 屬性。 |
| microsoft.aad.directory/groups/owners/read | 讀取 Azure Active Directory 中的 groups.owners 屬性。 |
| microsoft.aad.directory/groups/settings/read | 讀取 Azure Active Directory 中的 groups.settings 屬性。 |
| microsoft.aad.directory/groupSettings/basic/read | 讀取 Azure Active Directory 中 groupSettings 的基本屬性。 |
| microsoft.aad.directory/groupSettingTemplates/basic/read | 讀取 Azure Active Directory 中 groupSettingTemplates 的基本屬性。 |
| microsoft.aad.directory/oAuth2PermissionGrants/basic/read | 讀取 Azure Active Directory 中 oAuth2PermissionGrants 的基本屬性。 |
| microsoft.aad.directory/organization/basic/read | 讀取 Azure Active Directory 中 organization 的基本屬性。 |
| microsoft.aad.directory/organization/trustedCAsForPasswordlessAuth/read | 讀取 Azure Active Directory 中的 organization.trustedCAsForPasswordlessAuth 屬性。 |
| microsoft.aad.directory/roleAssignments/basic/read | 讀取 Azure Active Directory 中 roleAssignments 的基本屬性。 |
| microsoft.aad.directory/roleDefinitions/basic/read | 讀取 Azure Active Directory 中 roleDefinitions 的基本屬性。 |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/read | 讀取 Azure Active Directory 中的 servicePrincipals.appRoleAssignedTo 屬性。 |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/read | 讀取 Azure Active Directory 中的 servicePrincipals.appRoleAssignments 屬性。 |
| microsoft.aad.directory/servicePrincipals/basic/read | 讀取 Azure Active Directory 中 servicePrincipals 的基本屬性。 |
| microsoft.aad.directory/servicePrincipals/memberOf/read | 讀取 Azure Active Directory 中的 servicePrincipals.memberOf 屬性。 |
| microsoft.aad.directory/servicePrincipals/oAuth2PermissionGrants/basic/read | 讀取 Azure Active Directory 中的 servicePrincipals.oAuth2PermissionGrants 屬性。 |
| microsoft.aad.directory/servicePrincipals/ownedObjects/read | 讀取 Azure Active Directory 中的 servicePrincipals.ownedObjects 屬性。 |
| microsoft.aad.directory/servicePrincipals/owners/read | 讀取 Azure Active Directory 中的 servicePrincipals.owners 屬性。 |
| microsoft.aad.directory/servicePrincipals/policies/read | 讀取 Azure Active Directory 中的 servicePrincipals.policies 屬性。 |
| microsoft.aad.directory/subscribedSkus/basic/read | 讀取 Azure Active Directory 中 subscribedSkus 的基本屬性。 |
| microsoft.aad.directory/users/appRoleAssignments/read | 讀取 Azure Active Directory 中的 users.appRoleAssignments 屬性。 |
| microsoft.aad.directory/users/basic/read | 讀取 Azure Active Directory 中 users 的基本屬性。 |
| microsoft.aad.directory/users/directReports/read | 讀取 Azure Active Directory 中的 users.directReports 屬性。 |
| microsoft.aad.directory/users/invitedBy/read | 讀取 Azure Active Directory 中的 users.invitedBy 屬性。 |
| microsoft.aad.directory/users/invitedUsers/read | 讀取 Azure Active Directory 中的 users.invitedUsers 屬性。 |
| microsoft.aad.directory/users/manager/read | 讀取 Azure Active Directory 中的 users.manager 屬性。 |
| microsoft.aad.directory/users/memberOf/read | 讀取 Azure Active Directory 中的 users.memberOf 屬性。 |
| microsoft.aad.directory/users/oAuth2PermissionGrants/basic/read | 讀取 Azure Active Directory 中的 users.oAuth2PermissionGrants 屬性。 |
| microsoft.aad.directory/users/ownedDevices/read | 讀取 Azure Active Directory 中的 users.ownedDevices 屬性。 |
| microsoft.aad.directory/users/ownedObjects/read | 讀取 Azure Active Directory 中的 users.ownedObjects 屬性。 |
| microsoft.aad.directory/users/registeredDevices/read | 讀取 Azure Active Directory 中的 users.registeredDevices 屬性。 |

### <a name="directory-synchronization-accounts"></a>目錄同步處理帳戶
僅供 Azure AD Connect 服務使用。

  > [!NOTE]
  > 此角色會繼承角色的其他權限。
  >
  >

| **動作** | **說明** |
| --- | --- |
| microsoft.aad.directory/organization/dirSync/update | 更新 Azure Active Directory 中的 organization.dirSync 屬性。 |
| microsoft.aad.directory/policies/create | 在 Azure Active Directory 中建立原則。 |
| microsoft.aad.directory/policies/delete | 刪除 Azure Active Directory 中的原則。 |
| microsoft.aad.directory/policies/basic/read | 讀取 Azure Active Directory 中 policies 的基本屬性。 |
| microsoft.aad.directory/policies/basic/update | 更新 Azure Active Directory 中 policies 的基本屬性。 |
| microsoft.aad.directory/policies/owners/read | 讀取 Azure Active Directory 中的 policies.owners 屬性。 |
| microsoft.aad.directory/policies/owners/update | 更新 Azure Active Directory 中的 policies.owners 屬性。 |
| microsoft.aad.directory/policies/policiesAppliedTo/read | 讀取 Azure Active Directory 中的 policies.policiesAppliedTo 屬性。 |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/read | 讀取 Azure Active Directory 中的 servicePrincipals.appRoleAssignedTo 屬性。 |
| microsoft.aad.directory/servicePrincipals/appRoleAssignedTo/update | 更新 Azure Active Directory 中的 servicePrincipals.appRoleAssignedTo 屬性。 |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/read | 讀取 Azure Active Directory 中的 servicePrincipals.appRoleAssignments 屬性。 |
| microsoft.aad.directory/servicePrincipals/appRoleAssignments/update | 更新 Azure Active Directory 中的 servicePrincipals.appRoleAssignments 屬性。 |
| microsoft.aad.directory/servicePrincipals/basic/read | 讀取 Azure Active Directory 中 servicePrincipals 的基本屬性。 |
| microsoft.aad.directory/servicePrincipals/basic/update | 更新 Azure Active Directory 中 servicePrincipals 的基本屬性。 |
| microsoft.aad.directory/servicePrincipals/create | 在 Azure Active Directory 中建立 servicePrincipals。 |
| microsoft.aad.directory/servicePrincipals/memberOf/read | 讀取 Azure Active Directory 中的 servicePrincipals.memberOf 屬性。 |
| microsoft.aad.directory/servicePrincipals/oAuth2PermissionGrants/basic/read | 讀取 Azure Active Directory 中的 servicePrincipals.oAuth2PermissionGrants 屬性。 |
| microsoft.aad.directory/servicePrincipals/owners/read | 讀取 Azure Active Directory 中的 servicePrincipals.owners 屬性。 |
| microsoft.aad.directory/servicePrincipals/owners/update | 更新 Azure Active Directory 中的 servicePrincipals.owners 屬性。 |
| microsoft.aad.directory/servicePrincipals/ownedObjects/read | 讀取 Azure Active Directory 中的 servicePrincipals.ownedObjects 屬性。 |
| microsoft.aad.directory/servicePrincipals/policies/read | 讀取 Azure Active Directory 中的 servicePrincipals.policies 屬性。 |
| microsoft.aad.directory/servicePrincipals/policies/update | 更新 Azure Active Directory 中的 servicePrincipals.policies 屬性。 |
| microsoft.aad.directorySync/allEntities/allTasks | 執行 Azure AD Connect 中的所有動作。 |

### <a name="directory-writers"></a>目錄撰寫者
可讀取和寫入基本目錄資訊。 用來授與應用程式的存取權，不適用於使用者。

  > [!NOTE]
  > 此角色會繼承目錄讀取者角色的其他權限。
  >
  >

| **動作** | **說明** |
| --- | --- |
| microsoft.aad.directory/groups/create | 在 Azure Active Directory 中建立 groups。 |
| microsoft.aad.directory/groups/createAsOwner | 在 Azure Active Directory 中建立 groups。 建立者會新增為第一個擁有者，而建立的物件會算在建立者的 250 個建立物件配額中。 |
| microsoft.aad.directory/groups/appRoleAssignments/update | 更新 Azure Active Directory 中的 groups.appRoleAssignments 屬性。 |
| microsoft.aad.directory/groups/basic/update | 更新 Azure Active Directory 中 groups 的基本屬性。 |
| microsoft.aad.directory/groups/members/update | 更新 Azure Active Directory 中的 groups.members 屬性。 |
| microsoft.aad.directory/groups/owners/update | 更新 Azure Active Directory 中的 groups.owners 屬性。 |
| microsoft.aad.directory/groups/settings/update | 更新 Azure Active Directory 中的 groups.settings 屬性。 |
| microsoft.aad.directory/groupSettings/basic/update | 更新 Azure Active Directory 中 groupSettings 的基本屬性。 |
| microsoft.aad.directory/groupSettings/create | 在 Azure Active Directory 中建立 groupSettings。 |
| microsoft.aad.directory/groupSettings/delete | 在 Azure Active Directory 中刪除 groupSettings。 |
| microsoft.aad.directory/users/appRoleAssignments/update | 更新 Azure Active Directory 中的 users.appRoleAssignments 屬性。 |
| microsoft.aad.directory/users/assignLicense | 管理 Azure Active Directory 中的使用者授權。 |
| microsoft.aad.directory/users/basic/update | 更新 Azure Active Directory 中 users 的基本屬性。 |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | 使 Azure Active Directory 中的所有使用者重新整理權杖失效。 |
| microsoft.aad.directory/users/manager/update | 更新 Azure Active Directory 中的 users.manager 屬性。 |
| microsoft.aad.directory/users/userPrincipalName/update | 更新 Azure Active Directory 中的 users.userPrincipalName 屬性。 |

### <a name="dynamics-365-administrator"></a>Dynamics 365 系統管理員
可管理 Dynamics 365 產品的所有層面。 在 Microsoft Graph API、Azure AD Graph API 和 Azure AD PowerShell 中，會將此角色識別為「Dynamics 365 服務管理員」。 在 Azure 入口網站中則是「Dynamics 365 管理員」。


  > [!NOTE]
  > 此角色會繼承目錄讀取者角色的其他權限。
  >
  > 此角色還具有 Azure Active Directory 以外的其他權限。 如需詳細資訊，請參閱前述角色說明。
  >
  >

| **動作** | **說明** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | 管理 Azure 存取服務的所有層面。 |
| microsoft.azure.serviceHealth/allEntities/allTasks | 讀取及設定 Azure 服務健康情況。 |
| microsoft.azure.supportTickets/allEntities/allTasks | 建立和管理 Azure 支援票證。 |
| microsoft.powerApps.dynamics365/allEntities/allTasks | 管理 Dynamics 365 的所有層面。 |
| microsoft.office365.serviceHealth/allEntities/allTasks | 讀取及設定 Office 365 服務健康情況。 |
| microsoft.office365.supportTickets/allEntities/allTasks | 建立和管理 Office 365 支援票證。 |

### <a name="exchange-administrator"></a>Exchange 系統管理員
可管理 Exchange 產品的所有層面。 在 Microsoft Graph API、Azure AD Graph API 和 Azure AD PowerShell 中，會將此角色識別為「Exchange 服務管理員」。 在 Azure 入口網站中則是「Exchange 管理員」。

  > [!NOTE]
  > 此角色會繼承目錄讀取者角色的其他權限。
  >
  >

  > [!NOTE]
  > 此角色具有 Azure Active Directory 以外的其他權限。 如需詳細資訊，請參閱前述角色說明。
  >
  >

| **動作** | **說明** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | 管理 Azure 存取服務的所有層面。 |
| microsoft.azure.serviceHealth/allEntities/allTasks | 讀取及設定 Azure 服務健康情況。 |
| microsoft.azure.supportTickets/allEntities/allTasks | 建立和管理 Azure 支援票證。 |
| microsoft.aad.directory/groups/unified/create | 建立 Office 365 群組。 |
| microsoft.aad.directory/groups/unified/delete | 刪除 Office 365 群組。 |
| microsoft.aad.directory/groups/unified/basic/update | 更新 Office 365 群組的基本屬性。 |
| microsoft.aad.directory/groups/unified/members/update | 更新 Office 365 群組的成員資格。 |
| microsoft.aad.directory/groups/unified/owners/update | 更新 Office 365 群組的擁有權。 |
| microsoft.office365.exchange/allEntities/allTasks | 管理 Exchange Online 的所有層面。 |
| microsoft.office365.serviceHealth/allEntities/allTasks | 讀取及設定 Office 365 服務健康情況。 |
| microsoft.office365.supportTickets/allEntities/allTasks | 建立和管理 Office 365 支援票證。 |

### <a name="guest-inviter"></a>來賓邀請者
能夠邀請不受 [成員能夠邀請來賓] 設定限制的來賓使用者。

  > [!NOTE]
  > 此角色會繼承角色的其他權限。
  >
  >

| **動作** | **說明** |
| --- | --- |
| microsoft.aad.directory/users/appRoleAssignments/read | 讀取 Azure Active Directory 中的 users.appRoleAssignments 屬性。 |
| microsoft.aad.directory/users/basic/read | 讀取 Azure Active Directory 中 users 的基本屬性。 |
| microsoft.aad.directory/users/directReports/read | 讀取 Azure Active Directory 中的 users.directReports 屬性。 |
| microsoft.aad.directory/users/invitedBy/read | 讀取 Azure Active Directory 中的 users.invitedBy 屬性。 |
| microsoft.aad.directory/users/inviteGuest | 邀請 Azure Active Directory 中的來賓使用者。 |
| microsoft.aad.directory/users/invitedUsers/read | 讀取 Azure Active Directory 中的 users.invitedUsers 屬性。 |
| microsoft.aad.directory/users/manager/read | 讀取 Azure Active Directory 中的 users.manager 屬性。 |
| microsoft.aad.directory/users/memberOf/read | 讀取 Azure Active Directory 中的 users.memberOf 屬性。 |
| microsoft.aad.directory/users/oAuth2PermissionGrants/basic/read | 讀取 Azure Active Directory 中的 users.oAuth2PermissionGrants 屬性。 |
| microsoft.aad.directory/users/ownedDevices/read | 讀取 Azure Active Directory 中的 users.ownedDevices 屬性。 |
| microsoft.aad.directory/users/ownedObjects/read | 讀取 Azure Active Directory 中的 users.ownedObjects 屬性。 |
| microsoft.aad.directory/users/registeredDevices/read | 讀取 Azure Active Directory 中的 users.registeredDevices 屬性。 |

### <a name="helpdesk-administrator"></a>服務台系統管理員
能夠為非系統管理員與技術服務人員系統管理員重設密碼。

  > [!NOTE]
  > 此角色會繼承目錄讀取者角色的其他權限。
  >
  >

| **動作** | **說明** |
| --- | --- |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | 使 Azure Active Directory 中的所有使用者重新整理權杖失效。 |
| microsoft.aad.directory/users/password/update | 在 Azure Active Directory 中更新所有使用者的密碼。 如需詳細資訊，請參閱線上文件。 |
| microsoft.azure.accessService/allEntities/allTasks | 管理 Azure 存取服務的所有層面。 |
| microsoft.azure.serviceHealth/allEntities/allTasks | 讀取及設定 Azure 服務健康情況。 |
| microsoft.azure.supportTickets/allEntities/allTasks | 建立和管理 Azure 支援票證。 |
| microsoft.office365.serviceHealth/allEntities/allTasks | 讀取及設定 Office 365 服務健康情況。 |
| microsoft.office365.supportTickets/allEntities/allTasks | 建立和管理 Office 365 支援票證。 |

### <a name="information-protection-administrator"></a>資訊保護管理員
可管理 Azure 資訊保護產品的所有層面。

  > [!NOTE]
  > 此角色會繼承目錄讀取者角色的其他權限。
  >
  >

  > [!NOTE]
  > 此角色具有 Azure Active Directory 以外的其他權限。 如需詳細資訊，請參閱前述角色說明。
  >
  >

| **動作** | **說明** |
| --- | --- |
| microsoft.azure.informationProtection/allEntities/allTasks | 管理 Azure 資訊保護的所有層面。 |
| microsoft.azure.serviceHealth/allEntities/allTasks | 讀取及設定 Azure 服務健康情況。 |
| microsoft.azure.supportTickets/allEntities/allTasks | 建立和管理 Azure 支援票證。 |
| microsoft.office365.serviceHealth/allEntities/allTasks | 讀取及設定 Office 365 服務健康情況。 |
| microsoft.office365.supportTickets/allEntities/allTasks | 建立和管理 Office 365 支援票證。 |

### <a name="intune-administrator"></a>Intune 管理員
可管理 Intune 產品的所有層面。 在 Microsoft Graph API、Azure AD Graph API 和 Azure AD PowerShell 中，會將此角色識別為「Intune 服務管理員」。 在 Azure 入口網站中則是「Intune 管理員」。

  > [!NOTE]
  > 此角色會繼承目錄讀取者角色的其他權限。
  >
  >

  > [!NOTE]
  > 此角色具有 Azure Active Directory 以外的其他權限。 如需詳細資訊，請參閱前述角色說明。
  >
  >

| **動作** | **說明** |
| --- | --- |
| microsoft.aad.directory/contacts/basic/update | 更新 Azure Active Directory 中 contacts 的基本屬性。 |
| microsoft.aad.directory/contacts/create | 在 Azure Active Directory 中建立 contacts。 |
| microsoft.aad.directory/contacts/delete | 刪除 Azure Active Directory 中的 contacts。 |
| microsoft.aad.directory/devices/basic/update | 更新 Azure Active Directory 中 devices 的基本屬性。 |
| microsoft.aad.directory/devices/create | 在 Azure Active Directory 中建立 devices。 |
| microsoft.aad.directory/devices/delete | 刪除 Azure Active Directory 中的 devices。 |
| microsoft.aad.directory/devices/registeredOwners/update | 更新 Azure Active Directory 中的 devices.registeredOwners 屬性。 |
| microsoft.aad.directory/devices/registeredUsers/update | 更新 Azure Active Directory 中的 devices.registeredUsers 屬性。 |
| microsoft.aad.directory/groups/appRoleAssignments/update | 更新 Azure Active Directory 中的 groups.appRoleAssignments 屬性。 |
| microsoft.aad.directory/groups/basic/update | 更新 Azure Active Directory 中 groups 的基本屬性。 |
| microsoft.aad.directory/groups/create | 在 Azure Active Directory 中建立 groups。 |
| microsoft.aad.directory/groups/createAsOwner | 在 Azure Active Directory 中建立 groups。 建立者會新增為第一個擁有者，而建立的物件會算在建立者的 250 個建立物件配額中。 |
| microsoft.aad.directory/groups/delete | 刪除 Azure Active Directory 中的 groups。 |
| microsoft.aad.directory/groups/hiddenMembers/read | 讀取 Azure Active Directory 中的 groups.hiddenMembers 屬性。 |
| microsoft.aad.directory/groups/members/update | 更新 Azure Active Directory 中的 groups.members 屬性。 |
| microsoft.aad.directory/groups/owners/update | 更新 Azure Active Directory 中的 groups.owners 屬性。 |
| microsoft.aad.directory/groups/restore | 還原 Azure Active Directory 中的 groups。 |
| microsoft.aad.directory/groups/settings/update | 更新 Azure Active Directory 中的 groups.settings 屬性。 |
| microsoft.aad.directory/users/appRoleAssignments/update | 更新 Azure Active Directory 中的 users.appRoleAssignments 屬性。 |
| microsoft.aad.directory/users/basic/update | 更新 Azure Active Directory 中 users 的基本屬性。 |
| microsoft.aad.directory/users/manager/update | 更新 Azure Active Directory 中的 users.manager 屬性。 |
| microsoft.azure.supportTickets/allEntities/allTasks | 建立和管理 Azure 支援票證。 |
| microsoft.intune/allEntities/allTasks | 管理 Intune 的所有層面。 |
| microsoft.office365.supportTickets/allEntities/allTasks | 建立和管理 Office 365 支援票證。 |

### <a name="license-administrator"></a>授權管理員
可管理使用者和群組的產品授權。

  > [!NOTE]
  > 此角色會繼承目錄讀取者角色的其他權限。
  >
  >

| **動作** | **說明** |
| --- | --- |
| microsoft.aad.directory/users/assignLicense | 管理 Azure Active Directory 中的使用者授權。 |
| microsoft.aad.directory/users/usageLocation/update | 更新 Azure Active Directory 中的 users.usageLocation 屬性。 |
| microsoft.azure.accessService/allEntities/allTasks | 管理 Azure 存取服務的所有層面。 |
| microsoft.azure.serviceHealth/allEntities/allTasks | 讀取及設定 Azure 服務健康情況。 |
| microsoft.office365.serviceHealth/allEntities/allTasks | 讀取及設定 Office 365 服務健康情況。 |

### <a name="skype-for-business-administrator"></a>商務用 Skype 的管理員
可管理商務用 Skype 產品的所有層面。 在 Microsoft Graph API、Azure AD Graph API 和 Azure AD PowerShell 中，會將此角色識別為「商務用 Skype 服務管理員」。 在 Azure 入口網站中則是「商務用 Skype 管理員」。

  > [!NOTE]
  > 此角色會繼承目錄讀取者角色的其他權限。
  >
  >

  > [!NOTE]
  > 此角色具有 Azure Active Directory 以外的其他權限。 如需詳細資訊，請參閱前述角色說明。
  >
  >

| **動作** | **說明** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | 管理 Azure 存取服務的所有層面。 |
| microsoft.azure.serviceHealth/allEntities/allTasks | 讀取及設定 Azure 服務健康情況。 |
| microsoft.azure.supportTickets/allEntities/allTasks | 建立和管理 Azure 支援票證。 |
| microsoft.office365.serviceHealth/allEntities/allTasks | 讀取及設定 Office 365 服務健康情況。 |
| microsoft.office365.skypeForBusiness/allEntities/allTasks | 管理商務用 Skype Online 的所有層面。 |
| microsoft.office365.supportTickets/allEntities/allTasks | 建立和管理 Office 365 支援票證。 |

### <a name="message-center-reader"></a>訊息中心讀取者
只可在 Office 365 訊息中心讀取及更新其組織的訊息。 

  > [!NOTE]
  > 此角色會繼承目錄讀取者角色的其他權限。
  >
  >

  > [!NOTE]
  > 此角色具有 Azure Active Directory 以外的其他權限。 如需詳細資訊，請參閱前述角色說明。
  >
  >

| **動作** | **說明** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | 管理 Azure 存取服務的所有層面。 |
| microsoft.office365.messageCenter/messages/read | 讀取 microsoft.office365.messageCenter 中的訊息。 |

### <a name="partner-tier1-support"></a>合作夥伴第 1 層支援
請勿使用 - 不適用於一般用途。

  > [!NOTE]
  > 此角色會繼承目錄讀取者角色的其他權限。
  >
  >

  > [!NOTE]
  > 此角色具有 Azure Active Directory 以外的其他權限。 如需詳細資訊，請參閱前述角色說明。
  >
  >

| **動作** | **說明** |
| --- | --- |
| microsoft.aad.directory/contacts/basic/update | 更新 Azure Active Directory 中 contacts 的基本屬性。 |
| microsoft.aad.directory/contacts/create | 在 Azure Active Directory 中建立 contacts。 |
| microsoft.aad.directory/contacts/delete | 刪除 Azure Active Directory 中的 contacts。 |
| microsoft.aad.directory/groups/create | 在 Azure Active Directory 中建立 groups。 |
| microsoft.aad.directory/groups/createAsOwner | 在 Azure Active Directory 中建立 groups。 建立者會新增為第一個擁有者，而建立的物件會算在建立者的 250 個建立物件配額中。 |
| microsoft.aad.directory/groups/members/update | 更新 Azure Active Directory 中的 groups.members 屬性。 |
| microsoft.aad.directory/groups/owners/update | 更新 Azure Active Directory 中的 groups.owners 屬性。 |
| microsoft.aad.directory/users/appRoleAssignments/update | 更新 Azure Active Directory 中的 users.appRoleAssignments 屬性。 |
| microsoft.aad.directory/users/assignLicense | 管理 Azure Active Directory 中的使用者授權。 |
| microsoft.aad.directory/users/basic/update | 更新 Azure Active Directory 中 users 的基本屬性。 |
| microsoft.aad.directory/users/delete | 刪除 Azure Active Directory 中的 users。 |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | 使 Azure Active Directory 中的所有使用者重新整理權杖失效。 |
| microsoft.aad.directory/users/manager/update | 更新 Azure Active Directory 中的 users.manager 屬性。 |
| microsoft.aad.directory/users/password/update | 在 Azure Active Directory 中更新所有使用者的密碼。 如需詳細資訊，請參閱線上文件。 |
| microsoft.aad.directory/users/restore | 還原 Azure Active Directory 中已刪除的使用者。 |
| microsoft.aad.directory/users/userPrincipalName/update | 更新 Azure Active Directory 中的 users.userPrincipalName 屬性。 |
| microsoft.azure.accessService/allEntities/allTasks | 管理 Azure 存取服務的所有層面。 |
| microsoft.azure.serviceHealth/allEntities/allTasks | 讀取及設定 Azure 服務健康情況。 |
| microsoft.azure.supportTickets/allEntities/allTasks | 建立和管理 Azure 支援票證。 |
| microsoft.office365.serviceHealth/allEntities/allTasks | 讀取及設定 Office 365 服務健康情況。 |
| microsoft.office365.supportTickets/allEntities/allTasks | 建立和管理 Office 365 支援票證。 |

### <a name="partner-tier2-support"></a>合作夥伴第 2 層支援
請勿使用 - 不適用於一般用途。

  > [!NOTE]
  > 此角色會繼承目錄讀取者角色的其他權限。
  >
  >

  > [!NOTE]
  > 此角色具有 Azure Active Directory 以外的其他權限。 如需詳細資訊，請參閱前述角色說明。
  >
  >

| **動作** | **說明** |
| --- | --- |
| microsoft.aad.directory/contacts/basic/update | 更新 Azure Active Directory 中 contacts 的基本屬性。 |
| microsoft.aad.directory/contacts/create | 在 Azure Active Directory 中建立 contacts。 |
| microsoft.aad.directory/contacts/delete | 刪除 Azure Active Directory 中的 contacts。 |
| microsoft.aad.directory/domains/allTasks | 建立和刪除 domains，以及在 Azure Active Directory 中讀取和更新標準屬性。 |
| microsoft.aad.directory/groups/create | 在 Azure Active Directory 中建立 groups。 |
| microsoft.aad.directory/groups/delete | 刪除 Azure Active Directory 中的 groups。 |
| microsoft.aad.directory/groups/members/update | 更新 Azure Active Directory 中的 groups.members 屬性。 |
| microsoft.aad.directory/groups/restore | 還原 Azure Active Directory 中的 groups。 |
| microsoft.aad.directory/organization/basic/update | 更新 Azure Active Directory 中 organization 的基本屬性。 |
| microsoft.aad.directory/organization/trustedCAsForPasswordlessAuth/update | 更新 Azure Active Directory 中的 organization.trustedCAsForPasswordlessAuth 屬性。 |
| microsoft.aad.directory/users/appRoleAssignments/update | 更新 Azure Active Directory 中的 users.appRoleAssignments 屬性。 |
| microsoft.aad.directory/users/assignLicense | 管理 Azure Active Directory 中的使用者授權。 |
| microsoft.aad.directory/users/basic/update | 更新 Azure Active Directory 中 users 的基本屬性。 |
| microsoft.aad.directory/users/delete | 刪除 Azure Active Directory 中的 users。 |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | 使 Azure Active Directory 中的所有使用者重新整理權杖失效。 |
| microsoft.aad.directory/users/manager/update | 更新 Azure Active Directory 中的 users.manager 屬性。 |
| microsoft.aad.directory/users/password/update | 在 Azure Active Directory 中更新所有使用者的密碼。 如需詳細資訊，請參閱線上文件。 |
| microsoft.aad.directory/users/restore | 還原 Azure Active Directory 中已刪除的使用者。 |
| microsoft.aad.directory/users/userPrincipalName/update | 更新 Azure Active Directory 中的 users.userPrincipalName 屬性。 |
| microsoft.azure.accessService/allEntities/allTasks | 管理 Azure 存取服務的所有層面。 |
| microsoft.azure.serviceHealth/allEntities/allTasks | 讀取及設定 Azure 服務健康情況。 |
| microsoft.azure.supportTickets/allEntities/allTasks | 建立和管理 Azure 支援票證。 |
| microsoft.office365.serviceHealth/allEntities/allTasks | 讀取及設定 Office 365 服務健康情況。 |
| microsoft.office365.supportTickets/allEntities/allTasks | 建立和管理 Office 365 支援票證。 |

### <a name="power-bi-administrator"></a>Power BI 管理員
可管理 Power BI 產品的所有層面。 在 Microsoft Graph API、Azure AD Graph API 和 Azure AD PowerShell 中，會將此角色識別為「Power BI 服務管理員」。 在 Azure 入口網站中則是「Power BI 管理員」。

  > [!NOTE]
  > 此角色會繼承目錄讀取者角色的其他權限。
  >
  >

  > [!NOTE]
  > 此角色具有 Azure Active Directory 以外的其他權限。 如需詳細資訊，請參閱前述角色說明。
  >
  >

| **動作** | **說明** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | 管理 Azure 存取服務的所有層面。 |
| microsoft.azure.serviceHealth/allEntities/allTasks | 讀取及設定 Azure 服務健康情況。 |
| microsoft.azure.supportTickets/allEntities/allTasks | 建立和管理 Azure 支援票證。 |
| microsoft.powerApps.powerBI/allEntities/allTasks | 管理 Power BI 的所有層面。 |
| microsoft.office365.serviceHealth/allEntities/allTasks | 讀取及設定 Office 365 服務健康情況。 |
| microsoft.office365.supportTickets/allEntities/allTasks | 建立和管理 Office 365 支援票證。 |

### <a name="privileged-role-administrator"></a>特殊權限角色管理員
能夠管理 Azure AD 中的角色指派，以及 Privileged Identity Management 的所有層面。

  > [!NOTE]
  > 此角色會繼承目錄讀取者角色的其他權限。
  >
  >

  > [!NOTE]
  > 此角色具有 Azure Active Directory 以外的其他權限。 如需詳細資訊，請參閱前述角色說明。
  >
  >

| **動作** | **說明** |
| --- | --- |
| microsoft.aad.directory/directoryRoles/update | 更新 Azure Active Directory 中的 directoryRoles。 |
| microsoft.aad.privilegedIdentityManagement/allEntities/allTasks | 建立和刪除所有資源，以及讀取和更新 microsoft.aad.privilegedIdentityManagement 中的標準屬性。 |

### <a name="reports-reader"></a>報告讀者
可讀取登入與稽核報告。

  > [!NOTE]
  > 此角色會繼承目錄讀取者角色的其他權限。
  >
  >

  > [!NOTE]
  > 此角色具有 Azure Active Directory 以外的其他權限。 如需詳細資訊，請參閱前述角色說明。
  >
  >

| **動作** | **說明** |
| --- | --- |
| microsoft.aad.reports/allEntities/read | 讀取 Azure AD 報告。 |
| microsoft.azure.serviceHealth/allEntities/allTasks | 讀取及設定 Azure 服務健康情況。 |
| microsoft.office365.serviceHealth/allEntities/allTasks | 讀取及設定 Office 365 服務健康情況。 |
| microsoft.office365.usageReports/allEntities/read | 讀取 Office 365 使用量報告。 |

### <a name="security-administrator"></a>安全性系統管理員
能夠讀取安全性資訊與報表，以及管理 Azure AD 與 Office 365 中的設定。

  > [!NOTE]
  > 此角色會繼承目錄讀取者角色的其他權限。
  >
  >

  > [!NOTE]
  > 此角色具有 Azure Active Directory 以外的其他權限。 如需詳細資訊，請參閱前述角色說明。
  >
  >

| **動作** | **說明** |
| --- | --- |
| microsoft.aad.directory/applications/policies/update | 更新 Azure Active Directory 中的 applications.policies 屬性。 |
| microsoft.aad.directory/policies/basic/update | 更新 Azure Active Directory 中 policies 的基本屬性。 |
| microsoft.aad.directory/policies/create | 在 Azure Active Directory 中建立原則。 |
| microsoft.aad.directory/policies/delete | 刪除 Azure Active Directory 中的原則。 |
| microsoft.aad.directory/policies/owners/update | 更新 Azure Active Directory 中的 policies.owners 屬性。 |
| microsoft.aad.directory/servicePrincipals/policies/update | 更新 Azure Active Directory 中的 servicePrincipals.policies 屬性。 |
| microsoft.aad.identityProtection/allEntities/read | 讀取 microsoft.aad.identityProtection 中的所有資源。 |
| microsoft.aad.identityProtection/allEntities/update | 更新 microsoft.aad.identityProtection 中的所有資源。 |
| microsoft.aad.privilegedIdentityManagement/allEntities/read | 讀取 microsoft.aad.privilegedIdentityManagement 中的所有資源。 |
| microsoft.azure.accessService/allEntities/allTasks | 管理 Azure 存取服務的所有層面。 |
| microsoft.azure.serviceHealth/allEntities/allTasks | 讀取及設定 Azure 服務健康情況。 |
| microsoft.office365.protectionCenter/allEntities/read | 讀取 Office 365 防護中心的所有層面。 |
| microsoft.office365.protectionCenter/allEntities/update | 更新 microsoft.office365.protectionCenter 中的所有資源。 |
| microsoft.office365.serviceHealth/allEntities/allTasks | 讀取及設定 Office 365 服務健康情況。 |

### <a name="security-reader"></a>安全性讀取者
可讀取安全性資訊及 Azure AD 與 Office 365 中的報告。

  > [!NOTE]
  > 此角色會繼承目錄讀取者角色的其他權限。
  >
  >

  > [!NOTE]
  > 此角色具有 Azure Active Directory 以外的其他權限。 如需詳細資訊，請參閱前述角色說明。
  >
  >

| **動作** | **說明** |
| --- | --- |
| microsoft.aad.identityProtection/allEntities/read | 讀取 microsoft.aad.identityProtection 中的所有資源。 |
| microsoft.aad.privilegedIdentityManagement/allEntities/read | 讀取 microsoft.aad.privilegedIdentityManagement 中的所有資源。 |
| microsoft.azure.accessService/allEntities/allTasks | 管理 Azure 存取服務的所有層面。 |
| microsoft.azure.serviceHealth/allEntities/allTasks | 讀取及設定 Azure 服務健康情況。 |
| microsoft.office365.protectionCenter/allEntities/read | 讀取 Office 365 防護中心的所有層面。 |
| microsoft.office365.serviceHealth/allEntities/allTasks | 讀取及設定 Office 365 服務健康情況。 |

### <a name="service-support-administrator"></a>服務支援管理員
可讀取服務健康情況資訊及管理支援票證。

  > [!NOTE]
  > 此角色會繼承目錄讀取者角色的其他權限。
  >
  >

  > [!NOTE]
  > 此角色具有 Azure Active Directory 以外的其他權限。 如需詳細資訊，請參閱前述角色說明。
  >
  >

| **動作** | **說明** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | 管理 Azure 存取服務的所有層面。 |
| microsoft.azure.serviceHealth/allEntities/allTasks | 讀取及設定 Azure 服務健康情況。 |
| microsoft.azure.supportTickets/allEntities/allTasks | 建立和管理 Azure 支援票證。 |
| microsoft.office365.serviceHealth/allEntities/allTasks | 讀取及設定 Office 365 服務健康情況。 |
| microsoft.office365.supportTickets/allEntities/allTasks | 建立和管理 Office 365 支援票證。 |

### <a name="sharepoint-administrator"></a>SharePoint 管理員
可管理 SharePoint 服務的所有層面。 在 Microsoft Graph API、Azure AD Graph API 和 Azure AD PowerShell 中，會將此角色識別為「SharePoint 服務管理員」。 在 Azure 入口網站中則是「SharePoint 管理員」。

  > [!NOTE]
  > 此角色會繼承目錄讀取者角色的其他權限。
  >
  >

  > [!NOTE]
  > 此角色具有 Azure Active Directory 以外的其他權限。 如需詳細資訊，請參閱前述角色說明。
  >
  >

| **動作** | **說明** |
| --- | --- |
| microsoft.azure.accessService/allEntities/allTasks | 管理 Azure 存取服務的所有層面。 |
| microsoft.azure.serviceHealth/allEntities/allTasks | 讀取及設定 Azure 服務健康情況。 |
| microsoft.azure.supportTickets/allEntities/allTasks | 建立和管理 Azure 支援票證。 |
| microsoft.aad.directory/groups/unified/delete | 刪除 Office 365 群組。 |
| microsoft.aad.directory/groups/unified/basic/update | 更新 Office 365 群組的基本屬性。 |
| microsoft.aad.directory/groups/unified/members/update | 更新 Office 365 群組的成員資格。 |
| microsoft.aad.directory/groups/unified/owners/update | 更新 Office 365 群組的擁有權。 |
| microsoft.office365.serviceHealth/allEntities/allTasks | 讀取及設定 Office 365 服務健康情況。 |
| microsoft.office365.sharepoint/allEntities/allTasks | 建立和刪除所有資源，以及讀取和更新 microsoft.office365.sharepoint 中的標準屬性。 |
| microsoft.office365.supportTickets/allEntities/allTasks | 建立和管理 Office 365 支援票證。 |

### <a name="teams-communications-administrator"></a>Microsoft Teams 通訊系統管理員
能夠管理 Microsoft Teams 服務內的呼叫和會議功能。

  > [!NOTE]
  > 此角色會繼承目錄讀取者角色的其他權限。
  >
  >

  > [!NOTE]
  > 此角色具有 Azure Active Directory 以外的其他權限。 如需詳細資訊，請參閱前述角色說明。
  >
  >

| **動作** | **說明** |
| --- | --- |
| microsoft.aad.directory/policies/basic/read | 讀取 Azure Active Directory 中 policies 的基本屬性。 |
| microsoft.azure.accessService/allEntities/allTasks | 管理 Azure 存取服務的所有層面。 |
| microsoft.azure.serviceHealth/allEntities/allTasks | 讀取及設定 Azure 服務健康情況。 |
| microsoft.azure.supportTickets/allEntities/allTasks | 建立和管理 Azure 支援票證。 |
| microsoft.office365.serviceHealth/allEntities/allTasks | 讀取及設定 Office 365 服務健康情況。 |
| microsoft.office365.supportTickets/allEntities/allTasks | 建立和管理 Office 365 支援票證。 |
| microsoft.office365.usageReports/allEntities/read | 讀取 Office 365 使用量報告。 |

### <a name="teams-communications-support-engineer"></a>Microsoft Teams 通訊支援工程師
能夠使用進階工具針對 Microsoft Teams 內的通訊問題進行疑難排解。

  > [!NOTE]
  > 此角色會繼承目錄讀取者角色的其他權限。
  >
  >

  > [!NOTE]
  > 此角色具有 Azure Active Directory 以外的其他權限。 如需詳細資訊，請參閱前述角色說明。
  >
  >

| **動作** | **說明** |
| --- | --- |
| microsoft.aad.directory/policies/basic/read | 讀取 Azure Active Directory 中 policies 的基本屬性。 |
| microsoft.azure.accessService/allEntities/allTasks | 管理 Azure 存取服務的所有層面。 |
| microsoft.azure.serviceHealth/allEntities/allTasks | 讀取及設定 Azure 服務健康情況。 |
| microsoft.office365.serviceHealth/allEntities/allTasks | 讀取及設定 Office 365 服務健康情況。 |

### <a name="teams-communications-support-specialist"></a>Microsoft Teams 通訊支援專家
能夠使用基本工具針對 Microsoft Teams 內的通訊問題進行疑難排解。

  > [!NOTE]
  > 此角色會繼承目錄讀取者角色的其他權限。
  >
  >

  > [!NOTE]
  > 此角色具有 Azure Active Directory 以外的其他權限。 如需詳細資訊，請參閱前述角色說明。
  >
  >

| **動作** | **說明** |
| --- | --- |
| microsoft.aad.directory/policies/basic/read | 讀取 Azure Active Directory 中 policies 的基本屬性。 |
| microsoft.azure.accessService/allEntities/allTasks | 管理 Azure 存取服務的所有層面。 |
| microsoft.azure.serviceHealth/allEntities/allTasks | 讀取及設定 Azure 服務健康情況。 |
| microsoft.office365.serviceHealth/allEntities/allTasks | 讀取及設定 Office 365 服務健康情況。 |

### <a name="teams-administrator"></a>Microsoft Teams 管理員
能夠管理 Microsoft Teams 服務。 在 Microsoft Graph API、Azure AD Graph API 和 Azure AD PowerShell 中，會將此角色識別為「Microsoft Teams 服務管理員」。 在 Azure 入口網站中則是「Microsoft Teams 管理員」。

  > [!NOTE]
  > 此角色會繼承目錄讀取者角色的其他權限。
  >
  >

  > [!NOTE]
  > 此角色具有 Azure Active Directory 以外的其他權限。 如需詳細資訊，請參閱前述角色說明。
  >
  >

| **動作** | **說明** |
| --- | --- |
| microsoft.aad.directory/groups/hiddenMembers/read | 讀取 Azure Active Directory 中的 groups.hiddenMembers 屬性。 |
| microsoft.aad.directory/policies/basic/read | 讀取 Azure Active Directory 中 policies 的基本屬性。 |
| microsoft.aad.directory/groups/unified/delete | 刪除 Office 365 群組。 |
| microsoft.aad.directory/groups/unified/basic/update | 更新 Office 365 群組的基本屬性。 |
| microsoft.aad.directory/groups/unified/members/update | 更新 Office 365 群組的成員資格。 |
| microsoft.aad.directory/groups/unified/owners/update | 更新 Office 365 群組的擁有權。 |
| microsoft.azure.accessService/allEntities/allTasks | 管理 Azure 存取服務的所有層面。 |
| microsoft.azure.serviceHealth/allEntities/allTasks | 讀取及設定 Azure 服務健康情況。 |
| microsoft.azure.supportTickets/allEntities/allTasks | 建立和管理 Azure 支援票證。 |
| microsoft.office365.serviceHealth/allEntities/allTasks | 讀取及設定 Office 365 服務健康情況。 |
| microsoft.office365.supportTickets/allEntities/allTasks | 建立和管理 Office 365 支援票證。 |
| microsoft.office365.usageReports/allEntities/read | 讀取 Office 365 使用量報告。 |

### <a name="user-account-administrator"></a>使用者帳戶管理員
能夠管理使用者與群組的所有層面，包含為受限制的管理員重設密碼。

  > [!NOTE]
  > 此角色會繼承目錄讀取者角色的其他權限。
  >
  >

| **動作** | **說明** |
| --- | --- |
| microsoft.aad.directory/appRoleAssignments/create | 在 Azure Active Directory 中建立 appRoleAssignments。 |
| microsoft.aad.directory/appRoleAssignments/delete | 刪除 Azure Active Directory 中的 appRoleAssignments。 |
| microsoft.aad.directory/appRoleAssignments/update | 更新在 Azure Active Directory 中的 appRoleAssignments。 |
| microsoft.aad.directory/contacts/basic/update | 更新 Azure Active Directory 中 contacts 的基本屬性。 |
| microsoft.aad.directory/contacts/create | 在 Azure Active Directory 中建立 contacts。 |
| microsoft.aad.directory/contacts/delete | 刪除 Azure Active Directory 中的 contacts。 |
| microsoft.aad.directory/groups/appRoleAssignments/update | 更新 Azure Active Directory 中的 groups.appRoleAssignments 屬性。 |
| microsoft.aad.directory/groups/basic/update | 更新 Azure Active Directory 中 groups 的基本屬性。 |
| microsoft.aad.directory/groups/create | 在 Azure Active Directory 中建立 groups。 |
| microsoft.aad.directory/groups/createAsOwner | 在 Azure Active Directory 中建立 groups。 建立者會新增為第一個擁有者，而建立的物件會算在建立者的 250 個建立物件配額中。 |
| microsoft.aad.directory/groups/delete | 刪除 Azure Active Directory 中的 groups。 |
| microsoft.aad.directory/groups/hiddenMembers/read | 讀取 Azure Active Directory 中的 groups.hiddenMembers 屬性。 |
| microsoft.aad.directory/groups/members/update | 更新 Azure Active Directory 中的 groups.members 屬性。 |
| microsoft.aad.directory/groups/owners/update | 更新 Azure Active Directory 中的 groups.owners 屬性。 |
| microsoft.aad.directory/groups/restore | 還原 Azure Active Directory 中的 groups。 |
| microsoft.aad.directory/groups/settings/update | 更新 Azure Active Directory 中的 groups.settings 屬性。 |
| microsoft.aad.directory/users/appRoleAssignments/update | 更新 Azure Active Directory 中的 users.appRoleAssignments 屬性。 |
| microsoft.aad.directory/users/assignLicense | 管理 Azure Active Directory 中的使用者授權。 |
| microsoft.aad.directory/users/basic/update | 更新 Azure Active Directory 中 users 的基本屬性。 |
| microsoft.aad.directory/users/create | 在 Azure Active Directory 中建立 users。 |
| microsoft.aad.directory/users/delete | 刪除 Azure Active Directory 中的 users。 |
| microsoft.aad.directory/users/invalidateAllRefreshTokens | 使 Azure Active Directory 中的所有使用者重新整理權杖失效。 |
| microsoft.aad.directory/users/manager/update | 更新 Azure Active Directory 中的 users.manager 屬性。 |
| microsoft.aad.directory/users/password/update | 在 Azure Active Directory 中更新所有使用者的密碼。 如需詳細資訊，請參閱線上文件。 |
| microsoft.aad.directory/users/restore | 還原 Azure Active Directory 中已刪除的使用者。 |
| microsoft.aad.directory/users/userPrincipalName/update | 更新 Azure Active Directory 中的 users.userPrincipalName 屬性。 |
| microsoft.azure.accessService/allEntities/allTasks | 管理 Azure 存取服務的所有層面。 |
| microsoft.azure.serviceHealth/allEntities/allTasks | 讀取及設定 Azure 服務健康情況。 |
| microsoft.azure.supportTickets/allEntities/allTasks | 建立和管理 Azure 支援票證。 |
| microsoft.office365.serviceHealth/allEntities/allTasks | 讀取及設定 Office 365 服務健康情況。 |
| microsoft.office365.supportTickets/allEntities/allTasks | 建立和管理 Office 365 支援票證。 |

## <a name="deprecated-roles"></a>已被取代的角色

以下是不應使用的角色。 它們已被取代，而且未來將從 Azure AD 中移除。

* AdHoc 授權管理員
* 加入裝置
* 裝置管理員
* 裝置使用者
* 傳送電子郵件給經過驗證的使用者建立者
* 信箱管理員
* 加入工作場所裝置

## <a name="next-steps"></a>後續步驟

* 若要深入了解如何將使用者指派為 Azure 訂用帳戶的系統管理員，請參閱[使用 RBAC 和 Azure 入口網站管理存取權](../../role-based-access-control/role-assignments-portal.md)
* 若要深入了解如何在 Microsoft Azure 中控制資源存取，請參閱 [了解 Azure 中的資源存取](../../role-based-access-control/rbac-and-directory-admin-roles.md)
* 如需有關 Azure Active Directory 與您的 Azure 訂用帳戶產生關聯之方式的詳細資訊，請參閱 [Azure 訂用帳戶如何與 Azure Active Directory 產生關聯](../fundamentals/active-directory-how-subscriptions-associated-directory.md)
