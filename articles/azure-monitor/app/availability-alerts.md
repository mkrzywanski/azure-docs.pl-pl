---
title: Konfigurowanie alertów dostępności przy użyciu usługi Azure Application Insights | Microsoft Docs
description: Konfigurowanie testów sieci Web w usłudze Application Insights. Otrzymywanie alertów, kiedy witryna sieci Web staje się niedostępna lub wolno odpowiada.
ms.topic: conceptual
author: lgayhardt
ms.author: lagayhar
ms.date: 06/19/2019
ms.reviewer: sdash
ms.openlocfilehash: c53b43d811e5e2b9cd8d70e3c8e05af444895154
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/28/2020
ms.locfileid: "87320650"
---
# <a name="availability-alerts"></a>Alerty dostępności

Usługa [Azure Application Insights](./app-insights-overview.md) wysyła żądania sieci Web do aplikacji w regularnych odstępach czasu z punktów na całym świecie. Może to być alert, jeśli aplikacja nie odpowiada, lub jeśli reaguje zbyt wolno.

## <a name="enable-alerts"></a>Włączanie alertów

Alerty są teraz automatycznie włączane domyślnie, ale w celu pełnego skonfigurowania alertu musisz najpierw utworzyć swój Test dostępności.

![Tworzenie środowiska](./media/availability-alerts/create-test.png)

> [!NOTE]
>  Dzięki [nowym ujednoliconym alertom](../platform/alerts-overview.md)ważność reguły alertu i preferencje powiadamiania z [grupami akcji](../platform/action-groups.md) **musi być** skonfigurowana w środowisku alertów. Bez poniższych kroków będziesz otrzymywać tylko powiadomienia w portalu.

1. Po zapisaniu testu dostępności na karcie Szczegóły kliknij wielokropek przez właśnie wykonany test. Kliknij pozycję "Edytuj Alert".

   ![Edytuj po zapisaniu](./media/availability-alerts/edit-alert.png)

2. Ustaw żądany poziom ważności, opis reguły i najważniejsze — grupy akcji z preferencjami powiadomień, których chcesz użyć dla tej reguły alertu.

   ![Edytuj po zapisaniu](./media/availability-alerts/set-action-group.png)

> [!NOTE]
> Alerty dostępności utworzone za pośrednictwem tego środowiska są oparte na stanie. Oznacza to, że w przypadku spełnienia kryteriów alertów zostanie wygenerowany pojedynczy alert, gdy lokacja zostanie wykryta jako niedostępna. Jeśli witryna nadal jest w dalszym ciągu następnym razem, gdy zostaną ocenione kryteria alertu, nie spowoduje to wygenerowania nowego alertu. Jeśli więc witryna była wyłączona przez godzinę i skonfigurowano alert dotyczący wiadomości e-mail, otrzymasz wiadomość e-mail tylko wtedy, gdy lokacja zostanie wyłączona, a następnie wiadomość e-mail w przypadku utworzenia kopii zapasowej lokacji. Nie otrzymasz alertów ciągłych z przypomnieniem o tym, że witryna była nadal niedostępna.

### <a name="alert-on-x-out-of-y-locations-reporting-failures"></a>Alert dotyczący niepowodzeń zgłaszania lokalizacji X poza Y

Reguła alertu dotyczącego liczby lokalizacji X poza Y jest domyślnie włączona w [nowym środowisku ujednoliconych alertów](../platform/alerts-overview.md)podczas tworzenia nowego testu dostępności. Możesz zrezygnować z wyboru opcji "klasyczny" lub wybrać wyłączenie reguły alertu.

> [!NOTE]
> Skonfiguruj grupy akcji, aby otrzymywać powiadomienia o alertach, które są wyzwalane przez wykonanie powyższych kroków. Bez tego kroku powiadomienia w portalu będą odbierane tylko wtedy, gdy reguła jest wyzwalana.
>

### <a name="alert-on-availability-metrics"></a>Alert dotyczący metryk dostępności

Przy użyciu [nowych ujednoliconych alertów](../platform/alerts-overview.md)można generować alerty dotyczące metryk zagregowanych dostępności i testów oraz czasu trwania testu:

1. Wybierz zasób Application Insights w środowisku metryk i wybierz metrykę dostępności:

    ![Wybór metryk dostępności](./media/availability-alerts/select-metric.png)

2. Opcja Konfigurowanie alertów w menu spowoduje przejście do nowego środowiska, w którym można wybrać określone testy lub lokalizacje w celu skonfigurowania reguły alertu. W tym miejscu możesz również skonfigurować grupy akcji dla tej reguły alertu.

### <a name="alert-on-custom-analytics-queries"></a>Alert dotyczący niestandardowych zapytań analitycznych

Przy użyciu [nowych ujednoliconych alertów](../platform/alerts-overview.md)można generować alerty dotyczące [niestandardowych zapytań dzienników](../platform/alerts-unified-log.md). Za pomocą zapytań niestandardowych można otrzymywać alerty dotyczące dowolnego dowolnego warunku, który pomaga uzyskać najbardziej niezawodny sygnał związany z dostępnością. Ma to zastosowanie również w przypadku wysyłania niestandardowych wyników dostępności przy użyciu zestawu SDK TrackAvailability.

> [!Tip]
> Metryki dotyczące danych dostępności obejmują wszystkie niestandardowe wyniki dostępności, które mogą być przesyłane przez wywołanie naszego zestawu SDK TrackAvailability. Możesz użyć alertu dotyczącego obsługi metryk, aby otrzymywać alerty dotyczące niestandardowych wyników dostępności.
>

## <a name="automate-alerts"></a>Automatyzowanie alertów

Aby zautomatyzować ten proces za pomocą szablonów Azure Resource Manager, zapoznaj się z dokumentacją dotyczącą [tworzenia alertu metryki z szablonem Menedżer zasobów](../platform/alerts-metric-create-templates.md#template-for-an-availability-test-along-with-a-metric-alert) .

## <a name="troubleshooting"></a>Rozwiązywanie problemów

Dedykowany artykuł dotyczący [rozwiązywania problemów](troubleshoot-availability.md).

## <a name="next-steps"></a>Następne kroki

* [Wieloetapowe testy sieci Web](availability-multistep.md)
* [Testy sieci Web ping dla adresu URL](monitor-web-app-availability.md)

