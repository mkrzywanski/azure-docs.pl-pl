---
title: Włącz przegląd Azure Monitor dla maszyn wirtualnych
description: Dowiedz się, jak wdrażać i konfigurować Azure Monitor dla maszyn wirtualnych. Zapoznaj się z wymaganiami systemowymi.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 07/27/2020
ms.openlocfilehash: 96783955eac6ade90a155236891307720616ed20
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/28/2020
ms.locfileid: "87323948"
---
# <a name="enable-azure-monitor-for-vms-overview"></a>Włącz przegląd Azure Monitor dla maszyn wirtualnych

Ten artykuł zawiera omówienie opcji dostępnych w celu umożliwienia Azure Monitor dla maszyn wirtualnych monitorowania kondycji i wydajności następujących elementów:

- Maszyny wirtualne platformy Azure 
- Zestawy skalowania maszyn wirtualnych platformy Azure
- Hybrydowe maszyny wirtualne połączone z usługą Azure Arc
- Lokalne maszyny wirtualne
- Maszyny wirtualne hostowane w innym środowisku chmury.  

Aby skonfigurować Azure Monitor dla maszyn wirtualnych:

* Włącz pojedynczą maszynę wirtualną platformy Azure, usługę Azure VMSS lub maszynę Azure ARC, wybierając pozycję **szczegółowe** dane bezpośrednio z ich menu w Azure Portal.
* Włącz obsługę wielu maszyn wirtualnych platformy Azure, Azure VMSS lub Azure ARC przy użyciu Azure Policy. Ta metoda zapewnia, że dla istniejących i nowych maszyn wirtualnych i zestawów skalowania wymagane zależności są zainstalowane i poprawnie skonfigurowane. Zgłoszono niezgodne maszyny wirtualne i zestawy skalowania, dzięki czemu można zdecydować, czy włączyć je i skorygować.
* Włącz obsługę wielu maszyn wirtualnych platformy Azure, maszyn wirtualnych usługi Azure ARC, platformy Azure VMSS lub maszyn usługi Azure Arc w ramach określonej subskrypcji lub grupy zasobów przy użyciu programu PowerShell.
* Umożliwia Azure Monitor dla maszyn wirtualnych monitorowania maszyn wirtualnych lub komputerów fizycznych hostowanych w sieci firmowej lub w innym środowisku chmury.

## <a name="prerequisites"></a>Wymagania wstępne

Przed rozpoczęciem upewnij się, że rozumiesz informacje w poniższych sekcjach. 

>[!NOTE]
>Poniższe informacje opisane w tej sekcji dotyczą również [rozwiązania Service map](service-map.md).  

### <a name="log-analytics"></a>Log Analytics

Azure Monitor dla maszyn wirtualnych obsługuje obszar roboczy Log Analytics w następujących regionach:

- Zachodnio-środkowe stany USA
- Zachodnie stany USA
- Zachodnie stany USA 2
- South Central US
- East US
- Wschodnie stany USA 2
- Central US
- Północno-środkowe stany USA
- US Gov AZ
- US Gov VA
- Kanada Środkowa
- Południowe Zjednoczone Królestwo
- Europa Północna
- West Europe
- Azja Wschodnia
- Southeast Asia
- Indie Środkowe
- Japan East
- Australia Wschodnia
- Australia Południowo-Wschodnia

>[!NOTE]
>Maszyny wirtualne platformy Azure można monitorować w dowolnym regionie. Same maszyny wirtualne nie są ograniczone do regionów obsługiwanych przez obszar roboczy Log Analytics.
>

Jeśli nie masz obszaru roboczego Log Analytics, możesz go utworzyć przy użyciu jednego z zasobów:
* [Interfejs wiersza polecenia platformy Azure](../learn/quick-create-workspace-cli.md)
* [Program PowerShell](../platform/powershell-workspace-configuration.md)
* [Witryna Azure Portal](../learn/quick-create-workspace.md)
* [Azure Resource Manager](../platform/template-workspace-configuration.md)

- Maszyna wirtualna platformy Azure
- Zestaw skalowania maszyn wirtualnych platformy Azure
- Hybrydowa maszyna wirtualna połączona z usługą Azure Arc

## <a name="supported-operating-systems"></a>Obsługiwane systemy operacyjne

W poniższej tabeli wymieniono systemy operacyjne Windows i Linux obsługiwane przez Azure Monitor dla maszyn wirtualnych. W dalszej części tej sekcji znajdziesz pełną listę zawierającą szczegóły głównej i pomocniczej wersji systemu operacyjnego Linux oraz obsługiwane wersje jądra.

|Wersja systemu operacyjnego |Wydajność |Maps |
|-----------|------------|-----|
|Windows Server 2019 | X | X |
|System Windows Server 2016 1803 | X | X |
|Windows Server 2016 | X | X |
|Windows Server 2012 z dodatkiem R2 | X | X |
|Windows Server 2012 | X | X |
|Windows Server 2008 z dodatkiem R2 | X | X|
|System Windows 10 1803 | X | X |
|Windows 8.1 | X | X |
|Windows 8 | X | X |
|Windows 7 z dodatkiem SP1 | X | X |
|Red Hat Enterprise Linux (RHEL) 6, 7| X | X| 
|Ubuntu 18,04, 16,04 | X | X |
|CentOS Linux 7, 6 | X | X |
|SUSE Linux Enterprise Server (SLES) 12 | X | X |
|Debian 9,4, 8 | X<sup>1</sup> | |

<sup>1</sup> funkcja wydajności Azure monitor dla maszyn wirtualnych jest dostępna tylko z Azure monitor. Nie jest ona dostępna bezpośrednio z okienka po lewej stronie maszyny wirtualnej platformy Azure.

>[!NOTE]
>W systemie operacyjnym Linux:
> - Obsługiwane są tylko wersje domyślne i wersje SMP jądra systemu Linux.
> - Niestandardowe wersje jądra, takie jak rozszerzenie adresu fizycznego (PAE) i Xen, nie są obsługiwane w przypadku żadnej dystrybucji systemu Linux. Na przykład system z ciągiem wydania *2.6.16.21-0,8-Xen* nie jest obsługiwany.
> - Niestandardowe jądra, w tym ponowne kompilacje standardowych jądra, nie są obsługiwane.
> - CentOSPlus jądro jest obsługiwane.
> - Dla luki w zabezpieczeniach Spectre należy zainstalować jądro systemu Linux. Aby uzyskać więcej informacji, skontaktuj się z dostawcą dystrybucji systemu Linux.

#### <a name="red-hat-linux-7"></a>Red Hat Linux 7

| Wersja systemu operacyjnego | Wersja jądra |
|:--|:--|
| 7,6 | 3.10.0-957 |
| 7,5 | 3.10.0-862 |
| 7.4 | 3.10.0-693 |

#### <a name="red-hat-linux-6"></a>Red Hat Linux 6

| Wersja systemu operacyjnego | Wersja jądra |
|:--|:--|
| 6,10 | 2.6.32 — 754 |
| 6,9 | 2.6.32-696 |

#### <a name="centosplus"></a>CentOSPlus

| Wersja systemu operacyjnego | Wersja jądra |
|:--|:--|
| 6,10 | 2.6.32-754.3.5<br>2.6.32-696.30.1 |
| 6,9 | 2.6.32-696.30.1<br>2.6.32-696.18.7 |

#### <a name="ubuntu-server"></a>Ubuntu Server

| Wersja systemu operacyjnego | Wersja jądra |
|:--|:--|
| 18,04 | 5.3.0-1020<br>5,0 (obejmuje jądro dostosowane do platformy Azure)<br>4,18* <br> 4,15* |
| 16.04.3 | 4,15. * |
| 16,04 | 4,13.\*<br>4,11.\*<br>4,10.\*<br>4,8.\*<br>4,4.\* |

#### <a name="suse-linux-12-enterprise-server"></a>SUSE Linux 12 Enterprise Server

| Wersja systemu operacyjnego | Wersja jądra |
|:--|:--|
|12 SP4 | 4,12. * (obejmuje jądro dostosowane do platformy Azure) |
|12 SP3 | 4,4. * |
|12 Z DODATKIEM SP2 | 4,4. * |

#### <a name="debian"></a>Debian 

| Wersja systemu operacyjnego | Wersja jądra |
|:--|:--|
| 9 | 4,9 | 

## <a name="supported-azure-arc-machines"></a>Obsługiwane maszyny usługi Azure Arc
Azure Monitor dla maszyn wirtualnych jest dostępna dla serwerów z obsługą usługi Azure Arc w regionach, w których jest dostępna usługa rozszerzenia Arc. Musi być uruchomiona wersja 0,9 lub nowsza agenta Arc.

| Połączone źródło | Obsługiwane | Opis |
|:--|:--|:--|
| Agenci dla systemu Windows | Tak | Wraz z [agentem log Analytics dla systemu Windows](../platform/log-analytics-agent.md)agenci systemu Windows potrzebują agenta zależności. Aby uzyskać więcej informacji, zobacz [obsługiwane systemy operacyjne](#supported-operating-systems). |
| Agenci dla systemu Linux | Tak | Wraz z [agentem log Analytics dla systemu Linux](../platform/log-analytics-agent.md)agenci systemu Linux potrzebują agenta zależności. Aby uzyskać więcej informacji, zobacz [obsługiwane systemy operacyjne](#supported-operating-systems). |
| Grupa zarządzania programu System Center Operations Manager | Nie | |

## <a name="agents"></a>Agenci
Azure Monitor dla maszyn wirtualnych wymaga zainstalowania dwóch agentów na każdej maszynie wirtualnej lub w zestawie skalowania maszyn wirtualnych do monitorowania. Zainstalowanie tych agentów i połączenie ich z obszarem roboczym jest jedynym wymaganiem do dołączenia zasobu.

- [Agent log Analytics](../platform/log-analytics-agent.md). Zbiera dane zdarzeń i wydajności z maszyny wirtualnej lub zestawu skalowania maszyn wirtualnych i dostarcza go do obszaru roboczego Log Analytics. Metody wdrażania dla agenta Log Analytics w zasobach platformy Azure używają rozszerzenia maszyny wirtualnej dla [systemów Windows](../../virtual-machines/extensions/oms-windows.md) i [Linux](../../virtual-machines/extensions/oms-linux.md).
- Agent zależności. Zbiera odnalezione dane dotyczące procesów uruchomionych na maszynie wirtualnej i zewnętrznych zależności procesów, które są używane przez [funkcję map w Azure monitor dla maszyn wirtualnych](vminsights-maps.md). Agent zależności korzysta z agenta Log Analytics, aby dostarczyć dane do Azure Monitor. Metody wdrażania dla agenta zależności w zasobach platformy Azure używają rozszerzenia maszyny wirtualnej dla [systemów Windows](../../virtual-machines/extensions/agent-dependency-windows.md) i [Linux](../../virtual-machines/extensions/agent-dependency-linux.md).

> [!NOTE]
> Agent Log Analytics jest tym samym agentem, który jest używany przez System Center Operations Manager. Azure Monitor dla maszyn wirtualnych może monitorować agentów, które są również monitorowane przez Operations Manager, jeśli są połączone bezpośrednio, a na nich jest instalowany Agent zależności. Nie można monitorować agentów podłączonych do Azure Monitor za pomocą [połączenia z grupą zarządzania](../tform/../platform/om-agents.md) przez Azure monitor dla maszyn wirtualnych.

Poniżej przedstawiono wiele metod wdrażania tych agentów. 

| Metoda | Opis |
|:---|:---|
| [Witryna Azure Portal](vminsights-enable-single-vm.md) | Zainstaluj obu agentów na jednej maszynie wirtualnej, w zestawie skalowania maszyn wirtualnych lub hybrydowych maszyn wirtualnych połączonych z usługą Azure Arc. |
| [Szablony usługi Resource Manager](vminsights-enable-powershell.md) | Zainstaluj obu agentów przy użyciu dowolnej z obsługiwanych metod do wdrożenia szablonu Menedżer zasobów, w tym interfejsu wiersza polecenia i programu PowerShell. |
| [Azure Policy](vminsights-enable-at-scale-policy.md) | Przypisz inicjatywę Azure Policy, aby automatycznie zainstalować agentów podczas tworzenia maszyny wirtualnej lub zestawu skalowania maszyn wirtualnych. |
| [Instalacja ręczna](vminsights-enable-hybrid-cloud.md) | Zainstaluj agentów w systemie operacyjnym gościa na komputerach hostowanych poza platformą Azure, w tym w centrum danych lub w innych środowiskach w chmurze. |




## <a name="management-packs"></a>Pakiety administracyjne
W przypadku skonfigurowania Log Analytics obszaru roboczego dla Azure Monitor dla maszyn wirtualnych dwa pakiety administracyjne są przekazywane do wszystkich komputerów z systemem Windows połączonych z tym obszarem roboczym. Pakiety administracyjne mają nazwę *Microsoft. IntelligencePacks. ApplicationDependencyMonitor* i *Microsoft. IntelligencePacks. VMInsights* i są zapisywane w *%ProgramFiles%\Microsoft monitoring Agent\Agent\Health Service State\Management Pack \* . 

Źródło danych używane przez pakiet administracyjny *ApplicationDependencyMonitor* to **% Program Files%\Microsoft monitoring Agent\Agent\Health Service State\Resources \<AutoGeneratedID>\Microsoft.EnterpriseManagement.Advisor.ApplicationDependencyMonitorDataSource.dll*. Źródło danych używane przez pakiet administracyjny *VMInsights* to *% Program Files%\Microsoft monitorowania Agent\Agent\Health Service State\Resources \<AutoGeneratedID> \ Microsoft.VirtualMachineMonitoringModule.dll*.

## <a name="diagnostic-and-usage-data"></a>Dane diagnostyczne i użycia

Firma Microsoft automatycznie zbiera dane dotyczące użycia i wydajności za pomocą usługi Azure Monitor. Firma Microsoft używa tych danych w celu poprawienia jakości, bezpieczeństwa i integralności usługi. 

Aby zapewnić dokładne i wydajne możliwości rozwiązywania problemów, funkcja map zawiera dane dotyczące konfiguracji oprogramowania. Dane zawierają informacje takie jak system operacyjny, wersja, adres IP, nazwa DNS i nazwa stacji roboczej. Firma Microsoft nie zbiera nazw, adresów ani innych informacji kontaktowych.

Aby uzyskać więcej informacji na temat zbierania i używania danych, zobacz [zasady zachowania poufności informacji w witrynie Microsoft Online Services](https://go.microsoft.com/fwlink/?LinkId=512132).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się, jak korzystać z funkcji monitorowania wydajności, zobacz [wyświetlanie Azure monitor dla maszyn wirtualnych Performance](vminsights-performance.md). Aby wyświetlić odnalezione zależności aplikacji, zobacz [view Azure monitor dla maszyn wirtualnych map](vminsights-maps.md).

