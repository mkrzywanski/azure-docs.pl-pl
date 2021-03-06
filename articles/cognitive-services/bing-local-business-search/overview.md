---
title: Co to jest interfejs API wyszukiwania lokalnych firm Bing?
titleSuffix: Azure Cognitive Services
description: Interfejs API wyszukiwania lokalnych firm Bing jest usługą RESTful, która pozwala aplikacjom znajdować informacje o lokalnych miejscach i firmach na podstawie zapytań wyszukiwania.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-local-business
ms.topic: overview
ms.date: 03/24/2020
ms.author: aahi
ms.openlocfilehash: 685ee0c616234563981e55f14213e424daae32f5
ms.sourcegitcommit: 32592ba24c93aa9249f9bd1193ff157235f66d7e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/01/2020
ms.locfileid: "85611275"
---
# <a name="what-is-bing-local-business-search"></a>Co to jest lokalne wyszukiwanie w biznesie Bing?
Lokalny interfejs API wyszukiwania biznesowego Bing to usługa RESTful, która umożliwia aplikacjom Znajdowanie informacji o lokalnych firmach na podstawie zapytań wyszukiwania. Na przykład, `q=<business-name> in Redmond, Washington` lub `q=Italian restaurants near me` . 

## <a name="features"></a>Funkcje
| Cecha | Opis |  
| -- | -- | 
| [Znajdowanie lokalnych firm i lokalizacji](quickstarts/local-quickstart.md) | Interfejs API wyszukiwania lokalnego firmy Bing umożliwia zlokalizowanie wyników zapytania. Wyniki obejmują adres URL witryny internetowej firmy oraz wyświetla tekst, numer telefonu i lokalizację geograficzną, w tym: współrzędne GPS, miasto, ulica adresu |  
| [Filtrowanie wyników lokalnych ze granicami geograficznymi](specify-geographic-search.md) | Dodaj współrzędne jako parametry wyszukiwania, aby ograniczyć wyniki do określonego obszaru geograficznego, określonego przez okrągły obszar lub kwadratowe pole ograniczające. | 
| [Filtrowanie lokalnych wyników firmy według kategorii](local-categories.md) | Wyszukaj lokalne wyniki biznesowe według kategorii. Ta opcja używa odwrotnej lokalizacji IP lub współrzędnej GPS obiektu wywołującego, aby zwracać zlokalizowane wyniki w różnych kategoriach firmy.|

## <a name="workflow"></a>Przepływ pracy
Wywołaj interfejs API wyszukiwania w usłudze Bing Local Business Search z dowolnego języka programowania, który może wykonywać żądania HTTP i analizować odpowiedzi JSON. Ta usługa jest dostępna przy użyciu interfejsu API REST.
 
1. Utwórz [konto interfejsu API Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) z dostępem do interfejsy API wyszukiwania Bing. Jeśli nie masz subskrypcji platformy Azure, możesz [utworzyć bezpłatne konto](https://azure.microsoft.com/free/cognitive-services/).   
2. Kodowanie w adresie URL wyszukiwanych terminów dla `q=""` parametru zapytania. Na przykład: `q=nearby+restaurant` lub `q=nearby%20restaurant`. W razie konieczności Ustaw również stronicowanie. 
3. Wyślij [żądanie do interfejsu API wyszukiwania lokalnego usługi Bing](quickstarts/local-quickstart.md) 
4. Analizowanie odpowiedzi w formacie JSON 

> [!NOTE]
> Obecnie lokalne wyszukiwanie biznesowe: 
> * Obsługuje tylko `en-US` rynek. 
> * Program nie obsługuje automatyczne sugerowanie Bing. 

## <a name="next-steps"></a>Następne kroki
- [Zapytanie i odpowiedź](local-search-query-response.md)
- [Lokalne wyszukiwanie biznesowe — Szybki Start](quickstarts/local-quickstart.md)
- [Dokumentacja interfejsu API wyszukiwania lokalnych firm Bing](local-search-reference.md)
- [Wymagania dotyczące użycia i wyświetlania](use-display-requirements.md)
