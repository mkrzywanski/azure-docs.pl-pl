---
title: Mv2 — seria Virtual Machines platformy Azure
description: Specyfikacje dotyczące maszyn wirtualnych z serii Mv2.
author: ayshakeen
ms.service: virtual-machines
ms.subservice: sizes
ms.topic: conceptual
ms.date: 04/07/2020
ms.author: jushiman
ms.openlocfilehash: 74d8c4dc9f6b8f1a409f5f6766bea57b23b15ba7
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/28/2020
ms.locfileid: "87284799"
---
# <a name="mv2-series"></a>Seria Mv2

Seria Mv2 oferuje wysoką przepływność, platformę o małym opóźnieniu działającą na procesorze Intel® Xeon® Platinum 8180M 2,5 GHz (Skylake) z podstawową częstotliwością 2,5 GHz i maksymalną częstotliwością Turbo 3,8 GHz. Wszystkie rozmiary maszyn wirtualnych z serii Mv2 mogą korzystać z dysków trwałych w warstwach Standardowa i Premium. Wystąpienia serii Mv2 to rozmiary maszyn wirtualnych zoptymalizowane pod kątem pamięci, które zapewniają niezrównaną wydajność obliczeniową w celu obsługi dużych baz danych i obciążeń w pamięci, dzięki czemu można uzyskać wysoką ilość pamięci, która jest idealnym rozwiązaniem w przypadku serwerów relacyjnych baz danych, dużych pamięci podręcznych i analizy w pamięci.

Funkcja maszyny wirtualnej z serii Mv2 Intel® technologia wielowątkowości funkcji Hyper-Threading

Premium Storage: obsługiwane

Buforowanie Premium Storage: obsługiwane

Migracja na żywo: nieobsługiwane

Aktualizacje z zachowaniem pamięci: nieobsługiwane

Akcelerator zapisu: [obsługiwane](./windows/how-to-enable-write-accelerator.md)

|Rozmiar | Procesor wirtualny | Pamięć: GiB | Magazyn tymczasowy (SSD): GiB | Maks. liczba dysków danych | Maksymalna przepływność magazynu buforowanego i tymczasowego: liczba operacji we/wy na sekundę / MB/s (rozmiar pamięci podręcznej w GiB) | Maksymalna przepływność niebuforowanych dysków: liczba operacji we/wy na sekundę / MB/s | Maksymalna liczba kart sieciowych | Oczekiwana przepustowość sieci (MB/s) |
|---|---|---|---|---|---|---|---|---|
| Standard_M208ms_v2<sup>1</sup> | 208 | 5700 | 4096 | 64 | 80000/800 (7040) | 40000/1000 | 8 | 16000 |
| Standard_M208s_v2<sup>1</sup> | 208 | 2850 | 4096 | 64 | 80000/800 (7040) | 40000/1000 | 8 | 16000 |
| Standard_M416ms_v2<sup>1</sup> | 416 | 11400 | 8192 | 64 | 250000/1600 (14080) | 80000/2000 | 8 | 32000 |
| Standard_M416s_v2<sup>1</sup> | 416 | 5700 | 8192 | 64 | 250000/1600 (14080) | 80000/2000 | 8 | 32000 |

<sup>1</sup> maszyny wirtualne z serii Mv2 są tylko 2 generacji i obsługują podzbiór obrazów obsługiwanych przez generację 2. Zapoznaj się z pełną listą obsługiwanych obrazów dla serii Mv2. Jeśli używasz systemu Linux, zobacz [Obsługa maszyn wirtualnych 2. generacji na platformie Azure](./linux/generation-2.md) , aby uzyskać instrukcje dotyczące znajdowania i wybierania obrazu. Jeśli używasz systemu Windows, zapoznaj się z tematem [Obsługa maszyn wirtualnych 2. generacji na platformie Azure](./windows/generation-2.md) , aby uzyskać instrukcje dotyczące znajdowania i wybierania obrazu. 

- System Windows Server 2019 lub nowszy
- SUSE Linux Enterprise Server 12 z dodatkiem SP4 lub nowszym lub SUSE Linux Enterprise Server 15 SP1 i nowsze
- Red Hat Enterprise Linux 7,6, 7,7, 8,1 lub nowszy 
- Oracle Enterprise Linux 7,7 lub nowszy



[!INCLUDE [virtual-machines-common-sizes-table-defs](../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="other-sizes-and-information"></a>Inne rozmiary i informacje

- [Ogólnego przeznaczenia](sizes-general.md)
- [Optymalizacja pod kątem pamięci](sizes-memory.md)
- [Optymalizacja pod kątem magazynu](sizes-storage.md)
- [Optymalizacja pod kątem procesora GPU](sizes-gpu.md)
- [Obliczenia o wysokiej wydajności](sizes-hpc.md)
- [Poprzednie generacje](sizes-previous-gen.md)

Kalkulator cen: [Kalkulator cen](https://azure.microsoft.com/pricing/calculator/)

Więcej informacji na temat typów dysków: [typy dysków](https://docs.microsoft.com/azure/virtual-machines/linux/disks-types#ultra-ssd-preview/)


## <a name="next-steps"></a>Następne kroki

Dowiedz się więcej o tym, jak [usługa Azure COMPUTE units (ACU)](acu.md) może pomóc w porównaniu wydajności obliczeniowej w ramach jednostek SKU platformy Azure.
