---
title: Środowiska zarządzania wieloma dzierżawami
description: Zarządzanie zasobami delegowanymi przez platformę Azure umożliwia korzystanie z funkcji zarządzania między dzierżawcami.
ms.date: 07/31/2020
ms.topic: conceptual
ms.openlocfilehash: a6d5c7e06ed59ab76b15f4f8ae880408dc6f7835
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/31/2020
ms.locfileid: "87500882"
---
# <a name="cross-tenant-management-experiences"></a>Środowiska zarządzania wieloma dzierżawami

Jako dostawca usług możesz użyć [usługi Azure Lighthouse](../overview.md) do zarządzania zasobami dla wielu klientów z poziomu dzierżawy w [Azure Portal](https://portal.azure.com). Wiele zadań i usług można wykonywać w ramach zarządzanych dzierżawców przy użyciu funkcji [zarządzania zasobami delegowanymi przez platformę Azure](../concepts/azure-delegated-resource-management.md).

> [!NOTE]
> Zarządzanie zasobami delegowanymi przez platformę Azure może być również używane [w przedsiębiorstwie, które ma wiele dzierżawców usługi Azure AD](enterprise.md) , aby uprościć administrację między dzierżawcami.

## <a name="understanding-customer-tenants"></a>Zrozumienie dzierżawców klientów

Dzierżawa usługi Azure Active Directory (Azure AD) jest reprezentacją organizacji. Jest to dedykowane wystąpienie usługi Azure AD, które organizacja otrzymuje podczas tworzenia relacji z firmą Microsoft, rejestrując się na platformie Azure, Microsoft 365 lub innych usługach. Każda dzierżawa usługi Azure AD jest odrębna i oddzielona od innych dzierżaw usługi Azure AD i ma własny identyfikator dzierżawy (GUID). Aby uzyskać więcej informacji, zobacz [co to jest Azure Active Directory?](../../active-directory/fundamentals/active-directory-whatis.md)

Zazwyczaj w celu zarządzania zasobami platformy Azure dla klienta dostawcy usług będą musieli zalogować się do Azure Portal przy użyciu konta skojarzonego z dzierżawcą tego klienta, co wymaga od administratora dzierżawy klienta tworzenia kont użytkowników dla dostawcy usług i zarządzania nimi.

W przypadku usługi Azure Lighthouse proces dołączania określa użytkowników w dzierżawie dostawcy usług, którzy będą mogli korzystać z delegowanych subskrypcji i grup zasobów w dzierżawie klienta. Ci użytkownicy mogą następnie zalogować się do Azure Portal przy użyciu własnych poświadczeń. W ramach Azure Portal mogą zarządzać zasobami należącymi do wszystkich klientów, do których mają dostęp. Można to zrobić, odwiedzając stronę [moi klienci](../how-to/view-manage-customers.md) w Azure Portal lub pracując bezpośrednio w kontekście subskrypcji tego klienta w Azure Portal lub za pośrednictwem interfejsów API.

Usługa Azure Lighthouse zapewnia większą elastyczność zarządzania zasobami dla wielu klientów bez konieczności logowania się do różnych kont w różnych dzierżawach. Na przykład dostawca usług może mieć dwóch klientów z różnymi zakresami obowiązków i poziomów dostępu. Korzystając z usługi Azure Lighthouse, autoryzowani użytkownicy mogą zalogować się do dzierżawy dostawcy usług, aby uzyskać dostęp do tych zasobów.

![Zasoby klienta zarządzane za poorednictwem jednej dzierżawy dostawcy usług](../media/azure-delegated-resource-management-service-provider-tenant.jpg)

## <a name="apis-and-management-tool-support"></a>Obsługa interfejsów API i narzędzi do zarządzania

Zadania zarządzania można wykonywać w odniesieniu do zasobów delegowanych bezpośrednio w portalu lub za pomocą interfejsów API i narzędzi do zarządzania (takich jak interfejs wiersza polecenia platformy Azure i Azure PowerShell). Wszystkie istniejące interfejsy API mogą być używane podczas pracy z delegowanymi zasobami, o ile funkcjonalność jest obsługiwana w przypadku zarządzania między dzierżawcami, a użytkownik ma odpowiednie uprawnienia.

Polecenie interfejsu wiersza polecenia platformy Azure, takie jak [AZ Account List](/cli/azure/account?view=azure-cli-latest#az-account-list) , wyświetla atrybuty **homeTenantId** i **managedByTenants** dla każdej subskrypcji, co pozwala określić, czy zwrócona subskrypcja należy do dzierżawy dostawcy usług, czy do zarządzanej dzierżawy klienta.

> [!TIP]
> Jeśli te wartości nie są wyświetlane podczas korzystania z interfejsu wiersza polecenia platformy Azure, spróbuj wyczyścić pamięć podręczną, uruchamiając polecenie `az account clear` `az login --identity` .

Udostępniamy również interfejsy API, które są specyficzne dla wykonywania zadań Lighthouse platformy Azure. Aby uzyskać więcej informacji, zobacz sekcję dotyczącą **odwołania** .

## <a name="enhanced-services-and-scenarios"></a>Ulepszone usługi i scenariusze

Większość zadań i usług można wykonać w odniesieniu do zasobów delegowanych między zarządzanymi dzierżawcami. Poniżej przedstawiono niektóre kluczowe scenariusze, w których zarządzanie między dzierżawcami może być szczególnie skuteczne.

[Łuk systemu Azure](../../azure-arc/index.yml):

- Zarządzanie serwerami hybrydowymi w skali — [usługa Azure ARC dla serwerów (wersja zapoznawcza)](../../azure-arc/servers/overview.md):
  - [Łączenie maszyn z systemem Windows Server lub Linux poza platformą Azure](../../azure-arc/servers/onboard-portal.md) z delegowanymi subskrypcjami i/lub grupami zasobów na platformie Azure
  - Zarządzanie połączonymi maszynami przy użyciu konstrukcji platformy Azure, takich jak Azure Policy i tagowanie
  - Upewnij się, że ten sam zestaw zasad jest stosowany w środowiskach hybrydowych klientów
  - Używanie Azure Security Center do monitorowania zgodności między środowiskami hybrydowymi klientów
- Zarządzanie klastrami hybrydowymi Kubernetes w skali — [Kubernetes z włączoną funkcją Azure ARC (wersja zapoznawcza)](../../azure-arc/kubernetes/overview.md):
  - [Łączenie klastra Kubernetes z usługą Azure Arc](../../azure-arc/kubernetes/connect-cluster.md) do delegowanych subskrypcji i/lub grup zasobów na platformie Azure
  - [Użyj GitOps](../../azure-arc/kubernetes/use-gitops-connected-cluster.md) dla połączonych klastrów
  - Wymuś zasady w połączonych klastrach

[Azure Automation](../../automation/index.yml):

- Korzystanie z kont usługi Automation w celu uzyskiwania dostępu do delegowanych zasobów klienta i pracy z nim

[Azure Backup](../../backup/index.yml):

- Tworzenie kopii zapasowych i przywracanie danych klienta w dzierżawach klientów
- Użyj [Eksploratora kopii zapasowych](../../backup/monitor-azure-backup-with-backup-explorer.md) , aby ułatwić wyświetlanie informacji operacyjnych dotyczących elementów kopii zapasowej (w tym zasobów platformy Azure, które nie zostały jeszcze skonfigurowane do tworzenia kopii zapasowych) i informacji o monitorowaniu (zadania i alerty) dla delegowanych subskrypcji Eksplorator kopii zapasowych jest obecnie dostępny tylko dla danych maszyny wirtualnej platformy Azure.
- Za pomocą [raportów kopii zapasowych](../../backup/configure-reports.md) w ramach delegowanych subskrypcji można śledzić trendy historyczne, analizować użycie magazynu kopii zapasowych oraz przeprowadzać inspekcję i przywracanie kopii zapasowych.

[Azure Cost Management i rozliczanie](../../cost-management-billing/index.yml):

- Od dzierżawy zarządzającej partnerzy CSP mogą wyświetlać i analizować koszty użycia przed opodatkowaniem (nie włącznie z zakupami) dla klientów, którzy podlegają planowi Azure. Koszt będzie uzależniony od stawek detalicznych i dostępu do kontroli dostępu opartej na rolach (Azure RBAC) dla partnera klienta.

[Usługa Azure Kubernetes Service (AKS)](../../aks/index.yml):

- Zarządzanie hostowanymi środowiskami Kubernetes oraz wdrażanie aplikacji kontenerowych i zarządzanie nimi w ramach dzierżawców klientów

[Azure monitor](../../azure-monitor/index.yml):

- Wyświetlanie alertów dla delegowanych subskrypcji z możliwością wyświetlania alertów we wszystkich subskrypcjach
- Wyświetl szczegóły dziennika aktywności dla delegowanych subskrypcji
- Log Analytics: wykonywanie zapytań dotyczących danych ze zdalnych obszarów roboczych klientów w wielu dzierżawcach
- Tworzenie alertów w dzierżawach klientów, które wyzwalają automatyzację, taką jak Azure Automation elementów Runbook lub Azure Functions, w dzierżawie dostawcy usług za pomocą elementów webhook
- W przypadku obciążeń SAP [Monitoruj metryki rozwiązań SAP z zagregowanym widokiem w dzierżawach klientów](https://techcommunity.microsoft.com/t5/running-sap-applications-on-the/using-azure-lighthouse-and-azure-monitor-for-sap-solutions-to/ba-p/1537293)

[Sieć platformy Azure](../../networking/networking-overview.md):

- Wdrażaj [usługi Azure Virtual Network](../../virtual-network/index.yml) i karty interfejsu sieci wirtualnej (vNICs) oraz zarządzaj nimi w ramach dzierżawców klientów
- Wdrażanie i Konfigurowanie [zapory platformy Azure](../../firewall/overview.md) w celu ochrony zasobów Virtual Network klientów
- Zarządzanie usługami łączności, takimi jak [wirtualne sieci WAN platformy Azure](../../virtual-wan/virtual-wan-about.md), [ExpressRoute](../../expressroute/expressroute-introduction.md)i [bramy sieci VPN](../../vpn-gateway/vpn-gateway-about-vpngateways.md) dla klientów
- Obsługa kluczowych scenariuszy dla [programu msp sieci platformy Azure](../../networking/networking-partners-msp.md) przy użyciu usługi Azure Lighthouse


[Azure Policy](../../governance/policy/index.yml):

- Migawki zgodności pokazują szczegóły przypisanych zasad w ramach delegowanych subskrypcji
- Tworzenie i edytowanie definicji zasad w ramach delegowanej subskrypcji
- Przypisywanie definicji zasad zdefiniowanych przez klienta w ramach delegowanej subskrypcji
- Klienci widzą zasady utworzone przez dostawcę usług wraz ze wszystkimi utworzonymi przez siebie zasadami
- Może [skorygować deployIfNotExists lub zmodyfikować przypisania w ramach dzierżawy klienta](../how-to/deploy-policy-remediation.md)

[Wykres zasobów platformy Azure](../../governance/resource-graph/index.yml):

- Zawiera teraz identyfikator dzierżawy w zwróconych wynikach zapytania, co pozwala na ustalenie, czy subskrypcja należy do dzierżawy klienta lub dostawcy usług.

[Azure Security Center](../../security-center/index.yml):

- Widoczność między dzierżawami
  - Monitoruj zgodność z zasadami zabezpieczeń i zapewniaj pokrycie zabezpieczeń we wszystkich zasobach dzierżawców
  - Ciągłe monitorowanie zgodności z przepisami dla wielu klientów w jednym widoku
  - Monitoruj, klasyfikacja i ustalaj priorytety rekomendacji dotyczących zabezpieczeń z bezpiecznym obliczaniem wyniku
- Zarządzanie Stanami zabezpieczeń między dzierżawcami
  - Zarządzanie zasadami zabezpieczeń
  - Podejmowanie działań dotyczących zasobów niezgodnych z zaleceniami dotyczącymi zabezpieczeń z możliwością podejmowania działań
  - Zbieranie i przechowywanie danych związanych z zabezpieczeniami
- Wykrywanie zagrożeń i ochrona między dzierżawcami
  - Wykrywanie zagrożeń między zasobami dzierżawców
  - Stosowanie zaawansowanych kontrolek ochrony przed zagrożeniami, takich jak dostęp do maszyny wirtualnej just-in-Time (JIT)
  - Konfiguracja grupy zabezpieczeń sieci z ograniczeniami w ramach adaptacyjnej ochrony sieci
  - Upewnij się, że na serwerach działają tylko aplikacje i procesy, które powinny należeć do adaptacyjnego sterowania aplikacjami
  - Monitorowanie zmian ważnych plików i wpisów rejestru przy użyciu monitorowania integralności plików (FIM)

[Wskaźnik na platformie Azure](../../sentinel/multiple-tenants-service-providers.md):

- Zarządzanie zasobami wskaźnikowymi platformy Azure [w dzierżawach klientów](../../sentinel/multiple-tenants-service-providers.md)
- [Śledź ataki i wyświetlaj alerty zabezpieczeń dla wielu dzierżawców klientów](https://techcommunity.microsoft.com/t5/azure-sentinel/using-azure-lighthouse-and-azure-sentinel-to-monitor-across/ba-p/1043899)
- [Wyświetlanie zdarzeń](../../sentinel/multiple-workspace-view.md) w wielu obszarach roboczych wskaźnikowych rozmieszczonych w różnych dzierżawach klientów

[Azure Service Health](../../service-health/index.yml):

- Monitoruj kondycję zasobów klientów za pomocą Azure Resource Health
- Śledzenie kondycji usług platformy Azure używanych przez klientów

[Azure Site Recovery](../../site-recovery/index.yml):

- Zarządzanie opcjami odzyskiwania po awarii dla maszyn wirtualnych platformy Azure w dzierżawach klientów (należy pamiętać, że nie można używać `RunAs` kont do kopiowania rozszerzeń maszyn wirtualnych)

[Virtual Machines platformy Azure](../../virtual-machines/index.yml):

- Korzystanie z rozszerzeń maszyny wirtualnej w celu zapewnienia konfiguracji po wdrożeniu i zadań automatyzacji na maszynach wirtualnych platformy Azure w dzierżawach klientów
- Rozwiązywanie problemów z maszynami wirtualnymi platformy Azure w dzierżawach klientów przy użyciu diagnostyki rozruchu
- Dostęp do maszyn wirtualnych za pomocą konsoli szeregowej w dzierżawach klientów
- Integrowanie maszyn wirtualnych z Azure Key Vault haseł, wpisów tajnych lub kluczy kryptograficznych na potrzeby szyfrowania dysków przy użyciu [tożsamości zarządzanej za pośrednictwem zasad](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/templates/create-keyvault-secret), dzięki czemu klucze tajne są przechowywane w Key Vault w dzierżawach klientów
- Należy pamiętać, że nie można używać Azure Active Directory do logowania zdalnego do maszyn wirtualnych w dzierżawach klientów

Żądania obsługi:

- [Otwórz żądania pomocy technicznej z **pomocy i obsługi technicznej** ](../../azure-portal/supportability/how-to-create-azure-support-request.md#getting-started) w Azure Portal dla delegowanych zasobów (Wybierz plan pomocy technicznej dostępny dla delegowanego zakresu)

## <a name="current-limitations"></a>Bieżące ograniczenia
We wszystkich scenariuszach należy pamiętać o następujących bieżących ograniczeniach:

- Żądania obsługiwane przez Azure Resource Manager można wykonać przy użyciu funkcji zarządzania zasobami delegowanymi przez platformę Azure. Identyfikatory URI operacji dla tych żądań zaczynają się od `https://management.azure.com` . Jednak żądania, które są obsługiwane przez wystąpienie typu zasobu (na przykład dostęp do Key Vault Secret lub dostęp do danych magazynu), nie są obsługiwane przez delegowane zarządzanie zasobami platformy Azure. Identyfikatory URI operacji dla tych żądań zwykle zaczynają się od adresu, który jest unikatowy dla Twojego wystąpienia, takiego jak `https://myaccount.blob.core.windows.net` lub `https://mykeyvault.vault.azure.net/` . Te ostatnie również są zazwyczaj operacjami na danych, a nie operacjami zarządzania.
- Przypisania ról muszą używać [wbudowanych ról](../../role-based-access-control/built-in-roles.md)kontroli dostępu opartej na ROLACH (RBAC). Wszystkie wbudowane role są obecnie obsługiwane przez delegowane zarządzanie zasobami platformy Azure z wyjątkiem właściciela lub wszelkich wbudowanych ról z [`DataActions`](../../role-based-access-control/role-definitions.md#dataactions) uprawnieniem. Rola Administrator dostępu użytkowników jest obsługiwana tylko w przypadku ograniczonego użycia w [przypisywaniu ról do zarządzanych tożsamości](../how-to/deploy-policy-remediation.md#create-a-user-who-can-assign-roles-to-a-managed-identity-in-the-customer-tenant).  Role niestandardowe i [role administratora klasycznej subskrypcji](../../role-based-access-control/classic-administrators.md) nie są obsługiwane.
- W tej chwili można dołączać subskrypcje korzystające z Azure Databricks. Użytkownicy w dzierżawie zarządzającej nie mogą teraz uruchamiać Azure Databricks obszarów roboczych w delegowanej subskrypcji.
- Mimo że można dołączać subskrypcje i grupy zasobów z blokadami zasobów, te blokady nie uniemożliwią wykonywania akcji przez użytkowników w dzierżawie zarządzającej. [Odmów przypisań](../../role-based-access-control/deny-assignments.md) , które chronią zasoby zarządzane przez system, takie jak te utworzone przez aplikacje zarządzane przez platformę Azure lub plany platformy Azure (przypisań odmowy przypisanych do systemu), uniemożliwiają użytkownikom z dzierżawy zarządzającej wykonywanie tych zasobów. Jednak w tej chwili użytkownicy w dzierżawie klienta nie mogą tworzyć własnych przypisań Odmów (przypisań Odmów przez użytkownika).

## <a name="next-steps"></a>Następne kroki

- Dołączanie klientów do zarządzania zasobami delegowanymi przez platformę Azure za [pomocą szablonów Azure Resource Manager](../how-to/onboard-customer.md) lub [opublikowanie oferty usług zarządzanych w witrynie Azure Marketplace](../how-to/publish-managed-services-offers.md).
- [Wyświetlaj klientów i zarządzaj nimi](../how-to/view-manage-customers.md) , przechodząc do **moich klientów** w Azure Portal.
