---
title: 偵測臉部 - 電腦視覺
titleSuffix: Azure Cognitive Services
description: 使用電腦視覺 API 偵測臉部的相關概念。
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: pafarley
ms.openlocfilehash: bf358d1e8f60f989ced8db966bbf0a5179fab25b
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/16/2018
ms.locfileid: "49342308"
---
# <a name="detecting-faces"></a>偵測臉部

電腦視覺會偵測圖片中的人臉，並針對偵測到的臉部產生年齡、性別和矩形。 電腦視覺提供位於[臉部](/azure/cognitive-services/face/)的功能子集，且您可以使用「臉部」服務執行更詳細的分析，例如臉部識別和姿勢偵測。  

## <a name="face-detection-examples"></a>臉部偵測範例

第一個範例使用包含單一人臉的影像，示範電腦視覺傳回的 JSON 回應。

![視覺分析屋頂女人的臉部](./Images/woman_roof_face.png)

```json
{
    "faces": [
        {
            "age": 23,
            "gender": "Female",
            "faceRectangle": {
                "top": 45,
                "left": 194,
                "width": 44,
                "height": 44
            }
        }
    ],
    "requestId": "8439ba87-de65-441b-a0f1-c85913157ecd",
    "metadata": {
        "height": 200,
        "width": 300,
        "format": "Png"
    }
}
```

第二個範例使用包含多張人臉的影像來示範傳回的 JSON 回應。

![視覺分析家人合照的臉部](./Images/family_photo_face.png)

```json
{
    "faces": [
        {
            "age": 11,
            "gender": "Male",
            "faceRectangle": {
                "top": 62,
                "left": 22,
                "width": 45,
                "height": 45
            }
        },
        {
            "age": 11,
            "gender": "Female",
            "faceRectangle": {
                "top": 127,
                "left": 240,
                "width": 42,
                "height": 42
            }
        },
        {
            "age": 37,
            "gender": "Female",
            "faceRectangle": {
                "top": 55,
                "left": 200,
                "width": 41,
                "height": 41
            }
        },
        {
            "age": 41,
            "gender": "Male",
            "faceRectangle": {
                "top": 45,
                "left": 103,
                "width": 39,
                "height": 39
            }
        }
    ],
    "requestId": "3a383cbe-1a05-4104-9ce7-1b5cf352b239",
    "metadata": {
        "height": 230,
        "width": 300,
        "format": "Png"
    }
}
```

## <a name="next-steps"></a>後續步驟

了解有關[偵測特定網域內容](concept-detecting-domain-content.md)的概念。