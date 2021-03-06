---
title: 使用 Azure Maps 查找路线 | Microsoft Docs
description: 使用 Azure Maps 查找前往兴趣点的路线
author: walsehgal
ms.author: v-musehg
ms.date: 11/14/2018
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: a3807dc792c2e56c3e7c1b74f7d3e8f73ac0f4b0
ms.sourcegitcommit: 275eb46107b16bfb9cf34c36cd1cfb000331fbff
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2018
ms.locfileid: "51705083"
---
# <a name="route-to-a-point-of-interest-using-azure-maps"></a>使用 Azure Maps 查找前往兴趣点的路线

本教程介绍如何使用 Azure Maps 帐户和路线服务 SDK 来查找前往兴趣点的路线。 本教程介绍如何执行下列操作：

> [!div class="checklist"]
> * 使用 Map Control API 创建新网页
> * 设置地址坐标
> * 在路线服务中查询兴趣点的方向

## <a name="prerequisites"></a>先决条件

在继续之前，按照前面的教程中的步骤[创建 Azure Maps 帐户](./tutorial-search-location.md#createaccount)并且[获取帐户的订阅密钥](./tutorial-search-location.md#getkey)。

<a id="getcoordinates"></a>

## <a name="create-a-new-map"></a>创建新地图

以下步骤说明如何使用 Map Control API 创建一个嵌入式静态 HTML 页面。

1. 在本地计算机上，创建一个新文件并将其命名为 **MapRoute.html**。
2. 将以下 HTML 组件添加到该文件：

    ```HTML
    <!DOCTYPE html>
    <html>
    <head>
        <title>Map Route</title>
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

            #map {
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
    
    请注意，HTML 标头包含 Azure 地图控件库托管的 CSS 和 JavaScript 资源文件。 请注意页面正文中的 `onload` 事件。当页面正文加载时，此事件会调用 `GetMap` 函数。 此函数会包含用于访问 Azure Maps API 的内联 JavaScript 代码。 

3. 将以下 JavaScript 代码添加到 `GetMap` 函数。 使用从 Maps 帐户复制的主键替换字符串 **\<Your Azure Maps Key\>**。

    ```JavaScript
    //Add your Azure Maps subscription key to the map SDK. 
    atlas.setSubscriptionKey('<Your Azure Maps Key>');

    //Initialize a map instance.
    map = new atlas.Map('myMap');
    ```

    `atlas.Map` 提供一个可视和交互式的 Web 地图控件，它是 Azure Map Control API 的一个组件。

4. 保存文件并在浏览器中打开它。 此时，你有一个可以进一步开发的基础地图。

   ![查看基础地图](./media/tutorial-route-location/basic-map.png)

## <a name="define-how-the-route-will-be-rendered"></a>定义路线的呈现方式

在本教程中，在呈现简单的路线时，将使用符号图标来表示路线的起点和终点，用一条线来表示路线的路径。

1. 在 GetMap 函数中，请在初始化地图以后，添加以下 JavaScript 代码。

    ```JavaScript
    //Wait until the map resources have fully loaded.
    map.events.add('load', function () {

        //Create a data source and add it to the map.
        datasource = new atlas.source.DataSource();
        map.sources.add(datasource);

        //Add a layer for rendering the route lines and have it render under the map labels.
        map.layers.add(new atlas.layer.LineLayer(datasource, null, {
            strokeColor: '#2272B9',
            strokeWidth: 5,
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
    
    将会向地图添加一个加载事件。当地图资源完全加载以后，该事件会触发。 在地图加载事件处理程序中，将会创建一个数据源来存储路线以及起点和终点。 将会创建一个路线层并将其附加到数据源，以便定义路线的呈现方式。 路线会呈现为蓝色，其宽度为 5 个像素，其连接处和端口处以圆形表示。 将会添加一个筛选器，确保此层仅呈现 GeoJSON LineString 数据。 向地图添加此层时，会传递另一个值为 `'labels'` 的参数，指定在地图标签下呈现此层。 这样可确保路线不覆盖道路标签。 将会创建一个符号层并将其附加到数据源。 此层指定起点和终点的呈现方式。此示例中添加了表达式，用于从每个点对象的属性中检索图标图像和文本标签信息。 
    
2. 在本教程中，将起点设为 Microsoft，将终点设为西雅图的加油站。 在地图加载事件处理程序中，请添加以下代码。

    ```JavaScript
    //Create the GeoJSON objects which represent the start and end point of the route.
    var startPoint = new atlas.data.Feature(new atlas.data.Point([-122.130137, 47.644702]), {
        title: 'Microsoft',
        icon: 'pin-round-blue'
    });

    var endPoint = new atlas.data.Feature(new atlas.data.Point([-122.3352, 47.61397]), {
        title: 'Contoso Oil & Gas',
        icon: 'pin-blue'
    });    
    ```

    此代码创建两个 [GeoJSON 点对象](https://en.wikipedia.org/wiki/GeoJSON)来表示路线的起点和终点。 将会向每个点添加 `title` 和 `icon` 属性。
    
3. 接下来，添加以下 JavaScript 代码，将起点和终点的图钉添加到地图中：

    ```JavaScript
    //Add the data to the data source.
    datasource.add([startPoint, endPoint]);
    
    //Fit the map window to the bounding box defined by the start and end positions.
    map.setCamera({
        bounds: atlas.data.BoundingBox.fromData([startPoint, endPoint]),
        padding: 100
    });
    ```
    
    起点和终点会添加到数据源。 起点和终点的边框使用 `atlas.data.BoundingBox.fromData` 函数计算。 此边框用于通过 **map.setCamera** 函数设置基于起点和终点的地图照相机视图。 将会添加一个衬距来弥补符号图标的像素尺寸。

3. 保存“MapRoute.html”文件并刷新浏览器。 现在，地图的中心为西雅图，可以看到标记起点的圆形蓝色图钉和标记终点的蓝色图钉。

   ![查看标记了起点和终点的地图](./media/tutorial-route-location/map-pins.png)

<a id="getroute"></a>

## <a name="get-directions"></a>获取方向

本部分介绍如何使用 Maps 路线服务 API 查找从给定起点到目的地的路线。 路线服务提供多个 API，规划两个地点之间最快、最短、环保或令人兴奋的路线。 此外，它还允许用户使用 Azure 广泛的历史交通数据库和预测任何一天任何时间的路线时间来规划路线。 有关详细信息，请参阅 [Get route directions](https://docs.microsoft.com/rest/api/maps/route/getroutedirections)（获取路线指示）。 应该**在 map load eventListener 中**添加以下所有功能，以确保它们在地图完全加载之后加载。

1. 通过在地图加载事件处理程序中添加以下 Javascript 代码来实例化服务客户端。

    ```JavaScript
    //If the service client hasn't already been created, create an instance.
    if (!client) {
        client = new atlas.service.Client(atlas.getSubscriptionKey());
    }
    ```

2. 添加以下代码块来构造路由查询字符串。
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

3. 若要获取路由，请向脚本中添加以下代码块。 它通过 [getRouteDirections](https://docs.microsoft.com/javascript/api/azure-maps-rest/services.route?view=azure-iot-typescript-latest#getroutedirections) 方法查询 Azure Maps 路由服务，然后使用 [getGeoJsonRoutes](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.geojson.geojsonroutedirectionsresponse?view=azure-iot-typescript-latest#getgeojsonroutes) 将响应解析为 GeoJSON 格式。 然后，它会将响应中的路线添加到数据源，后者会将其自动呈现在地图上。

    ```JavaScript
    //Execute the car route query then add the route to the map once a response is received.
    client.route.getRouteDirections(routeQuery).then(function (response) {
        // Parse the response into GeoJSON
        var geoJsonResponse = new atlas.service.geojson.GeoJsonRouteDirectionsResponse(response);

        //Add the route line to the data source.
        datasource.add(geoJsonResponse.getGeoJsonRoutes().features[0]);
    });
    ```

5. 保存“MapRoute.html”文件并刷新 web 浏览器。 要成功连接 Maps API，应看到类似于以下内容的地图。

    ![Azure 地图控件和路线服务](./media/tutorial-route-location/map-route.png)

## <a name="next-steps"></a>后续步骤

本教程介绍了如何：

> [!div class="checklist"]
> * 使用 Map Control API 创建新网页
> * 设置地址坐标
> * 在路线服务中查询兴趣点的方向

可以在此处访问本教程的代码示例：

> [使用 Azure Maps 查找路线](https://github.com/Azure-Samples/AzureMapsCodeSamples/blob/master/AzureMapsCodeSamples/Tutorials/route.html)

[实时查看此处的这个示例](https://azuremapscodesamples.azurewebsites.net/?sample=Route%20to%20a%20destination)

下一教程演示如何在有限制的情况下（例如出行模式或货物类型）创建路线查询，以及如何在同一地图上显示多条路线。

> [!div class="nextstepaction"]
> [查找不同出行模式的路线](./tutorial-prioritized-routes.md)
