---
title: dołączanie pliku
description: dołączanie pliku
services: cosmos-db
author: LuisBosquez
ms.service: cosmos-db
ms.topic: include
ms.date: 04/15/2020
ms.author: lbosq
ms.custom: include file
ms.openlocfilehash: 8d12645bd302a7ea808f13c07e2a72f66846bad7
ms.sourcegitcommit: 23604d54077318f34062099ed1128d447989eea8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2020
ms.locfileid: "85121369"
---
Teraz można użyć narzędzia Eksplorator danych w Azure Portal, aby utworzyć interfejs API Azure Cosmos DB dla bazy danych i kontenera MongoDB. 

1. Wybierz pozycję **Eksplorator danych**  >  **nowy kontener**. 
    
    Obszar **Dodaj kontener** jest wyświetlany po prawej stronie, może być konieczne przewinięcie w prawo w celu wyświetlenia go.

    :::image type="content" source="./media/cosmos-db-create-collection/azure-cosmosdb-mongodb-data-explorer.png" alt-text="Eksplorator danych w witrynie Azure Portal, okienko Dodawanie kontenera":::

2. Na stronie **Dodawanie kontenera** wprowadź ustawienia dla nowego kontenera.

    |Ustawienie|Sugerowana wartość|Opis
    |---|---|---|
    |**Identyfikator bazy danych**|bazy danych|Wprowadź *bazę* danych jako nazwę nowej bazy danych. Nazwy baz danych muszą zawierać od 1 do 255 znaków i nie mogą zawierać znaków `/, \\, #, ?` ani mieć spacji na końcu. Sprawdź opcję **zainicjuj przepływność bazy danych** , która umożliwia udostępnianie przepływności dla bazy danych we wszystkich kontenerach w bazie danych. Ta opcja pomaga również w obniżyć kosztów. |
    |**Przepływność**|400|Pozostaw przepływność na 400 jednostek żądań na sekundę (RU/s). Jeśli chcesz zmniejszyć opóźnienie, możesz później przeskalować przepływność w górę. Możesz również wybrać [tryb skalowania automatycznego](https://docs.microsoft.com/azure/cosmos-db/provision-throughput-autoscale), który zapewni zakres ru/s, który będzie dynamicznie zwiększać i zmniejszać w razie konieczności.| 
    |**Identyfikator kolekcji**|Coll|Wprowadź `coll` nazwę nowego kontenera. W przypadku identyfikatorów kontenerów obowiązują takie same wymagania dotyczące znaków jak dla nazw baz danych.|
    |**Pojemność magazynu**|Stałe (10 GB)|Wprowadź *FIXED (10 GB)* dla tej aplikacji. W przypadku wybrania opcji *nieograniczone*konieczne będzie utworzenie elementu `Shard Key` , który będzie wymagał wszystkich wstawionych elementów.|
    |**Klucz fragmentu**| /_id| W przykładzie opisanym w tym artykule nie jest używany klucz fragmentu, więc ustawienie go na wartość */_id* spowoduje użycie pola automatycznie wygenerowanego identyfikatora jako klucza fragmentu. Dowiedz się więcej o fragmentowania, nazywanym również partycjonowaniem, na [partycjonowaniu w Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/partitioning-overview)|
        
    Wybierz przycisk **OK**. W Eksploratorze danych zostanie wyświetlona nowa baza danych i nowy kontener.
