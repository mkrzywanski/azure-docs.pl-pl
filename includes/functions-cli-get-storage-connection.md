---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/25/2020
ms.author: glenga
ms.openlocfilehash: 5cb345ef2d20f75066e90f9e6478be27f925b1b0
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/29/2020
ms.locfileid: "80673362"
---
### <a name="retrieve-the-azure-storage-connection-string"></a>Pobieranie parametrów połączenia usługi Azure Storage

Wcześniej utworzono konto usługi Azure Storage do użycia przez aplikację funkcji. Parametry połączenia dla tego konta są bezpiecznie przechowywane w ustawieniach aplikacji na platformie Azure. Pobierając ustawienia do pliku *Local. Settings. JSON* , można użyć zapisu tego połączenia do kolejki magazynu w ramach tego samego konta podczas lokalnego uruchamiania funkcji. 

1. Z poziomu katalogu głównego projektu uruchom następujące polecenie, zastępując `<app_name>` je nazwą aplikacji funkcji z poprzedniego przewodnika Szybki Start. To polecenie spowoduje zastąpienie wszelkich istniejących wartości w pliku.

    ```
    func azure functionapp fetch-app-settings <app_name>
    ```
    
1. Otwórz plik *Local. Settings. JSON* i Znajdź wartość o `AzureWebJobsStorage`nazwie, która jest ciągiem połączenia konta magazynu. Używasz nazwy `AzureWebJobsStorage` i parametrów połączenia w innych sekcjach tego artykułu.

> [!IMPORTANT]
> Ponieważ *Local. Settings. JSON* zawiera wpisy tajne pobrane z platformy Azure, należy zawsze wykluczyć ten plik z kontroli źródła. Plik *. gitignore* utworzony za pomocą projektu funkcji lokalnych domyślnie wyklucza plik.