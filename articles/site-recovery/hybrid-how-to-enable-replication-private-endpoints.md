---
title: Włącz replikację dla maszyn lokalnych z prywatnymi punktami końcowymi w Azure Site Recovery
description: W tym artykule opisano sposób konfigurowania replikacji dla maszyn lokalnych z prywatnymi punktami końcowymi przy użyciu Site Recovery.
author: mayurigupta13
ms.author: mayg
ms.service: site-recovery
ms.topic: article
ms.date: 07/14/2020
ms.openlocfilehash: c91c92a18570f569b6364b646e6fdf7bf534802f
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/23/2020
ms.locfileid: "87098829"
---
# <a name="replicate-on-premises-machines-with-private-endpoints"></a>Replikowanie maszyn lokalnych ze prywatnymi punktami końcowymi

Azure Site Recovery umożliwia używanie prywatnych punktów końcowych prywatnego [linku platformy Azure](../private-link/private-endpoint-overview.md) do replikowania maszyn lokalnych do sieci wirtualnej na platformie Azure. Obsługa dostępu do prywatnego punktu końcowego do magazynu odzyskiwania jest obsługiwana w następujących regionach:

- Komercyjne platformy Azure: Południowo-środkowe stany USA, zachodnie stany USA 2, Wschodnie stany USA
- Azure Government: US Gov Wirginia, US Gov Arizona, US Gov Teksas, US DoD (region wschodni), US DoD (region środkowy)

Ten artykuł zawiera instrukcje dotyczące wykonywania następujących czynności:

- Utwórz magazyn Recovery Services Azure Backup, aby chronić maszyny.
- Włącz zarządzaną tożsamość magazynu i przyznaj uprawnienia wymagane do uzyskiwania dostępu do kont magazynu klienta, aby replikować ruch z zasobów lokalnych do lokalizacji docelowych platformy Azure. Zarządzany dostęp do tożsamości dla magazynu jest wymagany podczas konfigurowania prywatnego dostępu do magazynu.
- Wprowadź zmiany DNS wymagane dla prywatnych punktów końcowych
- Tworzenie i zatwierdzanie prywatnych punktów końcowych dla magazynu w sieci wirtualnej
- Utwórz prywatne punkty końcowe dla kont magazynu. W razie potrzeby można nadal zezwalać na dostęp do magazynu publicznego lub przez zaporę. Tworzenie prywatnego punktu końcowego do uzyskiwania dostępu do magazynu nie jest obowiązkowe dla Azure Site Recovery.
  
Poniżej znajduje się Architektura referencyjna dotycząca sposobu, w jaki przepływ pracy replikacji zmienia się dla hybrydowego odzyskiwania po awarii za pomocą prywatnych punktów końcowych. Nie można utworzyć prywatnych punktów końcowych w sieci lokalnej. Aby można było używać linków prywatnych, należy utworzyć sieć wirtualną platformy Azure (o nazwie "Obejdź sieć" w tym artykule), nawiązać połączenie prywatne między siecią lokalną i pominięciem, a następnie utworzyć prywatne punkty końcowe w sieci pomijania. Wybór łączności prywatnej zależy od własnego uznania.

:::image type="content" source="./media/hybrid-how-to-enable-replication-private-endpoints/architecture.png" alt-text="Architektura referencyjna dla Site Recovery z prywatnymi punktami końcowymi.":::

## <a name="prerequisites-and-caveats"></a>Wymagania wstępne i zastrzeżenia

- Obsługa linków prywatnych jest włączona dla infrastruktury Site Recovery z wersją **9,35** i nowszą.
- Prywatne punkty końcowe można tworzyć tylko dla nowych magazynów Recovery Services, które nie mają żadnych elementów zarejestrowanych w magazynie. W związku z tym należy utworzyć prywatne punkty końcowe **przed dodaniem jakichkolwiek elementów do magazynu**. Zapoznaj się ze strukturą cenową [prywatnych punktów końcowych](https://azure.microsoft.com/pricing/details/private-link/).
- Po utworzeniu prywatnego punktu końcowego dla magazynu magazyn jest zablokowany i **nie jest dostępny z sieci innych niż te sieci, które mają prywatne punkty końcowe**.
- Azure Active Directory obecnie nie obsługuje prywatnych punktów końcowych. W związku z tym adresy IP i w pełni kwalifikowane nazwy domen wymagane do Azure Active Directory pracy w regionie muszą mieć dozwolony dostęp wychodzący z zabezpieczonej sieci wirtualnej platformy Azure. Można również użyć tagów grup zabezpieczeń sieci "Azure Active Directory" i tagów zapory platformy Azure, aby umożliwić dostęp do Azure Active Directory, zgodnie z wymaganiami.
- W sieci pomijania, w której tworzysz prywatny punkt końcowy, **są wymagane pięć adresów IP** . Podczas tworzenia prywatnego punktu końcowego dla magazynu Site Recovery tworzy pięć prywatnych linków, aby uzyskać dostęp do jego mikrousług.
- **Jeden dodatkowy adres IP jest wymagany** w sieci pomijanie łączności z prywatnym punktem końcowym z kontem magazynu pamięci podręcznej. Metoda łączności, taka jak Internet lub [ExpressRoute](../expressroute/index.yml), między środowiskiem lokalnym a punktem końcowym konta magazynu znajduje się na Twoim opisie. Nawiązywanie połączenia prywatnego jest opcjonalne. Prywatne punkty końcowe dla magazynu można utworzyć tylko w przypadku typu Ogólnego przeznaczenia v2. Zapoznaj się ze strukturą cenową [transferu danych w witrynie GPv2](https://azure.microsoft.com/pricing/details/storage/page-blobs/).

 ## <a name="creating-and-using-private-endpoints-for-site-recovery"></a>Tworzenie i używanie prywatnych punktów końcowych dla Site Recovery

 W tej sekcji omówiono kroki związane z tworzeniem i używaniem prywatnych punktów końcowych w celu Azure Site Recovery wewnątrz sieci wirtualnych.

> [!NOTE]
> Zdecydowanie zaleca się wykonanie tych kroków w tej samej kolejności, w jakiej zostały podane. Niewykonanie tej czynności może prowadzić do tego, że magazyn, który jest renderowany, nie może używać prywatnych punktów końcowych i wymaga ponownego uruchomienia procesu z nowym magazynem.

## <a name="create-a-recovery-services-vault"></a>Utwórz magazyn usługi Recovery Services.

Magazyn usługi Recovery Services jest jednostką, która zawiera informacje o replikacji maszyn i służy do wyzwalania operacji Site Recovery. Aby uzyskać instrukcje dotyczące tworzenia magazynu usługi Recovery Services w regionie świadczenia usługi Azure, w którym ma zostać przeprowadzona praca awaryjna w przypadku awarii, zobacz [Tworzenie magazynu Recovery Services](./azure-to-azure-tutorial-enable-replication.md#create-a-recovery-services-vault).

## <a name="enable-the-managed-identity-for-the-vault"></a>Włącz zarządzaną tożsamość magazynu.

[Zarządzana tożsamość](../active-directory/managed-identities-azure-resources/overview.md) zezwala magazynowi na uzyskanie dostępu do kont magazynu klienta. Site Recovery musi uzyskać dostęp do magazynu docelowego i kont magazynu pamięci podręcznej/dziennika w zależności od wymagań scenariusza. Dostęp do tożsamości zarządzanej jest istotny, gdy korzystasz z usługi linki prywatne dla magazynu.

1. Przejdź do magazynu Recovery Services. W obszarze _Ustawienia_wybierz pozycję **tożsamość** .

   :::image type="content" source="./media/hybrid-how-to-enable-replication-private-endpoints/enable-managed-identity-in-vault.png" alt-text="Pokazuje Azure Portal i Recovery Services strony.":::

1. Zmień **stan** na _włączone_ i wybierz pozycję **Zapisz**.

1. Generowany jest **Identyfikator obiektu** wskazujący, że magazyn jest teraz zarejestrowany w Azure Active Directory.

## <a name="create-private-endpoints-for-the-recovery-services-vault"></a>Tworzenie prywatnych punktów końcowych dla magazynu Recovery Services

Potrzebujesz jednego prywatnego punktu końcowego dla magazynu w sieci pomijania do ochrony maszyn w lokalnej sieci źródłowej. Utwórz prywatny punkt końcowy przy użyciu prywatnego centrum linku w portalu lub za pośrednictwem [Azure PowerShell](../private-link/create-private-endpoint-powershell.md).

1. Na pasku wyszukiwania Azure Portal Wyszukaj i wybierz pozycję "link prywatny". Ta akcja spowoduje przejście do prywatnego centrum linków.

   :::image type="content" source="./media/hybrid-how-to-enable-replication-private-endpoints/search-private-links.png" alt-text="Pokazuje przeszukiwanie Azure Portal dla prywatnego centrum linków.":::

1. Na pasku nawigacyjnym po lewej stronie wybierz pozycję **prywatne punkty końcowe**. Na stronie prywatne punkty końcowe wybierz pozycję ** \+ Dodaj** , aby rozpocząć tworzenie prywatnego punktu końcowego dla magazynu.

   :::image type="content" source="./media/hybrid-how-to-enable-replication-private-endpoints/create-private-endpoints.png" alt-text="Pokazuje, jak utworzyć prywatny punkt końcowy w prywatnym centrum połączeń.":::

1. Raz w środowisku "Tworzenie prywatnego punktu końcowego" wymagane jest określenie szczegółów dotyczących tworzenia połączenia prywatnego punktu końcowego.

   1. **Podstawowe**: Podaj podstawowe informacje o prywatnych punktach końcowych. Region powinien być taki sam jak w przypadku sieci pomijania.

      :::image type="content" source="./media/hybrid-how-to-enable-replication-private-endpoints/create-private-endpoints-basic-tab.png" alt-text="Pokazuje kartę podstawowe, szczegóły projektu, subskrypcję i inne powiązane pola służące do tworzenia prywatnego punktu końcowego w Azure Portal.":::

   1. **Zasób**: na tej karcie należy wspomnieć zasób platformy jako usługi, dla którego chcesz utworzyć połączenie. Wybierz pozycję _Microsoft. RecoveryServices/magazyny_ z **typu zasobu** dla wybranej subskrypcji. Następnie wybierz nazwę magazynu Recovery Services dla **zasobu** i ustaw _Azure Site Recovery_ jako **docelowy zasób podrzędny**.

      :::image type="content" source="./media/hybrid-how-to-enable-replication-private-endpoints/create-private-endpoints-resource-tab.png" alt-text="Pokazuje kartę zasób, typ zasobu, zasób i docelowe pola zasobu podrzędnego do konsolidacji do prywatnego punktu końcowego w Azure Portal.":::

   1. **Konfiguracja**: w obszarze Konfiguracja określ opcję Pomijaj sieć i podsieć, w której ma zostać utworzony prywatny punkt końcowy. Włącz integrację z prywatną strefą DNS, wybierając opcję **tak**.
      Wybierz już utworzoną strefę DNS lub Utwórz nową. Wybranie opcji **tak** automatycznie łączy strefę z siecią pomijania i dodaje rekordy DNS, które są wymagane do rozpoznawania nazw DNS nowych adresów IP i w pełni kwalifikowanych nazwy domen utworzonych dla prywatnego punktu końcowego.

      Upewnij się, że wybrano opcję utworzenia nowej strefy DNS dla każdego nowego prywatnego punktu końcowego łączącego się z tym samym magazynem. W przypadku wybrania istniejącej prywatnej strefy DNS poprzednie rekordy CNAME są zastępowane. Przed kontynuowaniem zapoznaj się ze [wskazówkami dotyczącymi prywatnych punktów końcowych](../private-link/private-endpoint-overview.md#private-endpoint-properties) .

      Jeśli środowisko ma model koncentratora i szprychy, musisz tylko jeden prywatny punkt końcowy i tylko jedną prywatną strefę DNS dla całej konfiguracji, ponieważ wszystkie sieci wirtualne mają już włączone komunikację równorzędną między nimi. Aby uzyskać więcej informacji, zobacz [prywatny punkt końcowy usługi DNS](../private-link/private-endpoint-dns.md#virtual-network-workloads-without-custom-dns-server).

      Aby ręcznie utworzyć prywatną strefę DNS, wykonaj kroki opisane w temacie [tworzenie prywatnych stref DNS i ręczne Dodawanie rekordów DNS](#create-private-dns-zones-and-add-dns-records-manually).

      :::image type="content" source="./media/hybrid-how-to-enable-replication-private-endpoints/create-private-endpoints-configuration-tab.png" alt-text="Przedstawia kartę Konfiguracja zawierającą pola Integracja sieci i usługi DNS w celu skonfigurowania prywatnego punktu końcowego w Azure Portal.":::

   1. **Tagi**: Opcjonalnie możesz dodać Tagi dla prywatnego punktu końcowego.

   1. **Przegląd \+ tworzenia**: po zakończeniu walidacji wybierz pozycję **Utwórz** , aby utworzyć prywatny punkt końcowy.

Po utworzeniu prywatnego punktu końcowego do prywatnego punktu końcowego dodawane są pięć w pełni kwalifikowanych nazw domen. Te linki umożliwiają dostęp do komputerów w sieci lokalnej w celu uzyskania dostępu za pośrednictwem sieci pomijania do wszystkich wymaganych Site Recovery mikrousług w kontekście magazynu. Ten sam prywatny punkt końcowy może być używany do ochrony dowolnej maszyny platformy Azure w sieci pomijania i wszystkich komunikacji równorzędnej.

Pięć nazw domen jest sformatowanych przy użyciu następującego wzorca:

`{Vault-ID}-asr-pod01-{type}-.{target-geo-code}.siterecovery.windowsazure.com`

## <a name="approve-private-endpoints-for-site-recovery"></a>Zatwierdź prywatne punkty końcowe dla Site Recovery

Jeśli użytkownik tworzący prywatny punkt końcowy jest również właścicielem magazynu Recovery Services, prywatny punkt końcowy utworzony powyżej zostanie zaakceptowany w ciągu kilku minut. W przeciwnym razie właściciel magazynu musi zatwierdzić prywatny punkt końcowy przed jego użyciem. Aby zatwierdzić lub odrzucić żądane połączenie prywatnego punktu końcowego, przejdź do pozycji **prywatne połączenia punktów końcowych** w obszarze "Ustawienia" na stronie Magazyn odzyskiwania.

Aby sprawdzić stan połączenia przed kontynuowaniem, można przejść do zasobu prywatnego punktu końcowego.

:::image type="content" source="./media/hybrid-how-to-enable-replication-private-endpoints/vault-private-endpoint-connections.png" alt-text="Pokazuje stronę połączenia prywatnego punktu końcowego magazynu i listę połączeń w Azure Portal.":::

## <a name="optional-create-private-endpoints-for-the-cache-storage-account"></a><a name="create-private-endpoints-for-the-cache-storage-account"></a>Obowiązkowe Utwórz prywatne punkty końcowe dla konta magazynu pamięci podręcznej

Można użyć prywatnego punktu końcowego w usłudze Azure Storage. Tworzenie prywatnych punktów końcowych dostępu do magazynu jest _opcjonalne_ dla replikacji Azure Site Recovery. Podczas tworzenia prywatnego punktu końcowego dla magazynu potrzebny jest prywatny punkt końcowy dla konta magazynu pamięci podręcznej/dziennika w sieci wirtualnej pomijania.

> [!NOTE]
> Prywatny punkt końcowy dla magazynu można utworzyć tylko na kontach magazynu **ogólnego przeznaczenia v2** . Aby uzyskać informacje o cenach, zobacz [standardowe ceny obiektów BLOB na stronie](https://azure.microsoft.com/pricing/details/storage/page-blobs/).

Postępuj zgodnie ze [wskazówkami dotyczącymi tworzenia prywatnego magazynu](../private-link/create-private-endpoint-storage-portal.md#create-your-private-endpoint) , aby utworzyć konto magazynu z prywatnym punktem końcowym. Upewnij się, że wybierz opcję **tak** , aby przeprowadzić integrację z prywatną strefą DNS. Wybierz już utworzoną strefę DNS lub Utwórz nową.

## <a name="grant-required-permissions-to-the-vault"></a>Przyznawanie wymaganych uprawnień do magazynu

W zależności od konfiguracji może być wymagane co najmniej jedno konto magazynu w docelowym regionie platformy Azure. Następnie Udziel uprawnień zarządzanej tożsamości dla wszystkich kont magazynu pamięci podręcznej/dzienników wymaganych przez Site Recovery. W takim przypadku należy wcześniej utworzyć wymagane konta magazynu.

Przed włączeniem replikacji maszyn wirtualnych, zarządzana tożsamość magazynu musi mieć następujące uprawnienia roli w zależności od typu konta magazynu:

- Konta magazynu oparte na Menedżer zasobów (typ standardowy):
  - [Współautor](../role-based-access-control/built-in-roles.md#contributor)
  - [Współautor danych obiektu blob magazynu](../role-based-access-control/built-in-roles.md#storage-blob-data-contributor)
- Konta magazynu oparte na Menedżer zasobów (typ warstwy Premium):
  - [Współautor](../role-based-access-control/built-in-roles.md#contributor)
  - [Właściciel danych obiektów blob magazynu](../role-based-access-control/built-in-roles.md#storage-blob-data-owner)
- Klasyczne konta magazynu:
  - [Współautor klasycznego konta magazynu](../role-based-access-control/built-in-roles.md#classic-storage-account-contributor)
  - [Rola usługi operatora kluczy klasycznego konta magazynu](../role-based-access-control/built-in-roles.md#classic-storage-account-key-operator-service-role)

W poniższych krokach opisano, jak dodać przypisanie roli do kont magazynu, pojedynczo:

1. Przejdź do konta magazynu i przejdź do obszaru **kontroli dostępu (IAM)** w lewej części strony.

1. Po włączeniu **kontroli dostępu (IAM)** w polu "Dodaj przypisanie roli" Wybierz pozycję **Dodaj**.

   :::image type="content" source="./media/hybrid-how-to-enable-replication-private-endpoints/storage-role-assignment.png" alt-text="Pokazuje stronę kontroli dostępu (IAM) na koncie magazynu i przycisk Dodaj przypisanie roli w Azure Portal.":::

1. Na stronie "Dodawanie przypisania roli" Wybierz rolę z powyższej listy na liście rozwijanej **rola** . Wprowadź **nazwę** magazynu i wybierz pozycję **Zapisz**.

   :::image type="content" source="./media/hybrid-how-to-enable-replication-private-endpoints/storage-role-assignment-select-role.png" alt-text="Pokazuje stronę kontroli dostępu (IAM) na koncie magazynu oraz opcje wyboru roli i jednostki, do której ma zostać przyznana rola w Azure Portal.":::

Oprócz tych uprawnień usługi firmy MS muszą mieć również dostęp do programu. Przejdź do obszaru "zapory i sieci wirtualne" i wybierz opcję "Zezwalaj na dostęp zaufanych usług firmy Microsoft do tego konta magazynu" w **wyjątkach**.

## <a name="protect-your-virtual-machines"></a>Ochrona maszyn wirtualnych

Po zakończeniu wszystkich powyższych konfiguracji przejdź do konfiguracji infrastruktury lokalnej.

- [Wdróż serwer konfiguracji dla oprogramowania VMware i maszyn fizycznych](./vmware-azure-deploy-configuration-server.md)
- LUB [Skonfiguruj środowisko funkcji Hyper-V na potrzeby replikacji](./hyper-v-azure-tutorial.md#set-up-the-source-environment)

Po zakończeniu instalacji Włącz replikację dla maszyn źródłowych. Upewnij się, że instalacja infrastruktury odbywa się dopiero po utworzeniu prywatnych punktów końcowych dla magazynu w sieci pomijania.

## <a name="create-private-dns-zones-and-add-dns-records-manually"></a>Utwórz prywatne strefy DNS i ręcznie Dodaj rekordy DNS

Jeśli nie wybrano opcji integracji z prywatną strefą DNS w momencie tworzenia prywatnego punktu końcowego dla magazynu, wykonaj kroki opisane w tej sekcji.

Utwórz jedną prywatną strefę DNS, aby zezwolić dostawcy Site Recovery (dla maszyn z funkcją Hyper-V) lub serwera przetwarzania (w przypadku maszyn wirtualnych VMware/fizycznych) w celu rozpoznania prywatnych adresów IP w pełni kwalifikowanych nazw domen.

1. Tworzenie prywatnej strefy DNS

   1. Wyszukaj ciąg "Prywatna strefa DNS Zone" na pasku wyszukiwania **wszystkie usługi** i wybierz pozycję "strefy prywatna strefa DNS" z listy rozwijanej.

      :::image type="content" source="./media/hybrid-how-to-enable-replication-private-endpoints/search-private-dns-zone.png" alt-text="Pokazuje wyszukiwanie prywatne strefy DNS na nowych zasobach w Azure Portal.":::

   1. Na stronie "Prywatna strefa DNS Zones" Wybierz przycisk ** \+ Dodaj** , aby rozpocząć tworzenie nowej strefy.

   1. Na stronie "Tworzenie prywatnej strefy DNS" Wprowadź wymagane szczegóły. Wprowadź nazwę prywatnej strefy DNS jako `privatelink.siterecovery.windowsazure.com` . Możesz wybrać dowolną grupę zasobów i dowolną subskrypcję, aby ją utworzyć.

      :::image type="content" source="./media/hybrid-how-to-enable-replication-private-endpoints/create-private-dns-zone.png" alt-text="Pokazuje kartę podstawowe strony Tworzenie Prywatna strefa DNS strefy i powiązane szczegóły projektu w Azure Portal.":::

   1. Przejdź do karty **Recenzja \+ Create (Tworzenie** ), aby przejrzeć i utworzyć strefę DNS.

1. Połącz prywatną strefę DNS z siecią wirtualną

   Prywatna strefa DNS utworzona powyżej musi teraz być połączona z obejściem.

   1. Przejdź do prywatnej strefy DNS, która została utworzona w poprzednim kroku, i przejdź do pozycji **linki sieci wirtualnej** w lewej części strony. Po wybraniu tej opcji wybierz przycisk ** \+ Dodaj** .

   1. Wprowadź wymagane szczegóły. Pola **subskrypcji** i **sieci wirtualnej** muszą być wypełnione odpowiednimi szczegółami sieci pomijania. Pozostałe pola muszą pozostać w postaci, w jakiej jest.

      :::image type="content" source="./media/hybrid-how-to-enable-replication-private-endpoints/add-virtual-network-link.png" alt-text="Pokazuje stronę umożliwiającą dodanie linku sieci wirtualnej z nazwą łącza, subskrypcją i pokrewną siecią wirtualną w Azure Portal.":::

1. Dodawanie rekordów DNS

   Po utworzeniu wymaganej prywatnej strefy DNS i prywatnego punktu końcowego należy dodać rekordy DNS do strefy DNS.

   > [!NOTE]
   > W przypadku korzystania z niestandardowej prywatnej strefy DNS upewnij się, że podobne wpisy zostały opisane poniżej.

   Ten krok wymaga wprowadzenia wpisów dla każdej w pełni kwalifikowanej nazwy domeny w prywatnym punkcie końcowym do prywatnej strefy DNS.

   1. Przejdź do prywatnej strefy DNS i przejdź do sekcji **Przegląd** w lewej części strony. Po wybraniu tej opcji wybierz pozycję ** \+ zestaw rekordów** , aby rozpocząć Dodawanie rekordów.

   1. Na otwartej stronie "Dodawanie zestawu rekordów" Dodaj wpis dla każdej w pełni kwalifikowanej nazwy domeny i prywatnego adresu IP jako rekord _typu._ Listę w pełni kwalifikowanych nazw domen i adresów IP można uzyskać ze strony "prywatny punkt końcowy" w temacie **Omówienie**. Jak pokazano w poniższym przykładzie, pierwsza w pełni kwalifikowana nazwa domeny z prywatnego punktu końcowego jest dodawana do zestawu rekordów w prywatnej strefie DNS.

      Te w pełni kwalifikowane nazwy domen pasują do wzorca:`{Vault-ID}-asr-pod01-{type}-.{target-geo-code}.siterecovery.windowsazure.com`

      :::image type="content" source="./media/hybrid-how-to-enable-replication-private-endpoints/add-record-set.png" alt-text="Pokazuje stronę umożliwiającą dodanie rekordu typu DNS dla w pełni kwalifikowanej nazwy domeny do prywatnego punktu końcowego w Azure Portal.":::

## <a name="next-steps"></a>Następne kroki

Teraz, po włączeniu prywatnych punktów końcowych dla replikacji maszyny wirtualnej, zobacz te inne strony, aby uzyskać dodatkowe i pokrewne informacje:

- [Wdrażanie lokalnego serwera konfiguracji](./vmware-azure-deploy-configuration-server.md)
- [Konfigurowanie odzyskiwania po awarii lokalnych maszyn wirtualnych funkcji Hyper-V na platformie Azure](./hyper-v-azure-tutorial.md)
