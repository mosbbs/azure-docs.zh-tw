---
title: '從 Azure Databricks 連線到不同的資料來源 '
description: 了解如何從 Azure Databricks 連線到不同的資料來源。
services: azure-databricks
author: mamccrea
ms.reviewer: jasonh
ms.service: azure-databricks
ms.workload: big-data
ms.topic: conceptual
ms.date: 03/21/2018
ms.author: mamccrea
ms.openlocfilehash: 8055b5d7e6c53abc385a99d9190a38603ebb968b
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/31/2018
ms.locfileid: "50417441"
---
# <a name="connect-to-data-sources-from-azure-databricks"></a>從 Azure Databricks 連線到資料來源

本文提供在 Azure 中連到 Azure Databricks 之所有不同資料來源的連結。 請遵循這些連結中的範例，以從 Azure 資料來源 (例如 Azure Blob 儲存體、Azure 事件中樞等) 將資料擷取到 Azure Databricks 叢集中，然後在其上執行分析工作。 

## <a name="prerequisites"></a>必要條件

* 您必須有 Azure Databricks 工作區和 Spark 叢集。 請遵循[開始使用 Azure Databricks](quickstart-create-databricks-workspace-portal.md) 中的指示。

## <a name="data-sources-for-azure-databricks"></a>Azure Databricks 的資料來源

下列清單提供 Azure 中可搭配 Azure Databricks 使用的資料來源。 如需可搭配 Azure Databricks 使用的完整資料來源清單，請參閱 [Azure Databricks 的資料來源](https://docs.azuredatabricks.net/spark/latest/data-sources/index.html)。

- [Azure SQL Database](https://docs.azuredatabricks.net/spark/latest/data-sources/sql-databases.html)

    此連結提供使用 JDBC 連線到 SQL 資料庫的 DataFrame API，以及如何控制透過 JDBC 介面的讀取平行處理原則。 本主題提供使用 Scala API 的詳細範例，並於結尾提供簡短的 Python 和 Spark SQL 範例。
- [Azure Data Lake Store](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/azure-datalake.html)

    此連結提供如何使用 Azure Active Directory 服務主體向 Data Lake Store 驗證的範例。 它也提供如何從 Azure Databricks 存取 Data Lake Store 中資料的指示。

- [Azure Blob 儲存體](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/azure-storage.html)

    此連結提供如何使用存取金鑰或給定容器的 SAS，直接從 Azure Databricks 存取 Azure Blob 儲存體的範例。 此連結也提供如何使用 RDD API 從 Azure Databricks 存取 Azure Blob 儲存體的資訊。

- [Azure Cosmos DB](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/cosmosdb-connector.html)

    此連結提供如何從 Azure Databricks 使用 [Azure Cosmos DB Spark 連接器](https://github.com/Azure/azure-cosmosdb-spark)存取 Azure Cosmos DB 中資料的指示。

- [Azure 事件中樞](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/eventhubs-connector.html)

    此連結提供如何從 Azure Databricks 使用 [Azure 事件中樞 Spark 連接器](https://github.com/Azure/azure-event-hubs-spark)存取 Azure 事件中樞中資料的指示。

- [Azure SQL 資料倉儲](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/sql-data-warehouse.html)

    此連結提供如何從 Azure Databricks 使用 Azure SQL 資料倉儲連接器連線的指示。
    

## <a name="next-steps"></a>後續步驟

若要了解可從中將資料匯入 Azure Databricks 的來源，請參閱 [Azure Databricks 的資料來源](https://docs.azuredatabricks.net/spark/latest/data-sources/index.html#)。


