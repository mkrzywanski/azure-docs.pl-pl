---
title: 'Szybki Start: sprawdzanie pisowni za pomocą interfejsu API REST i Node.js — sprawdzanie pisowni Bing'
titleSuffix: Azure Cognitive Services
description: Rozpocznij pracę z interfejsem API REST sprawdzanie pisowni Bing, aby sprawdzić pisownię i gramatykę w tym przewodniku Szybki Start.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-spell-check
ms.topic: quickstart
ms.date: 05/21/2020
ms.author: aahi
ms.custom: devx-track-javascript
ms.openlocfilehash: aaaa571928556a6972d3136ef4cacaa3bd4cb798
ms.sourcegitcommit: 42107c62f721da8550621a4651b3ef6c68704cd3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/29/2020
ms.locfileid: "87405011"
---
# <a name="quickstart-check-spelling-with-the-bing-spell-check-rest-api-and-nodejs"></a>Szybki Start: sprawdzanie pisowni przy użyciu interfejsu API REST sprawdzanie pisowni Bing i Node.js

Użyj tego przewodnika Szybki start, aby wykonać pierwsze wywołanie interfejsu API REST sprawdzania pisowni Bing. Ta prosta aplikacja JavaScript wysyła żądanie do interfejsu API i zwraca listę sugerowanych poprawek. 

Mimo że aplikacja jest zapisywana w języku JavaScript, interfejs API jest usługą sieci Web RESTful zgodną z większością języków programowania. Kod źródłowy tej aplikacji jest dostępny w usłudze [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/nodejs/Search/BingSpellCheckv7.js).

## <a name="prerequisites"></a>Wymagania wstępne

* Środowisko [Node.js 6](https://nodejs.org/en/download/) lub nowsze.

[!INCLUDE [cognitive-services-bing-spell-check-signup-requirements](../../../../includes/cognitive-services-bing-spell-check-signup-requirements.md)]


## <a name="create-and-initialize-a-project"></a>Tworzenie i inicjowanie projektu

1. Utwórz nowy plik JavaScript w ulubionym środowisku IDE lub edytorze. Ustawianie rygorystyczności i wymaganie `https` . Następnie utwórz zmienne dla hosta i ścieżki punktu końcowego interfejsu API oraz klucza subskrypcji. Możesz użyć globalnego punktu końcowego w poniższym kodzie lub użyć punktu końcowego [niestandardowej domeny](../../../cognitive-services/cognitive-services-custom-subdomains.md) podrzędnej wyświetlanego w Azure Portal dla zasobu.

    ```javascript
    'use strict';
    let https = require ('https');

    let host = 'api.cognitive.microsoft.com';
    let path = '/bing/v7.0/spellcheck';
    let key = '<ENTER-KEY-HERE>';
    ```

2. Utwórz zmienne dla parametrów wyszukiwania i tekst, który chcesz sprawdzić: 

   1. Przypisz kod rynkowy do `mkt` parametru `=` operatorem. Kod rynkowy to kod kraju/regionu, z którego pochodzi żądanie. 

   1. Dodaj `mode` parametr z `&` operatorem, a następnie przypisz tryb sprawdzania pisowni. Trybem może być albo `proof` (przechwycenie większości błędów pisowni/gramatyki) lub `spell` (przechwycenie większości błędów pisowni, ale nie tyle błędów gramatycznych).

    ```javascript
    let mkt = "en-US";
    let mode = "proof";
    let text = "Hollo, wrld!";
    let query_string = "?mkt=" + mkt + "&mode=" + mode;
    ```

## <a name="create-the-request-parameters"></a>Tworzenie parametrów żądania

Utwórz parametry żądania przez utworzenie nowego obiektu przy użyciu metody `POST`. Dodaj ścieżkę utworzoną przez połączenie ścieżki do punktu końcowego i ciągu zapytania. Następnie Dodaj swój klucz subskrypcji do `Ocp-Apim-Subscription-Key` nagłówka.

```javascript
let request_params = {
   method : 'POST',
   hostname : host,
   path : path + query_string,
   headers : {
   'Content-Type' : 'application/x-www-form-urlencoded',
   'Content-Length' : text.length + 5,
      'Ocp-Apim-Subscription-Key' : key,
   }
};
```

## <a name="create-a-response-handler"></a>Tworzenie procedury obsługi odpowiedzi

Utwórz funkcję o nazwie `response_handler`, która będzie przyjmować odpowiedź JSON z interfejsu API i wyświetlać ją. Utwórz zmienną na potrzeby treści odpowiedzi. Dołącz odpowiedź w przypadku `data` otrzymania flagi przy użyciu `response.on()` . Po `end` otrzymaniu flagi Wydrukuj treść JSON w konsoli programu.

```javascript
let response_handler = function (response) {
    let body = '';
    response.on ('data', function (d) {
        body += d;
    });
    response.on ('end', function () {
        let body_ = JSON.parse (body);
        console.log (body_);
    });
    response.on ('error', function (e) {
        console.log ('Error: ' + e.message);
    });
};
```

## <a name="send-the-request"></a>Wysyłanie żądania

Wywołaj interfejs API przy użyciu `https.request()` parametrów żądania i obsługi odpowiedzi. Napisz tekst do interfejsu API, a następnie Zakończ żądanie.

```javascript
let req = https.request (request_params, response_handler);
req.write ("text=" + text);
req.end ();
```


## <a name="run-the-application"></a>Uruchamianie aplikacji

1. Skompiluj i Uruchom projekt.

1. Jeśli używasz wiersza polecenia, użyj następującego polecenia, aby skompilować i uruchomić aplikację:

   ```bash
   node <FILE_NAME>.js
   ```


## <a name="example-json-response"></a>Przykładowa odpowiedź JSON

Po pomyślnym przetworzeniu żądania zostanie zwrócona odpowiedź w formacie JSON, jak pokazano w następującym przykładzie:

```json
{
   "_type": "SpellCheck",
   "flaggedTokens": [
      {
         "offset": 0,
         "token": "Hollo",
         "type": "UnknownToken",
         "suggestions": [
            {
               "suggestion": "Hello",
               "score": 0.9115257530801
            },
            {
               "suggestion": "Hollow",
               "score": 0.858039839213461
            },
            {
               "suggestion": "Hallo",
               "score": 0.597385084464481
            }
         ]
      },
      {
         "offset": 7,
         "token": "wrld",
         "type": "UnknownToken",
         "suggestions": [
            {
               "suggestion": "world",
               "score": 0.9115257530801
            }
         ]
      }
   ]
}
```

## <a name="next-steps"></a>Następne kroki

> [!div class="nextstepaction"]
> [Tworzenie jednostronicowej aplikacji internetowej](../tutorials/spellcheck.md)

- [Czym jest interfejs API sprawdzania pisowni Bing?](../overview.md)
- [Dokumentacja wersji 7 interfejsu API sprawdzanie pisowni Bing](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-spell-check-api-v7-reference)
