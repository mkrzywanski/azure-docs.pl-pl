---
title: Plik dyrektywy include
description: Plik dyrektywy include
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.date: 07/28/2020
ms.topic: include
ms.custom: include file
ms.author: diberry
ms.openlocfilehash: db866da43310f5407ce4daae1cade2c7512b91ea
ms.sourcegitcommit: f353fe5acd9698aa31631f38dd32790d889b4dbb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/29/2020
ms.locfileid: "87369275"
---
Użyj biblioteki klienta przewidywania Language Understanding (LUIS) dla języka Python, aby:

* Pobierz prognozowanie według miejsca
* Pobierz prognozowanie według wersji

[Dokumentacja](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/index?view=azure-python)  |  referencyjna [Kod](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/cognitiveservices/azure-cognitiveservices-language-luis/azure/cognitiveservices/language/luis)  |  źródłowy biblioteki [Pakiet środowiska uruchomieniowego przewidywania (PyPi)](https://pypi.org/project/azure-cognitiveservices-language-luis/)  |  [Przykłady](https://github.com/Azure-Samples/cognitive-services-quickstart-code/tree/master/python/LUIS/python-sdk-authoring-prediction/prediction_quickstart.py)

## <a name="prerequisites"></a>Wymagania wstępne

* Language Understanding (LUIS) — konto portalu — [Utwórz je bezpłatnie](https://www.luis.ai)
* [Python 3.x](https://www.python.org/)
* Identyfikator aplikacji LUIS — Użyj publicznego identyfikatora aplikacji IoT `df67dcdb-c37d-46af-88e1-8b97951ca1c2` . Zapytanie użytkownika używane w kodzie szybkiego startu jest specyficzne dla tej aplikacji.

## <a name="setting-up"></a>Konfigurowanie

### <a name="get-your-language-understanding-luis-runtime-key"></a>Pobierz klucz środowiska uruchomieniowego Language Understanding (LUIS)

Pobierz swój [klucz środowiska uruchomieniowego](../luis-how-to-azure-subscription.md) , tworząc zasób środowiska uruchomieniowego Luis. Zachowaj klucz i punkt końcowy klucza dla kolejnego kroku.

### <a name="create-a-new-python-file"></a>Utwórz nowy plik w języku Python

Utwórz nowy plik w języku Python za pomocą preferowanego edytora lub środowiska IDE o nazwie `prediction_quickstart.py` .

### <a name="install-the-sdk"></a>Instalacja zestawu SDK

W katalogu aplikacji zainstaluj bibliotekę klienta przewidywania środowiska uruchomieniowego Language Understanding (LUIS) dla języka Python za pomocą następującego polecenia:

```python
python -m pip install azure-cognitiveservices-language-luis
```

## <a name="object-model"></a>Model obiektów

Klient predykcyjny w środowisku uruchomieniowym Language Understanding (LUIS) to obiekt [LUISRuntimeClient](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.runtime.luisruntimeclient?view=azure-python) , który jest uwierzytelniany na platformie Azure, który zawiera klucz zasobu.

Po utworzeniu klienta Użyj tego klienta, aby uzyskać dostęp do funkcji, w tym:

* Prognozowanie według [miejsca przejściowego lub produkcyjnego](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.runtime.operations.predictionoperations?view=azure-python#get-slot-prediction-app-id--slot-name--prediction-request--verbose-none--show-all-intents-none--log-none--custom-headers-none--raw-false----operation-config-)
* Prognozowanie według [wersji](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.runtime.operations.predictionoperations?view=azure-python#get-version-prediction-app-id--version-id--prediction-request--verbose-none--show-all-intents-none--log-none--custom-headers-none--raw-false----operation-config-)

## <a name="code-examples"></a>Przykłady kodu

Te fragmenty kodu pokazują, jak wykonać następujące czynności za pomocą biblioteki klienckiej środowiska uruchomieniowego Language Understanding (LUIS) dla języka Python:

* [Prognozowanie według miejsca](#get-prediction-from-runtime)

## <a name="add-the-dependencies"></a>Dodawanie zależności

W katalogu projektu Otwórz `prediction_quickstart.py` plik w preferowanym edytorze lub w środowisku IDE. Dodaj następujące zależności:

[!code-python[Dependency statements](~/cognitive-services-quickstart-code/python/LUIS/python-sdk-authoring-prediction/prediction_quickstart.py?name=Dependencies)]

## <a name="authenticate-the-client"></a>Uwierzytelnianie klienta

1. Utwórz zmienne dla własnych wymaganych informacji LUIS: klucz predykcyjny i punkt końcowy.

    [!code-python[Dependency statements](~/cognitive-services-quickstart-code/python/LUIS/python-sdk-authoring-prediction/prediction_quickstart.py?name=AuthorizationVariables)]

1. Utwórz zmienną dla identyfikatora aplikacji ustawionej na publiczną aplikację IoT **`df67dcdb-c37d-46af-88e1-8b97951ca1c2`** . Utwórz zmienną, aby ustawić `production` opublikowane gniazdo.

    [!code-python[Dependency statements](~/cognitive-services-quickstart-code/python/LUIS/python-sdk-authoring-prediction/prediction_quickstart.py?name=OtherVariables)]


1. Utwórz obiekt poświadczeń przy użyciu klucza i użyj go w punkcie końcowym, aby utworzyć obiekt [LUISRuntimeClientConfiguration](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.runtime.luisruntimeclientconfiguration?view=azure-python) .

    [!code-python[Dependency statements](~/cognitive-services-quickstart-code/python/LUIS/python-sdk-authoring-prediction/prediction_quickstart.py?name=Client)]

## <a name="get-prediction-from-runtime"></a>Pobierz prognozowanie z środowiska uruchomieniowego

Dodaj następującą metodę, aby utworzyć żądanie do środowiska uruchomieniowego przewidywania.

Wypowiedź użytkownika jest częścią obiektu [prediction_request](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.runtime.models.predictionrequest?view=azure-python) .

Metoda **[get_slot_prediction](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.runtime.operations.predictionoperations?view=azure-python#get-slot-prediction-app-id--slot-name--prediction-request--verbose-none--show-all-intents-none--log-none--custom-headers-none--raw-false----operation-config-)** wymaga kilku parametrów, takich jak identyfikator aplikacji, nazwa gniazda i obiekt żądania prognozowania w celu spełnienia żądania. Inne opcje, takie jak verbose, pokazują wszystkie intencje i dzienniki są opcjonalne. Żądanie zwraca obiekt [PredictionResponse](https://docs.microsoft.com/python/api/azure-cognitiveservices-language-luis/azure.cognitiveservices.language.luis.runtime.models.predictionresponse?view=azure-python) .

[!code-python[Dependency statements](~/cognitive-services-quickstart-code/python/LUIS/python-sdk-authoring-prediction/prediction_quickstart.py?name=predict)]

## <a name="main-code-for-the-prediction"></a>Kod główny do prognozowania

Użyj następującej metody Main, aby powiązać zmienne i metody w celu uzyskania prognozowania.

```python
predict(luisAppID, luisSlotName)
```
## <a name="run-the-application"></a>Uruchamianie aplikacji

Uruchom aplikację za pomocą `python prediction_quickstart.py` polecenia z katalogu aplikacji.

```console
python prediction_quickstart.py
```

W konsoli szybkiego startu są wyświetlane dane wyjściowe:

```console
Top intent: HomeAutomation.TurnOn
Sentiment: None
Intents:
        "HomeAutomation.TurnOn"
Entities: {'HomeAutomation.Operation': ['on']}
```

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Po wykonaniu prognoz Wyczyść prace z tego przewodnika Szybki Start, usuwając plik i jego podkatalogi.
