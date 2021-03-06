---
title: Ocenianie maszyn wirtualnych VMware za pomocą oceny serwera Azure Migrate
description: Opisuje sposób oceny lokalnych maszyn wirtualnych VMware na potrzeby migracji na platformę Azure przy użyciu oceny serwera Azure Migrate.
ms.topic: tutorial
ms.date: 06/03/2020
ms.custom: mvc
ms.openlocfilehash: dd00f800003724b3a5c15d265a5428272e1762fb
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/28/2020
ms.locfileid: "87290213"
---
# <a name="assess-vmware-vms-with-server-assessment"></a>Ocenianie maszyn wirtualnych programu VMware przy użyciu narzędzia do oceny serwera

W tym artykule opisano sposób oceny lokalnych maszyn wirtualnych VMware przy użyciu [Azure Migrate: narzędzia do oceny serwera](migrate-services-overview.md#azure-migrate-server-assessment-tool) .


Ten samouczek jest drugą częścią serii, która pokazuje, jak oceniać i migrować maszyny wirtualne VMware na platformę Azure. Ten samouczek zawiera informacje na temat wykonywania następujących czynności:
> [!div class="checklist"]
> * Skonfiguruj projekt Azure Migrate.
> * Skonfiguruj urządzenie Azure Migrate uruchamiane lokalnie w celu oceny maszyn wirtualnych.
> * Rozpocznij ciągłe wykrywanie lokalnych maszyn wirtualnych. Urządzenie wysyła dane dotyczące konfiguracji i wydajności dla odnalezionych maszyn wirtualnych na platformę Azure.
> * Grupuj odnalezione maszyny wirtualne i Oceń grupę maszyn wirtualnych.
> * Przejrzyj ocenę.

> [!NOTE]
> Samouczki przedstawiają najprostszą ścieżkę wdrożenia dla scenariusza, dzięki czemu można szybko skonfigurować weryfikację koncepcji. Samouczki korzystają z domyślnych opcji, jeśli jest to możliwe, i nie wyświetlają wszystkich możliwych ustawień i ścieżek. Aby uzyskać szczegółowe instrukcje, zapoznaj się z artykułami z instrukcjami.

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/pricing/free-trial/).

## <a name="prerequisites"></a>Wymagania wstępne

- [Wykonaj pierwszy samouczek](tutorial-prepare-vmware.md) z tej serii. Jeśli tego nie zrobisz, instrukcje podane w tym samouczku nie będą działały.
- Oto co należy zrobić w pierwszym samouczku:
    - [Przygotuj platformę Azure](tutorial-prepare-vmware.md#prepare-azure) do pracy z Azure Migrate.
    - [Przygotuj oprogramowanie VMware do oceny](tutorial-prepare-vmware.md#prepare-for-assessment) w celu oceny. Obejmuje to sprawdzanie ustawień programu VMware, skonfigurowanie konta, za pomocą którego Azure Migrate może uzyskać dostęp do vCenter Server.
    - [Sprawdź](tutorial-prepare-vmware.md#verify-appliance-settings-for-assessment) , co jest potrzebne, aby wdrożyć urządzenie Azure Migrate na potrzeby oceny oprogramowania VMware.

## <a name="set-up-an-azure-migrate-project"></a>Konfigurowanie projektu Azure Migrate

Skonfiguruj nowy projekt Azure Migrate w następujący sposób:

1. W witrynie Azure Portal > **Wszystkie usługi** znajdź pozycję **Azure Migrate**.
2. W obszarze **Usługi** wybierz pozycję **Azure Migrate**.
3. W obszarze **Przegląd**w obszarze **odnajdywanie, ocenianie i Migrowanie serwerów**wybierz pozycję **Oceń i Przeprowadź migrację serwerów**.

   ![Przycisk umożliwiający ocenę i migrację serwerów](./media/tutorial-assess-vmware/assess-migrate.png)

4. W obszarze **wprowadzenie**wybierz pozycję **Dodaj narzędzia**.
5. W obszarze **Projekt migracji**wybierz subskrypcję platformy Azure i utwórz grupę zasobów, jeśli jej nie masz.     
6. W obszarze **szczegóły projektu**Określ nazwę projektu i geografię, w której chcesz utworzyć projekt. Przejrzyj obsługiwane lokalizacje geograficzne dla chmur [publicznych](migrate-support-matrix.md#supported-geographies-public-cloud) i [instytucji rządowych](migrate-support-matrix.md#supported-geographies-azure-government).

   ![Pola nazwy i regionu projektu](./media/tutorial-assess-vmware/migrate-project.png)

7. Wybierz pozycję **Dalej**.
8. W **narzędziu Wybierz ocenę**wybierz pozycję **Azure Migrate: Ocena serwera**  >  **dalej**.

   ![Wybór dla narzędzia do oceny serwera](./media/tutorial-assess-vmware/assessment-tool.png)

9. W obszarze **Wybierz narzędzie migracji** wybierz pozycję **Pomiń na razie dodawanie narzędzia migracji** > **Dalej**.
10. W oknie **Recenzja + Dodawanie narzędzi**przejrzyj ustawienia, a następnie wybierz pozycję **Dodaj narzędzia**.
11. Zaczekaj kilka minut, aż projekt usługi Azure Migrate zostanie wdrożony. Nastąpi przekierowanie do strony projektu. Jeśli nie widzisz projektu, możesz uzyskać do niego dostęp z obszaru **Serwery** na pulpicie nawigacyjnym usługi Azure Migrate.

## <a name="set-up-the-azure-migrate-appliance"></a>Konfigurowanie urządzenia Azure Migrate

Azure Migrate: Ocena serwera używa urządzenia uproszczonego Azure Migrate. Urządzenie wykonuje odnajdywanie maszyn wirtualnych i wysyła metadane maszyny wirtualnej i dane wydajności do Azure Migrate. Urządzenie można skonfigurować na wiele sposobów.

- Skonfiguruj na maszynie wirtualnej VMware przy użyciu pobranego szablonu komórki jajowe. Ta metoda jest używana w tym samouczku.
- Skonfiguruj na maszynie wirtualnej VMware lub na komputerze fizycznym za pomocą skryptu Instalatora programu PowerShell. [Tej metody](deploy-appliance-script.md) należy użyć, jeśli nie można skonfigurować maszyny wirtualnej przy użyciu szablonu komórki jajowe lub jeśli jesteś w Azure Government.

Po utworzeniu urządzenia sprawdź, czy może nawiązać połączenie z Azure Migrate: Ocena serwera, skonfiguruj ją po raz pierwszy i zarejestruj ją za pomocą projektu Azure Migrate.


### <a name="download-the-ova-template"></a>Pobierz szablon komórki jajowe

1. W obszarze serwery **celów migracji**  >  **Servers**  >  **Azure Migrate: Ocena serwera**, wybierz pozycję **odkryj**.
2. W obszarze **odnajdywanie**maszyn  >  **są zwirtualizowane maszyny?** wybierz pozycję **tak, aby uzyskać VMware vSphere funkcji hypervisor**.
3. Wybierz pozycję **Pobierz** , aby pobrać plik szablonu komórki jajowe.

   ![Wybrane do pobrania plik komórki jajowe](./media/tutorial-assess-vmware/download-ova.png)

### <a name="verify-security"></a>Weryfikuj zabezpieczenia

Przed wdrożeniem należy sprawdzić, czy plik komórki jajowe jest bezpieczny:

1. Na maszynie, na którą pobrano plik, otwórz okno wiersza polecenia administratora.
2. Uruchom następujące polecenie, aby wygenerować skrót dla pliku komórki jajowe:
  
   ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
   
   Przykład użycia: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```

3. Sprawdź najnowsze wersje urządzeń i wartości skrótu:

    - W przypadku chmury publicznej platformy Azure:
    
        **Algorytm** | **Pobieranie** | **SHA256**
        --- | --- | ---
        VMware (10,9 GB) | [Najnowsza wersja](https://aka.ms/migrate/appliance/vmware) | cacbdaef927fe5477fa4e1f494fcb7203cbd6b6ce7402b79f234bc0fe69663dd

    - Dla Azure Government:
    
        **Algorytm** | **Pobieranie** | **SHA256**
        --- | --- | ---
        VMware (63,1 MB) | [Najnowsza wersja](https://go.microsoft.com/fwlink/?linkid=2120300&clcid=0x409 ) | 3d5822038646b81f458d89d706832c0a2c0e827bfa9b0a55cc478eaf2757a4de


### <a name="create-the-appliance-vm"></a>Tworzenie maszyny wirtualnej urządzenia

Zaimportuj pobrany plik i Utwórz maszynę wirtualną:

1. W konsoli klienta vSphere wybierz pozycję **plik**  >  **Wdróż OVF szablon**.

   ![Polecenie menu do wdrażania szablonu OVF](./media/tutorial-assess-vmware/deploy-ovf.png)

2. W Kreatorze wdrażania szablonu OVF > **Źródło**Określ lokalizację pliku komórek jajowych.
3. W polu **Nazwa** i **Lokalizacja**Określ przyjazną nazwę maszyny wirtualnej. Wybierz obiekt spisu, w którym będzie hostowana maszyna wirtualna.
4. W obszarze **host/klaster**Określ hosta lub klaster, na którym będzie URUCHAMIANA maszyna wirtualna.
5. W obszarze **Magazyn**określ miejsce docelowe magazynu dla maszyny wirtualnej.
6. W obszarze **Disk Format** (Format dysku) określ typ i rozmiar dysku.
7. W polu **mapowanie sieci**Określ sieć, z którą zostanie nawiązane połączenie z maszyną wirtualną. Sieć wymaga łączności z Internetem w celu wysyłania metadanych do oceny serwera Azure Migrate.
8. Przejrzyj i Potwierdź ustawienia, a następnie wybierz pozycję **Zakończ**.

## <a name="verify-appliance-access-to-azure"></a>Weryfikowanie dostępu urządzenia do platformy Azure

Upewnij się, że maszyna wirtualna urządzenia może połączyć się z adresami URL platformy Azure dla chmur [publicznych](migrate-appliance.md#public-cloud-urls) i dla [instytucji rządowych](migrate-appliance.md#government-cloud-urls) .

### <a name="configure-the-appliance"></a>Konfigurowanie urządzenia

Skonfiguruj urządzenie po raz pierwszy.

> [!NOTE]
> Jeśli urządzenie zostanie skonfigurowane przy użyciu [skryptu programu PowerShell](deploy-appliance-script.md) zamiast pobranych komórek jajowych, pierwsze dwa kroki tej procedury nie są istotne.

1. W konsoli klienta vSphere kliknij prawym przyciskiem myszy maszynę wirtualną, a następnie wybierz polecenie **Otwórz konsolę**.
2. Podaj język, strefę czasową i hasło dla urządzenia.
3. Otwórz przeglądarkę na dowolnym komputerze, który może nawiązać połączenie z maszyną wirtualną, a następnie otwórz adres URL aplikacji sieci Web urządzenia: **https://*Nazwa urządzenia lub adres IP*: 44368**.

   Możesz też otworzyć aplikację na pulpicie urządzenia, wybierając skrót do aplikacji.
1. W aplikacji internetowej > **skonfigurować wymagania wstępne**, wykonaj następujące czynności:
   - **Licencja**: zaakceptuj postanowienia licencyjne i przeczytaj informacje o innych firmach.
   - **Łączność**: aplikacja sprawdza, czy maszyna wirtualna ma dostęp do Internetu. Jeśli maszyna wirtualna używa serwera proxy:
     - Wybierz pozycję **Ustawienia serwera proxy**i określ adres serwera proxy i port nasłuchujący w http://ProxyIPAddress formularzu http://ProxyFQDN lub.
     - Jeśli serwer proxy wymaga uwierzytelnienia, wprowadź poświadczenia.
     - Obsługiwane są tylko serwery proxy HTTP.
   - **Synchronizacja czasu**: czas na urządzeniu powinien być zsynchronizowany z czasem Internetu, aby odnajdywanie działało prawidłowo.
   - **Zainstaluj aktualizacje**: urządzenie zapewnia zainstalowanie najnowszych aktualizacji.
   - **Zainstaluj VDDK**: Urządzenie sprawdza, czy zainstalowano pakiet VMware vSphere Virtual Disk Development Kit (VDDK). Jeśli nie jest zainstalowany, Pobierz VDDK 6,7 z programu VMware i Wyodrębnij zawartość pliku zip do określonej lokalizacji na urządzeniu.

     Migracja serwera Azure Migrate przy użyciu programu VDDK umożliwia replikowanie maszyn podczas migracji na platformę Azure.       

### <a name="register-the-appliance-with-azure-migrate"></a>Zarejestruj urządzenie w Azure Migrate

1. Wybierz pozycję **Zaloguj**. Jeśli ta wartość nie jest wyświetlana, upewnij się, że w przeglądarce wyłączono blokowanie wyskakujących okienek.
2. Na nowej karcie Zaloguj się przy użyciu nazwy użytkownika i hasła platformy Azure.
   
   Logowanie przy użyciu numeru PIN nie jest obsługiwane.
3. Po pomyślnym zalogowaniu Wróć do aplikacji sieci Web.
4. Wybierz subskrypcję, w której został utworzony projekt Azure Migrate, a następnie wybierz projekt.
5. Określ nazwę urządzenia. Nazwa powinna być alfanumeryczna z 14 znakami lub mniej.
6. Wybierz pozycję **Rejestruj**.


## <a name="start-continuous-discovery"></a>Uruchom odnajdywanie ciągłe

Urządzenie musi połączyć się z vCenter Server, aby odnaleźć dane dotyczące konfiguracji i wydajności maszyn wirtualnych.

### <a name="specify-vcenter-server-details"></a>Określanie szczegółów programu vCenter Server
1. W obszarze **określ vCenter Server Szczegóły**Określ nazwę (FQDN) lub adres IP wystąpienia vCenter Server. Można pozostawić port domyślny lub określić port niestandardowy, dla którego vCenter Server nasłuchuje.
2. W polu **Nazwa użytkownika** i **hasło**określ poświadczenia konta vCenter Server, które będą używane przez urządzenie do odnajdywania maszyn wirtualnych w wystąpieniu vCenter Server. 

    - Należy skonfigurować konto z uprawnieniami wymaganymi w [poprzednim samouczku](tutorial-prepare-vmware.md#set-up-permissions-for-assessment).
    - Jeśli chcesz przeznaczyć zakres odnajdywania do określonych obiektów VMware (vCenter Server centrach danych, klastrów, folderu klastrów, hostów, folderu hostów lub poszczególnych maszyn wirtualnych), zapoznaj się z instrukcjami w [tym artykule](set-discovery-scope.md) , aby ograniczyć konto używane przez Azure Migrate.

3. Wybierz pozycję **Weryfikuj połączenie** , aby upewnić się, że urządzenie może połączyć się z vCenter Server.
4. W obszarze **Znajdź aplikacje i zależności na maszynach wirtualnych**Opcjonalnie kliknij pozycję **Dodaj poświadczenia**i określ system operacyjny, dla którego mają zastosowanie poświadczenia, oraz nazwę użytkownika i hasło poświadczenia. Następnie kliknij przycisk **Dodaj**.

    - Opcjonalnie możesz dodać poświadczenia tutaj, jeśli utworzono konto do użycia dla [funkcji odnajdywania aplikacji](how-to-discover-applications.md)lub [Funkcja analizy zależności bez agenta](how-to-create-group-machine-dependencies-agentless.md).
    - Jeśli nie używasz tych funkcji, możesz pominąć to ustawienie.
    - Przejrzyj poświadczenia potrzebne do [odnajdowania aplikacji](migrate-support-matrix-vmware.md#application-discovery-requirements)lub [analizy bez agentów](migrate-support-matrix-vmware.md#dependency-analysis-requirements-agentless).

5. **Zapisz i Rozpocznij odnajdywanie**, aby uruchomić odnajdywanie maszyn wirtualnych.

Odnajdywanie działa w następujący sposób:
- Odnalezienie metadanych maszyny wirtualnej w portalu może potrwać około 15 minut.
- Odnajdywanie zainstalowanych aplikacji, ról i funkcji zajmuje trochę czasu. Czas trwania zależy od liczby wykrytych maszyn wirtualnych. W przypadku maszyn wirtualnych 500 ilość spisu aplikacji do wyświetlenia w portalu Azure Migrate trwa około godzinę.

### <a name="verify-vms-in-the-portal"></a>Weryfikowanie maszyn wirtualnych w portalu

Po przeprowadzeniu odnajdywania możesz sprawdzić, czy maszyny wirtualne są wyświetlane w Azure Portal:

1. Otwórz pulpit nawigacyjny Azure Migrate.
2. W **Azure Migrate serwery**  >  **Azure Migrate: Ocena serwera**, wybierz ikonę, która wyświetla liczbę **odnalezionych serwerów**.

## <a name="set-up-an-assessment"></a>Konfigurowanie oceny

Za pomocą oceny serwera Azure Migrate można utworzyć dwa typy ocen:

**Typ oceny** | **Szczegóły**
--- | --- 
**Maszyna wirtualna platformy Azure** | Ocenianie migracji serwerów lokalnych do usługi Azure Virtual Machines. <br/><br/> Możesz ocenić lokalne [maszyny wirtualne VMware](how-to-set-up-appliance-vmware.md), [maszyny wirtualne funkcji Hyper-V](how-to-set-up-appliance-hyper-v.md)i [serwery fizyczne](how-to-set-up-appliance-physical.md) do migracji na platformę Azure przy użyciu tego typu oceny. [Dowiedz się więcej](concepts-assessment-calculation.md)
**Rozwiązanie Azure VMware (AVS)** | Ocenianie migracji serwerów lokalnych do [rozwiązania Azure VMware (Automatyczna synchronizacja)](../azure-vmware/introduction.md). <br/><br/> Za pomocą tego typu oceny można ocenić lokalne [maszyny wirtualne VMware](how-to-set-up-appliance-vmware.md) na potrzeby migracji do rozwiązania Azure VMware (Automatyczna synchronizacja). [Dowiedz się więcej](concepts-azure-vmware-solution-assessment-calculation.md)

Ocena serwera oferuje dwie opcje kryteriów ustalania rozmiarów:

**Kryteria ustalania wielkości** | **Szczegóły** | **Dane**
--- | --- | ---
**Oparta na wydajności** | Oceny, które podejmują zalecenia na podstawie zebranych danych wydajności | **Ocena maszyny wirtualnej platformy Azure**: zalecenie dotyczące rozmiaru maszyny wirtualnej bazuje na danych użycia procesora i pamięci.<br/><br/> Zalecenia dotyczące typu dysku (standardowy dysk twardy/SSD lub dyski zarządzane w warstwie Premium) jest oparty na liczbie operacji we/wy na sekundę i przepływności dysku lokalnego.<br/><br/> **Ocena rozwiązań VMware na platformie Azure (Automatyczna synchronizacja)**: zalecenia dotyczące synchronizacji węzłów są oparte na danych użycia procesora i pamięci.
**Zgodnie z lokalnym** | Oceny, które nie wykorzystują danych wydajności, aby wykonać zalecenia. | **Ocena maszyny wirtualnej platformy Azure**: zalecenie dotyczące rozmiaru maszyny wirtualnej bazuje na lokalnym rozmiarze maszyny wirtualnej<br/><br> Zalecany typ dysku jest określany na podstawie tego, co zostało wybrane w ustawieniu typ magazynu dla oceny.<br/><br/> **Ocena rozwiązań VMware na platformie Azure (Automatyczna synchronizacja)**: zalecenia dotyczące synchronizacji węzłów zależą od lokalnego rozmiaru maszyny wirtualnej.

## <a name="run-an-assessment"></a>Uruchamianie oceny

Wykonaj *ocenę maszyny wirtualnej platformy Azure* w następujący sposób:

1. Zapoznaj się z [najlepszymi rozwiązaniami](best-practices-assessment.md) dotyczącymi tworzenia ocen.
2. Na karcie **serwery** w kafelku **Azure Migrate: Ocena serwera** wybierz pozycję **Oceń**.

   ![Lokalizacja przycisku oceny](./media/tutorial-assess-vmware/assess.png)

3. W obszarze **ocenianie serwerów**wybierz typ oceny jako "maszyna wirtualna platformy Azure", wybierz źródło odnajdywania i określ nazwę oceny.

    ![Podstawowe informacje o ocenie](./media/tutorial-assess-vmware/assess-servers-azurevm.png)
 
4. Wybierz pozycję **Wyświetl wszystko**, a następnie przejrzyj właściwości oceny.

   ![Właściwości oceny](./media/tutorial-assess-vmware/view-all.png)

5. Kliknij przycisk **dalej** , aby **wybrać maszyny do oceny**. W obszarze **Wybierz lub Utwórz grupę**wybierz pozycję **Utwórz nową**, a następnie określ nazwę grupy. Grupa zbiera co najmniej jedną maszynę wirtualną w celu oceny.
6. W obszarze **Dodawanie maszyn do grupy**Wybierz Maszyny wirtualne, które mają zostać dodane do grupy.
7. Kliknij przycisk **dalej** , aby **przejrzeć i utworzyć ocenę** , aby przejrzeć szczegóły oceny.
8. Wybierz pozycję **Utwórz ocenę** , aby utworzyć grupę i uruchomić ocenę.

   ![Ocenianie serwerów](./media/tutorial-assess-vmware/assessment-create.png)

8. Po utworzeniu oceny Wyświetl ją w obszarze **serwery**  >  **Azure Migrate: oceny oceny serwera**  >  **Assessments**.
9. Wybierz pozycję **Ocena eksportu** , aby pobrać ją jako plik programu Excel.

Jeśli chcesz uruchomić **ocenę rozwiązania Azure VMware (Automatyczna synchronizacja)**, wykonaj kroki opisane [tutaj](how-to-create-azure-vmware-solution-assessment.md).


## <a name="review-an-assessment"></a>Przegląd oceny

Ocena zawiera opis:

- **Gotowość platformy Azure**: czy maszyny wirtualne są odpowiednie do migracji na platformę Azure.
- **Oszacowanie kosztów miesięcznych**: szacowane miesięczne koszty obliczeniowe i magazynowe związane z uruchamianiem maszyn wirtualnych na platformie Azure.
- **Oszacowanie kosztu miesięcznego magazynu**: szacowane koszty magazynu dyskowego po migracji.

Aby wyświetlić ocenę:

1. W obszarze serwery **celów migracji**  >  **Servers**wybierz pozycję **oceny** w **Azure Migrate: Ocena serwera**.
2. W obszarze **oceny**Wybierz ocenę, aby ją otworzyć.

   ![Podsumowanie oceny](./media/tutorial-assess-vmware/assessment-summary.png)

### <a name="review-azure-readiness"></a>Przegląd gotowości platformy Azure

1. W obszarze **gotowość platformy Azure**Sprawdź, czy maszyny wirtualne są gotowe do migracji na platformę Azure.
2. Sprawdź stan maszyny wirtualnej:
    - **Gotowe do użycia na platformie Azure**: używane, gdy Azure Migrate zaleca rozmiar maszyny wirtualnej i oszacowania kosztów dla maszyn wirtualnych w ocenie.
    - **Gotowe warunki**: pokazuje problemy i sugerowane korygowanie.
    - **Nie gotowy na platformę Azure**: zawiera problemy i sugerowane korygowanie.
    - **Nieznane gotowość**: używany, gdy Azure Migrate nie może ocenić gotowości z powodu problemów z dostępnością danych.

3. Wybierz stan **gotowości platformy Azure** . Możesz wyświetlić szczegóły gotowości maszyn wirtualnych. Możesz również przejść do szczegółów, aby wyświetlić szczegóły dotyczące maszyn wirtualnych, w tym ustawienia obliczeń, magazynu i sieci.

### <a name="review-cost-details"></a>Przejrzyj szczegóły kosztów

Podsumowanie oceny przedstawia szacowany koszt obliczeń i magazynu dla uruchomionych maszyn wirtualnych na platformie Azure. Koszty są agregowane dla wszystkich maszyn wirtualnych w ocenianej grupie. Możesz przejść do szczegółów, aby wyświetlić szczegóły kosztów dla określonych maszyn wirtualnych.

> [!NOTE]
> Oszacowania kosztów są oparte na zaleceniach dotyczących rozmiaru maszyny, jej dysków i właściwości. Oszacowania dotyczą uruchamiania lokalnych maszyn wirtualnych jako maszyn wirtualnych IaaS. Ocena serwera Azure Migrate nie uwzględnia kosztów PaaS ani SaaS.

Zagregowane koszty magazynu dla ocenianej grupy są podzielone na różne typy dysków magazynu. 

### <a name="review-confidence-rating"></a>Przegląd oceny zaufania

Azure Migrate oceny serwera przypisuje ocenę zaufania do oceny opartej na wydajności, od jednej gwiazdki (najniższej) do pięciu gwiazdek (najwyższa).

![Ocena zaufania](./media/tutorial-assess-vmware/confidence-rating.png)

Ocena zaufania pomaga oszacować niezawodność zaleceń dotyczących rozmiaru oceny. Klasyfikacja jest oparta na dostępności punktów danych niezbędnych do obliczenia oceny:

**Dostępność punktu danych** | **Ocena zaufania**
--- | ---
0%–20% | 1 gwiazdka
21%–40% | 2 gwiazdki
41%–60% | 3 gwiazdki
61%–80% | 4 gwiazdki
81%–100% | 5 gwiazdek

[Poznaj najlepsze rozwiązania](best-practices-assessment.md#best-practices-for-confidence-ratings) dotyczące klasyfikacji zaufania.

## <a name="next-steps"></a>Następne kroki

W tym samouczku skonfigurujesz urządzenie Azure Migrate. Można również utworzyć i przejrzeć ocenę.

Aby dowiedzieć się, jak przeprowadzić migrację maszyn wirtualnych VMware na platformę Azure przy użyciu migracji serwera Azure Migrate, przejdź do trzeciego samouczka w serii:

> [!div class="nextstepaction"]
> [Migrowanie maszyn wirtualnych VMware](./tutorial-migrate-vmware.md)
