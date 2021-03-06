---
title: Konfigurowanie połączenia ze źródłem danych przy użyciu tożsamości zarządzanej (wersja zapoznawcza)
titleSuffix: Azure Cognitive Search
description: Dowiedz się, jak skonfigurować połączenie indeksatora ze źródłem danych przy użyciu tożsamości zarządzanej (wersja zapoznawcza)
manager: luisca
author: markheff
ms.author: maheff
ms.devlang: rest-api
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 05/18/2020
ms.openlocfilehash: 48b94b8cd047f62ea13bf4e062254088ea11840e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "83664900"
---
# <a name="set-up-an-indexer-connection-to-a-data-source-using-a-managed-identity-preview"></a>Konfigurowanie połączenia indeksatora ze źródłem danych przy użyciu tożsamości zarządzanej (wersja zapoznawcza)

> [!IMPORTANT] 
> Obsługa konfigurowania połączenia ze źródłem danych przy użyciu tożsamości zarządzanej jest obecnie w publicznej wersji zapoznawczej. Funkcje wersji zapoznawczej są dostępne bez umowy dotyczącej poziomu usług i nie są zalecane w przypadku obciążeń produkcyjnych.
> Możesz zażądać dostępu do wersji zapoznawczej, wypełniając [ten formularz](https://aka.ms/azure-cognitive-search/mi-preview-request).

[Indeksator](search-indexer-overview.md) w usłudze Azure wyszukiwanie poznawcze to przeszukiwarka, która zapewnia sposób ściągania danych ze źródła danych do usługi Azure wyszukiwanie poznawcze. Indeksator uzyskuje połączenie ze źródłem danych z tworzonego obiektu źródła danych. Obiekt źródła danych zwykle zawiera poświadczenia dla docelowego źródła danych. Na przykład obiekt źródła danych może zawierać klucz konta usługi Azure Storage, jeśli chcesz zindeksować dane z kontenera magazynu obiektów BLOB.

W wielu przypadkach dostarczanie poświadczeń bezpośrednio w obiekcie źródła danych nie jest problemem, ale istnieją pewne wyzwania, które mogą być dostępne:
* Jak mogę zachować bezpieczeństwo poświadczeń w moim kodzie, który tworzy obiekt źródła danych?
* Jeśli nastąpi naruszenie zabezpieczeń klucza konta lub hasła i chcę je zmienić, teraz trzeba zaktualizować moje obiekty źródła danych przy użyciu nowego klucza konta lub hasła, aby mój indeksator mógł ponownie nawiązać połączenie ze źródłem danych.

Te problemy można rozwiązać przez skonfigurowanie połączenia przy użyciu tożsamości zarządzanej.

## <a name="using-managed-identities"></a>Korzystanie z tożsamości zarządzanych

[Zarządzane tożsamości](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) to funkcja, która zapewnia usługi platformy Azure z automatyczną tożsamością zarządzaną w usłudze Azure Active Directory (Azure AD). Za pomocą tej funkcji w usłudze Azure Wyszukiwanie poznawcze można utworzyć obiekt źródła danych z parametrami połączenia, które nie zawierają żadnych poświadczeń. Zamiast tego usługa wyszukiwania uzyska dostęp do źródła danych za pośrednictwem kontroli dostępu opartej na rolach (RBAC).

Podczas konfigurowania źródła danych przy użyciu tożsamości zarządzanej można zmienić poświadczenia źródła danych, a indeksatory nadal będą mogły nawiązywać połączenia ze źródłem danych. Można również tworzyć obiekty źródła danych w kodzie bez konieczności dołączania klucza konta lub używania Key Vault do pobierania klucza konta.

## <a name="limitations"></a>Ograniczenia

Następujące źródła danych obsługują Konfigurowanie połączenia indeksatora przy użyciu tożsamości zarządzanych. 

* [Azure Blob Storage, Azure Data Lake Storage Gen2 (wersja zapoznawcza), Azure Table Storage](search-howto-managed-identities-storage.md)
* [Azure Cosmos DB](search-howto-managed-identities-cosmos-db.md)
* [Azure SQL Database](search-howto-managed-identities-sql.md)

Następujące funkcje nie obsługują obecnie korzystania z tożsamości zarządzanych w celu skonfigurowania połączenia:
* Magazyn wiedzy
* Umiejętności niestandardowe
 
## <a name="next-steps"></a>Następne kroki

Dowiedz się więcej na temat konfigurowania połączenia indeksatora przy użyciu tożsamości zarządzanych:

* [Azure Blob Storage, Azure Data Lake Storage Gen2 (wersja zapoznawcza), Azure Table Storage](search-howto-managed-identities-storage.md)
* [Azure Cosmos DB](search-howto-managed-identities-cosmos-db.md)
* [Azure SQL Database](search-howto-managed-identities-sql.md)