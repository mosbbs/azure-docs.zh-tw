---
title: Azure 地圖服務的多個路線 | Microsoft Docs
description: 使用 Azure 地圖服務尋找不同行進模式的路線
author: walsehgal
ms.author: v-musehg
ms.date: 10/29/2018
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 67b68489f2e06b9149f842f293a769fa7f688be0
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/31/2018
ms.locfileid: "50412698"
---
# <a name="find-routes-for-different-modes-of-travel-using-azure-maps"></a>使用 Azure 地圖服務尋找不同行進模式的路線

本教學課程說明如何使用 Azure 地圖服務帳戶和路線規劃服務來尋找景點路線 (依行進模式設定優先順序)。 您會在地圖上顯示兩條不同的路線，一條是汽車專用，一條則是可能會因為載送貨物的高度、重量或危險性而導致路線受限的卡車專用。 在本教學課程中，您了解如何：

> [!div class="checklist"]
> * 使用地圖控制項 API 建立新的網頁
> * 在地圖上以視覺方式呈現流量
> * 建立會宣告行進模式的路線查詢
> * 在地圖上顯示多條路線

## <a name="prerequisites"></a>必要條件

在繼續之前，請遵循第一個教學課程中的步驟來[建立 Azure 地圖服務帳戶](./tutorial-search-location.md#createaccount)，並[取得帳戶的訂用帳戶金鑰](./tutorial-search-location.md#getkey)。

## <a name="create-a-new-map"></a>建立新的地圖

下列步驟顯示如何建立內嵌地圖控制項 API 的靜態 HTML 網頁。

1. 在您的本機機器上建立新檔案，並將其命名為 **MapTruckRoute.html**。
2. 在檔案中新增下列 HTML 元件：

    ```HTML
    <!DOCTYPE html>
    <html>
    <head>
        <title>Multiple routes by mode of travel</title>
        
        <meta charset="utf-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no" />

        <!-- Add references to the Azure Maps Map control JavaScript and CSS files. -->
        <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/css/atlas.min.css?api-version=1" type="text/css" />
        <script src="https://atlas.microsoft.com/sdk/js/atlas.min.js?api-version=1"></script>

        <!-- Add a reference to the Azure Maps Services Module JavaScript file. -->
        <script src="https://atlas.microsoft.com/sdk/js/atlas-service.js?api-version=1"></script>

        <script>
            var map, datasource, client;

            function GetMap() {
                //Add Map Control JavaScript code here.
            }
        </script>
        <style>
            html,
            body {
                width: 100%;
                height: 100%;
                padding: 0;
                margin: 0;
            }

            #myMap {
                position: relative;
                width: 100%;
                height: 100%;
            }
        </style>
    </head>
    <body onload="GetMap()">
        <div id="myMap"></div>
    </body>
    </html>
    ```
    
    請注意，HTML 標頭包含 Azure 地圖控制項程式庫所裝載的 CSS 和 JavaScript 資源檔案。 請注意頁面本文上的 `onload` 事件，此事件在頁面本文載入時將會呼叫 `GetMap` 函式。 此函式會包含內嵌的 JavaScript 程式碼，以便存取 Azure 地圖服務 API。

3. 在 `GetMap` 函式中新增下列 JavaScript 程式碼。 將字串 **\<您的 Azure 地圖服務金鑰\>** 取代為您從地圖服務帳戶所複製的主要金鑰。

    ```JavaScript
    //Add your Azure Maps subscription key to the map SDK. 
    atlas.setSubscriptionKey('<Your Azure Maps Key>');

    //Initialize a map instance.
    map = new atlas.Map('myMap');
    ```

    **atlas.Map** 是 Azure 地圖控制項 API 的元件，可供控制視覺化互動式網路地圖。

4. 儲存檔案並在瀏覽器中將其開啟。 至此，您已擁有可以進一步發展的基本地圖了。

   ![檢視基本地圖](./media/tutorial-prioritized-routes/basic-map.png)

## <a name="visualize-traffic-flow"></a>以視覺方式呈現流量

1. 在地圖上新增流量顯示。 `map.events.add` 可確保新增至地圖的所有地圖函式，都會在地圖完整載入之後載入。

    ```JavaScript
    map.events.add("load", function() {
        // Add Traffic Flow to the Map
        map.setTraffic({
            flow: "relative"
        });
    });
    ```
    
     載入事件會新增至地圖，而地圖將在地圖資源已完全載入時引發。 在地圖載入事件處理常式中，地圖上的流量設定會設為 `relative`，這是相對於自由流量的道路速度。 您也可以將其設定為道路的 `absolute` 速度，或設定為 `relative-delay`，以顯示不同於自由流量的相對速度。

2. 儲存 **MapTruckRoute.html** 檔案，並重新整理瀏覽器頁面。 您應該會看到洛杉磯的街道及其目前的流量資料。

   ![檢視流量地圖](./media/tutorial-prioritized-routes/traffic-map.png)

<a id="queryroutes"></a>

## <a name="define-how-the-route-will-be-rendered"></a>定義呈現路線的方式

在本教學課程中，會在地圖上計算並呈現兩個路線。 一個路線使用供汽車行駛的道路，另一個路線則使用供卡車行駛的道路。 在呈現時，我們將為路線的起點和終點顯示符號圖示，並以不同的彩色線條表示每個路線的路徑。

1. 在 GetMap 函式中，在地圖完成初始化之後新增下列 JavaScript 程式碼。

    ```JavaScript
    //Wait until the map resources have fully loaded.
    map.events.add('load', function () {

        //Create a data source and add it to the map.
        datasource = new atlas.source.DataSource();
        map.sources.add(datasource);

        //Add a layer for rendering the route lines and have it render under the map labels.
        map.layers.add(new atlas.layer.LineLayer(datasource, null, {
            strokeColor: ['get', 'strokeColor'],
            strokeWidth: ['get', 'strokeWidth'],
            lineJoin: 'round',
            lineCap: 'round',
            filter: ['==', '$type', 'LineString']
        }), 'labels');

        //Add a layer for rendering point data.
        map.layers.add(new atlas.layer.SymbolLayer(datasource, null, {
            iconOptions: {
                image: ['get', 'icon'],
                allowOverlap: true
            },
            textOptions: {
                textField: ['get', 'title'],
                offset: [0, 1.2]
            },
            filter: ['==', '$type', 'Point']
        }));
    });
    ```
    
    載入事件會新增至地圖，而地圖將在地圖資源已完全載入時引發。 在地圖載入事件處理常式中，會建立資料來源以儲存路線以及起點和終點。 系統會建立線條圖層並將其附加至資料來源，以定義呈現路線的方式。 此外也會使用運算式，從路線特性的屬性中擷取線條的寬度和色彩。 此時會新增一個篩選條件，以確定此圖層僅呈現 GeoJSON LineString 資料。 將圖層新增至地圖時，會傳入值為 `'labels'` 的第二個參數，而指定要將此圖層呈現在地圖標籤下方。 這樣可以確保路線不會遮住道路標籤。 系統會建立符號圖層，並將其附加至資料來源。 此圖層會指定起點和終點的呈現方式，而此案例中已新增運算式，用以從每個點物件的屬性中擷取圖示影像和文字標籤資訊。 
    
2. 在此教學課程中，將起點設在西雅圖的一家虛構公司 Fabrikam，並將終點設在 Microsoft 辦公室。 在地圖載入事件處理常式中，新增下列程式碼。

    ```JavaScript
    //Create the GeoJSON objects which represent the start and end point of the route.
    var startPoint = new atlas.data.Feature(new atlas.data.Point([-122.356099, 47.580045]), {
        title: 'Fabrikam, Inc.',
        icon: 'pin-round-blue'
    });

    var endPoint = new atlas.data.Feature(new atlas.data.Point([-122.201164, 47.616940]), {
        title: 'Microsoft - Lincoln Square',
        icon: 'pin-blue'
    });
    ```
    
    此程式碼會建立兩個 [GeoJSON 物件](https://en.wikipedia.org/wiki/GeoJSON)，代表路線的起點和終點。 `title` 和 `icon` 屬性會新增至每個點。

3. 接著，請新增下列 JavaScript 程式碼，以在地圖中新增起點和終點的圖釘：

    ```JavaScript
    //Add the data to the data source.
    datasource.add([startPoint, endPoint]);

    //Fit the map window to the bounding box defined by the start and end positions.
    map.setCamera({
        bounds: atlas.data.BoundingBox.fromData([startPoint, endPoint]),
        padding: 100
    });
    ```
    
    起點和終點會新增至資料來源。 系統會使用 `atlas.data.BoundingBox.fromData` 函式計算起點和終點的週框方塊。 此週框方塊會用來透過 `map.setCamera` 函式設定起點和終點的地圖相機檢視。 系統會加入邊框間距，以補償符號圖示的像素維度。

4. 儲存檔案並重新整理瀏覽器，以查看地圖上顯示的圖釘。 現在地圖會以西雅圖作為中心，而且您會看到以圓形藍色圖釘標示的起點，和以藍色圖釘標示的終點。

   ![檢視有起點和終點的地圖](./media/tutorial-prioritized-routes/pins-map.png)

<a id="multipleroutes"></a>

## <a name="render-routes-prioritized-by-mode-of-travel"></a>轉譯依行進模式設定優先順序的路線

本節說明如何使用地圖服務的路線規劃服務 API，根據運輸模式，尋找從給定起點到目的地的多條路線。 路線規劃服務會提供 API 來規劃兩個位置之間「最快速」、「最短」、「最環保」或「驚心動魄」的路線，並將即時路況考慮進去。 它也可讓使用者使用 Azure 廣泛的歷史路況資料庫，並預測任何日期和時間的路線時間，以規劃日後的路線。 如需詳細資訊，請參閱 [GetRoute 指示](https://docs.microsoft.com/rest/api/maps/route/getroutedirections)。 以下所有程式碼區塊都應該新增至**地圖載入 eventListener 內**，以確保會在地圖完整載入之後載入。

1. 藉由在地圖載入事件處理常式中新增下列 Javascript 程式碼，將服務用戶端具現化。

    ```JavaScript
    //If the service client hasn't been created, create an instance of it.
    if (!client) {
        client = new atlas.service.Client(atlas.getSubscriptionKey());
    }
    ```
    
2. 新增下列程式碼區塊，以建構路由查詢字串。

    ```JavaScript
    //Create the route request with the query being the start and end point in the format 'startLongitude,startLatitude:endLongitude,endLatitude'.
    var routeQuery = startPoint.geometry.coordinates[1] +
        ',' +
        startPoint.geometry.coordinates[0] +
        ':' +
        endPoint.geometry.coordinates[1] +
        ',' +
        endPoint.geometry.coordinates[0];
    ```

3. 新增下列 JavaScript 程式碼，以要求針對載運 USHazmatClass2 類貨物的卡車提供路線，並顯示結果：

    ```JavaScript
    //Execute the truck route query then add the route to the map.
    client.route.getRouteDirections(routeQuery, {
        travelMode: 'truck',
        vehicleWidth: 2,
        vehicleHeight: 2,
        vehicleLength: 5,
        vehicleLoadType: 'USHazmatClass2'
    }).then(function (response) {
        // Parse the response into GeoJSON
        var geoJsonResponse = new atlas.service.geojson.GeoJsonRouteDirectionsResponse(response);

        //Get the route line and add some style properties to it.
        var routeLine = geoJsonResponse.getGeoJsonRoutes().features[0];
        routeLine.properties.strokeColor = '#2272B9';
        routeLine.properties.strokeWidth = 9;

        //Add the route line to the data source. We want this to render below the car route which will likely be added to the data source faster, so insert it at index 0.
        datasource.add(routeLine, 0);
    });
    ```
    上方的程式碼片段會透過 [getRouteDirections](https://docs.microsoft.com/javascript/api/azure-maps-rest/services.route?view=azure-iot-typescript-latest#getroutedirections) 方法來查詢 Azure 地圖服務路線服務，然後使用 [getGeoJsonRouteDirectionsResponse](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.geojson.geojsonroutedirectionsresponse?view=azure-iot-typescript-latest) 將回應剖析為 GeoJSON 格式。 接著，將會建立傳回路線的座標陣列，並將其新增至資料來源，但也會新增 0 的索引，以確保它會在資料來源中的任何其他線條之前呈現。 之所以這麼做，是因為卡車路線計算的速度通常比汽車路線計算慢，若卡車路線在汽車路線之後新增至資料來源，則會呈現於汽車路線之上。 系統會將兩個屬性新增至卡車路線，一個呈現為鮮明藍色的筆觸色彩，和一個寬度為 9 像素的筆觸。 

4. 新增下列 JavaScript 程式碼以要求卡車路線並顯示結果：

    ```JavaScript
    //Execute the car route query then add the route to the map.
    client.route.getRouteDirections(routeQuery).then(function (response) {
        // Parse the response into GeoJSON
        var geoJsonResponse = new atlas.service.geojson.GeoJsonRouteDirectionsResponse(response);

        //Get the route line and add some style properties to it.
        var routeLine = geoJsonResponse.getGeoJsonRoutes().features[0];
        routeLine.properties.strokeColor = '#B76DAB';
        routeLine.properties.strokeWidth = 5;

        //Add the route line to the data source.
        datasource.add(routeLine);
    });
    ```
    此程式碼片段會對汽車使用相同的卡車路線查詢。 它會透過 [getRouteDirections](https://docs.microsoft.com/javascript/api/azure-maps-rest/services.route?view=azure-iot-typescript-latest#getroutedirections) 方法來查詢 Azure 地圖服務路線服務，然後使用 [getGeoJsonRouteDirectionsResponse](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.geojson.geojsonroutedirectionsresponse?view=azure-iot-typescript-latest) 將回應剖析為 GeoJSON 格式。 接著會建立傳回路線的座標陣列，並將其新增至資料來源。 系統會將兩個屬性新增至汽車路線，一個紫色的筆觸色彩，和一個寬度為 5 像素的筆觸。 

5. 儲存 **MapTruckRoute.html** 檔案並重新整理瀏覽器，以觀察結果。 如果您成功連線到地圖服務的 API，您應該會看到類似下面的地圖。

    ![使用 Azure 路線規劃服務設定優先順序的路線](./media/tutorial-prioritized-routes/prioritized-routes.png)

    卡車路線是較粗的藍色線，汽車路線則為較細的紫色線。 汽車路線會透過 I-90 穿越華盛頓湖，期間會經由隧道穿過住宅區底下，因此不得載運危險廢棄物貨物。 卡車路線 (指定了 USHazmatClass2 貨物類型) 則會正確引導使用不同的高速公路。

## <a name="next-steps"></a>後續步驟

在本教學課程中，您已了解如何：

> [!div class="checklist"]
> * 使用地圖控制項 API 建立新的網頁
> * 在地圖上以視覺方式呈現流量
> * 建立會宣告行進模式的路線查詢
> * 在地圖上顯示多條路線

您可以在這裡存取本教學課程的程式碼範例：

> [Azure 地圖服務的多個路線](https://github.com/Azure-Samples/AzureMapsCodeSamples/blob/master/AzureMapsCodeSamples/Tutorials/truckRoute.html)

[請參閱此處的實際範例](https://azuremapscodesamples.azurewebsites.net/?sample=Multiple%20routes%20by%20mode%20of%20travel)

若要深入了解 Azure 地圖服務的涵蓋範圍和功能：

> [!div class="nextstepaction"]
> [縮放層級和圖格格線](zoom-levels-and-tile-grid.md)

若要查看更多程式碼範例和互動式編碼體驗：

> [!div class="nextstepaction"]
> [如何使用地圖控制項](how-to-use-map-control.md)
