---
title: Dodawanie znacznika HTML do mapy | Mapy Microsoft Azure
description: W tym artykule opisano sposób dodawania znacznika HTML do mapy przy użyciu zestawu Microsoft Azure Web SDK mapy.
author: anastasia-ms
ms.author: v-stharr
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen, devx-track-javascript
ms.openlocfilehash: 9b7156bf5c266ccbba926a22a4afe46129ee3f1e
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/28/2020
ms.locfileid: "87286990"
---
# <a name="add-html-markers-to-the-map"></a>Dodawanie znaczników HTML do mapy

W tym artykule opisano sposób dodawania niestandardowego kodu HTML, takiego jak plik obrazu do mapy, jako znacznika HTML.

> [!NOTE]
> Znaczniki HTML nie łączą się ze źródłami danych. Zamiast tego informacje o pozycji są dodawane bezpośrednio do znacznika i znacznik jest dodawany do właściwości mapss, `markers` która jest [HtmlMarkerManager](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarkermanager?view=azure-iot-typescript-latest).

> [!IMPORTANT]
> W przeciwieństwie do większości warstw w Azure Maps formancie sieci Web, który używa WebGL do renderowania, znaczniki HTML używają tradycyjnych elementów DOM do renderowania. W związku z tym więcej znaczników HTML dodanych do strony, więcej elementów DOM. Wydajność może być gorsza po dodaniu kilku tysięcy znaczników HTML. W przypadku większych zestawów danych należy wziąć pod uwagę klastrowanie danych lub użycie symbolu lub warstwy bąbelkowej.

## <a name="add-an-html-marker"></a>Dodawanie znacznika HTML

Klasa [HtmlMarker](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarker?view=azure-iot-typescript-latest) ma styl domyślny. Znacznik można dostosować, ustawiając opcje koloru i tekstu znacznika. Domyślny styl klasy znaczników HTML jest szablon SVG, który ma `{color}` `{text}` symbol zastępczy i. Ustaw właściwości koloru i tekstu w opcjach znacznika HTML, aby szybko dostosowywać. 

Poniższy kod tworzy znacznik HTML i ustawia właściwość Color na wartość "DodgerBlue" i właściwość Text na "10". Do znacznika i `click` zdarzenia służy do przełączania widoczności okna podręcznego.

```javascript
//Create an HTML marker and add it to the map.
var marker = new atlas.HtmlMarker({
    color: 'DodgerBlue',
    text: '10',
    position: [0, 0],
    popup: new atlas.Popup({
        content: '<div style="padding:10px">Hello World</div>',
        pixelOffset: [0, -30]
    })
});

map.markers.add(marker);

//Add a click event to toggle the popup.
map.events.add('click',marker, () => {
    marker.togglePopup();
});
```

Poniżej znajduje się kompletny przykładowy kod wykonywany z powyższymi funkcjami.

<br/>

<iframe height='500' scrolling='no' title='Dodawanie znacznika HTML do mapy' src='//codepen.io/azuremaps/embed/MVoeVw/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zobacz pióro <a href='https://codepen.io/azuremaps/pen/MVoeVw/'>Dodaj znacznik HTML do mapy</a> według Azure Maps ( <a href='https://codepen.io/azuremaps'>@azuremaps</a> ) na <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="create-svg-templated-html-marker"></a>Tworzenie znacznika HTML z szablonem SVG

Domyślnym `htmlContent` znacznikiem HTML jest szablon SVG z folderami umieszczania `{color}` i `{text}` w nim. Można utworzyć niestandardowe ciągi SVG i dodać te same symbole zastępcze do SVG, takie jak Ustawianie `color` opcji i `text` znacznika aktualizowanie tych symboli zastępczych w formacie SVG.

<br/>

<iframe height='500' scrolling='no' title='Znacznik HTML z niestandardowym szablonem SVG' src='//codepen.io/azuremaps/embed/LXqMWx/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zobacz <a href='https://codepen.io/azuremaps/pen/LXqMWx/'>znacznik HTML pióra z niestandardowym szablonem SVG</a> , Azure Maps ( <a href='https://codepen.io/azuremaps'>@azuremaps</a> ) na <a href='https://codepen.io'>CodePen</a>.
</iframe>

> [!TIP]
> Zestaw SDK sieci Web Azure Maps udostępnia kilka szablonów obrazów SVG, które mogą być używane ze znacznikami HTML. Aby uzyskać więcej informacji, zobacz dokument [jak korzystać z szablonów obrazów](how-to-use-image-templates-web-sdk.md) .

## <a name="add-a-css-styled-html-marker"></a>Dodawanie znacznika HTML z stylem CSS

Jedną z zalet znaczników HTML jest to, że istnieje wiele doskonałych dostosowań, które można osiągnąć przy użyciu CSS. W tym przykładzie zawartość HtmlMarker składa się z kodu HTML i CSS, który tworzy animowany numer PIN, który spadnie w miejscu i pulsie.

<br/>

<iframe height='500' scrolling='no' title='Źródło danych HTML' src='//codepen.io/azuremaps/embed/qJVgMx/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zobacz <a href='https://codepen.io/azuremaps/pen/qJVgMx/'>Źródło danych HTML</a> pióra według Azure Maps ( <a href='https://codepen.io/azuremaps'>@azuremaps</a> ) na <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="draggable-html-markers"></a>Przeciągnięte znaczniki HTML

Ten przykład pokazuje, jak sprawić, aby można było przeciągnąć znacznik HTML. Znaczniki HTML obsługują `drag` `dragstart` zdarzenia, i `dragend` .

<br/>

<iframe height='500' scrolling='no' title='Przeciągany znacznik HTML' src='//codepen.io/azuremaps/embed/wQZoEV/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zobacz <a href='https://codepen.io/azuremaps/pen/wQZoEV/'>znacznik HTML z przeciągnięciem</a> piórem przez Azure Maps ( <a href='https://codepen.io/azuremaps'>@azuremaps</a> ) na <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="add-mouse-events-to-html-markers"></a>Dodawanie zdarzeń myszy do znaczników HTML

Te przykłady pokazują, jak dodać mysz i przeciągnąć zdarzenia do znacznika HTML.

<br/>

<iframe height='500' scrolling='no' title='Dodawanie zdarzeń myszy do znaczników HTML' src='//codepen.io/azuremaps/embed/RqOKRz/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zobacz pióro <a href='https://codepen.io/azuremaps/pen/RqOKRz/'>Dodawanie zdarzeń myszy do ZNACZNIKÓW HTML</a> przez Azure Maps ( <a href='https://codepen.io/azuremaps'>@azuremaps</a> ) na <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="next-steps"></a>Następne kroki

Dowiedz się więcej na temat klas i metod używanych w tym artykule:

> [!div class="nextstepaction"]
> [HtmlMarker](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarker?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [HtmlMarkerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarkeroptions?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [HtmlMarkerManager](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarkermanager?view=azure-iot-typescript-latest)

Aby uzyskać więcej przykładów kodu do dodania do usługi Maps, zobacz następujące artykuły:

> [!div class="nextstepaction"]
> [Jak używać szablonów obrazów](how-to-use-image-templates-web-sdk.md)

> [!div class="nextstepaction"]
> [Dodawanie warstwy symboli](./map-add-pin.md)

> [!div class="nextstepaction"]
> [Dodawanie warstwy bąbelkowej](./map-add-bubble-layer.md)
