---
title: Obsługiwane regiony połączonego obszaru roboczego usługi Log Analytics
description: W tym artykule opisano obsługiwane mapowania regionów między kontem usługi Automation i obszarem roboczym Log Analytics.
services: automation
ms.service: automation
ms.subservice: process-automation
author: mgoedtel
ms.author: magoedte
ms.date: 06/12/2020
ms.topic: conceptual
manager: carmonm
ms.custom: references_regions
ms.openlocfilehash: 4e5cad25c80661f9e707f545929e6ffcb00a1e42
ms.sourcegitcommit: cee72954f4467096b01ba287d30074751bcb7ff4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2020
ms.locfileid: "87447862"
---
# <a name="supported-regions-for-linked-log-analytics-workspace"></a>Obsługiwane regiony połączonego obszaru roboczego usługi Log Analytics

W Azure Automation można włączyć funkcje Update Management, Change Tracking i spisu oraz Start/Stop VMs during off-hours dla maszyn wirtualnych. Jednak tylko niektóre regiony są obsługiwane na potrzeby łączenia obszaru roboczego Log Analytics i konta usługi Automation w ramach subskrypcji. Mapowania regionów mają zastosowanie tylko do konta usługi Automation i obszaru roboczego Log Analytics. Obszar roboczy Log Analytics i konto usługi Automation muszą znajdować się w tej samej subskrypcji, ale mogą znajdować się w różnych grupach zasobów wdrożonych w tym samym regionie. Aby uzyskać więcej informacji, zobacz [log Analytics obszaru roboczego i konta usługi Automation](../../azure-monitor/insights/solutions.md#log-analytics-workspace-and-automation-account).

## <a name="supported-mappings"></a>Obsługiwane mapowania

W poniższej tabeli przedstawiono obsługiwane mapowania:

|**Log Analytics region obszaru roboczego**|**Region Azure Automation**|
|---|---|
|**USA**||
|Wschodnie<sup>1</sup>|EastUS2|
|WestUS2|WestUS2|
|WestCentralUS|WestCentralUS|
|**Kanada**||
|CanadaCentral|CanadaCentral|
|**Azja i Pacyfik**||
|AustraliaSoutheast|AustraliaSoutheast|
|SoutheastAsia|SoutheastAsia|
|CentralIndia|CentralIndia|
|ChinaEast2<sup>2</sup>|ChinaEast2|
|JapanEast|JapanEast|
|**Europa**||
|UKSouth|UKSouth|
|WestEurope|WestEurope|
|**US Gov**||
|USGovVirginia|USGovVirginia|
|USGovArizona<sup>2</sup>|USGovArizona|

<sup>1</sup> mapowanie Wschodnie dla obszarów roboczych log Analytics na konta usługi Automation nie jest dokładnym mapowaniem między regionami, ale jest poprawnym mapowaniem.

<sup>2</sup> w tym regionie obsługiwana jest tylko Update Management, a inne funkcje, takie jak Change Tracking i spis, nie są dostępne w tej chwili.

## <a name="unlink-a-workspace"></a>Odłączanie obszaru roboczego

Jeśli zdecydujesz, że nie chcesz już integrować konta usługi Automation z obszarem roboczym Log Analytics, możesz odłączyć swoje konto bezpośrednio od Azure Portal. Przed kontynuowaniem należy najpierw [usunąć](move-account.md#remove-features) Update Management, Change Tracking i spis, a Start/Stop VMS during off-hours, jeśli są używane. Jeśli ich nie usuniesz, nie można ukończyć operacji odłączania. 

Po usunięciu funkcji można wykonać poniższe kroki, aby odłączyć konto usługi Automation.

> [!NOTE]
> Niektóre funkcje, w tym wcześniejsze wersje rozwiązania Azure SQL monitoring, mogły utworzyć zasoby automatyzacji, które należy usunąć przed odłączeniem obszaru roboczego.

1. W Azure Portal Otwórz konto usługi Automation. Na stronie konto usługi Automation wybierz opcję **połączony obszar roboczy** w obszarze **powiązane zasoby**.

2. Na stronie Odłącz obszar roboczy wybierz opcję **Odłącz obszar roboczy**. Zostanie wyświetlony monit o zweryfikowanie, czy chcesz kontynuować.

3. Gdy Azure Automation odłączania konta od obszaru roboczego Log Analytics, możesz śledzić postęp w obszarze **powiadomienia** z menu.

4. Jeśli użyto Update Management, opcjonalnie możesz chcieć usunąć następujące elementy, które nie są już potrzebne:

    * Harmonogramy aktualizacji: każdy ma nazwę zgodną z utworzonym wdrożeniem aktualizacji.
    * Grupy hybrydowych procesów roboczych utworzone dla funkcji: każdy ma nazwę podobną do `machine1.contoso.com_9ceb8108-26c9-4051-b6b3-227600d715c8` .

5. Jeśli użyto Start/Stop VMs during off-hours, opcjonalnie można usunąć następujące elementy, które nie są już potrzebne:

    * Uruchamianie i zatrzymywanie harmonogramów elementów Runbook maszyny wirtualnej
    * Uruchamianie i zatrzymywanie elementów Runbook maszyny wirtualnej
    * Zmienne

Alternatywnie możesz odłączyć obszar roboczy od konta usługi Automation w obszarze roboczym.

1. W obszarze roboczym wybierz pozycję **konto usługi Automation** w obszarze **powiązane zasoby**.
2. Na stronie konto usługi Automation wybierz opcję **Odłącz konto**.

## <a name="next-steps"></a>Następne kroki

* Dowiedz się więcej na temat Update Management w [Update Management Omówienie](../update-management/update-mgmt-overview.md).
* Dowiedz się więcej na temat Change Tracking i spisu w temacie [Change Tracking i spisu](../change-tracking.md).
* Dowiedz się więcej na temat Start/Stop VMs during off-hours w [Start/Stop VMS during off-hours Omówienie](../automation-solution-vm-management.md).
