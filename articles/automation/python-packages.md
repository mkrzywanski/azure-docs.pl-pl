---
title: Zarządzanie pakietami Python 2 w Azure Automation
description: W tym artykule opisano, jak zarządzać pakietami Python 2 w Azure Automation.
services: automation
ms.subservice: process-automation
ms.date: 02/25/2019
ms.topic: conceptual
ms.custom: tracking-python
ms.openlocfilehash: f3ba52c1396928d8c76fb85fda3f29c625e60919
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "84561850"
---
# <a name="manage-python-2-packages-in-azure-automation"></a>Zarządzanie pakietami Python 2 w Azure Automation

Azure Automation umożliwia uruchamianie elementów Runbook języka Python 2 na platformie Azure i w systemie Linux hybrydowych procesów roboczych elementów Runbook. Aby ułatwić uproszczenie elementów Runbook, można użyć pakietów języka Python do zaimportowania potrzebnych modułów. W tym artykule opisano sposób zarządzania pakietami języka Python i używania ich w programie Azure Automation.

## <a name="import-packages"></a>Importowanie pakietów

Na koncie usługi Automation wybierz **pakiety Python 2** w obszarze **udostępnione zasoby**. Kliknij pozycję **+ Dodaj pakiet Python 2**.

![Dodaj pakiet języka Python](media/python-packages/add-python-package.png)

Na stronie Dodawanie pakietu Python 2 Wybierz pakiet lokalny do przekazania. Pakiet może być plikiem **. WHL** lub **. tar. gz** . Po wybraniu pakietu kliknij przycisk **OK** , aby go załadować.

![Dodaj pakiet języka Python](media/python-packages/upload-package.png)

Po zaimportowaniu pakietu jest on wyświetlany na stronie pakiety języka Python 2 na koncie usługi Automation. Jeśli musisz usunąć pakiet, wybierz pakiet, a następnie kliknij przycisk **Usuń**.

![Lista pakietów](media/python-packages/package-list.png)

## <a name="import-packages-with-dependencies"></a>Importuj pakiety z zależnościami

Usługa Azure Automation nie rozpoznaje zależności dla pakietów języka Python podczas procesu importowania. Istnieją dwa sposoby importowania pakietu ze wszystkimi jego zależnościami. Aby zaimportować pakiety do konta usługi Automation, należy użyć tylko jednego z następujących kroków.

### <a name="manually-download"></a>Pobierz ręcznie

Na komputerze z systemem Windows 64-bitowym, na którym zainstalowano środowisko [Python 2.7](https://www.python.org/downloads/release/latest/python2) i [PIP](https://pip.pypa.io/en/stable/) , uruchom następujące polecenie, aby pobrać pakiet i wszystkie jego zależności:

```cmd
C:\Python27\Scripts\pip2.7.exe download -d <output dir> <package name>
```

Po pobraniu pakietów można je zaimportować do konta usługi Automation.

### <a name="runbook"></a>Element Runbook

 Aby uzyskać element Runbook, [zaimportuj pakiety Python 2 z programu PyPi do konta Azure Automation](https://gallery.technet.microsoft.com/scriptcenter/Import-Python-2-packages-57f7d509) z galerii do konta usługi Automation. Upewnij się, że ustawienia uruchomieniowe są ustawione na **platformę Azure** , i uruchom element Runbook za pomocą parametrów. Aby konto usługi Automation działało, element Runbook wymaga konta Uruchom jako. Dla każdego parametru upewnij się, że został on uruchomiony z przełącznikiem, jak pokazano na poniższej liście i obrazie:

* -s\<subscriptionId\>
* -g\<resourceGroup\>
* -a\<automationAccount\>
* -m\<modulePackage\>

![Lista pakietów](media/python-packages/import-python-runbook.png)

Element Runbook umożliwia określenie pakietu do pobrania. Na przykład, użycie `Azure` parametru pobiera wszystkie moduły platformy Azure i wszystkie zależności (około 105).

Po zakończeniu elementu Runbook można sprawdzić **pakiety języka Python 2** w obszarze **udostępnione zasoby** na koncie usługi Automation, aby sprawdzić, czy pakiet został zaimportowany prawidłowo.

## <a name="use-a-package-in-a-runbook"></a>Korzystanie z pakietu w elemencie Runbook

Po zaimportowaniu pakietu można go używać w elemencie Runbook. W poniższym przykładzie zastosowano [pakiet narzędzi Azure Automation](https://github.com/azureautomation/azure_automation_utility). Ten pakiet ułatwia używanie języka Python z Azure Automation. Aby użyć pakietu, postępuj zgodnie z instrukcjami w repozytorium GitHub i Dodaj je do elementu Runbook. Można na przykład użyć, `from azure_automation_utility import get_automation_runas_credential` Aby zaimportować funkcję do pobrania konta Uruchom jako.

```python
import azure.mgmt.resource
import automationassets
from azure_automation_utility import get_automation_runas_credential

# Authenticate to Azure using the Azure Automation RunAs service principal
runas_connection = automationassets.get_automation_connection("AzureRunAsConnection")
azure_credential = get_automation_runas_credential()

# Intialize the resource management client with the RunAs credential and subscription
resource_client = azure.mgmt.resource.ResourceManagementClient(
    azure_credential,
    str(runas_connection["SubscriptionId"]))

# Get list of resource groups and print them out
groups = resource_client.resource_groups.list()
for group in groups:
    print group.name
```

## <a name="develop-and-test-runbooks-offline"></a>Opracowywanie i testowanie elementów Runbook w trybie offline

Aby tworzyć i testować elementy Runbook języka Python 2 w trybie offline, można [Azure Automation użyć modułu emulowane zasoby](https://github.com/azureautomation/python_emulated_assets) w języku Python w witrynie GitHub. Ten moduł umożliwia odwoływanie się do udostępnionych zasobów, takich jak poświadczenia, zmienne, połączenia i certyfikaty.

## <a name="next-steps"></a>Następne kroki

Aby przygotować element Runbook w języku Python, zobacz [Tworzenie elementu Runbook w języku Python](learn/automation-tutorial-runbook-textual-python2.md).
