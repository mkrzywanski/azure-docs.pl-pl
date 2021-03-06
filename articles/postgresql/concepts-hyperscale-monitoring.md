---
title: Monitorowanie i dostrajanie-skalowanie (Citus) — Azure Database for PostgreSQL
description: W tym artykule opisano funkcje monitorowania i dostrajania w Azure Database for PostgreSQL-Citus
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: fb24100eaebc54b98adf2f09fa3e557e4d6fa0be
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "82580659"
---
# <a name="monitor-and-tune-azure-database-for-postgresql---hyperscale-citus"></a>Monitorowanie i dostrajanie Azure Database for PostgreSQL-ze skalowaniem (Citus)

Monitorowanie danych dotyczących serwerów ułatwia rozwiązywanie problemów i optymalizację w obciążeniu. Funkcja Citus) oferuje różne opcje monitorowania, które umożliwiają wgląd w zachowanie węzłów w grupie serwerów.

## <a name="metrics"></a>Metryki

Funkcja Citus) udostępnia metryki dla każdego węzła w grupie serwerów. Metryki zapewniają wgląd w zachowanie obsługi zasobów. Każda Metryka jest emitowana z częstotliwością jednej minuty i ma do 30 dni historii.

Oprócz wyświetlania wykresów metryk można skonfigurować alerty. Aby uzyskać wskazówki krok po kroku, zobacz [jak skonfigurować alerty](howto-hyperscale-alert-on-metric.md).  Inne zadania obejmują Konfigurowanie zautomatyzowanych akcji, uruchamianie zaawansowanej analizy i archiwizowanie historii. Aby uzyskać więcej informacji, zobacz [Omówienie usługi Azure Metrics](../monitoring-and-diagnostics/monitoring-overview-metrics.md).

### <a name="list-of-metrics"></a>Lista metryk

Te metryki są dostępne dla węzłów ze skalowaniem (Citus):

|Metric|Nazwa wyświetlana metryki|Jednostka|Opis|
|---|---|---|---|
|active_connections|Aktywne połączenia|Liczba|Liczba aktywnych połączeń z serwerem.|
|cpu_percent|Procent użycia procesora CPU|Procent|Procent użycia procesora CPU.|
|Wejścia|Liczba operacji we/wy na sekundę|Liczba|Zobacz [definicję IOPS](../virtual-machines/linux/premium-storage-performance.md#iops) i [przepływność w ramach skalowania](concepts-hyperscale-configuration-options.md)|
|memory_percent|Procent pamięci|Procent|Procent używanej pamięci.|
|network_bytes_ingress|Sieć — wejście|Bajty|Sieć w ramach aktywnych połączeń.|
|network_bytes_egress|Sieć — wyjście|Bajty|Nawiązywanie połączeń sieciowych między aktywnymi połączeniami.|
|storage_percent|Procent miejsca do magazynowania|Procent|Wartość procentowa używanej przestrzeni dyskowej poza maksymalną.|
|storage_used|Używany magazyn|Bajty|Ilość używanej pamięci masowej. Magazyn używany przez usługę może obejmować pliki bazy danych, dzienniki transakcji i Dzienniki serwera.|

Platforma Azure nie udostępnia zagregowanych metryk dla klastra jako całości, ale metryki dla wielu węzłów można umieścić na tym samym grafie.

## <a name="next-steps"></a>Następne kroki

- Zobacz [jak skonfigurować alerty](howto-hyperscale-alert-on-metric.md) , aby uzyskać wskazówki dotyczące tworzenia alertu dotyczącego metryki.
