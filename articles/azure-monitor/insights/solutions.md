---
title: Monitorowanie rozwiązań w Azure Monitor | Microsoft Docs
description: Monitorowanie rozwiązań w Azure Monitor to zbiór reguł logiki, wizualizacji i pozyskiwania danych, które zapewniają metryki przestawiane wokół określonego obszaru problemu.  Ten artykuł zawiera informacje na temat instalowania i używania rozwiązań monitorowania.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 05/12/2020
ms.openlocfilehash: a2ece33974e5bb4d8cb6257d3ca68a258f9b8c81
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/28/2020
ms.locfileid: "87326005"
---
# <a name="monitoring-solutions-in-azure-monitor"></a>Monitorowanie rozwiązań w Azure Monitor

Rozwiązania do monitorowania wykorzystują usługi platformy Azure w celu zapewnienia dodatkowej analizy operacji określonej aplikacji lub usługi. Ten artykuł zawiera krótkie omówienie rozwiązań monitorowania na platformie Azure i szczegółowe informacje dotyczące korzystania z nich i instalowania ich. Możesz dodać rozwiązania do monitorowania do Azure Monitor dla wszystkich używanych aplikacji i usług. Są one zazwyczaj dostępne bezpłatnie, ale zbierają dane, które mogą wywołać opłaty za użycie.

## <a name="use-monitoring-solutions"></a>Używanie rozwiązań do monitorowania

Otwórz stronę **przeglądu** w Azure monitor, aby wyświetlić kafelek dla każdego rozwiązania zainstalowanego w obszarze roboczym.

1. Przejdź do witryny [Azure Portal](https://ms.portal.azure.com). Wyszukaj i wybierz pozycję **monitor**.
1. W menu **Insights (szczegółowe** dane) wybierz pozycję **więcej**.
1. Użyj pól rozwijanych w górnej części ekranu, aby zmienić obszar roboczy lub zakres czasu używany dla kafelków.
1. Kliknij kafelek rozwiązania, aby otworzyć jego widok, który zawiera bardziej szczegółową analizę zebranych danych.

![Omówienie](media/solutions/overview.png)

Rozwiązania do monitorowania mogą zawierać wiele typów zasobów platformy Azure i można wyświetlić wszystkie zasoby dołączone do rozwiązania, tak jak każdy inny zasób. Na przykład wszystkie zapytania dzienników zawarte w rozwiązaniu są wymienione w obszarze **zapytania dotyczące rozwiązań** w [Eksploratorze zapytań](../log-query/get-started-portal.md#load-queries) , których można używać podczas przeprowadzania analizy ad hoc przy użyciu [zapytań dzienników](../log-query/log-query-overview.md).

## <a name="list-installed-monitoring-solutions"></a>Wyświetlanie listy zainstalowanych rozwiązań monitorowania

### <a name="portal"></a>[Portal](#tab/portal)

Aby wyświetlić listę rozwiązań monitorowania zainstalowanych w ramach subskrypcji, należy wykonać poniższą procedurę.

1. Przejdź do witryny [Azure Portal](https://ms.portal.azure.com). Wyszukaj i wybierz **rozwiązania**.
1. Zostaną wyświetlone rozwiązania zainstalowane we wszystkich Twoich obszarach roboczych. Po nazwie rozwiązania następuje nazwa obszaru roboczego, w którym jest zainstalowana.
1. Użyj pól listy rozwijanej w górnej części ekranu, aby filtrować według subskrypcji lub grupy zasobów.

![Wyświetl listę wszystkich rozwiązań](media/solutions/list-solutions-all.png)

Kliknij nazwę rozwiązania, aby otworzyć jego stronę podsumowania. Na tej stronie są wyświetlane wszystkie widoki zawarte w rozwiązaniu i dostępne są różne opcje dotyczące samego rozwiązania oraz jego obszaru roboczego. Zapoznaj się ze stroną podsumowania rozwiązania, korzystając z jednej z powyższych procedur, aby wyświetlić listę rozwiązań, a następnie kliknij nazwę rozwiązania.

![Właściwości rozwiązania](media/solutions/solution-properties.png)

### <a name="azure-cli"></a>[Interfejs wiersza polecenia platformy Azure](#tab/azure-cli)

Użyj polecenia [AZ Monitor Log-Analytics Solution list](/cli/azure/ext/log-analytics-solution/monitor/log-analytics/solution#ext-log-analytics-solution-az-monitor-log-analytics-solution-list) , aby wyświetlić listę rozwiązań monitorowania zainstalowanych w ramach subskrypcji.   Przed uruchomieniem `list` polecenia postępuj zgodnie z warunkami wstępnymi znalezionymi w temacie [Instalowanie rozwiązania monitorowania](#install-a-monitoring-solution).

```azurecli
# List all log-analytics solutions in the current subscription.
az monitor log-analytics solution list

# List all log-analytics solutions for a specific subscription
az monitor log-analytics solution list --subscription MySubscription

# List all log-analytics solutions in a resource group
az monitor log-analytics solution list --resource-group MyResourceGroup
```

* * *

## <a name="install-a-monitoring-solution"></a>Zainstaluj rozwiązanie do monitorowania

### <a name="portal"></a>[Portal](#tab/portal)

Rozwiązania do monitorowania od firmy Microsoft i partnerów są dostępne w [portalu Azure Marketplace](https://azuremarketplace.microsoft.com). Dostępne rozwiązania można wyszukiwać i instalować przy użyciu poniższej procedury. W przypadku instalowania rozwiązania należy wybrać [obszar roboczy log Analytics](../platform/manage-access.md) , w którym zostanie zainstalowane rozwiązanie oraz gdzie będą zbierane dane.

1. Z [listy rozwiązań dla subskrypcji](#list-installed-monitoring-solutions)kliknij pozycję **Dodaj**.
1. Przeglądaj lub Wyszukaj rozwiązanie. Możesz również przeglądać rozwiązania z [tego linku wyszukiwania](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/management-tools?page=1&subcategories=management-solutions).
1. Znajdź odpowiednie rozwiązanie do monitorowania i przeczytaj jego opis.
1. Kliknij przycisk **Utwórz** , aby rozpocząć proces instalacji.
1. Po rozpoczęciu procesu instalacji zostanie wyświetlony monit o określenie obszaru roboczego Log Analytics i udostępnienie wymaganej konfiguracji rozwiązania.

![Instalowanie rozwiązania](media/solutions/install-solution.png)

### <a name="install-a-solution-from-the-community"></a>Instalowanie rozwiązania ze społeczności

Członkowie społeczności mogą przesyłać rozwiązania do zarządzania przy użyciu szablonów szybkiego startu platformy Azure. Te rozwiązania można zainstalować bezpośrednio lub pobrać szablony do późniejszej instalacji.

1. Postępuj zgodnie z procesem opisanym w [log Analytics obszarze roboczym i koncie usługi Automation](#log-analytics-workspace-and-automation-account) , aby połączyć obszar roboczy i konto.
2. Przejdź do [szablonów szybkiego startu platformy Azure](https://azure.microsoft.com/documentation/templates/).
3. Wyszukaj interesujące Cię rozwiązanie.
4. Wybierz rozwiązanie z wyników, aby wyświetlić jego szczegóły.
5. Kliknij przycisk **Wdróż na platformie Azure** .
6. Zostanie wyświetlony monit o podanie informacji, takich jak grupa zasobów i lokalizacja, oprócz wartości parametrów w rozwiązaniu.
7. Kliknij przycisk **Kup** , aby zainstalować rozwiązanie.

### <a name="azure-cli"></a>[Interfejs wiersza polecenia platformy Azure](#tab/azure-cli)

### <a name="prepare-your-environment"></a>Przygotowanie środowiska

1. Instalowanie interfejsu wiersza polecenia platformy Azure

   Musisz [zainstalować interfejs wiersza polecenia platformy Azure](/cli/azure/install-azure-cli) przed uruchomieniem poleceń referencyjnych interfejsu CLI.  Jeśli wolisz, możesz również użyć Azure Cloud Shell, aby wykonać kroki opisane w tym artykule.  Azure Cloud Shell to interaktywne środowisko powłoki, które jest używane w przeglądarce.  Rozpocznij Cloud Shell przy użyciu jednej z następujących metod:

   - Otwórz Cloud Shell, przechodząc do[https://shell.azure.com](https://shell.azure.com)

   - Wybierz przycisk **Cloud Shell** na pasku menu w prawym górnym rogu [Azure Portal](https://portal.azure.com)

1. Zaloguj się.

   Jeśli używasz lokalnej instalacji interfejsu wiersza polecenia, zaloguj się za pomocą polecenia [AZ login](/cli/azure/reference-index#az-login) .  Postępuj zgodnie z instrukcjami wyświetlanymi w terminalu, aby ukończyć proces uwierzytelniania.

    ```azurecli
    az login
    ```

1. Zainstaluj `log-analytics` rozszerzenie

   `log-analytics`Polecenie to eksperymentalne rozszerzenie interfejsu wiersza polecenia platformy Azure. Dowiedz się więcej na temat odwołań do rozszerzeń w [rozszerzeniu use przy użyciu interfejsu wiersza polecenia platformy Azure](/cli/azure/azure-cli-extensions-overview?).

   ```azurecli
   az extension add --name log-analytics
   ```

   Oczekiwane jest następujące ostrzeżenie.

   ```output
   The installed extension `log-analytics` is experimental and not covered by customer support.  Please use with discretion.
   ```

### <a name="install-a-solution-with-the-azure-cli"></a>Instalowanie rozwiązania przy użyciu interfejsu wiersza polecenia platformy Azure

W przypadku instalowania rozwiązania należy wybrać [obszar roboczy log Analytics](../platform/manage-access.md) , w którym zostanie zainstalowane rozwiązanie oraz gdzie będą zbierane dane.  Za pomocą interfejsu wiersza polecenia platformy Azure można zarządzać obszarami roboczymi za pomocą poleceń [AZ Monitor Log-Analytics obszaru roboczego](/cli/azure/monitor/log-analytics/workspace) .  Postępuj zgodnie z procesem opisanym w [log Analytics obszarze roboczym i koncie usługi Automation](#log-analytics-workspace-and-automation-account) , aby połączyć obszar roboczy i konto.

Użyj narzędzia [AZ Monitor Log-Analytics rozwiązanie Create](/cli/azure/ext/log-analytics-solution/monitor/log-analytics/solution) , aby zainstalować rozwiązanie do monitorowania.  Parametry w nawiasach kwadratowych są opcjonalne.

```azurecli
az monitor log-analytics solution create --name
                                         --plan-product
                                         --plan-publisher
                                         --resource-group
                                         --workspace
                                         [--no-wait]
                                         [--tags]
```

Oto przykładowy kod, który tworzy rozwiązanie log Analytics dla produktu plan OMSGallery/Containers.

```azurecli
az monitor log-analytics solution create --resource-group MyResourceGroup \
                                         --name Containers({SolutionName}) \
                                         --tags key=value \
                                         --plan-publisher Microsoft  \
                                         --plan-product "OMSGallery/Containers" \
                                         --workspace "/subscriptions/{SubID}/resourceGroups/{ResourceGroup}/providers/ \
                                           Microsoft.OperationalInsights/workspaces/{WorkspaceName}"
```

* * *

## <a name="log-analytics-workspace-and-automation-account"></a>Log Analytics obszaru roboczego i konta usługi Automation

Wszystkie rozwiązania do monitorowania wymagają [log Analytics obszar roboczy](../platform/manage-access.md) do przechowywania danych zbieranych przez rozwiązanie oraz do hostowania przeszukiwanych i widoków dzienników. Niektóre rozwiązania wymagają również, aby [konto usługi Automation](../../automation/automation-security-overview.md) zawierało elementy Runbook i powiązane zasoby. Obszar roboczy i konto muszą spełniać następujące wymagania.

* Każda instalacja rozwiązania może korzystać tylko z jednego obszaru roboczego Log Analytics i jednego konta usługi Automation. Rozwiązanie można zainstalować oddzielnie w wielu obszarach roboczych.
* Jeśli rozwiązanie wymaga konta usługi Automation, obszar roboczy Log Analytics i konto usługi Automation muszą być połączone ze sobą. Obszar roboczy Log Analytics może być połączony tylko z jednym kontem usługi Automation, a konto usługi Automation może być połączone tylko z jednym obszarem roboczym Log Analytics.
* Aby można było połączyć Log Analytics obszar roboczy i konto usługi Automation muszą znajdować się w tej samej subskrypcji, ale mogą znajdować się w różnych grupach zasobów wdrożonych w tym samym regionie. Wyjątkiem jest obszar roboczy w regionie Wschodnie stany USA i konto usługi Automation w regionach Wschodnie stany USA 2.

Po zainstalowaniu rozwiązania w portalu Azure Marketplace zostanie wyświetlony monit dotyczący obszaru roboczego i konta usługi Automation. Łącze między nimi jest tworzone, jeśli nie są jeszcze połączone.

### <a name="verify-the-link-between-a-log-analytics-workspace-and-automation-account"></a>Weryfikowanie połączenia między obszarem roboczym Log Analytics i kontem usługi Automation

Aby sprawdzić połączenie między obszarem roboczym Log Analytics i kontem usługi Automation, należy wykonać poniższą procedurę.

1. Wybierz konto usługi Automation w Azure Portal.
1. Przewiń do sekcji **powiązane zasoby** menu i wybierz pozycję **połączony obszar roboczy**.
1. Jeśli **obszar roboczy** jest połączony z kontem usługi Automation, na tej stronie znajduje się obszar roboczy, z którym jest on połączony. Jeśli wybierzesz nazwę obszaru roboczego, nastąpi przekierowanie do strony przegląd dla tego obszaru roboczego.

## <a name="remove-a-monitoring-solution"></a>Usuń rozwiązanie do monitorowania

### <a name="portal"></a>[Portal](#tab/portal)

Aby usunąć zainstalowane rozwiązanie przy użyciu portalu, Znajdź je na [liście zainstalowanych rozwiązań](#list-installed-monitoring-solutions). Kliknij nazwę rozwiązania, aby otworzyć jego stronę podsumowania, a następnie kliknij przycisk **Usuń**.

### <a name="azure-cli"></a>[Interfejs wiersza polecenia platformy Azure](#tab/azure-cli)

Aby usunąć zainstalowane rozwiązanie przy użyciu interfejsu wiersza polecenia platformy Azure, użyj polecenia [AZ Monitor Log-Analytics rozwiązanie Delete](/cli/azure/ext/log-analytics-solution/monitor/log-analytics/solution#ext-log-analytics-solution-az-monitor-log-analytics-solution-delete) .

```azurecli
az monitor log-analytics solution delete --name
                                         --resource-group
                                         [--no-wait]
                                         [--yes]
```

* * *

## <a name="next-steps"></a>Następne kroki

* Pobierz [listę rozwiązań monitorowania od firmy Microsoft](/azure/azure-monitor/monitor-reference).
* Dowiedz się, jak [tworzyć zapytania](../log-query/log-query-overview.md) do analizowania danych zbieranych przez rozwiązania monitorujące.
* Zapoznaj się z [poleceniami dla Azure monitor](/cli/azure/azure-cli-reference-for-monitor).

