---
title: Importowanie obrazów kontenerów
description: Zaimportuj obrazy kontenerów do usługi Azure Container Registry za pomocą interfejsów API platformy Azure bez konieczności uruchamiania poleceń platformy Docker.
ms.topic: article
ms.date: 03/16/2020
ms.openlocfilehash: a7a6566540880d027b1dc3428d394b352f34318d
ms.sourcegitcommit: 0100d26b1cac3e55016724c30d59408ee052a9ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/07/2020
ms.locfileid: "86023520"
---
# <a name="import-container-images-to-a-container-registry"></a>Importowanie obrazów kontenera do rejestru kontenerów

Można łatwo importować (kopiować) obrazy kontenerów do usługi Azure Container Registry bez używania poleceń platformy Docker. Na przykład zaimportuj obrazy z rejestru projektowego do rejestru produkcyjnego lub skopiuj obrazy podstawowe z rejestru publicznego.

Azure Container Registry obsługuje wiele typowych scenariuszy kopiowania obrazów z istniejącego rejestru:

* Importuj z rejestru publicznego

* Importuj z innego rejestru kontenera platformy Azure, w ramach tej samej lub innej subskrypcji platformy Azure

* Importuj z rejestru kontenerów prywatnych spoza platformy Azure

Importowanie obrazów do usługi Azure Container Registry ma następujące zalety, aby korzystać z poleceń interfejsu wiersza polecenia platformy Docker:

* Ze względu na to, że środowisko klienta nie wymaga lokalnej instalacji platformy Docker, zaimportuj dowolny obraz kontenera niezależnie od obsługiwanego typu systemu operacyjnego.

* Podczas importowania obrazów wieloarchitekturowych (takich jak oficjalne obrazy platformy Docker) obrazy wszystkich architektur i platform określonych na liście manifestów zostaną skopiowane.

Aby zaimportować obrazy kontenerów, ten artykuł wymaga uruchomienia interfejsu wiersza polecenia platformy Azure w Azure Cloud Shell lub lokalnie (zalecane jest w wersji 2.0.55 lub nowszej). Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure][azure-cli].

> [!NOTE]
> Jeśli konieczne jest dystrybuowanie identycznych obrazów kontenerów w wielu regionach platformy Azure, Azure Container Registry obsługuje również [replikację geograficzną](container-registry-geo-replication.md). Dzięki replikacji geograficznej rejestru (wymagana warstwa usługi Premium) można obsłużyć wiele regionów z identycznymi nazwami obrazów i tagów z jednego rejestru.
>

## <a name="prerequisites"></a>Wymagania wstępne

Jeśli nie masz jeszcze usługi Azure Container Registry, Utwórz rejestr. Aby uzyskać instrukcje, zobacz [Szybki Start: Tworzenie prywatnego rejestru kontenerów za pomocą interfejsu wiersza polecenia platformy Azure](container-registry-get-started-azure-cli.md).

Do zaimportowania obrazu do usługi Azure Container Registry tożsamość musi mieć uprawnienia do zapisu w rejestrze docelowym (co najmniej rola współautor). Zobacz [Azure Container Registry ról i uprawnień](container-registry-roles.md). 

## <a name="import-from-a-public-registry"></a>Importuj z rejestru publicznego

### <a name="import-from-docker-hub"></a>Importuj z usługi Docker Hub

Na przykład użyj polecenia [AZ ACR import][az-acr-import] w celu zaimportowania obrazu wieloarchitekturowego `hello-world:latest` z usługi Docker Hub do rejestru o nazwie Moje *Rejestr*. Ponieważ `hello-world` jest oficjalnym obrazem z usługi Docker Hub, ten obraz znajduje się w domyślnym `library` repozytorium. Dołącz nazwę repozytorium i opcjonalnie tag w wartości `--source` parametru obrazu. (Opcjonalnie można zidentyfikować obraz według jego skrótu manifestu zamiast znacznika, który gwarantuje określoną wersję obrazu).
 
```azurecli
az acr import \
  --name myregistry \
  --source docker.io/library/hello-world:latest \
  --image hello-world:latest
```

Aby sprawdzić, czy do tego obrazu są skojarzone wiele manifestów, należy uruchomić `az acr repository show-manifests` polecenie:

```azurecli
az acr repository show-manifests \
  --name myregistry \
  --repository hello-world
```

Poniższy przykład importuje obraz publiczny z `tensorflow` repozytorium w usłudze Docker Hub:

```azurecli
az acr import \
  --name myregistry \
  --source docker.io/tensorflow/tensorflow:latest-gpu \
  --image tensorflow:latest-gpu
```

### <a name="import-from-microsoft-container-registry"></a>Importuj z Container Registry firmy Microsoft

Na przykład zaimportuj `ltsc2019` obraz systemu Windows Server Core z `windows` repozytorium w programie Microsoft Container Registry.

```azurecli
az acr import \
--name myregistry \
--source mcr.microsoft.com/windows/servercore:ltsc2019 \
--image servercore:ltsc2019
```

## <a name="import-from-another-azure-container-registry"></a>Importuj z innego rejestru kontenerów platformy Azure

Możesz zaimportować obraz z innego rejestru kontenera platformy Azure przy użyciu zintegrowanych uprawnień Azure Active Directory.

* Twoja tożsamość musi mieć uprawnienia Azure Active Directory do odczytu z rejestru źródłowego (roli czytelnika) i zapisu w rejestrze docelowym (rola współautora).

* Rejestr może znajdować się w tej samej lub innej subskrypcji platformy Azure w tej samej dzierżawie Active Directory.

### <a name="import-from-a-registry-in-the-same-subscription"></a>Importuj z rejestru w ramach tej samej subskrypcji

Na przykład zaimportuj `aci-helloworld:latest` obraz ze źródłowego rejestru *mysourceregistry* do *rejestru* w tej samej subskrypcji platformy Azure.

```azurecli
az acr import \
  --name myregistry \
  --source mysourceregistry.azurecr.io/aci-helloworld:latest \
  --image aci-helloworld:latest
```

Poniższy przykład importuje obraz przez szyfrowanie manifestu (skrót SHA-256, reprezentowane jako `sha256:...` ), a nie przez tag:

```azurecli
az acr import \
  --name myregistry \
  --source mysourceregistry.azurecr.io/aci-helloworld@sha256:123456abcdefg 
```

### <a name="import-from-a-registry-in-a-different-subscription"></a>Importuj z rejestru w innej subskrypcji

W poniższym przykładzie *mysourceregistry* znajduje się w innej subskrypcji z *rejestru* w tej samej dzierżawie Active Directory. Podaj identyfikator zasobu rejestru źródłowego z `--registry` parametrem. Należy zauważyć, że `--source` parametr określa tylko repozytorium źródłowe i tag, a nie nazwę serwera logowania rejestru.

```azurecli
az acr import \
  --name myregistry \
  --source samples/aci-helloworld:latest \
  --image aci-hello-world:latest \
  --registry /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/sourceResourceGroup/providers/Microsoft.ContainerRegistry/registries/mysourceregistry
```

### <a name="import-from-a-registry-using-service-principal-credentials"></a>Importowanie z rejestru przy użyciu poświadczeń jednostki usługi

Aby zaimportować z rejestru, do którego nie można uzyskać dostępu przy użyciu uprawnień Active Directory, można użyć poświadczeń jednostki usługi (jeśli są dostępne). Podaj identyfikator appID i hasło [nazwy głównej usługi](container-registry-auth-service-principal.md) Active Directory, która ma dostęp ACRPull do rejestru źródłowego. Użycie jednostki usługi jest przydatne w przypadku systemów kompilacji i innych systemów nienadzorowanych, które muszą importować obrazy do rejestru.

```azurecli
az acr import \
  --name myregistry \
  --source sourceregistry.azurecr.io/sourcerrepo:tag \
  --image targetimage:tag \
  --username <SP_App_ID> \
  –-password <SP_Passwd>
```

## <a name="import-from-a-non-azure-private-container-registry"></a>Importuj z rejestru kontenerów prywatnych spoza platformy Azure

Zaimportuj obraz z rejestru prywatnego przez określenie poświadczeń umożliwiających uzyskanie dostępu ściągania do rejestru. Na przykład Pobierz obraz z prywatnego rejestru platformy Docker: 

```azurecli
az acr import \
  --name myregistry \
  --source docker.io/sourcerepo/sourceimage:tag \
  --image sourceimage:tag \
  --username <username> \
  --password <password>
```

## <a name="next-steps"></a>Następne kroki

W tym artykule przedstawiono informacje o importowaniu obrazów kontenerów do usługi Azure Container Registry z rejestru publicznego lub innego rejestru prywatnego. Dodatkowe opcje importowania obrazu można znaleźć w temacie [AZ ACR import][az-acr-import] Command Reference. 


<!-- LINKS - Internal -->
[az-login]: /cli/azure/reference-index#az-login
[az-acr-import]: /cli/azure/acr#az-acr-import
[azure-cli]: /cli/azure/install-azure-cli
