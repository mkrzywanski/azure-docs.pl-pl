---
title: Metryki obsługiwane przez Azure Event Grid
description: Ten artykuł zawiera Azure Monitor metryki obsługiwane przez usługę Azure Event Grid.
ms.topic: conceptual
ms.date: 07/07/2020
ms.openlocfilehash: 3b22beafc9f88d2d95b25fd7ad2f2308a4df9097
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/08/2020
ms.locfileid: "86116424"
---
# <a name="metrics-supported-by-azure-event-grid"></a>Metryki obsługiwane przez Azure Event Grid
Ten artykuł zawiera listę metryk Event Grid, które są podzielone według przestrzeni nazw. 

## <a name="microsofteventgriddomains"></a>Microsoft. EventGrid/domeny

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|PublishSuccessCount|Zdarzenia opublikowane|Liczba|Łącznie|Łączna liczba zdarzeń opublikowanych w tym temacie|Temat|
|PublishFailCount|Publikowanie zdarzeń zakończonych niepowodzeniem|Liczba|Łącznie|Całkowita liczba zdarzeń, których publikowanie nie powiodło się w tym temacie|Temat, Błądtype, błąd|
|PublishSuccessLatencyInMs|Czas oczekiwania na pomyślne publikowanie|)|Łącznie|Opóźnienie sukcesu publikacji w milisekundach|Brak|
|MatchedEventCount|Dopasowane zdarzenia|Liczba|Łącznie|Łączna liczba zdarzeń dopasowanych do tej subskrypcji zdarzeń|Temat, EventSubscriptionName, DomainEventSubscriptionName|
|DeliveryAttemptFailCount|Zdarzenia zakończonych niepowodzeniem|Liczba|Łącznie|Całkowita liczba zdarzeń, których dostarczenie do tej subskrypcji zdarzeń nie powiodło się|Temat, EventSubscriptionName, DomainEventSubscriptionName, Error, ErrorType|
|DeliverySuccessCount|Dostarczone zdarzenia|Liczba|Łącznie|Całkowita liczba zdarzeń dostarczonych do tej subskrypcji zdarzeń|Temat, EventSubscriptionName, DomainEventSubscriptionName|
|DestinationProcessingDurationInMs|Czas przetwarzania docelowego|)|Średnia|Czas trwania przetwarzania docelowego w milisekundach|Temat, EventSubscriptionName, DomainEventSubscriptionName|
|DroppedEventCount|Opuszczone zdarzenia|Liczba|Łącznie|Całkowita liczba porzuconych zdarzeń pasujących do tej subskrypcji zdarzeń|Temat, EventSubscriptionName, DomainEventSubscriptionName, DropReason|
|DeadLetteredCount|Zdarzenia utraconych wiadomości|Liczba|Łącznie|Łączna liczba utraconych zdarzeń, które pasują do tej subskrypcji zdarzeń|Temat, EventSubscriptionName, DomainEventSubscriptionName, DeadLetterReason|

## <a name="microsofteventgridtopics"></a>Microsoft. EventGrid/tematy

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|PublishSuccessCount|Zdarzenia opublikowane|Liczba|Łącznie|Łączna liczba zdarzeń opublikowanych w tym temacie|Brak|
|PublishFailCount|Publikowanie zdarzeń zakończonych niepowodzeniem|Liczba|Łącznie|Całkowita liczba zdarzeń, których publikowanie nie powiodło się w tym temacie|ErrorType, błąd|
|UnmatchedEventCount|Niedopasowane zdarzenia|Liczba|Łącznie|Łączna liczba zdarzeń, które nie pasują do żadnej subskrypcji zdarzeń dla tego tematu|Brak|
|PublishSuccessLatencyInMs|Czas oczekiwania na pomyślne publikowanie|)|Łącznie|Opóźnienie sukcesu publikacji w milisekundach|Brak|
|MatchedEventCount|Dopasowane zdarzenia|Liczba|Łącznie|Łączna liczba zdarzeń dopasowanych do tej subskrypcji zdarzeń|EventSubscriptionName|
|DeliveryAttemptFailCount|Zdarzenia zakończonych niepowodzeniem|Liczba|Łącznie|Całkowita liczba zdarzeń, których dostarczenie do tej subskrypcji zdarzeń nie powiodło się|Error, ErrorType, EventSubscriptionName|
|DeliverySuccessCount|Dostarczone zdarzenia|Liczba|Łącznie|Całkowita liczba zdarzeń dostarczonych do tej subskrypcji zdarzeń|EventSubscriptionName|
|DestinationProcessingDurationInMs|Czas przetwarzania docelowego|)|Średnia|Czas trwania przetwarzania docelowego w milisekundach|EventSubscriptionName|
|DroppedEventCount|Opuszczone zdarzenia|Liczba|Łącznie|Całkowita liczba porzuconych zdarzeń pasujących do tej subskrypcji zdarzeń|DropReason, EventSubscriptionName|
|DeadLetteredCount|Zdarzenia utraconych wiadomości|Liczba|Łącznie|Łączna liczba utraconych zdarzeń, które pasują do tej subskrypcji zdarzeń|DeadLetterReason, EventSubscriptionName|

## <a name="microsofteventgridsystemtopics"></a>Microsoft. EventGrid/systemTopics

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|PublishSuccessCount|Zdarzenia opublikowane|Liczba|Łącznie|Łączna liczba zdarzeń opublikowanych w tym temacie|Brak|
|PublishFailCount|Publikowanie zdarzeń zakończonych niepowodzeniem|Liczba|Łącznie|Całkowita liczba zdarzeń, których publikowanie nie powiodło się w tym temacie|ErrorType, błąd|
|UnmatchedEventCount|Niedopasowane zdarzenia|Liczba|Łącznie|Łączna liczba zdarzeń, które nie pasują do żadnej subskrypcji zdarzeń dla tego tematu|Brak|
|PublishSuccessLatencyInMs|Czas oczekiwania na pomyślne publikowanie|)|Łącznie|Opóźnienie sukcesu publikacji w milisekundach|Brak|
|MatchedEventCount|Dopasowane zdarzenia|Liczba|Łącznie|Łączna liczba zdarzeń dopasowanych do tej subskrypcji zdarzeń|EventSubscriptionName|
|DeliveryAttemptFailCount|Zdarzenia zakończonych niepowodzeniem|Liczba|Łącznie|Całkowita liczba zdarzeń, których dostarczenie do tej subskrypcji zdarzeń nie powiodło się|Error, ErrorType, EventSubscriptionName|
|DeliverySuccessCount|Dostarczone zdarzenia|Liczba|Łącznie|Całkowita liczba zdarzeń dostarczonych do tej subskrypcji zdarzeń|EventSubscriptionName|
|DestinationProcessingDurationInMs|Czas przetwarzania docelowego|)|Średnia|Czas trwania przetwarzania docelowego w milisekundach|EventSubscriptionName|
|DroppedEventCount|Opuszczone zdarzenia|Liczba|Łącznie|Całkowita liczba porzuconych zdarzeń pasujących do tej subskrypcji zdarzeń|DropReason, EventSubscriptionName|
|DeadLetteredCount|Zdarzenia utraconych wiadomości|Liczba|Łącznie|Łączna liczba utraconych zdarzeń, które pasują do tej subskrypcji zdarzeń|DeadLetterReason, EventSubscriptionName|

## <a name="microsofteventgrideventsubscriptions"></a>Microsoft. EventGrid/eventSubscriptions

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|MatchedEventCount|Dopasowane zdarzenia|Liczba|Łącznie|Łączna liczba zdarzeń dopasowanych do tej subskrypcji zdarzeń|Brak|
|DeliveryAttemptFailCount|Zdarzenia zakończonych niepowodzeniem|Liczba|Łącznie|Całkowita liczba zdarzeń, których dostarczenie do tej subskrypcji zdarzeń nie powiodło się|Błąd, Błądtype|
|DeliverySuccessCount|Dostarczone zdarzenia|Liczba|Łącznie|Całkowita liczba zdarzeń dostarczonych do tej subskrypcji zdarzeń|Brak|
|DestinationProcessingDurationInMs|Czas przetwarzania docelowego|)|Średnia|Czas trwania przetwarzania docelowego w milisekundach|Brak|
|DroppedEventCount|Opuszczone zdarzenia|Liczba|Łącznie|Całkowita liczba porzuconych zdarzeń pasujących do tej subskrypcji zdarzeń|DropReason|
|DeadLetteredCount|Zdarzenia utraconych wiadomości|Liczba|Łącznie|Łączna liczba utraconych zdarzeń, które pasują do tej subskrypcji zdarzeń|DeadLetterReason|

## <a name="microsofteventgridextensiontopics"></a>Microsoft. EventGrid/extensionTopics

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|PublishSuccessCount|Zdarzenia opublikowane|Liczba|Łącznie|Łączna liczba zdarzeń opublikowanych w tym temacie|Brak|
|PublishFailCount|Publikowanie zdarzeń zakończonych niepowodzeniem|Liczba|Łącznie|Całkowita liczba zdarzeń, których publikowanie nie powiodło się w tym temacie|ErrorType, błąd|
|UnmatchedEventCount|Niedopasowane zdarzenia|Liczba|Łącznie|Łączna liczba zdarzeń, które nie pasują do żadnej subskrypcji zdarzeń dla tego tematu|Brak|
|PublishSuccessLatencyInMs|Czas oczekiwania na pomyślne publikowanie|)|Łącznie|Opóźnienie sukcesu publikacji w milisekundach|Brak|

## <a name="next-steps"></a>Następne kroki
Zapoznaj się z następującym artykułem: [dzienniki diagnostyczne](diagnostic-logs.md)
