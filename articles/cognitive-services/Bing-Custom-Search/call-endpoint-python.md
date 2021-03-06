---
title: 'Szybki Start: wywoływanie punktu końcowego wyszukiwanie niestandardowe Bing przy użyciu języka Python | Microsoft Docs'
titleSuffix: Azure Cognitive Services
description: Skorzystaj z tego przewodnika Szybki Start, aby rozpocząć żądanie wyników wyszukiwania z wystąpienia wyszukiwanie niestandardowe Bing przy użyciu języka Python.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: quickstart
ms.date: 05/08/2020
ms.author: aahi
ms.custom: tracking-python
ms.openlocfilehash: b5a39951301256894553a036786ddd2fff251109
ms.sourcegitcommit: 1de57529ab349341447d77a0717f6ced5335074e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/09/2020
ms.locfileid: "84606007"
---
# <a name="quickstart-call-your-bing-custom-search-endpoint-using-python"></a>Szybki Start: wywoływanie punktu końcowego wyszukiwanie niestandardowe Bing przy użyciu języka Python

Skorzystaj z tego przewodnika Szybki Start, aby dowiedzieć się, jak żądać wyników wyszukiwania z wystąpienia wyszukiwanie niestandardowe Bing. Mimo że aplikacja jest zapisywana w języku Python, interfejs API wyszukiwania niestandardowego Bing jest usługą sieci Web RESTful zgodną z większością języków programowania. Kod źródłowy dla tego przykładu jest dostępny w witrynie [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/python/Search/BingCustomSearchv7.py).

## <a name="prerequisites"></a>Wymagania wstępne

- Wystąpienie wyszukiwania niestandardowego Bing. Aby uzyskać więcej informacji, zobacz [Szybki Start: Tworzenie pierwszego wystąpienia wyszukiwanie niestandardowe Bing](quick-start.md).
- Język [Python](https://www.python.org/) 2. x lub 3. x.

[!INCLUDE [cognitive-services-bing-custom-search-prerequisites](../../../includes/cognitive-services-bing-custom-search-signup-requirements.md)]


## <a name="create-and-initialize-the-application"></a>Tworzenie i inicjowanie aplikacji

- Utwórz nowy plik w języku Python w ulubionym środowisku IDE lub edytorze i dodaj następujące instrukcje importu. Utwórz zmienne dla klucza subskrypcji, niestandardowego identyfikatora konfiguracji i wyszukiwanego terminu.

    ```python
    import json
    import requests
    
    subscriptionKey = "YOUR-SUBSCRIPTION-KEY"
    customConfigId = "YOUR-CUSTOM-CONFIG-ID"
    searchTerm = "microsoft"
    ```

## <a name="send-and-receive-a-search-request"></a>Wysyłanie i odbieranie żądania wyszukiwania 

1. Utwórz adres URL żądania przez dołączenie terminu wyszukiwania do `q=` parametru zapytania i NIESTANDARDOWEGO identyfikatora konfiguracji wystąpienia wyszukiwania do `customconfig=` parametru. Oddziel parametry znakiem handlowego "i" ( `&` ). Możesz użyć globalnego punktu końcowego w poniższym kodzie lub użyć punktu końcowego [niestandardowej domeny](../../cognitive-services/cognitive-services-custom-subdomains.md) podrzędnej wyświetlanego w Azure Portal dla zasobu.

    ```python
    url = 'https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/search?' + 'q=' + searchTerm + '&' + 'customconfig=' + customConfigId
    ```

2. Wyślij żądanie do wystąpienia wyszukiwanie niestandardowe Bing i wydrukuj zwrócone wyniki wyszukiwania.  

    ```python
    r = requests.get(url, headers={'Ocp-Apim-Subscription-Key': subscriptionKey})
    print(r.text)
    ```

## <a name="next-steps"></a>Następne kroki

> [!div class="nextstepaction"]
> [Tworzenie aplikacji internetowej z funkcją wyszukiwania niestandardowego](./tutorials/custom-search-web-page.md)
