---
title: Kategoryzacja obrazu — przetwarzanie obrazów
titleSuffix: Azure Cognitive Services
description: Poznaj koncepcje związane z funkcją kategoryzacji obrazu interfejs API przetwarzania obrazów.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 04/17/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 477349f1addf71a30e8ecb179266d8eac5510887
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/29/2020
ms.locfileid: "80244754"
---
# <a name="categorize-images-by-subject-matter"></a>Klasyfikowanie obrazów według tematu

Oprócz tagów i opisu, przetwarzanie obrazów zwraca Kategorie oparte na taksonomii wykrytych w obrazie. W przeciwieństwie do tagów, kategorie są zorganizowane w hierarchii Hereditary nadrzędny/podrzędny i są mniejsze z nich (86, w przeciwieństwie do tysięcy tagów). Wszystkie nazwy kategorii są w języku angielskim. Kategoryzacja może być wykonywana przez siebie lub obok nowszej modelu znaczników.

## <a name="the-86-category-concept"></a>Koncepcja 86 kategorii

W przypadku komputerów można w szerokim stopniu klasyfikować obraz przy użyciu listy kategorii 86 na poniższym diagramie. Aby uzyskać informacje dotyczące pełnej taksonomii w formacie tekstowym, zobacz temat [Taksonomia kategorii](category-taxonomy.md).

![Pogrupowane listy wszystkich kategorii w taksonomii kategorii](./Images/analyze_categories-v2.png)

## <a name="image-categorization-examples"></a>Przykłady kategoryzacji obrazów

Poniższa odpowiedź JSON ilustruje, co przetwarzanie obrazów zwracać podczas kategoryzowania przykładowego obrazu na podstawie jego funkcji wizualizacji.

![Kobieta na dachu budynku apartamentu](./Images/woman_roof.png)

```json
{
    "categories": [
        {
            "name": "people_",
            "score": 0.81640625
        }
    ],
    "requestId": "bae7f76a-1cc7-4479-8d29-48a694974705",
    "metadata": {
        "height": 200,
        "width": 300,
        "format": "Jpeg"
    }
}
```

W poniższej tabeli przedstawiono typowy zestaw obrazów oraz kategorię zwracaną przez przetwarzanie obrazów dla każdego obrazu.

| Obraz | Kategoria |
|-------|----------|
| ![Cztery osoby powodowane jako rodzina](./Images/family_photo.png) | people_group |
| ![Puppy siedzący w polu trawy](./Images/cute_dog.png) | animal_dog |
| ![Osoba stojąca na skałie górskim o zachodzie słońca](./Images/mountain_vista.png) | outdoor_mountain |
| ![Stos ról chleba w tabeli](./Images/bread.png) | food_bread |

## <a name="use-the-api"></a>Używanie interfejsu API

Funkcja kategoryzacji jest częścią interfejsu API [analizowania obrazu](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) . Ten interfejs API można wywołać za pomocą natywnego zestawu SDK lub wywołań REST. Uwzględnij `Categories` w parametrze zapytania **visualFeatures** . Po otrzymaniu pełnej odpowiedzi JSON należy po prostu przeanalizować ciąg dla zawartości `"categories"` sekcji.

* [Szybki Start: przetwarzanie obrazów zestawu .NET SDK](./quickstarts-sdk/client-library.md?pivots=programming-language-csharp)
* [Szybki Start: analizowanie obrazu (interfejs API REST)](./quickstarts/csharp-analyze.md)

## <a name="next-steps"></a>Następne kroki

Poznaj powiązane koncepcje [tagowania obrazów](concept-tagging-images.md) i [opisywania obrazów](concept-describing-images.md).
