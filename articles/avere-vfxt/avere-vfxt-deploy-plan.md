---
title: 規劃您的 Avere vFXT 系統 - Azure
description: 說明在部署Avere vFXT for Azure 之前的規劃事項
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: f0e5523565dc561ed457dbc340835ad1889cb876
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/31/2018
ms.locfileid: "50669865"
---
# <a name="plan-your-avere-vfxt-system"></a>規劃您的 Avere vFXT 系統

本文說明如何規劃新的 Avere vFXT for Azure 叢集，以確保您所建立的叢集位置正確且大小適當，符合您的需求。 

前往 Azure Marketplace 或建立任何 VM 之前，請考慮叢集與 Azure 中其他元素互動的方式。 規劃叢集資源在私人網路和子網路中的位置，並決定您後端儲存體的位置。 請確定您所建立的叢集節點足夠強大，可支援您的工作流程。 

繼續閱讀以深入了解。

## <a name="resource-group-and-network-infrastructure"></a>資源群組和網路基礎結構

請考慮部署 Avere vFXT for Azure 元素的位置。 下圖顯示 Avere vFXT for Azure 元件可能的排列方式：

![本圖顯示一個子網路內的叢集控制器和叢集 VM。 子網路界限周圍是 Vnet 界限。 Vnet 內是一個代表儲存體服務端點的六邊形；它會以虛線箭號連線至 Vnet 外部的 Blob 儲存體。](media/avere-vfxt-components-option.png)

規劃 Avere vFXT 系統的網路基礎結構時，請遵循下列指導方針：

* 所有元素都應該使用針對 Avere vFXT 部署建立的新訂用帳戶管理。 此策略可簡化成本追蹤及清除，也有助於資料分割資源配額。 Avere vFXT 要搭配大量的用戶端使用，因此，在單一訂用帳戶中隔離用戶端與叢集可防止其他重要的工作負載可能在用戶端佈建期間進行資源節流。

* 找出接近 vFXT 叢集的用戶端計算系統。 後端儲存體可能更加遙遠。  

* 為了簡單起見，請在相同的虛擬網路 (Vnet) 和相同的資源群組中，找出 vFXT 叢集與叢集控制器 VM。 它們也應該使用相同的儲存體帳戶。 

* 此叢集必須位於自己的子網路中，以避免 IP 位址與用戶端或計算資源發生衝突。 

## <a name="ip-address-requirements"></a>IP 位址需求 

請確定您叢集子網路的 IP 位址範圍夠大，足以支援叢集。 

Avere vFXT 叢集會使用下列 IP 位址：

* 一個叢集管理 IP 位址。 此位址可能會在叢集中的節點之間移動，但永遠可以使用，讓您可以連線到 Avere 控制台設定工具。
* 對於每個叢集節點：
  * 至少有一個面向用戶端的 IP 位址  (所有面向用戶端的位址都由叢集的 *vserver* 管理，如有需要，這些位址可以在節點之間移動)。
  * 一個用於叢集通訊的 IP 位址
  * 一個執行個體 IP 位址 (指派給 VM)

如果您使用 Azure Blob 儲存體，它也可能需要來自叢集 Vnet 的 IP 位址：  

* Azure Blob 儲存體帳戶必須至少有五個 IP 位址。 如果您在與叢集相同的 Vnet 中找到 Blob 儲存體，請記住這項需求。
* 如果您使用叢集虛擬網路外的 Azure Blob 儲存體，您應該在 Vnet 內建立一個儲存體服務端點。 此端點不使用 IP 位址。

您可以選擇在叢集的不同資源群組中，找出網路資源和 Blob 儲存體 (如果使用的話)。

## <a name="vfxt-node-sizes"></a>vFXT 節點大小 

當作叢集節點的 VM 可決定您快取的要求輸送量和儲存體容量。 您可以從記憶體、處理器和本機儲存體特性都不同的兩個執行個體類型中選擇。 

每個 vFXT 節點都將相同。 也就是說，如果您建立一個三節點的叢集，您將會有三個相同類型和大小的 VM。 

| 執行個體類型 | vCPU | 記憶體  | 本機 SSD 儲存體  | 最大資料磁碟 | 取消快取的磁碟輸送量 | NIC (計數) |
| --- | --- | --- | --- | --- | --- | --- |
| Standard_D16s_v3 | 16  | 64 GiB  | 128 GB  | 32 | 25,600 IOPS <br/> 384 MBps | 8,000 MBps (8) |
| Standard_E32s_v3 | 32  | 256 GiB | 512 GB  | 32 | 51,200 IOPS <br/> 768 MBps | 16,000 MBps (8)  |

每個節點的磁碟快取都可以設定，而且範圍可以從 1000 GB 到 8000 GB。 每個節點 1 TB 是建議的 Standard_D16s_v3 節點快取大小，且每個節點 4 TB 建議用於 Standard_E32s_v3 節點。

如需有關這些 VM 的其他資訊，請閱讀下列 Microsoft Azure 文件：

* [一般用途的虛擬機器大小](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-general)
* [記憶體最佳化的虛擬機器大小](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-memory)

## <a name="account-quota"></a>帳戶配額

請確定您的訂用帳戶具有執行 Avere vFXT 叢集，以及所使用之任何計算或用戶端系統的容量。 如需詳細資訊，請閱讀 [vFXT 叢集配額](avere-vfxt-prereqs.md#quota-for-the-vfxt-cluster)。

## <a name="back-end-data-storage"></a>後端資料儲存體

當它不在快取中時，工作集將儲存在新的 Blob 容器，還是儲存在現有的雲端或硬體儲存體系統？

如果您想要將 Azure Blob 儲存體用於後端，您應該在建立 vFXT 叢集時建立新的容器。 使用 ``create-cloud-backed-container`` 部署指令碼，並為新的 Blob 容器提供儲存體帳戶。 此選項會建立並設定新的容器，使其可以在叢集就緒時使用。 如需詳細資訊，請閱讀[建立節點並設定叢集](avere-vfxt-deploy.md#create-nodes-and-configure-the-cluster)。

> [!NOTE]
> 只有空的 Blob 儲存體容器可以當作 Avere vFXT 系統的核心篩選使用。 vFXT 必須能夠管理其物件存放區，而不需要保留現有的資料。 
>
> 請閱讀[將資料移動到 vFXT 叢集](avere-vfxt-data-ingest.md)以了解如何使用用戶端電腦和 Avere vFXT 快取，將資料有效率地複製到叢集的新容器中。

如果您想要使用現有的內部部署儲存體系統，您必須在建立該儲存體系統之後，將其新增至 vFXT 叢集。 ``create-minimal-cluster`` 部署指令碼會建立不含後端儲存體的 vFXT 叢集。 如需有關如何將現有的儲存體系統新增至 Avere vFXT 叢集的詳細指示，請閱讀[設定儲存體](avere-vfxt-add-storage.md)。 

## <a name="next-step-understand-the-deployment-process"></a>下一步：了解部署程序

[部署概觀](avere-vfxt-deploy-overview.md)提供建立 Avere vFXT for Azure 系統，並準備好提供資料所需所有步驟的概觀。  