---
title: Bing 實體搜尋 SDK
titleSuffix: Azure Cognitive Services
description: 可搜尋 Web 的應用程式所適用的 Bing 實體搜尋 SDK。
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-entity-search
ms.topic: conceptual
ms.date: 1/24/2018
ms.author: v-gedod
ms.openlocfilehash: 8212f4ca5178a5af55a2b91e879f54727711092b
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2018
ms.locfileid: "48814996"
---
# <a name="bing-search-sdk"></a>Bing 搜尋 SDK
Bing 實體搜尋 API 範例包括下列情節：
1.  搜尋 Tom Cruise 這類實體，並取得豐富資訊。
2.  處理可能有多個意圖的查詢項目去除混淆。
3.  搜尋餐廳這類本機實體，並取得其豐富資訊。
4.  搜尋餐廳這類當地公司，並取得其豐富資訊。
5.  觸發錯誤要求和錯誤處理。

> [!NOTE] 
> 部分 SDK 目前處於 GA，並會擱置文件變更。 

Bing 搜尋 SDK 透過下列程式設計語言更輕易地存取 Web 搜尋功能：
* 開始使用 [.NET 範例](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7) 
    * [Nuget 套件](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.EntitySearch/1.2.0)
    * 另請參閱 [.NET 程式庫](https://github.com/Azure/azure-sdk-for-net/tree/psSdkJson6/src/SDKs/CognitiveServices/dataPlane/Search/BingEntitySearch)，以了解定義和相依性。
* 開始使用 [Node.js 範例](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples) 
    * 另請參閱 [Node.js 程式庫](https://github.com/Azure/azure-sdk-for-node/tree/master/lib/services/entitySearch)，以了解定義和相依性。
* 開始使用 [Java 範例](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples) 
    * 另請參閱 [Java 程式庫](https://github.com/Azure/azure-sdk-for-java/tree/master/azure-cognitiveservices/search/bingentitysearch)，以了解定義和相依性。
* 開始使用 [Python 範例](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples) 
    * 另請參閱 [Python 程式庫](https://github.com/Azure/azure-sdk-for-python/tree/master/azure-cognitiveservices-search-entitysearch)，以了解定義和相依性。

每種語言的 SDK 範例都會包括讀我檔案，內含必要條件和安裝/執行範例的詳細資料。
