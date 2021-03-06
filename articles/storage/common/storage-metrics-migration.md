---
title: Przenieś z metryk analityka magazynu, aby Azure Monitor metryki | Microsoft Docs
description: Dowiedz się, jak przechodzić z metryk analityka magazynu (metryki klasyczne) do metryk w Azure Monitor.
author: normesta
ms.service: storage
ms.topic: conceptual
ms.date: 07/28/2020
ms.author: normesta
ms.reviewer: fryu
ms.subservice: common
ms.custom: monitoring
ms.openlocfilehash: a1f977cef614a52853407c0d0665399f1a249c53
ms.sourcegitcommit: e71da24cc108efc2c194007f976f74dd596ab013
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/29/2020
ms.locfileid: "87422066"
---
# <a name="transition-to-metrics-in-azure-monitor"></a>Przejście do metryk w Azure Monitor

Usługa Azure Storage integruje teraz metryki z platformą Azure Monitor. Ten artykuł pomaga w wprowadzeniu przejścia.

## <a name="steps-to-complete-the-transition"></a>Procedura kończenia przejścia

Aby przejść do metryk w Azure Monitor, zalecamy następujące podejście.

1. Zapoznaj się z [najważniejszymi różnicami](#key-differences-between-classic-metrics-and-metrics-in-azure-monitor) między klasycznymi metrykami i metrykami w Azure monitor. 

2. Kompiluj listę klasycznych metryk, które są obecnie używane.

3. Określ, [które metryki w Azure monitor](#metrics-mapping-between-old-metrics-and-new-metrics) zawierają te same dane, co aktualnie używane metryki. 
   
4. Utwórz [wykresy](https://docs.microsoft.com/learn/modules/gather-metrics-blob-storage/2-viewing-blob-metrics-in-azure-portal) lub [pulpity nawigacyjne](https://docs.microsoft.com/learn/modules/gather-metrics-blob-storage/4-using-dashboards-in-the-azure-portal) , aby wyświetlić dane metryk.

   > [!NOTE]
   > Metryki w Azure Monitor są domyślnie włączone, więc nie trzeba wykonywać operacji przechwytywania metryk. Należy jednak utworzyć wykresy lub pulpity nawigacyjne, aby wyświetlić te metryki. 
 
5. Jeśli utworzono reguły alertów oparte na klasycznych metrykach magazynu, [Utwórz reguły alertów](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-overview) oparte na metrykach w Azure monitor. 

6. Gdy będzie można zobaczyć wszystkie metryki w Azure Monitor, można wyłączyć rejestrowanie klasyczne. 

<a id="key-differences-between-classic-metrics-and-metrics-in-azure-monitor"></a>

## <a name="classic-metrics-vs-metrics-in-azure-monitor"></a>Metryki klasyczne a metryki w Azure Monitor

W tej sekcji opisano kilka najważniejszych różnic między tymi dwoma platformami metryk.

Główna różnica polega na tym, jak są zarządzane metryki. Metryki klasyczne są zarządzane przez usługę Azure Storage, a metryki w Azure Monitor są zarządzane przez Azure Monitor. W przypadku metryk klasycznych usługa Azure Storage zbiera wartości metryk, agreguje je i zapisuje je w tabelach, które znajdują się na koncie magazynu. Dzięki metrykom w Azure Monitor usługa Azure Storage wysyła dane metryk do Azure Monitor zaplecza. Azure Monitor zapewnia ujednolicone środowisko monitorowania, które obejmuje dane z Azure Portal, a także dane, które są pozyskiwane. 

W odniesieniu do metryk, klasyczne metryki zapewniają metryki **pojemności** tylko dla magazynu obiektów blob platformy Azure. Metryki w Azure Monitor zapewniają metryki pojemności dla obiektów blob, tabel, plików, kolejek i Premium Storage. Metryki klasyczne zapewniają metryki **transakcji** dla obiektów blob, tabel, Azure File i queue storage. Metryki w Azure Monitor dodać do tej listy usługę Premium Storage.

Jeśli działanie na koncie nie wyzwala metryki, w przypadku tej metryki metryki klasyczne wyświetlą wartość zero (0). Metryki w Azure Monitor będą całkowicie pomijać dane, co prowadzi do raportów dotyczących czyszczenia. Na przykład w przypadku metryk klasycznych, jeśli nie zostaną zgłoszone błędy limitu czasu serwera, `ServerTimeoutError` wartość w tabeli metryk jest równa 0. Azure Monitor nie zwraca żadnych danych podczas wykonywania zapytania o wartość metryki `Transactions` z wymiarem `ResponseType` równym `ServerTimeoutError` . 

Aby dowiedzieć się więcej o metrykach w Azure Monitor, zobacz [metryki w Azure monitor](https://docs.microsoft.com/azure/azure-monitor/platform/data-platform-metrics).

<a id="metrics-mapping-between-old-metrics-and-new-metrics"></a>

## <a name="map-classic-metrics-to-metrics-in-azure-monitor"></a>Mapowanie klasycznych metryk na metryki w Azure Monitor

 Te tabele służą do identyfikowania, które metryki w Azure Monitor zawierają te same dane, co aktualnie używane metryki. 

**Metryki pojemności**

| Metryka klasyczna | Metryka w Azure Monitor |
| ------------------- | ----------------- |
| `Capacity`            | `BlobCapacity`z wymiarem `BlobType` równym `BlockBlob` lub`PageBlob` |
| `ObjectCount`        | `BlobCount`z wymiarem `BlobType` równym `BlockBlob` lub`PageBlob` |
| `ContainerCount`      | `ContainerCount` |

> [!NOTE]
> Istnieje również kilka nowych metryk pojemności, które nie były dostępne jako metryki klasyczne. Aby wyświetlić kompletną listę, zobacz [metryki](../common/monitor-storage-reference.md#metrics).

**Metryki transakcji**

| Metryka klasyczna | Metryka w Azure Monitor |
| ------------------- | ----------------- |
| `AnonymousAuthorizationError` | Transakcje z wymiarem `ResponseType` równym `AuthorizationError` i wymiarem `Authentication` równym`Anonymous` |
| `AnonymousClientOtherError` | Transakcje z wymiarem `ResponseType` równym `ClientOtherError` i wymiarem `Authentication` równym`Anonymous` |
| `AnonymousClientTimeoutError` | Transakcje z wymiarem `ResponseType` równym `ClientTimeoutError` i wymiarem `Authentication` równym`Anonymous` |
| `AnonymousNetworkError` | Transakcje z wymiarem `ResponseType` równym `NetworkError` i wymiarem `Authentication` równym`Anonymous` |
| `AnonymousServerOtherError` | Transakcje z wymiarem `ResponseType` równym `ServerOtherError` i wymiarem `Authentication` równym`Anonymous` |
| `AnonymousServerTimeoutError` | Transakcje z wymiarem `ResponseType` równym `ServerTimeoutError` i wymiarem `Authentication` równym`Anonymous` |
| `AnonymousSuccess` | Transakcje z wymiarem `ResponseType` równym `Success` i wymiarem `Authentication` równym`Anonymous` |
| `AnonymousThrottlingError` | Transakcje z wymiarem `ResponseType` równym `ClientThrottlingError` lub `ServerBusyError` i `Authentication` równym wymiarowi równemu`Anonymous` |
| `AuthorizationError` | Transakcje z wymiarem `ResponseType` równym`AuthorizationError` |
| `Availability` | `Availability` |
| `AverageE2ELatency` | `SuccessE2ELatency` |
| `AverageServerLatency` | `SuccessServerLatency` |
| `ClientOtherError` | Transakcje z wymiarem `ResponseType` równym`ClientOtherError` |
| `ClientTimeoutError` | Transakcje z wymiarem `ResponseType` równym`ClientTimeoutError` |
| `NetworkError` | Transakcje z wymiarem `ResponseType` równym`NetworkError` |
| `PercentAuthorizationError` | Transakcje z wymiarem `ResponseType` równym`AuthorizationError` |
| `PercentClientOtherError` | Transakcje z wymiarem `ResponseType` równym`ClientOtherError` |
| `PercentNetworkError` | Transakcje z wymiarem `ResponseType` równym`NetworkError` |
| `PercentServerOtherError` | Transakcje z wymiarem `ResponseType` równym`ServerOtherError` |
| `PercentSuccess` | Transakcje z wymiarem `ResponseType` równym`Success` |
| `PercentThrottlingError` | Transakcje z wymiarem `ResponseType` równym `ClientThrottlingError` lub`ServerBusyError` |
| `PercentTimeoutError` | Transakcje z wymiarem `ResponseType` równym `ServerTimeoutError` lub `ResponseType` równym`ClientTimeoutError` |
| `SASAuthorizationError` | Transakcje z wymiarem `ResponseType` równym `AuthorizationError` i wymiarem `Authentication` równym`SAS` |
| `SASClientOtherError` | Transakcje z wymiarem `ResponseType` równym `ClientOtherError` i wymiarem `Authentication` równym`SAS` |
| `SASClientTimeoutError` | Transakcje z wymiarem `ResponseType` równym `ClientTimeoutError` i wymiarem `Authentication` równym`SAS` |
| `SASNetworkError` | Transakcje z wymiarem `ResponseType` równym `NetworkError` i wymiarem `Authentication` równym`SAS` |
| `SASServerOtherError` | Transakcje z wymiarem `ResponseType` równym `ServerOtherError` i wymiarem `Authentication` równym`SAS` |
| `SASServerTimeoutError` | Transakcje z wymiarem `ResponseType` równym `ServerTimeoutError` i wymiarem `Authentication` równym`SAS` |
| `SASSuccess` | Transakcje z wymiarem `ResponseType` równym `Success` i wymiarem `Authentication` równym`SAS` |
| `SASThrottlingError` | Transakcje z wymiarem `ResponseType` równym `ClientThrottlingError` lub `ServerBusyError` i `Authentication` równym wymiarowi równemu`SAS` |
| `ServerOtherError` | Transakcje z wymiarem `ResponseType` równym`ServerOtherError` |
| `ServerTimeoutError` | Transakcje z wymiarem `ResponseType` równym`ServerTimeoutError` |
| `Success` | Transakcje z wymiarem `ResponseType` równym`Success` |
| `ThrottlingError` | `Transactions`z wymiarem `ResponseType` równym `ClientThrottlingError` lub`ServerBusyError`|
| `TotalBillableRequests` | `Transactions` |
| `TotalEgress` | `Egress` |
| `TotalIngress` | `Ingress` |
| `TotalRequests` | `Transactions` |

## <a name="next-steps"></a>Następne kroki

* [Azure Monitor](../../monitoring-and-diagnostics/monitoring-overview.md)
* [Metryki magazynu w Azure Monitor](./storage-metrics-in-azure-monitor.md)
