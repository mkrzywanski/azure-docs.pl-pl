---
title: Łączenie maszyn hybrydowych z platformą Azure na dużą skalę
description: W tym artykule dowiesz się, jak połączyć maszyny z platformą Azure przy użyciu usługi Azure ARC dla serwerów (wersja zapoznawcza) przy użyciu nazwy głównej usługi.
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-servers
author: mgoedtel
ms.author: magoedte
ms.date: 07/23/2020
ms.topic: conceptual
ms.openlocfilehash: 7ac04b29853ce0d4f6ac4004bdfad4effd283170
ms.sourcegitcommit: 0e8a4671aa3f5a9a54231fea48bcfb432a1e528c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/24/2020
ms.locfileid: "87132997"
---
# <a name="connect-hybrid-machines-to-azure-at-scale"></a>Łączenie maszyn hybrydowych z platformą Azure na dużą skalę

Usługę Azure Arc można włączyć dla serwerów (wersja zapoznawcza) dla wielu maszyn z systemem Windows lub Linux w środowisku z kilkoma elastycznymi opcjami, w zależności od wymagań. Za pomocą szablonu skryptu udostępniamy, można zautomatyzować każdy krok instalacji, w tym ustanowienie połączenia z usługą Azure Arc. Jednak użytkownik musi interaktywnie wykonać ten skrypt przy użyciu konta, które ma podwyższone uprawnienia na maszynie docelowej i na platformie Azure. Aby połączyć komputery z usługą Azure ARC dla serwerów, można użyć jednostki [usługi](../../active-directory/develop/app-objects-and-service-principals.md) Azure Active Directory, zamiast korzystać z tożsamości uprzywilejowanej, aby [interaktywnie połączyć maszynę](onboard-portal.md). Nazwa główna usługi jest specjalną ograniczoną tożsamością zarządzania, która ma przyznane tylko minimalne uprawnienia wymagane do łączenia maszyn z platformą Azure przy użyciu `azcmagent` polecenia. Jest to bezpieczniejsze niż użycie wyższego konta uprzywilejowanego, takiego jak Administrator dzierżawy, i postępuj zgodnie z najlepszymi rozwiązaniami w zakresie zabezpieczeń kontroli dostępu. Nazwa główna usługi jest używana tylko podczas dołączania, nie jest używana do żadnego innego celu.  

Metody instalacji i konfigurowania agenta połączonego maszyny wymagają, aby stosowana Metoda zautomatyzowana miała uprawnienia administratora na maszynach. W systemie Linux przy użyciu konta głównego i w systemie Windows jako członek lokalnej grupy administratorów.

Przed rozpoczęciem należy zapoznać się z wymaganiami [wstępnymi](agent-overview.md#prerequisites) i upewnić się, że Twoja subskrypcja i zasoby spełniają wymagania.

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

Po zakończeniu tego procesu nastąpi pomyślne Podłączenie maszyn hybrydowych do usługi Azure ARC dla serwerów.

## <a name="create-a-service-principal-for-onboarding-at-scale"></a>Tworzenie jednostki usługi na potrzeby dołączania na dużą skalę

Za pomocą [Azure PowerShell](/powershell/azure/install-az-ps) można utworzyć jednostkę usługi za pomocą polecenia cmdlet [New-AzADServicePrincipal](/powershell/module/Az.Resources/New-AzADServicePrincipal) . Możesz też wykonać kroki opisane w sekcji [Tworzenie nazwy głównej usługi przy użyciu Azure Portal,](../../active-directory/develop/howto-create-service-principal-portal.md) aby wykonać to zadanie.

> [!NOTE]
> Podczas tworzenia nazwy głównej usługi konto musi być właścicielem lub administratorem dostępu użytkownika w subskrypcji, która ma być używana do dołączania. Jeśli nie masz wystarczających uprawnień do tworzenia przypisań ról, można utworzyć nazwę główną usługi, ale nie będzie ona mogła dołączyć maszyn.
>

Aby utworzyć nazwę główną usługi przy użyciu programu PowerShell, wykonaj następujące czynności.

1. Uruchom następujące polecenie. Dane wyjściowe [`New-AzADServicePrincipal`](/powershell/module/az.resources/new-azadserviceprincipal) polecenia cmdlet muszą być przechowywane w zmiennej lub nie będzie można pobrać hasła wymaganego w późniejszym kroku.

    ```azurepowershell-interactive
    $sp = New-AzADServicePrincipal -DisplayName "Arc-for-servers" -Role "Azure Connected Machine Onboarding"
    $sp
    ```

    ```output
    Secret                : System.Security.SecureString
    ServicePrincipalNames : {ad9bcd79-be9c-45ab-abd8-80ca1654a7d1, https://Arc-for-servers}
    ApplicationId         : ad9bcd79-be9c-45ab-abd8-80ca1654a7d1
    ObjectType            : ServicePrincipal
    DisplayName           : Hybrid-RP
    Id                    : 5be92c87-01c4-42f5-bade-c1c10af87758
    Type                  :
    ```

2. Aby pobrać hasło przechowywane w `$sp` zmiennej, uruchom następujące polecenie:

    ```azurepowershell-interactive
    $credential = New-Object pscredential -ArgumentList "temp", $sp.Secret
    $credential.GetNetworkCredential().password
    ```

3. W danych wyjściowych Znajdź wartość hasła w polu **hasło** pola i skopiuj ją. Znajdź również wartość w polu Identyfikator **aplikacji** i skopiuj ją. Zapisz je później w bezpiecznym miejscu. Jeśli zapomnisz lub utracisz hasło nazwy głównej usługi, możesz zresetować ją przy użyciu [`New-AzADSpCredential`](/powershell/module/azurerm.resources/new-azurermadspcredential) polecenia cmdlet.

Wartości z następujących właściwości są używane z parametrami przekazaną do `azcmagent` :

* Wartość właściwości identyfikator **aplikacji** jest używana dla `--service-principal-id` wartości parametru
* Wartość właściwości **Password** służy `--service-principal-secret` do łączenia się z agentem.

> [!NOTE]
> Upewnij się, że używasz **Właściwości głównej nazwy** usługi, a nie właściwości **ID** .
>

Rola **dołączania maszyny połączonej z platformą Azure** zawiera tylko uprawnienia wymagane do dołączenia maszyny. Można przypisać uprawnienia nazwy głównej usługi, aby umożliwić jej zakresowi uwzględnienie grupy zasobów lub subskrypcji. Aby dodać przypisanie roli, zobacz [Dodawanie lub usuwanie przypisań ról przy użyciu funkcji RBAC platformy Azure i Azure Portal](../../role-based-access-control/role-assignments-portal.md) lub [Dodawanie lub usuwanie przypisań ról przy użyciu usług Azure RBAC i interfejsu wiersza polecenia platformy Azure](../../role-based-access-control/role-assignments-cli.md).

## <a name="install-the-agent-and-connect-to-azure"></a>Instalowanie agenta i nawiązywanie połączenia z platformą Azure

Poniższe kroki instalują i konfigurują agenta podłączonego maszyny na maszynach hybrydowych przy użyciu szablonu skryptu, który wykonuje podobne kroki opisane w artykule [łączenie maszyn hybrydowych z platformą Azure z artykułu Azure Portal](onboard-portal.md) . Różnica znajduje się w ostatnim kroku, w którym można nawiązać połączenie z usługą Azure ARC przy użyciu `azcmagent` polecenia przy użyciu nazwy głównej usługi.

Poniżej przedstawiono ustawienia, które należy skonfigurować dla jednostki `azcmagent` usługi.

* `tenant-id`: Unikatowy identyfikator (GUID) reprezentujący dedykowane wystąpienie usługi Azure AD.
* `subscription-id`: Identyfikator subskrypcji (GUID) subskrypcji platformy Azure, w której mają być używane maszyny.
* `resource-group`: Nazwa grupy zasobów, do której mają należeć połączone maszyny.
* `location`: Zobacz [Obsługiwane regiony platformy Azure](overview.md#supported-regions). Ta lokalizacja może być taka sama lub inna, jak lokalizacja grupy zasobów.
* `resource-name`: (*Opcjonalnie*) używany do reprezentacji zasobów platformy Azure na komputerze lokalnym. Jeśli ta wartość nie zostanie określona, zostanie użyta nazwa hosta maszyny.

Więcej informacji na temat `azcmagent` narzędzia wiersza polecenia można uzyskać, przeglądając [odwołanie Azcmagent](./manage-agent.md).

### <a name="windows-installation-script"></a>Skrypt instalacji systemu Windows

Poniżej znajduje się przykładowy skrypt instalacji agenta połączonej maszyny dla systemu Windows, który został zmodyfikowany w celu użycia jednostki usługi do obsługi w pełni zautomatyzowanej, nieinteraktywnej instalacji agenta.

```
 # Download the package
function download() {$ProgressPreference="SilentlyContinue"; Invoke-WebRequest -Uri https://aka.ms/AzureConnectedMachineAgent -OutFile AzureConnectedMachineAgent.msi}
download

# Install the package
msiexec /i AzureConnectedMachineAgent.msi /l*v installationlog.txt /qn | Out-String

# Run connect command
& "$env:ProgramFiles\AzureConnectedMachineAgent\azcmagent.exe" connect `
  --service-principal-id "{serviceprincipalAppID}" `
  --service-principal-secret "{serviceprincipalPassword}" `
  --resource-group "{ResourceGroupName}" `
  --tenant-id "{tenantID}" `
  --location "{resourceLocation}" `
  --subscription-id "{subscriptionID}"
```

>[!NOTE]
>Skrypt obsługuje tylko uruchamianie z 64-bitowej wersji programu Windows PowerShell.
>

### <a name="linux-installation-script"></a>Skrypt instalacji systemu Linux

Poniżej znajduje się przykładowy skrypt instalacji programu Connected Machine Agent dla systemu Linux, który został zmodyfikowany w celu korzystania z jednostki usługi w celu obsługi w pełni zautomatyzowanej, nieinteraktywnej instalacji agenta.

```
# Download the installation package
wget https://aka.ms/azcmagent -O ~/install_linux_azcmagent.sh

# Install the hybrid agent
bash ~/install_linux_azcmagent.sh

# Run connect command
azcmagent connect \
  --service-principal-id "{serviceprincipalAppID}" \
  --service-principal-secret "{serviceprincipalPassword}" \
  --resource-group "{ResourceGroupName}" \
  --tenant-id "{tenantID}" \
  --location "{resourceLocation}" \
  --subscription-id "{subscriptionID}"
```

>[!NOTE]
>Aby uruchamiać **azcmagent**, musisz mieć uprawnienia dostępu *głównego* na maszynach z systemem Linux.

Po zainstalowaniu agenta programu i skonfigurowaniu go w celu nawiązania połączenia z usługą Azure ARC dla serwerów (wersja zapoznawcza) przejdź do Azure Portal, aby sprawdzić, czy serwer został pomyślnie połączony. Wyświetl maszyny w witrynie [Azure Portal](https://aka.ms/hybridmachineportal).

![Pomyślne połączenie z serwerem](./media/onboard-portal/arc-for-servers-successful-onboard.png)

## <a name="next-steps"></a>Następne kroki

- Dowiedz się, jak zarządzać maszyną za pomocą [Azure Policy](../../governance/policy/overview.md), na przykład w [konfiguracji gościa](../../governance/policy/concepts/guest-configuration.md)maszyny wirtualnej, sprawdzając, czy komputer jest raportowany do oczekiwanego log Analytics obszaru roboczego, włącz monitorowanie za pomocą [Azure monitor z maszynami wirtualnymi](../../azure-monitor/insights/vminsights-enable-at-scale-policy.md)i wiele więcej.

- Dowiedz się więcej o [agencie log Analytics](../../azure-monitor/platform/log-analytics-agent.md). Agent Log Analytics dla systemów Windows i Linux jest wymagany, gdy użytkownik chce aktywnie monitorować system operacyjny i obciążenia uruchomione na komputerze, zarządzać nim za pomocą elementów Runbook usługi Automation lub rozwiązań, takich jak Update Management, lub używać innych usług platformy Azure, takich jak [Azure Security Center](../../security-center/security-center-intro.md).
