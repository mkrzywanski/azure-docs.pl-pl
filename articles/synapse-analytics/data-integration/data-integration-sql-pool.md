---
title: Pozyskiwanie danych do puli SQL
description: Dowiedz się, jak pozyskiwanie danych do puli SQL w usłudze Azure Synapse Analytics
services: synapse-analytics
author: djpmsft
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql
ms.date: 04/15/2020
ms.author: daperlov
ms.reviewer: jrasnick
ms.openlocfilehash: 63e83e69e5e09c17b2a2ddb5ca7bee6474e2fddd
ms.sourcegitcommit: 5b8fb60a5ded05c5b7281094d18cf8ae15cb1d55
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/29/2020
ms.locfileid: "87386677"
---
# <a name="ingest-data-into-a-sql-pool"></a>Pozyskiwanie danych do puli SQL

W tym artykule dowiesz się, jak pozyskać dane z konta magazynu Azure Data Lake Gen 2 do puli SQL przy użyciu usługi Azure Synapse Analytics.

## <a name="prerequisites"></a>Wymagania wstępne

* **Subskrypcja platformy Azure**: Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem Utwórz [bezpłatne konto platformy Azure](https://azure.microsoft.com/free/) .
* **Konto usługi Azure Storage**: używasz Azure Data Lake Storage Gen 2 jako magazynu danych *źródłowych* . Jeśli nie masz konta magazynu, zobacz [Tworzenie konta usługi Azure Storage](../../storage/blobs/data-lake-storage-quickstart-create-account.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) , aby uzyskać instrukcje.
* **Azure Synapse Analytics**: używasz puli SQL jako magazynu danych *ujścia* . Jeśli nie masz wystąpienia usługi Azure Synapse Analytics, zapoznaj się z tematem [Tworzenie puli SQL](../../azure-sql/database/single-database-create-quickstart.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) na potrzeby kroków, aby je utworzyć.

## <a name="create-linked-services"></a>Tworzenie połączonych usług

W usłudze Azure Synapse Analytics, połączona Usługa polega na definiowaniu informacji o połączeniu z innymi usługami. Ta sekcja umożliwia dodanie połączonej usługi Azure Synapse Analytics i Azure Data Lake Storage Gen2.

1. Otwórz środowisko Azure Synapse Analytics i przejdź na kartę **Zarządzanie** .
1. W obszarze **połączenia zewnętrzne**wybierz pozycję **połączone usługi**.
1. Aby dodać połączoną usługę, kliknij przycisk **Nowy**.
1. Wybierz z listy kafelek Azure Data Lake Storage Gen2 i kliknij przycisk **Kontynuuj**.
1. Wprowadź poświadczenia uwierzytelniania. Klucz konta, nazwa główna usługi i tożsamość zarządzana są obecnie obsługiwanymi typami uwierzytelniania. Kliknij pozycję Testuj połączenie, aby sprawdzić, czy Twoje poświadczenia są poprawne. Po zakończeniu kliknij przycisk **Utwórz** .
1. Powtórz kroki 3-5, ale nie Azure Data Lake Storage Gen2, wybierz kafelek Azure Synapse Analytics i wprowadź odpowiednie poświadczenia połączenia. W przypadku usługi Azure Synapse Analytics, uwierzytelniania SQL, tożsamości zarządzanej i nazwy głównej usługi są obecnie obsługiwane.

## <a name="create-pipeline"></a>Tworzenie potoku

Potok zawiera przepływ logiczny dla wykonywania zestawu działań. W tej sekcji utworzysz potok zawierający działanie kopiowania, które pozyskuje dane z ADLS Gen2 w puli SQL.

1. Przejdź do karty **aranżacja** . Kliknij ikonę plus obok nagłówka potoki i wybierz pozycję **potok**.
1. W obszarze **Przenieś i Przekształć** w okienku działania przeciągnij pozycję **Kopiuj dane** na kanwę potoku.
1. Kliknij działanie kopiowania i przejdź do karty **Źródło** . kliknij pozycję **Nowy** , aby utworzyć nowy źródłowy zestaw danych.
1. Wybierz Azure Data Lake Storage Gen2 jako magazyn danych, a następnie kliknij przycisk Kontynuuj.
1. Wybierz pozycję DelimitedText jako format i kliknij przycisk Kontynuuj.
1. W okienku Ustawianie właściwości wybierz utworzoną usługę ADLS. Określ ścieżkę pliku danych źródłowych i określ, czy pierwszy wiersz ma nagłówek. Możesz zaimportować schemat z magazynu plików lub pliku przykładowego. Po zakończeniu kliknij przycisk OK.
1. Przejdź do karty **ujścia** . kliknij pozycję **Nowy** , aby utworzyć nowy zestaw danych ujścia.
1. Wybierz pozycję Azure Synapse Analytics jako magazyn danych, a następnie kliknij przycisk Kontynuuj.
1. W okienku Ustawianie właściwości wybierz utworzoną usługę Azure Synapse Analytics połączonej. Jeśli zapisujesz do istniejącej tabeli, wybierz ją z listy rozwijanej. W przeciwnym razie Sprawdź pozycję **Edytuj** i wprowadź nazwę nowej tabeli. Po zakończeniu kliknij przycisk OK.
1. Jeśli tworzysz tabelę, Włącz funkcję **AutoCreate tabeli** w polu opcji tabeli.

## <a name="debug-and-publish-pipeline"></a>Debuguj i Publikuj potok

Po zakończeniu konfigurowania potoku możesz wykonać przebieg debugowania przed opublikowaniem artefaktów, aby upewnić się, że wszystkie elementy są poprawne.

1. Aby debugować potok, wybierz na pasku narzędzi pozycję **Debuguj**. Na karcie **Dane wyjściowe** w dolnej części okna wyświetlany jest stan uruchomienia potoku. 
1. Po pomyślnym uruchomieniu potoku na górnym pasku narzędzi wybierz pozycję **Opublikuj wszystko**. Ta akcja powoduje opublikowanie jednostek (zestawów danych i potoków) utworzonych w usłudze Synapse Analytics.
1. Poczekaj na wyświetlenie komunikatu **Pomyślnie opublikowano**. Aby wyświetlić komunikaty powiadomień, kliknij przycisk dzwonka w prawym górnym rogu. 


## <a name="trigger-and-monitor-the-pipeline"></a>Wyzwalanie i monitorowanie potoku

W tym kroku ręcznie wyzwolimy potok opublikowany w poprzednim kroku. 

1. Na pasku narzędzi wybierz pozycję **Dodaj wyzwalacz** , a następnie wybierz pozycję **Wyzwól teraz**. Na stronie **Uruchomienie potoku** wybierz przycisk **Zakończ**.  
1. Przejdź do karty **monitorowanie** znajdującej się na lewym pasku bocznym. Widoczne jest uruchomienie potoku, które zostało wyzwolone za pomocą wyzwalacza ręcznego. Możesz użyć linków w kolumnie **Akcje** , aby wyświetlić szczegóły działania i ponownie uruchomić potok.
1. Aby wyświetlić uruchomienia działań skojarzone z uruchomieniem potoku, wybierz link **Wyświetl uruchomienia działań** w kolumnie **Akcje**. W tym przykładzie istnieje tylko jedno działanie, dlatego na liście jest widoczny tylko jeden wpis. Aby uzyskać szczegółowe informacje na temat operacji kopiowania, wybierz link **Szczegóły** (ikona okularów) w kolumnie **Akcje**. Wybierz pozycję **uruchomienia potoku** u góry, aby wrócić do widoku uruchomienia potoków. Aby odświeżyć widok, wybierz pozycję **Odśwież**.
1. Sprawdź, czy dane są poprawnie zapisywane w puli SQL.


## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji na temat integracji danych na potrzeby usługi Synapse Analytics, zobacz artykuł pozyskiwanie [danych do Azure Data Lake Storage Gen2](data-integration-data-lake.md) .
