---
title: 如何為自訂開發的應用程式變更權杖存留期預設值 | Microsoft Docs
description: 如何為針對 Azure AD 開發的應用程式更新權杖存留期原則
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: celested
ms.openlocfilehash: 4a730c340ea4d1e1137a7449c6d1005ea59917bf
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2018
ms.locfileid: "48814003"
---
# <a name="how-to-change-the-token-lifetime-defaults-for-a-custom-developed-application"></a>如何為自訂開發的應用程式變更權杖存留期預設值

Azure AD Premium 可讓應用程式開發人員與租用戶系統管理員為針對非機密用戶端簽發的權杖設定存留期。 權杖存留期原則是以整個租用戶為基礎所設定，或針對要存取的資源所設定。

1. 若要設定權杖存留期原則，您必須下載 [Azure AD PowerShell 模組 (英文)](https://www.powershellgallery.com/packages/AzureADPreview)。
1. 執行 **Connect-AzureAD -Confirm** 命令。

    下列範例原則會設定單一要素重新整理權杖存留期上限。 建立原則：```New-AzureADPolicy -Definition @('{"TokenLifetimePolicy":{"Version":1, "MaxAgeSingleFactor":"until-revoked"}}') -DisplayName "OrganizationDefaultPolicyScenario" -IsOrganizationDefault $true -Type "TokenLifetimePolicy"```

## <a name="next-steps"></a>後續步驟

* 請參閱 [Azure AD 中可設定的權杖存留期](https://docs.microsoft.com/azure/active-directory/active-directory-configurable-token-lifetimes) \(英文\)，以了解如何設定 Azure AD 所簽發的權杖存留期，包括如何為貴組織中的所有應用程式、多租用戶應用程式或貴組織中的特定服務主體設定權杖存留期。 
* [Azure AD 權杖參考](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims)

