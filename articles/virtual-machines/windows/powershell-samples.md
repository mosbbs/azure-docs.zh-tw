---
title: Azure 虛擬機器 PowerShell 範例 | Microsoft Docs
description: Azure 虛擬機器 PowerShell 範例
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 10/19/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 3571ccfbb2f15738fd951c0fa55248745eac281a
ms.sourcegitcommit: 17633e545a3d03018d3a218ae6a3e4338a92450d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/22/2018
ms.locfileid: "49637428"
---
# <a name="azure-virtual-machine-powershell-samples"></a>Azure 虛擬機器 PowerShell 範例

下表提供可建立及管理 Windows 虛擬機器 (VM) 之 PowerShell 指令碼範例的連結。

| | |
|---|---|
|**建立虛擬機器**||
| [快速建立虛擬機器](./../scripts/virtual-machines-windows-powershell-sample-create-vm-quick.md?toc=%2fpowershell%2fmodule%2ftoc.json) | 在最少提示的情況下，建立資源群組、虛擬機器及所有相關資源。|
| [建立完整設定的虛擬機器](./../scripts/virtual-machines-windows-powershell-sample-create-vm.md?toc=%2fpowershell%2fmodule%2ftoc.json) | 建立資源群組、虛擬機器及所有相關資源。|
| [建立高可用性的虛擬機器](./../scripts/virtual-machines-windows-powershell-sample-create-nlb-vm.md?toc=%2fpowershell%2fmodule%2ftoc.json) | 依據高可用性和負載平衡設定建立數個虛擬機器。|
| [建立 VM 並執行設定指令碼](./../scripts/virtual-machines-windows-powershell-sample-create-vm-iis.md?toc=%2fpowershell%2fmodule%2ftoc.json) | 建立虛擬機器，並使用 Azure 自訂指令碼擴充功能來安裝 IIS。 |
| [建立 VM 並執行 DSC 設定](./../scripts/virtual-machines-windows-powershell-sample-create-iis-using-dsc.md?toc=%2fpowershell%2fmodule%2ftoc.json) | 建立虛擬機器，並使用 Azure 預期狀態設定 (DSC) 擴充功能來安裝 IIS。 |
| [上傳 VHD 並建立 VM](./../scripts/virtual-machines-windows-powershell-upload-generalized-script.md) | 將本機 VHD 檔案上傳至 Azure、從 VHD 建立映像，然後從該映像建立 VM。 |
| [從受控 OS 磁碟建立 VM](./../scripts/virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json) | 連結現有受控磁碟作為 OS 磁碟，以建立虛擬機器。 |
| [從快照集建立 VM](./../scripts/virtual-machines-windows-powershell-sample-create-vm-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json) | 藉由先從快照集建立受控磁碟，然後連結新的受控磁碟作為 OS 磁碟，從快照集建立虛擬機器。 |
|**管理儲存體**||
| [在相同或不同的訂用帳戶中從 VHD 建立受控磁碟](../scripts/virtual-machines-windows-powershell-sample-create-managed-disk-from-vhd.md?toc=%2fpowershell%2fmodule%2ftoc.json) | 在相同或不同的訂用帳戶中，從作為 OS 磁碟的特製化 VHD 或從作為資料磁碟的資料 VHD 建立受控磁碟。  |
| [從快照集建立受控磁碟](../scripts/virtual-machines-windows-powershell-sample-create-managed-disk-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json) | 從快照集建立受控磁碟。 |
| [將受控磁碟複製到相同或不同的訂用帳戶](../scripts/virtual-machines-windows-powershell-sample-copy-managed-disks-to-same-or-different-subscription.md?toc=%2fcli%2fmodule%2ftoc.json) | 將受控磁碟複製到與父受控磁碟相同區域中相同或不同的訂用帳戶。 
| [將快照集以 VHD 形式匯出到儲存體帳戶](../scripts/virtual-machines-windows-powershell-sample-copy-snapshot-to-storage-account.md?toc=%2fpowershell%2fmodule%2ftoc.json) | 將受控快照集以 VHD 形式匯出到不同區域中的儲存體帳戶。 |
| [將受控磁碟的 VHD 匯出到儲存體帳戶](../scripts/virtual-machines-windows-powershell-sample-copy-managed-disks-vhd.md?toc=%2fpowershell%2fmodule%2ftoc.json) | 將受控磁碟的底層 VHD 匯出到不同區域中的儲存體帳戶。 |
| [從 VDH 建立快照集](../scripts/virtual-machines-windows-powershell-sample-create-snapshot-from-vhd.md?toc=%2fpowershell%2fmodule%2ftoc.json) | 從 VHD 建立快照集，然後使用該快照集來快速建立多個相同的受控磁碟。  |
| [將快照集複製到相同或不同的訂用帳戶](../scripts/virtual-machines-windows-powershell-sample-copy-snapshot-to-same-or-different-subscription.md?toc=%2fpowershell%2fmodule%2ftoc.json) | 將快照集複製到與父快照集相同區域中相同或不同的訂用帳戶。 |
|**保護虛擬機器**||
| [加密 VM 及其資料磁碟](./../scripts/virtual-machines-windows-powershell-sample-encrypt-vm.md?toc=%2fpowershell%2fazure%2ftoc.json) | 建立 Azure Key Vault、加密金鑰及服務主體，然後加密 VM。 |
|**監視虛擬機器**||
| [透過 Log Analytics 監視 VM](./../scripts/virtual-machines-windows-powershell-sample-create-vm-oms.md?toc=%2fpowershell%2fmodule%2ftoc.json) | 建立虛擬機器、安裝 Azure Log Analytics 代理程式，並在 Log Analytics 工作區中註冊 VM。  |
| | |
