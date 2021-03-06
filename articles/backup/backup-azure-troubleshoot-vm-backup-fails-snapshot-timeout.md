---
title: 針對 Azure 備份失敗：無法使用客體代理程式狀態進行疑難排解
description: 與代理程式、延伸模組及磁碟相關之 Azure 備份失敗的徵狀、原因和解決方案。
services: backup
author: genlin
manager: cshepard
keywords: Azure 備份; VM 代理程式; 網路連線;
ms.service: backup
ms.topic: troubleshooting
ms.date: 10/30/2018
ms.author: genli
ms.openlocfilehash: 9511e4f90348d58c7b5f6e85d9a5eb74af276461
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/07/2018
ms.locfileid: "51260494"
---
# <a name="troubleshoot-azure-backup-failure-issues-with-the-agent-or-extension"></a>針對 Azure 備份失敗進行疑難排解：與代理程式或延伸模組相關的問題

本文提供疑難排解步驟，協助您解決與 VM 代理程式和延伸模組通訊相關的 Azure 備份錯誤。

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="UserErrorGuestAgentStatusUnavailable-vm-agent-unable-to-communicate-with-azure-backup"></a>UserErrorGuestAgentStatusUnavailable - VM 代理程式無法與 Azure 備份通訊

**錯誤碼**：UserErrorGuestAgentStatusUnavailable <br>
**錯誤訊息**：VM 代理程式無法與 Azure 備份通訊<br>

在註冊及排程備份服務的 VM 之後，備份就會藉由與 VM 代理程式通訊以取得時間點快照集，來起始作業。 下列任一種狀況都可能會阻止觸發快照集。 若未觸發快照集，備份可能會失敗。 請依照列出的順序完成下列疑難排解步驟，然後重試作業：<br>
**原因 1：[VM 沒有網際網路存取](#the-vm-has-no-internet-access)**  
**原因 2：[代理程式已安裝到 VM 中，但沒有回應 (適用於 Windows VM)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**    
**原因 3︰[VM 中安裝的代理程式已過時 (適用於 Linux VM)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  
**原因 4︰[無法擷取快照集狀態或無法取得快照集](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**    
**原因 5︰[備份延伸模組無法更新或載入](#the-backup-extension-fails-to-update-or-load)**  

## <a name="guestagentsnapshottaskstatuserror---could-not-communicate-with-the-vm-agent-for-snapshot-status"></a>GuestAgentSnapshotTaskStatusError - 無法與 VM 代理程式通訊來取得快照集狀態

**錯誤碼**：GuestAgentSnapshotTaskStatusError<br>
**錯誤訊息**：無法與 VM 代理程式通訊來取得快照集狀態 <br>

在註冊及排程 Azure 備份服務的 VM 之後，備份就會藉由與 VM 備份擴充功能通訊以取得時間點快照，來起始作業。 下列任一種狀況都可能會阻止觸發快照集。 如果未觸發快照集，可能會發生備份失敗。 請依照列出的順序完成下列疑難排解步驟，然後重試作業：  
**原因 1：[代理程式已安裝到 VM 中，但沒有回應 (適用於 Windows VM)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**  
**原因 2︰[VM 中安裝的代理程式已過時 (適用於 Linux VM)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  
**原因 3：[VM 沒有網際網路存取](#the-vm-has-no-internet-access)**

## <a name="usererrorrpcollectionlimitreached---the-restore-point-collection-max-limit-has-reached"></a>UserErrorRpCollectionLimitReached - 已達到還原點集合上限

**錯誤碼**：UserErrorRpCollectionLimitReached <br>
**錯誤訊息**：已達到還原點集合上限。 <br>
* 如果鎖定復原點資源群組以防止復原點自動清除，就可能會發生此問題。
* 如果每日觸發多個備份，也會發生此問題。 目前，我們建議每日只能觸發一個備份，因為立即 RP 會保留 7 天，而一段指定時間內只能讓 18 個立即 RP 與 VM 相關聯。 <br>

建議的動作：<br>
若要解決此問題，請移除資源群組上的鎖定，並重試此作業來觸發清除動作。

> [!NOTE]
    > 備份服務會建立與 VM 資源群組不同的資源群組，來儲存還原點集合。 建議客戶請勿鎖定建立給備份服務使用的資源群組。 備份服務建立的資源群組命名格式為：AzureBackupRG_`<Geo>`_`<number>` 例如：AzureBackupRG_northeurope_1


**步驟 1：[從還原點資源群組中移除鎖定](#remove_lock_from_the_recovery_point_resource_group)** <br>
**步驟 2：[清除還原點集合](#clean_up_restore_point_collection)**<br>

## <a name="ExtensionSnapshotFailedNoNetwork-snapshot-operation-failed-due-to-no-network-connectivity-on-the-virtual-machine"></a>ExtensionSnapshotFailedNoNetwork - 因為虛擬機器沒有網路連線，所以快照集作業失敗

**錯誤碼**：ExtensionSnapshotFailedNoNetwork<br>
**錯誤訊息**：因為虛擬機器沒有網路連線，所以快照集作業失敗<br>

在註冊及排程 Azure 備份服務的 VM 之後，備份就會藉由與 VM 備份擴充功能通訊以取得時間點快照，來起始作業。 下列任一種狀況都可能會阻止觸發快照集。 如果未觸發快照集，可能會發生備份失敗。 請依照列出的順序完成下列疑難排解步驟，然後重試作業：    
**原因 1：[VM 沒有網際網路存取](#the-vm-has-no-internet-access)**  
**原因 2︰[無法擷取快照集狀態或無法取得快照集](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**  
**原因 3︰[備份延伸模組無法更新或載入](#the-backup-extension-fails-to-update-or-load)**  

## <a name="ExtentionOperationFailed-vmsnapshot-extension-operation-failed"></a>ExtentionOperationFailed - VMSnapshot 延伸模組作業失敗

**錯誤碼**：ExtentionOperationFailed <br>
**錯誤訊息**：VMSnapshot 延伸模組作業失敗<br>

在註冊及排程 Azure 備份服務的 VM 之後，備份就會藉由與 VM 備份擴充功能通訊以取得時間點快照，來起始作業。 下列任一種狀況都可能會阻止觸發快照集。 如果未觸發快照集，可能會發生備份失敗。 請依照列出的順序完成下列疑難排解步驟，然後重試作業：  
**原因 1︰[無法擷取快照集狀態或無法取得快照集](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**  
**原因 2︰[備份延伸模組無法更新或載入](#the-backup-extension-fails-to-update-or-load)**  
**原因 3：[代理程式已安裝到 VM 中，但沒有回應 (適用於 Windows VM)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**  
**原因 4︰[VM 中安裝的代理程式已過時 (適用於 Linux VM)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**

## <a name="backupoperationfailed--backupoperationfailedv2---backup-fails-with-an-internal-error"></a>BackUpOperationFailed / BackUpOperationFailedV2 - 備份失敗，發生內部錯誤

**錯誤碼**：BackUpOperationFailed / BackUpOperationFailedV2 <br>
**錯誤訊息**：備份因為內部錯誤而失敗 - 請在幾分鐘內重試此作業 <br>

在註冊及排程 Azure 備份服務的 VM 之後，備份就會藉由與 VM 備份擴充功能通訊以取得時間點快照，來起始作業。 下列任一種狀況都可能會阻止觸發快照集。 如果未觸發快照集，可能會發生備份失敗。 請依照列出的順序完成下列疑難排解步驟，然後重試作業：  
**原因 1：[VM 沒有網際網路存取](#the-vm-has-no-internet-access)**  
**原因 2：[代理程式已安裝到 VM 中，但沒有回應 (適用於 Windows VM)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**  
**原因 3︰[VM 中安裝的代理程式已過時 (適用於 Linux VM)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  
**原因 4︰[無法擷取快照集狀態或無法取得快照集](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**  
**原因 5︰[備份延伸模組無法更新或載入](#the-backup-extension-fails-to-update-or-load)**  
**原因 6：[備份服務因資源群組鎖定而沒有刪除舊還原點的權限](#backup-service-does-not-have-permission-to-delete-the-old-restore-points-due-to-resource-group-lock)**

## <a name="usererrorunsupporteddisksize---currently-azure-backup-does-not-support-disk-sizes-greater-than-1023gb"></a>UserErrorUnsupportedDiskSize - Azure 備份目前不支援容量大於 1023 GB 的磁碟

**錯誤碼**：UserErrorUnsupportedDiskSize <br>
**錯誤訊息**：Azure 備份目前不支援容量大於 1023 GB 的磁碟 <br>

備份磁碟大小超過 1023GB 的 VM 時，備份作業可能會失敗，因為您的保存庫並未升級到 Azure VM 備份堆疊 V2。 升級至 Azure VM 備份堆疊 V2 將提供最高 4 TB 支援。 請檢閱這些[優點](backup-upgrade-to-vm-backup-stack-v2.md)與[考量](backup-upgrade-to-vm-backup-stack-v2.md#considerations-before-upgrade)，然後依照這些[指示](backup-upgrade-to-vm-backup-stack-v2.md#upgrade)繼續升級。  

## <a name="usererrorstandardssdnotsupported---currently-azure-backup-does-not-support-standard-ssd-disks"></a>UserErrorStandardSSDNotSupported - 目前 Azure 備份不支援標準 SSD 磁碟

**錯誤碼**：UserErrorStandardSSDNotSupported <br>
**錯誤訊息**：目前 Azure 備份不支援標準 SSD 磁碟 <br>

目前 Azure 備份只針對升級至 Azure VM 備份堆疊 V2 的保存庫支援標準 SSD 磁碟。 請檢閱這些[優點](backup-upgrade-to-vm-backup-stack-v2.md)與[考量](backup-upgrade-to-vm-backup-stack-v2.md#considerations-before-upgrade)，然後依照這些[指示](backup-upgrade-to-vm-backup-stack-v2.md#upgrade)繼續升級。


## <a name="causes-and-solutions"></a>原因和解決方案

### <a name="the-vm-has-no-internet-access"></a>VM 沒有網際網路存取
根據部署需求，VM 無法存取網際網路。 或者，它可能會有防止存取 Azure 基礎結構的限制。

備份延伸模組需要連線到 Azure 公用 IP 位址，才能正確運作。 延伸模組會將命令傳送至 Azure 儲存體端點 (HTTPS URL) 來管理 VM 的快照集。 如果延伸模組無法存取公用網際網路，則備份最終會失敗。

您可以部署 Proxy 伺服器來路由傳送 VM 流量。
##### <a name="create-a-path-for-https-traffic"></a>建立 HTTPS 流量的路徑

1. 如果您已有網路限制 (例如，網路安全性群組)，請部署 HTTPS Proxy 伺服器來路由傳送流量。
2. 若要允許從 HTTPS Proxy 伺服器存取網際網路，可將規則新增到網路安全性群組 (如果您有一個)。

若要了解如何設定 VM 備份的 HTTPS Proxy，請參閱[準備環境以備份 Azure 虛擬機器](backup-azure-arm-vms-prepare.md#establish-network-connectivity)。

已備份的 VM，抑或用於路由傳送流量的 Proxy 伺服器需要存取 Azure 公用 IP 位址

####  <a name="solution"></a>解決方法
若要解決此問題，請嘗試下列其中一個方法：

##### <a name="allow-access-to-azure-storage-that-corresponds-to-the-region"></a>允許存取對應該區域的 Azure 儲存體

您可以使用[服務標籤](../virtual-network/security-overview.md#service-tags)，允許連線至特定區域的儲存體。 請確定允許存取儲存體帳戶的規則優先順序，高於封鎖網際網路存取的規則。

![網路安全性群組與區域的儲存體標籤](./media/backup-azure-arm-vms-prepare/storage-tags-with-nsg.png)

若要了解設定服務標記的逐步程序，請觀看[這個影片](https://youtu.be/1EjLQtbKm1M)。

> [!WARNING]
> 儲存體服務標籤處於預覽狀態。 僅在特定區域中提供使用。 如需區域清單，請參閱[儲存體的服務標籤](../virtual-network/security-overview.md#service-tags)。

如果您使用 Azure 受控磁碟，您可能需要在防火牆上開啟其他連接埠 (連接埠 8443)。

此外，如果您的子網路沒有互聯網輸出流量的路由，則您需要將含有服務標籤「Microsoft.Storage」的服務端點新增至您的子網路。

### <a name="the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms"></a>代理程式已安裝在 VM 中，但沒有回應 (適用於 Windows VM)

#### <a name="solution"></a>解決方法
VM 代理程式可能已損毀，或服務可能已停止。 重新安裝 VM 代理程式有助於取得最新版本。 也有助於重新開始與服務通訊。

1. 判斷 Windows 客體代理程式服務是否在 VM 服務 (services.msc) 中執行。 請嘗試重新啟動 Windows 客體代理程式服務並啟動備份。    
2. 如果 Windows 客體代理程式服務未顯示在控制台的服務中，請移至 [程式和功能] 來判斷是否已安裝 Windows 客體代理程式服務。
4. 如果此 Windows 客體代理程式顯示在 [程式和功能] 中，請將 Windows 客體代理程式解除安裝。
5. 下載並安裝[最新版的代理程式 MSI](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409)。 您必須擁有系統管理員權限，才能完成安裝。
6. 確認 Windows 客體代理程式服務顯示在服務中。
7. 執行隨選備份：
    * 在入口網站中，選取 [立即備份]。

此外，確認 VM 中[已安裝 Microsoft .NET 4.5](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed)。 VM 代理程式需有 .NET 4.5 才能與服務通訊。

### <a name="the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms"></a>VM 中安裝的代理程式已過時 (適用於 Linux VM)

#### <a name="solution"></a>解決方法
針對 Linux VM，與代理程式或擴充功能相關的多數失敗是由於會影響過時 VM 代理程式的問題所造成。 若要對此問題進行疑難排解，請遵循下列一般方針：

1. 請遵循[更新 Linux VM 代理程式](../virtual-machines/linux/update-agent.md)的指示。

 > [!NOTE]
 > 我們強烈建議您只透過散發套件存放庫更新代理程式。 我們不建議直接從 GitHub 下載代理程式程式碼，並加以更新。 如果最新的代理程式不適用於您的散發套件，請連絡散發套件支援以取得如何進行安裝的指示。 若要檢查最新的代理程式，請移至 GitHub 儲存機制中的 [Microsoft Azure Linux 代理程式 (英文)](https://github.com/Azure/WALinuxAgent/releases) 頁面。

2. 執行下列命令，確定 Azure 代理程式正在 VM 上執行：`ps -e`

 如果此程序不在執行中，請使用下列命令來重新啟動它：

 * 針對 Ubuntu：`service walinuxagent start`
 * 針對其他散發套件︰`service waagent start`

3. [設定自動重新啟動代理程式](https://github.com/Azure/WALinuxAgent/wiki/Known-Issues#mitigate_agent_crash)。
4. 執行新的測試備份。 如果失敗持續發生，請從 VM 收集下列記錄：

   * /var/lib/waagent/*.xml
   * /var/log/waagent.log
   * /var/log/azure/*

如果我們需要 waagent 的詳細資訊記錄，請遵循下列步驟：

1. 在 /etc/waagent.conf 檔案中，找出下一行︰**Enable verbose logging (y|n)**
2. 將 **Logs.Verbose** 值從 *n* 變更為 *y*。
3. 儲存變更，然後完成本節前面所述的步驟來重新啟動 waagent。

###  <a name="the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken"></a>無法擷取快照集狀態或無法取得快照集
VM 備份仰賴發給底層儲存體帳戶的快照命令。 備份可能會失敗，因為它無權存取儲存體帳戶，或是因為快照集工作延遲執行。

#### <a name="solution"></a>解決方法
下列狀況可能導致快照集工作失敗：

| 原因 | 解決方法 |
| --- | --- |
| 因為遠端桌面通訊協定 (RDP) 中的 VM 關機，而導致報告的 VM 狀態不正確。 | 如果您關閉 RDP 中的 VM，請檢查入口網站，以判斷 VM 狀態是否正確。 如果不正確，可使用 VM 儀表板上的 [關閉] 選項來關閉入口網站中的 VM。 |
| VM 無法從 DHCP 取得主機或網狀架構位址。 | 必須在來賓內啟用 DHCP，IaaS VM 備份才能運作。 如果 VM 無法從 DHCP 回應 245 取得主機或網狀架構位址，則無法下載或執行任何延伸模組。 如果您需要靜態私人 IP 位址，請透過平台進行設定。 VM 內的 DHCP 選項應保持啟用。 如需詳細資訊，請參閱[設定靜態內部私人 IP 位址](../virtual-network/virtual-networks-reserved-private-ip.md)。 |

### <a name="the-backup-extension-fails-to-update-or-load"></a>備份擴充功能無法更新或載入
如果無法載入延伸模組，備份就會因為無法取得快照集而失敗。

#### <a name="solution"></a>解決方法

將擴充功能解除安裝，以強制重新載入 VMSnapshot 擴充功能。 下一次備份嘗試會重新載入解除安裝。

若要將解除安裝解除安裝：

1. 在 [Azure 入口網站](https://portal.azure.com/)中，移至發生備份失敗的 VM。
2. 選取 [Settings] \(設定) 。
3. 選取 [擴充功能]。
4. 選取 [Vmsnapshot 解除安裝]。
5. 選取 [解除安裝]。

針對 Linux VM，如果 VMSnapshot 延伸模組未顯示在 Azure 入口網站中，[更新 Azure Linux 代理程式](../virtual-machines/linux/update-agent.md)，然後再執行備份。

完成這些步驟之後，下一次備份期間會重新安裝延伸模組。

### <a name="remove_lock_from_the_recovery_point_resource_group"></a>從還原點資源群組中移除鎖定
1. 登入 [Azure 入口網站](http://portal.azure.com/)。
2. 移至 [所有資源] 選項，選取下列格式的還原點集合資源群組：AzureBackupRG_`<Geo>`_`<number>`。
3. 在 [設定] 區段中，選取 [鎖定] 來顯示鎖定項目。
4. 若要移除鎖定，請選取省略符號，然後按一下 [刪除]。

    ![刪除鎖定 ](./media/backup-azure-arm-vms-prepare/delete-lock.png)

### <a name="clean_up_restore_point_collection"></a> 清除還原點集合
移除鎖定之後，必須清除還原點。 若要清除還原點，請遵循下列任一方法：<br>
* [執行臨機操作備份來清除還原點集合](#clean-up-restore-point-collection-by-running-ad-hoc-backup)<br>
* [從 Azure 入口網站清除還原點集合](#clean-up-restore-point-collection-from-azure-portal)<br>

#### <a name="clean-up-restore-point-collection-by-running-ad-hoc-backup"></a>執行臨機操作備份來清除還原點集合
移除鎖定之後，觸發臨機操作/手動備份。 這可確保還原點會自動清除。 我們預期第一次執行此臨機操作/手動作業會失敗，但這可確保還原點會自動清除，而不是要手動刪除還原點。 完成清除作業之後，下一個排定的備份應該會成功。

> [!NOTE]
    > 觸發臨機操作/手動備份的幾個小時後，系統會自動執行清除作業。 如果您排定的備份仍然失敗，請使用[此處](#clean-up-restore-point-collection-from-azure-portal)列出的步驟，嘗試手動刪除還原點集合。

#### <a name="clean-up-restore-point-collection-from-azure-portal"></a>從 Azure 入口網站清除還原點集合 <br>

若要手動清除因為鎖定而無法清除的還原點集合，請嘗試下列步驟：
1. 登入 [Azure 入口網站](http://portal.azure.com/)。
2. 在 [中樞] 功能表上按一下 [所有資源]，選取下列格式的資源群組：AzureBackupRG_`<Geo>`_`<number>`，也就是您 VM 所在的位置。

    ![刪除鎖定 ](./media/backup-azure-arm-vms-prepare/resource-group.png)

3. 按一下 [資源群組]，[概觀] 刀鋒視窗會隨即出現。
4. 選取 [顯示隱藏的類型] 選項，以顯示所有隱藏的資源。 選取下列格式的還原點集合：AzureBackupRG_`<VMName>`_`<number>`。

    ![刪除鎖定 ](./media/backup-azure-arm-vms-prepare/restore-point-collection.png)

5. 按一下 [刪除]，即可清除還原點集合。
6. 重試備份作業。
