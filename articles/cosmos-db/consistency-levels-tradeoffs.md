---
title: Azure Cosmos DB kompromisy dotyczące spójności, dostępności i wydajności
description: Wady dostępności i wydajności dla różnych poziomów spójności w Azure Cosmos DB.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/23/2020
ms.reviewer: sngun
ms.openlocfilehash: 4de696e2538bf1fa4823aafe30f931b7852535a7
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "82191740"
---
# <a name="consistency-availability-and-performance-tradeoffs"></a>Kompromisy w zakresie spójności, dostępności i wydajności

Rozproszone bazy danych polegające na replikacji do zapewnienia wysokiej dostępności, małych opóźnień lub obu tych charakterystyk muszą iść na pewne kompromisy. Te kompromisy dotyczą spójności odczytu wobec dostępności, opóźnienia i przepływności.

Azure Cosmos DB podejścia do spójności danych jako szeroki wybór. Takie podejście obejmuje więcej opcji niż dwa skrajne silne i ostateczne spójność. W spektrum spójności można wybierać spośród pięciu dokładnie zdefiniowanych poziomów. Od najsilniejszych do najsłabszych poziomów są:

- *Silna*
- *Powiązana nieaktualność*
- *Sesja*
- *Spójny prefiks*
- *Ostateczna*

Każdy poziom zapewnia kompromisy dostępności i wydajności i jest wspierany przez kompleksowe umowy SLA.

## <a name="consistency-levels-and-latency"></a>Poziomy spójności i czas oczekiwania

Opóźnienie odczytu dla wszystkich poziomów spójności zawsze gwarantuje mniej niż 10 milisekund w 99 percentylu. To opóźnienie odczytu jest obsługiwane przez umowę SLA. Średnie opóźnienie odczytu w pięćdziesiąt percentylu jest zwykle 4 milisekund lub mniej.

Czas oczekiwania na zapis dla wszystkich poziomów spójności jest zawsze gwarantowany poniżej 10 milisekund w 99 percentylu. To opóźnienie zapisu jest obsługiwane przez umowę SLA. Średnie opóźnienie zapisu w pięćdziesiąt percentylu jest zwykle 5 milisekund lub mniej. W przypadku kont usługi Azure Cosmos, które obejmują kilka regionów i skonfigurowano silną spójność, są wyjątkiem od tej gwarancji.

### <a name="write-latency-and-strong-consistency"></a>Opóźnienie zapisu i silna spójność

W przypadku kont usługi Azure Cosmos skonfigurowanych pod kątem silnej spójności z więcej niż jednym regionem opóźnienie zapisu jest równe dwukrotnemu czasowi błądzenia (RTT) między dowolnymi z dwóch najwyższych regionów i 10 milisekund w 99 percentylu. Wysoka wartość czasu RTT w sieci między regionami zostanie przeprowadzona w celu uzyskania większej opóźnienia w przypadku żądań Cosmos DB, ponieważ silna spójność kończy działanie dopiero po upewnieniu się, że został on przekazany do wszystkich regionów w ramach konta.

Dokładne opóźnienie czasu RTT to funkcja szybkości i topologii sieci platformy Azure. Sieci platformy Azure nie zapewniają umowy SLA opóźnienia dla RTT między dwoma regionami świadczenia usługi Azure. W Azure Portal są wyświetlane opóźnienia replikacji w ramach konta usługi Azure Cosmos. Możesz użyć Azure Portal (przejdź do bloku metryk, wybierz kartę spójności), aby monitorować opóźnienia replikacji między różnymi regionami, które są skojarzone z kontem usługi Azure Cosmos.

> [!IMPORTANT]
> Silna spójność kont z regionami obejmującymi ponad 5000 kilometrów (8000 kilometrów) jest domyślnie zablokowana z powodu dużego opóźnienia zapisu. Aby włączyć tę funkcję, skontaktuj się z pomocą techniczną.

## <a name="consistency-levels-and-throughput"></a>Poziomy spójności i przepływność

- Aby zapewnić silną i powiązana nieaktualność, odczyty są przeprowadzane w przypadku dwóch replik z czterema zestawem replik (kworum mniejszościowego) w celu zapewnienia spójności. Sesja, spójny prefiks i ostateczne odczyty pojedynczej repliki. W efekcie, w przypadku tej samej liczby jednostek żądania, przepływność odczytu dla silnych i ograniczonych nieodświeżonych jest połowami innych poziomów spójności.

- Dla danego typu operacji zapisu, takich jak INSERT, Replace, upsert i DELETE, przepływność zapisu dla jednostek żądania jest taka sama dla wszystkich poziomów spójności.

|**Poziom spójności**|**Odczyty kworum**|**Zapisy kworum**|
|--|--|--|
|**Silna**|Lokalna mniejszości|Większość globalna|
|**Powiązana nieaktualność**|Lokalna mniejszości|Większość lokalna|
|**Sesja**|Pojedyncza replika (przy użyciu tokenu sesji)|Większość lokalna|
|**Spójny prefiks**|Pojedyncza replika|Większość lokalna|
|**Ostateczna**|Pojedyncza replika|Większość lokalna|

## <a name="consistency-levels-and-data-durability"></a><a id="rto"></a>Poziomy spójności i trwałość danych

W globalnie rozproszonym środowisku bazy danych istnieje bezpośrednia relacja między poziomem spójności a trwałością danych w przypadku awarii całego regionu. Podczas opracowywania planu ciągłości biznesowej należy zrozumieć maksymalny akceptowalny czas, po upływie którego aplikacja zostanie w pełni odzyskana po zdarzeniu zakłócania. Czas wymagany do pełnego odzyskania aplikacji jest znany jako **cel czasu odzyskiwania** (**RTO**). Należy również zrozumieć maksymalny okres ostatnich aktualizacji danych, które aplikacja może tolerować podczas odzyskiwania po wystąpieniu zdarzenia zakłócenia. Przedział czasu aktualizacji, które mogą zostać utracone, jest określany jako **cel punktu odzyskiwania** (**RPO**).

W poniższej tabeli zdefiniowano relacje między modelem spójności i trwałością danych w przypadku awarii całego regionu. Należy pamiętać, że w systemie rozproszonym, nawet ze silną spójnością, nie można mieć rozproszonej bazy danych z RPO i RTO zero ze względu na zakończenie theorem. Aby dowiedzieć się więcej na temat przyczyn, zobacz [poziomy spójności w Azure Cosmos DB](consistency-levels.md).

|**Regiony**|**Tryb replikacji**|**Poziom spójności**|**CEL PUNKTU ODZYSKIWANIA**|**RTO**|
|---------|---------|---------|---------|---------|
|1|Jeden lub wiele wzorców|Dowolny poziom spójności|< 240 minut|<1 tydzień|
|>1|Pojedynczy wzorzec|Sesja, spójny prefiks, ostateczna|< 15 minut|< 15 minut|
|>1|Pojedynczy wzorzec|Powiązana nieaktualność|*K*  &  *T*|< 15 minut|
|>1|Pojedynczy wzorzec|Silna|0|< 15 minut|
|>1|Wiele wzorców|Sesja, spójny prefiks, ostateczna|< 15 minut|0|
|>1|Wiele wzorców|Powiązana nieaktualność|*K*  &  *T*|0|

*K* = liczba wersji *"K"* (tj. aktualizacji) elementu.

*T* = przedział czasu *"T"* od momentu ostatniej aktualizacji.

## <a name="strong-consistency-and-multi-master"></a>Silna spójność i wiele wzorców

Kont Cosmos skonfigurowanych dla wielu wzorców nie można skonfigurować pod kątem silnej spójności, ponieważ nie jest to możliwe w systemie rozproszonym, aby zapewnić cel punktu odzyskiwania równy zero i RTO równy zero. Ponadto nie ma korzyści z opóźnienia zapisu w przypadku używania silnej spójności z wieloma wzorcami, ponieważ każdy zapis w dowolnym regionie musi być replikowany i zatwierdzony do wszystkich skonfigurowanych regionów w ramach konta. Powoduje to takie samo opóźnienie zapisu jak pojedyncze konto główne.

## <a name="next-steps"></a>Następne kroki

Dowiedz się więcej o globalnej dystrybucji i ogólnym kompromisie spójności w systemach rozproszonych. Zobacz następujące artykuły:

- [Założenia dotyczące spójności w nowoczesnych systemach rozproszonej bazy danych](https://www.computer.org/csdl/magazine/co/2012/02/mco2012020037/13rRUxjyX7k)
- [Wysoka dostępność](high-availability.md)
- [Azure Cosmos DB umowy SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_2/)
