---
title: Simple — typ jednostki — LUIS
titleSuffix: Azure Cognitive Services
description: Prosta Jednostka opisuje pojedyncze koncepcje z kontekstu uczenia maszynowego. Dodaj listę fraz przy użyciu prostej jednostki w celu poprawienia wyników.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 09/29/2019
ms.author: diberry
ms.openlocfilehash: 1b5754be3c9941101a53f332841ace93caf9acdd
ms.sourcegitcommit: 50673ecc5bf8b443491b763b5f287dde046fdd31
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/20/2020
ms.locfileid: "83684554"
---
# <a name="simple-entity"></a>Prosta jednostka

Prosta jednostka to ogólna jednostka, która opisuje pojedyncze koncepcje i jest poznania z kontekstu uczenia maszynowego. Ponieważ proste jednostki są zazwyczaj nazwami, takimi jak nazwy firmowe, nazwy produktów lub inne kategorie nazw, Dodaj [listę fraz](luis-concept-feature.md) przy użyciu prostej jednostki, aby zwiększyć sygnał użytych nazw.

**Jednostka jest dobrym dopasowaniem w przypadku:**

* Dane nie są ciągle sformatowane, ale wskazują na to samo.

![Jednostka prosta](./media/luis-concept-entities/simple-entity.png)

## <a name="example-json"></a>Przykładowy kod JSON

`Bob Jones wants 3 meatball pho`

W poprzednim wypowiedź `Bob Jones` jest oznaczona jako prosta `Customer` jednostka.

Dane zwrócone z punktu końcowego zawierają nazwę jednostki, odnaleziony tekst z wypowiedź, lokalizację odnalezionego tekstu oraz wynik:

#### <a name="v2-prediction-endpoint-response"></a>[Odpowiedź punktu końcowego przewidywania wersji 2](#tab/V2)

```JSON
"entities": [
  {
  "entity": "bob jones",
  "type": "Customer",
  "startIndex": 0,
  "endIndex": 8,
  "score": 0.473899543
  }
]
```

#### <a name="v3-prediction-endpoint-response"></a>[Odpowiedź punktu końcowego przewidywania v3](#tab/V3)

Jest to kod JSON, jeśli `verbose=false` jest ustawiony w ciągu zapytania:

```json
"entities": {
    "Customer": [
        "Bob Jones"
    ]
}```

This is the JSON if `verbose=true` is set in the query string:

```json
"entities": {
    "Customer": [
        "Bob Jones"
    ],
    "$instance": {
        "Customer": [
            {
                "type": "Customer",
                "text": "Bob Jones",
                "startIndex": 0,
                "length": 9,
                "score": 0.9339134,
                "modelTypeId": 1,
                "modelType": "Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ]
    }
}
```

* * *

|Obiekt danych|Nazwa jednostki|Wartość|
|--|--|--|
|Jednostka prosta|`Customer`|`bob jones`|

## <a name="next-steps"></a>Następne kroki

> [!div class="nextstepaction"]
> [Poznaj składnię wzorca](reference-pattern-syntax.md)