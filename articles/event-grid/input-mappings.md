---
title: 將自訂欄位對應到 Azure 事件格線結構描述
description: 說明如何將自訂結構描述轉換為 Azure 事件格線結構描述。
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: conceptual
ms.date: 10/02/2018
ms.author: tomfitz
ms.openlocfilehash: f79fa096484edc34294ea0a69584e12788dba647
ms.sourcegitcommit: 3856c66eb17ef96dcf00880c746143213be3806a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/02/2018
ms.locfileid: "48043384"
---
# <a name="map-custom-fields-to-event-grid-schema"></a>將自訂欄位對應到事件格線結構描述

如果您的事件資料不符合預期的[事件方格結構描述](event-schema.md)，您仍然可以使用事件方格來將事件路由傳送給訂閱者。 此文章說明如何將結構描述對應到事件格線結構描述。

[!INCLUDE [event-grid-preview-feature-note.md](../../includes/event-grid-preview-feature-note.md)]

## <a name="original-event-schema"></a>原始的事件結構描述

讓我們假設您的應用程式會以下列格式傳送事件：

```json
[
  {
    "myEventTypeField":"Created",
    "resource":"Users/example/Messages/1000",
    "resourceData":{"someDataField1":"SomeDataFieldValue"}
  }
]
```

儘管該格式不符合必要的結構描述，但事件格線可讓您將欄位對應到結構描述。 或者，您可以在原始結構描述中接收值。

## <a name="create-custom-topic-with-mapped-fields"></a>使用對應的欄位建立自訂主題

建立自訂主題時，指定如何將欄位從您的原始事件對應到事件格線結構描述。 您可用來自訂對應的屬性有三個：

* `--input-schema` 參數會指定結構描述的類型。 可用的選項為 *cloudeventv01schema*、*customeventschema* 和 *eventgridschema*。 預設值為 eventgridschema。 在您的結構描述與事件格線結構描述之間建立自訂對應時，請使用 customeventschema。 當事件位於 CloudEvents 結構描述時，則使用 cloudeventv01schema。

* `--input-mapping-default-values` 參數會針對事件格線結構描述中的欄位指定預設值。 您可以設定 `subject`、`eventtype` 和 `dataversion` 的預設值。 一般而言，當您的自訂結構描述不包含對應到那三個欄位其中一個的欄位時，您會使用此參數。 例如，您可以指定一律將該資料版本設定為 **1.0**。

* `--input-mapping-fields` 參數會將欄位從您的結構描述對應到事件格線結構描述。 您會以空格分隔的索引鍵/值組來指定值。 針對索引鍵名稱，使用事件格線欄位的名稱。 針對值，使用欄位的名稱。 您可以針對 `id`、`topic`、`eventtime`、`subject`、`eventtype` 和 `dataversion` 使用索引鍵名稱。

下列範例會建立自訂主題，其中含有某些對應和預設欄位：

```azurecli-interactive
# if you have not already installed the extension, do it now.
# This extension is required for preview features.
az extension add --name eventgrid

az eventgrid topic create \
  -n demotopic \
  -l eastus2 \
  -g myResourceGroup \
  --input-schema customeventschema \
  --input-mapping-fields eventType=myEventTypeField \
  --input-mapping-default-values subject=DefaultSubject dataVersion=1.0
```

## <a name="subscribe-to-event-grid-topic"></a>訂閱事件格線主題

訂閱自訂主題時，需指定您想要用來接收事件的結構描述。 您會使用 `--event-delivery-schema` 參數，並將它設定為 *cloudeventv01schema*、*eventgridschema* 或 *inputeventschema*。 預設值為 eventgridschema。

此節中的範例會針對事件處理常式使用佇列儲存體。 如需詳細資訊，請參閱[將自訂事件路由傳送到 Azure 佇列儲存體](custom-event-to-queue-storage.md)。

下列範例會訂閱事件方格主題，並使用事件方格結構描述：

```azurecli-interactive
az eventgrid event-subscription create \
  --topic-name demotopic \
  -g myResourceGroup \
  --name eventsub1 \
  --event-delivery-schema eventgridschema \
  --endpoint-type storagequeue \
  --endpoint <storage-queue-url>
```

下一個範例會使用事件的輸入結構描述：

```azurecli-interactive
az eventgrid event-subscription create \
  --topic-name demotopic \
  -g myResourceGroup \
  --name eventsub2 \
  --event-delivery-schema inputeventschema \
  --endpoint-type storagequeue \
  --endpoint <storage-queue-url>
```

## <a name="publish-event-to-topic"></a>將事件發行到主題

您現在已經準備好將事件傳送到自訂主題，並查看對應的結果。 下列指令碼會在[範例結構描述](#original-event-schema)中公佈事件：

```azurecli-interactive
endpoint=$(az eventgrid topic show --name demotopic -g myResourceGroup --query "endpoint" --output tsv)
key=$(az eventgrid topic key list --name demotopic -g myResourceGroup --query "key1" --output tsv)

event='[ { "myEventTypeField":"Created", "resource":"Users/example/Messages/1000", "resourceData":{"someDataField1":"SomeDataFieldValue"} } ]'

curl -X POST -H "aeg-sas-key: $key" -d "$event" $endpoint
```

現在，請查看您的佇列儲存體。 這兩個訂用帳戶在不同的結構描述中傳遞了事件。

第一個訂用帳戶使用了事件格線結構描述。 已傳遞事件的格式如下：

```json
{
  "Id": "016b3d68-881f-4ea3-8a9c-ed9246582abe",
  "EventTime": "2018-05-01T20:00:25.2606434Z",
  "EventType": "Created",
  "DataVersion": "1.0",
  "MetadataVersion": "1",
  "Topic": "/subscriptions/<subscription-id>/resourceGroups/myResourceGroup/providers/Microsoft.EventGrid/topics/demotopic",
  "Subject": "DefaultSubject",
  "Data": {
    "myEventTypeField": "Created",
    "resource": "Users/example/Messages/1000",
    "resourceData": { "someDataField1": "SomeDataFieldValue" } 
  }
}
```

這些欄位包含自訂主題的對應。 **myEventTypeField** 會對應到 **EventType**。 使用了 **DataVersion** 和 **Subject** 的預設值。 **Data** 物件包含原始的事件結構描述欄位。

第二個訂用帳戶使用了輸入事件結構描述。 已傳遞事件的格式如下：

```json
{
  "myEventTypeField": "Created",
  "resource": "Users/example/Messages/1000",
  "resourceData": { "someDataField1": "SomeDataFieldValue" }
}
```

請注意已傳遞原始欄位。

## <a name="next-steps"></a>後續步驟

* 如需事件傳遞和重試的相關資訊，請參閱[事件格線訊息傳遞與重試](delivery-and-retry.md)。
* 如需 Event Grid 的簡介，請參閱[關於 Event Grid](overview.md)。
* 若要快速地開始使用 Event Grid，請參閱[使用 Azure Event Grid 建立和路由傳送自訂事件](custom-event-quickstart.md)。
