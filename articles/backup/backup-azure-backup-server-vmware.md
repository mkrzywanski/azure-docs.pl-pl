---
title: Tworzenie kopii zapasowych maszyn wirtualnych VMware przy użyciu Azure Backup Server
description: W tym artykule dowiesz się, jak używać Azure Backup Server do tworzenia kopii zapasowych maszyn wirtualnych VMware działających na serwerze VMware vCenter/ESXi.
ms.topic: conceptual
ms.date: 05/24/2020
ms.openlocfilehash: c9868012698fcdf5a2352c289de85261b6899dc3
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/20/2020
ms.locfileid: "86497917"
---
# <a name="back-up-vmware-vms-with-azure-backup-server"></a>Tworzenie kopii zapasowych maszyn wirtualnych VMware przy użyciu Azure Backup Server

W tym artykule wyjaśniono, jak utworzyć kopię zapasową maszyn wirtualnych VMware działających na VMware ESXi hostach/vCenter Server na platformie Azure przy użyciu Azure Backup Server.

W tym artykule wyjaśniono, jak:

- Skonfiguruj bezpieczny kanał, aby Azure Backup Server mógł komunikować się z serwerami VMware za pośrednictwem protokołu HTTPS.
- Skonfiguruj konto VMware, którego Azure Backup Server używa do uzyskiwania dostępu do serwera VMware.
- Dodaj poświadczenia konta do Azure Backup.
- Dodaj serwer vCenter lub ESXi do Azure Backup Server.
- Skonfiguruj grupę ochrony zawierającą maszyny wirtualne VMware, dla których chcesz utworzyć kopię zapasową, określ ustawienia kopii zapasowej i Zaplanuj kopię zapasową.

## <a name="before-you-start"></a>Przed rozpoczęciem

- Sprawdź, czy korzystasz z wersji programu vCenter/ESXi, która jest obsługiwana na potrzeby tworzenia kopii zapasowych. Zapoznaj się z macierzą pomocy technicznej [tutaj](./backup-mabs-protection-matrix.md).
- Upewnij się, że skonfigurowano Azure Backup Server. Jeśli jeszcze tego nie zrobiono, [zrób to](backup-azure-microsoft-azure-backup.md) przed rozpoczęciem. Należy uruchomić Azure Backup Server z najnowszymi aktualizacjami.
- Upewnij się, że następujące porty sieciowe są otwarte:
  - TCP 443 między serwera usługi MAB i vCenter
  - TCP 443 i TCP 902 między serwera usługi MAB i hostem ESXi

## <a name="create-a-secure-connection-to-the-vcenter-server"></a>Utwórz bezpieczne połączenie z vCenter Server

Domyślnie Azure Backup Server komunikuje się z serwerami VMware za pośrednictwem protokołu HTTPS. Aby skonfigurować połączenie HTTPS, Pobierz certyfikat urzędu certyfikacji VMware i zaimportuj go na Azure Backup Server.

### <a name="before-you-begin"></a>Przed rozpoczęciem

- Jeśli nie chcesz używać protokołu HTTPS, możesz [wyłączyć weryfikację certyfikatu HTTPS dla wszystkich serwerów VMware](backup-azure-backup-server-vmware.md#disable-https-certificate-validation).
- Zazwyczaj nawiązywane jest połączenie z przeglądarki na komputerze Azure Backup Server z serwerem vCenter/ESXi za pomocą klienta sieci Web vSphere. Po raz pierwszy połączenie nie jest bezpieczne i będzie zawierać następujące elementy.
- Ważne jest, aby zrozumieć, jak Azure Backup Server obsługuje kopie zapasowe.
  - Pierwszy krok Azure Backup Server Tworzenie kopii zapasowych danych na dysku lokalnym. Azure Backup Server używa puli magazynów, zestawu dysków i woluminów, na których Azure Backup Server są przechowywane punkty odzyskiwania dysku dla chronionych danych. Pula magazynów może być bezpośrednio dołączonym magazynem (DAS), siecią SAN Fiber Channel lub urządzeniem magazynującym iSCSI lub SAN. Należy upewnić się, że masz wystarczającą ilość miejsca dla lokalnej kopii zapasowej danych maszyny wirtualnej VMware.
  - Azure Backup Server następnie tworzy kopię zapasową z magazynu na dysku lokalnym na platformie Azure.
  - [Uzyskaj pomoc](/system-center/dpm/create-dpm-protection-groups#figure-out-how-much-storage-space-you-need) , aby ustalić ilość potrzebnego miejsca do magazynowania. Informacje dotyczą programu DPM, ale mogą być również używane dla Azure Backup Server.

### <a name="set-up-the-certificate"></a>Konfigurowanie certyfikatu

Skonfiguruj bezpieczny kanał w następujący sposób:

1. W przeglądarce na Azure Backup Server wprowadź adres URL klienta sieci Web vSphere. Jeśli strona logowania nie zostanie wyświetlona, sprawdź ustawienia połączenia i serwera proxy przeglądarki.

    ![Klient sieci Web vSphere](./media/backup-azure-backup-server-vmware/vsphere-web-client.png)

2. Na stronie logowania klienta sieci Web vSphere kliknij pozycję **Pobierz certyfikaty zaufanych głównych urzędów certyfikacji**.

    ![Pobierz certyfikat zaufanego głównego urzędu certyfikacji](./media/backup-azure-backup-server-vmware/vmware-download-ca-cert-prompt.png)

3. Pobierany jest plik o nazwie **pobieranie** . W zależności od przeglądarki pojawia się komunikat z pytaniem, czy otworzyć lub zapisać plik.

    ![Pobierz certyfikat urzędu certyfikacji](./media/backup-azure-backup-server-vmware/download-certs.png)

4. Zapisz plik na maszynie Azure Backup Server z rozszerzeniem. zip.

5. Kliknij prawym przyciskiem myszy pozycję **download.zip**  >  **Wyodrębnij wszystko**. Plik. zip wyodrębnia jego zawartość do folderu **certs** , który zawiera następujące elementy:
   - Plik certyfikatu głównego z rozszerzeniem rozpoczynającym się od numerowanej sekwencji takiej jak. 0 i. 1.
   - Plik listy CRL ma rozszerzenie zaczynające się od sekwencji like. r0 lub. R1. Plik listy CRL jest skojarzony z certyfikatem.

    ![Pobrane certyfikaty](./media/backup-azure-backup-server-vmware/extracted-files-in-certs-folder.png)

6. W folderze **Certyfikaty** kliknij prawym przyciskiem myszy plik certyfikatu głównego, > **zmienić nazwę**.

    ![Zmień nazwę certyfikatu głównego](./media/backup-azure-backup-server-vmware/rename-cert.png)

7. Zmień rozszerzenie certyfikatu głównego na. CRT i potwierdź. Ikona pliku zmieni się na taki, który reprezentuje certyfikat główny.

8. Kliknij prawym przyciskiem myszy certyfikat główny i z menu podręcznego wybierz pozycję **Zainstaluj certyfikat**.

9. W **Kreatorze importu certyfikatów**wybierz pozycję **komputer lokalny** jako miejsce docelowe certyfikatu, a następnie kliknij przycisk **dalej**. Potwierdź, czy zostanie wyświetlony monit o zezwolenie na wprowadzanie zmian na komputerze.

    ![Kreator — Zapraszamy!](./media/backup-azure-backup-server-vmware/certificate-import-wizard1.png)

10. Na stronie **Magazyn certyfikatów** wybierz opcję **Umieść wszystkie certyfikaty w następującym magazynie**, a następnie kliknij przycisk **Przeglądaj** , aby wybrać magazyn certyfikatów.

    ![Magazyn certyfikatów](./media/backup-azure-backup-server-vmware/cert-import-wizard-local-store.png)

11. W obszarze **Wybierz magazyn certyfikatów**wybierz pozycję **Zaufane główne** urzędy certyfikacji jako folder docelowy dla certyfikatów, a następnie kliknij przycisk **OK**.

    ![Folder docelowy certyfikatu](./media/backup-azure-backup-server-vmware/certificate-store-selected.png)

12. W obszarze **Kończenie pracy Kreatora importu certyfikatów**sprawdź folder, a następnie kliknij przycisk **Zakończ**.

    ![Sprawdź, czy certyfikat znajduje się w odpowiednim folderze](./media/backup-azure-backup-server-vmware/cert-wizard-final-screen.png)

13. Po potwierdzeniu importu certyfikatu Zaloguj się do vCenter Server, aby upewnić się, że połączenie jest bezpieczne.

### <a name="disable-https-certificate-validation"></a>Wyłącz weryfikację certyfikatu HTTPS

Jeśli w organizacji znajdują się bezpieczne granice i nie chcesz używać protokołu HTTPS między serwerami VMware i maszyną Azure Backup Server, Wyłącz protokół HTTPS w następujący sposób:

1. Skopiuj i wklej następujący tekst do pliku txt.

    ```text
    Windows Registry Editor Version 5.00
    [HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft Data Protection Manager\VMWare]
    "IgnoreCertificateValidation"=dword:00000001
    ```

2. Zapisz plik na maszynie Azure Backup Server przy użyciu nazwy **DisableSecureAuthentication. reg**.

3. Kliknij dwukrotnie plik, aby aktywować wpis rejestru.

## <a name="create-a-vmware-role"></a>Tworzenie roli VMware

Azure Backup Server musi mieć konto użytkownika z uprawnieniami dostępu do hosta programu v-Center Server/ESXi. Utwórz rolę VMware z określonymi uprawnieniami, a następnie skojarz konto użytkownika z rolą.

1. Zaloguj się do vCenter Server (lub hosta ESXi, jeśli nie używasz vCenter Server).
2. W panelu **Nawigator** kliknij pozycję **Administracja**.

    ![Administracja](./media/backup-azure-backup-server-vmware/vmware-navigator-panel.png)

3. W **Administration**obszarze  >  **role**administracyjne kliknij ikonę Dodaj rolę (symbol +).

    ![Dodaj rolę](./media/backup-azure-backup-server-vmware/vmware-define-new-role.png)

4. W polu **Utwórz**  >  **nazwę roli**roli wprowadź *BackupAdminRole*. Nazwa roli może być dowolnie taka, ale powinna być rozpoznawalna dla celu roli.

5. Wybierz uprawnienia, które zostały podsumowane w poniższej tabeli, a następnie kliknij przycisk **OK**.  Nowa rola zostanie wyświetlona na liście w panelu **role** .
   - Kliknij ikonę obok etykiety nadrzędnej, aby rozwinąć element nadrzędny i wyświetlić uprawnienia podrzędne.
   - Aby wybrać uprawnienia VirtualMachine, musisz przejść na kilka poziomów do nadrzędnej hierarchii podrzędnej.
   - Nie musisz wybierać wszystkich uprawnień podrzędnych w ramach uprawnień nadrzędnych.

    ![Hierarchia uprawnień nadrzędnego elementu podrzędnego](./media/backup-azure-backup-server-vmware/cert-add-privilege-expand.png)

### <a name="role-permissions"></a>Uprawnienia roli

W poniższej tabeli przedstawiono uprawnienia, które należy przypisać do tworzonego konta użytkownika:

| Uprawnienia dla konta użytkownika vCenter 6,5                          | Uprawnienia dla konta użytkownika vCenter 6,7                            |
|----------------------------------------------------------------------------|----------------------------------------------------------------------------|
| Magazyn danych cluster.Configuruj klaster magazynu danych                           | Magazyn danych cluster.Configuruj klaster magazynu danych                           |
| Magazyn danych. AllocateSpace                                                    | Magazyn danych. AllocateSpace                                                    |
| Magazyn danych. Przeglądaj magazyn danych                                                 | Magazyn danych. Przeglądaj magazyn danych                                                 |
| Magazyn danych. operacje na plikach niskiego poziomu                                        | Magazyn danych. operacje na plikach niskiego poziomu                                        |
| Global. Disable, metody                                                     | Global. Disable, metody                                                     |
| Global. Enable — metody                                                      | Global. Enable — metody                                                      |
| Globalne. licencje                                                            | Globalne. licencje                                                            |
| Zdarzenie globalne. log                                                           | Zdarzenie globalne. log                                                           |
| Global. Zarządzaj atrybutami niestandardowymi                                            | Global. Zarządzaj atrybutami niestandardowymi                                            |
| Global. Set — atrybut niestandardowy                                                | Global. Set — atrybut niestandardowy                                                |
| Operacje hosta. local. Utwórz maszynę wirtualną                               | Operacje hosta. local. Utwórz maszynę wirtualną                               |
| Sieć. Przypisywanie sieci                                                     | Sieć. Przypisywanie sieci                                                     |
| Zasoby. Przypisz maszynę wirtualną do puli zasobów                          | Zasoby. Przypisz maszynę wirtualną do puli zasobów                          |
| vApp. Dodaj maszynę wirtualną                                                   | vApp. Dodaj maszynę wirtualną                                                   |
| vApp. Przypisz pulę zasobów                                                  | vApp. Przypisz pulę zasobów                                                  |
| vApp. Unregister                                                            | vApp. Unregister                                                            |
| VirtualMachine.Configwersja. Dodaj lub Usuń urządzenie                         | VirtualMachine.Configwersja. Dodaj lub Usuń urządzenie                         |
| Wersja Virtual machine.Config. Dzierżawa dysku                                   | Wersja Virtual machine.Config. Pozyskiwanie dzierżawy dysku                           |
| Wersja Virtual machine.Config. Dodaj nowy dysk                                 | Wersja Virtual machine.Config. Dodaj nowy dysk                                 |
| Wersja Virtual machine.Config. Doświadczonych                                     | Wersja Virtual machine.Config. Konfiguracja zaawansowana                       |
| Wersja Virtual machine.Config. Śledzenie zmian dysku                         | Wersja Virtual machine.Config. Przełącz śledzenie zmian dysku                  |
| Wersja Virtual machine.Config. Urządzenie hosta USB                              | Wirtualne urządzenie USB hosta uruj machine.Configuration.Config                    |
| Wersja Virtual machine.Config. Zwiększ dysk wirtualny                          | Wersja Virtual machine.Config. Zwiększ dysk wirtualny                          |
| Wersja Virtual machine.Config. Wykonywanie zapytań dotyczących plików nienależących do użytkownika                          | Wersja Virtual machine.Config. Wykonywanie zapytań dotyczących plików nienależących do użytkownika                          |
| Wersja Virtual machine.Config. Swapfile                           | Wersja Virtual machine.Config. Zmień umieszczanie swapfile                    |
| Maszyna wirtualna. Operacje gościa. wykonywanie programu operacji gościa         | Maszyna wirtualna. Operacje gościa. wykonywanie programu operacji gościa         |
| Maszyna wirtualna. Operacje gościa. modyfikacje operacji gościa             | Maszyna wirtualna. Operacje gościa. modyfikacje operacji gościa             |
| Maszyna wirtualna. Operacje gościa. zapytania dotyczące operacji gościa                   | Maszyna wirtualna. Operacje gościa. zapytania dotyczące operacji gościa                   |
| Maszyna wirtualna. Udziału. Połączenie z urządzeniem                            | Maszyna wirtualna. Udziału. Połączenie z urządzeniem                            |
| Maszyna wirtualna. Udziału. Zarządzanie systemem operacyjnym gościa za pomocą interfejsu API VIX | Maszyna wirtualna. Udziału. Zarządzanie systemem operacyjnym gościa za pomocą interfejsu API VIX |
| Maszyna wirtualna. Udziału. Zasilanie wyłączone                                    | Maszyna wirtualna. Udziału. Zasilanie wyłączone                                    |
| Maszyna wirtualna. Spis. Utwórz nowy                                      | Maszyna wirtualna. Spis. Utwórz nowy                                      |
| Maszyna wirtualna. Spis. Remove                                          | Maszyna wirtualna. Spis. Remove                                          |
| Maszyna wirtualna. Spis. Rejestr                                        | Maszyna wirtualna. Spis. Rejestr                                        |
| Maszyna wirtualna. Inicjowanie obsługi administracyjnej. Zezwalaj na dostęp do dysku                            | Maszyna wirtualna. Inicjowanie obsługi administracyjnej. Zezwalaj na dostęp do dysku                            |
| Maszyna wirtualna. Inicjowanie obsługi administracyjnej. Zezwalaj na dostęp do plików                            | Maszyna wirtualna. Inicjowanie obsługi administracyjnej. Zezwalaj na dostęp do plików                            |
| Maszyna wirtualna. Inicjowanie obsługi administracyjnej. Zezwalaj na dostęp do dysku tylko do odczytu                  | Maszyna wirtualna. Inicjowanie obsługi administracyjnej. Zezwalaj na dostęp do dysku tylko do odczytu                  |
| Maszyna wirtualna. Inicjowanie obsługi administracyjnej. Zezwalaj na pobieranie maszyny wirtualnej               | Maszyna wirtualna. Inicjowanie obsługi administracyjnej. Zezwalaj na pobieranie maszyny wirtualnej               |
| Maszyna wirtualna. Zarządzanie migawkami. Tworzenie migawki                      | Maszyna wirtualna. Zarządzanie migawkami. Tworzenie migawki                      |
| Maszyna wirtualna. Zarządzanie migawkami. Usuń migawkę                       | Maszyna wirtualna. Zarządzanie migawkami. Usuń migawkę                       |
| Maszyna wirtualna. Zarządzanie migawkami. Przywróć migawkę                    | Maszyna wirtualna. Zarządzanie migawkami. Przywróć migawkę                    |

> [!NOTE]
> W poniższej tabeli wymieniono uprawnienia dla kont użytkowników vCenter 6,0 i vCenter 5,5.

| Uprawnienia dla konta użytkownika vCenter 6,0 | Uprawnienia dla konta użytkownika vCenter 5,5 |
| --- | --- |
| Magazyn danych. AllocateSpace | Network. Assign |
| Global. Zarządzaj atrybutami niestandardowymi | Magazyn danych. AllocateSpace |
| Global. Set — atrybut niestandardowy | VirtualMachine.Config. Śledzenia zmian |
| Operacje hosta. local. Utwórz maszynę wirtualną | VirtualMachine. State. RemoveSnapshot |
| NFS. Przypisywanie sieci | VirtualMachine. State. issnapshot |
| Zasoby. Przypisz maszynę wirtualną do puli zasobów | VirtualMachine. Provisioning. DiskRandomRead |
| Wersja Virtual machine.Config. Dodaj nowy dysk | VirtualMachine. współdziała. wyłączenie |
| Wersja Virtual machine.Config. Doświadczonych | VirtualMachine. Inventory. Create |
| Wersja Virtual machine.Config. Śledzenie zmian dysku | VirtualMachine.Config. AddNewDisk |
| Wersja Virtual machine.Config. Urządzenie hosta USB | VirtualMachine.Config. HostUSBDevice |
| Wersja Virtual machine.Config. Wykonywanie zapytań dotyczących plików nienależących do użytkownika | VirtualMachine.Config. AdvancedConfig |
| Wersja Virtual machine.Config. Swapfile | VirtualMachine.Config. SwapPlacement |
| Maszyna wirtualna. Interakcja. Zasilanie wyłączone | Global. ManageCustomFields |
| Maszyna wirtualna. Towar. Tworzenie nowego elementu |   |
| Maszyna wirtualna. Inicjowanie obsługi administracyjnej. Zezwalaj na dostęp do dysku |   |
| Maszyna wirtualna. Aprowizacji. Zezwalaj na dostęp do dysku tylko do odczytu |   |
| Maszyna wirtualna. Zarządzanie migawkami. Utwórz migawkę |   |
| Maszyna wirtualna. Zarządzanie migawkami. Usuń migawkę |   |

## <a name="create-a-vmware-account"></a>Utwórz konto VMware

1. W vCenter Server panelu **nawigatora** kliknij pozycję **Użytkownicy i grupy**. Jeśli nie używasz vCenter Server, Utwórz konto na odpowiednim hoście ESXi.

    ![Opcja Użytkownicy i grupy](./media/backup-azure-backup-server-vmware/vmware-userandgroup-panel.png)

    Zostanie wyświetlony panel **Użytkownicy i grupy vCenter** .

2. W panelu **Użytkownicy i grupy vCenter** wybierz kartę **Użytkownicy** , a następnie kliknij ikonę Dodaj użytkowników (symbol +).

    ![Panel Użytkownicy i grupy vCenter](./media/backup-azure-backup-server-vmware/usersandgroups.png)

3. W oknie dialogowym **nowy użytkownik** Dodaj informacje o użytkowniku > **OK**. W tej procedurze nazwa użytkownika to BackupAdmin.

    ![Okno dialogowe Nowy użytkownik](./media/backup-azure-backup-server-vmware/vmware-new-user-account.png)

4. Aby skojarzyć konto użytkownika z rolą, w panelu **Nawigator** kliknij pozycję **uprawnienia globalne**. W panelu **uprawnienia globalne** wybierz kartę **Zarządzanie** , a następnie kliknij ikonę Dodaj (symbol +).

    ![Panel uprawnień globalnych](./media/backup-azure-backup-server-vmware/vmware-add-new-perms.png)

5. W obszarze **główny uprawnień globalnych — Dodaj uprawnienie**, kliknij przycisk **Dodaj** , aby wybrać użytkownika lub grupę.

    ![Wybierz użytkownika lub grupę](./media/backup-azure-backup-server-vmware/vmware-add-new-global-perm.png)

6. W obszarze **Wybierz użytkowników/grupy**wybierz pozycję **BackupAdmin**  >  **Dodaj**. W obszarze **Użytkownicy**, w przypadku konta użytkownika jest używany format *domena \ nazwa_użytkownika* . Jeśli chcesz użyć innej domeny, wybierz ją z listy **domen** . Kliknij przycisk **OK** , aby dodać wybranych użytkowników do okna dialogowego **Dodawanie uprawnienia** .

    ![Dodawanie użytkownika BackupAdmin](./media/backup-azure-backup-server-vmware/vmware-assign-account-to-role.png)

7. Z listy rozwijanej z **przypisaną rolą**wybierz pozycję **BackupAdminRole**  >  **OK**.

    ![Przypisywanie użytkownika do roli](./media/backup-azure-backup-server-vmware/vmware-choose-role.png)

Na karcie **Zarządzanie** w panelu **uprawnienia globalne** na liście zostanie wyświetlona nowa konto użytkownika i skojarzona rola.

## <a name="add-the-account-on-azure-backup-server"></a>Dodaj konto na Azure Backup Server

1. Otwórz Azure Backup Server. Jeśli nie możesz znaleźć ikony na pulpicie, otwórz Microsoft Azure Backup z listy aplikacji.

    ![Ikona Azure Backup Server](./media/backup-azure-backup-server-vmware/mabs-icon.png)

2. W konsoli Azure Backup Server kliknij pozycję **Zarządzanie**  >   **serwery produkcyjne**  >  **Zarządzanie programem VMware**.

    ![Konsola Azure Backup Server](./media/backup-azure-backup-server-vmware/add-vmware-credentials.png)

3. W oknie dialogowym **Zarządzanie poświadczeniami** kliknij przycisk **Dodaj**.

    ![Okno dialogowe Zarządzanie poświadczeniami Azure Backup Server](./media/backup-azure-backup-server-vmware/mabs-manage-credentials-dialog.png)

4. W polu **Dodaj poświadczenie**wprowadź nazwę i opis nowego poświadczenia, a następnie określ nazwę użytkownika i hasło zdefiniowane na serwerze VMware. Nazwa, *poświadczenie contoso vCenter* , służy do identyfikowania poświadczeń w tej procedurze. Jeśli serwer VMware i Azure Backup Server nie znajdują się w tej samej domenie, określ domenę w polu Nazwa użytkownika.

    ![Okno dialogowe Dodawanie poświadczeń Azure Backup Server](./media/backup-azure-backup-server-vmware/mabs-add-credential-dialog2.png)

5. Kliknij przycisk **Dodaj** , aby dodać nowe poświadczenie.

    ![Okno dialogowe Zarządzanie poświadczeniami Azure Backup Server](./media/backup-azure-backup-server-vmware/new-list-of-mabs-creds.png)

## <a name="add-the-vcenter-server"></a>Dodaj vCenter Server

Dodaj vCenter Server do Azure Backup Server.

1. W konsoli Azure Backup Server kliknij pozycję **Zarządzanie**  >  **serwerami produkcyjnymi**  >  **Dodaj**.

    ![Kreator otwierania dodawania serwera produkcyjnego](./media/backup-azure-backup-server-vmware/add-vcenter-to-mabs.png)

2. W **Kreatorze dodawania do serwera produkcyjnego**  >  **Wybierz opcję Typ serwera produkcyjnego** , wybierz pozycję **serwery VMware**, a następnie kliknij przycisk **dalej**.

    ![Kreator dodawania do serwera produkcyjnego](./media/backup-azure-backup-server-vmware/production-server-add-wizard.png)

3. W polu **Wybierz komputery**  **nazwa/adres IP**Określ nazwę FQDN lub adres IP serwera VMware. Jeśli wszystkie serwery ESXi są zarządzane przez ten sam program vCenter, określ nazwę serwera vCenter. W przeciwnym razie Dodaj hosta ESXi.

    ![Określ serwer VMware](./media/backup-azure-backup-server-vmware/add-vmware-server-provide-server-name.png)

4. W polu **Port SSL**Wprowadź port używany do komunikowania się z serwerem VMware. 443 jest portem domyślnym, ale można go zmienić, jeśli serwer VMware nasłuchuje na innym porcie.

5. W obszarze **Określ poświadczenie**wybierz utworzone wcześniej poświadczenia.

    ![Określ poświadczenie](./media/backup-azure-backup-server-vmware/identify-creds.png)

6. Kliknij przycisk **Dodaj** , aby dodać serwer VMware do listy serwerów. Następnie kliknij przycisk **Dalej**.

    ![Dodaj serwer i poświadczenie oprogramowania VMWare](./media/backup-azure-backup-server-vmware/add-vmware-server-credentials.png)

7. Na stronie **Podsumowanie** kliknij przycisk **Dodaj** , aby dodać serwer VMware do Azure Backup Server. Nowy serwer zostanie dodany natychmiast, na serwerze VMware nie jest wymagany żaden Agent.

    ![Dodaj serwer VMware do Azure Backup Server](./media/backup-azure-backup-server-vmware/tasks-screen.png)

8. Sprawdź ustawienia na stronie **zakończenie** .

   ![Strona końcowa](./media/backup-azure-backup-server-vmware/summary-screen.png)

Jeśli masz wiele hostów ESXi, które nie są zarządzane przez program vCenter Server, lub masz wiele wystąpień vCenter Server, musisz ponownie uruchomić kreatora, aby dodać serwery.

## <a name="configure-a-protection-group"></a>Konfigurowanie grupy ochrony

Dodaj maszyny wirtualne VMware na potrzeby tworzenia kopii zapasowych. Grupy ochrony zbierają wiele maszyn wirtualnych i stosują te same ustawienia przechowywania i tworzenia kopii zapasowych danych do wszystkich maszyn wirtualnych w grupie.

1. W konsoli Azure Backup Server kliknij pozycję **Ochrona**, > **Nowy**.

    ![Otwórz Kreatora tworzenia nowej grupy ochrony](./media/backup-azure-backup-server-vmware/open-protection-wizard.png)

1. Na stronie powitalnej Kreatora **tworzenia nowej grupy ochrony** kliknij przycisk **dalej**.

    ![Kreator tworzenia nowej grupy ochrony — okno dialogowe](./media/backup-azure-backup-server-vmware/protection-wizard.png)

1. Na stronie **Wybierz typ grupy ochrony** wybierz pozycję **serwery** , a następnie kliknij przycisk **dalej**. Zostanie wyświetlona strona **Wybierz członków grupy** .

1. W obszarze **Wybierz członków grupy**Wybierz Maszyny wirtualne (lub foldery maszyn wirtualnych), których kopię zapasową chcesz utworzyć. Następnie kliknij przycisk **Dalej**.

    - Po wybraniu folderu zostaną również wybrane maszyny wirtualne lub foldery znajdujące się w tym folderze. Możesz wyczyścić foldery lub maszyny wirtualne, których kopia zapasowa ma nie być wykonana.
1. Jeśli tworzona jest już kopia zapasowa maszyny wirtualnej lub folderu, nie można jej wybrać. Gwarantuje to, że nie zostaną utworzone zduplikowane punkty odzyskiwania dla maszyny wirtualnej.

    ![Wybierz członków grupy](./media/backup-azure-backup-server-vmware/server-add-selected-members.png)

1. Na stronie **Wybierz metodę ochrony danych** wprowadź nazwę grupy ochrony i ustawienia ochrony. Aby utworzyć kopię zapasową na platformie Azure, ustaw krótkoterminową ochronę na **dysku** i Włącz ochronę w trybie online. Następnie kliknij przycisk **Dalej**.

    ![Wybieranie metody ochrony danych](./media/backup-azure-backup-server-vmware/name-protection-group.png)

1. W obszarze **Określ cele krótkoterminowe**Określ, jak długo mają być przechowywane kopie zapasowe danych na dysku.
   - W obszarze **Zakres przechowywania**Określ, ile dni mają być przechowywane punkty odzyskiwania dysków.
   - W obszarze **częstotliwość synchronizacji**Określ, jak często mają być pobierane punkty odzyskiwania dysków.
       - Jeśli nie chcesz ustawiać interwału kopii zapasowych, możesz sprawdzić **tuż przed punktem odzyskiwania** , aby kopia zapasowa była uruchamiana tuż przed zaplanowaniem każdego punktu odzyskiwania.
       - Krótkoterminowe kopie zapasowe są pełnymi kopiami zapasowymi i nie są przyrostowe.
       - Kliknij przycisk **Modyfikuj** , aby zmienić czasy/daty w przypadku wystąpienia krótkoterminowych kopii zapasowych.

         ![Określ cele krótkoterminowe](./media/backup-azure-backup-server-vmware/short-term-goals.png)

1. Na stronie **Przejrzyj przydział dysku**Przejrzyj miejsce na dysku udostępnione dla kopii zapasowych maszyn wirtualnych. dla maszyn wirtualnych.

   - Zalecane alokacje dysków są zależne od określonego zakresu przechowywania, typu obciążenia i rozmiaru chronionych danych. Wprowadź wymagane zmiany, a następnie kliknij przycisk **dalej**.
   - **Rozmiar danych:** Rozmiar danych w grupie ochrony.
   - **Miejsce na dysku:** Zalecana ilość miejsca na dysku dla grupy ochrony. Jeśli chcesz zmodyfikować to ustawienie, należy przydzielić całkowite miejsce, które jest nieco większe niż wielkość Szacowana każdego źródła danych.
   - **Przeszukaj dane:** Jeśli włączysz wspólną lokalizację, wiele źródeł danych w ramach ochrony będzie można mapować na pojedynczą replikę i wolumin punktu odzyskiwania. Współlokalizacja nie jest obsługiwana dla wszystkich obciążeń.
   - **Automatycznie rośnie:** Jeśli włączysz to ustawienie, jeśli dane w grupie ochrony przekroczą alokację początkową, Azure Backup Server próbuje zwiększyć rozmiar dysku o 25%.
   - **Szczegóły puli magazynu:** Pokazuje stan puli magazynów, w tym całkowity rozmiar dysku i pozostałe.

    ![Przejrzyj przydział dysku](./media/backup-azure-backup-server-vmware/review-disk-allocation.png)

1. Na stronie **Wybierz metodę tworzenia repliki** Określ sposób tworzenia początkowej kopii zapasowej, a następnie kliknij przycisk **dalej**.
   - Wartość domyślna jest **automatycznie przez sieć** i **teraz**.
   - Jeśli używasz domyślnego, zalecamy określenie czasu poza godzinami szczytu. Wybierz pozycję **dalej** i określ datę i godzinę.
   - W przypadku dużych ilości danych lub mniej niż optymalnych warunków sieciowych należy rozważyć replikację danych w trybie offline za pomocą nośników wymiennych.

    ![Wybierz metodę tworzenia repliki](./media/backup-azure-backup-server-vmware/replica-creation.png)

1. W obszarze **Opcje sprawdzania spójności**wybierz, jak i kiedy zautomatyzować sprawdzanie spójności. Następnie kliknij przycisk **Dalej**.
      - Sprawdzanie spójności można uruchomić, gdy dane repliki staną się niespójne lub zgodnie z ustalonym harmonogramem.
      - Jeśli nie chcesz konfigurować automatycznego sprawdzania spójności, możesz uruchomić sprawdzanie ręczne. Aby to zrobić, kliknij prawym przyciskiem myszy grupę ochrony > **Przeprowadź sprawdzanie spójności**.

1. Na stronie **Określ dane ochrony online** Wybierz Maszyny wirtualne lub foldery maszyn wirtualnych, dla których chcesz utworzyć kopię zapasową. Możesz wybrać członków indywidualnie lub kliknąć przycisk **Zaznacz wszystko** , aby wybrać wszystkich członków. Następnie kliknij przycisk **Dalej**.

    ![Określ dane ochrony w trybie online](./media/backup-azure-backup-server-vmware/select-data-to-protect.png)

1. Na stronie **Określanie harmonogramu tworzenia kopii zapasowych online** Określ, jak często mają być wykonywane kopie zapasowe danych z magazynu lokalnego na platformę Azure.

    - Punkty odzyskiwania w chmurze dla danych będą generowane zgodnie z harmonogramem. Następnie kliknij przycisk **Dalej**.
    - Po wygenerowaniu punkt odzyskiwania zostanie przeniesiony do magazynu Recovery Services na platformie Azure.

    ![Określ harmonogram kopii zapasowych online](./media/backup-azure-backup-server-vmware/online-backup-schedule.png)

1. Na stronie **Określanie zasad przechowywania w trybie online** wskaż, jak długo mają być przechowywane punkty odzyskiwania tworzone z kopii zapasowych codziennie/co tydzień/miesiąc/rok na platformie Azure. następnie kliknij przycisk **dalej**.

    - Nie ma limitu czasu, w którym można przechowywać dane na platformie Azure.
    - Jedynym ograniczeniem jest to, że nie można mieć więcej niż 9999 punktów odzyskiwania dla każdego chronionego wystąpienia. W tym przykładzie chronione wystąpienie jest serwerem VMware.

    ![Określ zasady przechowywania danych online](./media/backup-azure-backup-server-vmware/retention-policy.png)

1. Na stronie **Podsumowanie** przejrzyj ustawienia, a następnie kliknij przycisk **Utwórz grupę**.

    ![Członek grupy ochrony i podsumowanie ustawień](./media/backup-azure-backup-server-vmware/protection-group-summary.png)

## <a name="vmware-parallel-backups"></a>Równoległe kopie zapasowe VMware

>[!NOTE]
> Ta funkcja ma zastosowanie do serwera usługi MAB v3 UR1.

We wcześniejszych wersjach programu serwera usługi MAB równoległe kopie zapasowe były wykonywane tylko w grupach ochrony. W przypadku serwera usługi MAB v3 UR1 wszystkie kopie zapasowe maszyn wirtualnych VMWare w ramach jednej grupy ochrony są równoległe, co prowadzi do szybszego tworzenia kopii zapasowych maszyn wirtualnych. Wszystkie zadania replikacji różnicowej programu VMWare są uruchamiane równolegle. Domyślnie liczba zadań do uruchomienia równoległego jest ustawiona na 8.

Liczbę zadań można zmodyfikować przy użyciu klucza rejestru, jak pokazano poniżej (nieobecny domyślnie, należy go dodać):

**Ścieżka klucza**:`Software\Microsoft\Microsoft Data Protection Manager\Configuration\ MaxParallelIncrementalJobs\VMWare`<BR>
**Typ klucza**: wartość DWORD (32-bitowa).

> [!NOTE]
> Liczbę zadań można zmodyfikować na wyższą wartość. W przypadku ustawienia numeru zadania na 1 zadania replikacji są uruchamiane sekwencyjnie. Aby zwiększyć liczbę do wyższej wartości, należy wziąć pod uwagę wydajność programu VMWare. Należy wziąć pod uwagę liczbę używanych zasobów i dodatkowe użycie wymagane przez program VMWare vSphere Server oraz określić liczbę zadań replikacji różnicowej, które mają być uruchamiane równolegle. Ponadto ta zmiana będzie miała wpływ tylko na nowo utworzone grupy ochrony. W przypadku istniejących grup ochrony należy tymczasowo dodać kolejną maszynę wirtualną do grupy ochrony. Należy odpowiednio zaktualizować konfigurację grupy ochrony. Po zakończeniu procedury można usunąć tę maszynę wirtualną z grupy ochrony.

## <a name="vmware-vsphere-67"></a>VMWare vSphere 6,7

Aby utworzyć kopię zapasową vSphere 6,7, wykonaj następujące czynności:

- Włącz protokół TLS 1,2 na serwerze DPM

>[!NOTE]
>W przypadku oprogramowania VMWare 6,7 włączono protokół TLS.

- Ustaw klucze rejestru w następujący sposób:

```text
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\.NETFramework\v2.0.50727]
"SystemDefaultTlsVersions"=dword:00000001
"SchUseStrongCrypto"=dword:00000001

[HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\.NETFramework\v4.0.30319]
"SystemDefaultTlsVersions"=dword:00000001
"SchUseStrongCrypto"=dword:00000001

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v2.0.50727]
"SystemDefaultTlsVersions"=dword:00000001
"SchUseStrongCrypto"=dword:00000001

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\.NETFramework\v4.0.30319]
"SystemDefaultTlsVersions"=dword:00000001
"SchUseStrongCrypto"=dword:00000001
```

## <a name="exclude-disk-from-vmware-vm-backup"></a>Wyklucz dysk z kopii zapasowej maszyny wirtualnej VMware

> [!NOTE]
> Ta funkcja ma zastosowanie do serwera usługi MAB v3 UR1.

Za pomocą serwera usługi MAB v3 UR1 można wykluczyć określony dysk z kopii zapasowej maszyny wirtualnej VMware. **ExcludeDisk.ps1** skryptu konfiguracyjnego znajduje się w `C:\Program Files\Microsoft Azure Backup Server\DPM\DPM\bin folder` .

Aby skonfigurować wykluczenie dysku, wykonaj następujące czynności:

### <a name="identify-the-vmware-vm-and-disk-details-to-be-excluded"></a>Zidentyfikuj szczegóły maszyny wirtualnej VMWare i dysku do wykluczenia

  1. W konsoli programu VMware przejdź do ustawień maszyny wirtualnej, dla których chcesz wykluczyć dysk.
  2. Wybierz dysk, który ma zostać wykluczony, i Zanotuj ścieżkę dla tego dysku.

        Na przykład aby wykluczyć dysk twardy 2 z TestVM4, ścieżka do dysku twardego 2 to **[datastore1] TestVM4/TestVM4 \_ 1. vmdk**.

        ![Dysk twardy, który ma zostać wykluczony](./media/backup-azure-backup-server-vmware/test-vm.png)

### <a name="configure-mabs-server"></a>Konfigurowanie serwera serwera usługi MAB

Przejdź do serwera serwera usługi MAB, na którym maszyna wirtualna VMware została skonfigurowana pod kątem ochrony, aby skonfigurować wykluczanie dysku.

  1. Zapoznaj się ze szczegółami hosta VMware, który jest chroniony na serwerze serwera usługi MAB.

        ```powershell
        $psInfo = get-DPMProductionServer
        $psInfo
        ```

        ```output
        ServerName   ClusterName     Domain            ServerProtectionState
        ----------   -----------     ------            ---------------------
        Vcentervm1                   Contoso.COM       NoDatasourcesProtected
        ```

  2. Wybierz hosta VMware i Wyświetl listę ochrony maszyn wirtualnych dla hosta VMware.

        ```powershell
        $vmDsInfo = get-DPMDatasource -ProductionServer $psInfo[0] -Inquire
        $vmDsInfo
        ```

        ```output
        Computer     Name     ObjectType
        --------     ----     ----------
        Vcentervm1  TestVM2      VMware
        Vcentervm1  TestVM1      VMware
        Vcentervm1  TestVM4      VMware
        ```

  3. Wybierz maszynę wirtualną, dla której chcesz wykluczyć dysk.

        ```powershell
        $vmDsInfo[2]
        ```

        ```output
        Computer     Name      ObjectType
        --------     ----      ----------
        Vcentervm1   TestVM4   VMware
        ```

  4. Aby wykluczyć dysk, przejdź do `Bin` folderu i uruchom skrypt *ExcludeDisk.ps1* z następującymi parametrami:

        > [!NOTE]
        > Przed uruchomieniem tego polecenia Zatrzymaj usługę DPMRA na serwerze serwera usługi MAB. W przeciwnym razie skrypt zwraca sukces, ale nie aktualizuje listy wykluczeń. Przed zatrzymaniem usługi upewnij się, że żadne zadania nie są w toku.

     **Aby dodać/usunąć dysk z wykluczenia, uruchom następujące polecenie:**

      ```powershell
      ./ExcludeDisk.ps1 -Datasource $vmDsInfo[0] [-Add|Remove] "[Datastore] vmdk/vmdk.vmdk"
      ```

     **Przykład**:

     Aby dodać wykluczenie dysku dla TestVM4, uruchom następujące polecenie:

       ```powershell
      C:\Program Files\Microsoft Azure Backup Server\DPM\DPM\bin> ./ExcludeDisk.ps1 -Datasource $vmDsInfo[2] -Add "[datastore1] TestVM4/TestVM4\_1.vmdk"
       ```

      ```output
       Creating C:\Program Files\Microsoft Azure Backup Server\DPM\DPM\bin\excludedisk.xml
       Disk : [datastore1] TestVM4/TestVM4\_1.vmdk, has been added to disk exclusion list.
      ```

  5. Sprawdź, czy dysk został dodany do wykluczenia.

     **Aby wyświetlić istniejące wykluczenie dla określonych maszyn wirtualnych, uruchom następujące polecenie:**

        ```powershell
        ./ExcludeDisk.ps1 -Datasource $vmDsInfo[0] [-view]
        ```

     **Przykład**

        ```powershell
        C:\Program Files\Microsoft Azure Backup Server\DPM\DPM\bin> ./ExcludeDisk.ps1 -Datasource $vmDsInfo[2] -view
        ```

        ```output
        <VirtualMachine>
        <UUID>52b2b1b6-5a74-1359-a0a5-1c3627c7b96a</UUID>
        <ExcludeDisk>[datastore1] TestVM4/TestVM4\_1.vmdk</ExcludeDisk>
        </VirtualMachine>
        ```

     Po skonfigurowaniu ochrony dla tej maszyny wykluczonej dysk nie będzie wyświetlany podczas ochrony.

        > [!NOTE]
        > Jeśli wykonujesz te kroki dla już chronionej maszyny wirtualnej, musisz ręcznie uruchomić sprawdzanie spójności po dodaniu dysku do wykluczenia.

### <a name="remove-the-disk-from-exclusion"></a>Usuń dysk z wykluczenia

Aby usunąć dysk z wykluczenia, uruchom następujące polecenie:

```powershell
C:\Program Files\Microsoft Azure Backup Server\DPM\DPM\bin> ./ExcludeDisk.ps1 -Datasource $vmDsInfo[2] -Remove "[datastore1] TestVM4/TestVM4\_1.vmdk"
```

## <a name="next-steps"></a>Następne kroki

Informacje dotyczące rozwiązywania problemów podczas konfigurowania kopii zapasowych znajdują się w [przewodniku rozwiązywania problemów Azure Backup Server](./backup-azure-mabs-troubleshoot.md).
