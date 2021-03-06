---
title: 使用 Azure Cosmos DB 全域散發資料 | Microsoft Docs
description: 了解如何從 Azure Cosmos DB (全域散發的多模型資料庫服務)，使用全域資料庫進行全球規模的異地複寫、多重主機、容錯移轉及資料復原。
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 10/26/2018
ms.author: mjbrown
ms.openlocfilehash: 4aa5e4ff46eeaa4e8d8c723f626dd1f1193fd12a
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/08/2018
ms.locfileid: "51281603"
---
# <a name="global-data-distribution-with-azure-cosmos-db"></a>透過 Azure Cosmos DB 進行全域資料散發

現在許多應用程式都會以全球性規模執行。 這些應用程式永遠都處於啟用狀態，並且可供世界各地的使用者存取。 提供高效能和高可用性的同時，還要管理這些應用程式使用的全域散發資料，並不是簡單的事。 Azure Cosmos DB 是一個全域散發的資料庫服務，旨在提供高效能和高可用性。 基於這些因素，Azure Cosmos DB 最適合用於這些即時應用程式。

Cosmos DB 是基本的 Azure 服務，預設為可在所有 [Azure 區域](https://azure.microsoft.com/global-infrastructure/regions/)上使用。 Microsoft 會操控世界上超過 54 個區域中的 Azure 資料中心，並繼續進行擴充，以滿足不斷成長的客戶需求。 建立 Cosmos DB 帳戶時，您會決定要部署此帳戶的區域。 Microsoft 會全天候操控 Cosmos DB 服務，讓您可以專注在您的應用程式上。

您可以將資料庫設定為全域散發，並且在超過 50 個 Azure 區域的任一個區域中使用。 若要降低延遲，您應該將資料放置於更接近使用者的位置。 選擇所需的區域，取決於您應用程式能夠觸達的全域範圍以及使用者所在的位置。 Cosmos DB 會以透明方式將您帳戶內的資料複寫到所有已設定的區域。 它會提供您 Cosmos 資料庫和容器的單一系統映像，讓您的應用程式可以透過該映像在本機讀取和寫入。 透過 Cosmos DB，您可以隨時新增或移除任何與您帳戶相關聯的區域。 您不需暫停或重新部署應用程式，就能新增或移除區域。 由於服務所提供的多路連接功能，讓它始終都能持續保持高度可用狀態。

## <a name="key-benefits-of-global-distribution"></a>全域散發的主要優點

**建置全域主動-主動應用程式**：利用多重主機功能，每個區域都是一個寫入區域 (除了可讀取)。 多重主機功能也會保證：無限制的彈性寫入延展性、在世界各地全都享有 99.999% 的讀取和寫入可用性，以及保證會在第 99 個百分位數提供小於 10 毫秒的讀取/寫入。  

藉由使用 Azure Cosmos DB 的多路連接 API，您的應用程式就能感知最靠近的區域，並且可將要求傳送至該區域。 識別最靠近的區域不會變更任何組態。 當您在 Cosmos DB 帳戶中新增及移除區域時，應用程式不需要重新部署，就可繼續保有高可用性。

**建置回應迅速的應用程式**：您可以輕鬆地設計應用程式，讓其在您為資料庫選擇的所有區域中，執行近乎即時的讀取和寫入作業，而且只有個位數的毫秒延遲。  Azure Cosmos DB 會在內部處理區域之間的資料複寫，以保證達到為 Cosmos 帳戶所選擇的一致性層級。

許多應用程式都受益於多重區域 (本機) 寫入功能帶來的效能提升。 某些需要強式一致性的應用程式偏好將所有寫入傳送到單一區域。 為了支援這些應用程式，Cosmos DB 同時支援單一區域和多重區域設定。

**建置高可用性應用程式**：在多個區域中執行資料庫可提高資料庫的可用性。 如果一個區域無法使用，其他區域就會自動處理應用程式的要求。 Azure Cosmos DB 可為多重區域資料庫提供達 99.999% 的讀取和寫入可用性。

**區域性中斷期間的業務持續性**：發生區域性中斷時，Azure Cosmos DB 可支援[自動容錯移轉](how-to-manage-database-account.md#enable-automatic-failover-for-your-cosmos-account)。 此外，在區域性中斷期間，Cosmos DB 會繼續維持其延遲性、可用性、一致性和輸送量 SLA。 為了協助確保整個應用程式的高可用性，Azure Cosmos DB 會提供手動容錯移轉 API 來模擬區域性中斷。 您可以使用此 API 來執行一般的業務持續性演練。

**全域讀取和寫入延展性**：透過多重主機功能，您可以彈性地調整世界各地的讀取和寫入輸送量。 多重主機功能可確保您應用程式在 Azure Cosmos DB 資料庫或容器上設定的輸送量會傳遞到所有區域，並且受到[費用補償 SLA](https://aka.ms/acdbsla) 的保護。

**多個定義完善的一致性模型**：Azure Cosmos DB 的複寫通訊協定是設計來提供五個定義完善、實用且直覺式的一致性模型。 每個一致性模型都會在一致性與效能之間進行權衡取捨。 這些一致性模型可讓您輕鬆建置全域散發的應用程式。

## <a id="Next Steps"></a>接續步驟

從下列文章中深入了解全域散發：

* [全域散發 - 運作原理](global-dist-under-the-hood.md)
* [如何設定多路連接的用戶端](how-to-manage-database-account.md#configure-clients-for-multi-homing)
* [如何從您的 Cosmos 帳戶新增/移除區域](how-to-manage-database-account.md#addremove-regions-from-your-database-account)
* [如何建立 SQL API 帳戶的自訂衝突解決原則](how-to-manage-conflicts.md#create-a-custom-conflict-resolution-policy)