---
title: Korzystanie ze środowisk oprogramowania
titleSuffix: Azure Machine Learning
description: Twórz środowiska i zarządzaj nimi, aby uzyskać model szkoleń i wdrożeń. Zarządzaj pakietami języka Python i innymi ustawieniami środowiska.
services: machine-learning
author: rastala
ms.author: roastala
ms.reviewer: nibaccam
ms.service: machine-learning
ms.subservice: core
ms.date: 07/23/2020
ms.topic: conceptual
ms.custom: how-to, tracking-python
ms.openlocfilehash: 9464544e8c80ad32269ad27ed3592871d0077713
ms.sourcegitcommit: 8def3249f2c216d7b9d96b154eb096640221b6b9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/03/2020
ms.locfileid: "87544034"
---
# <a name="create--use-software-environments-in-azure-machine-learning"></a>Tworzenie & korzystania ze środowisk oprogramowania w programie Azure Machine Learning
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

W tym artykule dowiesz się, jak tworzyć [środowiska](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py)Azure Machine Learning i zarządzać nimi. Używaj środowisk do śledzenia i odtwarzania zależności oprogramowania projektów podczas ich rozwoju.

Zarządzanie zależnościami oprogramowania to typowe zadanie dla deweloperów. Chcesz upewnić się, że kompilacje są odtwarzalne bez rozbudowanej ręcznej konfiguracji oprogramowania. Klasy Azure Machine Learning są `Environment` kontami dla lokalnych rozwiązań programistycznych, takich jak PIP i Conda i rozbudowane programowanie w chmurze za pomocą możliwości platformy Docker.

W przykładach w tym artykule przedstawiono sposób wykonywania tych operacji:

* Utwórz środowisko i określ zależności pakietów.
* Pobierz i zaktualizuj środowiska.
* Użyj środowiska do uczenia się.
* Użyj środowiska do wdrożenia usługi sieci Web.

Aby uzyskać ogólne omówienie sposobu działania środowisk w Azure Machine Learning, zobacz [co to są środowiska ml?](concept-environments.md) Informacje o konfigurowaniu środowisk programistycznych znajdują się [tutaj](how-to-configure-environment.md).

## <a name="prerequisites"></a>Wymagania wstępne

* [Zestaw Azure Machine Learning SDK dla języka Python](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py)
* [Obszar roboczy Azure Machine Learning](how-to-manage-workspace.md)

## <a name="create-an-environment"></a>Tworzenie środowiska

W poniższych sekcjach opisano różne sposoby tworzenia środowiska dla eksperymentów.

### <a name="use-a-curated-environment"></a>Korzystanie z nadzorowanego środowiska

Środowiska nadzorowane zawierają kolekcje pakietów języka Python i są domyślnie dostępne w obszarze roboczym. Te środowiska są obsługiwane przez buforowane obrazy platformy Docker, co zmniejsza koszt przygotowania do uruchomienia. Możesz wybrać jedno z tych popularnych środowisk nadzorowanych, aby zacząć od: 

* Środowisko _Azure-minimalne_ zawiera minimalny zestaw pakietów umożliwiających śledzenie uruchamiania i przekazywanie zasobów. Można go użyć jako punktu wyjścia dla własnego środowiska.

* Środowisko uczenia maszynowego zawiera wspólne pakiety _do_ nauki o danych. Pakiety te obejmują Scikit-uczenie, Pandas, matplotlib i większy zestaw pakietów Azure SDK.

Aby zapoznać się z listą środowisk nadzorowanych, zapoznaj się z [artykułem środowiska nadzorowane](resource-curated-environments.md).

Użyj `Environment.get` metody, aby wybrać jedno z nadzorowanych środowisk:

```python
from azureml.core import Workspace, Environment

ws = Workspace.from_config()
env = Environment.get(workspace=ws, name="AzureML-Minimal")
```

Aby zmodyfikować środowisko nadzorowane, należy je skopiować:

```python
env = Environment.get(workspace=ws, name="AzureML-Tutorial").clone("new_env")
```
Można wyświetlić listę środowisk nadzorowanych i ich pakietów przy użyciu następującego kodu:

```python
envs = Environment.list(workspace=ws)

for env in envs:
    if env.startswith("AzureML"):
        print("Name",env)
        print("packages", envs[env].python.conda_dependencies.serialize_to_string())
```

> [!WARNING]
>  Nie uruchamiaj nazwy środowiska z prefiksem platformy _Azure_ . Ten prefiks jest zarezerwowany dla środowisk nadzorowanych.


### <a name="instantiate-an-environment-object"></a>Tworzenie wystąpienia obiektu środowiska

Aby ręcznie utworzyć środowisko, zaimportuj `Environment` klasę z zestawu SDK. Następnie użyj poniższego kodu, aby utworzyć wystąpienie obiektu środowiska.

```python
from azureml.core.environment import Environment
Environment(name="myenv")
```

W przypadku definiowania własnego środowiska należy wyświetlić listę `azureml-defaults` z wersją >= 1.0.45 jako zależność PIP. Ten pakiet zawiera funkcje, które są konieczne do hostowania modelu jako usługi sieci Web.

### <a name="use-conda-and-pip-specification-files"></a>Użyj plików specyfikacji Conda i PIP

Środowisko można utworzyć na podstawie specyfikacji Conda lub pliku wymagań PIP. Użyj [`from_conda_specification()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py#from-conda-specification-name--file-path-) metody lub [`from_pip_requirements()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py#from-pip-requirements-name--file-path-) metody. W argumencie metody Podaj nazwę środowiska i ścieżkę do pliku, który chcesz. 

```python
# From a Conda specification file
myenv = Environment.from_conda_specification(name = "myenv",
                                             file_path = "path-to-conda-specification-file")

# From a pip requirements file
myenv = Environment.from_pip_requirements(name = "myenv"
                                          file_path = "path-to-pip-requirements-file")                                          
```

### <a name="use-existing-environments"></a>Korzystanie z istniejących środowisk

Jeśli masz istniejące środowisko Conda na komputerze lokalnym, możesz użyć usługi do utworzenia obiektu środowiska. Korzystając z tej strategii, można ponownie użyć lokalnego środowiska interaktywnego na zdalnych uruchomieniach.

Poniższy kod tworzy obiekt środowiska z istniejącego środowiska Conda `mycondaenv` . Używa [`from_existing_conda_environment()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py#from-existing-conda-environment-name--conda-environment-name-) metody.

``` python
myenv = Environment.from_existing_conda_environment(name = "myenv",
                                                    conda_environment_name = "mycondaenv")
```

Definicję środowiska można zapisać w katalogu w łatwo edytowalnym formacie za pomocą [`save_to_directory()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py#save-to-directory-path--overwrite-false-) metody. Po zmodyfikowaniu nowe środowisko można utworzyć wystąpienie przez załadowanie plików z katalogu.

```python
myenv = Environment.save_to_directory(path = "path-to-destination-directory", overwrite = False)
# modify the environment definition
newenv = Environment.load_from_directory(path = "path-to-source-directory")
```

### <a name="create-environments-automatically"></a>Twórz środowiska automatycznie

Automatyczne tworzenie środowiska przez przesłanie przebiegu szkoleniowego. Prześlij przebieg przy użyciu `submit()` metody. Podczas przesyłania przebiegu szkoleniowego Kompilowanie nowego środowiska może potrwać kilka minut. Czas trwania kompilacji zależy od rozmiaru wymaganych zależności. 

Jeśli nie określisz środowiska w konfiguracji uruchomieniowej przed przesłaniem przebiegu, zostanie utworzone środowisko domyślne.

```python
from azureml.core import ScriptRunConfig, Experiment, Environment
# Create experiment 
myexp = Experiment(workspace=ws, name = "environment-example")

# Attach training script and compute target to run config
runconfig = ScriptRunConfig(source_directory=".", script="example.py")
runconfig.run_config.target = "local"

# Submit the run
run = myexp.submit(config=runconfig)

# Show each step of run 
run.wait_for_completion(show_output=True)
```

Podobnie, jeśli używasz [`Estimator`](https://docs.microsoft.com//python/api/azureml-train-core/azureml.train.estimator.estimator?view=azure-ml-py) obiektu do szkolenia, możesz bezpośrednio przesłać wystąpienie szacowania jako uruchomienie bez określania środowiska. `Estimator`Obiekt już hermetyzuje środowisko i element docelowy obliczeń.

## <a name="add-packages-to-an-environment"></a>Dodawanie pakietów do środowiska

Dodaj pakiety do środowiska przy użyciu plików Conda, PIP lub Private koła. Określ każdą zależność pakietu przy użyciu [`CondaDependency`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.conda_dependencies.condadependencies?view=azure-ml-py) klasy. Dodaj go do środowiska `PythonSection` .

### <a name="conda-and-pip-packages"></a>Pakiety Conda i PIP

Jeśli pakiet jest dostępny w repozytorium pakietu Conda, zalecamy użycie instalacji programu Conda zamiast instalacji PIP. Pakiety Conda są zwykle dołączone do wstępnie skompilowanych plików binarnych, które zwiększają niezawodność instalacji.

Poniższy przykład dodaje do środowiska. Dodaje wersję 1.17.0 `numpy` . Dodaje również `pillow` pakiet, `myenv` . W przykładzie zastosowano [`add_conda_package()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.conda_dependencies.condadependencies?view=azure-ml-py#add-conda-package-conda-package-) odpowiednio metodę i [`add_pip_package()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.conda_dependencies.condadependencies?view=azure-ml-py#add-pip-package-pip-package-) metodę.

```python
from azureml.core.environment import Environment
from azureml.core.conda_dependencies import CondaDependencies

myenv = Environment(name="myenv")
conda_dep = CondaDependencies()

# Installs numpy version 1.17.0 conda package
conda_dep.add_conda_package("numpy==1.17.0")

# Installs pillow package
conda_dep.add_pip_package("pillow")

# Adds dependencies to PythonSection of myenv
myenv.python.conda_dependencies=conda_dep
```

Możesz również dodać zmienne środowiskowe do środowiska. Stają się one dostępne przy użyciu systemu operacyjnego. environ —. get w skrypcie szkoleniowym.

```python
myenv.environment_variables = {"MESSAGE":"Hello from Azure Machine Learning"}
```

>[!IMPORTANT]
> Jeśli używasz tej samej definicji środowiska dla innego uruchomienia, usługa Azure Machine Learning ponownie używa buforowanego obrazu środowiska. W przypadku tworzenia środowiska z przypiętym zależnością pakietu, na przykład, ```numpy``` to środowisko będzie korzystać z zainstalowanej wersji pakietu _w momencie tworzenia środowiska_. Ponadto wszystkie przyszłe środowiska ze zgodną definicją będą nadal używane w starej wersji. Aby uzyskać więcej informacji, zobacz [Tworzenie środowiska, buforowanie i ponowne użycie](https://docs.microsoft.com/azure/machine-learning/concept-environments#environment-building-caching-and-reuse).

### <a name="private-python-packages"></a>Prywatne pakiety języka Python

Aby w sposób prywatny i bezpieczny używać pakietów języka Python bez ujawniania ich w publicznej sieci Internet, zapoznaj się z artykułem [jak używać prywatnych pakietów języka Python](how-to-use-private-python-packages.md).

## <a name="manage-environments"></a>Zarządzanie środowiskami

Zarządzaj środowiskami, aby można było je aktualizować, śledzić i ponownie używać w celach obliczeniowych i z innymi użytkownikami obszaru roboczego.

### <a name="register-environments"></a>Zarejestruj środowiska

Środowisko jest automatycznie rejestrowane w obszarze roboczym podczas przesyłania uruchomienia lub wdrożenia usługi sieci Web. Możesz również ręcznie zarejestrować środowisko przy użyciu [`register()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment(class)?view=azure-ml-py#register-workspace-) metody. Ta operacja powoduje, że środowisko jest śledzone i zgodne z wersją w chmurze. Jednostka może być współdzielona między użytkownikami obszaru roboczego.

Poniższy kod rejestruje środowisko w `myenv` `ws` obszarze roboczym.

```python
myenv.register(workspace=ws)
```

W przypadku korzystania ze środowiska po raz pierwszy w szkole lub wdrożeniu zostanie ono zarejestrowane w obszarze roboczym. Następnie zostanie skompilowany i wdrożony w miejscu docelowym obliczeń. Usługa buforuje środowiska. W przypadku korzystania z pamięci podręcznej trwa znacznie mniej czasu niż w przypadku użycia nowej usługi lub aktualizacji, która została zaktualizowana.

### <a name="get-existing-environments"></a>Pobierz istniejące środowiska

`Environment`Klasa oferuje metody umożliwiające pobieranie istniejących środowisk w obszarze roboczym. Możesz pobrać środowiska według nazwy, listy lub określonego przebiegu szkoleniowego. Te informacje ułatwiają rozwiązywanie problemów, inspekcję i odtwarzalność.

#### <a name="view-a-list-of-environments"></a>Wyświetlanie listy środowisk

Wyświetl środowiska w obszarze roboczym przy użyciu [`Environment.list(workspace="workspace_name")`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment(class)?view=azure-ml-py#list-workspace-) klasy. Następnie wybierz środowisko do ponownego użycia.

#### <a name="get-an-environment-by-name"></a>Pobieranie środowiska według nazwy

Możesz również uzyskać określone środowisko według nazwy i wersji. Poniższy kod używa [`get()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment(class)?view=azure-ml-py#get-workspace--name--version-none-) metody do pobierania wersji `1` `myenv` środowiska w `ws` obszarze roboczym.

```python
restored_environment = Environment.get(workspace=ws,name="myenv",version="1")
```

#### <a name="train-a-run-specific-environment"></a>Uczenie środowiska określonego dla uruchamiania

Aby uzyskać środowisko, które zostało użyte do określonego uruchomienia po zakończeniu szkolenia, użyj [`get_environment()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run?view=azure-ml-py#get-environment--) metody w `Run` klasie.

```python
from azureml.core import Run
Run.get_environment()
```

### <a name="update-an-existing-environment"></a>Aktualizowanie istniejącego środowiska

Załóżmy, że zmienisz istniejące środowisko, na przykład dodając pakiet języka Python. To zajmie czas kompilowania, ponieważ nowa wersja środowiska zostanie utworzona podczas przesyłania przebiegu, wdrażania modelu lub ręcznego rejestrowania środowiska. Wersja pozwala przeglądać zmiany środowiska w miarę upływu czasu. 

Aby zaktualizować wersję pakietu języka Python w istniejącym środowisku, określ numer wersji tego pakietu. Jeśli nie używasz dokładnego numeru wersji, Azure Machine Learning będzie ponownie używać istniejącego środowiska z oryginalnymi wersjami pakietu.

### <a name="debug-the-image-build"></a>Debuguj kompilację obrazu

Poniższy przykład używa [`build()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment(class)?view=azure-ml-py#build-workspace--image-build-compute-none-) metody do ręcznego tworzenia środowiska jako obrazu platformy Docker. Monitoruje dzienniki wyjściowe z kompilacji obrazu przy użyciu [`wait_for_completion()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.image(class)?view=azure-ml-py#wait-for-creation-show-output-false-) . Utworzony obraz zostanie wyświetlony w wystąpieniu Azure Container Registry obszaru roboczego. Te informacje ułatwiają debugowanie.

```python
from azureml.core import Image
build = env.build(workspace=ws)
build.wait_for_completion(show_output=True)
```

Warto najpierw skompilować obrazy lokalnie przy użyciu [`build_local()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py#build-local-workspace--platform-none----kwargs-) metody. Ustawienie parametru opcjonalnego `pushImageToWorkspaceAcr = True` spowoduje wypychanie obrazu do rejestru kontenerów obszaru roboczego usługi Azure ml. 

## <a name="enable-docker"></a>Włącz platformę Docker

Kontener platformy Docker zapewnia wydajny sposób hermetyzacji zależności. Po włączeniu platformy Docker usługa Azure ML kompiluje obraz platformy Docker i utworzy środowisko Python w tym kontenerze z uwzględnieniem specyfikacji. Obrazy platformy Docker są buforowane i ponownie używane: pierwszy przebieg w nowym środowisku zazwyczaj trwa dłużej, gdy obraz jest kompilowany.

[`DockerSection`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.dockersection?view=azure-ml-py) `Environment` Klasa Azure Machine Learning umożliwia precyzyjne dostosowywanie i kontrolowanie systemu operacyjnego gościa, w którym jest uruchamiane szkolenie. `arguments`Zmienna może służyć do określania dodatkowych argumentów do przekazania do polecenia Docker Run.

```python
# Creates the environment inside a Docker container.
myenv.docker.enabled = True
```

Nowo utworzony obraz platformy Docker jest domyślnie wyświetlany w rejestrze kontenerów skojarzonym z obszarem roboczym.  Nazwa repozytorium ma postać *Azure/azureml_ \<uuid\> *. Część unikatowego identyfikatora (*UUID*) nazwy odpowiada skrótowi obliczonemu na podstawie konfiguracji środowiska. Ta zgodność umożliwia usłudze określenie, czy obraz danego środowiska już istnieje do ponownego użycia.

### <a name="use-a-custom-docker-image-or-dockerfile"></a>Używanie niestandardowego obrazu platformy Docker lub pliku dockerfile 
Usługa automatycznie używa jednego z [obrazów podstawowych](https://github.com/Azure/AzureML-Containers)opartych na Ubuntu Linux. Instaluje określone pakiety języka Python. Obraz podstawowy ma wersje procesora CPU i procesora GPU. Azure Machine Learning automatycznie wykrywa, która wersja ma być używana. Istnieje również możliwość użycia [niestandardowego obrazu platformy Docker](https://docs.microsoft.com/azure/machine-learning/how-to-deploy-custom-docker-image#create-a-custom-base-image).

```python
# Specify custom Docker base image and registry, if you don't want to use the defaults
myenv.docker.base_image="your_base-image"
myenv.docker.base_image_registry="your_registry_location"
```

>[!IMPORTANT]
> Azure Machine Learning obsługuje tylko obrazy platformy Docker, które udostępniają następujące oprogramowanie:
> * Ubuntu 16,04 lub nowszy.
> * Conda 4.5. # lub nowszej.
> * Python 3.5. #, 3.6. # lub 3.7. #.

Możesz również określić niestandardowy pliku dockerfile. Najprostszym rozwiązaniem jest rozpoczęcie od jednego z Azure Machine Learning obrazów podstawowych przy użyciu polecenia Docker ```FROM``` , a następnie dodanie własnych kroków niestandardowych. Użyj tej metody, jeśli chcesz zainstalować pakiety inne niż środowisko Python jako zależności. Pamiętaj, aby ustawić obraz podstawowy na brak.

```python
# Specify docker steps as a string. 
dockerfile = r"""
FROM mcr.microsoft.com/azureml/base:intelmpi2018.3-ubuntu16.04
RUN echo "Hello from custom container!"
"""

# Set base image to None, because the image is defined by dockerfile.
myenv.docker.base_image = None
myenv.docker.base_dockerfile = dockerfile

# Alternatively, load the string from a file.
myenv.docker.base_image = None
myenv.docker.base_dockerfile = "./Dockerfile"
```

### <a name="specify-your-own-python-interpreter"></a>Określ własny interpreter języka Python

W niektórych sytuacjach niestandardowy obraz podstawowy może już zawierać środowisko Python z pakietami, które mają być używane.

Aby użyć własnych zainstalowanych pakietów i wyłączyć Conda, należy ustawić parametr `Environment.python.user_managed_dependencies = True` . Upewnij się, że obraz podstawowy zawiera interpreter języka Python i ma pakiety, których potrzebuje Twój skrypt szkoleniowy.

Na przykład aby uruchomić program w podstawowym środowisku Miniconda z zainstalowanym pakietem NumPy, należy najpierw określić pliku dockerfile z krokiem instalacji pakietu. Następnie ustaw zależności zarządzane przez użytkownika na `True` . 

Możesz również określić ścieżkę do określonego interpretera języka Python w obrazie, ustawiając `Environment.python.interpreter_path` zmienną.

```python
dockerfile = """
FROM mcr.microsoft.com/azureml/base:intelmpi2018.3-ubuntu16.04
RUN conda install numpy
"""

myenv.docker.base_image = None
myenv.docker.base_dockerfile = dockerfile
myenv.python.user_managed_dependencies=True
myenv.python.interpreter_path = "/opt/miniconda/bin/python"
```

> [!WARNING]
> Jeśli zainstalowano niektóre zależności języka Python w obrazie platformy Docker i zapomnisz ustawić user_managed_dependencies = true, te pakiety nie będą istniały w środowisku wykonywania w taki sposób, aby powodowały błędy środowiska uruchomieniowego. Domyślnie platforma Azure ML utworzy środowisko Conda z określonymi zależnościami i wykona przebieg w tym środowisku, zamiast korzystać z bibliotek języka Python zainstalowanych na podstawowym obrazie.

## <a name="use-environments-for-training"></a>Używanie środowisk do szkolenia

Aby przesłać przebieg szkolenia, należy połączyć środowisko, [obiekt docelowy obliczeń](concept-compute-target.md)i Twój szkoleniowy skrypt w języku Python do konfiguracji uruchomieniowej. Ta konfiguracja jest obiektem otoki używanym do przesyłania przebiegów.

Podczas przesyłania przebiegu szkoleniowego Kompilowanie nowego środowiska może potrwać kilka minut. Czas trwania zależy od rozmiaru wymaganych zależności. Środowiska są buforowane przez usługę. Tak długo, jak definicja środowiska pozostaje niezmieniona, ponosisz pełny czas instalacji tylko raz.

Poniższy przykład lokalnego uruchomienia skryptu pokazuje, gdzie używać [`ScriptRunConfig`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.script_run_config.scriptrunconfig?view=azure-ml-py) jako obiektu otoki.

```python
from azureml.core import ScriptRunConfig, Experiment
from azureml.core.environment import Environment

exp = Experiment(name="myexp", workspace = ws)
# Instantiate environment
myenv = Environment(name="myenv")

# Add training script to run config
runconfig = ScriptRunConfig(source_directory=".", script="train.py")

# Attach compute target to run config
runconfig.run_config.target = "local"

# Attach environment to run config
runconfig.run_config.environment = myenv

# Submit run 
run = exp.submit(runconfig)
```

> [!NOTE]
> Aby wyłączyć historię uruchamiania lub uruchamiać migawki, użyj ustawienia w obszarze `ScriptRunConfig.run_config.history` .

Jeśli nie określisz środowiska w konfiguracji uruchomieniowej, usługa utworzy środowisko domyślne podczas przesyłania przebiegu.

### <a name="use-an-estimator-for-training"></a>Użyj szacowania do szkolenia

Jeśli używasz [szacowania](how-to-train-ml-models.md) do szkolenia, możesz przesłać wystąpienie usługi szacowania bezpośrednio. Jest już hermetyzowane środowisko i element docelowy obliczeń.

Poniższy kod używa szacowania do szkolenia jednego węzła. Jest ona uruchamiana na zdalnym obliczaniu dla `scikit-learn` modelu. Przyjęto założenie, że wcześniej utworzono obiekt docelowy obliczeń, `compute_target` i obiekt magazynu danych `ds` .

```python
from azureml.train.estimator import Estimator

script_params = {
    '--data-folder': ds.as_mount(),
    '--regularization': 0.8
}

sk_est = Estimator(source_directory='./my-sklearn-proj',
                   script_params=script_params,
                   compute_target=compute_target,
                   entry_script='train.py',
                   conda_packages=['scikit-learn'])

# Submit the run 
run = experiment.submit(sk_est)
```
### <a name="retrieve-dockerfile-from-a-run"></a>Pobierz pliku dockerfile z przebiegu

Użyj poniższego kodu, aby uzyskać pliku dockerfile dla uruchomienia z obsługą platformy Docker.
```python
print(run.get_environment().get_image_details().dockerfile)
```

## <a name="use-environments-for-web-service-deployment"></a>Używanie środowisk do wdrażania usług sieci Web

Środowiska można używać podczas wdrażania modelu jako usługi sieci Web. Ta funkcja umożliwia przeodtwarzalny, połączony przepływ pracy. W tym przepływie pracy można nauczyć, testować i wdrażać model przy użyciu tych samych bibliotek w ramach obliczeń szkoleniowych i obliczeń wnioskowania.

Aby wdrożyć usługę sieci Web, Połącz środowisko, obliczenia wnioskowania, skrypt oceniania i zarejestrowany model w obiekcie wdrożenia [`deploy()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#deploy-workspace--name--models--inference-config-none--deployment-config-none--deployment-target-none--overwrite-false-) . Aby uzyskać więcej informacji, zobacz [jak i gdzie wdrażać modele](how-to-deploy-and-where.md).

W tym przykładzie przyjęto założenie, że zostało wykonane uruchomienie szkoleniowe. Teraz chcesz wdrożyć ten model do Azure Container Instances. Podczas kompilowania usługi sieci Web pliki modelu i oceniania są instalowane na obrazie, a do obrazu zostanie dodany stos wnioskowania Azure Machine Learning.

```python
from azureml.core.model import InferenceConfig, Model
from azureml.core.webservice import AciWebservice, Webservice

# Register the model to deploy
model = run.register_model(model_name = "mymodel", model_path = "outputs/model.pkl")

# Combine scoring script & environment in Inference configuration
inference_config = InferenceConfig(entry_script="score.py", environment=myenv)

# Set deployment configuration
deployment_config = AciWebservice.deploy_configuration(cpu_cores = 1, memory_gb = 1)

# Define the model, inference, & deployment configuration and web service name and location to deploy
service = Model.deploy(
    workspace = ws,
    name = "my_web_service",
    models = [model],
    inference_config = inference_config,
    deployment_config = deployment_config)
```

## <a name="notebooks"></a>Notebooks

Ten [artykuł](https://docs.microsoft.com/azure/machine-learning/how-to-run-jupyter-notebooks#add-new-kernels) zawiera informacje dotyczące sposobu instalowania środowiska Conda jako jądra w notesie.

Ten [przykładowy Notes](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training/using-environments) rozwija się wraz z pojęciami i metodami pokazanymi w tym artykule.

W tym [przykładowym notesie](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-on-local/train-on-local.ipynb) pokazano, jak uczenie modelu lokalnie przy użyciu różnych typów środowisk.

[Wdrożenie modelu przy użyciu niestandardowego obrazu podstawowego platformy Docker](how-to-deploy-custom-docker-image.md) demonstruje sposób wdrażania modelu przy użyciu niestandardowego obrazu podstawowego platformy Docker.

W tym [przykładowym notesie](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/deployment/spark) pokazano, jak wdrożyć model Spark jako usługę sieci Web.

## <a name="create-and-manage-environments-with-the-cli"></a>Tworzenie środowisk i zarządzanie nimi za pomocą interfejsu wiersza polecenia

[Interfejs wiersza polecenia Azure Machine Learning](reference-azure-machine-learning-cli.md) stanowi odbicie większości funkcji zestawu SDK języka Python. Służy do tworzenia środowisk i zarządzania nimi. Polecenia omówione w tej sekcji przedstawiają podstawowe funkcje.

Następujące polecenie tworzy szkielet plików dla domyślnej definicji środowiska w określonym katalogu. Te pliki są plikami JSON. Działają one podobnie do odpowiedniej klasy w zestawie SDK. Za pomocą tych plików można tworzyć nowe środowiska z ustawieniami niestandardowymi. 

```azurecli-interactive
az ml environment scaffold -n myenv -d myenvdir
```

Uruchom następujące polecenie, aby zarejestrować środowisko z określonego katalogu.

```azurecli-interactive
az ml environment register -d myenvdir
```

Uruchom następujące polecenie, aby wyświetlić listę wszystkich zarejestrowanych środowisk.

```azurecli-interactive
az ml environment list
```

Pobierz zarejestrowane środowisko przy użyciu następującego polecenia.

```azurecli-interactive
az ml environment download -n myenv -d downloaddir
```

## <a name="next-steps"></a>Następne kroki

* Aby użyć zarządzanego obiektu docelowego obliczeń do uczenia modelu, zobacz [Samouczek: uczenie modelu](tutorial-train-models-with-aml.md).
* Po uzyskaniu przeszkolonego modelu Dowiedz się, [jak i gdzie wdrażać modele](how-to-deploy-and-where.md).
* Wyświetl [ `Environment` odwołanie do zestawu SDK klasy](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment(class)?view=azure-ml-py).
