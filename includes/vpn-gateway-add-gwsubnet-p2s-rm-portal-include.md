---
title: 包含檔案
description: 包含檔案
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 10/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: bc1e2803a420b95e1abec0886733c5e42a8cfdb4
ms.sourcegitcommit: f6050791e910c22bd3c749c6d0f09b1ba8fccf0c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/25/2018
ms.locfileid: "50026588"
---
1. 在[入口網站](http://portal.azure.com)中，瀏覽至要建立虛擬網路閘道的 Resource Manager 虛擬網路。
2. 在 VNet 頁面的 [設定] 中，按一下 [子網路] 以展開 [子網路] 頁面。
3. 在 [子網路] 頁面中，按一下 [+閘道子網路] 以開啟 [新增子網路] 頁面。 

  ![新增閘道子網路](./media/vpn-gateway-add-gwsubnet-p2s-rm-portal-include/addgwsubnet.png "新增閘道子網路")
4. 子網路的 [名稱] 會自動填入 'GatewaySubnet' 這個值。 為了讓 Azure 將此子網路視為閘道子網路，需要有這個值。 調整自動填入的 [位址範圍] 值，以符合您的組態需求。 請勿設定路由表或服務端點。

  ![新增子網路](./media/vpn-gateway-add-gwsubnet-p2s-rm-portal-include/p2sgwsub.png "新增子網路")
5.  按一下頁面底部的 [確定] 以建立子網路。