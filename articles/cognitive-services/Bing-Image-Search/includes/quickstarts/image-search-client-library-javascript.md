---
title: wyszukiwanie obrazów Bing przewodniku szybki start dla biblioteki klienta JavaScript
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 03/04/2020
ms.author: aahi
ms.custom: devx-track-javascript
ms.openlocfilehash: 953648f5cf83d5ffd22683ba0ce02335a637f18a
ms.sourcegitcommit: 42107c62f721da8550621a4651b3ef6c68704cd3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/29/2020
ms.locfileid: "87407232"
---
Ten przewodnik Szybki Start umożliwia przeszukiwanie pierwszego obrazu przy użyciu biblioteki klienta wyszukiwanie obrazów Bing, która jest otoką dla interfejsu API i zawiera te same funkcje. Ta prosta aplikacja JavaScript wysyła zapytanie dotyczące wyszukania obrazu, analizuje odpowiedź w formacie JSON i wyświetla adres URL pierwszego zwróconego obrazu.

Kod źródłowy dla tego przykładu jest dostępny w usłudze [GitHub](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples/blob/master/Samples/imageSearch.js) wraz z dodatkową obsługą błędów i adnotacjami.

## <a name="prerequisites"></a>Wymagania wstępne

* [Zestaw Cognitive Services Image Search SDK dla środowiska Node.js](https://www.npmjs.com/package/@azure/cognitiveservices-imagesearch)
    * Zainstaluj przy użyciu polecenia `npm install @azure/cognitiveservices-imagesearch`
* Moduł [Node.js Azure Rest](https://www.npmjs.com/package/ms-rest-azure)
    * Zainstaluj przy użyciu polecenia `npm install ms-rest-azure`

[!INCLUDE [cognitive-services-bing-image-search-signup-requirements](~/includes/cognitive-services-bing-image-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Tworzenie i inicjowanie aplikacji

1. Utwórz nowy plik JavaScript w swoim ulubionym środowisku IDE lub edytorze i ustaw poziom ścisłości, protokół HTTPS oraz inne wymagania.

    ```javascript
    'use strict';
    const ImageSearchAPIClient = require('@azure/cognitiveservices-imagesearch');
    const CognitiveServicesCredentials = require('ms-rest-azure').CognitiveServicesCredentials;
    ```

2. W metodzie głównej projektu utwórz zmienne dla swojego ważnego klucza subskrypcji, wyników obrazów do zwrócenia przez usługę Bing i terminu wyszukiwania. Następnie utwórz wystąpienie klienta wyszukiwania obrazów za pomocą klucza.

    ```javascript
    //replace this value with your valid subscription key.
    let serviceKey = "ENTER YOUR KEY HERE";

    //the search term for the request
    let searchTerm = "canadian rockies";

    //instantiate the image search client
    let credentials = new CognitiveServicesCredentials(serviceKey);
    let imageSearchApiClient = new ImageSearchAPIClient(credentials);

    ```

## <a name="create-an-asynchronous-helper-function"></a>Tworzenie asynchronicznej funkcji pomocnika

1. Utwórz funkcję do asynchronicznego wywołania klienta, a następnie zwróć odpowiedź z usługi wyszukiwania obrazów Bing.
    ```javascript
    //a helper function to perform an async call to the Bing Image Search API
    const sendQuery = async () => {
        return await imageSearchApiClient.imagesOperations.search(searchTerm);
    };
    ```
   ## <a name="send-a-query-and-handle-the-response"></a>Wysyłanie zapytania i obsługa odpowiedzi

1. Wywołaj funkcję pomocnika i obsłuż jej element `promise`, aby przeanalizować wyniki obrazów zwrócone w odpowiedzi.

    Jeśli odpowiedź zawiera wyniki wyszukiwania, zapisz pierwszy wynik i wyświetl jego szczegóły, takie jak adres URL miniatury i oryginalny adres URL, wraz z całkowitą liczbą zwróconych obrazów.
    ```javascript
    sendQuery().then(imageResults => {
        if (imageResults == null) {
        console.log("No image results were found.");
        }
        else {
            console.log(`Total number of images returned: ${imageResults.value.length}`);
            let firstImageResult = imageResults.value[0];
            //display the details for the first image result. After running the application,
            //you can copy the resulting URLs from the console into your browser to view the image.
            console.log(`Total number of images found: ${imageResults.value.length}`);
            console.log(`Copy these URLs to view the first image returned:`);
            console.log(`First image thumbnail url: ${firstImageResult.thumbnailUrl}`);
            console.log(`First image content url: ${firstImageResult.contentUrl}`);
        }
      })
      .catch(err => console.error(err))
    ```

## <a name="next-steps"></a>Następne kroki

> [!div class="nextstepaction"]
> [Samouczek dotyczący jednostronicowej aplikacji wyszukiwania obrazów Bing](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/tutorial-bing-image-search-single-page-app)

## <a name="see-also"></a>Zobacz także

* [Czym jest funkcja wyszukiwania obrazów Bing?](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/overview)
* [Wypróbuj interaktywny pokaz online](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/)
* [Przykłady dla zestawu Azure Cognitive Services SDK w środowisku Node.js](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples)
* [Dokumentacja usług Azure Cognitive Services](https://docs.microsoft.com/azure/cognitive-services)
* [Dokumentacja interfejsu API wyszukiwania obrazów Bing](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-images-api-v7-reference)
