---
title: Azure Cosmos DB 中的高可用性
description: 這篇文章說明 Azure Cosmos DB 如何提供高可用性
services: cosmos-db
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/15/2018
ms.author: mjbrown
ms.reviewer: sngun
ms.openlocfilehash: 5dc43aa5b98f097bd8bdf927f40b2d3efc878d48
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/08/2018
ms.locfileid: "51283490"
---
# <a name="high-availability-in-azure-cosmos-db"></a>Azure Cosmos DB 中的高可用性

Azure Cosmos DB 會以透明方式，在與 Cosmos 帳戶相關聯的所有 Azure 區域之間複寫您的資料。 Cosmos DB 針對您的資料採用多層備援，如下圖所示：

![資源的資料分割](./media/high-availability/figure1.png) 

- Cosmos 容器內的資料是水平分割的。

- 在每個區域內，每個分割區都會受到一個複本集所保護，而且所有寫入都會由大多數複本所複寫且永久認可。 複本會分散到多達 10-20 個容錯網域中。

- 所有區域中的每個分割區都會複寫。 每個區域都包含 Cosmos 容器的所有資料分割區，並且可以接受寫入與處理讀取。  

如果您的 Cosmos 帳戶分散在 N 個 Azure 區域中，則所有資料至少會有 N x 4 個複本。 除了在與您 Cosmos 帳戶相關聯的區域之間提供低延遲資料存取及調整寫入/讀取輸送量之外，擁有更多區域 (更高的 N) 還可以提高可用性。  

## <a name="slas-for-availability"></a>用於提供可用性的 SLA   

作為全域散發的資料庫，Cosmos DB 會提供全方位的 SLA，以包含輸送量、第 99 個百分位數的延遲、一致性和高可用性。 在下表中，我們會說明 Cosmos DB 為單一和多重區域帳戶所提供的高可用性的相關保證。 如需高可用性，請將您的 Cosmos 帳戶設定為擁有多個寫入區域。

|作業類型  | 單一區域 |多重區域 (單一區域寫入)|多重區域 (多重區域寫入) |
|---------|---------|---------|-------|
|寫入    | 99.99    |99.99   |99.999|
|讀取     | 99.99    |99.999  |99.999|

> [!NOTE]
> 實際上，限定過期、工作階段，一致性前置詞和最終一致性模型的實際寫入可用性，明顯高於已發行的 SLA。 所有一致性層級的實際讀取可用性，明顯高於已發行的 SLA。

## <a name="regional-outages"></a>區域中斷

區域中斷時不是很常見，且 Azure Cosmos DB 可確保您的資料庫永遠可供使用。 下列詳細資料會根據您的 Cosmos 帳戶設定來擷取中斷期間的 Cosmos DB 行為： 

- 使用多重寫入區域設定的多重區域帳戶，將針對寫入和讀取維持高可用性。 區域性容錯移轉是即時的，不需要對應用程式進行任何變更。

- 具有單一寫入區域的多重區域帳戶：在寫入區域中斷期間，這些帳戶將針對讀取維持高可用性。 不過，對於寫入，您必須在 Cosmos 帳戶上「啟用自動容錯移轉」，以將受影響的區域容錯移轉到相關聯的其他區域。 容錯移轉將依照您所指定的區域優先順序進行。 最後，當受影響的區域再度上線時，在中斷期間出現於受影響寫入區域中的未複寫資料，會透過衝突摘要來提供。 應用程式可以讀取衝突摘要，根據應用程式的特定邏輯解決衝突，再視情況將更新後的資料寫回 Cosmos 容器。 一旦先前受影響的寫入區域復原，它將自動作為讀取區域使用。 您可以叫用手動容錯移轉，並將受影響的區域作為寫入區域帶回。 您可以使用 [Azure CLI 或 Azure 入口網站](how-to-manage-database-account.md#enable-manual-failover-for-your-cosmos-account)進行手動容錯移轉。  

- 具有單一寫入區域的多重區域帳戶：在讀取區域中斷期間，這些帳戶將針對讀取和寫入維持高可用性。 受影響的區域將自動與寫入區域中斷連線，並將標示為離線。 Cosmos DB SDK 會將讀取呼叫重新導向至慣用的區域清單中的下一個可用區域。 如果慣用區域清單中的區域都無法使用，呼叫會自動切換回目前的寫入區域。 不需要對應用程式的程式碼做任何變更來處理讀取區域中斷。 最後，當受影響的區域再度上線時，先前受影響的讀取區域將自動與目前寫入區域同步處理，並可再次用於處理讀取要求。 後續的讀取會被重新導向至復原的區域，不需要對應用程式的程式碼做任何變更。 在容錯移轉並重新加入先前失敗的區域期間，Cosmos DB 會繼續遵守讀取一致性保證。

- 如果發生區域中斷，單一區域帳戶可能會失去可用性。 建議您使用 Cosmos 帳戶設定至少兩個區域 (最好是至少兩個寫入區域)，以確保始終保持高可用性。

## <a name="building-highly-available-applications"></a>高度可用的應用程式

- 若要確保寫入和讀取高可用性，請將 Cosmos 帳戶設定為跨越至少兩個具有多重寫入區域的區域。 此設定將可確保 SLA 所支援之讀取和寫入的可用性、最低延遲及延展性。 若要深入了解，請參閱如何[使用多重寫入區域設定您的 Cosmos 帳戶](tutorial-global-distribution-sql-api.md)。

- 針對使用單一寫入區域設定的多重區域 Cosmos 帳戶，[使用 Azure CLI 或 Azure 入口網站來啟用自動容錯移轉](how-to-manage-database-account.md#enable-automatic-failover-for-your-cosmos-account)。 啟用自動容錯移轉之後，只要發生區域性災難，Cosmos DB 就會自動容錯移轉您的帳戶。  

- 即使您的 Cosmos 帳戶具有高可用性，您的應用程式也可能無法正確設計以維持高可用性。 若要確保應用程式的端對端高可用性，請定期叫用[手動容錯移轉 (使用 Azure CLI 或 Azure 入口網站)](how-to-manage-database-account.md#enable-manual-failover-for-your-cosmos-account)，作為應用程式測試或災害復原 (DR) 演練的一部分。 

## <a name="next-steps"></a>後續步驟

接下來，您可以在下列文章中了解調整輸送量：

* [調整輸送量](scaling-throughput.md)

* [各種一致性層級的可用性和效能權衡取捨](consistency-levels-tradeoffs.md)

* [全域調整佈建的輸送量](scaling-throughput.md)

* [全域散發 - 運作原理](global-dist-under-the-hood.md)

* [Azure Cosmos DB 中的一致性層級](consistency-levels.md)


