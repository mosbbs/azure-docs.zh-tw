---
title: 高可用性 - Azure SQL Database 服務 | Microsoft Docs
description: 了解 Azure SQL Database 服務的高可用性功能
services: sql-database
ms.service: sql-database
ms.subservice: ''
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: carlrab, sashan
manager: craigg
ms.date: 10/15/2018
ms.openlocfilehash: 0b2fa1541eafa3acf28690005a6d40fac76deba6
ms.sourcegitcommit: 8e06d67ea248340a83341f920881092fd2a4163c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/16/2018
ms.locfileid: "49353470"
---
# <a name="high-availability-and-azure-sql-database"></a>高可用性和 Azure SQL Database

Azure SQL Database 是高度可用的資料庫平台即服務，可確保您的資料庫啟動並執行 99.99％ 的時間，無須擔心維護和停機時間。 這是 Azure 雲端中託管的完全受控 SQL Server Database Engine 流程，可確保隨時升級/修補 SQL Server Database，而不會影響您的工作負載。 當修補或容錯移轉執行個體時，如果您在應用程式中[採用重試邏輯](sql-database-develop-overview.md#resiliency)，則停機時間通常不明顯。 如果完成容錯移轉的時間超過 60 秒，您應該開啟支援個案。 即使在最關鍵的情況下，Azure SQL Database 也可以快速復原，確保您的資料隨時可用。

Azure 平台可完全管理每個 Azure SQL Database，並保證不會遺失任何資料，也不會影響高度的資料可用性。 Azure 會自動處理修補、備份、複寫、失敗偵測，基礎潛在硬體、軟體或網路失敗、部署錯誤修正、容錯移轉、資料庫升級和其他維護工作。 SQL Server 工程師已執行了最知名的實務，確保所有維護作業在資料庫生命週期不到 0.01% 的時間內完成。 此架構旨在確保已認可的資料不會遺失，且在不影響工作負載的情況下執行維護作業。 在升級或維護資料庫時，不會有維護視窗或停機時間要求您停止工作負載。 Azure SQL Database 中內建的高可用性可確保資料庫絕對不會成為軟體架構中的單一失敗點。

Azure SQL Database 是以會針對雲端環境調整的 SQL Server 資料庫引擎架構為基礎，以確保 99.99% 的可用性 (即使在基礎結構失敗的情況下)。 Azure SQL Database 中使用兩個高可用性架構模型 (兩者均確保 99.99% 的可用性)：

- 以計算和儲存體分隔為基礎的標準/一般用途服務層模型。 此架構模型倚賴儲存層的高可用性和可靠性，但是在維護活動期間可能會有一些潛在的效能衰退。
- 以資料庫引擎程序叢集為基礎的進階/業務關鍵服務層模型。 此架構模型依賴事實上一律有法定的可用資料庫引擎節點數，而且對您工作負載的效能影響最小 (即使在維護活動期間亦然)。

Azure 透明地升級和修補基礎作業系統、驅動程式及 SQL Server Database Engine，為使用者將停機時間降到最低。 Azure SQL Database 會在 SQL Server Database Engine 和 Windows 作業系統最新穩定版本上執行，而且大部分使用者不會注意到連續執行升級。

## <a name="basic-standard-and-general-purpose-service-tier-availability"></a>基本、標準和一般目的服務層可用性

標準可用性是指在基本、標準和一般目的服務層中應用的 99.99% SLA。 分隔計算與儲存體層，以及複寫儲存體層中的資料，可達到此架構模型中的高可用性。

下圖顯示標準架構模型中具有分隔計算和儲存體層的四個節點。

![分隔計算與儲存體](media/sql-database-managed-instance/general-purpose-service-tier.png)

標準可用性模型中有兩層：

- 無狀態計算層，執行 `sqlserver.exe` 流程並且僅包含暫時性和快取資料 (例如計畫快取、緩衝集區、列儲存集區)。 此無狀態 SQL Server 節點是由 Azure Service Fabric 操作，可初始化流程、控制節點的健康情況，並在必要時執行故障轉移至其他位置。
- 具狀態資料層，包含儲存在 Azure 進階儲存體中的資料庫檔案 (.mdf/.ldf)。 Azure 儲存體可確保任何資料庫檔案中放置的任何記錄都不會遺失資料。 Azure 儲存體具有內建的資料可用性/備援，即使 SQL Server 流程損毀，也可以確保保留資料檔案中記錄檔或頁面中的每項記錄。

每當升級資料庫引擎或作業系統，部份的基礎結構失敗，或者在 SQL Server 流程中偵測到某些關鍵問題時，Azure Service Fabric 都會將無狀態 SQL Server 流程移至另一個無狀態計算節點。 發生容錯移轉時，會有一組備用節點等候執行新的計算服務，以便將容錯移轉的時間縮到最短。 Azure 儲存體層中的資料不受影響，而資料/記錄檔會附加到新初始化的 SQL Server 流程。 此流程可保證 99.99％ 的可用性，但由於轉換時間和新 SQL Server 節點以冷快取啟動，可能會對正在執行的繁重工作負載產生一些效能影響。

## <a name="premium-and-business-critical-service-tier-availability"></a>進階與商務關鍵性服務層可用性

進階可用性在 Azure SQL Database 的進階和商務關鍵性服務層中啟用，是專為密集工作負載而設計，這些工作負載無法負擔因正在進行的維護作業所導致的任何效能影響。

在進階模式中，Azure SQL Database 於單一節點上整合了計算和儲存體。 使用類似 SQL Server [Always On 可用性群組](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server)的技術，複寫 4 節點叢集中部署的計算 (SQL Server 資料庫引擎程序) 和儲存體 (本機連結的 SSD)，可達到此架構模型中的高可用性。

![資料庫引擎節點的叢集](media/sql-database-managed-instance/business-critical-service-tier.png)

SQL 資料庫引擎程序和基礎 mdf/ldf 檔案都放在具有本機連接 SSD 儲存體的同一節點上，為您的工作負載提供低延遲。 使用類似 SQL Server [Always On 可用性群組](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server)的技術，實作高可用性。 每個資料庫都屬於資料庫節點叢集，其中一個主要資料庫可供客戶工作負載存取，還有三個包含資料副本的次要流程。 主要節點持續將更改推送到次要節點，以確保主要節點因故損毀時，次要複本仍可提供資料。 容錯轉移由 SQL Server Database Engine 處理 - 一個次要複本成為主要節點，並建立新的次要複本，以確保叢集中有足夠的節點。 工作負載會自動重新導向至新的主要節點。

此外，商務關鍵性叢集具有內建的[讀取縮放](sql-database-read-scale-out.md)功能，會提供免費的內建唯讀節點，該節點可用於執行不會影響主要工作負載效能的唯讀狀態 (例如報告)。

## <a name="zone-redundant-configuration"></a>區域備援設定

預設會在相同的資料中心內建立本機儲存體設定的仲裁集複本。 隨著 [Azure 可用性區域](../availability-zones/az-overview.md)的導入，您便能夠將仲裁集內的不同複本放到相同區域的不同可用性區域中。 為了避免發生單點失敗，系統也會跨多個區域將控制環複寫成三個閘道環 (GW)。 [Azure 流量管理員](../traffic-manager/traffic-manager-overview.md) (ATM) 會控制特定閘道的路由。 由於區域備援設定並不會建立額外的資料庫備援，因此在進階或商務關鍵性服務層中使用「可用性區域」並不需要額外付費。 藉由選取區域備援資料庫，您無須進行任何應用程式邏輯變更，即可讓您的進階或業務關鍵資料庫在面對一組更大規模的失敗情況 (包括災難性的資料中心服務中斷) 時，也能夠復原。 您也可以將任何現有的進階或業務關鍵資料庫或彈性集區轉換成區域備援組態。

由於區域備援仲裁集在不同資料中心 (資料中心彼此之間有些距離) 內都有複本，增加的網路延遲可能導致認可時間增加，因而影響某些 OLTP 工作負載的效能。 您一律可以停用區域備援設定來回到單一區域設定。 此程序是資料大小作業，類似於一般的服務層更新。 在此程序結束時，資料庫或集區會從區域備援環移轉成單一區域環，或反之亦然。

> [!IMPORTANT]
> 目前只有在「進階」服務層中才支援區域備援資料庫和彈性集區。 根據預設，備份和稽核記錄會儲存在 RA-GRS 儲存體中，因此在發生全區域服務中斷時，可能不會自動提供這些記錄。 

下圖說明區域備援版的高可用性架構：

![高可用性架構區域備援](./media/sql-database-high-availability/high-availability-architecture-zone-redundant.png)

## <a name="accelerated-database-recovery-adr"></a>加速資料庫復原 (ADR)

[加速資料庫復原 (ADR)](sql-database-accelerated-database-recovery.md) 是新的 SQL 資料庫引擎功能，藉由重新設計 SQL 資料庫引擎復原程序，來大幅改善資料庫可用性 (特別是在有長時間執行的交易時)。 ADR 目前適用於單一資料庫、彈性集區和 Azure SQL 資料倉儲。

## <a name="conclusion"></a>結論

Azure SQL Database 與 Azure 平台緊密整合，並與 Service Fabric 高度相依以提供失敗偵測和復原、與「Azure 儲存體 Blob」高度相依以提供資料保護，以及與「可用性區域」高度相依以提供更高的容錯能力。 同時，Azure SQL Database 也充分利用 SQL Server 盒裝產品的 Always On 可用性群組技術來提供複寫和容錯移轉。 這些技術的組合可讓應用程式完全了解混合式儲存體模型的優點，並支援最嚴苛的 SLA。

## <a name="next-steps"></a>後續步驟

- 了解 [Azure 可用性區域](../availability-zones/az-overview.md)
- 了解 [Service Fabric](../service-fabric/service-fabric-overview.md)
- 了解 [Azure 流量管理員](../traffic-manager/traffic-manager-overview.md)
- 如需其他高可用性和災害復原的選項，請參閱[商務持續性](sql-database-business-continuity.md)
