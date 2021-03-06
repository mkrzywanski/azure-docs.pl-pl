---
title: 'Znane problemy: Migrowanie z MongoDB do platformy Azure CosmosDB'
titleSuffix: Azure Database Migration Service
description: Informacje o znanych problemach i ograniczeniach migracji z MongoDB do Azure Cosmos DB przy użyciu Azure Database Migration Service.
services: database-migration
author: pochiraju
ms.author: rajpo
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: seo-lt-2019
ms.topic: article
ms.date: 02/27/2020
ms.openlocfilehash: ae5d5f2d282c546f5172ca1c8cb0e420d3b6e96b
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "80878055"
---
# <a name="known-issuesmigration-limitations-with-migrations-from-mongodb-to-azure-cosmos-dbs-api-for-mongodb"></a>Znane problemy i ograniczenia dotyczące migracji z migracji z MongoDB do interfejsu API Azure Cosmos DB dla MongoDB

Znane problemy i ograniczenia związane z migracjami z MongoDB do interfejsu API Cosmos DB dla MongoDB zostały opisane w poniższych sekcjach.

## <a name="migration-fails-as-a-result-of-using-the-incorrect-ssl-cert"></a>Migracja nie powiedzie się z powodu użycia nieprawidłowego certyfikatu SSL

* **Objaw**: ten problem występuje, gdy użytkownik nie może nawiązać połączenia z serwerem źródłowym MongoDB. Mimo że wszystkie porty zapory nie są otwarte, użytkownik nadal nie może nawiązać połączenia.

| Przyczyna         | Rozwiązanie |
| ------------- | ------------- |
| Korzystanie z certyfikatu z podpisem własnym w Azure Database Migration Service może prowadzić do niepowodzenia migracji z powodu nieprawidłowego certyfikatu SSL. Komunikat o błędzie może obejmować "certyfikat zdalny jest nieprawidłowy zgodnie z procedurą walidacji". | Użyj oryginalnego certyfikatu z urzędu certyfikacji.  Certyfikaty z podpisem własnym są zwykle używane tylko w przypadku testów wewnętrznych. W przypadku instalowania oryginalnego certyfikatu z urzędu certyfikacji można użyć protokołu SSL w Azure Database Migration Service bez problemu (połączenia Cosmos DB korzystać z protokołu SSL za pośrednictwem interfejsu API Mongo).<br><br> |

## <a name="unable-to-get-the-list-of-databases-to-map-in-dms"></a>Nie można pobrać listy baz danych do zamapowania na DMS

* **Objaw**: nie można pobrać listy baz danych w bloku **Ustawienia bazy** , gdy używane są **dane z trybu usługi Azure Storage** w bloku **Wybierz źródło** .

| Przyczyna         | Rozwiązanie |
| ------------- | ------------- |
| W parametrach połączenia konta magazynu brakuje informacji o sygnaturze dostępu współdzielonego i w związku z tym nie można ich uwierzytelnić. | Utwórz sygnaturę dostępu współdzielonego w kontenerze obiektów BLOB w Eksplorator usługi Storage i użyj adresu URL z informacjami o sygnaturze SAS kontenera jako parametrów połączenia ze szczegółami źródła.<br><br> |

## <a name="using-an-unsupported-version-of-the-database"></a>Korzystanie z nieobsługiwanej wersji bazy danych

* **Objaw**: Migracja nie powiedzie się.

| Przyczyna         | Rozwiązanie |
| ------------- | ------------- |
| Podjęto próbę przeprowadzenia migracji do Azure Cosmos DB z nieobsługiwanej wersji programu MongoDB. | Po udostępnieniu nowych wersji MongoDB są one sprawdzane w celu zapewnienia zgodności z Azure Database Migration Service, a usługa jest okresowo aktualizowana w celu zaakceptowania najnowszych wersji. Jeśli konieczne jest natychmiastowe przeprowadzenie migracji, jako obejście można wyeksportować bazy danych/kolekcje do usługi Azure Storage, a następnie wskazać źródło otrzymanego zrzutu. Utwórz sygnaturę dostępu współdzielonego w kontenerze obiektów BLOB w Eksplorator usługi Storage, a następnie użyj adresu URL z informacjami o sygnaturze SAS kontenera jako parametrów połączenia ze szczegółami źródła.<br><br> |

## <a name="next-steps"></a>Następne kroki

* Zapoznaj się z samouczkiem [Migrowanie MongoDB Azure Cosmos DB do interfejsu API usługi MongoDB w trybie online za pomocą usługi DMS](tutorial-mongodb-cosmos-db-online.md).
* Zapoznaj się z samouczkiem [Migrowanie MongoDB Azure Cosmos DB do interfejsu API usługi MongoDB w trybie offline za pomocą usługi DMS](tutorial-mongodb-cosmos-db.md).