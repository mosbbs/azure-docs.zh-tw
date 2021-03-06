---
title: 擷取供應項目 API | Microsoft Docs
description: API 會在發行者命名空間下擷取摘述的供應項目清單。
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: reference
ms.date: 09/13/2018
ms.author: pbutlerm
ms.openlocfilehash: 3429917fcee520ae932253f1fdfead4ffb6535e6
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2018
ms.locfileid: "48805474"
---
<a name="retrieve-offers"></a>擷取供應項目
===============

在發行者命名空間下擷取摘述的供應項目清單。

 `GET https://cloudpartner.azure.com/api/publishers/<publisherId>/offers?api-version=2017-10-31`


<a name="uri-parameters"></a>URI 參數
--------------

| **名稱**        |  **說明**                         |  **資料類型** |
| -------------    |  ------------------------------------    |  -----------   |
|  publisherId     | 發行者識別碼，例如 `contoso` |   字串    |
|  api-version     | API 的最新版本                    |    日期        |
|  |  |


<a name="header"></a>頁首
------

|  **名稱**        |         **值**       |
|  --------------- |       ----------------  |
|  Content-Type    | `application/json`      |
|  Authorization   | `Bearer YOUR_TOKEN`     |
|  |  |


<a name="body-example"></a>主體範例
------------

### <a name="response"></a>Response

``` json
  200 OK 
  [ 
      {  
          "offerTypeId": "microsoft-azure-virtualmachines",
          "publisherId": "contoso",
          "status": "published",
          "id": "059afc24-07de-4126-b004-4e42a51816fe",
          "version": 1,
          "definition": {
              "displayText": "Contoso Virtual Machine"
          },
          "changedTime":"2017-05-23T23:33:47.8802283Z"
      }
  ]
```

### <a name="response-body-properties"></a>回應主體屬性

|  **名稱**       |       **說明**                                                                                                  |
|  -------------  |      --------------------------------------------------------------------------------------------------------------    |
|  offerTypeId    | 指出供應項目類型                                                                                           |
|  publisherId    | 可唯一識別發行者的識別碼                                                                      |
|  status         | 供應項目的狀態。 如需可能值清單，請參閱下面的[供應項目狀態](#offer-status)。                         |
|  id             | 可在發行者命名空間中唯一識別供應項目的 GUID。                                                    |
|  version        | 供應項目的目前版本。 用戶端無法修改版本屬性。 每次發行時，它都會累加。 |
|  定義     | 包含工作負載實際定義的摘述檢視。 若要取得詳細定義，請使用[擷取特定供應項目] (./cloud-partner-portal-api-retrieve-specific-offer.md) API。 |
|  changedTime    | 供應項目上次修改時間 (UTC 日期時間)                                                                              |
|  |  |


### <a name="response-status-codes"></a>回應狀態碼

| **代碼**  |  **說明**                                                                                                   |
| -------   |  ----------------------------------------------------------------------------------------------------------------- |
|  200      | `OK` - 已成功處理要求，且發行者下的所有供應項目已傳回給用戶端。  |
|  400      | `Bad/Malformed request` - 錯誤回應主體可能包含更多資訊。                                    |
|  403      | `Forbidden` - 用戶端沒有所指定命名空間的存取權。                                          |
|  404      | `Not found` - 指定的實體不存在。                                                                 |
|  |  |


### <a name="offer-status"></a>供應項目狀態

|  **名稱**                    | **說明**                                  |
|  ------------------------    | -----------------------------------------------  |
|  NeverPublished              | 供應項目從未發行。                  |
|  NotStarted                  | 供應項目是新的，但未啟動。                 |
|  WaitingForPublisherReview   | 供應項目正在等候發行者核准。         |
|  執行中                     | 正在處理供應項目提交。             |
|  Succeeded                   | 已完成處理供應項目提交。       |
|  Canceled                    | 已取消供應項目提交。                   |
|  Failed                      | 供應項目提交失敗。                         |
|  |  |
