---
title: Azure SQL Database 計量和診斷記錄 | Microsoft Docs
description: 了解如何設定 Azure SQL Database ，以儲存資源使用量、連線及查詢執行統計資料。
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: danimir
ms.author: v-daljep
ms.reviewer: carlrab
manager: craigg
ms.date: 09/20/2018
ms.openlocfilehash: 8f66c95202e0ccdef86f9630f7a98c20023a8955
ms.sourcegitcommit: 5de9de61a6ba33236caabb7d61bee69d57799142
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/25/2018
ms.locfileid: "50087741"
---
# <a name="azure-sql-database-metrics-and-diagnostics-logging"></a>Azure SQL Database 計量和診斷記錄 

Azure SQL Database、彈性集區、受控執行個體，以及受控執行個體中的資料庫可以發出計量和診斷記錄，讓您以較輕鬆的方式監視效能。 您可以將資料庫設定為將資源使用量、背景工作角色與工作階段及連線串流到下列其中一項 Azure 資源：

* **Azure SQL 分析**：用來作為整合的 Azure 資料庫智慧型效能監控解決方案，其中具備報告、警示及緩解功能。
* **Azure 事件中樞**：用於整合 SQL Database 遙測與自訂監視解決方案或管線。
* **Azure 儲存體**：用於封存大量遙測資料，價格實惠。

    ![架構](./media/sql-database-metrics-diag-logging/architecture.png)

若要了解各種 Azure 服務所支援的計量和記錄類別，請考慮閱讀：

* [Microsoft Azure 中的計量概觀](../monitoring-and-diagnostics/monitoring-overview-metrics.md)
* [Azure 診斷記錄的概觀](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) 

## <a name="enable-logging-of-diagnostics-telemetry"></a>啟用診斷遙測的記錄功能

使用本文件的第一節來啟用資料庫的診斷遙測，以及使用文件的第二部分來啟用彈性集區或受控執行個體的診斷遙測。 使用本文件的後面幾節，將 Azure SQL 分析設定為檢視串流資料庫診斷遙測的監視工具。

> [!NOTE]
> 如果您使用彈性集區或受控執行個體，則除了啟用資料庫的診斷遙測，也建議您啟用這些資源的診斷遙測。 這是因為彈性集區和受控執行個體的角色都是資料庫容器：將其自己的診斷遙測與個別資料庫診斷遙測分開。 
>

您可以使用下列其中一種方法來啟用及管理計量和診斷遙測記錄功能︰

- Azure 入口網站
- PowerShell
- Azure CLI
- Azure 監視器 REST API 
- Azure Resource Manager 範本

當您啟用計量和診斷記錄功能時，您必須指定要收集所選資料的 Azure 資源目的地。 可用的選項包括︰

- Azure SQL 分析
- Azure 事件中心
- Azure 儲存體

您可以佈建新的 Azure 資源，或選取現有的資源。 選取資源之後，使用 [診斷設定] 選項時，您需要指定要收集的資料。

## <a name="enable-logging-for-azure-sql-database-or-databases-in-managed-instance"></a>針對 Azure SQL Database 或受控執行個體中的資料庫啟用記錄功能

在 SQL Database 和受控執行個體中的資料庫上，並未預設啟用計量和診斷記錄功能。

下列診斷遙測可用來收集 Azure SQL Database 和受控執行個體中的資料庫：

| 監視資料庫的遙測 | 支援 Azure SQL Database | 支援受控執行個體中的資料庫 |
| :------------------- | ------------------- | ------------------- |
| [所有計量](sql-database-metrics-diag-logging.md#all-metrics)：包含 DTU/CPU 百分比、DTU/CPU 限制、實體資料讀取百分比、記錄寫入百分比、成功/失敗/防火牆封鎖的連線、工作階段百分比、背景工作角色百分比、儲存體、儲存體百分比和 XTP 儲存體百分比。 | 是 | 否 |
| [QueryStoreRuntimeStatistics](sql-database-metrics-diag-logging.md#query-store-runtime-statistics)：包含關於查詢執行階段統計資料的資訊，例如 CPU 使用率和查詢持續時間統計資料。 | 是 | 是 |
| [QueryStoreWaitStatistics](sql-database-metrics-diag-logging.md#query-store-wait-statistics)：包含關於查詢所等候內容的查詢等候統計資料的資訊，例如 CPU、LOG 和 LOCKING。 | 是 | 是 |
| [錯誤](sql-database-metrics-diag-logging.md#errors-dataset)：包含關於此資料庫上所發生 SQL 錯誤的資訊。 | 是 | 否 |
| [DatabaseWaitStatistics](sql-database-metrics-diag-logging.md#database-wait-statistics-dataset)：包含和資料庫花費在不同等候類型的等候時間長度有關的資訊。 | 是 | 否 |
| [逾時](sql-database-metrics-diag-logging.md#time-outs-dataset)：包含與資料庫上發生的逾時有關的資訊。 | 是 | 否 |
| [封鎖](sql-database-metrics-diag-logging.md#blockings-dataset)：包含與資料庫上發生的封鎖事件有關的資訊。 | 是 | 否 |
| [SQLInsights](sql-database-metrics-diag-logging.md#intelligent-insights-dataset)：將 Intelligent Insights 納入效能。 [深入了解 Intelligent Insights](sql-database-intelligent-insights.md)。 | 是 | 是 |

### <a name="azure-portal"></a>Azure 入口網站

將 Azure SQL Database 和受控執行個體中資料庫的診斷遙測串流處理到 Azure 儲存體、事件中樞或 Log Analytics 的目的地，會透過 Azure 入口網站中每個資料庫的 [診斷設定] 功能表來設定。

### <a name="configure-streaming-of-diagnostics-telemetry-for-azure-sql-database"></a>設定 Azure SQL Database 診斷遙測的串流處理

   ![SQL Database 圖示](./media/sql-database-metrics-diag-logging/icon-sql-database-text.png)

若要啟用 **Azure SQL Database** 診斷遙測的串流處理，請遵循下列步驟：

1. 移至您的 Azure SQL Database 資源
2. 選取 [診斷設定]
3. 如果沒有先前的設定存在，請選取 [開啟診斷]，或者選取 [編輯設定] 來編輯先前的設定
- 最多可建立三 (3) 個平行連線來串流處理診斷遙測。 若要設定診斷資料到多個資源的多個平行串流處理，請選取 [+新增診斷設定] 來建立額外的設定。

   ![啟用 SQL Database 的診斷功能](./media/sql-database-metrics-diag-logging/diagnostics-settings-database-sql-enable.png)

4. 輸入設定名稱，以供您自己參考
5. 選取要將資料庫的診斷資料串流處理到哪個資源：**封存至儲存體帳戶**、**串流至事件中樞**或**傳送至 Log Analytics**
6. 針對標準的監視體驗，選取資料庫診斷記錄遙測的核取方塊：**SQLInsights**、**AutomaticTuning**、**QueryStoreRuntimeStatistics**、**QueryStoreWaitStatistics**、**錯誤**、**DatabaseWaitStatistics**、**逾時**、**區塊**、**死結**。 此遙測會以事件為基礎，並提供標準的監視體驗。
7. 針對進階的監視體驗，選取 **AllMetrics** 的核取方塊。 如前所述，這是對於資料庫診斷遙測所進行的遙測 (以 1 分鐘為基準)。 
8. 按一下 [儲存] 

   ![設定 SQL Database 的診斷功能](./media/sql-database-metrics-diag-logging/diagnostics-settings-database-sql-selection.png)

> [!NOTE]
> 稽核記錄無法從資料庫 [診斷] 設定啟用。 若要啟用稽核記錄串流，請參閱[設定資料庫的稽核](sql-database-auditing.md#subheading-2)，也請參閱 [Azure Log Analytics 和 Azure 事件中樞中的 SQL 稽核記錄](https://blogs.msdn.microsoft.com/sqlsecurity/2018/09/13/sql-audit-logs-in-azure-log-analytics-and-azure-event-hubs/)。
>

> [!TIP]
> 針對您想要監視的每個 Azure SQL Database 重複執行上述步驟。 
>

### <a name="configure-streaming-of-diagnostics-telemetry-for-databases-in-managed-instance"></a>設定受控執行個體中資料庫診斷遙測的串流處理

   ![受控執行個體中的資料庫圖示](./media/sql-database-metrics-diag-logging/icon-mi-database-text.png)

若要啟用**受控執行個體中的資料庫**診斷遙測的串流處理，請遵循下列步驟：

1. 移至受控執行個體中的資料庫
2. 選取 [診斷設定]
3. 如果沒有先前的設定存在，請選取 [開啟診斷]，或者選取 [編輯設定] 來編輯先前的設定
- 最多可建立三 (3) 個平行連線來串流處理診斷遙測。 若要設定診斷資料到多個資源的多個平行串流處理，請選取 [+新增診斷設定] 來建立額外的設定。

   ![啟用受控執行個體資料庫的診斷功能](./media/sql-database-metrics-diag-logging/diagnostics-settings-database-mi-enable.png)

4. 輸入設定名稱，以供您自己參考
5. 選取要將資料庫的診斷資料串流處理到哪個資源：**封存至儲存體帳戶**、**串流至事件中樞**或**傳送至 Log Analytics**
6. 選取資料庫診斷遙測的核取方塊：**SQLInsights**、**QueryStoreRuntimeStatistics**、**QueryStoreWaitStatistics** 及**錯誤**
7. 按一下 [儲存] 

   ![設定受控執行個體資料庫的診斷功能](./media/sql-database-metrics-diag-logging/diagnostics-settings-database-mi-selection.png)

> [!TIP]
> 針對您想要監視之每個受控執行個體中的資料庫重複執行上述步驟。
>

## <a name="enable-logging-for-elastic-pools-or-managed-instance"></a>啟用彈性集區或受控執行個體的記錄功能

作為資料庫容器的彈性集區和受控執行個體會將其自己的診斷遙測與資料分開。 預設不會啟用此診斷遙測。 

### <a name="configure-streaming-of-diagnostics-telemetry-for-elastic-pools"></a>設定彈性集區診斷遙測的串流處理

   ![彈性集區圖示](./media/sql-database-metrics-diag-logging/icon-elastic-pool-text.png)

下列診斷遙測可用來收集彈性集區資源：

| 資源 | 監視遙測 |
| :------------------- | ------------------- |
| **彈性集區** | [所有計量](sql-database-metrics-diag-logging.md#all-metrics)包含 eDTU/CPU 百分比、eDTU/CPU 限制、實體資料讀取百分比、記錄寫入百分比、工作階段百分比、背景工作百分比、儲存體、儲存體百分比、儲存體限制，以及 XTP 儲存體百分比。 |

若要啟用**彈性集區資源**診斷遙測的串流處理，請遵循下列步驟：

1. 在 Azure 入口網站中移至彈性集區資源
2. 選取 [診斷設定]
3. 如果沒有先前的設定存在，請選取 [開啟診斷]，或者選取 [編輯設定] 來編輯先前的設定

   ![啟用彈性集區的診斷功能](./media/sql-database-metrics-diag-logging/diagnostics-settings-container-elasticpool-enable.png)

4. 輸入設定名稱，以供您自己參考
5. 選取要將彈性集區的診斷資料串流處理到哪個資源：**封存至儲存體帳戶**、**串流至事件中樞**或**傳送至 Log Analytics**
6. 如果選取 Log Analytics，則選取 [設定]，然後選取 [+建立新工作區] 來建立新的工作區，或選取現有的工作區
7. 選取彈性集區診斷遙測 **AllMetrics** 的核取方塊
8. 按一下 [儲存] 

   ![設定彈性集區的診斷功能](./media/sql-database-metrics-diag-logging/diagnostics-settings-container-elasticpool-selection.png)

> [!TIP]
> 針對您想要監視的每個彈性集區重複執行上述步驟。
>

### <a name="configure-streaming-of-diagnostics-telemetry-for-managed-instance"></a>設定受控執行個體診斷遙測的串流處理

   ![受控執行個體圖示](./media/sql-database-metrics-diag-logging/icon-managed-instance-text.png)

下列診斷遙測可用來收集受控執行個體資源：

| 資源 | 監視遙測 |
| :------------------- | ------------------- |
| **受控執行個體** | [ResourceUsageStats](sql-database-metrics-diag-logging.md#resource-usage-stats) 包含 V 核心計數、平均 CPU 百分比、IO 要求、讀取/寫入的位元組、保留的儲存空間、使用的儲存空間。 |

若要啟用**受控執行個體資源**診斷遙測的串流處理，請遵循下列步驟：

1. 在 Azure 入口網站中移至受控執行個體資源
2. 選取 [診斷設定]
3. 如果沒有先前的設定存在，請選取 [開啟診斷]，或者選取 [編輯設定] 來編輯先前的設定

   ![啟用受控執行個體的診斷功能](./media/sql-database-metrics-diag-logging/diagnostics-settings-database-mi-enable.png)

4. 輸入設定名稱，以供您自己參考
5. 選取要將彈性集區的診斷資料串流處理到哪個資源：**封存至儲存體帳戶**、**串流至事件中樞**或**傳送至 Log Analytics**
6. 如果選取 Log Analytics，請建立或使用現有的工作區
7. 選取執行個體診斷遙測 **ResourceUsageStats** 的核取方塊
8. 按一下 [儲存] 

   ![設定受控執行個體的診斷功能](./media/sql-database-metrics-diag-logging/diagnostics-settings-database-mi-selection.png)

> [!TIP]
> 針對您想要監視的每個受控執行個體重複執行上述步驟。
>

### <a name="powershell"></a>PowerShell

若要使用 Powershell 啟用計量和診斷記錄功能，請使用下列 Cmdlet：

- 若要啟用儲存體帳戶中診斷記錄的儲存體，請使用下列命令：

   ```powershell
   Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -StorageAccountId [your storage account id] -Enabled $true
   ```

   儲存體帳戶識別碼是您要傳送記錄之目標儲存體帳戶的資源識別碼。

- 若要將診斷記錄串流至事件中樞，請使用下列命令：

   ```powershell
   Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -ServiceBusRuleId [your service bus rule id] -Enabled $true
   ```

   Azure 服務匯流排規則識別碼是此格式的字串︰

   ```powershell
   {service bus resource ID}/authorizationrules/{key name}
   ``` 

- 若要將診斷記錄傳送到 Log Analytics 工作區，請使用下列命令：

   ```powershell
   Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -WorkspaceId [resource id of the log analytics workspace] -Enabled $true
   ```

- 您可以使用下列命令取得 Log Analytics 工作區的資源識別碼：

   ```powershell
   (Get-AzureRmOperationalInsightsWorkspace).ResourceId
   ```

您可以結合這些參數讓多個輸出選項。

### <a name="to-configure-multiple-azure-resources"></a>若要設定多個 Azure 資源

若要支援多個訂用帳戶，從[使用 PowerShell 啟用 Azure 資源計量記錄](https://blogs.technet.microsoft.com/msoms/2017/01/17/enable-azure-resource-metrics-logging-using-powershell/)使用 PowerShell 指令碼。

當執行指令碼 (Enable-AzureRMDiagnostics.ps1) 以將診斷資料從多個資源傳送至工作區時，提供工作區資源識別碼 &lt;$WSID&gt; 作為參數。 若要取得您想要傳送診斷資料至其中的工作區識別碼 &lt;$WSID&gt;，請在下列指令碼中將 &lt;subID&gt; 取代為訂用帳戶識別碼、將 &lt;RG_NAME&gt; 取代為資源群組名稱，以及將 &lt;WS_NAME&gt; 取代為工作區名稱。

- 若要設定多個 Azure 資源，請使用下列命令：

    ```powershell
    PS C:\> $WSID = "/subscriptions/<subID>/resourcegroups/<RG_NAME>/providers/microsoft.operationalinsights/workspaces/<WS_NAME>"
    PS C:\> .\Enable-AzureRMDiagnostics.ps1 -WSID $WSID
    ```

### <a name="azure-cli"></a>Azure CLI

若要使用 Azure CLI 啟用計量和診斷記錄功能，請使用下列 Cmdlet：

- 若要啟用儲存體帳戶中診斷記錄的儲存體，請使用下列命令：

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --storageId <storageAccountId> --enabled true
   ```

   儲存體帳戶識別碼是您要傳送記錄之目標儲存體帳戶的資源識別碼。

- 若要將診斷記錄串流至事件中樞，請使用下列命令：

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --serviceBusRuleId <serviceBusRuleId> --enabled true
   ```

   服務匯流排規則識別碼是此格式的字串︰

   ```azurecli-interactive
   {service bus resource ID}/authorizationrules/{key name}
   ```

- 若要將診斷記錄傳送到 Log Analytics 工作區，請使用下列命令：

   ```azurecli-interactive
   azure insights diagnostic set --resourceId <resourceId> --workspaceId <resource id of the log analytics workspace> --enabled true
   ```

您可以結合這些參數讓多個輸出選項。

### <a name="rest-api"></a>REST API

了解如何[使用 Azure 監視器 REST API 變更診斷設定](https://docs.microsoft.com/rest/api/monitor/diagnosticsettings)。 

### <a name="resource-manager-template"></a>Resource Manager 範本

了解如何[使用 Resource Manager 範本在建立資源時啟用診斷設定](../monitoring-and-diagnostics/monitoring-enable-diagnostic-logs-using-template.md)。 

## <a name="stream-into-azure-sql-analytics"></a>Azure SQL 分析中的資料流 

Azure SQL 分析是一個雲端監視解決方案，可以透過單一窗格跨多個訂用帳戶大規模監視 Azure SQL Database、彈性集區和受控執行個體的效能。 它會收集重要的 Azure SQL Database 效能計量，並且以視覺效果方式呈現，具有內建智慧可以執行效能疑難排解。

![Azure SQL 分析概觀](../log-analytics/media/log-analytics-azure-sql/azure-sql-sol-overview.png)

使用入口網站的 [診斷設定] 刀鋒視窗中內建的 [傳送至 Log Analytics] 選項，即可將 SQL Database 計量和診斷記錄串流到 Azure SQL 分析中。 您也可以透過 PowerShell Cmdlet、Azure CLI 或 Azure 監視器 REST API 使用診斷設定來啟用 Log Analytics。

### <a name="installation-overview"></a>安裝概觀

透過 Azure SQL 分析可以輕易監視 Azure SQL Database Fleet。 需要三個步驟：

1. 從 Azure Marketplace 建立 Azure SQL 分析解決方案
2. 在解決方案中建立監視工作區
3. 將資料庫設定為將診斷遙測串流到您所建立的工作區中。

如果您使用彈性集區或受控執行個體，則除了設定資料庫診斷遙測以外，也要設定來自這些資源的診斷遙測資料流。

### <a name="create-azure-sql-analytics-resource"></a>建立 Azure SQL 分析資源

1. 在 Azure Marketplace 中搜尋 Azure SQL 分析並加以選取

   ![在入口網站中搜尋 Azure SQL 分析](./media/sql-database-metrics-diag-logging/sql-analytics-in-marketplace.png)
   
2. 在解決方案的 [概觀] 畫面上選取 [建立]

3. 在 Azure SQL 分析表單中填入所需的其他資訊：工作區名稱、訂用帳戶、資源群組、位置及定價層。
 
   ![在入口網站中設定 Azure SQL 分析](./media/sql-database-metrics-diag-logging/sql-analytics-configuration-blade.png)

4. 選取 [確定] 進行確認，並選取 [建立] 以完成作業

### <a name="configure-databases-to-record-metrics-and-diagnostics-logs"></a>將資料庫設定為記錄計量和診斷記錄

若要設定資料庫記錄其計量的位置，最簡單的方法就是透過 Azure 入口網站 (如上所述)。 在入口網站中，移至您的 SQL Database 資源並選取 [診斷設定]。

如果您使用彈性集區或受控執行個體，則也必須在這些資源中設定 [診斷設定]，以及將自己的診斷遙測串流到您已建立的工作區中。

### <a name="use-the-sql-analytics-solution"></a>使用 SQL 分析解決方案

SQL 分析是一個階層式儀表板，可讓您在 SQL Database 資源階層之間移動。 若要深入了解如何使用 SQL 分析解決方案，請參閱[使用 SQL 分析解決方案監視 SQL Database](../log-analytics/log-analytics-azure-sql.md)。

## <a name="stream-into-event-hubs"></a>串流至事件中樞

SQL Database 計量和診斷記錄可以串流到事件中樞，方法是使用入口網站中內建的 [串流至事件中樞] 選項。 您也可以透過 PowerShell Cmdlet、Azure CLI 或 Azure 監視器 REST API 使用診斷設定來啟用服務匯流排規則識別碼。 

### <a name="what-to-do-with-metrics-and-diagnostics-logs-in-event-hubs"></a>如何在事件中樞處理計量和診斷記錄
所選的資料串流到事件中樞之後，您很快就能啟用進階監視案例。 事件中樞是作為事件管線的大門。 資料收集到事件中樞之後，這些資料可以透過任何即時分析提供者或批次/儲存體配接器來轉換和儲存。 事件中樞會讓事件串流的產生從這些事件的取用分離。 如此一來，事件消費者可以在自己的排程存取事件。 如需事件中樞的詳細資訊，請參閱：

- [Azure 事件中樞是什麼？](../event-hubs/event-hubs-what-is-event-hubs.md)
- [開始使用事件中心](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)

這裡有一些您可以使用串流功能的方法：

* **透過將最忙碌路徑串流至 PowerBI 以檢視服務健康情況**。 您可以使用事件中樞、串流分析和 PowerBI，輕鬆快速地將計量和診斷資料轉換為 Azure 服務上的深入解析。 如需如何設定事件中樞、使用串流分析處理資料，以及使用 PowerBI 作為輸出的概觀，請參閱[串流分析和 Power BI](../stream-analytics/stream-analytics-power-bi-dashboard.md)。

* **將記錄串流至第三方記錄和遙測資料流**。 使用事件中樞串流，您可以將計量和診斷記錄放入不同的第三方監視和記錄分析解決方案中。 

* **建置自訂遙測及記錄平台**。 如果您已有自建遙測平台或正好在考慮建置一個，事件中樞所具備的高度可調整的發佈訂閱特質可讓您靈活擷取診斷記錄。 請參閱 [Dan Rosanova 指南，以在全球級別的遙測平台中使用事件中樞](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/)。

## <a name="stream-into-storage"></a>串流到儲存體

SQL Database 計量和診斷記錄可以儲存在儲存體，方法是使用入口網站中內建的 [封存至儲存體帳戶] 選項。 您也可以透過 PowerShell Cmdlet、Azure CLI 或 Azure 監視器 REST API 使用診斷設定來啟用儲存體。

### <a name="schema-of-metrics-and-diagnostics-logs-in-the-storage-account"></a>儲存體帳戶中的計量和診斷記錄結構描述

設定計量和診斷記錄集合之後，當第一批資料列可用時，系統會在您選取的儲存體帳戶中建立儲存體容器。 這些 blob 的結構為：

```powershell
insights-{metrics|logs}-{category name}/resourceId=/SUBSCRIPTIONS/{subscription ID}/ RESOURCEGROUPS/{resource group name}/PROVIDERS/Microsoft.SQL/servers/{resource_server}/ databases/{database_name}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```
    
或者，形式更簡單：

```powershell
insights-{metrics|logs}-{category name}/resourceId=/{resource Id}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```

例如，所有計量的 blob 名稱可能是︰

```powershell
insights-metrics-minute/resourceId=/SUBSCRIPTIONS/s1id1234-5679-0123-4567-890123456789/RESOURCEGROUPS/TESTRESOURCEGROUP/PROVIDERS/MICROSOFT.SQL/ servers/Server1/databases/database1/y=2016/m=08/d=22/h=18/m=00/PT1H.json
```

如果您想要記錄彈性集區中的資料，blob 名稱會有點不同：

```powershell
insights-{metrics|logs}-{category name}/resourceId=/SUBSCRIPTIONS/{subscription ID}/ RESOURCEGROUPS/{resource group name}/PROVIDERS/Microsoft.SQL/servers/{resource_server}/ elasticPools/{elastic_pool_name}/y={four-digit numeric year}/m={two-digit numeric month}/d={two-digit numeric day}/h={two-digit 24-hour clock hour}/m=00/PT1H.json
```

### <a name="download-metrics-and-logs-from-storage"></a>從儲存體下載計量和記錄

了解如何[從儲存體下載計量和診斷記錄](../storage/blobs/storage-quickstart-blobs-dotnet.md#download-the-sample-application)。

## <a name="data-retention-policy-and-pricing"></a>資料保留原則和價格

如果您選取事件中樞或儲存體帳戶，您可以指定保留原則。 此原則會刪除早於選取時間期間的資料。 如果您指定 Log Analytics，則保留原則取決於所選的定價層。 診斷遙測的使用量若超過每個月所配置的免費資料擷取單位，則需付費。 所提供的免費資料擷取單位可讓您每個月免費監視多個資料庫。 請注意，相較於閒置的資料庫，較繁重工作負載的更多作用中資料庫將會擷取更多資料。 如需詳細資訊，請參閱 [Log Analytics 定價](https://azure.microsoft.com/pricing/details/monitor/)。 

如果您使用的是 Azure SQL 分析，您可以藉由選取 Azure SQL 分析導覽功能表上的 OMS 工作區，然後選取 [使用量和估計成本]，輕鬆地監視您解決方案中的資料擷取使用量。

## <a name="metrics-and-logs-available"></a>可用的計量和記錄檔

使用[SQL 分析語言](https://docs.microsoft.com/azure/log-analytics/query-language/get-started-queries)，可將所收集的監視遙測用於自己的**自訂分析**和**應用程式開發**。 所收集資料、計量和記錄的結構列於下方。

## <a name="all-metrics"></a>所有計量

### <a name="all-metrics-for-elastic-pools"></a>彈性集區的所有計量

|**Resource**|**計量**|
|---|---|
|彈性集區|eDTU 百分比、使用的 eDTU、eDTU 限制、CPU 百分比、實體資料讀取百分比、記錄寫入百分比、工作階段百分比、背景工作百分比、儲存體、儲存體百分比、儲存體限制、XTP 儲存體百分比 |

### <a name="all-metrics-for-azure-sql-database"></a>Azure SQL Database 的所有計量

|**Resource**|**計量**|
|---|---|
|連接字串|DTU 百分比、使用的 DTU、DTU 限制、CPU 百分比、實體資料讀取百分比、記錄寫入百分比、成功/失敗/防火牆封鎖的連線、工作階段百分比、背景工作百分比、儲存體、儲存體百分比、XTP 儲存體百分比和死結 |

## <a name="logs"></a>記錄檔

### <a name="logs-for-managed-instance"></a>受控執行個體的記錄

### <a name="resource-usage-stats"></a>資源使用統計資料

|屬性|說明|
|---|---|
|TenantId|您的租用戶識別碼。|
|SourceSystem|一律：Azure|
|TimeGenerated [UTC]|記錄檔記錄時的時間戳記。|
|類型|一律：AzureDiagnostics|
|ResourceProvider|資源提供者名稱。 一律：MICROSOFT.SQL|
|類別|類別名稱。 一律：ResourceUsageStats|
|資源|資源名稱。|
|ResourceType|資源類型名稱。 一律：MANAGEDINSTANCES|
|SubscriptionId|資料庫所屬的訂用帳戶 GUID。|
|ResourceGroup|資料庫所屬的資源群組名稱。|
|LogicalServerName_s|受控執行個體的名稱。|
|ResourceId|資源 URI。|
|SKU_s|受控執行個體產品 SKU|
|virtual_core_count_s|可用的虛擬核心數目|
|avg_cpu_percent_s|CPU 百分比平均|
|reserved_storage_mb_s|受控執行個體上的保留儲存體容量|
|storage_space_used_mb_s|受控執行個體上已使用儲存體|
|io_requests_s|IOPS 計數|
|io_bytes_read_s|讀取的 IOPS 位元組|
|io_bytes_written_s|寫入的 IOPS 位元組|

### <a name="logs-for-azure-sql-database-and-managed-instance-database"></a>Azure SQL Database 和受控執行個體資料庫的記錄

### <a name="query-store-runtime-statistics"></a>查詢存放區執行階段統計資料

|屬性|說明|
|---|---|
|TenantId|您的租用戶識別碼。|
|SourceSystem|一律：Azure|
|TimeGenerated [UTC]|記錄檔記錄時的時間戳記。|
|類型|一律：AzureDiagnostics|
|ResourceProvider|資源提供者名稱。 一律：MICROSOFT.SQL|
|類別|類別名稱。 一律：QueryStoreRuntimeStatistics|
|OperationName|作業名稱。 一律：QueryStoreRuntimeStatisticsEvent|
|資源|資源名稱。|
|ResourceType|資源類型名稱。 一律：SERVERS/DATABASES|
|SubscriptionId|資料庫所屬的訂用帳戶 GUID。|
|ResourceGroup|資料庫所屬的資源群組名稱。|
|LogicalServerName_s|資料庫所屬的伺服器名稱。|
|ElasticPoolName_s|資料庫所屬的彈性集區名稱 (如果有的話)。|
|DatabaseName_s|資料庫名稱。|
|ResourceId|資源 URI。|
|query_hash_s|查詢雜湊。|
|query_plan_hash_s|查詢計劃雜湊。|
|statement_sql_handle_s|陳述式 SQL 控制代碼。|
|interval_start_time_d|間隔的開始 datetimeoffset 刻度數目從 1900-1-1 起。|
|interval_end_time_d|間隔的結束 datetimeoffset 刻度數目從 1900-1-1 起。|
|logical_io_writes_d|邏輯 IO 寫入總數。|
|max_logical_io_writes_d|每次執行的邏輯 IO 寫入次數上限。|
|physical_io_reads_d|實體 IO 讀取總數。|
|max_physical_io_reads_d|每次執行的邏輯 IO 讀取次數上限。|
|logical_io_reads_d|邏輯 IO 讀取總數。|
|max_logical_io_reads_d|每次執行的邏輯 IO 讀取次數上限。|
|execution_type_d|執行類型。|
|count_executions_d|查詢的執行次數。|
|cpu_time_d|查詢所耗用的總 CPU 時間 (毫秒)。|
|max_cpu_time_d|單次執行可耗用的 CPU 時間上限 (毫秒)。|
|dop_d|平行處理原則的程度總和。|
|max_dop_d|單次執行所用之平行處理原則的最大程度。|
|rowcount_d|傳回的資料列總數。|
|max_rowcount_d|單次執行傳回的資料列數目上限。|
|query_max_used_memory_d|使用的記憶體總量 (KB)。|
|max_query_max_used_memory_d|單次執行所使用的記憶體數量上限 (KB)。|
|duration_d|總執行時間 (毫秒)。|
|max_duration_d|單次執行的執行時間上限。|
|num_physical_io_reads_d|實體讀取總數。|
|max_num_physical_io_reads_d|每次執行的實體讀取次數上限。|
|log_bytes_used_d|使用的記錄檔位元組總數。|
|max_log_bytes_used_d|每次執行所使用的記錄檔位元組數量上限。|
|query_id_d|查詢存放區中查詢的識別碼。|
|plan_id_d|查詢存放區中計劃的識別碼。|

深入了解[查詢存放區執行階段統計資料](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-query-store-runtime-stats-transact-sql)。

### <a name="query-store-wait-statistics"></a>查詢存放區等候統計資料

|屬性|說明|
|---|---|
|TenantId|您的租用戶識別碼。|
|SourceSystem|一律：Azure|
|TimeGenerated [UTC]|記錄檔記錄時的時間戳記。|
|類型|一律：AzureDiagnostics|
|ResourceProvider|資源提供者名稱。 一律：MICROSOFT.SQL|
|類別|類別名稱。 一律：QueryStoreWaitStatistics|
|OperationName|作業名稱。 一律：QueryStoreWaitStatisticsEvent|
|資源|資源名稱|
|ResourceType|資源類型名稱。 一律：SERVERS/DATABASES|
|SubscriptionId|資料庫所屬的訂用帳戶 GUID。|
|ResourceGroup|資料庫所屬的資源群組名稱。|
|LogicalServerName_s|資料庫所屬的伺服器名稱。|
|ElasticPoolName_s|資料庫所屬的彈性集區名稱 (如果有的話)。|
|DatabaseName_s|資料庫名稱。|
|ResourceId|資源 URI。|
|wait_category_s|等候的類別。|
|is_parameterizable_s|查詢是否可參數化。|
|statement_type_s|陳述式類型。|
|statement_key_hash_s|陳述式索引鍵雜湊。|
|exec_type_d|執行類型。|
|total_query_wait_time_ms_d|特定等候類別的查詢等候總時間。|
|max_query_wait_time_ms_d|特定等候類別個別執行時的查詢等候時間上限。|
|query_param_type_d|0|
|query_hash_s|查詢存放區中的查詢雜湊。|
|query_plan_hash_s|查詢存放區中的查詢計劃。|
|statement_sql_handle_s|查詢存放區中的陳述式控制代碼。|
|interval_start_time_d|間隔的開始 datetimeoffset 刻度數目從 1900-1-1 起。|
|interval_end_time_d|間隔的結束 datetimeoffset 刻度數目從 1900-1-1 起。|
|count_executions_d|查詢的執行計數。|
|query_id_d|查詢存放區中查詢的識別碼。|
|plan_id_d|查詢存放區中計劃的識別碼。|

深入了解[查詢存放區等候統計資料](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-query-store-wait-stats-transact-sql)。

### <a name="errors-dataset"></a>錯誤資料集

|屬性|說明|
|---|---|
|TenantId|您的租用戶識別碼。|
|SourceSystem|一律：Azure|
|TimeGenerated [UTC]|記錄檔記錄時的時間戳記。|
|類型|一律：AzureDiagnostics|
|ResourceProvider|資源提供者名稱。 一律：MICROSOFT.SQL|
|類別|類別名稱。 一律：Errors|
|OperationName|作業名稱。 一律：ErrorEvent|
|資源|資源名稱|
|ResourceType|資源類型名稱。 一律：SERVERS/DATABASES|
|SubscriptionId|資料庫所屬的訂用帳戶 GUID。|
|ResourceGroup|資料庫所屬的資源群組名稱。|
|LogicalServerName_s|資料庫所屬的伺服器名稱。|
|ElasticPoolName_s|資料庫所屬的彈性集區名稱 (如果有的話)。|
|DatabaseName_s|資料庫名稱。|
|ResourceId|資源 URI。|
|訊息|純文字的錯誤訊息。|
|user_defined_b|錯誤是否為使用者定義的位元。|
|error_number_d|錯誤碼。|
|嚴重性|錯誤的嚴重性。|
|state_d|錯誤的狀態。|
|query_hash_s|失敗查詢的查詢雜湊 (如果有的話)。|
|query_plan_hash_s|失敗查詢的查詢計劃雜湊 (如果有的話)。|

深入了解 [SQL Server 錯誤訊息](https://msdn.microsoft.com/library/cc645603.aspx)。

### <a name="database-wait-statistics-dataset"></a>資料庫等候統計資料資料集

|屬性|說明|
|---|---|
|TenantId|您的租用戶識別碼。|
|SourceSystem|一律：Azure|
|TimeGenerated [UTC]|記錄檔記錄時的時間戳記。|
|類型|一律：AzureDiagnostics|
|ResourceProvider|資源提供者名稱。 一律：MICROSOFT.SQL|
|類別|類別名稱。 一律：DatabaseWaitStatistics|
|OperationName|作業名稱。 一律：DatabaseWaitStatisticsEvent|
|資源|資源名稱|
|ResourceType|資源類型名稱。 一律：SERVERS/DATABASES|
|SubscriptionId|資料庫所屬的訂用帳戶 GUID。|
|ResourceGroup|資料庫所屬的資源群組名稱。|
|LogicalServerName_s|資料庫所屬的伺服器名稱。|
|ElasticPoolName_s|資料庫所屬的彈性集區名稱 (如果有的話)。|
|DatabaseName_s|資料庫名稱。|
|ResourceId|資源 URI。|
|wait_type_s|等候類型的名稱。|
|start_utc_date_t [UTC]|測量的時段開始時間。|
|end_utc_date_t [UTC]|測量的時段結束時間。|
|delta_max_wait_time_ms_d|每次執行的等候時間上限|
|delta_signal_wait_time_ms_d|訊號總等候時間。|
|delta_wait_time_ms_d|期間內的總等候時間。|
|delta_waiting_tasks_count_d|等候工作數目。|

深入了解[資料庫等候統計資料](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql)。

### <a name="time-outs-dataset"></a>逾時資料集

|屬性|說明|
|---|---|
|TenantId|您的租用戶識別碼。|
|SourceSystem|一律：Azure|
|TimeGenerated [UTC]|記錄檔記錄時的時間戳記。|
|類型|一律：AzureDiagnostics|
|ResourceProvider|資源提供者名稱。 一律：MICROSOFT.SQL|
|類別|類別名稱。 一律：Timeouts|
|OperationName|作業名稱。 一律：TimeoutEvent|
|資源|資源名稱|
|ResourceType|資源類型名稱。 一律：SERVERS/DATABASES|
|SubscriptionId|資料庫所屬的訂用帳戶 GUID。|
|ResourceGroup|資料庫所屬的資源群組名稱。|
|LogicalServerName_s|資料庫所屬的伺服器名稱。|
|ElasticPoolName_s|資料庫所屬的彈性集區名稱 (如果有的話)。|
|DatabaseName_s|資料庫名稱。|
|ResourceId|資源 URI。|
|error_state_d|錯誤狀態碼。|
|query_hash_s|查詢雜湊 (如果有的話)。|
|query_plan_hash_s|查詢計劃雜湊 (如果有的話)。|

### <a name="blockings-dataset"></a>封鎖資料集

|屬性|說明|
|---|---|
|TenantId|您的租用戶識別碼。|
|SourceSystem|一律：Azure|
|TimeGenerated [UTC]|記錄檔記錄時的時間戳記。|
|類型|一律：AzureDiagnostics|
|ResourceProvider|資源提供者名稱。 一律：MICROSOFT.SQL|
|類別|類別名稱。 一律：Blocks|
|OperationName|作業名稱。 一律：BlockEvent|
|資源|資源名稱|
|ResourceType|資源類型名稱。 一律：SERVERS/DATABASES|
|SubscriptionId|資料庫所屬的訂用帳戶 GUID。|
|ResourceGroup|資料庫所屬的資源群組名稱。|
|LogicalServerName_s|資料庫所屬的伺服器名稱。|
|ElasticPoolName_s|資料庫所屬的彈性集區名稱 (如果有的話)。|
|DatabaseName_s|資料庫名稱。|
|ResourceId|資源 URI。|
|lock_mode_s|查詢所使用的鎖定模式。|
|resource_owner_type_s|鎖定擁有者。|
|blocked_process_filtered_s|已封鎖的處理序報告 XML。|
|duration_d|鎖定的持續時間 (微秒)。|

### <a name="deadlocks-dataset"></a>死結 (Deadlock) 資料集

|屬性|說明|
|---|---|
|TenantId|您的租用戶識別碼。|
|SourceSystem|一律：Azure|
|TimeGenerated [UTC] |記錄檔記錄時的時間戳記。|
|類型|一律：AzureDiagnostics|
|ResourceProvider|資源提供者名稱。 一律：MICROSOFT.SQL|
|類別|類別名稱。 一律：Deadlocks|
|OperationName|作業名稱。 一律：DeadlockEvent|
|資源|資源名稱。|
|ResourceType|資源類型名稱。 一律：SERVERS/DATABASES|
|SubscriptionId|資料庫所屬的訂用帳戶 GUID。|
|ResourceGroup|資料庫所屬的資源群組名稱。|
|LogicalServerName_s|資料庫所屬的伺服器名稱。|
|ElasticPoolName_s|資料庫所屬的彈性集區名稱 (如果有的話)。|
|DatabaseName_s|資料庫名稱。 |
|ResourceId|資源 URI。|
|deadlock_xml_s|死結報表 XML。|

### <a name="automatic-tuning-dataset"></a>自動調整資料集

|屬性|說明|
|---|---|
|TenantId|您的租用戶識別碼。|
|SourceSystem|一律：Azure|
|TimeGenerated [UTC]|記錄檔記錄時的時間戳記。|
|類型|一律：AzureDiagnostics|
|ResourceProvider|資源提供者名稱。 一律：MICROSOFT.SQL|
|類別|類別名稱。 一律：AutomaticTuning|
|資源|資源名稱。|
|ResourceType|資源類型名稱。 一律：SERVERS/DATABASES|
|SubscriptionId|資料庫所屬的訂用帳戶 GUID。|
|ResourceGroup|資料庫所屬的資源群組名稱。|
|LogicalServerName_s|資料庫所屬的伺服器名稱。|
|LogicalDatabaseName_s|資料庫名稱。|
|ElasticPoolName_s|資料庫所屬的彈性集區名稱 (如果有的話)。|
|DatabaseName_s|資料庫名稱。|
|ResourceId|資源 URI。|
|RecommendationHash_s|自動調整建議的唯一雜湊。|
|OptionName_s|自動調整作業。|
|Schema_s|資料庫結構描述。|
|Table_s|受影響的資料表。|
|IndexName_s|索引名稱。|
|IndexColumns_s|資料行名稱。|
|IncludedColumns_s|包含的資料行。|
|EstimatedImpact_s|自動調整建議 JSON 的預估影響。|
|Event_s|自動調整事件的類型。|
|Timestamp_t|上一次更新的時間戳記。|

### <a name="intelligent-insights-dataset"></a>Intelligent Insights 資料集
深入了解 [Intelligent Insights 記錄格式](sql-database-intelligent-insights-use-diagnostics-log.md)。

## <a name="next-steps"></a>後續步驟

若要了解如何啟用記錄，並了解各種 Azure 服務支援的計量和記錄類別，請閱讀：

 * [Microsoft Azure 中的計量概觀](../monitoring-and-diagnostics/monitoring-overview-metrics.md)
 * [Azure 診斷記錄的概觀](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)

若要了解事件中樞，請閱讀：

* [Azure 事件中樞是什麼？](../event-hubs/event-hubs-what-is-event-hubs.md)
* [開始使用事件中心](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)

若要深入了解儲存體，請參閱如何[從儲存體下載計量和診斷記錄](../storage/blobs/storage-quickstart-blobs-dotnet.md#download-the-sample-application)。
