---
title: Prebudowana jednostka w wersji 2 — LUIS
titleSuffix: Azure Cognitive Services
description: Ten artykuł zawiera wstępnie skompilowane informacje o jednostkach geographyV2 w Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 10/04/2019
ms.author: diberry
ms.openlocfilehash: b2b2b0781abce59628660b669f43110bf91b15e6
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/29/2020
ms.locfileid: "78273493"
---
# <a name="geographyv2-prebuilt-entity-for-a-luis-app"></a>GeographyV2 prekompilowaną jednostkę dla aplikacji LUIS
Wstępnie utworzona jednostka geographyV2 wykrywa miejsca. Ponieważ ta jednostka jest już przeszkolone, nie trzeba dodawać przykładowej wyrażenia długości zawierającej GeographyV2 do intencji aplikacji. Jednostka GeographyV2 jest obsługiwana w [kulturze](luis-reference-prebuilt-entities.md)angielskiej.

## <a name="subtypes"></a>Podtypów
Lokalizacje geograficzne mają Podtyp:

|Podtyp|Przeznaczenie|
|--|--|
|`poi`|punkt orientacyjny|
|`city`|Nazwa miasta|
|`countryRegion`|Nazwa kraju lub regionu|
|`continent`|Nazwa kontynentu|
|`state`|Nazwa województwa|


## <a name="resolution-for-geographyv2-entity"></a>Rozwiązanie dla jednostki GeographyV2

Następujące obiekty jednostki są zwracane dla zapytania:

`Carol is visiting the sphinx in gizah egypt in africa before heading to texas.`

#### <a name="v3-response"></a>[Odpowiedź v3](#tab/V3)

Poniższy kod JSON jest z `verbose` parametrem ustawionym `false`na:

```json
"entities": {
    "geographyV2": [
        {
            "value": "the sphinx",
            "type": "poi"
        },
        {
            "value": "gizah",
            "type": "city"
        },
        {
            "value": "egypt",
            "type": "countryRegion"
        },
        {
            "value": "africa",
            "type": "continent"
        },
        {
            "value": "texas",
            "type": "state"
        }
    ]
}
```

W poprzednim pliku JSON `poi` jest skrótem dla **punktu orientacyjnego**.

#### <a name="v3-verbose-response"></a>[Pełna odpowiedź w wersji 3](#tab/V3-verbose)

Poniższy kod JSON jest z `verbose` parametrem ustawionym `true`na:

```json
"entities": {
    "geographyV2": [
        {
            "value": "the sphinx",
            "type": "poi"
        },
        {
            "value": "gizah",
            "type": "city"
        },
        {
            "value": "egypt",
            "type": "countryRegion"
        },
        {
            "value": "africa",
            "type": "continent"
        },
        {
            "value": "texas",
            "type": "state"
        }
    ],
    "$instance": {
        "geographyV2": [
            {
                "type": "builtin.geographyV2.poi",
                "text": "the sphinx",
                "startIndex": 18,
                "length": 10,
                "modelTypeId": 2,
                "modelType": "Prebuilt Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            },
            {
                "type": "builtin.geographyV2.city",
                "text": "gizah",
                "startIndex": 32,
                "length": 5,
                "modelTypeId": 2,
                "modelType": "Prebuilt Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            },
            {
                "type": "builtin.geographyV2.countryRegion",
                "text": "egypt",
                "startIndex": 38,
                "length": 5,
                "modelTypeId": 2,
                "modelType": "Prebuilt Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            },
            {
                "type": "builtin.geographyV2.continent",
                "text": "africa",
                "startIndex": 47,
                "length": 6,
                "modelTypeId": 2,
                "modelType": "Prebuilt Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            },
            {
                "type": "builtin.geographyV2.state",
                "text": "texas",
                "startIndex": 72,
                "length": 5,
                "modelTypeId": 2,
                "modelType": "Prebuilt Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ]
    }
}
```
#### <a name="v2-response"></a>[Odpowiedź w wersji 2](#tab/V2)

Poniższy przykład pokazuje rozdzielczość jednostki **wbudowanej. geographyV2** .

```json
"entities": [
    {
        "entity": "the sphinx",
        "type": "builtin.geographyV2.poi",
        "startIndex": 18,
        "endIndex": 27
    },
    {
        "entity": "gizah",
        "type": "builtin.geographyV2.city",
        "startIndex": 32,
        "endIndex": 36
    },
    {
        "entity": "egypt",
        "type": "builtin.geographyV2.countryRegion",
        "startIndex": 38,
        "endIndex": 42
    },
    {
        "entity": "africa",
        "type": "builtin.geographyV2.continent",
        "startIndex": 47,
        "endIndex": 52
    },
    {
        "entity": "texas",
        "type": "builtin.geographyV2.state",
        "startIndex": 72,
        "endIndex": 76
    },
    {
        "entity": "carol",
        "type": "builtin.personName",
        "startIndex": 0,
        "endIndex": 4
    }
]
```
* * *

## <a name="next-steps"></a>Następne kroki

Dowiedz się więcej o [punkcie końcowym przewidywania v3](luis-migration-api-v3.md).

Dowiedz się więcej o jednostkach [poczty e-mail](luis-reference-prebuilt-email.md), [liczb](luis-reference-prebuilt-number.md)i [porządkowej](luis-reference-prebuilt-ordinal.md) .
