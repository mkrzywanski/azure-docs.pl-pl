---
title: 'Importuj dane: odwołanie do modułu'
titleSuffix: Azure Machine Learning
description: Dowiedz się, jak za pomocą modułu Importuj dane w Azure Machine Learning załadować dane do potoku uczenia maszynowego z istniejących usług danych w chmurze.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 10/22/2019
ms.openlocfilehash: e47cb1180bbc6eaaaffd79b78563ab1b1d5e016a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "85611819"
---
# <a name="import-data-module"></a>Importuj moduł danych

W tym artykule opisano moduł w programie Azure Machine Learning Designer (wersja zapoznawcza).

Ten moduł służy do ładowania danych do potoku uczenia maszynowego z istniejących usług danych w chmurze. 

> [!Note]
> Wszystkie funkcje zapewniane przez ten moduł mogą być wykonywane przez **Magazyn** **danych i DataSets** na stronie docelowej obszaru roboczego. Zalecamy korzystanie z **magazynu** danych i **zestawu danych** , który zawiera dodatkowe funkcje, takie jak monitorowanie dane. Aby dowiedzieć się więcej, zobacz artykuł [jak uzyskać dostęp do danych](../how-to-access-data.md) i [jak rejestrować zestawy danych](../how-to-create-register-datasets.md) .
> Po zarejestrowaniu zestawu danych możesz go znaleźć w kategorii **zestawy**danych  ->  **MOJE ZESTAWY** danych w interfejsie projektanta. Ten moduł jest zarezerwowany dla użytkowników programu Studio (klasycznych) w celu uzyskania znanego środowiska. 
>

Moduł **Importuj dane** obsługuje odczyt danych z następujących źródeł:

- Adres URL za pośrednictwem protokołu HTTP
- Magazyny w chmurze platformy Azure za pomocą [**magazynów**](../how-to-access-data.md)danych)
    - Kontener obiektów blob platformy Azure
    - Udział plików platformy Azure
    - Azure Data Lake
    - Azure Data Lake Gen2
    - Azure SQL Database
    - Azure PostgreSQL    

Przed rozpoczęciem korzystania z magazynu w chmurze musisz najpierw zarejestrować magazyn danych w obszarze roboczym Azure Machine Learning. Aby uzyskać więcej informacji, zobacz [jak uzyskać dostęp do danych](../how-to-access-data.md). 

Po zdefiniowaniu danych, które chcesz połączyć ze źródłem, **[Importowanie danych](./import-data.md)** powoduje, że dane są uwzględniane w poszczególnych kolumnach na podstawie wartości, które zawiera, i ładowania danych do potoku projektanta. Dane wyjściowe **importu** są zestawem danych, który może być używany z dowolnym potokiem projektanta.

Jeśli dane źródłowe zmienią się, można odświeżyć zestaw danych i dodać nowe dane przez ponowne uruchomienie [importowania danych](./import-data.md).

> [!WARNING]
> Jeśli obszar roboczy znajduje się w sieci wirtualnej, musisz skonfigurować magazyny danych tak, aby korzystały z funkcji wizualizacji z danymi projektanta. Aby uzyskać więcej informacji na temat sposobu korzystania z magazynów danych i ich zestawów w sieci wirtualnej, zapoznaj się z tematem [izolacja sieci podczas uczenia & wnioskowania z prywatnymi sieciami wirtualnymi](../how-to-enable-virtual-network.md#machine-learning-studio).


## <a name="how-to-configure-import-data"></a>Jak skonfigurować Importowanie danych

1. Dodaj moduł **Import danych** do potoku. Ten moduł można znaleźć w kategorii dane **wejściowe i wyjściowe** w projektancie.

1. Wybierz moduł, aby otworzyć okienko po prawej stronie.

1. Wybierz pozycję **Źródło danych**i wybierz typ źródła danych. Może to być HTTP lub magazyn danych.

    Jeśli wybierzesz pozycję Magazyn danych, możesz wybrać istniejące magazyny danych, które zostały już zarejestrowane w obszarze roboczym Azure Machine Learning, lub utworzyć nowy magazyn danych. Następnie należy zdefiniować ścieżkę danych do zaimportowania w magazynie danych. Możesz łatwo przeglądać ścieżkę, klikając kolejno pozycje **Przeglądaj ścieżka** ![ Importuj-dane-ścieżka](media/module/import-data-path.png)

1. Wybierz schemat podglądu, aby odfiltrować kolumny, które chcesz dołączyć. Możesz również zdefiniować ustawienia zaawansowane, takie jak ogranicznik w opcjach analizy.

    ![Importowanie danych — wersja zapoznawcza](media/module/import-data.png)

1. Pole wyboru, **Wygeneruj ponownie dane wyjściowe**, decyduje o tym, czy uruchomić moduł w celu ponownego wygenerowania danych wyjściowych w czasie wykonywania. 

    Jest to domyślnie niezaznaczone, co oznacza, że moduł został wykonany z tymi samymi parametrami, system użyje danych wyjściowych z ostatniego uruchomienia, aby skrócić czas wykonywania. 

    W przypadku wybrania tej operacji system ponownie uruchomi moduł w celu ponownego wygenerowania danych wyjściowych. Wybierz tę opcję, jeśli dane podstawowe w magazynie zostaną zaktualizowane, może to pomóc w uzyskaniu najnowszych danych.


1. Prześlij potok.

    Gdy importowanie danych powoduje załadowanie danych do projektanta, wnioskuje typ danych każdej kolumny na podstawie wartości, które zawiera, wartość numeryczną lub kategorii.

    Jeśli nagłówek jest obecny, nagłówek jest używany do nazwy kolumn wyjściowego zestawu danych.

    Jeśli w danych nie ma nagłówków istniejących kolumn, nowe nazwy kolumn są generowane przy użyciu formatu Kol1, Col2,... , coln*.

## <a name="results"></a>Wyniki

Po zakończeniu importu kliknij wyjściowy zestaw danych i wybierz polecenie **Wizualizuj** , aby sprawdzić, czy dane zostały zaimportowane pomyślnie.

Jeśli chcesz zapisać dane do ponownego użycia, zamiast zaimportować nowy zestaw danych przy każdym uruchomieniu potoku, wybierz ikonę **zarejestruj zestaw** danych na karcie dane **wyjściowe** w prawym panelu modułu. Wybierz nazwę dla zestawu danych. Zapisany zestaw danych zachowuje dane podczas zapisywania, zestaw danych nie zostanie zaktualizowany po ponownym uruchomieniu potoku, nawet jeśli zestaw danych w potoku ulegnie zmianie. Może to być przydatne przy tworzeniu migawek danych.

Po zaimportowaniu danych może zajść potrzeba przeprowadzenia pewnych dodatkowych przygotowań do modelowania i analizy:

- Użyj opcji [Edytuj metadane](./edit-metadata.md) , aby zmienić nazwy kolumn, obsłużyć kolumnę jako inny typ danych lub wskazać, że niektóre kolumny są etykietami lub funkcjami.

- Użyj [opcji wybierz kolumny w zestawie danych](./select-columns-in-dataset.md) , aby wybrać podzbiór kolumn do przekształcenia lub użycia podczas modelowania. Kolumny przekształcone lub usunięte można łatwo ponownie przyłączyć do oryginalnego zestawu danych przy użyciu modułu [Dodaj kolumny](./add-columns.md) .  

- Użyj [partycji i przykładu](./partition-and-sample.md) , aby podzielić zestaw danych, wykonać próbkowanie lub pobrać pierwsze n wierszy.

## <a name="next-steps"></a>Następne kroki

Zapoznaj się z [zestawem modułów dostępnych](module-reference.md) do Azure Machine Learning. 
