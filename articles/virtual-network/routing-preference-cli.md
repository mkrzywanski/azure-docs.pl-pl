---
title: Konfigurowanie preferencji routingu dla publicznego adresu IP przy użyciu interfejsu wiersza polecenia platformy Azure
titlesuffix: Azure Virtual Network
description: Dowiedz się, jak utworzyć publiczny adres IP z preferencją routingu ruchu internetowego
services: virtual-network
documentationcenter: na
author: KumudD
manager: mtillman
ms.service: virtual-network
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/18/2020
ms.author: mnayak
ms.custom: devx-track-azurecli
ms.openlocfilehash: 64284b198fc76c219ffe0dfbc57461b587b23130
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/31/2020
ms.locfileid: "87504606"
---
# <a name="configure-routing-preference-for-a-public-ip-address-using-azure-cli"></a>Konfigurowanie preferencji routingu dla publicznego adresu IP przy użyciu interfejsu wiersza polecenia platformy Azure

W tym artykule opisano sposób konfigurowania preferencji routingu za pośrednictwem sieci usługodawcy internetowego (opcja**internetowa** ) dla publicznego adresu IP przy użyciu interfejsu wiersza polecenia platformy Azure. Po utworzeniu publicznego adresu IP możesz skojarzyć go z poniższymi zasobami platformy Azure dla ruchu przychodzącego i wychodzącego do Internetu:

* Maszyna wirtualna
* Zestaw skalowania maszyn wirtualnych
* Azure Kubernetes Service (AKS)
* Moduł równoważenia obciążenia dostępny z Internetu
* Application Gateway
* Azure Firewall

Domyślnie preferencja routingu dla publicznego adresu IP jest ustawiana na sieć globalna firmy Microsoft dla wszystkich usług platformy Azure i może być skojarzona z dowolną usługą platformy Azure.

> [!IMPORTANT]
> Preferencje routingu są obecnie dostępne w publicznej wersji zapoznawczej.
> Ta wersja zapoznawcza nie jest objęta umową dotyczącą poziomu usług i nie zalecamy korzystania z niej w przypadku obciążeń produkcyjnych. Niektóre funkcje mogą być nieobsługiwane lub ograniczone. Aby uzyskać więcej informacji, zobacz [Uzupełniające warunki korzystania z wersji zapoznawczych platformy Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Jeśli nie masz subskrypcji platformy Azure, utwórz teraz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]
Jeśli zdecydujesz się zainstalować interfejs wiersza polecenia platformy Azure i korzystać z niego lokalnie, ten przewodnik Szybki Start wymaga użycia interfejsu wiersza polecenia platformy Azure w wersji 2.0.49 lub nowszej. Aby dowiedzieć się, jaka wersja jest zainstalowana, uruchom polecenie `az --version`. Aby uzyskać informacje na temat instalacji i uaktualnienia, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure](/cli/azure/install-azure-cli).

## <a name="register-the-feature-for-your-subscription"></a>Rejestrowanie funkcji dla subskrypcji
Funkcja preferencji routingu jest obecnie w wersji zapoznawczej. Zarejestruj funkcję subskrypcji w następujący sposób:
```azurecli
az feature register --namespace Microsoft.Network --name AllowRoutingPreferenceFeature
```

## <a name="create-a-resource-group"></a>Tworzenie grupy zasobów
Utwórz grupę zasobów za pomocą polecenia [az group create](/cli/azure/group#az-group-create). Poniższy przykład tworzy grupę zasobów w regionie platformy Azure **Wschodnie stany USA** :

```azurecli
  az group create --name myResourceGroup --location eastus
```
## <a name="create-a-public-ip-address"></a>Tworzenie publicznego adresu IP

Utwórz publiczny adres IP z preferencją routingu typu "Internet" przy użyciu polecenia [AZ Network Public-IP Create](/cli/azure/network/public-ip?view=azure-cli-latest#az-network-public-ip-create)w formacie, jak pokazano poniżej.

Następujące polecenie tworzy nowy publiczny adres IP z preferencją routingu **internetowego** w regionie platformy Azure **Wschodnie stany USA** .

```azurecli
az network public-ip create \
--name MyRoutingPrefIP \
--resource-group MyResourceGroup \
--location eastus \
--ip-tags 'RoutingPreference=Internet' \
--sku STANDARD \
--allocation-method static \
--version IPv4
```

> [!NOTE]
>  Obecnie preferencje routingu obsługują tylko publiczne adresy IP w protokole IPV4.

Powyższy utworzony publiczny adres IP można skojarzyć z maszyną wirtualną z [systemem Windows](../virtual-machines/windows/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) lub [Linux](../virtual-machines/linux/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) . Użyj sekcji interfejsu wiersza polecenia na stronie samouczka: [Skojarz publiczny adres IP z maszyną wirtualną](associate-public-ip-address-vm.md#azure-cli) w celu skojarzenia publicznego adresu IP z maszyną wirtualną. Możesz również skojarzyć publiczny adres IP utworzony powyżej z programem przy użyciu [Azure Load Balancer](../load-balancer/load-balancer-overview.md), przypisując go do konfiguracji **frontonu** modułu równoważenia obciążenia. Publiczny adres IP służy jako wirtualny adres IP (VIP) o zrównoważonym obciążeniu.

## <a name="next-steps"></a>Następne kroki

- Dowiedz się więcej o [preferencjach routingu w publicznych adresach IP](routing-preference-overview.md). 
- [Skonfiguruj preferencję routingu dla maszyny wirtualnej przy użyciu interfejsu wiersza polecenia platformy Azure](configure-routing-preference-virtual-machine-cli.md).

