---
title: Dowiedz się, jak przeprowadzić inspekcję zawartości maszyn wirtualnych
description: Dowiedz się, w jaki sposób Azure Policy używa agenta konfiguracji gościa do inspekcji ustawień wewnątrz maszyn wirtualnych.
ms.date: 05/20/2020
ms.topic: conceptual
ms.openlocfilehash: f2f07a3e88984a84ca1529052d5899ad8570a268
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/23/2020
ms.locfileid: "87072814"
---
# <a name="understand-azure-policys-guest-configuration"></a>Opis konfiguracji gościa usługi Azure Policy

Azure Policy może przeprowadzać inspekcję ustawień wewnątrz komputera, zarówno w przypadku maszyn działających na platformie Azure, jak i na [urządzeniach połączonych](../../../azure-arc/servers/overview.md)z usługą Arc.
Taka weryfikacja jest wykonywana przez klienta i rozszerzenie konfiguracji gościa. Rozszerzenie to, obsługiwane za pośrednictwem klienta, umożliwia weryfikację następujących ustawień:

- Konfiguracja systemu operacyjnego
- Konfiguracja lub obecność aplikacji
- Ustawienia środowiska

W tej chwili większość Azure Policy zasad konfiguracji gościa tylko ustawienia inspekcji na tym komputerze.
Nie dotyczą one konfiguracji. Wyjątek jest jedną z wbudowanych zasad, [do których odwołuje się poniżej](#applying-configurations-using-guest-configuration).

## <a name="enable-guest-configuration"></a>Włącz konfigurację gościa

Aby przeprowadzić inspekcję stanu maszyn w środowisku, w tym maszyn na platformie Azure i w połączonych maszynach, zapoznaj się z poniższymi szczegółami.

## <a name="resource-provider"></a>Dostawca zasobów

Aby można było korzystać z konfiguracji gościa, należy zarejestrować dostawcę zasobów. Dostawca zasobów jest automatycznie rejestrowany w przypadku przypisywania zasad konfiguracji gościa za pomocą portalu. Możesz zarejestrować się ręcznie za pomocą [portalu](../../../azure-resource-manager/management/resource-providers-and-types.md#azure-portal), [Azure PowerShell](../../../azure-resource-manager/management/resource-providers-and-types.md#azure-powershell)lub [interfejsu wiersza polecenia platformy Azure](../../../azure-resource-manager/management/resource-providers-and-types.md#azure-cli).

## <a name="deploy-requirements-for-azure-virtual-machines"></a>Wdróż wymagania dotyczące usługi Azure Virtual Machines

Aby przeprowadzić inspekcję ustawień wewnątrz maszyny, [rozszerzenie maszyny wirtualnej](../../../virtual-machines/extensions/overview.md) jest włączone, a komputer musi mieć tożsamość zarządzaną przez system. Rozszerzenie pobiera odpowiednie przypisanie zasad i odpowiednią definicję konfiguracji. Tożsamość służy do uwierzytelniania maszyny podczas odczytywania i zapisywania w usłudze konfiguracji gościa. Rozszerzenie nie jest wymagane dla maszyn połączonych z łukiem, ponieważ znajduje się w agencie połączonej maszyny.

> [!IMPORTANT]
> Do inspekcji maszyn wirtualnych platformy Azure wymagane jest rozszerzenie konfiguracji gościa i zarządzana tożsamość. Aby > rozszerzenie konfiguracji gościa jest wymagane do przeprowadzania inspekcji na maszynach wirtualnych platformy Azure. Aby wdrożyć rozszerzenie w odpowiedniej skali, przypisz następujące inicjatywy zasad: > Wdróż rozszerzenie na dużą skalę, przypisz następujące definicje zasad: 
>  - [Wdróż wymagania wstępne, aby włączyć zasady konfiguracji gościa na maszynach wirtualnych](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F12794019-7a00-42cf-95c2-882eed337cc8)

### <a name="limits-set-on-the-extension"></a>Limity ustawione dla rozszerzenia

Aby ograniczyć rozszerzenie do wpływu aplikacji uruchomionych na maszynie, konfiguracja gościa nie może przekroczyć więcej niż 5% procesora CPU. To ograniczenie istnieje dla wbudowanych i niestandardowych definicji. Ta sama wartość dotyczy usługi konfiguracji gościa w agencie połączonej maszyny.

### <a name="validation-tools"></a>Narzędzia sprawdzania poprawności

W ramach maszyny klient konfiguracji gościa używa lokalnych narzędzi do uruchomienia inspekcji.

W poniższej tabeli przedstawiono listę narzędzi lokalnych używanych w każdym obsługiwanym systemie operacyjnym. W przypadku zawartości wbudowanej konfiguracja gościa obsługuje automatyczne ładowanie tych narzędzi.

|System operacyjny|Narzędzie walidacji|Uwagi|
|-|-|-|
|Windows|[Konfiguracja żądanego stanu programu PowerShell](/powershell/scripting/dsc/overview/overview) w wersji 2| Załadowany bezpośrednio do folderu, który jest używany przez Azure Policy. Nie powoduje konfliktu z konfiguracją DSC programu Windows PowerShell. Program PowerShell Core nie został dodany do ścieżki systemowej.|
|Linux|[Chef — Specyfikacja](https://www.chef.io/inspec/)| Instaluje Chef IN2.2.61 w domyślnej lokalizacji i dodaje ją do ścieżki systemowej. Należy również zainstalować zależności dla pakietu INSPEC, w tym Ruby i Python. |

### <a name="validation-frequency"></a>Częstotliwość walidacji

Klient konfiguracji gościa sprawdza nową zawartość co 5 minut. Po odebraniu przypisania gościa ustawienia tej konfiguracji są ponownie sprawdzane w przedziale 15-minutowym. Gdy inspekcja zostanie zakończona, wyniki są wysyłane do dostawcy zasobów konfiguracji gościa. Po wystąpieniu [wyzwalacza oceny](../how-to/get-compliance-data.md#evaluation-triggers) zasad stan maszyny jest zapisywana w dostawcy zasobów konfiguracji gościa. Ta aktualizacja powoduje, że Azure Policy ocen Azure Resource Manager właściwości. Ocena Azure Policy na żądanie Pobiera najnowszą wartość z dostawcy zasobów konfiguracji gościa. Jednak nie wyzwala on nowej inspekcji konfiguracji w ramach maszyny.

## <a name="supported-client-types"></a>Obsługiwane typy klientów

Zasady konfiguracji gościa obejmują nowe wersje. Starsze wersje systemów operacyjnych dostępnych w portalu Azure Marketplace są wykluczone, jeśli Agent konfiguracji gościa nie jest zgodny.
W poniższej tabeli przedstawiono listę obsługiwanych systemów operacyjnych w usłudze Azure images:

|Publisher|Nazwa|Wersje|
|-|-|-|
|Canonical|Ubuntu Server|14,04 i nowsze|
|Credativ|Debian|8 i nowsze|
|Microsoft|Windows Server|2012 i nowsze|
|Microsoft|Klient systemu Windows|Windows 10|
|OpenLogic|CentOS|7,3 i nowsze|
|Red Hat|Red Hat Enterprise Linux|7,4 – 7,8, 9,0 i nowsze|
|Szło|SLES|12 SP3 i nowsze|

Niestandardowe obrazy maszyn wirtualnych są obsługiwane przez zasady konfiguracji gościa, o ile są one jednym z systemów operacyjnych w powyższej tabeli.

## <a name="guest-configuration-extension-network-requirements"></a>Wymagania dotyczące sieci rozszerzenia konfiguracji gościa

Aby komunikować się z dostawcą zasobów konfiguracji gościa na platformie Azure, maszyny wymagają dostępu wychodzącego do centrów danych platformy Azure na porcie **443**. Jeśli sieć na platformie Azure nie zezwala na ruch wychodzący, skonfiguruj wyjątki z regułami [sieciowych grup zabezpieczeń](../../../virtual-network/manage-network-security-group.md#create-a-security-rule) . [Tag usługi](../../../virtual-network/service-tags-overview.md) "GuestAndHybridManagement" może służyć do odwoływania się do usługi konfiguracji gościa.

## <a name="managed-identity-requirements"></a>Wymagania dotyczące tożsamości zarządzanej

Zasady z inicjatywy [wdrażające wymagania wstępne dotyczące włączania zasad konfiguracji gościa na maszynach wirtualnych](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F12794019-7a00-42cf-95c2-882eed337cc8) włączają tożsamość zarządzaną przypisaną przez system, jeśli taka nie istnieje. W ramach inicjatywy, która zarządza tworzeniem tożsamości, istnieją dwa definicje zasad. Warunki IF w definicjach zasad zapewniają poprawne zachowanie na podstawie bieżącego stanu zasobu maszyny na platformie Azure.

Jeśli na komputerze nie ma obecnie żadnych tożsamości zarządzanych, obowiązujące zasady będą: [ \[ wersja zapoznawcza \] : Dodawanie tożsamości zarządzanej przypisanej przez system do włączania przypisań konfiguracji gościa na maszynach wirtualnych bez tożsamości](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F3cf2ab00-13f1-4d0c-8971-2ac904541a7e)

Jeśli komputer ma obecnie tożsamość systemową przypisaną przez użytkownika, obowiązujące zasady będą: [ \[ wersja zapoznawcza \] : Dodawanie tożsamości zarządzanej przypisanej przez system do włączenia przypisań konfiguracji gościa na maszynach wirtualnych z tożsamością przypisaną przez użytkownika](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F497dff13-db2a-4c0f-8603-28fa3b331ab6)

## <a name="guest-configuration-definition-requirements"></a>Wymagania definicji konfiguracji gościa

Każde uruchomienie inspekcji według konfiguracji gościa wymaga dwóch definicji zasad, definicji **DeployIfNotExists** i definicji **AuditIfNotExists** . Definicje zasad **DeployIfNotExists** zarządzają zależnościami dla przeprowadzania inspekcji na poszczególnych komputerach.

Definicja zasad **DeployIfNotExists** weryfikuje i koryguje następujące elementy:

- Sprawdź, czy na komputerze została przypisana konfiguracja do obliczenia. Jeśli żadne przypisanie nie jest obecnie dostępne, Pobierz przypisanie i przygotuj maszynę przez:
  - Uwierzytelnianie na maszynie przy użyciu [tożsamości zarządzanej](../../../active-directory/managed-identities-azure-resources/overview.md)
  - Instalowanie najnowszej wersji rozszerzenia **Microsoft. GuestConfiguration**
  - Instalowanie [narzędzi i zależności walidacji](#validation-tools) , w razie konieczności

Jeśli przypisanie **DeployIfNotExists** jest niezgodne, może zostać użyte [zadanie korygowania](../how-to/remediate-resources.md#create-a-remediation-task) .

Gdy przypisanie **DeployIfNotExists** jest zgodne, przypisanie zasad **AuditIfNotExists** określa, czy przypisanie gościa jest zgodne, czy niezgodne. Narzędzie sprawdzania poprawności zapewnia wyniki dla klienta konfiguracji gościa. Klient przekazuje wyniki do rozszerzenia gościa, co sprawia, że są dostępne za pomocą dostawcy zasobów konfiguracji gościa.

Azure Policy używa właściwości **complianceStatus** dostawcy zasobów konfiguracji gościa, aby zgłosić zgodność w węźle **zgodność** . Aby uzyskać więcej informacji, zobacz [pobieranie danych zgodności](../how-to/get-compliance-data.md).

> [!NOTE]
> Zasady **DeployIfNotExists** są wymagane do zwracania wyników przez zasady **AuditIfNotExists** . Bez **DeployIfNotExists**zasady **AuditIfNotExists** są wyświetlane jako stan zasobów "0 z 0".

Wszystkie wbudowane zasady konfiguracji gościa są zawarte w inicjatywie do grupowania definicji do użycia w przypisaniach. Wbudowana inicjatywa o nazwie _ \[ wersja zapoznawcza \] : Inspekcja zabezpieczeń haseł w systemach Linux i Windows_ zawiera 18 zasad. Istnieje sześć par **DeployIfNotExists** i **AuditIfNotExists** dla systemu Windows i trzech par w systemie Linux. Logika [definicji zasad](definition-structure.md#policy-rule) sprawdza, czy jest oceniany tylko docelowy system operacyjny.

#### <a name="auditing-operating-system-settings-following-industry-baselines"></a>Inspekcja ustawień systemu operacyjnego po liniach bazowych branżowych

Jedna z inicjatyw w Azure Policy umożliwia przeprowadzenie inspekcji ustawień systemu operacyjnego po "linii bazowej". Definicja, _ \[ wersja zapoznawcza \] : Inspekcja maszyn wirtualnych z systemem Windows, które nie są zgodne z ustawieniami linii bazowej zabezpieczeń platformy Azure,_ obejmuje zestaw reguł opartych na zasady grupy Active Directory.

Większość ustawień jest dostępnych jako parametry. Parametry umożliwiają dostosowanie elementów podlegających inspekcji.
Dopasuj zasady do swoich wymagań lub zamapuj zasady na informacje innych firm, takie jak standardy branżowe.

Niektóre parametry obsługują zakres wartości całkowitych. Na przykład ustawienie maksymalnego wieku hasła może prowadzić inspekcję obowiązującego ustawienia zasady grupy. Zakres "1, 70" potwierdzi, że użytkownicy muszą zmieniać hasła co najmniej co 70 dni, ale nie mniej niż jeden dzień.

Jeśli zasady są przypisywane przy użyciu szablonu Azure Resource Manager (szablon ARM), należy użyć pliku parametrów do zarządzania wyjątkami. Zaewidencjonuj pliki do systemu kontroli wersji, takiego jak Git. Komentarze dotyczące zmian plików zawierają informacje o tym, dlaczego przypisanie jest wyjątkiem od oczekiwanej wartości.

#### <a name="applying-configurations-using-guest-configuration"></a>Stosowanie konfiguracji przy użyciu konfiguracji gościa

Najnowsza funkcja Azure Policy konfiguruje ustawienia wewnątrz maszyn. Definicja _konfiguruje strefę czasową na maszynach z systemem Windows_ wprowadza zmiany na komputerze przez skonfigurowanie strefy czasowej.

Podczas przypisywania definicji zaczynających się od _konfiguracji_należy również przypisać _wymagania wstępne wdrażania definicji, aby włączyć zasady konfiguracji gościa na maszynach wirtualnych z systemem Windows_. Możesz połączyć te definicje w ramach inicjatywy, jeśli wybierzesz opcję.

#### <a name="assigning-policies-to-machines-outside-of-azure"></a>Przypisywanie zasad do maszyn poza platformą Azure

Zasady inspekcji dostępne dla konfiguracji gościa obejmują typ zasobu **Microsoft. HybridCompute/Machines** . Wszystkie maszyny dołączone do [usługi Azure ARC dla serwerów](../../../azure-arc/servers/overview.md) , które znajdują się w zakresie przypisania zasad, są automatycznie dołączane.

### <a name="multiple-assignments"></a>Wiele przypisań

Zasady konfiguracji gościa obsługują obecnie tylko jednokrotne przypisanie tego samego przypisania gościa na komputerze, nawet jeśli przypisanie zasad używa różnych parametrów.

## <a name="client-log-files"></a>Pliki dziennika klienta

Rozszerzenie konfiguracji gościa zapisuje pliki dzienników w następujących lokalizacjach:

Systemy`C:\ProgramData\GuestConfig\gc_agent_logs\gc_agent.log`

System`/var/lib/GuestConfig/gc_agent_logs/gc_agent.log`

Gdzie `<version>` odwołuje się do bieżącego numeru wersji.

### <a name="collecting-logs-remotely"></a>Zdalne zbieranie dzienników

Pierwszym krokiem w rozwiązywaniu problemów z konfiguracjami lub modułami konfiguracji gościa jest użycie `Test-GuestConfigurationPackage` polecenia cmdlet zgodnie z instrukcjami [dotyczącymi tworzenia niestandardowych zasad inspekcji konfiguracji Gości dla systemu Windows](../how-to/guest-configuration-create.md#step-by-step-creating-a-custom-guest-configuration-audit-policy-for-windows).
Jeśli to nie powiodło się, zbieranie dzienników klienta może pomóc zdiagnozować problemy.

#### <a name="windows"></a>Windows

Przechwyć informacje z plików dziennika za pomocą [polecenia Uruchom maszynę wirtualną platformy Azure](../../../virtual-machines/windows/run-command.md), który może być przydatny w poniższym przykładzie skryptu programu PowerShell.

```powershell
$linesToIncludeBeforeMatch = 0
$linesToIncludeAfterMatch = 10
$logPath = 'C:\ProgramData\GuestConfig\gc_agent_logs\gc_agent.log'
Select-String -Path $logPath -pattern 'DSCEngine','DSCManagedEngine' -CaseSensitive -Context $linesToIncludeBeforeMatch,$linesToIncludeAfterMatch | Select-Object -Last 10
```

#### <a name="linux"></a>Linux

Przechwyć informacje z plików dziennika za pomocą [polecenia Uruchom maszynę wirtualną platformy Azure](../../../virtual-machines/linux/run-command.md), który może być przydatny w poniższym przykładzie skryptu bash.

```Bash
linesToIncludeBeforeMatch=0
linesToIncludeAfterMatch=10
logPath=/var/lib/GuestConfig/gc_agent_logs/gc_agent.log
egrep -B $linesToIncludeBeforeMatch -A $linesToIncludeAfterMatch 'DSCEngine|DSCManagedEngine' $logPath | tail
```

## <a name="guest-configuration-samples"></a>Przykłady konfiguracji gościa

Wbudowane zasady konfiguracji gościa są dostępne w następujących lokalizacjach:

- [Wbudowane definicje zasad — Konfiguracja gościa](../samples/built-in-policies.md#guest-configuration)
- [Wbudowane inicjatywy — konfiguracja gościa](../samples/built-in-initiatives.md#guest-configuration)
- [Azure Policy przykłady repozytorium GitHub](https://github.com/Azure/azure-policy/tree/master/built-in-policies/policySetDefinitions/Guest%20Configuration)

## <a name="next-steps"></a>Następne kroki

- Dowiedz się, jak wyświetlić szczegóły poszczególnych ustawień z [widoku zgodności konfiguracji gościa](../how-to/determine-non-compliance.md#compliance-details-for-guest-configuration)
- Zapoznaj się z przykładami w [Azure Policy Samples](../samples/index.md).
- Przejrzyj temat [Struktura definicji zasad Azure Policy](definition-structure.md).
- Przejrzyj [wyjaśnienie działania zasad](effects.md).
- Dowiedz się, jak [programowo utworzyć zasady](../how-to/programmatically-create.md).
- Dowiedz się, jak [uzyskać dane zgodności](../how-to/get-compliance-data.md).
- Dowiedz się, jak [skorygować niezgodne zasoby](../how-to/remediate-resources.md).
- Zapoznaj się z informacjami o tym, czym jest Grupa zarządzania, aby [zorganizować swoje zasoby za pomocą grup zarządzania platformy Azure](../../management-groups/overview.md).
