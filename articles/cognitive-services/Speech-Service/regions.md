---
title: 語音服務區域
titlesuffix: Azure Cognitive Services
description: 「語音服務」區域的參考。
services: cognitive-services
author: mahilleb-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: mahilleb
ms.openlocfilehash: 088e581da7511797a0f39959d867c6298262462a
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2018
ms.locfileid: "50242325"
---
# <a name="regions-of-the-speech-service"></a>語音服務的區域

「語音服務」在不同的區域可供使用。
建立訂用帳戶時，可以根據需求選取可用區域。

使用訂用帳戶時，必須考慮選取的區域。

## <a name="rest-api"></a>REST API

使用 REST API 選取正確的區域特定端點。
如需詳細資訊，請參閱 [REST API](rest-apis.md)。

## <a name="speech-sdk"></a>語音 SDK

在[語音服務 SDK](speech-sdk.md) 中，會以字串方式指定區域 (例如，在「適用於 C# 的語音 SDK」中，會作為 `SpeechConfig.FromSubscription` 的參數)。

### <a name="regions-for-speech-recognition-and-translation"></a>語音辨識和轉譯的區域

下表列出**語音辨識**和**翻譯**的可用區域。

  區域 | 語音 SDK 參數 | 語音自訂入口網站
 ------|-------|--------
 美國西部 | `westus` | https://westus.cris.ai
 美國西部 2 | `westus2` | https://westus2.cris.ai 
 美國東部 | `eastus` | https://eastus.cris.ai
 美國東部 2 | `eastus2` | https://eastus2.cris.ai
 東亞 | `eastasia` | https://eastasia.cris.ai
 東南亞 | `southeastasia` | https://southeastasia.cris.ai
 北歐 | `northeurope` | https://northeurope.cris.ai
 西歐 | `westeurope` | https://westeurope.cris.ai


### <a name="regions-for-intent-recognition"></a>意圖辨識的區域

可透過語音 SDK 用於**意圖辨識**的區域列在 [Language Understanding 服務區域頁面](/azure/cognitive-services/luis/luis-reference-regions)上。
列出的每個發佈區域會將對應的語音 SDK 區域參數判定為端點網域名稱的第一個部分。
例如，使用 `westus` 指定美國西部發佈區域。
