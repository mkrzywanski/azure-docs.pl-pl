---
title: Transferowanie danych do Azure Files za pomocą usługi Azure Import/Export | Microsoft Docs
description: Dowiedz się, jak utworzyć zadania importowania w Azure Portal, aby przesłać dane do Azure Files.
author: alkohli
services: storage
ms.service: storage
ms.topic: how-to
ms.date: 04/08/2019
ms.author: alkohli
ms.subservice: common
ms.openlocfilehash: a88cf9981d4f3a69a503c9caa56be1b5f35029f6
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/08/2020
ms.locfileid: "86105187"
---
# <a name="use-azure-importexport-service-to-import-data-to-azure-files"></a>Use Azure Import/Export service to import data to Azure Files (Używanie usługi Azure Import/Export do importowania danych do usługi Azure Files)

Ten artykuł zawiera instrukcje krok po kroku dotyczące korzystania z usługi Azure Import/Export do bezpiecznego importowania dużych ilości danych do Azure Files. Aby można było zaimportować dane, usługa wymaga dostarczenia obsługiwanych dysków zawierających dane do centrum danych platformy Azure.  

Usługa Import/Export obsługuje tylko importowanie Azure Files do usługi Azure Storage. Eksportowanie Azure Files nie jest obsługiwane.

## <a name="prerequisites"></a>Wymagania wstępne

Przed utworzeniem zadania importowania w celu transferu danych do Azure Files należy uważnie przejrzeć i wykonać poniższą listę wymagań wstępnych. Należy:

- Mieć aktywną subskrypcję platformy Azure do użycia z usługą Import/Export.
- Mieć co najmniej jedno konto usługi Azure Storage. Zapoznaj się z listą [obsługiwanych kont magazynu i typów magazynów dla usługi Import/Export](storage-import-export-requirements.md). Aby uzyskać informacje dotyczące tworzenia nowego konta magazynu, zobacz [jak utworzyć konto magazynu](storage-account-create.md).
- Ma wystarczającą liczbę dysków [obsługiwanych typów](storage-import-export-requirements.md#supported-disks).
- System Windows z uruchomioną [obsługiwaną wersją systemu operacyjnego](storage-import-export-requirements.md#supported-operating-systems).
- [Pobierz WAImportExport w wersji 2](https://aka.ms/waiev2) w systemie Windows. Rozpakuj do folderu domyślnego `waimportexport` . Na przykład `C:\WaImportExport`.
- Mieć konto FedEx/DHL. Jeśli chcesz użyć operatora innego niż FedEx/DHL, skontaktuj się z zespołem operacyjnym Azure Data Box `adbops@microsoft.com` .  
    - Konto musi być prawidłowe, powinno mieć saldo i musi mieć możliwości wysyłki zwrotnej.
    - Generuj numer śledzenia dla zadania eksportu.
    - Każde zadanie powinno mieć oddzielny numer śledzenia. Nie ma możliwości obsługi wielu zadań o tym samym numerze śledzenia.
    - Jeśli nie masz konta nośnego, przejdź do:
        - [Utwórz konto FedEx](https://www.fedex.com/en-us/create-account.html)lub
        - [Utwórz konto DHL](http://www.dhl-usa.com/en/express/shipping/open_account.html).



## <a name="step-1-prepare-the-drives"></a>Krok 1. Przygotowanie dysków

Ten krok powoduje wygenerowanie pliku dziennika. W pliku dziennika są przechowywane podstawowe informacje, takie jak numer seryjny dysku, klucz szyfrowania i szczegóły konta magazynu.

Wykonaj poniższe kroki, aby przygotować dyski.

1. Podłącz nasze stacje dysków do systemu Windows za pomocą łączników SATA.
2. Utwórz pojedynczy wolumin NTFS na każdym dysku. Przypisz literę dysku do woluminu. Nie należy używać mountpoints.
3. Zmodyfikuj plik *dataset.csv* w folderze głównym, w którym znajduje się narzędzie. W zależności od tego, czy chcesz zaimportować plik, czy folder, należy dodać wpisy w pliku *dataset.csv* podobnie jak w poniższych przykładach.  

   - **Aby zaimportować plik**: w poniższym przykładzie dane do skopiowania znajdują się na dysku F:. Plik *MyFile1.txt* jest kopiowany do katalogu głównego *MyAzureFileshare1*. Jeśli *MyAzureFileshare1* nie istnieje, zostanie on utworzony na koncie usługi Azure Storage. Struktura folderów jest utrzymywana.

       ```
           BasePath,DstItemPathOrPrefix,ItemType,Disposition,MetadataFile,PropertiesFile
           "F:\MyFolder1\MyFile1.txt","MyAzureFileshare1/MyFile1.txt",file,rename,"None",None

       ```
   - **Aby zaimportować folder**: wszystkie pliki i foldery w obszarze *MyFolder2* są rekursywnie kopiowane do udziału plików. Struktura folderów jest utrzymywana.

       ```
           "F:\MyFolder2\","MyAzureFileshare1/",file,rename,"None",None

       ```
     W tym samym pliku można wprowadzić wiele wpisów odpowiadających zaimportowanym folderom lub plikom.

       ```
           "F:\MyFolder1\MyFile1.txt","MyAzureFileshare1/MyFile1.txt",file,rename,"None",None
           "F:\MyFolder2\","MyAzureFileshare1/",file,rename,"None",None

       ```
     Dowiedz się więcej [na temat przygotowania pliku CSV zestawu danych](storage-import-export-tool-preparing-hard-drives-import.md).


4. Zmodyfikuj plik *driveset.csv* w folderze głównym, w którym znajduje się narzędzie. Dodaj wpisy w pliku *driveset.csv* podobnie jak w poniższych przykładach. Plik driveset zawiera listę dysków i odpowiednie litery dysku, dzięki czemu narzędzie może prawidłowo wybrać listę dysków do przygotowania.

    W tym przykładzie przyjęto założenie, że są dołączone dwa dyski i podstawowe woluminy NTFS G:\ i H:\ są tworzone. H:\is nie jest zaszyfrowany, a G: jest już zaszyfrowana. Narzędzie formatuje i szyfruje dysk obsługujący H:\ tylko (a nie G: \) .

   - **Dla nieszyfrowanego dysku**: Określ *szyfrowanie* , aby włączyć szyfrowanie funkcji BitLocker na dysku.

       ```
       DriveLetter,FormatOption,SilentOrPromptOnFormat,Encryption,ExistingBitLockerKey
       H,Format,SilentMode,Encrypt,
       ```

   - **Dla dysku, który jest już zaszyfrowany**: Określ *AlreadyEncrypted* i Podaj klucz funkcji BitLocker.

       ```
       DriveLetter,FormatOption,SilentOrPromptOnFormat,Encryption,ExistingBitLockerKey
       G,AlreadyFormatted,SilentMode,AlreadyEncrypted,060456-014509-132033-080300-252615-584177-672089-411631
       ```

     Wiele wpisów można wykonać w tym samym pliku odpowiadającym wielu dyskom. Dowiedz się więcej o [przygotowaniu pliku CSV driveset](storage-import-export-tool-preparing-hard-drives-import.md).

5. Użyj `PrepImport` opcji, aby skopiować i przygotować dane na dysku. W pierwszej sesji kopiowania do kopiowania katalogów i/lub plików z nową sesją kopiowania Uruchom następujące polecenie:

    ```cmd
    .\WAImportExport.exe PrepImport /j:<JournalFile> /id:<SessionId> [/logdir:<LogDirectory>] [/sk:<StorageAccountKey>] [/silentmode] [/InitialDriveSet:<driveset.csv>] DataSet:<dataset.csv>
    ```

   Poniższy przykład importowania jest przedstawiony poniżej.

    ```cmd
    .\WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#1  /sk:************* /InitialDriveSet:driveset.csv /DataSet:dataset.csv /logdir:C:\logs
    ```

6. Plik dziennika o nazwie podanej z `/j:` parametrem jest tworzony dla każdego przebiegu wiersza polecenia. Każdy przygotowany dysk ma plik dziennika, który należy przekazać podczas tworzenia zadania importu. Dyski bez plików dziennika nie są przetwarzane.

    > [!IMPORTANT]
    > - Nie należy modyfikować danych na dyskach lub w pliku dziennika po zakończeniu przygotowywania dysku.

Aby uzyskać dodatkowe przykłady, przejdź do [przykładów dla plików dziennika](#samples-for-journal-files).

## <a name="step-2-create-an-import-job"></a>Krok 2. Tworzenie zadania importu

Wykonaj następujące kroki, aby utworzyć zadanie importowania w Azure Portal.
1. Zaloguj się do https://portal.azure.com/ .
2. Przejdź do obszaru **wszystkie usługi > magazyn > zadania importowania/eksportowania**.

    ![Przejdź do importowania/eksportowania](./media/storage-import-export-data-to-blobs/import-to-blob1.png)

3. Kliknij pozycję **Utwórz zadanie importu/eksportu**.

    ![Kliknij przycisk Importuj/Eksportuj zadanie](./media/storage-import-export-data-to-blobs/import-to-blob2.png)

4. **Podstawowe informacje**:

    - Wybierz pozycję **Importuj na platformie Azure**.
    - Wprowadź opisową nazwę zadania importu. Ta nazwa służy do śledzenia zadań w trakcie ich wykonywania i po ich zakończeniu.
        -  Ta nazwa może zawierać tylko małe litery, cyfry, łączniki i podkreślenia.
        -  Nazwa musi rozpoczynać się od litery i nie może zawierać spacji.
    - Wybierz subskrypcję.
    - Wybierz grupę zasobów.

        ![Tworzenie zadania importowania — krok 1](./media/storage-import-export-data-to-blobs/import-to-blob3.png)

3. W **szczegółach zadania**:

    - Przekaż pliki dziennika utworzone w poprzednim [kroku 1: przygotowanie dysków](#step-1-prepare-the-drives).
    - Wybierz konto magazynu, do którego zostaną zaimportowane dane.
    - Lokalizacja Dropoff jest automatycznie wypełniana na podstawie regionu wybranego konta magazynu.

       ![Tworzenie zadania importowania — krok 2](./media/storage-import-export-data-to-blobs/import-to-blob4.png)

4. W oknie **Informacje o wysyłce zwrotu**:

    - Z listy rozwijanej wybierz pozycję przewoźnik. Jeśli chcesz użyć operatora innego niż FedEx/DHL, wybierz istniejącą opcję z listy rozwijanej. Skontaktuj się z zespołem ds. operacyjnych Azure Data Box `adbops@microsoft.com` z informacjami dotyczącymi przewoźnika, którego zamierzasz używać.
    - Wprowadź prawidłowy numer konta nośnego, który został utworzony za pomocą tego operatora. Firma Microsoft korzysta z tego konta, aby po zakończeniu zadania importowania dostarczać dyski z powrotem do użytkownika.
    - Podaj pełną i poprawną nazwę kontaktu, numer telefonu, adres e-mail, ulica, miasto, kod pocztowy, Województwo i kraj/region.

        > [!TIP]
        > Zamiast określania adresu e-mail dla pojedynczego użytkownika, podaj adres e-mail grupy. Dzięki temu będziesz otrzymywać powiadomienia nawet w przypadku opuszczenia przez administratora.

       ![Tworzenie zadania importowania — krok 3](./media/storage-import-export-data-to-blobs/import-to-blob5.png)


5. **Podsumowanie**:

    - Podaj adres wysyłkowy centrum danych platformy Azure na potrzeby wysyłania dysków z powrotem do platformy Azure. Upewnij się, że nazwa zadania i pełny adres są wymienione na etykiecie wysyłkowej.
    - Kliknij przycisk **OK** , aby zakończyć tworzenie zadania importu.

        ![Tworzenie zadania importowania — krok 4](./media/storage-import-export-data-to-blobs/import-to-blob6.png)

## <a name="step-3-ship-the-drives-to-the-azure-datacenter"></a>Krok 3. dostarczenie dysków do centrum danych platformy Azure

[!INCLUDE [storage-import-export-ship-drives](../../../includes/storage-import-export-ship-drives.md)]

## <a name="step-4-update-the-job-with-tracking-information"></a>Krok 4. aktualizowanie zadania przy użyciu informacji śledzenia

[!INCLUDE [storage-import-export-update-job-tracking](../../../includes/storage-import-export-update-job-tracking.md)]

## <a name="step-5-verify-data-upload-to-azure"></a>Krok 5. Weryfikacja przekazywania danych na platformę Azure

Śledź zadanie do ukończenia. Po zakończeniu zadania Sprawdź, czy dane zostały przekazane do platformy Azure. Usuń dane lokalne dopiero po sprawdzeniu, że przekazywanie zakończyło się pomyślnie.

## <a name="samples-for-journal-files"></a>Przykłady dla plików dziennika

Aby **dodać więcej dysków**, Utwórz nowy plik driveset i uruchom polecenie w następujący sposób.

W przypadku kolejnych sesji kopiowania na różne stacje dysków, które nie są określone w pliku *InitialDriveset. csv* , określ nowy plik driveset *. csv* i podaj go jako wartość parametru `AdditionalDriveSet` . Użyj tej **samej nazwy pliku dziennika** i podaj **nowy identyfikator sesji**. Format pliku CSV AdditionalDriveset jest taki sam jak format InitialDriveSet.

```cmd
WAImportExport.exe PrepImport /j:<JournalFile> /id:<SessionId> /AdditionalDriveSet:<driveset.csv>
```

Poniższy przykład importowania jest przedstawiony poniżej.

```cmd
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#3  /AdditionalDriveSet:driveset-2.csv
```


Aby dodać dodatkowe dane do tego samego driveset, użyj polecenia PrepImport w celu kopiowania dodatkowych plików/katalogów do kolejnych sesji kopiowania.

W przypadku kolejnych sesji kopiowania na te same stacje dysków twardych określonych w *InitialDriveset.csv* pliku należy określić tę **samą nazwę pliku dziennika** i podać **nowy identyfikator sesji**; nie ma potrzeby podania klucza konta magazynu.

```cmd
WAImportExport PrepImport /j:<JournalFile> /id:<SessionId> /j:<JournalFile> /id:<SessionId> [/logdir:<LogDirectory>] DataSet:<dataset.csv>
```

Poniższy przykład importowania jest przedstawiony poniżej.

```cmd
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2  /DataSet:dataset-2.csv
```

## <a name="next-steps"></a>Następne kroki

* [Wyświetlanie stanu zadania i dysku](storage-import-export-view-drive-status.md)
* [Przejrzyj wymagania dotyczące importu/eksportu](storage-import-export-requirements.md)
