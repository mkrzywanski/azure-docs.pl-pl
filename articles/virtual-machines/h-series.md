---
title: Seria H Virtual Machines platformy Azure
description: Specyfikacje dla maszyn wirtualnych z serii H.
author: ju-shim
ms.service: virtual-machines
ms.subservice: sizes
ms.topic: conceptual
ms.date: 08/01/2020
ms.author: amverma
ms.reviewer: jushiman
ms.openlocfilehash: 797a036b9cf2e77dfbcdf8dc7596179c4673e6a6
ms.sourcegitcommit: 29400316f0c221a43aff3962d591629f0757e780
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/02/2020
ms.locfileid: "87513744"
---
# <a name="h-series"></a>Seria H

Maszyny wirtualne z serii H są zoptymalizowane pod kątem aplikacji z wysoką częstotliwością procesora CPU lub dużą ilością pamięci na podstawowe wymagania. Maszyny wirtualne serii H funkcja 8 lub 16 rdzeni procesora Intel Xeon E5 2667 v3, do 14 GB pamięci RAM na rdzeń procesora CPU i bez wielowątkowości. Funkcja serii H 56 GB/s karta Mellanox FDR InfiniBand w nieblokującej konfiguracji drzewa FAT dla spójnej wydajności RDMA. Maszyny wirtualne z serii H obsługują technologię Intel MPI 5. x i MS-MPI.

ACU: 290–300

Premium Storage: nieobsługiwane

Buforowanie Premium Storage: nieobsługiwane

Migracja na żywo: nieobsługiwane

Aktualizacje z zachowaniem pamięci: nieobsługiwane

| Rozmiar | Procesor wirtualny | Procesor | Pamięć (GB) | Przepustowość pamięci GB/s | Podstawowa częstotliwość procesora CPU (GHz) | Częstotliwość wszystkich rdzeni (GHz, szczyt) | Częstotliwość jednordzeniowa (GHz, szczytowa) | Wydajność RDMA (GB/s) | Obsługa MPI | Magazyn tymczasowy (GB) | Maks. liczba dysków danych | Maksymalna przepływność dysków: liczba operacji we/wy na sekundę | Maksymalna liczba kart sieciowych Ethernet |
| --- | --- |--- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Standardowa_H8   | 8  | Intel Xeon E5 2667 v3 | 56 | 40 | 3.2 | 3.3 | 3,6 | - | Intel 5. x, MS-MPI | 1000 | 32 | 32 x 500 | 2 |
| Standardowa_H16  | 16 | Intel Xeon E5 2667 v3 | 112 | 80 | 3.2 | 3.3 | 3,6 | - | Intel 5. x, MS-MPI | 2000 | 64 | 64 x 500 | 4 |
| Standardowa_H8m  | 8  | Intel Xeon E5 2667 v3 | 112 | 40 | 3.2 | 3.3 | 3,6 | - | Intel 5. x, MS-MPI | 1000 | 32 | 32 x 500 | 2 |
| Standardowa_H16m | 16 | Intel Xeon E5 2667 v3 | 224 | 80 | 3.2 | 3.3 | 3,6 | - | Intel 5. x, MS-MPI | 2000 | 64 | 64 x 500 | 4 |
| Standard_H16r <sup>1</sup>  | 16 | Intel Xeon E5 2667 v3 | 112 | 80 | 3.2 | 3.3 | 3,6 | 56 | Intel 5. x, MS-MPI | 2000 | 64 | 64 x 500 | 4 |
| Standard_H16mr <sup>1</sup> | 16 | Intel Xeon E5 2667 v3 | 224 | 80 | 3.2 | 3.3 | 3,6 | 56 | Intel 5. x, MS-MPI | 2000 | 64 | 64 x 500 | 4 |

<sup>1</sup> w przypadku aplikacji MPI dedykowana sieć zaplecza RDMA jest włączona przez sieć FDR InfiniBand.

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../includes/virtual-machines-common-sizes-table-defs.md)]


## <a name="supported-os-images-linux"></a>Obsługiwane obrazy systemu operacyjnego (Linux)
 
Portal Azure Marketplace ma wiele dystrybucji systemu Linux obsługujących łączność RDMA:
  
* **CentOS HPC** — dla maszyn wirtualnych z obsługą wirtualizacji SR-IOV, CentOS w wersji 6,5 HPC lub nowszej, do 7,5 są odpowiednie. W przypadku maszyn wirtualnych z serii H zaleca się używanie wersji 7,1 do 7,5. Sterowniki RDMA i Intel MPI 5,1 są zainstalowane na maszynie wirtualnej.
  W przypadku maszyn wirtualnych SR-IOV CentOS-HPC 7,6 jest zoptymalizowane i wstępnie załadowane ze sterownikami RDMA i zainstalowanymi różnymi pakietami MPI.
  W przypadku innych obrazów maszyn wirtualnych RHEL/CentOS Dodaj rozszerzenie InfiniBandLinux, aby włączyć funkcję InfiniBand. To rozszerzenie maszyny wirtualnej z systemem Linux instaluje sterowniki Mellanox OFED (na maszynach wirtualnych SR-IOV) na potrzeby łączności RDMA. Następujące polecenie cmdlet programu PowerShell instaluje najnowszą wersję (w wersji 1,0) rozszerzenia InfiniBandDriverLinux na istniejącej maszynie wirtualnej z obsługą funkcji RDMA. Maszyna wirtualna z obsługą funkcji RDMA ma nazwę *myVM* i jest wdrażana w grupie zasobów o nazwie Moja *zasobów* w regionie *zachodnie stany USA* w następujący sposób:

  ```powershell
  Set-AzVMExtension -ResourceGroupName "myResourceGroup" -Location "westus" -VMName "myVM" -ExtensionName "InfiniBandDriverLinux" -Publisher "Microsoft.HpcCompute" -Type "InfiniBandDriverLinux" -TypeHandlerVersion "1.0"
  ```
  Alternatywnie rozszerzenia maszyn wirtualnych można dołączać do szablonów Azure Resource Manager, aby ułatwić wdrażanie za pomocą następującego elementu JSON:
  ```json
  "properties":{
  "publisher": "Microsoft.HpcCompute",
  "type": "InfiniBandDriverLinux",
  "typeHandlerVersion": "1.0",
  } 
  ```
  
  Poniższe polecenie instaluje najnowszą wersję 1,0 rozszerzenia InfiniBandDriverLinux na wszystkich maszynach wirtualnych z obsługą funkcji RDMA w istniejącym zestawie skalowania maszyn wirtualnych o nazwie *myVMSS* wdrożonych w grupie zasobów o nazwie Moja *zasobów*:
  ```powershell
  $VMSS = Get-AzVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myVMSS"
  Add-AzVmssExtension -VirtualMachineScaleSet $VMSS -Name "InfiniBandDriverLinux" -Publisher "Microsoft.HpcCompute" -Type "InfiniBandDriverLinux" -TypeHandlerVersion "1.0"
  Update-AzVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "MyVMSS" -VirtualMachineScaleSet $VMSS
  Update-AzVmssInstance -ResourceGroupName "myResourceGroup" -VMScaleSetName "myVMSS" -InstanceId "*"
  ```
  
  > [!NOTE]
  > W obrazach HPC opartych na CentOS aktualizacje jądra są wyłączone w pliku konfiguracyjnym **yum** . Wynika to z faktu, że sterowniki RDMA systemu Linux są dystrybuowane jako pakiet RPM, a aktualizacje sterowników mogą nie zadziałać, jeśli jądro zostało zaktualizowane.
  >
  

* **SUSE Linux Enterprise Server** -SLES 12 SP3 dla HPC, SLES 12 SP3 dla HPC (Premium), SLES 12 SP1 dla HPC, SLES 12 SP1 dla HPC (Premium), SLES 12 SP4 i SLES 15. Sterowniki RDMA są zainstalowane i pakiety Intel MPI są dystrybuowane na maszynie wirtualnej. Zainstaluj program MPI, uruchamiając następujące polecenie:

  ```bash
  sudo rpm -v -i --nodeps /opt/intelMPI/intel_mpi_packages/*.rpm
  ```
  
* **Ubuntu** -Ubuntu Server 16,04 LTS, 18,04 LTS. Skonfiguruj sterowniki RDMA na maszynie wirtualnej i zarejestruj się w firmie Intel, aby pobrać firmę Intel MPI:

  [!INCLUDE [virtual-machines-common-ubuntu-rdma](../../includes/virtual-machines-common-ubuntu-rdma.md)]  

  Aby uzyskać więcej informacji na temat włączania funkcji InfiniBand, konfigurowania MPI, zobacz [enable InfiniBand](./workloads/hpc/enable-infiniband.md).

## <a name="other-sizes"></a>Inne rozmiary

- [Ogólnego przeznaczenia](sizes-general.md)
- [Optymalizacja pod kątem pamięci](sizes-memory.md)
- [Optymalizacja pod kątem magazynu](sizes-storage.md)
- [Optymalizacja pod kątem procesora GPU](sizes-gpu.md)
- [Obliczenia o wysokiej wydajności](sizes-hpc.md)
- [Poprzednie generacje](sizes-previous-gen.md)

## <a name="next-steps"></a>Następne kroki

- Dowiedz się więcej na temat optymalizowania aplikacji HPC na platformie Azure i niektórych przykładów w [obciążeniach HPC](./workloads/hpc/overview.md).
- Przeczytaj o najnowszych anonsach i niektórych przykładach HPC oraz wyniki na [blogach społecznościowych usługi Azure COMPUTE](https://techcommunity.microsoft.com/t5/azure-compute/bg-p/AzureCompute).
- Aby zapoznać się z widokiem architektury w przypadku uruchamiania obciążeń HPC, zobacz [wysoka wydajność obliczeń (HPC) na platformie Azure](/azure/architecture/topics/high-performance-computing/).
- Dowiedz się więcej o tym, jak [usługa Azure COMPUTE units (ACU)](acu.md) może pomóc w porównaniu wydajności obliczeniowej w ramach jednostek SKU platformy Azure.
