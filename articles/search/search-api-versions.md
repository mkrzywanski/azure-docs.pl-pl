---
title: Wersje interfejsu API
titleSuffix: Azure Cognitive Search
description: Zasady wersji dla interfejsów API REST platformy Azure Wyszukiwanie poznawcze i biblioteki klienta w zestawie SDK platformy .NET.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 07/20/2020
ms.openlocfilehash: 5be50453dff9acaf4a9876eec1d95b56abebf745
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/23/2020
ms.locfileid: "87029845"
---
# <a name="api-versions-in-azure-cognitive-search"></a>Wersje interfejsu API na platformie Azure Wyszukiwanie poznawcze

Usługa Azure Wyszukiwanie poznawcze regularnie przedstawia aktualizacje funkcji. Czasami, ale nie zawsze, te aktualizacje wymagają nowej wersji interfejsu API w celu zachowania zgodności z poprzednimi wersjami. Opublikowanie nowej wersji pozwala określić, kiedy i w jaki sposób integrują aktualizacje usługi Search w kodzie.

Zgodnie z regułą zespół usługi Azure Wyszukiwanie poznawcze publikuje nowe wersje tylko wtedy, gdy jest to konieczne, ponieważ może to wymagać pewnego wysiłku, aby uaktualnić kod w celu użycia nowej wersji interfejsu API. Nowa wersja jest wymagana tylko wtedy, gdy jakiś aspekt interfejsu API zmienił się w taki sposób, który powoduje przerwanie zgodności z poprzednimi wersjami. Takie zmiany mogą wystąpić z powodu poprawek do istniejących funkcji lub z powodu nowych funkcji, które zmieniają istniejący obszar powierzchni interfejsu API.

Ta sama reguła dotyczy aktualizacji zestawu SDK. Zestaw SDK platformy Azure Wyszukiwanie poznawcze jest zgodny z regułami [wersji semantycznej](https://semver.org/) , co oznacza, że jej wersja ma trzy części: główna, pomocnicza i numer kompilacji (na przykład 1.1.0). Nowa główna wersja zestawu SDK jest wydawana tylko dla zmian, które Przerwij zgodność z poprzednią wersją. Niekrytyczne aktualizacje funkcji spowodują zwiększenie wersji pomocniczej, a poprawki błędów spowodują zwiększenie wersji kompilacji.

> [!Important]
> Zestawy Azure SDK dla platform .NET, Java, Python i JavaScript wdrażają nowe biblioteki klienckie dla Wyszukiwanie poznawcze platformy Azure. Obecnie żadna z bibliotek zestawu Azure SDK nie obsługuje najnowszych interfejsów API REST wyszukiwania (2020-06-30) lub interfejsów API REST zarządzania (2020-03-13), ale ta zmiana zostanie zmieniona z upływem czasu. Możesz okresowo sprawdzać Tę stronę lub [nowości](whats-new.md) dotyczące ogłoszeń dotyczących ulepszeń funkcjonalnych. 

## <a name="rest-apis"></a>Interfejsy API REST

Wystąpienie usługi Azure Wyszukiwanie poznawcze obsługuje kilka wersji interfejsu API REST, w tym najnowsze. Możesz nadal korzystać z wersji, gdy nie jest już Najnowsza, ale zalecamy przeprowadzenie [migracji kodu](search-api-migration.md) w celu użycia najnowszej wersji. W przypadku korzystania z interfejsu API REST należy określić wersję interfejsu API w każdym żądaniu za pośrednictwem parametru API-Version. Podczas korzystania z zestawu SDK platformy .NET wersja zestawu SDK określa odpowiednią wersję interfejsu API REST. Jeśli używasz starszego zestawu SDK, możesz nadal uruchamiać ten kod bez zmian, nawet jeśli usługa zostanie uaktualniona w celu obsługi nowszej wersji interfejsu API.

W poniższej tabeli przedstawiono historię wersji bieżących i wcześniej wydanych wersji interfejsu API REST Search Service. Dokumentacja jest publikowana tylko dla najnowszych stabilnych i wersji zapoznawczych.

### <a name="search-service-apis"></a>Interfejsy API Search Service

Utwórz zawartość usługi Search i zarządzaj nią.

| Wersja&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;   | Stan | Problem ze zgodnością wstecz |
|-------------------------|--------|------------------------------|
| [Wyszukiwanie 2020-06-30](https://docs.microsoft.com/rest/api/searchservice/index)| Stable | Najnowsza stabilna wersja interfejsów API REST wyszukiwania, z zaawansowaniem oceny przydatności i ogólnie dostęp do sklepu z bazami danych.|
| [Wyszukiwanie 2020-06-30-Preview](https://docs.microsoft.com/rest/api/searchservice/index-preview)| Wersja zapoznawcza | Wersja zapoznawcza skojarzona z stabilną wersją. Zawiera wiele [funkcji w wersji zapoznawczej](search-api-preview.md). |
| Wyszukiwanie 2019-05-06 | Stable | Dodaje [typy złożone](search-howto-complex-data-types.md). |
| Wyszukiwanie 2019-05-06 — wersja zapoznawcza | Wersja zapoznawcza | Wersja zapoznawcza skojarzona z stabilną wersją. |
| Wyszukaj 2017-11-11 | Stable  | Dodaje [wzbogacanie](cognitive-search-concept-intro.md)umiejętności i AI. |
| Wyszukiwanie 2017-11-11 — wersja zapoznawcza | Wersja zapoznawcza | Wersja zapoznawcza skojarzona z stabilną wersją. |
| Wyszukaj 2016-09-01 |Stable | Dodaje [indeksatory](search-indexer-overview.md). |
| Wyszukiwanie 2016-09-01 — wersja zapoznawcza | Wersja zapoznawcza | Wersja zapoznawcza skojarzona z stabilną wersją.|
| Wyszukaj 2015-02-28 | Stable  | Pierwsza ogólnie dostępna wersja.  |
| Wyszukiwanie 2015-02-28 — wersja zapoznawcza | Wersja zapoznawcza | Wersja zapoznawcza skojarzona z stabilną wersją. |
| Wyszukiwanie 2014-10-20 — wersja zapoznawcza | Wersja zapoznawcza | Druga publiczna wersja zapoznawcza. |
| Wyszukiwanie 2014-07-31 — wersja zapoznawcza | Wersja zapoznawcza | Pierwsza publiczna wersja zapoznawcza. |

### <a name="management-rest-apis"></a>Interfejsy API REST zarządzania

Utwórz i skonfiguruj usługę wyszukiwania oraz Zarządzaj kluczami interfejsu API.

| Wersja&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;   | Stan | Problem ze zgodnością wstecz |
|-------------------------|--------|------------------------------|
| [Zarządzanie 2020-03-13](https://docs.microsoft.com/rest/api/searchmanagement/) | Stable | Najnowsza stabilna wersja interfejsów API REST zarządzania z zaawansowaniem w programie Endpoint Protection. Dodaje [prywatny punkt końcowy](service-create-private-endpoint.md) za poorednictwem prywatnego linku i [reguły protokołu IP sieci](service-configure-firewall.md) dla nowych usług. |
| [Zarządzanie 2019-10-01-Preview](https://docs.microsoft.com/rest/api/searchmanagement/index-2019-10-01-preview) | Wersja zapoznawcza  | Mimo numeru wersji jest to nadal bieżąca wersja zapoznawcza interfejsów API REST zarządzania. Obecnie nie ma żadnych funkcji w wersji zapoznawczej. Wszystkie funkcje w wersji zapoznawczej zostały ostatnio przenoszone do ogólnej dostępności. |
| Zarządzanie 2015-08-19  | Stable | Pierwsza ogólnie dostępna wersja interfejsów API REST zarządzania. Zapewnia obsługę administracyjną, skalowanie w górę i zarządzanie kluczami interfejsu API. |
| Zarządzanie 2015-08-19 — wersja zapoznawcza | Wersja zapoznawcza | Pierwsza wersja zapoznawcza interfejsów API REST zarządzania. |

## <a name="azure-sdk-for-net"></a>Zestaw Azure SDK dla platformy .NET

Historia wersji pakietu jest dostępna w witrynie NuGet.org. Ta tabela zawiera linki do poszczególnych stron pakietu.


| Wersja zestawu SDK | Stan | Opis |
|-------------|--------|------------------------------|
| [Azure.Search.Documents 11,0](https://www.nuget.org/packages/Azure.Search.Documents/1.0.0-preview.4) | Stable | Nowa Biblioteka kliencka z zestawu Azure .NET SDK wydana w lipcu 2020. Celem interfejsu API REST wyszukiwania — wersja = 2020-06-30 REST API, ale nie obsługuje jeszcze filtrów geograficznych ani [FieldBuilder](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.fieldbuilder?view=azure-dotnet). |
| [Microsoft. Azure. Search 10,0](https://www.nuget.org/packages/Microsoft.Azure.Search/) | Stable | Wydanie może 2019. Jest przeznaczony dla interfejsu API REST wyszukiwania — Version = 2019-05-06.|
| [Microsoft. Azure. Search 8,0 — wersja zapoznawcza](https://www.nuget.org/packages/Microsoft.Azure.Search/8.0.0-preview) | Wersja zapoznawcza | wydana kwiecień 2019. Celem interfejsu API REST wyszukiwania — wersja = 2019-05 -06-Preview.|
| [Microsoft. Azure. Management. Search 3.0.0](https://docs.microsoft.com/dotnet/api/overview/azure/search/management?view=azure-dotnet) | Stable | Jest przeznaczony dla interfejsu API REST zarządzania — Version = 2015-08-19.  |

## <a name="azure-sdk-for-java"></a>Zestaw Azure SDK dla języka Java

| Wersja zestawu SDK | Stan | Opis  |
|-------------|--------|------------------------------|
| [Java Azure — wyszukiwanie-dokumenty 11](https://azuresdkdocs.blob.core.windows.net/$web/java/azure-search-documents/11.0.0/index.html) | Stable | Nowa Biblioteka kliencka z zestawu Azure .NET SDK wydana w lipcu 2020. Jest przeznaczony dla interfejsu API REST wyszukiwania — Version = 2019-05-06. |
| [1.35.0 klienta zarządzania Java](https://docs.microsoft.com/java/api/overview/azure/search/management?view=azure-java-stable) | Stable | Jest przeznaczony dla interfejsu API REST zarządzania — Version = 2015-08-19. |

## <a name="azure-sdk-for-javascript"></a>Zestaw Azure SDK dla języka JavaScript

| Wersja zestawu SDK | Stan | Opis  |
|-------------|--------|------------------------------|
| [JavaScript Azure — wyszukiwanie 11,0](https://azure.github.io/azure-sdk-for-node/azure-search/latest/) | Stable | Nowa Biblioteka kliencka z zestawu Azure .NET SDK wydana w lipcu 2020. Dotyczy interfejsu API REST wyszukiwania-Version = 2016-09-01. |
| [JavaScript Azure — ARM — wyszukiwanie](https://azure.github.io/azure-sdk-for-node/azure-arm-search/latest/) | Stable | Jest przeznaczony dla interfejsu API REST zarządzania — Version = 2015-08-19. |

## <a name="azure-sdk-for-python"></a>Zestaw Azure SDK dla środowiska Python

| Wersja zestawu SDK | Stan | Opis  |
|-------------|--------|------------------------------|
| [Python Azure-Search-Documents 11,0](https://azuresdkdocs.blob.core.windows.net/$web/python/azure-search-documents/11.0.0/index.html) | Stable | Nowa Biblioteka kliencka z zestawu Azure .NET SDK wydana w lipcu 2020. Jest przeznaczony dla interfejsu API REST wyszukiwania — Version = 2019-05-06. |
| [Python Azure-zarządzanie — wyszukiwanie 1,0](https://docs.microsoft.com/python/api/overview/azure/search?view=azure-python) | Stable | Jest przeznaczony dla interfejsu API REST zarządzania — Version = 2015-08-19. |