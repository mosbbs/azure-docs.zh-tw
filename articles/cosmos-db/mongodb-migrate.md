---
title: 使用 mongoimport 和 mongorestore 搭配適用於 MongoDB 的 Azure Cosmos DB API | Microsoft Docs
description: 了解如何使用 mongoimport 和 mongorestore 將資料匯入適用於 MongoDB 的 API 帳戶
keywords: mongoimport, mongorestore
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.component: cosmosdb-mongo
ms.devlang: na
ms.topic: tutorial
ms.date: 05/07/2018
ms.author: sngun
ms.custom: mvc
ms.openlocfilehash: d3a7ddcd4a95660264bdf9609f54af39a05c97b3
ms.sourcegitcommit: ae45eacd213bc008e144b2df1b1d73b1acbbaa4c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/01/2018
ms.locfileid: "50741015"
---
# <a name="tutorial-migrate-your-data-to-azure-cosmos-db-mongodb-api-account"></a>教學課程：將您的資料移轉至 Azure Cosmos DB MongoDB API 帳戶

本教學課程提供將儲存在 MongoDB 中的資料移轉至 Azure Cosmos DB MongoDB API 帳戶的指示。 如果您從 MongoDB 匯入資料，而且想要將這些資料用於 Azure Cosmos DB SQL API，則必須使用[資料移轉工具](import-data.md)匯入資料。

本教學課程涵蓋下列工作：

> [!div class="checklist"]
> * 為移轉做規劃
> * 移轉的必要條件
> * 使用 mongoimport 移轉資料
> * 使用 mongorestore 移轉資料

將資料移轉至 MongoDB API 帳戶之前，請確定您有一些範例 MongoDB 資料。 如果您沒有範例 MongoDB 資料庫，您可以下載並安裝 [MongoDB 社群伺服器](https://www.mongodb.com/download-center)、建立範例資料庫，並使用 mongoimport.exe 或 mongorestore.exe 來上傳範例資料。 

## <a name="plan-for-migration"></a>為移轉做規劃

1. 預先建立並調整您的集合：
        
    * 根據預設，Azure Cosmos DB 會佈建含每秒 1,000 個要求單位 (RU/秒) 的新 MongoDB 集合。 使用 mongoimport 或 mongorestore 開始進行移轉之前，請先從 [Azure 入口網站](https://portal.azure.com)或 MongoDB 驅動程式與工具預先建立您的所有集合。 如果資料大小超過 10 GB，請務必使用適當的分區索引鍵建立[分割集合](partition-data.md)。

    * 在 [Azure 入口網站](https://portal.azure.com)中，僅針對移轉的需求增加集合的輸送量，單一分割區集合從 1,000 RU/秒起，分區集合則從 2,500 RU/秒起。 藉由較高的輸送量，您可以避免速率限制，並花費較少的時間進行移轉。 您可以在移轉之後立即減少輸送量，以節省成本。

    * 除了佈建集合層級的 Ru/秒，您也可以為一組集合佈建父資料庫層級的 RU/秒。 這必須預先建立資料庫和集合，並定義每個集合的分區索引鍵。

    * 您可以透過您慣用的工具、驅動程式或 SDK 來建立分區化集合。 在此範例中，我們使用 Mongo 殼層來建立分區化集合：

        ```bash
        db.runCommand( { shardCollection: "admin.people", key: { region: "hashed" } } )
        ```
    
        結果：

        ```JSON
        {
            "_t" : "ShardCollectionResponse",
            "ok" : 1,
            "collectionsharded" : "admin.people"
        }
        ```

1. 估算單一文件消耗的 RU：

   a. 從 MongoDB 殼層連線至您的 Azure Cosmos DB MongoDB API 帳戶。 您可以在[將 MongoDB 應用程式連接到 Azure Cosmos DB](connect-mongodb-account.md) 中找到指示。
    
   b. 從 MongoDB 殼層使用其中一個範例文件來執行範例插入命令：
   
      ```bash
      db.coll.insert({ "playerId": "a067ff", "hashedid": "bb0091", "countryCode": "hk" })
      ```
        
   c. 執行 ```db.runCommand({getLastRequestStatistics: 1})```，您將會收到如下的回應：
     
      ```bash
        globaldb:PRIMARY> db.runCommand({getLastRequestStatistics: 1})
        {
            "_t": "GetRequestStatisticsResponse",
            "ok": 1,
            "CommandName": "insert",
            "RequestCharge": 10,
            "RequestDurationInMilliSeconds": NumberLong(50)
        }
      ```
        
    d. 記下要求費用。
    
1. 判斷從您的電腦到 Azure Cosmos DB 雲端服務的延遲：
    
    a. 從 MongoDB 殼層使用此命令，以啟用詳細資訊記錄：```setVerboseShell(true)```
    
    b. 對資料庫執行簡單的查詢：```db.coll.find().limit(1)```。 您將會收到如下的回應：

       ```bash
       Fetched 1 record(s) in 100(ms)
       ```
        
1. 移轉之前請先移除插入的文件，以確保不會有重複的文件。 您可以使用此命令來移除文件：```db.coll.remove({})```

1. 估算 *batchSize* 和 *numInsertionWorkers* 值：

    * *batchSize* 的算法是將總佈建的 RU 數，除以步驟 3 中寫入單一文件所花費的 RU 數。
    
    * 如果算出的 *batchSize* <= 24，請使用該數字做為您的 *batchSize* 值。
    
    * 如果算出的 *batchSize* > 24，則將 *batchSize* 的值設為 24。
    
    * *numInsertionWorkers* 的算法是使用此方程式：*numInsertionWorkers =  (佈建輸送量 * 延遲秒數) / (批次大小 * 單一寫入花費的 RU 數)*。
        
    |屬性|值|
    |--------|-----|
    |batchSize| 24 |
    |佈建的 RU | 10000 |
    |Latency | 0.100 秒 |
    |寫入 1 個文件花費的 RU | 10 RU |
    |numInsertionWorkers | ? |
    
    *numInsertionWorkers = (10000 RU x 0.1 s) / (24 x 10 RU) = 4.1666*

1. 執行移轉命令。 用來移轉資料的選項將於後續章節說明。

   ```bash
   mongoimport.exe --host cosmosdb-mongodb-account.documents.azure.com:10255 -u cosmosdb-mongodb-account -p <Your_MongoDB_password> --ssl --sslAllowInvalidCertificates --jsonArray --db dabasename --collection collectionName --file "C:\sample.json" --numInsertionWorkers 4 --batchSize 24
   ```
   或使用 mongorestore (請確定所有集合的輸送量都設定為等於或大於先前計算中使用的 RU 數目)：
   
   ```bash
   mongorestore.exe --host cosmosdb-mongodb-account.documents.azure.com:10255 -u cosmosdb-mongodb-account -p <Your_MongoDB_password> --ssl --sslAllowInvalidCertificates ./dumps/dump-2016-12-07 --numInsertionWorkersPerCollection 4 --batchSize 24
   ```

## <a name="prerequisites-for-migration"></a>移轉的必要條件

* **增加輸送量︰** 資料移轉的時間長短取決於您為個別集合或一組集合設定的輸送量。 針對較大資料移轉，請務必增加輸送量。 完成移轉之後，再降低輸送量以節省成本。 如需在 [Azure 入口網站](https://portal.azure.com)增加輸送量的詳細資訊，請參閱 [Azure Cosmos DB 中的效能等級和定價層](performance-levels.md)。

* **啟用 SSL：** Azure Cosmos DB 具有嚴格的安全性需求和標準。 與您的帳戶互動時，請務必啟用 SSL。 本文其他部分的程序包括如何針對 mongoimport 和 mongorestore 啟用 SSL。

* **建立 Azure Cosmos DB 資源：** 在您開始遷移資料之前，請先從 Azure 入口網站預先建立所有集合。 如果您要遷移至具有資料庫層級輸送量的 Azure Cosmos DB 帳戶，請務必在建立 Azure Cosmos DB 集合時提供分割區索引鍵。

## <a name="get-your-connection-string"></a>取得您的連接字串 

1. 在 [Azure 入口網站](https://portal.azure.com)的左窗格中，按一下 [Azure Cosmos DB] 項目。
1. 在 [訂用帳戶] 窗格中，選取您的帳戶名稱。
1. 在 [連接字串] 刀鋒視窗中，按一下 [連接字串]。

   右窗格中有您成功連接到您的帳戶所需的所有資訊。

   ![[連接字串] 刀鋒視窗](./media/mongodb-migrate/ConnectionStringBlade.png)

## <a name="migrate-data-by-using-mongoimport"></a>使用 mongoimport 移轉資料

若要將資料匯入您的 Azure Cosmos DB 帳戶，請使用下列範本。 填寫專屬於您的帳戶的 [主機]、[使用者名稱]、[密碼] 值。  

範本：

```bash
mongoimport.exe --host <your_hostname>:10255 -u <your_username> -p <your_password> --db <your_database> --collection <your_collection> --ssl --sslAllowInvalidCertificates --type json --file "C:\sample.json"
```

範例：  

```bash
mongoimport.exe --host cosmosdb-mongodb-account.documents.azure.com:10255 -u cosmosdb-mongodb-account -p <Your_MongoDB_password> --ssl --sslAllowInvalidCertificates --db sampleDB --collection sampleColl --type json --file "C:\Users\admin\Desktop\*.json"
```

## <a name="migrate-data-by-using-mongorestore"></a>使用 mongorestore 移轉資料

若要將資料還原至適用於 MongoDB 的 API 帳戶，請使用下列範本執行匯入。 填寫專屬於您的帳戶的 [主機]、[使用者名稱]、[密碼] 值。

範本：

```bash
mongorestore.exe --host <your_hostname>:10255 -u <your_username> -p <your_password> --db <your_database> --collection <your_collection> --ssl --sslAllowInvalidCertificates <path_to_backup>
```

範例：

```bash
mongorestore.exe --host cosmosdb-mongodb-account.documents.azure.com:10255 -u cosmosdb-mongodb-account -p <Your_MongoDB_password> --ssl --sslAllowInvalidCertificates ./dumps/dump-2016-12-07
```

## <a name="next-steps"></a>後續步驟

您可以繼續進行下一個教學課程，了解如何使用 Azure Cosmos DB 查詢 MongoDB 資料。 

> [!div class="nextstepaction"]
>[如何查詢 MongoDB 資料？](../cosmos-db/tutorial-query-mongodb.md)
