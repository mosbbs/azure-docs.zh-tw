---
author: dlepow
ms.service: container-instances
ms.topic: include
ms.date: 11/09/2018
ms.author: danlep
ms.openlocfilehash: 44bdaec78e1fad574f29a5945b07041b588aaff8
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/12/2018
ms.locfileid: "51571893"
---
| 資源 | 預設限制 |
| --- | :--- |
| 每一訂用帳戶的[容器群組](../articles/billing-buy-sign-up-azure-subscription.md) | 100<sup>1</sup> |
| 每個容器群組的容器數目 | 60 |
| 每個容器群組的磁碟區數目 | 20 |
| 每個 IP 的連接埠數目 | 5 |
| 每小時的容器建立 |300<sup>1</sup> |
| 每 5 分鐘的容器建立 | 100<sup>1</sup> |
| 每小時的容器刪除 | 300<sup>1</sup> |
| 每 5 分鐘的容器刪除 | 100<sup>1</sup> |
| 每個容器群組的多個容器 | 僅限 Linux<sup>2</sup> |
| Azure 檔案磁碟區 | 僅限 Linux<sup>2</sup> |
| GitRepo 磁碟區 | 僅限 Linux<sup>2</sup> |
| 祕密磁碟區 | 僅限 Linux<sup>2</sup> |

<sup>1</sup> 建立 [Azure 支援要求][azure-support]可要求提高限制。<br />
<sup>2</sup> 這項功能的 Windows 支援已在規劃中。

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
