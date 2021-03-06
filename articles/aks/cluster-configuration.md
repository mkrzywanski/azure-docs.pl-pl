---
title: Konfiguracja klastra w usłudze Azure Kubernetes Services (AKS)
description: Dowiedz się, jak skonfigurować klaster w usłudze Azure Kubernetes Service (AKS)
services: container-service
ms.topic: conceptual
ms.date: 07/02/2020
ms.author: jpalma
author: palma21
ms.openlocfilehash: f1329aa056e8d1db951e01555634cf1ea709608b
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2020
ms.locfileid: "86252015"
---
# <a name="configure-an-aks-cluster"></a>Konfigurowanie klastra AKS

W ramach tworzenia klastra AKS może być konieczne dostosowanie konfiguracji klastra zgodnie z potrzebami. W tym artykule przedstawiono kilka opcji dostosowywania klastra AKS.

## <a name="os-configuration-preview"></a>Konfiguracja systemu operacyjnego (wersja zapoznawcza)

Program AKS obsługuje teraz Ubuntu 18,04 jako system operacyjny węzła (OS) w wersji zapoznawczej. W trakcie okresu zapoznawczego dostępne są zarówno Ubuntu 16,04, jak i Ubuntu 18,04.

> [!IMPORTANT]
> Pule węzłów utworzone w Kubernetes v 1.18 lub nowszym są domyślne dla wymaganego `AKS Ubuntu 18.04` obrazu węzła. Pule węzłów w obsługiwanej wersji Kubernetes mniejszej niż 1,18 `AKS Ubuntu 16.04` są odbierane jako obraz węzła, ale zostaną zaktualizowane do `AKS Ubuntu 18.04` chwili, gdy wersja Kubernetes puli węzłów zostanie zaktualizowana do wersji v 1.18 lub nowszej.
> 
> Zdecydowanie zaleca się przetestowanie obciążeń w puli węzłów AKS Ubuntu 18,04 przed użyciem klastrów w witrynie 1,18 lub nowszej. Przeczytaj, jak [przetestować pule węzłów Ubuntu 18,04](#use-aks-ubuntu-1804-existing-clusters-preview).

Wymagane są następujące zasoby:

- [Interfejs wiersza polecenia platformy Azure][azure-cli-install]w wersji 2.2.0 lub nowszej
- Rozszerzenie AKS-Preview 0.4.35

Aby zainstalować rozszerzenie AKS-Preview 0.4.35 lub nowsze, użyj następujących poleceń interfejsu wiersza polecenia platformy Azure:

```azurecli
az extension add --name aks-preview
az extension list
```

Zarejestruj `UseCustomizedUbuntuPreview` funkcję:

```azurecli
az feature register --name UseCustomizedUbuntuPreview --namespace Microsoft.ContainerService
```

Wyświetlenie stanu jako **zarejestrowanego**może potrwać kilka minut. Stan rejestracji można sprawdzić za pomocą polecenia [AZ Feature list](/cli/azure/feature?view=azure-cli-latest#az-feature-list) :

```azurecli
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/UseCustomizedUbuntuPreview')].{Name:name,State:properties.state}"
```

Gdy stan jest wyświetlany jako zarejestrowane, Odśwież rejestrację `Microsoft.ContainerService` dostawcy zasobów przy użyciu polecenia [AZ Provider Register](/cli/azure/provider?view=azure-cli-latest#az-provider-register) :

```azurecli
az provider register --namespace Microsoft.ContainerService
```

### <a name="use-aks-ubuntu-1804-on-new-clusters-preview"></a>Korzystanie z AKS Ubuntu 18,04 w nowych klastrach (wersja zapoznawcza)

Skonfiguruj klaster tak, aby używał Ubuntu 18,04 podczas tworzenia klastra. Użyj `--aks-custom-headers` flagi, aby ustawić Ubuntu 18,04 jako domyślny system operacyjny.

```azurecli
az aks create --name myAKSCluster --resource-group myResourceGroup --aks-custom-headers CustomizedUbuntu=aks-ubuntu-1804
```

Jeśli chcesz utworzyć klastry z obrazem AKS Ubuntu 16,04, możesz to zrobić, pomijając `--aks-custom-headers` znacznik niestandardowy.

### <a name="use-aks-ubuntu-1804-existing-clusters-preview"></a>Używanie istniejących klastrów w programie AKS Ubuntu 18,04 (wersja zapoznawcza)

Skonfiguruj nową pulę węzłów do używania Ubuntu 18,04. Użyj `--aks-custom-headers` flagi, aby ustawić Ubuntu 18,04 jako domyślny system operacyjny dla tej puli węzłów.

```azurecli
az aks nodepool add --name ubuntu1804 --cluster-name myAKSCluster --resource-group myResourceGroup --aks-custom-headers CustomizedUbuntu=aks-ubuntu-1804
```

Jeśli chcesz utworzyć pule węzłów za pomocą obrazu AKS Ubuntu 16,04, możesz to zrobić, pomijając `--aks-custom-headers` tag niestandardowy.


## <a name="container-runtime-configuration-preview"></a>Konfiguracja środowiska uruchomieniowego kontenera (wersja zapoznawcza)

Środowisko uruchomieniowe kontenera to oprogramowanie, które wykonuje kontenery i zarządza obrazami kontenerów w węźle. Środowisko uruchomieniowe ułatwia abstrakcyjne wywołania sys lub systemu operacyjnego (OS) do uruchamiania kontenerów w systemie Linux lub Windows. Dzisiaj AKS korzysta z [Moby](https://mobyproject.org/) (nadrzędnym Docker) jako środowiska uruchomieniowego kontenera. 
    
![Docker CRI](media/cluster-configuration/docker-cri.png)

[`Containerd`](https://containerd.io/)jest zgodnym podstawowym środowiskiem uruchomieniowym [kontenera (Open](https://opencontainers.org/) Container Initiative), który zapewnia minimalny zestaw funkcji wymaganych do wykonywania kontenerów i zarządzania obrazami w węźle. Zostało ono [przekazano do](https://www.cncf.io/announcement/2017/03/29/containerd-joins-cloud-native-computing-foundation/) natywnej usługi obliczeniowej Cloud Foundation (CNCF) w marcu 2017. Bieżąca wersja Moby używana przez AKS już dziś i została utworzona w oparciu o `containerd` , jak pokazano powyżej. 

W przypadku węzłów węzła i węzłów opartych na kontenerach, a nie rozmowy z `dockershim` , kubelet będzie komunikować się bezpośrednio z `containerd` za pośrednictwem wtyczki CRI (interfejs środowiska uruchomieniowego kontenera), usuwając dodatkowe przeskoki w przepływie w porównaniu do implementacji platformy Docker CRI. W związku z tym zobaczysz lepszy czas oczekiwania na uruchomienie i mniejszą ilość zasobów (procesor CPU i pamięć).

Przy użyciu `containerd` for AKS nodes, pod kątem opóźnień uruchamiania, zwiększa i zmniejsza zużycie zasobów węzła przez środowisko uruchomieniowe kontenera. Te ulepszenia są włączane przez tę nową architekturę, w której kubelet się bezpośrednio do programu `containerd` za pomocą wtyczki CRI, a w przypadku architektury Moby/Docker kubelet będzie komunikować się z `dockershim` aparatem platformy Docker przed osiągnięciem `containerd` , dzięki czemu mają dodatkowe przeskoki w przepływie.

![Docker CRI](media/cluster-configuration/containerd-cri.png)

`Containerd`działa na każdej wersji systemu Kubernetes w AKS, a w każdej wersji Kubernetes w strumieniu, w którym znajduje się nowsza wersja, i obsługuje wszystkie funkcje Kubernetes i AKS.

> [!IMPORTANT]
> Gdy `containerd` stanie się ogólnie dostępna w usłudze AKS, będzie to ustawienie domyślne i opcja dostępna tylko dla środowiska uruchomieniowego kontenera w nowych klastrach. Nadal można używać Moby nodepools i klastrów w starszych obsługiwanych wersjach, dopóki te nie zostaną objęte wsparciem. 
> 
> Zalecamy przetestowanie obciążeń w `containerd` pulach węzłów przed uaktualnieniem lub utworzeniem nowych klastrów przy użyciu tego środowiska uruchomieniowego kontenera.

### <a name="use-containerd-as-your-container-runtime-preview"></a>Użyj `containerd` jako środowiska uruchomieniowego kontenera (wersja zapoznawcza)

Wymagane są następujące wymagania wstępne:

- Zainstalowano [interfejs wiersza polecenia platformy Azure][azure-cli-install]w wersji 2.8.0 lub nowszej
- Rozszerzenie AKS-Preview w wersji 0.4.53 lub nowszej
- `UseCustomizedContainerRuntime`Zarejestrowano flagę funkcji
- `UseCustomizedUbuntuPreview`Zarejestrowano flagę funkcji

Aby zainstalować rozszerzenie AKS-Preview 0.4.53 lub nowsze, użyj następujących poleceń interfejsu wiersza polecenia platformy Azure:

```azurecli
az extension add --name aks-preview
az extension list
```

Zarejestruj `UseCustomizedContainerRuntime` funkcje i `UseCustomizedUbuntuPreview` :

```azurecli
az feature register --name UseCustomizedContainerRuntime --namespace Microsoft.ContainerService
az feature register --name UseCustomizedUbuntuPreview --namespace Microsoft.ContainerService

```

Wyświetlenie stanu jako **zarejestrowanego**może potrwać kilka minut. Stan rejestracji można sprawdzić za pomocą polecenia [AZ Feature list](/cli/azure/feature?view=azure-cli-latest#az-feature-list) :

```azurecli
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/UseCustomizedContainerRuntime')].{Name:name,State:properties.state}"
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/UseCustomizedUbuntuPreview')].{Name:name,State:properties.state}"
```

Gdy stan jest wyświetlany jako zarejestrowane, Odśwież rejestrację `Microsoft.ContainerService` dostawcy zasobów przy użyciu polecenia [AZ Provider Register](/cli/azure/provider?view=azure-cli-latest#az-provider-register) :

```azurecli
az provider register --namespace Microsoft.ContainerService
```  

### <a name="use-containerd-on-new-clusters-preview"></a>Użyj `containerd` w nowych klastrach (wersja zapoznawcza)

Skonfiguruj klaster, który ma być używany `containerd` podczas tworzenia klastra. Użyj `--aks-custom-headers` flagi, aby ustawić `containerd` jako środowisko uruchomieniowe kontenera.

> [!NOTE]
> `containerd`Środowisko uruchomieniowe jest obsługiwane tylko w węzłach i pulach węzłów przy użyciu obrazu AKS Ubuntu 18,04.

```azurecli
az aks create --name myAKSCluster --resource-group myResourceGroup --aks-custom-headers CustomizedUbuntu=aks-ubuntu-1804,ContainerRuntime=containerd
```

Jeśli chcesz utworzyć klastry za pomocą środowiska uruchomieniowego Moby (Docker), możesz to zrobić, pomijając tag niestandardowy `--aks-custom-headers` .

### <a name="use-containerd-on-existing-clusters-preview"></a>Użyj `containerd` w istniejących klastrach (wersja zapoznawcza)

Skonfiguruj nową pulę węzłów do użycia `containerd` . Użyj `--aks-custom-headers` flagi, aby ustawić `containerd` jako środowisko uruchomieniowe dla tej puli węzłów.

```azurecli
az aks nodepool add --name ubuntu1804 --cluster-name myAKSCluster --resource-group myResourceGroup --aks-custom-headers CustomizedUbuntu=aks-ubuntu-1804,ContainerRuntime=containerd
```

Jeśli chcesz utworzyć pule węzłów za pomocą środowiska uruchomieniowego Moby (Docker), możesz to zrobić, pomijając `--aks-custom-headers` tag niestandardowy.


### <a name="containerd-limitationsdifferences"></a>`Containerd`ograniczenia/różnice

* Aby użyć `containerd` programu jako środowiska uruchomieniowego kontenera, musisz użyć AKS Ubuntu 18,04 jako obrazu podstawowego systemu operacyjnego.
* Mimo że zestaw narzędzi platformy Docker nadal znajduje się w węzłach, Kubernetes używa `containerd` jako środowiska uruchomieniowego kontenera. W związku z tym, ponieważ Moby/Docker nie zarządza kontenerami utworzonymi przez Kubernetes w węzłach, nie można wyświetlać kontenerów ani korzystać z nich przy użyciu poleceń platformy Docker (takich jak `docker ps` ) ani interfejsu API platformy Docker.
* W przypadku programu `containerd` zalecamy używanie [`crictl`](https://kubernetes.io/docs/tasks/debug-application-cluster/crictl) jako zastępczego interfejsu wiersza polecenia zamiast interfejsu wiersza polecenia platformy Docker w celu **rozwiązywania problemów** z magazynami, kontenerami i obrazami kontenerów w węzłach Kubernetes (na przykład `crictl ps` ). 
   * Nie zapewnia to pełnej funkcjonalności interfejsu wiersza polecenia platformy Docker. Jest ona przeznaczona tylko do rozwiązywania problemów.
   * `crictl`oferuje bardziej przyjazny kubernetesy widok kontenerów z pojęciami, takimi jak zasobniki itp.
* `Containerd`konfiguruje rejestrowanie przy użyciu standardowego `cri` formatu rejestrowania (który jest inny niż to, co jest obecnie uzyskiwane ze sterownika JSON platformy Docker). Twoje rozwiązanie do rejestrowania musi obsługiwać `cri` Format rejestrowania (na przykład [Azure monitor for Containers](../azure-monitor/insights/container-insights-enable-new-cluster.md))
* Nie można już uzyskiwać dostępu do aparatu platformy Docker `/var/run/docker.sock` lub używać platformy Docker-Docker (DinD).
  * W przypadku obecnie wyodrębniania dzienników aplikacji lub monitorowania danych z aparatu platformy Docker należy zamiast tego użyć takich elementów jak [Azure monitor for Containers](../azure-monitor/insights/container-insights-enable-new-cluster.md) . Ponadto AKS nie obsługuje uruchamiania jakichkolwiek poleceń poza pasmem w węzłach agenta, które mogą spowodować niestabilność.
  * Nawet w przypadku korzystania z Moby/Docker nie zaleca się tworzenia obrazów i bezpośrednio wykorzystujących aparat platformy Docker za pomocą powyższych metod. Kubernetes nie jest w pełni świadomy tych używanych zasobów, a te podejścia omawiają wiele problemów szczegółowych [tutaj](https://jpetazzo.github.io/2015/09/03/do-not-use-docker-in-docker-for-ci/) [, a na](https://securityboulevard.com/2018/05/escaping-the-whale-things-you-probably-shouldnt-do-with-docker-part-1/)przykład.
* Kompilowanie obrazów — Zalecanym podejściem do tworzenia obrazów jest użycie [zadań ACR](../container-registry/container-registry-quickstart-task-cli.md). Alternatywnym podejściem jest użycie bardziej bezpiecznych opcji w klastrze, takich jak [Docker buildx](https://github.com/docker/buildx).

## <a name="generation-2-virtual-machines-preview"></a>Maszyny wirtualne generacji 2 (wersja zapoznawcza)

Platforma Azure obsługuje [maszyny wirtualne generacji 2 (Gen2)](../virtual-machines/windows/generation-2.md). Maszyny wirtualne generacji 2 obsługują kluczowe funkcje, które nie są obsługiwane w maszynach wirtualnych generacji 1 (Gen1). Te funkcje obejmują zwiększoną ilość pamięci, rozszerzenia funkcji ochrony oprogramowania firmy Intel (Intel SGX) i zwirtualizowaną pamięć trwałą (vPMEM).

Maszyny wirtualne generacji 2 wykorzystują nową architekturę rozruchową opartą na interfejsie UEFI zamiast architektury opartej na systemie BIOS używanej przez maszyny wirtualne generacji 1.
Tylko określone jednostki SKU i rozmiary obsługują maszyny wirtualne Gen2. Sprawdź [listę obsługiwanych rozmiarów](../virtual-machines/windows/generation-2.md#generation-2-vm-sizes), aby sprawdzić, czy jednostka SKU obsługuje lub wymaga Gen2.

Ponadto nie wszystkie obrazy maszyn wirtualnych obsługują Gen2, na maszynach wirtualnych AKS Gen2 będzie używany nowy [obraz AKS Ubuntu 18,04](#os-configuration-preview). Ten obraz obsługuje wszystkie jednostki SKU i rozmiary Gen2.

Aby korzystać z maszyn wirtualnych Gen2 w wersji zapoznawczej, wymagane są:
- `aks-preview`Rozszerzenie interfejsu wiersza polecenia zostało zainstalowane.
- `Gen2VMPreview`Zarejestrowano flagę funkcji.

Zarejestruj `Gen2VMPreview` funkcję:

```azurecli
az feature register --name Gen2VMPreview --namespace Microsoft.ContainerService
```

Wyświetlenie stanu jako **zarejestrowanego**może potrwać kilka minut. Stan rejestracji można sprawdzić za pomocą polecenia [AZ Feature list](/cli/azure/feature?view=azure-cli-latest#az-feature-list) :

```azurecli
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/Gen2VMPreview')].{Name:name,State:properties.state}"
```

Gdy stan jest wyświetlany jako zarejestrowane, Odśwież rejestrację `Microsoft.ContainerService` dostawcy zasobów przy użyciu polecenia [AZ Provider Register](/cli/azure/provider?view=azure-cli-latest#az-provider-register) :

```azurecli
az provider register --namespace Microsoft.ContainerService
```

Aby zainstalować rozszerzenie interfejsu wiersza polecenia AKS-Preview, użyj następujących poleceń interfejsu wiersza polecenia platformy Azure:

```azurecli
az extension add --name aks-preview
```

Aby zaktualizować rozszerzenie interfejsu wiersza polecenia AKS-Preview, użyj następujących poleceń interfejsu wiersza polecenia platformy Azure:

```azurecli
az extension update --name aks-preview
```

### <a name="use-gen2-vms-on-new-clusters-preview"></a>Korzystanie z maszyn wirtualnych Gen2 w nowych klastrach (wersja zapoznawcza)
Skonfiguruj klaster tak, aby korzystał z maszyn wirtualnych Gen2 dla wybranej jednostki SKU podczas tworzenia klastra. Użyj `--aks-custom-headers` flagi, aby ustawić Gen2 jako generację maszyny wirtualnej w nowym klastrze.

```azure-cli
az aks create --name myAKSCluster --resource-group myResourceGroup -s Standard_D2s_v3 --aks-custom-headers usegen2vm=true
```

Jeśli chcesz utworzyć zwykły klaster przy użyciu maszyn wirtualnych generacji 1 (Gen1), możesz to zrobić, pomijając `--aks-custom-headers` znacznik niestandardowy. Możesz również dodać więcej maszyn wirtualnych Gen1 lub Gen2, tak jak na poniższej liście.

### <a name="use-gen2-vms-on-existing-clusters-preview"></a>Korzystanie z maszyn wirtualnych Gen2 w istniejących klastrach (wersja zapoznawcza)
Skonfiguruj nową pulę węzłów do używania maszyn wirtualnych Gen2. Użyj `--aks-custom-headers` flagi, aby ustawić Gen2 jako generację maszyny wirtualnej dla tej puli węzłów.

```azure-cli
az aks nodepool add --name gen2 --cluster-name myAKSCluster --resource-group myResourceGroup -s Standard_D2s_v3 --aks-custom-headers usegen2vm=true
```

Jeśli chcesz utworzyć regularne pule węzłów Gen1, możesz to zrobić, pomijając `--aks-custom-headers` znacznik niestandardowy.

## <a name="custom-resource-group-name"></a>Nazwa niestandardowej grupy zasobów

Podczas wdrażania klastra usługi Azure Kubernetes na platformie Azure zostanie utworzona druga grupa zasobów dla węzłów procesu roboczego. Domyślnie AKS będzie nazwać grupę zasobów węzła `MC_resourcegroupname_clustername_location` , ale możesz również podać własną nazwę.

Aby określić własną nazwę grupy zasobów, zainstaluj rozszerzenie AKS-Preview interfejsu wiersza polecenia platformy Azure w wersji 0.3.2 lub nowszej. Korzystając z interfejsu wiersza polecenia platformy Azure, użyj `--node-resource-group` parametru `az aks create` polecenie, aby określić niestandardową nazwę grupy zasobów. W przypadku wdrażania klastra AKS za pomocą szablonu Azure Resource Manager można zdefiniować nazwę grupy zasobów za pomocą `nodeResourceGroup` właściwości.

```azurecli
az aks create --name myAKSCluster --resource-group myResourceGroup --node-resource-group myNodeResourceGroup
```

Dodatkowa grupa zasobów jest automatycznie tworzona przez dostawcę zasobów platformy Azure we własnej subskrypcji. Podczas tworzenia klastra można określić niestandardową nazwę grupy zasobów. 

Podczas pracy z grupą zasobów węzła należy pamiętać, że nie można:

- Określ istniejącą grupę zasobów dla grupy zasobów węzła.
- Określ inną subskrypcję dla grupy zasobów węzła.
- Zmień nazwę grupy zasobów węzła po utworzeniu klastra.
- Określ nazwy zarządzanych zasobów w grupie zasobów węzła.
- Modyfikuj lub Usuń Tagi zarządzane przez platformę Azure w ramach grupy zasobów węzła.

## <a name="next-steps"></a>Następne kroki

- Dowiedz się `Kured` , jak [stosować aktualizacje zabezpieczeń i jądra do węzłów systemu Linux](node-updates-kured.md) w klastrze.
- Zobacz [Uaktualnianie klastra usługi Azure Kubernetes Service (AKS)](upgrade-cluster.md) , aby dowiedzieć się, jak uaktualnić klaster do najnowszej wersji Kubernetes.
- Przeczytaj więcej na temat [ `containerd` i Kubernetes](https://kubernetes.io/blog/2018/05/24/kubernetes-containerd-integration-goes-ga/)
- Zapoznaj się z listą [często zadawanych pytań dotyczących AKS](faq.md) , aby znaleźć odpowiedzi na niektóre często zadawane pytania dotyczące AKS.


<!-- LINKS - internal -->
[azure-cli-install]: /cli/azure/install-azure-cli
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-provider-register]: /cli/azure/provider#az-provider-register
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-provider-register]: /cli/azure/provider#az-provider-register
