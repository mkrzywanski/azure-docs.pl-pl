---
title: Ev4 i Esv4 — seria Virtual Machines platformy Azure
description: Specyfikacje dla maszyn wirtualnych z serii Ev4 i Esv4.
author: brbell
ms.author: brbell
ms.reviewer: cynthn
ms.custom: mimckitt
ms.service: virtual-machines
ms.subservice: sizes
ms.topic: conceptual
ms.date: 6/8/2020
ms.openlocfilehash: 28bde63bb9972b8e8de6261282007c1762fd6818
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/28/2020
ms.locfileid: "87269312"
---
# <a name="ev4-and-esv4-series"></a>Serie Ev4 i Esv4

Seria Ev4 i Esv4 jest uruchamiana na &reg; &reg; procesorach Intel Xeon Platinum 8272CL (kaskad Lake) w konfiguracji wielowątkowej, idealnie sprawdza się w przypadku różnych aplikacji przedsiębiorstwa intensywnie korzystających z pamięci oraz funkcji do 504GiB pamięci RAM.

> [!NOTE]
> W przypadku często zadawanych pytań zapoznaj się z tematem [rozmiary maszyn wirtualnych platformy Azure bez lokalnego dysku tymczasowego](azure-vms-no-temp-disk.md).

## <a name="ev4-series"></a>Seria Ev4

Rozmiary serii Ev4 są uruchamiane w technologii Intel Xeon &reg; Platinum 8272CL (Kaskada Lake). Wystąpienia serii Ev4 są idealne dla aplikacji korporacyjnych intensywnie korzystających z pamięci. Maszyny wirtualne z serii Ev4 są wyposażone w &reg; technologię wielowątkowości Intel.

Magazyn danych zdalnych jest rozliczany osobno od maszyn wirtualnych. Aby korzystać z dysków magazynu Premium Storage, użyj rozmiarów Esv4. Liczniki cen i rozliczeń dla rozmiarów Esv4 są takie same jak dla serii Ev4.

> [!IMPORTANT]
> Te nowe rozmiary są obecnie dostępne tylko w publicznej wersji zapoznawczej. W [tym miejscu](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR_Y3toRKxchLjARedqtguBRURE1ZSkdDUzg1VzJDN0cwWUlKTkcyUlo5Mi4u)możesz wyEv4ać te serie i Esv4. 

ACU: 195 – 210

Premium Storage: nieobsługiwane

Buforowanie Premium Storage: nieobsługiwane

Migracja na żywo: obsługiwane

Aktualizacje z zachowaniem pamięci: obsługiwane

| Rozmiar | Procesor wirtualny | Pamięć: GiB | Magazyn tymczasowy (SSD): GiB | Maks. liczba dysków danych | Maksymalna liczba kart sieciowych|Oczekiwana przepustowość sieci (MB/s) |
|---|---|---|---|---|---|---|
| Standard_E2_v4  | 2 | 16   | Tylko Magazyn zdalny | 4 | 2|1000  |
| Standard_E4_v4  | 4 | 32  | Tylko Magazyn zdalny | 8 | 2|2000  |
| Standard_E8_v4  | 8 | 64 | Tylko Magazyn zdalny | 16 | 4|4000 |
| Standard_E16_v4 | 16 | 128 | Tylko Magazyn zdalny | 32 | 8|8000 |
| Standard_E20_v4 | 20 | 160 | Tylko Magazyn zdalny | 32 | 8|10 000 |
| Standard_E32_v4 | 32 | 256 | Tylko Magazyn zdalny | 32 | 8|16000 |
| Standard_E48_v4 | 48 | 384 | Tylko Magazyn zdalny | 32 | 8|24000 |
| Standard_E64_v4 | 64 | 504 | Tylko Magazyn zdalny | 32| 8|30000 |


## <a name="esv4-series"></a>Seria Esv4

Rozmiary serii Esv4 są uruchamiane w technologii Intel &reg; Xeon &reg; Platinum 8272CL (Kaskada Lake). Wystąpienia serii Esv4 są idealne dla aplikacji korporacyjnych intensywnie korzystających z pamięci. Maszyny wirtualne z serii Evs4 są wyposażone w &reg; technologię wielowątkowości Intel. Magazyn danych zdalnych jest rozliczany osobno od maszyn wirtualnych.

> [!IMPORTANT]
> Te nowe rozmiary są obecnie dostępne tylko w publicznej wersji zapoznawczej. W [tym miejscu](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR_Y3toRKxchLjARedqtguBRURE1ZSkdDUzg1VzJDN0cwWUlKTkcyUlo5Mi4u)możesz wyEv4ać te serie i Esv4. 

ACU: 195-210

Premium Storage: obsługiwane

Buforowanie Premium Storage: obsługiwane

Migracja na żywo: obsługiwane

Aktualizacje z zachowaniem pamięci: obsługiwane

| Rozmiar | Procesor wirtualny | Pamięć: GiB | Magazyn tymczasowy (SSD): GiB | Maks. liczba dysków danych | Maksymalna przepływność pamięci podręcznej: IOPS/MB/s (rozmiar pamięci podręcznej w GiB) | Maksymalna przepływność dysku w pamięci podręcznej: liczba operacji we/wy na sekundę | Maksymalna liczba kart sieciowych|Oczekiwana przepustowość sieci (MB/s) |
|---|---|---|---|---|---|---|---|---|
| Standard_E2s_v4  | 2 | 16  | Tylko Magazyn zdalny | 4 | 19000/120 (50) | 3200/48 | 2|1000  |
| Standard_E4s_v4  | 4 | 32  | Tylko Magazyn zdalny | 8 | 38500/242 (100) | 6400/96 | 2|2000  |
| Standard_E8s_v4  | 8 | 64  | Tylko Magazyn zdalny | 16 | 77000/485 (200) | 12800/192 | 4|4000 |
| Standard_E16s_v4 | 16 | 128 | Tylko Magazyn zdalny | 32 | 154000/968 (400) | 25600/384 | 8|8000 |
| Standard_E20s_v4 | 20 | 160 | Tylko Magazyn zdalny | 32 | 193000/1211 (500) | 32000/480  | 8|10 000 |
| Standard_E32s_v4 | 32 | 256 | Tylko Magazyn zdalny | 32 | 308000/1936 (800) | 51200/768  | 8|16000 |
| Standard_E48s_v4 | 48 | 384 | Tylko Magazyn zdalny | 32 | 462000/2904 (1200) | 76800/1152 | 8|24000 |
| Standard_E64s_v4 <sup>1</sup> | 64 | 504| Tylko Magazyn zdalny | 32 | 615000/3872 (1600) | 80000/1200 | 8|30000 |

dostępne są <sup>1</sup> [ograniczone rozmiary rdzeni](./windows/constrained-vcpu.md).

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
