---
title: 以雲端服務提供者身分管理 Azure Stack 的使用量和帳單 | Microsoft Docs
description: 以雲端提供者 (CSP) 身分註冊 Azure Stack 及新增要列入計費的客戶。
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/15/2018
ms.author: sethm
ms.reviewer: alfredo
ms.openlocfilehash: 209152b157ef2cfae872490bcff4f2a7100c3a4d
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/16/2018
ms.locfileid: "49339333"
---
# <a name="manage-usage-and-billing-for-azure-stack-as-a-cloud-service-provider"></a>以雲端服務提供者身分管理 Azure Stack 的使用量和帳單 

「適用於：Azure Stack 整合系統」

本文會逐步引導您以雲端提供者 (CSP) 身分註冊 Azure Stack 及新增客戶。

身為 CSP，您會與不同客戶一起使用您的 Azure Stack。 每個客戶在 Azure 中都有 CSP 訂用帳戶。 您必須從 Azure Stack 管理每個使用者訂用帳戶的使用量。

下圖顯示您在選擇共用服務帳戶及使用 Azure Stack 帳戶註冊 Azure 帳戶時，所需進行的步驟。 註冊後，您就可以讓終端使用者上線。

**以 CSP 身分新增使用量追蹤的步驟**

[ ![以雲端服務提供者身分啟用使用方式和管理的流程](media\azure-stack-add-manage-billing-as-a-csp\process-add-useage-as-a-csp.png "以雲端服務提供者身分啟用使用方式和管理的流程") ](media\azure-stack-add-manage-billing-as-a-csp\process-add-useage-as-a-csp.png#lightbox)

## <a name="create-a-csp-or-apss-subscription"></a>建立 CSP 或 APSS 訂用帳戶

### <a name="cloud-service-provider-subscription-types"></a>雲端服務提供者訂用帳戶類型

您必須選擇您用於 Azure Stack 的共用服務帳戶類型。 可用於註冊多租用戶 Azure Stack 的訂用帳戶類型為：

 - 雲端服務提供者 
 - 合作夥伴共用服務訂用帳戶 

#### <a name="azure-partner-shared-services"></a>Azure 合作夥伴共用服務

直接 CSP 或 CSP 散發者在營運 Azure Stack 時，會偏好使用 Azure 合作夥伴共用服務 (APSS) 訂用帳戶來進行註冊。

APSS 訂用帳戶與共用服務租用戶相關聯。 當您註冊 Azure Stack 時，必須提供屬於訂用帳戶擁有者之帳戶的認證。 您用來註冊 Azure Stack 的帳戶可以不同於用於部署的系統管理員帳戶。 此外，這兩個帳戶「不必」屬於相同網域。 換句話說，您可以使用您已使用的租用戶進行部署。 例如，您可以使用 ContosoCSP.onmicrosoft.com，然後使用不同的租用戶進行註冊，例如 IURContosoCSP.onmicrosoft.com。 當您進行日常 Azure Stack 管理時，必須記住您要使用 ContosoCSP.onmicrosoft.com 來登入。 當您必須執行註冊作業時，請使用 IURContosoCSP.onmicrosoft.com 來登入 Azure。

請參考下列資訊以取得 APSS 訂用帳戶的說明，以及如何建立訂用帳戶的指示：[新增 Azure 合作夥伴共用服務](https://msdn.microsoft.com/partner-center/shared-services)。

#### <a name="csp-subscriptions"></a>CSP 訂用帳戶

雲端服務提供者 (CSP) 轉銷商或終端客戶在營運 Azure Stack 時，會偏好使用 CSP 訂用帳戶來進行註冊。

## <a name="register-azure-stack"></a>註冊 Azure Stack

使用依上一節資訊建立的 APSS 訂用帳戶，向 Azure 註冊 Azure Stack。 如需詳細資訊，請參閱[使用您的 Azure 訂用帳戶註冊 Azure Stack](azure-stack-registration.md)。

## <a name="add-end-customer"></a>新增終端客戶

若要設定 Azure Stack，以便新的租用戶在使用資源時，系統會將其使用量報告給雲端服務提供者 (CSP) 訂用帳戶，請參閱[將用於使用量與帳單的租用戶新增至 Azure Stack](azure-stack-csp-howto-register-tenants.md)。

## <a name="charge-the-right-subscriptions"></a>向正確的訂用帳戶收取費用

Azure Stack 會使用稱為「註冊」的功能。 註冊是儲存在 Azure 中的物件。 註冊物件會記載要使用哪個 Azure 訂用帳戶來收取特定 Azure Stack 的費用。 本節說明註冊的重要性。

透過註冊，Azure Stack 可以：
 - 將 Azure Stack 使用量資料轉送給 Azure Commerce，並向 Azure 訂用帳戶收費。
 - 使用多租用戶 Azure Stack 部署報告每個客戶在不同訂用帳戶上的使用量。 多租用戶可讓 Azure Stack 在相同的 Azure Stack 執行個體上支援不同的組織。

對於每個 Azure Stack，會有一個預設的訂用帳戶，以及多個租用戶訂用帳戶。 如果沒有任何租用戶特定的訂用帳戶，則預設訂用帳戶就是計費的 Azure 訂用帳戶。 它必須是第一個註冊的訂用帳戶。 若要讓多租用戶使用量報告生效，訂用帳戶必須為 CSP 或 APSS 訂用帳戶。

然後，會使用 Azure 訂用帳戶針對每個要使用 Azure Stack 的租用戶更新註冊。 租用戶訂用帳戶必須為 CSP 類型，且必須積存至擁有預設訂用帳戶的合作夥伴。 換句話說，您無法註冊其他人的客戶。

當 Azure Stack 將使用量資訊轉送至全域 Azure 時，Azure 中的服務會查閱註冊，並將每個租用戶的使用量對應至適當的租用戶訂用帳戶。 如果租用戶尚未註冊，該使用量會進入其來源 Azure Stack 執行個體的預設訂用帳戶中。

由於租用戶訂用帳戶是 CSP 訂用帳戶，其帳單會傳送至 CSP 合作夥伴，且終端客戶看不到使用量資訊。

## <a name="next-steps"></a>後續步驟

 - 若要深入了解 CSP 方案，請參閱[雲端解決方案提供者方案](https://partner.microsoft.com/solutions/microsoft-cloud-solutions)。
 - 若要深入了解如何取出 Azure Stack 的資源使用量資訊，請參閱 [Azure Stack 中的使用量與帳單](azure-stack-billing-and-chargeback.md)。
