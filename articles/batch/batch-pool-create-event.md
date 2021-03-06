---
title: Zdarzenie tworzenia puli Azure Batch
description: Odwołanie do zdarzenia tworzenia puli usługi Batch, które jest emitowane po utworzeniu puli. Zawartość dziennika spowoduje udostępnienie ogólnych informacji o puli.
ms.topic: reference
ms.date: 04/20/2017
ms.openlocfilehash: eee512bbeed223269c43bde77435fbff2b67b533
ms.sourcegitcommit: 5cace04239f5efef4c1eed78144191a8b7d7fee8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/08/2020
ms.locfileid: "86147319"
---
# <a name="pool-create-event"></a>Zdarzenie utworzenia puli

 To zdarzenie jest emitowane po utworzeniu puli. Zawartość dziennika spowoduje udostępnienie ogólnych informacji o puli. Należy pamiętać, że jeśli rozmiar docelowy puli ma więcej niż 0 węzłów obliczeniowych, zdarzenie rozpoczęcia zmiany rozmiaru puli będzie podążać bezpośrednio po tym zdarzeniu.

 Poniższy przykład przedstawia treść puli Utwórz zdarzenie dla puli utworzonej przy użyciu `CloudServiceConfiguration` właściwości.

```
{
    "id": "myPool1",
    "displayName": "Production Pool",
    "vmSize": "Standard_F1s",
    "imageType": "VirtualMachineConfiguration",
    "cloudServiceConfiguration": {
        "osFamily": "3",
        "targetOsVersion": "*"
    },
    "networkConfiguration": {
        "subnetId": " "
    },
    "virtualMachineConfiguration": {
          "imageReference": {
            "publisher": " ",
            "offer": " ",
            "sku": " ",
            "version": " "
          },
          "nodeAgentId": " "
        },
    "resizeTimeout": "300000",
    "targetDedicatedNodes": 2,
    "targetLowPriorityNodes": 2,
    "maxTasksPerNode": 1,
    "vmFillType": "Spread",
    "enableAutoScale": false,
    "enableInterNodeCommunication": false,
    "isAutoPool": false
}
```

|Element|Typ|Uwagi|
|-------------|----------|-----------|
|`id`|String|Identyfikator puli.|
|`displayName`|String|Nazwa wyświetlana puli.|
|`vmSize`|String|Rozmiar maszyn wirtualnych w puli. Wszystkie maszyny wirtualne w puli mają ten sam rozmiar. <br/><br/> Aby uzyskać informacje o dostępnych rozmiarach maszyn wirtualnych dla pul Cloud Services (pule utworzone za pomocą cloudServiceConfiguration), zobacz [rozmiary Cloud Services](../cloud-services/cloud-services-sizes-specs.md). Program Batch obsługuje wszystkie Cloud Services rozmiary maszyn wirtualnych z wyjątkiem `ExtraSmall` .<br/><br/> Aby uzyskać informacje o dostępnych rozmiarach maszyn wirtualnych dla pul przy użyciu obrazów z witryny Virtual Machines Marketplace (pule utworzone za pomocą virtualMachineConfiguration), zobacz [rozmiary Virtual Machines](../virtual-machines/linux/sizes.md?toc=%2Fazure%2Fvirtual-machines%2Flinux%2Ftoc.json) (Linux) lub [rozmiary dla Virtual Machines](../virtual-machines/windows/sizes.md?toc=%2Fazure%2Fvirtual-machines%2Fwindows%2Ftoc.json) (Windows). Usługa Batch obsługuje wszystkie rozmiary maszyn wirtualnych platformy Azure oprócz `STANDARD_A0` i maszyn z usługi Premium Storage (seria `STANDARD_GS`, `STANDARD_DS` i `STANDARD_DSV2`).|
|`imageType`|String|Metoda wdrożenia obrazu. Obsługiwane wartości to `virtualMachineConfiguration` lub`cloudServiceConfiguration`|
|[`cloudServiceConfiguration`](#bk_csconf)|Typ złożony|Konfiguracja usługi w chmurze dla puli.|
|[`virtualMachineConfiguration`](#bk_vmconf)|Typ złożony|Konfiguracja maszyny wirtualnej dla puli.|
|[`networkConfiguration`](#bk_netconf)|Typ złożony|Konfiguracja sieci dla puli.|
|`resizeTimeout`|Godzina|Limit czasu przydziału węzłów obliczeniowych do puli określonej dla operacji ostatniej zmiany rozmiaru w puli.  (Początkowe rozmiary podczas tworzenia puli jest traktowane jako zmiana rozmiaru).|
|`targetDedicatedNodes`|Int32|Liczba dedykowanych węzłów obliczeniowych, które są żądane dla puli.|
|`targetLowPriorityNodes`|Int32|Liczba węzłów obliczeniowych o niskim priorytecie, które są żądane dla puli.|
|`enableAutoScale`|Wartość logiczna|Określa, czy rozmiar puli automatycznie dostosowuje się w miarę upływu czasu.|
|`enableInterNodeCommunication`|Wartość logiczna|Określa, czy pula jest skonfigurowana do bezpośredniej komunikacji między węzłami.|
|`isAutoPool`|Wartość logiczna|Określa, czy pula została utworzona za pomocą mechanizmu autopuli zadań.|
|`maxTasksPerNode`|Int32|Maksymalna liczba zadań, które można uruchamiać współbieżnie w jednym węźle obliczeniowym w puli.|
|`vmFillType`|String|Definiuje, w jaki sposób usługa Batch dystrybuuje zadania między węzłami obliczeniowymi w puli. Prawidłowe wartości to rozmieszczenie lub pakiet.|

###  <a name="cloudserviceconfiguration"></a><a name="bk_csconf"></a>cloudServiceConfiguration

|Nazwa elementu|Typ|Uwagi|
|------------------|----------|-----------|
|`osFamily`|String|Rodzina systemów operacyjnych gościa platformy Azure do zainstalowania na maszynach wirtualnych w puli.<br /><br /> Możliwe wartości:<br /><br /> **2** — Rodzina systemów operacyjnych 2, równoważna z systemem Windows Server 2008 R2 z dodatkiem SP1.<br /><br /> **3** — Rodzina systemów operacyjnych 3, równoważna z systemem Windows Server 2012.<br /><br /> **4** — Rodzina systemów operacyjnych 4, równoważna z systemem Windows Server 2012 R2.<br /><br /> Aby uzyskać więcej informacji, zobacz [wersje systemu operacyjnego gościa platformy Azure](../cloud-services/cloud-services-guestos-update-matrix.md#releases).|
|`targetOSVersion`|String|Wersja systemu operacyjnego gościa platformy Azure do zainstalowania na maszynach wirtualnych w puli.<br /><br /> Wartość domyślna to **\*** określa najnowszą wersję systemu operacyjnego dla określonej rodziny.<br /><br /> Aby uzyskać inne dozwolone wartości, zobacz [wersje systemu operacyjnego gościa platformy Azure](../cloud-services/cloud-services-guestos-update-matrix.md#releases).|

###  <a name="virtualmachineconfiguration"></a><a name="bk_vmconf"></a>virtualMachineConfiguration

|Nazwa elementu|Typ|Uwagi|
|------------------|----------|-----------|
|[`imageReference`](#bk_imgref)|Typ złożony|Określa informacje o platformie lub obrazie witryny Marketplace do użycia.|
|`nodeAgentId`|String|Jednostka SKU agenta węzła partii obsługiwana w węźle obliczeniowym.|
|[`windowsConfiguration`](#bk_winconf)|Typ złożony|Określa ustawienia systemu operacyjnego Windows na maszynie wirtualnej. Ta właściwość nie może być określona, jeśli elementu imagereference odwołuje się do obrazu systemu operacyjnego Linux.|

###  <a name="imagereference"></a><a name="bk_imgref"></a>Elementu imagereference

|Nazwa elementu|Typ|Uwagi|
|------------------|----------|-----------|
|`publisher`|String|Wydawca obrazu.|
|`offer`|String|Oferta obrazu.|
|`sku`|String|Jednostka SKU obrazu.|
|`version`|String|Wersja obrazu.|

###  <a name="windowsconfiguration"></a><a name="bk_winconf"></a>windowsConfiguration

|Nazwa elementu|Typ|Uwagi|
|------------------|----------|-----------|
|`enableAutomaticUpdates`|Boolean|Wskazuje, czy dla maszyny wirtualnej włączono automatyczne aktualizacje. Jeśli ta właściwość nie jest określona, wartość domyślna to true.|

###  <a name="networkconfiguration"></a><a name="bk_netconf"></a>networkConfiguration

|Nazwa elementu|Typ|Uwagi|
|------------------|--------------|----------|
|`subnetId`|String|Określa identyfikator zasobu podsieci, w którym są tworzone węzły obliczeniowe puli.|
