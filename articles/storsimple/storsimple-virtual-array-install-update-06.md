---
title: Zainstaluj aktualizację 0,6 w macierzy wirtualnej StorSimple | Microsoft Docs
description: Opisuje sposób użycia interfejsu użytkownika sieci Web macierzy wirtualnej StorSimple do zastosowania aktualizacji przy użyciu metody Azure Portal i poprawki
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: how-to
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 05/18/2017
ms.author: alkohli
ms.openlocfilehash: 02b85cb90948f35cb6f6c855cfbe81fd58301de0
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "85513588"
---
# <a name="install-update-06-on-your-storsimple-virtual-array"></a>Zainstaluj aktualizację 0,6 w macierzy wirtualnej StorSimple

## <a name="overview"></a>Omówienie

W tym artykule opisano kroki wymagane do zainstalowania aktualizacji 0,6 w macierzy wirtualnej StorSimple za pomocą lokalnego interfejsu użytkownika sieci Web i za pośrednictwem Azure Portal. Aby zapewnić aktualność macierzy wirtualnej StorSimple, należy zastosować aktualizacje lub poprawki oprogramowania.

Przed zastosowaniem aktualizacji zaleca się, aby najpierw przełączyć woluminy lub udziały w tryb offline na hoście, a następnie na urządzeniu. Minimalizuje to ryzyko uszkodzenia danych. Gdy woluminy lub udziały są w trybie offline, należy również ręcznie wykonać kopię zapasową urządzenia.

> [!IMPORTANT]
>
> - Aktualizacja 0,6 jest zgodna z wersją oprogramowania **10.0.10293.0** na urządzeniu. Aby uzyskać informacje na temat Nowości w tej aktualizacji, przejdź do informacji o [wersji aktualizacji 0,6](storsimple-virtual-array-update-06-release-notes.md).
>
> - W przypadku korzystania z aktualizacji 0,2 lub nowszej zaleca się zainstalowanie aktualizacji za pomocą Azure Portal. W przypadku korzystania z aktualizacji 0,1 lub GA wersji oprogramowania należy użyć metody poprawek za pomocą lokalnego interfejsu użytkownika sieci Web, aby zainstalować aktualizację 0,6.
>
> - Należy pamiętać, że zainstalowanie aktualizacji lub poprawki powoduje ponowne uruchomienie urządzenia. W przypadku, gdy Macierz wirtualna StorSimple jest urządzeniem z jednym węzłem, wszystkie trwające operacji we/wy są zakłócane, a urządzenie nie będzie miało przestoju.

## <a name="use-the-azure-portal"></a>Korzystanie z witryny Azure Portal

W przypadku korzystania z aktualizacji 0,2 i nowszych zaleca się zainstalowanie aktualizacji za pomocą Azure Portal. Procedura portalu wymaga od użytkownika skanowania, pobrania i zainstalowania aktualizacji. Wykonanie tej procedury zajmuje około 7 minut. Wykonaj następujące kroki, aby zainstalować aktualizację lub poprawkę.

[!INCLUDE [storsimple-virtual-array-install-update-via-portal](../../includes/storsimple-virtual-array-install-update-via-portal-04.md)]

Po zakończeniu instalacji przejdź do usługi StorSimple Menedżer urządzeń. Wybierz pozycję **urządzenia** , a następnie wybierz i kliknij urządzenie, które właśnie zostało zaktualizowane. Przejdź do pozycji **Ustawienia, > zarządzanie aktualizacjami > urządzeń**. Wyświetlana wersja oprogramowania powinna być **10.0.10293.0**.

## <a name="use-the-local-web-ui"></a>Korzystanie z lokalnego interfejsu użytkownika sieci Web

W przypadku korzystania z lokalnego interfejsu użytkownika sieci Web należy wykonać dwa kroki:

* Pobierz aktualizację lub poprawkę
* Zainstaluj aktualizację lub poprawkę

### <a name="download-the-update-or-the-hotfix"></a>Pobierz aktualizację lub poprawkę

Wykonaj następujące kroki, aby pobrać aktualizację oprogramowania z Wykazu usługi Microsoft Update.

#### <a name="to-download-the-update-or-the-hotfix"></a>Aby pobrać aktualizację lub poprawkę

1. Uruchom program Internet Explorer i przejdź do [https://catalog.update.microsoft.com](https://catalog.update.microsoft.com) .

2. Jeśli używasz wykazu Microsoft Update po raz pierwszy na tym komputerze, kliknij przycisk **Zainstaluj** po wyświetleniu monitu, aby zainstalować dodatek katalogu Microsoft Update.

3. W polu wyszukiwania w wykazie Microsoft Update wprowadź numer bazy wiedzy (KB) poprawki, którą chcesz pobrać. Wprowadź **4023268** dla aktualizacji 0,6, a następnie kliknij przycisk **Wyszukaj**.
   
    Zostanie wyświetlona lista poprawek, na przykład **StorSimple Virtual Array Update 0,6**.
   
    ![Przeszukiwanie wykazu](./media/storsimple-virtual-array-install-update-06/download1.png)

4. Kliknij pozycję **Pobierz**.

5. Powinno zostać wyświetlone pięć plików do pobrania. Pobierz wszystkie te pliki do folderu. Folder można też skopiować do udziału sieciowego osiągalnego z urządzenia.

6. Otwórz folder, w którym znajdują się pliki.
    ![Pliki w pakiecie](./media/storsimple-virtual-array-install-update-06/update06folder.png)

    Zobaczysz:
    -  Microsoft Update autonomiczny plik pakietu `WindowsTH-KB3011067-x64` . Ten plik jest używany do aktualizacji oprogramowania urządzenia.
    - Plik pakietu agenta monitorowania Genewa `GenevaMonitoringAgentPackageInstaller` . Ten plik jest używany do aktualizowania agenta usług monitorowania i diagnostyki. Kliknij dwukrotnie plik cab. Zostanie wyświetlony plik _MSI_ . Wybierz plik, kliknij prawym przyciskiem myszy, a następnie **Wyodrębnij** plik. Aby zaktualizować agenta, należy użyć pliku _. msi_ .

        ![Wyodrębnij plik aktualizacji agenta usług MDS](./media/storsimple-virtual-array-install-update-06/extract-geneva-monitoring-agent-installer.png)

        > [!IMPORTANT]
        > Nie trzeba aktualizować agenta usług MDS, jeśli jest używany program StorSimple Update 0,5 (0.0.10293.0).

    - Trzy pliki zawierające krytyczne aktualizacje zabezpieczeń systemu Windows, `windows8.1-kb4012213-x64` , `windows8.1-kb3205400-x64` i `windows8.1-kb4019213-x64` .


### <a name="install-the-update-or-the-hotfix"></a>Zainstaluj aktualizację lub poprawkę

Przed zainstalowaniem aktualizacji lub poprawki upewnij się, że masz aktualizację lub poprawkę pobraną lokalnie na hoście lub dostępną za pośrednictwem udziału sieciowego.

Ta metoda służy do instalowania aktualizacji na urządzeniu z oprogramowaniem w wersji GA lub Update 0,1. Wykonanie tej procedury trwa około 12 minut. Wykonaj następujące kroki, aby zainstalować aktualizację lub poprawkę.

#### <a name="to-install-the-update-or-the-hotfix"></a>Aby zainstalować aktualizację lub poprawkę

1. W lokalnym interfejsie użytkownika sieci Web przejdź do pozycji **konserwacja**  >  **Aktualizowanie oprogramowania**. Zanotuj wersję oprogramowania, która jest uruchamiana. W przypadku korzystania z programu **10.0.10290.0**nie trzeba aktualizować agenta usług MDS w kroku 6.
   
    ![aktualizowanie urządzenia](./media/storsimple-virtual-array-install-update-05/update1m.png)

2. W polu **ścieżka pliku aktualizacji**wprowadź nazwę pliku aktualizacji lub poprawki. Możesz również przejść do pliku instalacyjnego aktualizacji lub poprawek, jeśli znajduje się on w udziale sieciowym. Kliknij przycisk **Zastosuj**.
   
    ![aktualizowanie urządzenia](./media/storsimple-virtual-array-install-update-05/update2m.png)

3. Wyświetlane jest ostrzeżenie. Po zastosowaniu aktualizacji Macierz wirtualna jest urządzeniem z jednym węzłem, a następnie następuje ponowne uruchomienie urządzenia i wystąpiła przestoje. Kliknij ikonę zaznaczania.
   
   ![aktualizowanie urządzenia](./media/storsimple-virtual-array-install-update-05/update3m.png)

4. Rozpocznie się aktualizacja. Po pomyślnym zaktualizowaniu urządzenia zostanie ono ponownie uruchomione. Lokalny interfejs użytkownika nie jest dostępny w tym czasie trwania.
   
    ![aktualizowanie urządzenia](./media/storsimple-virtual-array-install-update-05/update5m.png)

5. Po ponownym uruchomieniu nastąpi przekierowanie do strony **logowania** . Aby sprawdzić, czy oprogramowanie urządzenia zostało zaktualizowane, w lokalnym interfejsie użytkownika sieci Web przejdź do pozycji **konserwacja**  >  **Aktualizowanie oprogramowania**. Wyświetlana wersja oprogramowania powinna być **10.0.0.0.0.10293** dla aktualizacji 0,6.
   
   > [!NOTE]
   > Firma Microsoft zgłasza wersje oprogramowania w nieco inny sposób w lokalnym interfejsie użytkownika sieci Web i Azure Portal. Na przykład lokalny interfejs użytkownika sieci Web raportuje **10.0.0.0.0.10293** oraz Azure Portal raporty **10.0.10293.0** dla tej samej wersji.
   
    ![aktualizowanie urządzenia](./media/storsimple-virtual-array-install-update-06/update6m.png)

6. Pomiń ten krok, jeśli przed zainstalowaniem tej aktualizacji StorSimple wirtualną macierzą 0,5 (**10.0.10290.0**). Zawarto zanotować wersję oprogramowania w kroku 1 przed rozpoczęciem aktualizacji. W przypadku korzystania z aktualizacji 0,5 agent usług MDS jest już aktualny.

    W przypadku korzystania z wersji oprogramowania przed aktualizacją 0,5, następnym krokiem dla Ciebie jest aktualizacja agenta usług MDS. Na stronie **Aktualizacja oprogramowania** przejdź do **ścieżki pliku aktualizacji** i przejdź do `GenevaMonitoringAgentPackageInstaller.msi` pliku. Powtórz kroki 2-4. Po ponownym uruchomieniu macierzy wirtualnej Zaloguj się do lokalnego interfejsu użytkownika sieci Web.

7. Powtórz krok 2-4, aby zainstalować poprawki zabezpieczeń systemu Windows przy użyciu plików `windows8.1-kb4012213-x64` , `windows8.1-kb3205400-x64` i `windows8.1-kb4019213-x64` . Macierz wirtualna jest uruchamiana ponownie po każdej instalacji i należy zalogować się do lokalnego interfejsu użytkownika sieci Web.

## <a name="next-steps"></a>Następne kroki

Dowiedz się więcej [na temat administrowania wirtualną macierzą StorSimple](storsimple-ova-web-ui-admin.md).

