---
title: 辨識印刷和手寫的文字 - 電腦視覺
titleSuffix: Azure Cognitive Services
description: 使用電腦視覺 API 辨識影像中印刷和手寫文字的相關概念。
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: pafarley
ms.openlocfilehash: 6827bf5f983834dc5222a3f3028386f8bbcb253a
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/16/2018
ms.locfileid: "49338095"
---
# <a name="recognizing-printed-and-handwritten-text"></a>辨識印刷和手寫的文字

電腦視覺可從表層和背景不同的各種物件影像中偵測並擷取出印刷或手寫的文字，例如收據、海報、名片、信件和白板等。

文字辨識可節省時間和心力。 您可以採用文字的影像而非謄寫，來提高生產力。 文字辨識讓記事得以數位化。 此一數位化可讓您實作快速且輕鬆的搜尋。 同時也可減少紙張用量。

## <a name="text-recognition-requirements"></a>文字辨識需求

電腦視覺可在符合下列需求的影像中辨識印刷和手寫文字：

- 必須以 JPEG、PNG 或 BMP 格式呈現的影像
- 影像的檔案大小必須小於 4 MB
- 影像的大小必須介於 50 x 50 與 4200 x 4200 像素之間

> [!NOTE]
> 這項技術目前為預覽狀態，且只適用於英文文字。

## <a name="next-steps"></a>後續步驟

了解關於[使用 OCR 擷取文字](concept-extracting-text-ocr.md)的概念。
