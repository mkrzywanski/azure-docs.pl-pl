---
title: Rozszerzony geometrie GEOJSON | Mapy Microsoft Azure
description: W tym artykule dowiesz się, jak mapy Microsoft Azure rozszerzają specyfikację GEOJSON do reprezentowania niektórych geometrie.
author: sataneja
ms.author: sataneja
ms.date: 05/17/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.openlocfilehash: 98db10f0fc7a417f39d4bb00e77af6bdea034a03
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "84687501"
---
# <a name="extended-geojson-geometries"></a>Rozszerzony geometrie GEOJSON

Azure Maps zawiera listę zaawansowanych interfejsów API do wyszukiwania w funkcjach geograficznych i. Te interfejsy API są zgodne ze standardową [specyfikacją GEOJSON][1] reprezentującą funkcje geograficzne.  

[Specyfikacja GEOJSON][1] obsługuje tylko następujące geometrie:

* GeometryCollection
* LineString
* MultiLineString
* Usług
* MultiPolygon
* Moment
* Wielokąt

Niektóre Azure Maps interfejsy API akceptują geometrie, które nie są częścią [specyfikacji GEOJSON][1]. Na przykład [Wyszukiwanie w](https://docs.microsoft.com/rest/api/maps/search/postsearchinsidegeometry) interfejsie API geometry akceptuje okrąg i wielokąty.

Ten artykuł zawiera szczegółowy opis sposobu, w jaki Azure Maps rozszerza [specyfikację GEOJSON][1] , aby reprezentować niektóre geometrie.

## <a name="circle"></a>Okrąg

`Circle`Geometria nie jest obsługiwana przez [specyfikację GEOJSON][1]. Używamy `GeoJSON Point Feature` obiektu do reprezentowania okręgu.

`Circle`Geometria reprezentowana przy użyciu `GeoJSON Feature` obiektu __musi__ zawierać następujące współrzędne i właściwości:

- Wyśrodkuj

    Środek okręgu jest reprezentowany przy użyciu `GeoJSON Point` obiektu.

- Radius

    Okrąg `radius` jest przedstawiany przy użyciu `GeoJSON Feature` właściwości. Wartość promienia jest w _metrach_ i musi być typu `double` .

- SubType

    Geometria okręgu musi również zawierać `subType` Właściwość. Ta właściwość musi być częścią `GeoJSON Feature` właściwości, a jej wartością powinna być _koło_

#### <a name="example"></a>Przykład

Poniżej przedstawiono sposób reprezentowania okręgu przy użyciu `GeoJSON Feature` obiektu. Wyśrodkuj okrąg na szerokości geograficznej: 47,639754 i długości geograficznej:-122,126986 i przypisz mu promień równy 100 mierników:

```json            
{
    "type": "Feature",
    "geometry": {
        "type": "Point",
        "coordinates": [-122.126986, 47.639754]
    },
    "properties": {
        "subType": "Circle",
        "radius": 100
    }
}          
```

## <a name="rectangle"></a>Prostokąt

`Rectangle`Geometria nie jest obsługiwana przez [specyfikację GEOJSON][1]. Używamy `GeoJSON Polygon Feature` obiektu do reprezentowania prostokąta. Rozszerzenie prostokąta jest używane głównie przez moduł narzędzi do rysowania zestawu SDK sieci Web.

`Rectangle`Geometria reprezentowana przy użyciu `GeoJSON Polygon Feature` obiektu __musi__ zawierać następujące współrzędne i właściwości:

- Rogi

    Rogi prostokąta są reprezentowane za pomocą współrzędnych `GeoJSON Polygon` obiektu. Należy mieć pięć współrzędnych, po jednym dla każdego rogu. I, piąta Współrzędna, która jest taka sama jak pierwsza Współrzędna, w celu zamknięcia pierścienia wielokąta. Zakłada się, że te współrzędne są wyrównane, a deweloper może je obrócić.

- SubType

    Geometria prostokąta musi również zawierać `subType` Właściwość. Ta właściwość musi być częścią `GeoJSON Feature` właściwości, a jej wartością powinna być _prostokąt_

### <a name="example"></a>Przykład

```json
{
    "type": "Feature",
    "geometry": {
        "type": "Polygon",
        "coordinates": [[[5,25],[14,25],[14,29],[5,29],[5,25]]]
    },
    "properties": {
        "subType": "Rectangle"
    }
}

```
## <a name="next-steps"></a>Następne kroki

Dowiedz się więcej o danych GEOJSON w Azure Maps:

> [!div class="nextstepaction"]
> [Format geonotacji GEOJSON](geofence-geojson.md)

Zapoznaj się ze słownikiem typowych warunków technicznych związanych z aplikacjami do analizy Azure Maps i lokalizacji:

> [!div class="nextstepaction"]
> [Słownik usługi Azure Maps](glossary.md)

[1]: https://tools.ietf.org/html/rfc7946
