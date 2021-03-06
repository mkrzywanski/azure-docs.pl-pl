---
title: Identyfikator URI sygnatury dostępu współdzielonego dla obrazów maszyn wirtualnych — Azure Marketplace
description: Wygeneruj identyfikator URI sygnatury dostępu współdzielonego (SAS) dla wirtualnych dysków twardych (VHD) w witrynie Azure Marketplace.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: article
author: iqshahmicrosoft
ms.author: iqshah
ms.date: 07/29/2020
ms.openlocfilehash: 2bc129fc37347bd108ad62409490c5ce31b7728f
ms.sourcegitcommit: 8def3249f2c216d7b9d96b154eb096640221b6b9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/03/2020
ms.locfileid: "87538935"
---
# <a name="get-shared-access-signature-uri-for-your-vm-image"></a>Uzyskiwanie identyfikatora URI sygnatury dostępu współdzielonego dla obrazu maszyny wirtualnej

W tym artykule opisano sposób generowania Uniform Resource Identifier (SAS) sygnatury dostępu współdzielonego dla każdego wirtualnego dysku twardego (VHD).

Podczas procesu publikowania należy podać identyfikator URI dla każdego wirtualnego dysku twardego skojarzonego z planami (dawniej jednostki SKU). Firma Microsoft potrzebuje dostępu do tych wirtualnych dysków twardych podczas procesu certyfikacji. Ten identyfikator URI zostanie wprowadzony na karcie **plany** w centrum partnerskim.

Podczas generowania identyfikatorów URI sygnatury dostępu współdzielonego dla wirtualnych dysków twardych wykonaj następujące wymagania:

* Obsługiwane są tylko niezarządzane wirtualne dyski twarde.
* Tylko `List` i `Read` uprawnienia są wymagane. Nie zapewniaj dostępu do zapisu ani usuwania.
* Czas trwania dostępu (Data wygaśnięcia) powinien mieć co najmniej trzy tygodnie od momentu utworzenia identyfikatora URI SAS.
* Aby chronić przed zmianami czasu UTC, ustaw datę rozpoczęcia na jeden dzień przed bieżącą datą. Na przykład, jeśli bieżąca data to 6 października 2019, wybierz pozycję 10/5/2019.

## <a name="generate-the-sas-address"></a>Generuj adres SAS

Istnieją dwa popularne narzędzia służące do tworzenia adresu SAS (URL):

* **Eksplorator usługi Microsoft Azure Storage** — narzędzie graficzne dostępne w Azure Portal.
* **Interfejs wiersza polecenia Microsoft Azure** — zalecane dla systemów operacyjnych innych niż Windows oraz zautomatyzowanych lub ciągłych środowisk integracji.

### <a name="use-microsoft-azure-storage-explorer"></a>Użyj Eksplorator usługi Microsoft Azure Storage

1. Przejdź do swojego konta magazynu w Azure Portal.
2. W okienku Eksploratora po lewej stronie Otwórz narzędzie **Eksplorator usługi Storage** (wersja zapoznawcza).
3. Kliknij prawym przyciskiem myszy dysk VHD, a następnie wybierz pozycję **Pobierz sygnaturę dostępu współdzielonego**.
4. Zostanie wyświetlone okno dialogowe **sygnatura dostępu współdzielonego** . Wypełnij następujące pola:

    * **Czas rozpoczęcia** — Data rozpoczęcia uprawnienia dostępu do dysku VHD. Podaj datę, która jest dniem poprzedzającym bieżącą datę.
    * **Czas wygaśnięcia** — Data wygaśnięcia uprawnień dla dostępu do dysku VHD. Podaj datę, która jest co najmniej trzy tygodnie poza bieżącą datą.
    * **Uprawnienia** — wybierz uprawnienia do odczytu i listy.
    * Na **poziomie kontenera** — sprawdź pole wyboru **Generuj identyfikator URI sygnatury dostępu współdzielonego na poziomie kontenera** .

        :::image type="content" source="media/create-sas-uri-storage-explorer.png" alt-text="Ilustruje okno dialogowe sygnatura dostępu współdzielonego":::

5. Aby utworzyć skojarzony identyfikator URI sygnatury dostępu współdzielonego dla tego wirtualnego dysku twardego, wybierz pozycję **Utwórz**. Okno dialogowe odświeża i pokazuje szczegółowe informacje o tej operacji.
6. Skopiuj **Identyfikator URI** i Zapisz go w pliku tekstowym w bezpiecznej lokalizacji.

    :::image type="content" source="media/create-sas-uri-shared-access-signature-details.png" alt-text="Ilustruje pole szczegóły sygnatury dostępu współdzielonego":::
7. Powtórz te kroki dla każdego wirtualnego dysku twardego w planach, które będą publikowane.

### <a name="using-azure-cli"></a>Korzystanie z interfejsu wiersza polecenia platformy Azure

1. Pobierz i zainstaluj [interfejs wiersza polecenia Microsoft Azure](https://azure.microsoft.com/documentation/articles/xplat-cli-install/). Wersje są dostępne dla systemów Windows, macOS i różnych dystrybucje systemu Linux.
2. Utwórz plik programu PowerShell (rozszerzenie pliku ps1), Skopiuj poniższy kod, a następnie zapisz go lokalnie.

    ```PowerShell
    az storage container generate-sas --connection-string 'DefaultEndpointsProtocol=https;AccountName=<account-name>;AccountKey=<account-key>;EndpointSuffix=core.windows.net' --name <vhd-name> --permissions rl --start '<start-date>' --expiry '<expiry-date>'
    ```

3. Edytuj plik, aby użyć następujących wartości parametrów. Podaj daty w formacie daty/godziny UTC, np `2020-04-01T00:00:00Z` ..

    * `<account-name>`— Nazwa konta usługi Azure Storage
    * `<account-key>`— Klucz konta usługi Azure Storage
    * `<vhd-name>`— Nazwa wirtualnego dysku twardego
    * `<start-date>`— Data rozpoczęcia uprawnienia dostępu do dysku VHD. Podaj datę o jeden dzień przed bieżącą datą.
    * `<expiry-date>`— Data wygaśnięcia uprawnień dla dostępu do dysku VHD. Podaj datę z co najmniej trzech tygodni od bieżącej daty.

    Ten przykład przedstawia poprawne wartości parametrów (w czasie pisania):

    ```PowerShell
    az storage container generate-sas --connection-string 'DefaultEndpointsProtocol=https;AccountName=st00009;AccountKey=6L7OWFrlabs7Jn23OaR3rvY5RykpLCNHJhxsbn9ONc+bkCq9z/VNUPNYZRKoEV1FXSrvhqq3aMIDI7N3bSSvPg==;EndpointSuffix=core.windows.net' --name vhds --permissions rl --start '2020-04-01T00:00:00Z' --expiry '2021-04-01T00:00:00Z'
    ```

4. Zapisz zmiany.
5. Korzystając z jednej z następujących metod, Uruchom ten skrypt z uprawnieniami administracyjnymi, aby utworzyć **Parametry połączenia sygnatury dostępu współdzielonego** na poziomie kontenera:

    * Uruchom skrypt z konsoli programu. W systemie Windows kliknij prawym przyciskiem myszy skrypt, a następnie wybierz polecenie **Uruchom jako administrator**.
    * Uruchom skrypt z edytora skryptów programu PowerShell, takiego jak [Windows PowerShell ISE](https://docs.microsoft.com/powershell/scripting/components/ise/introducing-the-windows-powershell-ise). Ten ekran przedstawia tworzenie parametrów połączenia sygnatury dostępu współdzielonego w ramach tego edytora:

     :::image type="content" source="media/create-sas-uri-power-shell-ise.png" alt-text="Ilustruje tworzenie parametrów połączenia sygnatury dostępu współdzielonego za pomocą Windows PowerShell ISE":::

6. Skopiuj parametry połączenia sygnatury dostępu współdzielonego i Zapisz je w pliku tekstowym w bezpiecznej lokalizacji. Edytuj ten ciąg, aby dodać informacje o lokalizacji wirtualnego dysku twardego w celu utworzenia końcowego identyfikatora URI sygnatury dostępu współdzielonego.
7. W Azure Portal przejdź do magazynu obiektów blob, który zawiera dysk VHD skojarzony z nowym identyfikatorem URI.
8. Skopiuj adres URL **BLOB Service punktu końcowego**, jak pokazano na poniższym zrzucie ekranu

    :::image type="content" source="media/create-sas-uri-blob-endpoint.png" alt-text="Ilustruje Blob service punkt końcowy":::

9. Edytuj plik tekstowy przy użyciu parametrów połączenia sygnatury dostępu współdzielonego z kroku 6. Utwórz pełny identyfikator URI sygnatury dostępu współdzielonego przy użyciu tego formatu:

    `<blob-service-endpoint-url> + /vhds/ + <vhd-name>? + <sas-connection-string>`

    Jeśli na przykład nazwa wirtualnego dysku twardego to `TestRGVM2.vhd` , identyfikator URI sygnatury dostępu współdzielonego będzie:

    `https://catech123.blob.core.windows.net/vhds/TestRGVM2.vhd?st=2018-05-06T07%3A00%3A00Z&se=2019-08-02T07%3A00%3A00Z&sp=rl&sv=2017-04-17&sr=c&sig=wnEw9RfVKeSmVgqDfsDvC9IHhis4x0fc9Hu%2FW4yvBxk%3D`

Powtórz te kroki dla każdego wirtualnego dysku twardego w planach, które będą publikowane.

## <a name="verify-the-sas-uri"></a>Weryfikowanie identyfikatora URI sygnatury dostępu współdzielonego

Przejrzyj każdy utworzony identyfikator URI SYGNATURy dostępu współdzielonego za pomocą poniższej listy kontrolnej, aby sprawdzić, czy:

* Identyfikator URI wygląda następująco:`<blob-service-endpoint-url>` + `/vhds/` + `<vhd-name>?` + `<sas-connection-string>`
* Identyfikator URI zawiera nazwę pliku obrazu VHD, w tym rozszerzenie nazwy pliku ". VHD".
* `sp=rl`pojawia się blisko środka identyfikatora URI. Ten ciąg pokazuje, że `Read` i `List` jest określony dostęp.
* Gdy jest `sr=c` wyświetlany, oznacza to, że jest określony dostęp na poziomie kontenera.
* Skopiuj i wklej identyfikator URI do przeglądarki w celu przetestowania i pobrania obiektu BLOB (możesz anulować operację przed ukończeniem pobierania).

## <a name="next-step"></a>Następny krok

Jeśli masz problemy z tworzeniem identyfikatora URI sygnatury dostępu współdzielonego, zobacz [typowe problemy dotyczące adresów URL SAS](common-sas-uri-issues.md). W przeciwnym razie Zapisz identyfikatory URI sygnatury dostępu współdzielonego w bezpiecznej lokalizacji do późniejszego użycia. Będzie ona potrzebna do opublikowania oferty maszyny wirtualnej w centrum partnerskim.

* [Tworzenie oferty maszyny wirtualnej platformy Azure](azure-vm-create-offer.md)
