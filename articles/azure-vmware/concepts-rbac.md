---
title: Pojęcia — kontrola dostępu oparta na rolach (RBAC)
description: Poznaj kluczowe możliwości kontroli dostępu opartej na rolach dla rozwiązań VMware platformy Azure (Automatyczna synchronizacja)
ms.topic: conceptual
ms.date: 06/30/2020
ms.openlocfilehash: 8628c88c300ef8ed271f5e06a8e8dfae40231fec
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "85660906"
---
# <a name="role-based-access-control-rbac-for-azure-vmware-solution-avs"></a>Kontrola dostępu oparta na rolach (RBAC) dla rozwiązań VMware platformy Azure (Automatyczna synchronizacja)

W przypadku wdrożenia lokalnego programu vCenter i ESXi administrator ma dostęp do administrator@vsphere.local konta vCenter i może mieć przypisanych dodatkowych użytkowników i grup Active Directory (AD). Jednak w ramach wdrożenia programu Azure VMware Solution (Automatyczna synchronizacja) administrator nie ma dostępu do konta użytkownika administratora, ale może przypisywać użytkowników i grupy usługi AD do roli CloudAdmin w programie vCenter.  Ponadto użytkownik chmury prywatnej automatycznej synchronizacji nie ma uprawnień dostępu do określonych składników zarządzania obsługiwanych przez firmę Microsoft i zarządzania nimi, takich jak klastry, hosty, magazyny danych i rozproszone przełączniki wirtualne.


W obszarze automatyczna synchronizacja program vCenter ma wbudowanego użytkownika lokalnego o nazwie cloudadmin, który jest przypisany do wbudowanej roli CloudAdmin. Lokalny użytkownik cloudadmin służy do konfigurowania dodatkowych użytkowników w usłudze AD. Rola CloudAdmin, ogólnie rzecz biorąc, ma uprawnienia do tworzenia obciążeń i zarządzania nimi w chmurze prywatnej (maszyn wirtualnych, pul zasobów, magazynach danych i sieciach). Rola CloudAdmin w ramach automatycznej synchronizacji ma określony zestaw uprawnień programu vCenter, które różnią się od innych rozwiązań w chmurze VMware.   

> [!NOTE]
> Automatyczna synchronizacja nie oferuje obecnie ról niestandardowych w programie vCenter lub w portalu automatycznej synchronizacji. 

## <a name="avs-cloudadmin-role-on-vcenter"></a>Automatyczna synchronizacja roli CloudAdmin na serwerze vCenter

Możesz wyświetlić uprawnienia przyznane do roli CloudAdmina automatyczna synchronizacja w chmurze prywatnej chmury.

1. Zaloguj się do klienta SDDC vSphere i przejdź do **menu**  >  **Administracja**.
1. W obszarze **Access Control**wybierz pozycję **role**.
1. Z listy ról wybierz pozycję **CloudAdmin** , a następnie wybierz pozycję **uprawnienia**. 

   :::image type="content" source="media/rbac-cloudadmin-role-privileges.png" alt-text="Jak wyświetlić uprawnienia roli CloudAdmin w kliencie vSphere":::

Rola CloudAdmin w ramach automatycznej synchronizacji ma następujące uprawnienia w programie vCenter. Szczegółowe wyjaśnienie poszczególnych uprawnień można znaleźć w [dokumentacji produktu VMware](https://docs.vmware.com/en/VMware-vSphere/7.0/com.vmware.vsphere.security.doc/GUID-ED56F3C4-77D0-49E3-88B6-B99B8B437B62.html) .

| Privilege | Opis |
| --------- | ----------- |
| **Alarmy** | Potwierdzanie alarmu<br />Utwórz alarm<br />Wyłącz akcję alarmu<br />Modyfikuj alarm<br />Usuń alarm<br />Ustawianie stanu alarmu |
| **Uprawnienia** | Modyfikuj uprawnienia |
| **Biblioteka zawartości** | Dodaj element biblioteki<br />Tworzenie subskrypcji dla opublikowanej biblioteki<br />Utwórz bibliotekę lokalną<br />Utwórz subskrybowaną bibliotekę<br />Usuń element biblioteki<br />Usuń bibliotekę lokalną<br />Usuń subskrybowaną bibliotekę<br />Usuń subskrypcję opublikowanej biblioteki<br />Pobieranie plików<br />Wyklucz elementy biblioteki<br />Wykluczanie subskrybowanej biblioteki<br />Importuj magazyn<br />Informacje o subskrypcji sondy<br />Publikowanie elementu biblioteki dla subskrybentów<br />Publikowanie biblioteki dla subskrybentów<br />Odczytaj magazyn<br />Synchronizuj element biblioteki<br />Synchronizuj subskrybowaną bibliotekę<br />Introspekcji typu<br />Aktualizowanie ustawień konfiguracji<br />Pliki aktualizacji<br />Biblioteka aktualizacji<br />Aktualizuj element biblioteki<br />Aktualizuj bibliotekę lokalną<br />Aktualizuj subskrybowaną bibliotekę<br />Aktualizowanie subskrypcji opublikowanej biblioteki<br />Wyświetl ustawienia konfiguracji |
| **Operacje kryptograficzne** | Bezpośredni dostęp |
| **Magazyn danych** | Przydziel miejsce<br />Przeglądaj magazyn danych<br />Konfigurowanie magazynu danych<br />Operacje na plikach niskiego poziomu<br />Usuń pliki<br />Aktualizowanie metadanych maszyny wirtualnej |
| **Folder** | Utwórz folder<br />Usuń folder<br />Przenieś folder<br />Zmień nazwę folderu |
| **Globalnie** | Anuluj zadanie<br />Tag globalny<br />Służba zdrowia<br />Rejestruj zdarzenie<br />Zarządzanie atrybutami niestandardowymi<br />Menedżerowie usług<br />Ustaw atrybut niestandardowy<br />Tag systemowy |
| **Host** | Replikacja vSphere<br />&#160;&#160;&#160;&#160;zarządzanie replikacją |
| **tagowanie vSphere** | Przypisywanie i cofanie przypisania znacznika vSphere<br />Utwórz tag vSphere<br />Utwórz kategorię tagów vSphere<br />Usuń tag vSphere<br />Usuń kategorię tagu vSphere<br />Edytuj tag vSphere<br />Edytuj kategorię tagów vSphere<br />Modyfikuj pole UsedBy dla kategorii<br />Modyfikuj pole UsedBy dla tagu |
| **Sieć** | Przypisywanie sieci |
| **Zasób** | Zastosuj zalecenie<br />Przypisywanie vApp do puli zasobów<br />Przypisz maszynę wirtualną do puli zasobów<br />Utwórz pulę zasobów<br />Migrowanie wyłączone z maszyny wirtualnej<br />Migrowanie na maszynie wirtualnej<br />Modyfikuj pulę zasobów<br />Przenieś pulę zasobów<br />VMotion zapytania<br />Usuń pulę zasobów<br />Zmień nazwę puli zasobów |
| **Zaplanowane zadanie** | Tworzenie zadania<br />Modyfikowanie zadania<br />Usuń zadanie<br />Uruchom zadanie |
| **Sesje** | Komunikat<br />Weryfikuj sesję |
| **Profil** | Widok magazynu oparty na profilach |
| **Widok magazynu** | Widok |
| **vApp** | Dodaj maszynę wirtualną<br />Przypisz pulę zasobów<br />Przypisz vApp<br />Klonowanie<br />Utwórz<br />Usuń<br />Eksportowanie<br />Importuj<br />Move<br />Wyłączanie<br />Włącz<br />Zmień nazwę<br />Wstrzymanie<br />Unregister<br />Wyświetl środowisko OVF<br />Konfiguracja aplikacji vApp<br />Konfiguracja wystąpienia vApp<br />Konfiguracja vApp zarządzane<br />Konfiguracja zasobów vApp |
| **Maszyna wirtualna** | Zmień konfigurację<br />&#160;&#160;&#160;&#160;uzyskać dzierżawy dysku<br />&#160;&#160;&#160;&#160;dodać istniejącego dysku<br />&#160;&#160;&#160;&#160;dodać nowego dysku<br />&#160;&#160;&#160;&#160;dodać lub usunąć urządzenie<br />&#160;&#160;&#160;&#160;Konfiguracja zaawansowana<br />&#160;&#160;&#160;&#160;Zmień liczbę procesorów<br />&#160;&#160;&#160;&#160;Zmień pamięć<br />&#160;&#160;&#160;&#160;zmienić ustawień<br />&#160;&#160;&#160;&#160;zmienić rozmieszczenie swapfile<br />&#160;&#160;&#160;&#160;zmienić zasobu<br />&#160;&#160;&#160;&#160;skonfigurować urządzenie USB hosta<br />&#160;&#160;&#160;&#160;skonfigurować urządzenie RAW<br />&#160;&#160;&#160;&#160;skonfigurować zarządzane<br />&#160;&#160;&#160;&#160;wyświetlania ustawień połączenia<br />&#160;&#160;&#160;&#160;zwiększyć dysku wirtualnego<br />&#160;&#160;&#160;&#160;zmodyfikować ustawień urządzenia<br />&#160;&#160;&#160;&#160;zgodności z odpornością błędów zapytania<br />&#160;&#160;&#160;&#160;plików nienależących do zapytania<br />&#160;&#160;&#160;&#160;Załaduj ponownie ze ścieżek<br />&#160;&#160;&#160;&#160;usunąć dysku<br />&#160;&#160;&#160;&#160;zmienić nazwy<br />&#160;&#160;&#160;&#160;resetowania informacji o gościu<br />&#160;&#160;&#160;&#160;ustawić adnotację<br />&#160;&#160;&#160;&#160;Przełącz śledzenie zmian dysku<br />&#160;&#160;&#160;&#160;Przełącz element nadrzędny rozwidlenia<br />&#160;&#160;&#160;&#160;uaktualnić zgodność maszyny wirtualnej<br />Edytuj spis<br />&#160;&#160;&#160;&#160;utworzyć na podstawie istniejącego<br />&#160;&#160;&#160;&#160;Utwórz nowy<br />&#160;&#160;&#160;&#160;Przenieś<br />Rejestr &#160;&#160;&#160;&#160;<br />&#160;&#160;&#160;&#160;usunąć<br />Wyrejestrowywanie &#160;&#160;&#160;&#160;<br />Operacje gościa<br />&#160;&#160;&#160;&#160;modyfikowanie aliasu operacji gościa<br />Zapytanie aliasu operacji gościa &#160;&#160;&#160;&#160;<br />&#160;&#160;&#160;&#160;modyfikacje operacji gościa<br />&#160;&#160;&#160;&#160;wykonywania programu operacji gościa<br />&#160;&#160;&#160;&#160;zapytań dotyczących operacji gościa<br />Interakcja<br />&#160;&#160;&#160;&#160;pytania odpowiedzi<br />&#160;&#160;&#160;&#160;operacji tworzenia kopii zapasowej na maszynie wirtualnej<br />&#160;&#160;&#160;&#160;skonfigurować nośnik CD<br />&#160;&#160;&#160;&#160;skonfigurować dyskietkę<br />&#160;&#160;&#160;&#160;Łączenie urządzeń<br />Interakcja z konsolą &#160;&#160;&#160;&#160;<br />&#160;&#160;&#160;&#160;utworzyć zrzut ekranu<br />&#160;&#160;&#160;&#160;defragmentacja wszystkich dysków<br />&#160;&#160;&#160;&#160;przeciąganie i upuszczanie<br />&#160;&#160;&#160;&#160;zarządzanie systemem operacyjnym gościa za pomocą interfejsu API VIX<br />&#160;&#160;&#160;&#160;wstrzyknąć kody skanowania HID USB<br />&#160;&#160;&#160;&#160;instalacji narzędzi VMware<br />&#160;&#160;&#160;&#160;Wstrzymywanie lub wstrzymywanie wstrzymania<br />&#160;&#160;&#160;&#160;wykonywanie operacji czyszczenia lub zmniejszania<br />&#160;&#160;&#160;&#160;wyłączyć<br />&#160;&#160;&#160;&#160;Włącz<br />&#160;&#160;&#160;&#160;rejestrowania sesji na maszynie wirtualnej<br />&#160;&#160;&#160;&#160;powtarzania sesji na maszynie wirtualnej<br />&#160;&#160;&#160;&#160;wstrzymywanie<br />&#160;&#160;&#160;&#160;wstrzymywanie odporności na uszkodzenia<br />&#160;&#160;&#160;&#160;test pracy w trybie failover<br />&#160;&#160;&#160;&#160;ponownie uruchom pomocniczą maszynę wirtualną<br />&#160;&#160;&#160;&#160;wyłączyć odporność na uszkodzenia<br />&#160;&#160;&#160;&#160;włączyć odporności na uszkodzenia<br />Inicjowanie obsługi<br />&#160;&#160;&#160;&#160;zezwolić na dostęp do dysku<br />&#160;&#160;&#160;&#160;zezwolić na dostęp do plików<br />&#160;&#160;&#160;&#160;zezwolić na dostęp do dysku tylko do odczytu<br />&#160;&#160;&#160;&#160;zezwolić na pobieranie maszyny wirtualnej<br />&#160;&#160;&#160;&#160;szablon klonowania<br />&#160;&#160;&#160;&#160;klonowania maszyny wirtualnej<br />&#160;&#160;&#160;&#160;utworzyć szablonu z maszyny wirtualnej<br />&#160;&#160;&#160;&#160;dostosować gościa<br />Szablon wdrażania &#160;&#160;&#160;&#160;<br />&#160;&#160;&#160;&#160;Oznacz jako szablon<br />&#160;&#160;&#160;&#160;zmodyfikować specyfikacji dostosowania<br />&#160;&#160;&#160;&#160;podwyższanie poziomu dysków<br />&#160;&#160;&#160;&#160;Specyfikacja dostosowania odczytu<br />Konfiguracja usługi<br />&#160;&#160;&#160;&#160;Zezwalaj na powiadomienia<br />&#160;&#160;&#160;&#160;zezwolić na sondowanie globalnych powiadomień o zdarzeniach<br />&#160;&#160;&#160;&#160;zarządzać konfiguracją usługi<br />&#160;&#160;&#160;&#160;zmodyfikować konfiguracji usługi<br />&#160;&#160;&#160;&#160;konfiguracja usługi zapytań<br />&#160;&#160;&#160;&#160;odczytać konfiguracji usługi<br />Zarządzanie migawkami<br />&#160;&#160;&#160;&#160;utworzyć migawki<br />&#160;&#160;&#160;&#160;usunąć migawki<br />&#160;&#160;&#160;&#160;Zmień nazwę migawki<br />&#160;&#160;&#160;&#160;przywrócenia migawki<br />Replikacja vSphere<br />&#160;&#160;&#160;&#160;skonfigurować replikację<br />&#160;&#160;&#160;&#160;zarządzanie replikacją<br />&#160;&#160;&#160;&#160;replikacji monitora |
| **vService** | Utwórz zależność<br />Zniszcz zależność<br />Skonfiguruj ponownie konfigurację zależności<br />Aktualizuj zależność |



## <a name="next-steps"></a>Następne kroki

Szczegółowe wyjaśnienie poszczególnych uprawnień można znaleźć w [dokumentacji produktu VMware](https://docs.vmware.com/en/VMware-vSphere/7.0/com.vmware.vsphere.security.doc/GUID-ED56F3C4-77D0-49E3-88B6-B99B8B437B62.html) .

<!-- LINKS - internal -->

<!-- LINKS - external-->
[VMware product documentation]: https://docs.vmware.com/en/VMware-vSphere/7.0/com.vmware.vsphere.security.doc/GUID-ED56F3C4-77D0-49E3-88B6-B99B8B437B62.html

