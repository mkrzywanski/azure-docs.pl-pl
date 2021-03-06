---
title: Wizualizowanie danych z Azure Monitor | Microsoft Docs
description: Zawiera podsumowanie dostępnych metod wizualizacji danych metryk i dzienników przechowywanych w Azure Monitor.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 03/17/2020
ms.openlocfilehash: 195e606a66b1b49821fc1b46381fdc551f142a6a
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/28/2020
ms.locfileid: "87325529"
---
# <a name="visualizing-data-from-azure-monitor"></a>Wizualizowanie danych z usługi Azure Monitor
Ten artykuł zawiera podsumowanie dostępnych metod wizualizacji danych dziennika i metryk przechowywanych w Azure Monitor.

Wizualizacje, takie jak wykresy i wykresy, mogą ułatwić analizowanie danych monitorowania w celu przechodzenia do szczegółów dotyczących problemów i identyfikowania wzorców. W zależności od używanego narzędzia możesz również mieć możliwość udostępniania wizualizacji innym użytkownikom wewnątrz i na zewnątrz organizacji.

## <a name="workbooks"></a>Skoroszyty
[Skoroszyty](./platform/workbooks-overview.md) to interaktywne dokumenty, które zapewniają szczegółowe informacje o danych, badaniu i współpracy w zespole. Konkretne przykłady, w których przydatne są skoroszyty, to przewodniki dotyczące rozwiązywania problemów i postmortem zdarzeń.

![skoroszyt](media/visualizations/workbook.png)

### <a name="advantages"></a>Zalety
- Obsługuje metryki i dzienniki.
- Obsługuje parametry włączające interaktywne raporty, w których wybranie elementu w tabeli spowoduje dynamiczne aktualizowanie skojarzonych wykresów i wizualizacji.
- Przepływ podobny do dokumentu.
- Opcja dla skoroszytów osobistych lub udostępnionych.
- Łatwe, przyjazne dla współpracy środowisko tworzenia.
- Szablony obsługują publiczną galerię szablonów opartą na witrynie GitHub.

### <a name="limitations"></a>Ograniczenia
- Brak automatycznego odświeżania.
- Nie ma układu gęstego, takiego jak pulpity nawigacyjne, co sprawia, że skoroszyty są mniej przydatne jako pojedyncze okienko szkła. Zamierzone więcej, aby zapewnić dokładniejszy wgląd w szczegółowe dane.


## <a name="azure-dashboards"></a>Pulpity nawigacyjne platformy Azure
[Pulpity nawigacyjne platformy Azure](../azure-portal/azure-portal-dashboards.md) są podstawową technologią nawigacyjną dla platformy Azure. Są one szczególnie przydatne w przypadku udostępniania pojedynczego okienka Glass przez infrastrukturę i usługi platformy Azure, co pozwala na szybkie identyfikowanie ważnych problemów.

![Pulpit nawigacyjny](media/visualizations/dashboard.png)

Oto przewodnik wideo dotyczący tworzenia pulpitów nawigacyjnych.

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE4AslH]

### <a name="advantages"></a>Zalety
- Głębokiej integracji z platformą Azure. Wizualizacje można przypinać do pulpitów nawigacyjnych z wielu stron platformy Azure, w tym Eksplorator metryk, Log Analytics i Application Insights.
- Obsługuje metryki i dzienniki.
- Połącz dane z wielu źródeł, w tym dane wyjściowe z [Eksploratora metryk](platform/metrics-charts.md), [zapytań dzienników](log-query/log-query-overview.md)i [map](app/app-map.md) i dostępności w Application Insights.
- Opcja dla osobistych lub udostępnionych pulpitów nawigacyjnych. Integracja z [uwierzytelnianiem opartym na rolach (RBAC) na](../role-based-access-control/overview.md)platformie Azure.
- Automatyczne odświeżanie. Odświeżanie metryk zależy od zakresu czasu, który jest co najmniej pięć minut. Dzienniki są odświeżane co godzinę przy użyciu opcji ręcznego odświeżania na żądanie, klikając ikonę "Odśwież" w danej wizualizacji lub odświeżając pełny pulpit nawigacyjny.
- Parametry, które są pulpitami nawigacyjnymi metryk z sygnaturami czasowymi i parametry niestandardowe.
- Elastyczne opcje układu.
- Tryb pełnoekranowy.


### <a name="limitations"></a>Ograniczenia
- Ograniczona kontrola nad wizualizacjami dzienników bez obsługi tabel danych. Łączna liczba serii danych jest ograniczona do 10 i dalsze serie danych są pogrupowane w _innym_ zasobniku.
- Brak obsługi parametrów niestandardowych dla wykresów dzienników.
- Wykresy dzienników są ograniczone do 30 ostatnich dni.
- Wykresy dzienników można przypinać tylko do udostępnionych pulpitów nawigacyjnych.
- Brak interakcji z danymi pulpitu nawigacyjnego.
- Ograniczone kontekstowe przechodzenie do szczegółów.


## <a name="power-bi"></a>Power BI
[Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/) jest szczególnie przydatna do tworzenia ukierunkowanych na działalność firmowych pulpitów nawigacyjnych i raportów, a także raportów umożliwiających analizowanie długoterminowych trendów kluczowych wskaźników wydajności. [Wyniki zapytania dziennika można zaimportować](platform/powerbi.md) do zestawu danych Power BI, dzięki czemu można korzystać z jego funkcji, takich jak łączenie danych z różnych źródeł i udostępnianie raportów w sieci Web i na urządzeniach przenośnych.

![Power BI](media/visualizations/power-bi.png)

### <a name="advantages"></a>Zalety
- Rozbudowane wizualizacje.
- Rozległe interakcje, w tym powiększanie i filtrowanie krzyżowe.
- Łatwa do udostępnienia w całej organizacji.
- Integracja z innymi danymi z wielu źródeł danych.
- Lepsza wydajność z wynikami buforowanymi w module.


### <a name="limitations"></a>Ograniczenia
- Obsługuje dzienniki, ale nie metryki.
- Brak integracji z platformą Azure. Nie można zarządzać pulpitami nawigacyjnymi i modelami za poorednictwem Azure Resource Manager.
- Aby można było skonfigurować wyniki zapytania, należy je zaimportować do modelu Power BI. Ograniczenie rozmiaru i odświeżania wyniku.
- Ograniczone odświeżanie danych przez osiem razy dziennie.


## <a name="grafana"></a>Grafana
[Grafana](https://grafana.com/) to otwarta platforma, którą program Excel ma w operacyjnych pulpitach nawigacyjnych. Jest to szczególnie przydatne w przypadku wykrywania i izolowania i segregowania zdarzeń operacyjnych. Możesz dodać [wtyczkę Grafana Azure monitor źródła danych](platform/grafana-plugin.md) do subskrypcji platformy Azure, aby wizualizować dane metryk platformy Azure.

![Grafana](media/visualizations/grafana.png)

### <a name="advantages"></a>Zalety
- Rozbudowane wizualizacje.
- Bogaty ekosystem źródeł danych.
- Interaktywność danych, w tym powiększanie.
- Obsługuje parametry.

### <a name="limitations"></a>Ograniczenia
- Brak integracji z platformą Azure. Nie można zarządzać pulpitami nawigacyjnymi i modelami za poorednictwem Azure Resource Manager.
- Koszt obsługi dodatkowej infrastruktury usługi Grafana lub dodatkowego kosztu chmury Grafana.


## <a name="build-your-own-custom-application"></a>Tworzenie własnej aplikacji niestandardowej
Możesz uzyskać dostęp do danych w dziennikach i danych metryk w Azure Monitor za pomocą interfejsu API za pomocą dowolnego klienta REST, który umożliwia tworzenie własnych niestandardowych witryn sieci Web i aplikacji.

### <a name="advantages"></a>Zalety
- Pełna elastyczność interfejsu użytkownika, wizualizacji, interakcji i funkcji.
- Łączenie metryk i danych dzienników z innymi źródłami danych.

### <a name="disadvantages"></a>Wady
- Wymagany jest znaczny nakład pracy inżynieryjnej.


## <a name="azure-monitor-views"></a>Widoki Azure Monitor

> [!IMPORTANT]
> Widoki są w trakcie przestarzałe. Aby uzyskać wskazówki dotyczące konwertowania widoków na skoroszyty, zobacz temat [Azure monitor View Designer to skoroszyts](platform/view-designer-conversion-overview.md) .

[Widoki w Azure monitor](platform/view-designer.md) umożliwiają tworzenie niestandardowych wizualizacji przy użyciu danych dziennika. Są one używane przez [rozwiązania monitorujące](insights/solutions.md) , które umożliwiają prezentowanie zbieranych danych.


![Widok](media/visualizations/view.png)

### <a name="advantages"></a>Zalety
- Rozbudowane wizualizacje danych dziennika.
- Eksportuj i Importuj widoki, aby przenieść je do innych grup zasobów i subskrypcji.
- Integruje się z modelem zarządzania Azure Monitor z obszarami roboczymi i rozwiązaniami do monitorowania.
- [Filtry](platform/view-designer-filters.md) dla parametrów niestandardowych.
- Interaktywny, obsługuje przechodzenie na wiele poziomów (widok przechodzenia do szczegółów w innym widoku)

### <a name="limitations"></a>Ograniczenia
- Obsługuje dzienniki, ale nie metryki.
- Brak widoków osobistych. Dostępne dla wszystkich użytkowników mających dostęp do obszaru roboczego.
- Brak automatycznego odświeżania.
- Ograniczone opcje układu.
- Brak obsługi zapytań w wielu obszarach roboczych lub aplikacjach Application Insights.
- Zapytania są ograniczone w rozmiarze odpowiedzi do 8 MB i czas wykonywania zapytania wynoszący 110 sekund.

## <a name="next-steps"></a>Następne kroki
- Dowiedz się więcej na temat [danych zbieranych przez Azure monitor](platform/data-platform.md).
- Dowiedz się więcej o [pulpitach nawigacyjnych platformy Azure](../azure-portal/azure-portal-dashboards.md).
- Dowiedz się więcej o [widokach w Azure monitor](platform/view-designer.md).
- Dowiedz się więcej na temat [skoroszytów](./platform/workbooks-overview.md).
- Dowiedz się więcej [na temat importowania danych dziennika do Power BI](./platform/powerbi.md).
- Dowiedz się więcej na temat [wtyczki źródła danych Grafana Azure monitor](./platform/grafana-plugin.md).

