---
title: Dodawanie warstwy wytłoczenia wielokąta do mapy | Mapy Microsoft Azure
description: Jak dodać warstwę wytłoczenia wielokąta do Microsoft Azure Maps Web SDK.
author: anastasia-ms
ms.author: v-stharr
ms.date: 10/08/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen, devx-track-javascript
ms.openlocfilehash: cd12242367c6f3b5b2c64df28f7f5e52cf1a4f97
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/28/2020
ms.locfileid: "87282878"
---
# <a name="add-a-polygon-extrusion-layer-to-the-map"></a>Dodaj warstwę wytłoczenia wielokąta do mapy

W tym artykule pokazano, jak za pomocą warstwy wytłoczenia wielokąta renderować obszary `Polygon` i `MultiPolygon` funkcje geometrie jako kształty wyciągnięcia. Zestaw SDK sieci Web Azure Maps obsługuje renderowanie okręgu geometrie zgodnie z definicją w [schemacie Extended GEOJSON](extend-geojson.md#circle). Te okręgi można przekształcać na wielokąty, gdy są renderowane na mapie. Wszystkie funkcje geometrie można łatwo aktualizować, gdy są opakowane w [Atlas. Shape](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape?view=azure-iot-typescript-latest) — Klasa.

## <a name="use-a-polygon-extrusion-layer"></a>Użyj warstwy wytłoczenia wielokątu

Połącz [warstwę wytłoczenia wielokąta](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.polygonextrusionlayer?view=azure-maps-typescript-latest) ze źródłem danych. Następnie załadowano ją na mapie. Warstwa wytłoczenia wielokąta renderuje obszary `Polygon` i `MultiPolygon` funkcje jako kształty wyciągnięcia. `height`Właściwości i `base` warstwy wytłoczenia wielokąta definiują odległość bazową od powierzchni i wysokości naciągnięcia kształtu w **licznikach**. Poniższy kod ilustruje sposób tworzenia wielokątów, dodawania go do źródła danych i renderowania przy użyciu klasy warstwy wytłoczenia wielokąt.

> [!Note]
> `base`Wartość zdefiniowana w warstwie wytłoczenia wielokąta powinna być mniejsza lub równa liczbie `height` .

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Wytłaczany Wielokąt" src="https://codepen.io/azuremaps/embed/wvvBpvE?height=265&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
Zobacz <a href='https://codepen.io/azuremaps/pen/wvvBpvE'>Wielokąt</a> wyciąganie z piórem Azure Maps ( <a href='https://codepen.io/azuremaps'>@azuremaps</a> ) na <a href='https://codepen.io'>CodePen</a>.</iframe>


## <a name="add-data-driven-polygons"></a>Dodawanie wielokątów opartych na danych

Mapę na mapie choropleth można renderować przy użyciu warstwy wytłoczenia wielokąta. Ustaw `height` właściwości i `fillColor` warstwy wytłoczenia na pomiar zmiennej statystycznej w `Polygon` `MultiPolygon` funkcji i geometrie. Poniższy przykładowy kod przedstawia wytłaczaną mapę na mapie choropleth U. S na podstawie pomiaru gęstości populacji według stanu.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Mapa na mapie choropleth wytłaczana" src="https://codepen.io/azuremaps/embed/eYYYNox?height=265&theme-id=0&default-tab=result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
Zobacz <a href='https://codepen.io/azuremaps/pen/eYYYNox'>mapę na mapie choropleth</a> , która została wyświetlona przez Azure Maps ( <a href='https://codepen.io/azuremaps'>@azuremaps</a> ) na <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="add-a-circle-to-the-map"></a>Dodaj okrąg do mapy

Azure Maps używa rozszerzonej wersji schematu GEOJSON, który zawiera definicję okręgów, jak pokazano [tutaj](https://docs.microsoft.com/azure/azure-maps/extend-geojson#circle). Wykreślone koło może być renderowane na mapie przez utworzenie `point` funkcji z `subType` właściwością `Circle` i `Radius` właściwością numerowaną reprezentującą promień w **licznikach**. Na przykład:

```Javascript
{
    "type": "Feature",
    "geometry": {
        "type": "Point",
        "coordinates": [-105.203135, 39.664087]
    },
    "properties": {
        "subType": "Circle",
        "radius": 1000
    }
} 
```

Zestaw SDK sieci Web Azure Maps konwertuje te `Point` funkcje na `Polygon` funkcje pod okapem. Te `Point` funkcje mogą być renderowane na mapie przy użyciu warstwy wytłoczenia wielokątów, jak pokazano w poniższym przykładzie kodu.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Wielokąt drona miejsca" src="https://codepen.io/azuremaps/embed/zYYYrxo?height=265&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
Zapoznaj się z <a href='https://codepen.io/azuremaps/pen/zYYYrxo'>wielokąta dronaego miejsca w miejscu</a> dla pióra według Azure Maps ( <a href='https://codepen.io/azuremaps'>@azuremaps</a> ) na <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="customize-a-polygon-extrusion-layer"></a>Dostosuj warstwę wytłoczenia wielokąta

Warstwa wytłoczenia wielokąta ma kilka opcji stylów. Oto narzędzie do wypróbowania.

<br/>

<iframe height='700' scrolling='no' title='PoogBRJ' src='//codepen.io/azuremaps/embed/PoogBRJ/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zobacz pióro <a href='https://codepen.io/azuremaps/pen/PoogBRJ/'>PoogBRJ</a> według Azure Maps ( <a href='https://codepen.io/azuremaps'>@azuremaps</a> ) na <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="next-steps"></a>Następne kroki

Dowiedz się więcej na temat klas i metod używanych w tym artykule:

> [!div class="nextstepaction"]
> [Wielokąt](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.polygon?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [warstwa wytłoczenia wielokątu](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.polygonextrusionlayer?view=azure-maps-typescript-latest)

Dodatkowe zasoby:

> [!div class="nextstepaction"]
> [Rozszerzenie specyfikacji GEOJSON Azure Maps](extend-geojson.md#circle)
