---
title: 使用 Azure 虛擬網路資料來源的內部部署資料閘道 | Microsoft Docs
description: 了解如何將伺服器設定為使用 VNet 上的資料來源閘道。
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 10/18/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 62f6413854ecdfc257655fd864167d8e24d35e06
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/19/2018
ms.locfileid: "49427297"
---
# <a name="use-gateway-for-data-sources-on-an-azure-virtual-network-vnet"></a>使用 Azure 虛擬網路 (VNet) 上的資料來源閘道

本文描述當資料來源位於 [Azure 虛擬網路 (VNet)](../virtual-network/virtual-networks-overview.md) 上時可使用的 **AlwaysUseGateway** 伺服器屬性。

## <a name="server-access-to-vnet-data-sources"></a>對 VNet 資料來源的伺服器存取

如果透過 VNET 存取資料來源，則在您自己的環境中，Azure Analysis Services 伺服器必須像在內部部署一樣地連線至這些資料來源。 您可以設定 **AlwaysUseGateway** 伺服器屬性，指定要讓伺服器透過[內部部署閘道](analysis-services-gateway.md)存取所有的資料來源資料。 

> [!NOTE]
> 已安裝和設定[內部部署資料閘道](analysis-services-gateway.md)時，這個屬性才會有效。 閘道可以位於 VNet 上。

## <a name="configure-alwaysusegateway-property"></a>設定 AlwaysUseGateway 屬性

1. 在 [SSMS] > [伺服器] > [屬性] > [一般]，選取 [顯示進階 (所有) 屬性]。
2. 在 **ASPaaS\AlwaysUseGateway** 中，選取 [true]。

    ![一律使用閘道屬性](media/analysis-services-vnet-gateway/aas-ssms-always-property.png)


## <a name="see-also"></a>另請參閱
[連線至內部部署資料來源](analysis-services-gateway.md)   
[安裝及設定內部部署資料閘道](analysis-services-gateway-install.md)   
[Azure 虛擬網路 (VNET)](../virtual-network/virtual-networks-overview.md)   

