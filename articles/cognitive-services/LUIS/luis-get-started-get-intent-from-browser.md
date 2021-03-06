---
title: 'Szybki Start: zapytanie o prognozowanie za pomocą przeglądarki LUIS'
description: W tym przewodniku szybki start Użyj dostępnej publicznej aplikacji LUIS, aby określić zamiar użytkownika w przeglądarce.
ms.topic: quickstart
ms.date: 04/21/2020
ms.openlocfilehash: 5ba86882ebf3cb538ad6b865382342fcbd43d27c
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/28/2020
ms.locfileid: "81769982"
---
# <a name="quickstart-query-prediction-runtime-with-user-text"></a>Szybki Start: badanie prognoz środowiska uruchomieniowego z tekstem użytkownika

Aby zrozumieć, co zwraca punkt końcowy przewidywania usługi LUIS, wyświetl wynik przewidywania w przeglądarce internetowej.

## <a name="prerequisites"></a>Wymagania wstępne

Aby można było wykonywać zapytania dotyczące aplikacji publicznej, potrzebne są:

* Informacje o zasobie Language Understanding (LUIS):
    * **Klucz predykcyjny** , który można uzyskać z [portalu Luis](https://www.luis.ai/). Jeśli nie masz jeszcze subskrypcji, aby utworzyć klucz, możesz zarejestrować się w celu uzyskania [bezpłatnego konta](https://azure.microsoft.com/free/).
    * **Poddomena punktu końcowego przewidywania** — poddomena jest również **nazwą** zasobu Luis.
* Identyfikator aplikacji LUIS — Użyj publicznego identyfikatora aplikacji IoT `df67dcdb-c37d-46af-88e1-8b97951ca1c2`. Zapytanie użytkownika używane w kodzie szybkiego startu jest specyficzne dla tej aplikacji.

## <a name="use-the-browser-to-see-predictions"></a>Używanie przeglądarki do wyświetlania prognoz

1. Otwórz przeglądarkę internetową.
1. Użyj pełnych adresów URL poniżej, `YOUR-KEY` zastępując je własnym kluczem przewidywania Luis. Żądania ODBIERAją żądania i obejmują autoryzację z kluczem przewidywania LUIS jako parametr ciągu zapytania.

    #### <a name="v3-prediction-request"></a>[Żądanie prognozowania v3](#tab/V3-1-1)


    Format adresu URL v3 dla żądania **Get** Endpoint (według gniazd) to:

    `
    https://YOUR-LUIS-ENDPOINT-SUBDOMAIN.api.cognitive.microsoft.com/luis/prediction/v3.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2/slots/production/predict?query=turn on all lights&subscription-key=YOUR-LUIS-PREDICTION-KEY
    `

    #### <a name="v2-prediction-request"></a>[Żądanie przewidywania w wersji 2](#tab/V2-1-2)

    Format adresu URL w wersji 2 dla żądania **Get** Endpoint:

    `
    https://YOUR-LUIS-ENDPOINT-SUBDOMAIN.api.cognitive.microsoft.com/luis/v2.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2?subscription-key=YOUR-LUIS-PREDICTION-KEY&q=turn on all lights
    `

1. Wklej adres URL w oknie przeglądarki, a następnie naciśnij klawisz Enter. W przeglądarce zostanie wyświetlony wynik w formacie JSON, który wskazuje, że usługa LUIS wykryła intencję `HomeAutomation.TurnOn` jako główną intencję oraz jednostkę `HomeAutomation.Operation` o wartości `on`.

    #### <a name="v3-prediction-response"></a>[Odpowiedź przewidywania v3](#tab/V3-2-1)

    ```JSON
    {
        "query": "turn on all lights",
        "prediction": {
            "topIntent": "HomeAutomation.TurnOn",
            "intents": {
                "HomeAutomation.TurnOn": {
                    "score": 0.5375382
                }
            },
            "entities": {
                "HomeAutomation.Operation": [
                    "on"
                ]
            }
        }
    }
    ```

    #### <a name="v2-prediction-response"></a>[Odpowiedź przewidywania w wersji 2](#tab/V2-2-2)

    ```json
    {
        "query": "turn on all lights",
        "topScoringIntent": {
            "intent": "HomeAutomation.TurnOn",
            "score": 0.5375382
        },
        "entities": [
            {
                "entity": "on",
                "type": "HomeAutomation.Operation",
                "startIndex": 5,
                "endIndex": 6,
                "score": 0.724984169
            }
        ]
    }
    ```

    * * *

1. Aby wyświetlić wszystkie intencje, Dodaj odpowiedni parametr ciągu zapytania.

    #### <a name="v3-prediction-endpoint"></a>[Punkt końcowy przewidywania v3](#tab/V3-3-1)

    Dodaj `show-all-intents=true` na końcu ciągu QueryString, aby **pokazać wszystkie intencje**:

    `
    https://YOUR-LUIS-ENDPOINT-SUBDOMAIN.api.cognitive.microsoft.com/luis/predict/v3.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2/slots/production/predict?query=turn on all lights&subscription-key=YOUR-LUIS-PREDICTION-KEY&show-all-intents=true
    `

    ```JSON
    {
        "query": "turn off the living room light",
        "prediction": {
            "topIntent": "HomeAutomation.TurnOn",
            "intents": {
                "HomeAutomation.TurnOn": {
                    "score": 0.5375382
                },
                "None": {
                    "score": 0.08687421
                },
                "HomeAutomation.TurnOff": {
                    "score": 0.0207554
                }
            },
            "entities": {
                "HomeAutomation.Operation": [
                    "on"
                ]
            }
        }
    }
    ```

    #### <a name="v2-prediction-endpoint"></a>[Punkt końcowy przewidywania wersji 2](#tab/V2)

    Dodaj `verbose=true` na końcu ciągu QueryString, aby **pokazać wszystkie intencje**:

    `
    https://YOUR-LUIS-ENDPOINT-SUBDOMAIN.api.cognitive.microsoft.com/luis/v2.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2?q=turn on all lights&subscription-key=YOUR-LUIS-PREDICTION-KEY&verbose=true
    `

    ```json
    {
        "query": "turn on all lights",
        "topScoringIntent": {
            "intent": "HomeAutomation.TurnOn",
            "score": 0.5375382
        },
        "intents": [
            {
                "intent": "HomeAutomation.TurnOn",
                "score": 0.5375382
            },
            {
                "intent": "None",
                "score": 0.08687421
            },
            {
                "intent": "HomeAutomation.TurnOff",
                "score": 0.0207554
            }
        ],
        "entities": [
            {
                "entity": "on",
                "type": "HomeAutomation.Operation",
                "startIndex": 5,
                "endIndex": 6,
                "score": 0.724984169
            }
        ]
    }
    ```

## <a name="next-steps"></a>Następne kroki

Dowiedz się więcej o usługach:
* [Punkt końcowy przewidywania v3](luis-migration-api-v3.md)
* [Niestandardowe poddomeny](../cognitive-services-custom-subdomains.md)

> [!div class="nextstepaction"]
> [Tworzenie aplikacji w portalu LUIS](get-started-portal-build-app.md)
