---
title: Uruchamianie, monitorowanie i anulowanie przebiegów szkoleniowych w języku Python
titleSuffix: Azure Machine Learning
description: Dowiedz się, jak uruchamiać, ustawiać stan, oznaczać i organizować eksperymenty uczenia maszynowego.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.author: roastala
author: rastala
manager: cgronlun
ms.reviewer: nibaccam
ms.date: 01/09/2020
ms.topic: conceptual
ms.custom: how-to, tracking-python
ms.openlocfilehash: 38e7ecf371beb003137b4ae1b2315046a474d090
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/28/2020
ms.locfileid: "87319732"
---
# <a name="start-monitor-and-cancel-training-runs-in-python"></a>Uruchamianie, monitorowanie i anulowanie przebiegów szkoleniowych w języku Python
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

[Zestaw Azure Machine Learning SDK dla języka Python](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py), [Machine Learning interfejsu wiersza polecenia](reference-azure-machine-learning-cli.md)i [Azure Machine Learning Studio](https://ml.azure.com) udostępnia różne metody monitorowania, organizowania i zarządzania przebiegami w celu uczenia i eksperymentowania.

W tym artykule przedstawiono przykłady następujących zadań:

* Monitoruj wydajność uruchamiania.
* Anulowanie lub niepowodzenie uruchomienia.
* Utwórz uruchomienia podrzędne.
* Tagi i Znajdź uruchomienia.

## <a name="prerequisites"></a>Wymagania wstępne

Potrzebne będą następujące elementy:

* Subskrypcja platformy Azure. Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz bezpłatne konto. Wypróbuj [bezpłatną lub płatną wersję Azure Machine Learning](https://aka.ms/AMLFree) dzisiaj.

* [Obszar roboczy Azure Machine Learning](how-to-manage-workspace.md).

* Zestaw Azure Machine Learning SDK dla języka Python (wersja 1.0.21 lub nowsza). Aby zainstalować lub zaktualizować najnowszą wersję zestawu SDK, zobacz [Instalowanie lub aktualizowanie zestawu SDK](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py).

    Aby sprawdzić wersję zestawu SDK Azure Machine Learning, użyj następującego kodu:

    ```python
    print(azureml.core.VERSION)
    ```

* [Interfejs wiersza polecenia platformy Azure](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) i [rozszerzenie interfejsu wiersza polecenia dla Azure Machine Learning](reference-azure-machine-learning-cli.md).

## <a name="start-a-run-and-its-logging-process"></a>Rozpocznij przebieg i proces rejestrowania

### <a name="using-the-sdk"></a>Używanie zestawu SDK

Skonfiguruj eksperyment przez zaimportowanie klas [obszaru roboczego](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py), [eksperymentu](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment.experiment?view=azure-ml-py), [uruchamiania](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py)i [ScriptRunConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.scriptrunconfig?view=azure-ml-py) z pakietu [Azure. Core](https://docs.microsoft.com/python/api/azureml-core/azureml.core?view=azure-ml-py) .

```python
import azureml.core
from azureml.core import Workspace, Experiment, Run
from azureml.core import ScriptRunConfig

ws = Workspace.from_config()
exp = Experiment(workspace=ws, name="explore-runs")
```

Rozpocznij przebieg i proces rejestrowania przy użyciu [`start_logging()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment(class)?view=azure-ml-py#start-logging--args----kwargs-) metody.

```python
notebook_run = exp.start_logging()
notebook_run.log(name="message", value="Hello from run!")
```

### <a name="using-the-cli"></a>Korzystanie z interfejsu wiersza polecenia

Aby rozpocząć wykonywanie eksperymentu, wykonaj następujące czynności:

1. Z poziomu powłoki lub wiersza polecenia Użyj interfejsu wiersza poleceń platformy Azure do uwierzytelniania w ramach subskrypcji platformy Azure:

    ```azurecli-interactive
    az login
    ```
    
    [!INCLUDE [select-subscription](../../includes/machine-learning-cli-subscription.md)] 

1. Dołącz konfigurację obszaru roboczego do folderu, który zawiera skrypt szkoleniowy. Zamień `myworkspace` na obszar roboczy Azure Machine Learning. Zamień `myresourcegroup` na grupę zasobów platformy Azure, która zawiera obszar roboczy:

    ```azurecli-interactive
    az ml folder attach -w myworkspace -g myresourcegroup
    ```

    To polecenie tworzy `.azureml` podkatalog zawierający przykładowe pliki środowiska runconfig i Conda. Zawiera również `config.json` plik, który jest używany do komunikowania się z obszarem roboczym Azure Machine Learning.

    Aby uzyskać więcej informacji, zobacz [AZ ml folder Attach](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/folder?view=azure-cli-latest#ext-azure-cli-ml-az-ml-folder-attach).

2. Aby uruchomić przebieg, użyj następującego polecenia. Korzystając z tego polecenia, należy określić nazwę pliku runconfig (tekst przed \* . runconfig, Jeśli przeglądasz system plików) z parametrem-c.

    ```azurecli-interactive
    az ml run submit-script -c sklearn -e testexperiment train.py
    ```

    > [!TIP]
    > `az ml folder attach`Polecenie utworzyło `.azureml` podkatalog, który zawiera dwa przykładowe pliki runconfig.
    >
    > Jeśli masz skrypt języka Python, który programowo tworzy obiekt konfiguracji uruchomieniowej, możesz użyć [runconfig. Save ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfiguration?view=azure-ml-py#save-path-none--name-none--separate-environment-yaml-false-) , aby zapisać go jako plik runconfig.
    >
    > Więcej przykładowych plików runconfig można znaleźć w temacie [https://github.com/MicrosoftDocs/pipelines-azureml/](https://github.com/MicrosoftDocs/pipelines-azureml/) .

    Aby uzyskać więcej informacji, zobacz [AZ ml Run Submit-Script](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/run?view=azure-cli-latest#ext-azure-cli-ml-az-ml-run-submit-script).

### <a name="using-azure-machine-learning-studio"></a>Korzystanie z programu Azure Machine Learning Studio

Aby rozpocząć przesyłanie potoku w projektancie (wersja zapoznawcza), wykonaj następujące czynności:

1. Ustaw domyślny element docelowy obliczeń dla potoku.

1. Wybierz pozycję **Uruchom** w górnej części kanwy potoku.

1. Wybierz eksperyment, aby zgrupować uruchomienia potoku.

## <a name="monitor-the-status-of-a-run"></a>Monitorowanie stanu przebiegu

### <a name="using-the-sdk"></a>Używanie zestawu SDK

Pobierz stan uruchomienia za pomocą [`get_status()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#get-status--) metody.

```python
print(notebook_run.get_status())
```

Aby uzyskać identyfikator uruchomienia, czas wykonywania i dodatkowe szczegóły dotyczące przebiegu, użyj [`get_details()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py#get-details--) metody.

```python
print(notebook_run.get_details())
```

Po pomyślnym zakończeniu przebiegu Użyj [`complete()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#complete--set-status-true-) metody, aby oznaczyć ją jako zakończoną.

```python
notebook_run.complete()
print(notebook_run.get_status())
```

Jeśli używasz `with...as` wzorca projektowego Python, przebieg zostanie automatycznie oznaczony jako wykonany, gdy przebieg jest poza zakresem. Nie musisz ręcznie oznaczyć przebiegu jako zakończony.

```python
with exp.start_logging() as notebook_run:
    notebook_run.log(name="message", value="Hello from run!")
    print(notebook_run.get_status())

print(notebook_run.get_status())
```

### <a name="using-the-cli"></a>Korzystanie z interfejsu wiersza polecenia

1. Aby wyświetlić listę przebiegów eksperymentu, użyj następującego polecenia. Zamień na `experiment` nazwę eksperymentu:

    ```azurecli-interactive
    az ml run list --experiment-name experiment
    ```

    To polecenie zwraca dokument JSON zawierający informacje o przebiegach dla tego eksperymentu.

    Aby uzyskać więcej informacji, zobacz [AZ ml eksperyment list](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/experiment?view=azure-cli-latest#ext-azure-cli-ml-az-ml-experiment-list).

2. Aby wyświetlić informacje dotyczące określonego przebiegu, użyj następującego polecenia. Zamień na `runid` Identyfikator przebiegu:

    ```azurecli-interactive
    az ml run show -r runid
    ```

    To polecenie zwraca dokument JSON, który zawiera listę informacji o przebiegu.

    Aby uzyskać więcej informacji, zobacz [AZ ml Run show](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/run?view=azure-cli-latest#ext-azure-cli-ml-az-ml-run-show).


### <a name="using-azure-machine-learning-studio"></a>Korzystanie z programu Azure Machine Learning Studio

Aby wyświetlić liczbę aktywnych przebiegów eksperymentu w programie Studio.

1. Przejdź do sekcji **eksperymenty** . 

1. Wybierz eksperyment.

    Na stronie eksperymentów można zobaczyć liczbę aktywnych elementów docelowych obliczeń i czas trwania każdego uruchomienia. 

1. Wybierz konkretny numer uruchomienia.

1. Na karcie **dzienniki** można znaleźć dzienniki diagnostyczne i błędy dla uruchomienia potoku.


## <a name="cancel-or-fail-runs"></a>Anulowanie lub niepowodzenie przebiegów

Jeśli zauważysz błąd lub jeśli wykonywanie przebiegu trwa zbyt długo, możesz anulować przebieg.

### <a name="using-the-sdk"></a>Używanie zestawu SDK

Aby anulować przebieg przy użyciu zestawu SDK, użyj [`cancel()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#cancel--) metody:

```python
run_config = ScriptRunConfig(source_directory='.', script='hello_with_delay.py')
local_script_run = exp.submit(run_config)
print(local_script_run.get_status())

local_script_run.cancel()
print(local_script_run.get_status())
```

Jeśli przebieg zostanie zakończony, ale zawiera błąd (na przykład użyto nieprawidłowego skryptu szkoleniowego), można użyć [`fail()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)#fail-error-details-none--error-code-none---set-status-true-) metody, aby oznaczyć ją jako niepowodzenie.

```python
local_script_run = exp.submit(run_config)
local_script_run.fail()
print(local_script_run.get_status())
```

### <a name="using-the-cli"></a>Korzystanie z interfejsu wiersza polecenia

Aby anulować uruchomienie przy użyciu interfejsu wiersza polecenia, należy użyć następujące polecenie. Zamień na `runid` Identyfikator przebiegu

```azurecli-interactive
az ml run cancel -r runid -w workspace_name -e experiment_name
```

Aby uzyskać więcej informacji, zobacz [AZ ml Run Cancel](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/run?view=azure-cli-latest#ext-azure-cli-ml-az-ml-run-cancel).

### <a name="using-azure-machine-learning-studio"></a>Korzystanie z programu Azure Machine Learning Studio

Aby anulować uruchomienie w programie Studio, wykonaj następujące czynności:

1. Przejdź do działającego potoku w sekcji **eksperymenty** lub **potoki** . 

1. Wybierz numer uruchomienia potoku, który chcesz anulować.

1. Na pasku narzędzi wybierz pozycję **Anuluj** .


## <a name="create-child-runs"></a>Tworzenie przebiegów podrzędnych

Utwórz uruchomienia podrzędne, aby grupować powiązane z nimi przebiegi, na przykład dla różnych iteracji dostrajania parametrów.

> [!NOTE]
> Uruchomienia podrzędne można tworzyć tylko za pomocą zestawu SDK.

Ten przykład kodu używa `hello_with_children.py` skryptu do tworzenia partii pięciu przebiegów podrzędnych z poziomu przesłanego przebiegu przy użyciu [`child_run()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#child-run-name-none--run-id-none--outputs-none-) metody:

```python
!more hello_with_children.py
run_config = ScriptRunConfig(source_directory='.', script='hello_with_children.py')

local_script_run = exp.submit(run_config)
local_script_run.wait_for_completion(show_output=True)
print(local_script_run.get_status())

with exp.start_logging() as parent_run:
    for c,count in enumerate(range(5)):
        with parent_run.child_run() as child:
            child.log(name="Hello from child run", value=c)
```

> [!NOTE]
> Gdy przechodzą poza zakres, uruchomienia podrzędne są automatycznie oznaczane jako ukończone.

Aby wydajnie tworzyć wiele podrzędnych przebiegów, użyj [`create_children()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run?view=azure-ml-py#create-children-count-none--tag-key-none--tag-values-none-) metody. Ze względu na to, że każde utworzenie powoduje wywołanie sieciowe, tworzenie partii przebiegów jest bardziej wydajne niż ich tworzenie.

### <a name="submit-child-runs"></a>Prześlij uruchomienia podrzędne

Uruchomienia podrzędne mogą być również przesyłane z przebiegu nadrzędnego. Pozwala to na tworzenie hierarchii uruchamiania obiektów nadrzędnych i podrzędnych. 

Możesz chcieć, aby dziecko uruchomiło inną konfigurację przebiegu niż uruchomienie nadrzędne. Na przykład można użyć mniej zaawansowanej konfiguracji opartej na procesorach CPU dla elementu nadrzędnego, a jednocześnie używać konfiguracji opartych na procesorze GPU dla elementów podrzędnych. Innym typowym pragnieniem jest przekazanie każdego elementu podrzędnego różnych argumentów i danych. Aby dostosować uruchomienie podrzędne, należy przekazać `RunConfiguration` obiekt do konstruktora elementu podrzędnego `ScriptRunConfig` . Ten przykład kodu, który będzie częścią `ScriptRunConfig` skryptu obiektu nadrzędnego:

- Tworzy do `RunConfiguration` pobrania nazwany zasób obliczeniowy`"gpu-compute"`
- Wykonuje iterację przez różne wartości argumentu, aby można było przekazywać je do obiektów podrzędnych. `ScriptRunConfig`
- Tworzy i przesyła nowe uruchomienie podrzędne przy użyciu niestandardowego zasobu obliczeniowego i argumentu
- Bloki do momentu zakończenia wszystkich elementów podrzędnych

```python
# parent.py
# This script controls the launching of child scripts
from azureml.core import Run, ScriptRunConfig, RunConfiguration

run_config_for_aml_compute = RunConfiguration()
run_config_for_aml_compute.target = "gpu-compute"
run_config_for_aml_compute.environment.docker.enabled = True 

run = Run.get_context()

child_args = ['Apple', 'Banana', 'Orange']
for arg in child_args: 
    run.log('Status', f'Launching {arg}')
    child_config = ScriptRunConfig(source_directory=".", script='child.py', arguments=['--fruit', arg], run_config = run_config_for_aml_compute)
    # Starts the run asynchronously
    run.submit_child(child_config)

# Experiment will "complete" successfully at this point. 
# Instead of returning immediately, block until child runs complete

for child in run.get_children():
    child.wait_for_completion()
```

Aby utworzyć wiele podrzędnych przebiegów z identycznymi konfiguracjami, argumentami i danymi wejściowymi, użyj [`create_children()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run?view=azure-ml-py#create-children-count-none--tag-key-none--tag-values-none-) metody. Ze względu na to, że każde utworzenie powoduje wywołanie sieciowe, tworzenie partii przebiegów jest bardziej wydajne niż ich tworzenie.

W ramach uruchomienia podrzędnego można wyświetlić identyfikator uruchomienia obiektu nadrzędnego:

```python
## In child run script
child_run = Run.get_context()
child_run.parent.id
```

### <a name="query-child-runs"></a>Uruchomienia podrzędne zapytania

Aby zbadać podrzędne uruchomienia określonego elementu nadrzędnego, użyj [`get_children()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#get-children-recursive-false--tags-none--properties-none--type-none--status-none---rehydrate-runs-true-) metody. ``recursive = True``Argument umożliwia wykonywanie zapytań do zagnieżdżonego drzewa elementów podrzędnych i podrzędne.

```python
print(parent_run.get_children())
```

## <a name="tag-and-find-runs"></a>Tagi i Znajdź przebiegi

W Azure Machine Learning można użyć właściwości i tagów, aby ułatwić organizowanie i wykonywanie zapytań dotyczących przebiegów w celu uzyskania ważnych informacji.

### <a name="add-properties-and-tags"></a>Dodaj właściwości i Tagi

#### <a name="using-the-sdk"></a>Używanie zestawu SDK

Aby dodać metadane z możliwością wyszukiwania do przebiegów, użyj [`add_properties()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#add-properties-properties-) metody. Na przykład poniższy kod dodaje `"author"` Właściwość do przebiegu:

```Python
local_script_run.add_properties({"author":"azureml-user"})
print(local_script_run.get_properties())
```

Właściwości są niezmienne, więc tworzą stałe rekordy do celów inspekcji. Poniższy przykład kodu powoduje błąd, ponieważ został już dodany `"azureml-user"` jako `"author"` wartość właściwości w poprzednim kodzie:

```Python
try:
    local_script_run.add_properties({"author":"different-user"})
except Exception as e:
    print(e)
```

W przeciwieństwie do właściwości, Tagi są modyfikowalne. Aby dodać wyszukiwanie i istotne informacje dla klientów eksperymentu, użyj [`tag()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run(class)?view=azure-ml-py#tag-key--value-none-) metody.

```Python
local_script_run.tag("quality", "great run")
print(local_script_run.get_tags())

local_script_run.tag("quality", "fantastic run")
print(local_script_run.get_tags())
```

Możesz również dodać proste Tagi ciągu. Gdy Tagi są wyświetlane w słowniku tagów jako klucze, mają one wartość `None` .

```Python
local_script_run.tag("worth another look")
print(local_script_run.get_tags())
```

#### <a name="using-the-cli"></a>Korzystanie z interfejsu wiersza polecenia

> [!NOTE]
> Za pomocą interfejsu wiersza polecenia można dodawać i aktualizować tylko Tagi.

Aby dodać lub zaktualizować tag, użyj następującego polecenia:

```azurecli-interactive
az ml run update -r runid --add-tag quality='fantastic run'
```

Aby uzyskać więcej informacji, zobacz [AZ ml Run Update](https://docs.microsoft.com/cli/azure/ext/azure-cli-ml/ml/run?view=azure-cli-latest#ext-azure-cli-ml-az-ml-run-update).

### <a name="query-properties-and-tags"></a>Właściwości zapytania i Tagi

Możesz wykonywać zapytania w ramach eksperymentu, aby zwrócić listę przebiegów zgodnych z określonymi właściwościami i tagami.

#### <a name="using-the-sdk"></a>Używanie zestawu SDK

```Python
list(exp.get_runs(properties={"author":"azureml-user"},tags={"quality":"fantastic run"}))
list(exp.get_runs(properties={"author":"azureml-user"},tags="worth another look"))
```

#### <a name="using-the-cli"></a>Korzystanie z interfejsu wiersza polecenia

Interfejs wiersza polecenia platformy Azure obsługuje zapytania [JMESPath](http://jmespath.org) , które mogą służyć do filtrowania przebiegów w oparciu o właściwości i Tagi. Aby użyć zapytania JMESPath z interfejsem wiersza polecenia platformy Azure, określ go za pomocą `--query` parametru. W poniższych przykładach przedstawiono podstawowe zapytania przy użyciu właściwości i tagów:

```azurecli-interactive
# list runs where the author property = 'azureml-user'
az ml run list --experiment-name experiment [?properties.author=='azureml-user']
# list runs where the tag contains a key that starts with 'worth another look'
az ml run list --experiment-name experiment [?tags.keys(@)[?starts_with(@, 'worth another look')]]
# list runs where the author property = 'azureml-user' and the 'quality' tag starts with 'fantastic run'
az ml run list --experiment-name experiment [?properties.author=='azureml-user' && tags.quality=='fantastic run']
```

Aby uzyskać więcej informacji na temat wykonywania zapytań dotyczących wyników interfejsu wiersza polecenia platformy Azure, zobacz temat [zapytanie dotyczące danych wyjściowych poleceń platformy Azure](https://docs.microsoft.com/cli/azure/query-azure-cli?view=azure-cli-latest).

### <a name="using-azure-machine-learning-studio"></a>Korzystanie z programu Azure Machine Learning Studio

1. Przejdź do sekcji **potoki** .

1. Korzystając z paska wyszukiwania, można filtrować potoki przy użyciu tagów, opisów, nazw eksperymentów i nazwiska osoby przesyłającej.

## <a name="example-notebooks"></a>Przykładowe notesy

W następujących notesach przedstawiono Koncepcje opisane w tym artykule:

* Aby dowiedzieć się więcej na temat interfejsów API rejestrowania, zobacz artykuł [Rejestrowanie interfejsu API](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/track-and-monitor-experiments/logging-api/logging-api.ipynb).

* Aby uzyskać więcej informacji na temat zarządzania przebiegami z zestawem SDK Azure Machine Learning, zobacz [Notes zarządzanie przebiegiem](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/track-and-monitor-experiments/manage-runs/manage-runs.ipynb).

## <a name="next-steps"></a>Następne kroki

* Aby dowiedzieć się, jak rejestrować metryki dla eksperymentów, zobacz [Dziennik metryk podczas przebiegów szkoleniowych](how-to-track-experiments.md).
* Aby dowiedzieć się, jak monitorować zasoby i dzienniki z Azure Machine Learning, zobacz [Azure Machine Learning monitorowania](monitor-azure-machine-learning.md).
