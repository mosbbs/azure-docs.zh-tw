---
title: Azure 監視器中的整合警示和監視取代了傳統警示和監視
description: 傳統監視服務和功能 (之前顯示在 Azure 入口網站的 [警示 (傳統)] 底下) 的淘汰概觀。 傳統警示和監視包含適用於 Azure 資源的傳統計量警示、適用於 Application Insights 的傳統計量警示、適用於 Application Insights 的傳統 WebTest 警示、適用於 Application Insights 的傳統自訂計量型警示，以及適用於 Application Insights SmartDetection v1 的傳統警示
author: msvijayn
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 10/04/2018
ms.author: vinagara
ms.component: alerts
ms.openlocfilehash: 589aae8321d2c081f09ed46d9def2229d3973ffd
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/13/2018
ms.locfileid: "51613190"
---
# <a name="unified-alerting--monitoring-in-azure-monitor-replaces-classic-alerting--monitoring"></a>Azure 監視器中的整合警示和監視取代了傳統警示和監視

Azure 監視器現在已成為整合的完整堆疊監視服務，其現在可跨資源支援「一個計量」和「一個警示」；如需詳細資訊，請參閱[關於新 Azure 監視器的部落格文章](https://azure.microsoft.com/blog/new-full-stack-monitoring-capabilities-in-azure-monitor/)。新的 Azure 監視和警示平台經過建置，變得更快速、更聰明，且可擴充，以跟上雲端運算日益擴張的版圖，並和 Microsoft Intelligent Cloud 的原則保持一致。 

隨著新的 Azure 監視和警示平台到來，我們即將淘汰「傳統」監視和警示平台 (裝載於 Azure 警示的 [檢視傳統警示] 區段，且將於 2019 年 6 月前淘汰)。

 ![Azure 入口網站中的傳統警示](media/monitoring-classic-retirement/monitor-alert-screen2.png) 

建議您開始使用警示，並在新的平台中重新建立警示。 對於有大量警示的客戶，我們正設法提供自動化的方式，讓其可以將現有傳統警示移至新的警示系統，而不會中斷作業或增加成本。

## <a name="unified-metrics-and-alerts-in-application-insights"></a>Application Insights 中的整合計量和警示

Azure 監視器的較新計量平台現在可強化來自 Application Insights 的監視功能。 這項變動表示 Application Insights 將會連結至動作群組，從而能夠提供更多的選項，而不只是先前的電子郵件和 Webhook 呼叫。 警示現在可以觸發語音電話、Azure Functions、Logic Apps、簡訊和 ITSM 工具 (例如 ServiceNow 和自動化 Runbook)。 由於有近乎即時的監視和警示功能，新的平台可讓 Application Insights 使用者利用相同技術，來強化其他 Azure 資源的監視功能，並支援 Microsoft 產品的監視功能。

新的 Application Insights 整合監視和警示將會包含：

- **Application Insights 平台計量** - 可提供來自 Application Insights 產品的熱門預建計量。 如需詳細資訊，請參閱這篇關於使用[新 Azure 監視器上的 Application Insights 平台計量](../application-insights/pre-aggregated-metrics-log-metrics.md#pre-aggregated-metrics)的文章。
- **Application Insights 可用性和 Web 測試** - 可讓您能夠評估 Web 應用程式或伺服器的回應能力和可用性。 如需詳細資訊，請參閱這篇關於使用[新 Azure 監視器上的 Application Insights 可用性測試和警示](../application-insights/app-insights-monitor-web-app-availability.md)的文章。
- **Application Insights 自訂計量** - 可讓您定義和發出自己的監視和警示計量。 如需詳細資訊，請參閱這篇關於使用[新 Azure 監視器上的 Application Insights 自訂計量](../application-insights/pre-aggregated-metrics-log-metrics.md#custom-metrics-dimensions-and-pre-aggregation)的文章。
- **Application Insights 失敗異常 (智慧偵測的一部分)** - 可在 Web 應用程式的失敗 HTTP 要求或相依性呼叫比率異常增加時，以幾乎即時的方式自動通知您。 屬於新 Azure 監視器一部分的 Application Insights 失敗異常 (智慧偵測的一部分) 即將推出，因為其推出時間是在未來幾個月，所以我們將會在下一版更新此文件和連結。

## <a name="unified-metrics--alerts-for-other-azure-resources"></a>其他 Azure 資源的整合計量和警示

自 2018 年 3 月起，新一代的 Azure 資源警示和多維度監視功能便已推出。 現在，較新的計量平台和警示由於有近乎即時的功能，所以執行速度更快。 更重要的是，較新的計量平台警示可提供更多的細微性，因為較新的平台包含維度選項，可讓您配量和篩選至特定的值組合、條件或作業。 和新 Azure 監視器中的所有警示一樣，較新的計量警示由於使用 ActionGroups (讓通知工具不再只有電子郵件或 Webhook，還可使用簡訊、語音、Azure 函式、自動化 Runbook 等等)，所以更具擴充性。
適用於 Azure 資源的較新計量可透過下列形式來取得：

- **Azure 監視器標準平台計量** - 可提供來自各種 Azure 服務和產品的熱門預先填入計量。 如需詳細資訊，請參閱這篇關於 [Azure 監視器上所支援的計量](monitoring-near-real-time-metric-alerts.md#metrics-and-dimensions-supported)和 [Azure 監視器上所支援的計量警示](alert-metric-overview.md#supported-resource-types-for-metric-alerts)的文章。
- **Azure 監視器自訂計量** - 可提供來自使用者導向來源 (包括 Azure 診斷代理程式) 的計量。 如需詳細資訊，請參閱這篇關於 [Azure 監視器中的自訂計量](metrics-custom-overview.md)的文章。 使用自訂計量，您還可以發佈 [Windows Azure 診斷代理程式](metrics-store-custom-guestos-resource-manager-vm.md)和 [InfluxData Telegraf 代理程式](metrics-store-custom-linux-telegraf.md)所收集的計量。

## <a name="retirement-of-classic-monitoring-and-alerting-platform"></a>淘汰傳統監視和警示平台

如前所述，目前可從 Azure 入口網站的[警示 (傳統) 區段](monitoring-overview-alerts-classic.md)使用的傳統監視和警示平台，由於已由較新的系統取代，因此將會在未來幾個月內淘汰。
較舊的傳統監視和警示將在 2019 年 6 月 30 日淘汰；相關的 API、Azure 入口網站介面及其中的服務也會隨之關閉。 具體來說，這些功能將會淘汰：

- 目前可透過 Azure 入口網站 [[警示 (傳統)] 區段](monitoring-overview-alerts-classic.md)使用的 Azure 資源舊版 (傳統) 計量和警示；可以 [microsoft.insights/alertrules](https://docs.microsoft.com/rest/api/monitor/alertrules) 資源的形式來存取
- 目前可透過 Azure 入口網站 [[警示 (傳統)] 區段](monitoring-overview-alerts-classic.md)使用的 Application Insights 舊版 (傳統) 平台與自訂計量及警示；且可以 [microsoft.insights/alertrules](https://docs.microsoft.com/rest/api/monitor/alertrules) 資源的形式來存取
- 目前可在 Azure 入口網站以 [Application Insights 中的智慧偵測](../application-insights/app-insights-proactive-diagnostics.md)提供的舊版 (傳統) 失敗異常警示；所設定的警示會顯示於 Azure 入口網站的 [[警示 (傳統)] 區段](monitoring-overview-alerts-classic.md)

所有傳統監視和警示系統 (包括對應的 [API](https://msdn.microsoft.com/library/azure/dn931945.aspx)、[PowerShell](insights-alerts-powershell.md)、[CLI](insights-alerts-command-line-interface.md)、Azure 入口網站頁面和[資源範本](monitoring-enable-alerts-using-template.md)在 2019 年 6 月之前仍可繼續使用。 此日期過後，傳統的監視和警示服務就會淘汰且無法再使用；2019 年 6 月之後繼續存在於 [警示 (傳統)] 的警示規則雖可繼續執行，但無法再修改。

2019 年 6 月之後仍留在傳統監視和警示平台的警示，將會在 2019 年 7 月由 Microsoft 自動遷移至其在新 Azure 監視器平台的對等位置。 此程序無須停機即可順利進行，並可確保客戶不會遺失任何監視涵蓋範圍。

我們很快就會提供工具，讓您可以自行從 Azure 入口網站的 [[警示 (傳統)] 區段](monitoring-overview-alerts-classic.md)遷移至新的 Azure 警示。 [警示 (傳統)] 中所設定並遷移至新 Azure 監視器的規則全都會維持免費，不會收費。 所遷移的傳統警示規則也不會因為透過電子郵件、Webhook 或 LogicApp 推送通知而需要承擔任何費用。 不過，使用較新的通知或動作類型 (例如簡訊、語音電話、ITSM 整合等等) 則會收費，不論其新增到遷移的警示還是新的警示都是如此。 如需詳細資訊，請參閱 [Azure 監視器定價](https://azure.microsoft.com/pricing/details/monitor/)。

此外，根據 [Azure 監視器定價](https://azure.microsoft.com/pricing/details/monitor/)的適用範圍，下列項目也會收費：

- 在新的 Azure 監視器平台上，超出免費單位所建立的任何新的 (非遷移) 警示規則
- 超出 Azure 監視器所含免費單位而擷取和保留的任何資料
- Application Insights 所執行的任何多測試 Web 測試
- 超出 Azure 監視器所含免費單位而儲存的任何自訂計量

這篇文章會持續更新關於新 Azure 監視和警示功能的連結和詳細資料，以及可協助使用者採用新 Azure 監視器平台的工具可用性。


## <a name="next-steps"></a>後續步驟

* 了解[新的整合 Azure 監視器](../azure-monitor/overview.md)。
* 深入了解新的 [Azure 警示](monitoring-overview-alerts.md)。
