---
title: 'Szybki Start: wywoływanie punktu końcowego wyszukiwanie niestandardowe Bing przy użyciu Node.js | Microsoft Docs'
titleSuffix: Azure Cognitive Services
description: Skorzystaj z tego przewodnika Szybki Start, aby rozpocząć żądanie wyników wyszukiwania z wystąpienia wyszukiwanie niestandardowe Bing przy użyciu Node.js.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: quickstart
ms.date: 05/08/2020
ms.author: aahi
ms.custom: devx-track-javascript
ms.openlocfilehash: dcfb5ed0b7a7fe2ec004ef5ceebf9ee51112c67c
ms.sourcegitcommit: 42107c62f721da8550621a4651b3ef6c68704cd3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/29/2020
ms.locfileid: "87406456"
---
# <a name="quickstart-call-your-bing-custom-search-endpoint-using-nodejs"></a>Szybki Start: wywoływanie punktu końcowego wyszukiwanie niestandardowe Bing przy użyciu Node.js

Skorzystaj z tego przewodnika Szybki Start, aby dowiedzieć się, jak żądać wyników wyszukiwania z wystąpienia wyszukiwanie niestandardowe Bing. Mimo że aplikacja jest zapisywana w języku JavaScript, interfejs API wyszukiwania niestandardowego Bing jest usługą sieci Web RESTful zgodną z większością języków programowania. Kod źródłowy dla tego przykładu jest dostępny w witrynie [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/nodejs/Search/BingCustomSearchv7.js).

## <a name="prerequisites"></a>Wymagania wstępne

- Wystąpienie wyszukiwania niestandardowego Bing. Aby uzyskać więcej informacji, zobacz [Szybki Start: Tworzenie pierwszego wystąpienia wyszukiwanie niestandardowe Bing](quick-start.md).

- [Node.js środowiska uruchomieniowego JavaScript](https://www.nodejs.org/).

- [Biblioteka żądań JavaScript](https://github.com/request/request).

[!INCLUDE [cognitive-services-bing-custom-search-prerequisites](../../../includes/cognitive-services-bing-custom-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Tworzenie i inicjowanie aplikacji

- Utwórz nowy plik JavaScript w swoim ulubionym środowisku IDE lub edytorze i dodaj instrukcję `require()` dla biblioteki żądań. Utwórz zmienne dla klucza subskrypcji, niestandardowego identyfikatora konfiguracji i wyszukiwanego terminu.

    ```javascript
    var request = require("request");
    
    var subscriptionKey = 'YOUR-SUBSCRIPTION-KEY';
    var customConfigId = 'YOUR-CUSTOM-CONFIG-ID';
    var searchTerm = 'microsoft';
    ```

## <a name="send-and-receive-a-search-request"></a>Wysyłanie i odbieranie żądania wyszukiwania 

1. Utwórz zmienną do przechowywania informacji wysyłanych w żądaniu. Utwórz adres URL żądania przez dołączenie terminu wyszukiwania do `q=` parametru zapytania i NIESTANDARDOWEGO identyfikatora konfiguracji wystąpienia wyszukiwania do `customconfig=` parametru. Oddziel parametry znakiem handlowego "i" ( `&` ). Możesz użyć globalnego punktu końcowego w poniższym kodzie lub użyć punktu końcowego [niestandardowej domeny](../../cognitive-services/cognitive-services-custom-subdomains.md) podrzędnej wyświetlanego w Azure Portal dla zasobu.

    ```javascript
    var info = {
        url: 'https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/search?' + 
            'q=' + searchTerm + "&" +
            'customconfig=' + customConfigId,
        headers: {
            'Ocp-Apim-Subscription-Key' : subscriptionKey
        }
    }
    ```

1. Użyj biblioteki żądań języka JavaScript, aby wysłać żądanie wyszukiwania do wystąpienia wyszukiwanie niestandardowe Bing i wydrukować informacje o wynikach, w tym jego nazwę, adres URL i datę ostatniego przeszukiwania strony sieci Web.

    ```javascript
    request(info, function(error, response, body){
            var searchResponse = JSON.parse(body);
            for(var i = 0; i < searchResponse.webPages.value.length; ++i){
                var webPage = searchResponse.webPages.value[i];
                console.log('name: ' + webPage.name);
                console.log('url: ' + webPage.url);
                console.log('displayUrl: ' + webPage.displayUrl);
                console.log('snippet: ' + webPage.snippet);
                console.log('dateLastCrawled: ' + webPage.dateLastCrawled);
                console.log();
            }
    ```

## <a name="next-steps"></a>Następne kroki

> [!div class="nextstepaction"]
> [Tworzenie aplikacji internetowej z funkcją wyszukiwania niestandardowego](./tutorials/custom-search-web-page.md)
