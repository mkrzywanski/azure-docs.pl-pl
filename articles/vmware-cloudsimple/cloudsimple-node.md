---
title: Rozwiązanie VMware firmy Azure według CloudSimple — Omówienie węzłów
description: Dowiedz się więcej o węzłach i pojęciach CloudSimple.
author: sharaths-cs
ms.author: dikamath
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 65afe26a98a53b00b72a1ea2b49799db2049b727
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "77024929"
---
# <a name="cloudsimple-nodes-overview"></a>Przegląd węzłów CloudSimple

Węzły są blokami konstrukcyjnymi chmury prywatnej. Węzeł:

* Dedykowany Host obliczeniowy bez systemu operacyjnego, na którym zainstalowano VMware ESXi funkcji hypervisor  
* Jednostka obliczeniowa, którą można zainicjować lub zarezerwować w celu utworzenia chmur prywatnych
* Dostępne do aprowizacji lub rezerwowania w regionie, w którym jest dostępna usługa CloudSimple

Chmurę prywatną można utworzyć na podstawie inicjowanych węzłów. Aby utworzyć chmurę prywatną, potrzebne są co najmniej trzy węzły tej samej jednostki SKU. Aby rozwinąć chmurę prywatną, Dodaj dodatkowe węzły.  Można dodać węzły do istniejącego klastra lub utworzyć nowy klaster przez węzły aprowizacji w Azure Portal i skojarzyć je z usługą CloudSimple.  Wszystkie węzły obsługiwane przez usługę CloudSimple są widoczne.  

## <a name="provisioned-nodes"></a>Węzły inicjowane

Węzły z zainicjowaną obsługą zapewniają pojemność płatność zgodnie z rzeczywistym użyciem. Węzły aprowizacji ułatwiają szybkie skalowanie klastra VMware na żądanie. Możesz dodać węzły w razie potrzeby lub usunąć węzeł aprowizacji, aby przeskalować swój klaster VMware. Na węzłach inicjowanych jest rozliczana co miesiąc i naliczana jest opłata za subskrypcję, w której są one inicjowane.

* Jeśli płacisz za subskrypcję platformy Azure za pomocą karty kredytowej, karta jest rozliczana od razu.
* Jeśli opłata jest naliczana według faktury, opłaty są naliczane na następnej fakturze.

## <a name="vmware-solution-by-cloudsimple-nodes-sku"></a>Rozwiązanie VMware według jednostki SKU węzłów CloudSimple

Następujące typy węzłów są dostępne do aprowizacji lub rezerwacji.

| SKU           | CS28 — węzeł                 | CS36 — węzeł                 | CS36m — węzeł                |
|---------------|-----------------------------|-----------------------------|-----------------------------|
| Region        | Wschodnie stany USA, zachodnie stany USA            | Wschodnie stany USA, zachodnie stany USA            | Europa Zachodnia                 |
| Procesor CPU           | 2.2,2 GHz, 28 rdzeni (56 HT) | 2 rdzenie 2,3 GHz, 36 (72 HT) | 2 rdzenie 2,3 GHz, 36 (72 HT) |
| Pamięć RAM           | 256 GB                      | 512 GB                      | 576 GB                      |
| Dysk pamięci podręcznej    | 1,6 – TB interfejsu NVMe                 | 3,2 – TB interfejsu NVMe                 | 3,2 – TB interfejsu NVMe                 |
| Dysk pojemności | 5,625 TB nieprzetworzonych                | 11,25 TB nieprzetworzonych                | 15,36 TB nieprzetworzonych                |
| Typ magazynu  | Wszystkie lampy błyskowe                   | Wszystkie lampy błyskowe                   | Wszystkie lampy błyskowe                   |

## <a name="limits"></a>Limity

Poniższe limity węzłów mają zastosowanie do chmur prywatnych.

| Zasób | Limit |
|----------|-------|
| Minimalna liczba węzłów do utworzenia chmury prywatnej | 3 |
| Maksymalna liczba węzłów w klastrze w chmurze prywatnej | 16 |
| Maksymalna liczba węzłów w chmurze prywatnej | 64 |
| Minimalna liczba węzłów w nowym klastrze | 3 |

## <a name="next-steps"></a>Następne kroki

* Dowiedz się, jak [inicjować obsługę węzłów](create-nodes.md)
* Informacje o [chmurach prywatnych](cloudsimple-private-cloud.md)
