---
title: 'Szybki Start: Tworzenie obszaru roboczego Synapse'
description: Utwórz obszar roboczy Synapse, wykonując czynności opisane w tym przewodniku.
services: synapse-analytics
author: pimorano
ms.service: synapse-analytics
ms.topic: quickstart
ms.subservice: ''
ms.date: 04/15/2020
ms.author: pimorano
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: da7f115224db10ad1d66e8ffe7b86e58e43ae866
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/23/2020
ms.locfileid: "87052452"
---
# <a name="quickstart-create-a-synapse-workspace"></a>Szybki Start: Tworzenie obszaru roboczego Synapse

W tym przewodniku szybki start opisano kroki tworzenia obszaru roboczego usługi Azure Synapse za pomocą Azure Portal.

Jeśli nie masz subskrypcji platformy Azure, [przed rozpoczęciem utwórz bezpłatne konto](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Wymagania wstępne

- [Konto magazynu Azure Data Lake Storage Gen2](../storage/common/storage-account-create.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json)

## <a name="sign-in-to-the-azure-portal"></a>Logowanie się do witryny Azure Portal

Zaloguj się w witrynie [Azure Portal](https://portal.azure.com/).

## <a name="create-an-azure-synapse-workspace-using-the-azure-portal"></a>Tworzenie obszaru roboczego usługi Azure Synapse za pomocą Azure Portal

1. W okienku wyszukiwania Microsoft Azure wprowadź **Synapse obszar roboczy** , a następnie wybierz pozycję Ta usługa.
![Azure Portal pasku wyszukiwania przy użyciu obszarów roboczych usługi Azure Synapse wpisanych w temacie.](media/quickstart-create-synapse-workspace/workspace-search.png)
2. Na stronie **obszary robocze Synapse** kliknij pozycję **+ Dodaj**.
![Polecenie utworzenia nowego wyróżnionego obszaru roboczego usługi Azure Synapse.](media/quickstart-create-synapse-workspace/create-workspace-02.png)
3. Wypełnij formularz **obszaru roboczego usługi Azure Synapse** , podając następujące informacje:

    | Ustawienie | Sugerowana wartość | Opis |
    | :------ | :-------------- | :---------- |
    | **Subskrypcja** | *Twoja subskrypcja* | Aby uzyskać szczegółowe informacje o subskrypcjach, zobacz [Subskrypcje](https://account.windowsazure.com/Subscriptions). |
    | **Grupa zasobów** | *Dowolna Grupa zasobów* | Prawidłowe nazwy grup zasobów opisano w artykule [Naming rules and restrictions](/azure/architecture/best-practices/resource-naming?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) (Reguły i ograniczenia nazewnictwa). |
    | **Nazwa obszaru roboczego** | mysampleworkspace | Określa nazwę obszaru roboczego, który będzie również używany dla punktów końcowych połączenia.|
    | **Region** | Wschodnie stany USA 2 | Określa lokalizację obszaru roboczego.|
    | **Data Lake Storage Gen2** | Koncie`storage account name` </br> System plików:`root file system to use` | Określa nazwę konta magazynu ADLS Gen2, która ma być używana jako magazyn podstawowy i system plików do użycia.|
    ||||

    ![Przepływ obszaru roboczego — podstawowe karty.](media/quickstart-create-synapse-workspace/create-workspace-03.png)

    Konto magazynu można wybrać z:
    - Lista kont ADLS Gen2 dostępnych w ramach subskrypcji
    - Wprowadzono ręcznie przy użyciu nazwy konta

    > [!IMPORTANT]
    > Obszar roboczy usługi Azure Synapse musi mieć możliwość odczytu i zapisu na wybranym koncie ADLS Gen2. Ponadto dla dowolnego konta magazynu, które można połączyć jako konto magazynu podstawowego, należy włączyć **hierarchiczną przestrzeń nazw** podczas tworzenia konta magazynu.
    >
    > Poniżej pól wyboru ADLS Gen2 należy zauważyć, że zarządzana tożsamość obszaru roboczego zostanie przypisana do roli **współautor danych obiektów blob magazynu** w wybranym Data Lake Storage Gen2 systemie plików, przyznając im pełny dostęp.

4. Obowiązkowe Zmodyfikuj dowolną kartę **Ustawienia zabezpieczeń i sieci** :
5. Obowiązkowe Dodaj dowolne Tagi na karcie **Tagi** .
6. Na karcie **Podsumowanie** zostaną uruchomione wymagane walidacje, aby upewnić się, że obszar roboczy może zostać pomyślnie utworzony. Po pomyślnym zakończeniu walidacji kliknij pozycję **Utwórz** ![ przepływ obsługi obszaru roboczego — karta potwierdzenie.](media/quickstart-create-synapse-workspace/create-workspace-05.png)
7. Po pomyślnym zakończeniu procesu aprowizacji zasobów zobaczysz wpis dla utworzonego obszaru roboczego na liście obszarów roboczych Synapse. ![Lista obszarów roboczych Synapse z nowo zainicjowanym obszarem roboczym.](media/quickstart-create-synapse-workspace/create-workspace-07.png)

## <a name="clean-up-resources"></a>Czyszczenie zasobów

Wykonaj poniższe kroki, aby usunąć obszar roboczy usługi Azure Synapse.
> [!WARNING]
> Usunięcie obszaru roboczego usługi Azure Synapse spowoduje usunięcie aparatów analizy i danych przechowywanych w bazie danych zawartych pul SQL i metadanych obszaru roboczego. Nie będzie już można łączyć się z punktami końcowymi SQL, Apache Spark punkty końcowe. Wszystkie artefakty kodu zostaną usunięte (zapytania, notesy, definicje zadań i potoki).
>
> Usunięcie obszaru roboczego **nie** wpłynie na dane w Data Lake Store Gen2 połączonym z obszarem roboczym.

Jeśli chcesz usunąć obszar roboczy usługi Azure Synapse, wykonaj następujące czynności:

1. Przejdź do obszaru roboczego usługi Azure Synapse, aby usunąć.
1. Naciśnij klawisz **delete** na pasku poleceń.
 ![Omówienie obszaru roboczego usługi Azure Synapse — usunięte polecenie Usuń.](media/quickstart-create-synapse-workspace/create-workspace-10.png)
1. Potwierdź usunięcie i naciśnij przycisk **Usuń** .
 ![Omówienie obszaru roboczego usługi Azure Synapse — usuwanie okna dialogowego potwierdzenia obszaru roboczego.](media/quickstart-create-synapse-workspace/create-workspace-11.png)
1. Po pomyślnym zakończeniu procesu obszar roboczy usługi Azure Synapse nie będzie już wyświetlany na liście obszarów roboczych.

## <a name="next-steps"></a>Następne kroki

Następnie możesz [utworzyć pule SQL](quickstart-create-sql-pool-studio.md) lub [utworzyć pule Apache Spark](quickstart-create-apache-spark-pool-studio.md) , aby rozpocząć analizowanie i Eksplorowanie danych.
