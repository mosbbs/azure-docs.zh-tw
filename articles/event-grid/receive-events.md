---
title: 從 Azure 事件格線接收 HTTP 端點的事件
description: 描述如何驗證 HTTP 端點，然後從 Azure 事件格線接收和還原序列化事件
services: event-grid
author: banisadr
manager: darosa
ms.service: event-grid
ms.topic: conceptual
ms.date: 10/01/2018
ms.author: babanisa
ms.openlocfilehash: 7d8ee60f033d824a3ff83a7c6948c72160e24c1d
ms.sourcegitcommit: 5843352f71f756458ba84c31f4b66b6a082e53df
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/01/2018
ms.locfileid: "47584280"
---
# <a name="receive-events-to-an-http-endpoint"></a>接收 HTTP 端點的事件

本文說明如何[驗證 HTTP 端點](security-authentication.md#webhook-event-delivery)以從事件訂閱接收事件，然後接收和還原序列化事件。 本文針對示範用途使用 Azure Function，但是不論應用程式裝載的位置都適用相同概念。

> [!NOTE]
> **強烈**建議您在使用事件格線觸發 Azure Function 時使用[事件格線觸發程序](../azure-functions/functions-bindings-event-grid.md)。 這裡使用的泛型 WebHook 觸發程序是示範。

## <a name="prerequisites"></a>必要條件

您需要具有 HTTP 觸發函式的函數應用程式。

## <a name="add-dependencies"></a>新增相依性

若您正以 .NET 進行開發，請為 `Microsoft.Azure.EventGrid`[Nuget 套件](https://www.nuget.org/packages/Microsoft.Azure.EventGrid)[新增相依性](../azure-functions/functions-reference-csharp.md#referencing-custom-assemblies)到您的函式。 本文的範例需要 1.4.0 版或更新版本。

其他語言的 SDK 可以透過[發行 SDK](./sdk-overview.md#data-plane-sdks) 參考取得。 這些套件包含原生事件類型的模型，例如 `EventGridEvent`、`StorageBlobCreatedEventData` 和 `EventHubCaptureFileCreatedEventData`。

按一下 Azure Function 中的 [檢視檔案] 連結 (Azure Functions 入口網站中最右邊的窗格)，然後建立名為 project.json 的檔案。 將下列內容新增至 `project.json` 檔案，並加以儲存：

 ```json
{
  "frameworks": {
    "net46":{
      "dependencies": {
        "Microsoft.Azure.EventGrid": "2.0.0"
      }
    }
   }
}
```

![已新增 NuGet 套件](./media/receive-events/add-dependencies.png)

## <a name="endpoint-validation"></a>端點驗證

首先您應該做的是處理 `Microsoft.EventGrid.SubscriptionValidationEvent` 事件。 每次有人訂閱事件時，事件方格即會將驗證事件傳送至在資料承載中具有 `validationCode` 的端點。 需要端點才能在回應主體中回應以[證明端點有效且為您所擁有](security-authentication.md#webhook-event-delivery)。 若您是使用[事件方格觸發程序](../azure-functions/functions-bindings-event-grid.md)而非 WebHook 觸發的函式，系統會為您處理端點的驗證。 如果您使用第三方 API 服務 (例如 [Zapier](https://zapier.com) 或 [IFTTT](https://ifttt.com/))，可能就無法以程式設計方式回應驗證程式碼。 針對那些服務，您可以使用要在訂用帳戶驗證事件中傳送的驗證 URL，以手動方式驗證訂用帳戶。 在 `validationUrl` 屬性中複製該 URL，並透過 REST 用戶端或您的網頁瀏覽器傳送 GET 要求。

手動驗證為預覽狀態。 若要使用它，您必須為 [Azure CLI](/cli/azure/install-azure-cli) 安裝[事件格線延伸模組](/cli/azure/azure-cli-extensions-list)。 您可以使用 `az extension add --name eventgrid` 進行安裝。 若您正使用 REST API，請確定您使用了 `api-version=2018-05-01-preview`。

在 C# 中，`DeserializeEventGridEvents()` 函式會將事件方格事件還原序列化。 事件資料會還原序列化為適當的類型，例如 StorageBlobCreatedEventData。 使用 `Microsoft.Azure.EventGrid.EventTypes` 類別，以取得支援的事件類型和名稱。

若要以程式設計方式回應驗證程式碼，請使用下列程式碼。 您可以在[事件方格取用者範例](https://github.com/Azure-Samples/event-grid-dotnet-publish-consume-events/tree/master/EventGridConsumer)中找到相關的範例。

```csharp
using System.Net;
using Newtonsoft.Json;
using Microsoft.Azure.EventGrid.Models;
using Microsoft.Azure.EventGrid;

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
{
    log.Info($"C# HTTP trigger function begun");
    string response = string.Empty;

    string requestContent = await req.Content.ReadAsStringAsync();
    log.Info($"Received events: {requestContent}");

    EventGridSubscriber eventGridSubscriber = new EventGridSubscriber();

    EventGridEvent[] eventGridEvents = eventGridSubscriber.DeserializeEventGridEvents(requestContent);

    foreach (EventGridEvent eventGridEvent in eventGridEvents)
    {
        if (eventGridEvent.Data is SubscriptionValidationEventData)
        {
            var eventData = (SubscriptionValidationEventData)eventGridEvent.Data;
            log.Info($"Got SubscriptionValidation event data, validation code: {eventData.ValidationCode}, topic: {eventGridEvent.Topic}");
            // Do any additional validation (as required) and then return back the below response

            var responseData = new SubscriptionValidationResponse()
            {
                ValidationResponse = eventData.ValidationCode
            };

            return req.CreateResponse(HttpStatusCode.OK, responseData);
        }
    }

    return req.CreateResponse(HttpStatusCode.OK, response);
}
```

```javascript
module.exports = function (context, req) {
    context.log('JavaScript HTTP trigger function begun');
    var validationEventType = "Microsoft.EventGrid.SubscriptionValidationEvent";

    for (var events in req.body) {
        var body = req.body[events];
        // Deserialize the event data into the appropriate type based on event type
        if (body.data && body.eventType == validationEventType) {
            context.log("Got SubscriptionValidation event data, validation code: " + body.data.validationCode + " topic: " + body.topic);

            // Do any additional validation (as required) and then return back the below response
            var code = body.data.validationCode;
            context.res = { status: 200, body: { "ValidationResponse": code } };
        }
    }
    context.done();
};
```

### <a name="test-validation-response"></a>測試驗證回應

藉由將範例事件貼至函式的測試欄位來測試驗證回應函式：

```json
[{
  "id": "2d1781af-3a4c-4d7c-bd0c-e34b19da4e66",
  "topic": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "subject": "",
  "data": {
    "validationCode": "512d38b6-c7b8-40c8-89fe-f46f9e9622b6"
  },
  "eventType": "Microsoft.EventGrid.SubscriptionValidationEvent",
  "eventTime": "2018-01-25T22:12:19.4556811Z",
  "metadataVersion": "1",
  "dataVersion": "1"
}]
```

當您按一下 [執行] 時，在主體中輸出應該是「200 OK」和 `{"ValidationResponse":"512d38b6-c7b8-40c8-89fe-f46f9e9622b6"}`：

![驗證回應](./media/receive-events/validation-response.png)

## <a name="handle-blob-storage-events"></a>處理 Blob 儲存體事件

現在，我們要擴充函式以處理 `Microsoft.Storage.BlobCreated`：

```cs
using System.Net;
using Newtonsoft.Json;
using Microsoft.Azure.EventGrid.Models;
using Microsoft.Azure.EventGrid;

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
{
    log.Info($"C# HTTP trigger function begun");
    string response = string.Empty;

    string requestContent = await req.Content.ReadAsStringAsync();
    log.Info($"Received events: {requestContent}");

    EventGridSubscriber eventGridSubscriber = new EventGridSubscriber();

    EventGridEvent[] eventGridEvents = eventGridSubscriber.DeserializeEventGridEvents(requestContent);

    foreach (EventGridEvent eventGridEvent in eventGridEvents)
    {
        if (eventGridEvent.Data is SubscriptionValidationEventData)
        {
            var eventData = (SubscriptionValidationEventData)eventGridEvent.Data;
            log.Info($"Got SubscriptionValidation event data, validation code: {eventData.ValidationCode}, topic: {eventGridEvent.Topic}");
            // Do any additional validation (as required) and then return back the below response

            var responseData = new SubscriptionValidationResponse()
            {
                ValidationResponse = eventData.ValidationCode
            };

            return req.CreateResponse(HttpStatusCode.OK, responseData);
        }
        else if (eventGridEvent.Data is StorageBlobCreatedEventData)
        {
            var eventData = (StorageBlobCreatedEventData)eventGridEvent.Data;
            log.Info($"Got BlobCreated event data, blob URI {eventData.Url}");
        }
    }

    return req.CreateResponse(HttpStatusCode.OK, response);
}
```

```javascript
module.exports = function (context, req) {
    context.log('JavaScript HTTP trigger function begun');
    var validationEventType = "Microsoft.EventGrid.SubscriptionValidationEvent";
    var storageBlobCreatedEvent = "Microsoft.Storage.BlobCreated";

    for (var events in req.body) {
        var body = req.body[events];
        // Deserialize the event data into the appropriate type based on event type  
        if (body.data && body.eventType == validationEventType) {
            context.log("Got SubscriptionValidation event data, validation code: " + body.data.validationCode + " topic: " + body.topic);

            // Do any additional validation (as required) and then return back the below response
            var code = body.data.validationCode;
            context.res = { status: 200, body: { "ValidationResponse": code } };
        }

        else if (body.data && body.eventType == storageBlobCreatedEvent) {
            var blobCreatedEventData = body.data;
            context.log("Relaying received blob created event payload:" + JSON.stringify(blobCreatedEventData));
        }
    }
    context.done();
};

```

### <a name="test-blob-created-event-handling"></a>測試 Blob 建立事件處理

測試函式的新功能，方法是將 [Blob 儲存體事件](./event-schema-blob-storage.md#example-event)放入測試欄位並執行：

```json
[{
  "topic": "/subscriptions/{subscription-id}/resourceGroups/Storage/providers/Microsoft.Storage/storageAccounts/xstoretestaccount",
  "subject": "/blobServices/default/containers/testcontainer/blobs/testfile.txt",
  "eventType": "Microsoft.Storage.BlobCreated",
  "eventTime": "2017-06-26T18:41:00.9584103Z",
  "id": "831e1650-001e-001b-66ab-eeb76e069631",
  "data": {
    "api": "PutBlockList",
    "clientRequestId": "6d79dbfb-0e37-4fc4-981f-442c9ca65760",
    "requestId": "831e1650-001e-001b-66ab-eeb76e000000",
    "eTag": "0x8D4BCC2E4835CD0",
    "contentType": "text/plain",
    "contentLength": 524288,
    "blobType": "BlockBlob",
    "url": "https://example.blob.core.windows.net/testcontainer/testfile.txt",
    "sequencer": "00000000000004420000000000028963",
    "storageDiagnostics": {
      "batchId": "b68529f3-68cd-4744-baa4-3c0498ec19f0"
    }
  },
  "dataVersion": "",
  "metadataVersion": "1"
}]
```

您應該會在函式記錄中看到 Blob URL 輸出：

![輸出記錄](./media/receive-events/blob-event-response.png)

您也可以進行測試，方法是建立 Blob 儲存體帳戶或一般用途 V2 (GPv2) 儲存體帳戶、[新增事件訂閱](../storage/blobs/storage-blob-event-quickstart.md)，以及將端點設為函式 URL：

![函式 URL](./media/receive-events/function-url.png)

## <a name="handle-custom-events"></a>處理自訂事件

最後，讓我們再次擴充函式，讓它也可以處理自訂事件。 

在 C# 中，SDK 支援將事件類型名稱對應到事件的資料類型。 使用 `AddOrUpdateCustomEventMapping()` 函式來對應自訂事件。

為您的事件 `Contoso.Items.ItemReceived` 新增檢查。 最終的程式碼應該會如下所示：

```cs
using System.Net;
using Newtonsoft.Json;
using Microsoft.Azure.EventGrid.Models;
using Microsoft.Azure.EventGrid;

class ContosoItemReceivedEventData
{
    [JsonProperty(PropertyName = "itemSku")]
    public string ItemSku { get; set; }
}

public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
{
    log.Info($"C# HTTP trigger function begun");
    string response = string.Empty;

    string requestContent = await req.Content.ReadAsStringAsync();
    log.Info($"Received events: {requestContent}");

    EventGridSubscriber eventGridSubscriber = new EventGridSubscriber();
    eventGridSubscriber.AddOrUpdateCustomEventMapping("Contoso.Items.ItemReceived", typeof(ContosoItemReceivedEventData));
    EventGridEvent[] eventGridEvents = eventGridSubscriber.DeserializeEventGridEvents(requestContent);

    foreach (EventGridEvent eventGridEvent in eventGridEvents)
    {
        if (eventGridEvent.Data is SubscriptionValidationEventData)
        {
            var eventData = (SubscriptionValidationEventData)eventGridEvent.Data;
            log.Info($"Got SubscriptionValidation event data, validation code: {eventData.ValidationCode}, topic: {eventGridEvent.Topic}");
            // Do any additional validation (as required) and then return back the below response

            var responseData = new SubscriptionValidationResponse()
            {
                ValidationResponse = eventData.ValidationCode
            };

            return req.CreateResponse(HttpStatusCode.OK, responseData);
        }
        else if (eventGridEvent.Data is StorageBlobCreatedEventData)
        {
            var eventData = (StorageBlobCreatedEventData)eventGridEvent.Data;
            log.Info($"Got BlobCreated event data, blob URI {eventData.Url}");
        }
        else if (eventGridEvent.Data is ContosoItemReceivedEventData)
        {
            var eventData = (ContosoItemReceivedEventData)eventGridEvent.Data;
            log.Info($"Got ContosoItemReceived event data, item SKU {eventData.ItemSku}");
        }
    }

    return req.CreateResponse(HttpStatusCode.OK, response);
}
```

```javascript
module.exports = function (context, req) {
    context.log('JavaScript HTTP trigger function begun');
    var validationEventType = "Microsoft.EventGrid.SubscriptionValidationEvent";
    var storageBlobCreatedEvent = "Microsoft.Storage.BlobCreated";
    var customEventType = "Contoso.Items.ItemReceived";

    for (var events in req.body) {
        var body = req.body[events];
        // Deserialize the event data into the appropriate type based on event type
        if (body.data && body.eventType == validationEventType) {
            context.log("Got SubscriptionValidation event data, validation code: " + body.data.validationCode + " topic: " + body.topic);

            // Do any additional validation (as required) and then return back the below response
            var code = body.data.validationCode;
            context.res = { status: 200, body: { "ValidationResponse": code } };
        }

        else if (body.data && body.eventType == storageBlobCreatedEvent) {
            var blobCreatedEventData = body.data;
            context.log("Relaying received blob created event payload:" + JSON.stringify(blobCreatedEventData));
        }

        else if (body.data && body.eventType == customEventType) {
            var payload = body.data;
            context.log("Relaying received custom payload:" + JSON.stringify(payload));
        }
    }
    context.done();
};
```

### <a name="test-custom-event-handling"></a>測試自訂事件處理

最後，測試您的函式現在是否可以處理您的自訂事件類型：

```json
[{
    "subject": "Contoso/foo/bar/items",
    "eventType": "Contoso.Items.ItemReceived",
    "eventTime": "2017-08-16T01:57:26.005121Z",
    "id": "602a88ef-0001-00e6-1233-1646070610ea",
    "data": { 
            "itemSku": "Standard"
            },
    "dataVersion": "",
    "metadataVersion": "1"
}]
```

您也可以即時測試此功能，方法是 [從入口網站傳送自訂事件與 CURL](./custom-event-quickstart-portal.md)，或使用可以張貼到端點的任何服務或應用程式 (例如 [Postman](https://www.getpostman.com/)) [張貼到自訂主題](./post-to-custom-topic.md)。 在端點設為函式 URL 時建立自訂主題和事件訂閱。

## <a name="next-steps"></a>後續步驟

* 探索 [Azure 事件格線管理和發行 SDK](./sdk-overview.md)
* 深入了解如何[張貼到自訂主題](./post-to-custom-topic.md)
* 嘗試其中一個深入的事件格線和函式教學課程，例如[調整上傳至 Blob 儲存體的映像大小](resize-images-on-storage-blob-upload-event.md)
