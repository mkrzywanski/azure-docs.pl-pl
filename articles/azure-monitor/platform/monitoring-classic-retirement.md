---
title: Aktualizacja klasycznego alertu & monitorowania w programie Azure Monitor
description: Opis wycofania usług i funkcji monitorowania klasycznego, wcześniej pokazanych w Azure Portal w obszarze alerty (klasyczne).
author: yanivlavi
services: azure-monitor
ms.topic: conceptual
ms.date: 2/7/2019
ms.author: yalavi
ms.subservice: alerts
ms.openlocfilehash: 2f8f1cceb5cfee4bf910674ecd50c7fba1d5c021
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/28/2020
ms.locfileid: "87318372"
---
# <a name="unified-alerting--monitoring-in-azure-monitor-replaces-classic-alerting--monitoring"></a>Ujednolicone & monitorowania alertów w Azure Monitor zastępują klasyczne alerty & monitorowania

Azure Monitor teraz stać się ujednoliconą pełną usługą monitorowania stosu, która obsługuje teraz "jedną metrykę" i "jedno alerty" między zasobami; Aby uzyskać więcej informacji, zobacz nasz [wpis w blogu dotyczący nowych Azure monitor](https://azure.microsoft.com/blog/new-full-stack-monitoring-capabilities-in-azure-monitor/). Nowe platformy do monitorowania i zgłaszania alertów platformy Azure zostały skompilowane, aby były szybsze, inteligentniejsze i rozszerzalne — dzięki rozwijającej się expanse w chmurze obliczeniowej i w firmie Microsoft Intelligent Cloud. 

Dzięki nowej platformie monitorowania i wysyłania alertów platformy Azure zostanie wycofana "Klasyczna" platforma monitorowania i powiadamiania o alertach — obsługiwana w sekcji *Wyświetl alerty klasyczne* w ramach alertów platformy Azure **zostanie wycofana z sierpnia 2019 w chmurach publicznych platformy Azure**. Nie wpłynie to na [Azure government w chmurze](../../azure-government/documentation-government-welcome.md) i na [platformie Azure (Chiny](https://docs.azure.cn/) ).

> [!NOTE]
> Z powodu opóźnienia w wycofywaniu narzędzia migracji Data wycofania migracji klasycznych alertów została [rozszerzona do 31 sierpnia 2019](https://azure.microsoft.com/updates/azure-monitor-classic-alerts-retirement-date-extended-to-august-31st-2019/) od pierwotnie ogłoszonej daty 30 czerwca 2019.

 ![Alert klasyczny w Azure Portal](media/monitoring-classic-retirement/monitor-alert-screen2.png) 

Zachęcamy do rozpoczęcia i ponownego utworzenia alertów na nowej platformie. W przypadku klientów, którzy mają dużą liczbę alertów, wdrażamy [w fazach](alerts-understand-migration.md#rollout-phases), czyli [dobrowolne narzędzie do migracji](alerts-using-migration-tool.md) , które przenosi istniejące alerty klasyczne do nowego systemu alertów, bez zakłócania ani dodawania kosztów.

> [!IMPORTANT]
> Klasyczne reguły alertów utworzone w dzienniku aktywności nie będą przestarzałe ani zmigrowane. Dostęp do wszystkich klasycznych reguł alertów utworzonych w dzienniku aktywności można uzyskać, korzystając z nowych alertów Azure Monitor. Aby uzyskać więcej informacji, zobacz [Tworzenie i wyświetlanie alertów dziennika aktywności oraz zarządzanie nimi za pomocą Azure monitor](./alerts-activity-log.md). Podobnie można uzyskać dostęp do alertów dotyczących Service Health i używać ich w przypadku, gdy pochodzi z nowej sekcji Service Health. Aby uzyskać szczegółowe informacje, zobacz [alerty dotyczące powiadomień o kondycji usługi](../../service-health/alerts-activity-log-service-notifications-portal.md).

## <a name="unified-metrics-and-alerts-in-application-insights"></a>Ujednolicone metryki i alerty w Application Insights

Nowsza platforma metryczna Azure Monitor będzie teraz monitorować energię z Application Insights. To przeniesienie oznacza, Application Insights będą podłączane do grup akcji, co pozwala na znacznie więcej opcji niż tylko poprzednie wywołania poczty e-mail i elementu webhook. Alerty mogą teraz wyzwalać wywołania głosowe, Azure Functions, Logic Apps, SMS i narzędzia ITSM, takie jak usługi ServiceNow i elementy Runbook automatyzacji. Dzięki monitorowaniu i alertom w czasie rzeczywistym Nowa platforma umożliwia Application Insights użytkownikom korzystanie z tej samej technologii, co w przypadku innych zasobów platformy Azure i obpinanie monitorowania produktów firmy Microsoft.

Nowe ujednolicone monitorowanie i alerty dla Application Insights obejmie:

- **Metryki platformy Application Insights** , które udostępniają popularne wstępnie skompilowane metryki z Application Insights produktu. Aby uzyskać więcej informacji, zobacz ten artykuł dotyczący używania [metryk platformy dla Application Insights w przypadku nowych Azure monitor](../app/pre-aggregated-metrics-log-metrics.md#pre-aggregated-metrics).
- **Application Insights dostępność i testowanie sieci Web** — dzięki czemu można ocenić czas odpowiedzi i dostępność aplikacji lub serwera sieci Web. Aby uzyskać więcej informacji, zapoznaj się z artykułem dotyczącym korzystania z [testów dostępności i alertów dotyczących Application Insights na nowych Azure monitor](../app/monitor-web-app-availability.md).
- **Application Insights metryki niestandardowych** — pozwalające definiować i emitować własne metryki dotyczące monitorowania i alertów. Aby uzyskać więcej informacji, zobacz ten artykuł dotyczący używania [metryki niestandardowej na potrzeby Application Insights na nowych Azure monitor](../app/pre-aggregated-metrics-log-metrics.md#custom-metrics-dimensions-and-pre-aggregation).
- **Anomalie niepowodzeń Application Insights (część wykrywania inteligentnego)** — które automatycznie powiadamia, gdy aplikacja sieci Web napotyka nietypowy wzrost liczby nieudanych żądań HTTP lub wywołań zależności. Aby uzyskać więcej informacji, zobacz ten artykuł dotyczący używania [anomalii wykrywania Smart Detection](../app/proactive-failure-diagnostics.md).

## <a name="unified-metrics-and-alerts-for-other-azure-resources"></a>Ujednolicone metryki i alerty dla innych zasobów platformy Azure

Od marca 2018 Kolejna generacja alertów i monitorowania wielowymiarowego dla zasobów platformy Azure były dostępne. Teraz nowsza platforma metryczna i alerty są szybsze dzięki możliwościom niemal w czasie rzeczywistym. Co więcej, nowsze alerty platformy metrycznej zapewniają większy poziom szczegółowości, ponieważ nowsza platforma zawiera opcję wymiarów, która umożliwia wycięcie i przefiltrowanie do określonej kombinacji wartości, warunku lub operacji. Podobnie jak w przypadku wszystkich alertów w nowym Azure Monitor, nowsze alerty metryk są bardziej rozszerzalne przy użyciu ActionGroups — Zezwalanie na powiadomienia po rozszerzeniu wiadomości e-mail lub elementu webhook do wiadomości SMS, głosu, funkcji platformy Azure, elementu Runbook usługi Automation i innych. Aby uzyskać więcej informacji, zobacz [Tworzenie i wyświetlanie alertów metryk i zarządzanie nimi przy użyciu Azure monitor](./alerts-metric.md).
Nowsze metryki dla zasobów platformy Azure są dostępne jako:

- **Azure monitor metryki standardowej platformy** — oferuje popularne, wstępnie wypełnione metryki z różnych usług i produktów platformy Azure. Aby uzyskać więcej informacji, zapoznaj się z artykułem dotyczącym [obsługiwanych metryk w witrynie Azure monitor](./alerts-metric-near-real-time.md#metrics-and-dimensions-supported) i [Obsługuj alerty dotyczące metryk w Azure monitor](./alerts-metric-overview.md#supported-resource-types-for-metric-alerts).
- **Azure monitor metryki niestandardowych** — zapewniające metryki z źródeł opartych na użytkownikach, w tym agenta Diagnostyka Azure. Aby uzyskać więcej informacji, zapoznaj się z artykułem dotyczącym [niestandardowych metryk w Azure monitor](./metrics-custom-overview.md). Przy użyciu metryk niestandardowych można także publikować metryki zbierane przez program [Windows Diagnostyka Azure Agent](./collect-custom-metrics-guestos-resource-manager-vm.md) i [agenta telegraf InfluxData](./collect-custom-metrics-linux-telegraf.md).

## <a name="retirement-of-classic-monitoring-and-alerting-platform"></a>Wycofanie klasycznej platformy monitorowania i zgłaszania alertów

Jak wspomniano wcześniej, klasyczna platforma monitorowania i zgłaszania alertów, która jest obecnie używana z [sekcji alertów (klasycznej)](./alerts-classic.overview.md) Azure Portal zostanie wycofana w ciągu najbliższych miesięcy, ponieważ zostały zastąpione przez nowszy system.
Starsze klasyczne monitorowanie i alerty zostaną wycofane z dniem 31 sierpnia 2019; Dołączanie do zamykania powiązanych interfejsów API, Azure Portal interfejsu i usług. W tym celu funkcje te będą przestarzałe:

- Starsze (klasyczne) metryki i alerty dla zasobów platformy Azure, które są obecnie dostępne za pośrednictwem [sekcji alertów (klasyczne)](./alerts-classic.overview.md) w Azure Portal; dostępne jako [Microsoft. Insights/alertrules](/rest/api/monitor/alertrules) Resource
- Starsza (klasyczna) platforma i metryki niestandardowe dla Application Insights, a także alerty dla nich, które są obecnie dostępne za pośrednictwem [sekcji alerty (klasyczne)](./alerts-classic.overview.md) w Azure Portal i dostępne jako [Microsoft. Insights/alertrules](/rest/api/monitor/alertrules) Resource
- Stan starszej wersji (klasycznej) alert anomalii o niepowodzeń jest obecnie dostępny jako [Inteligentne wykrywanie wewnątrz Application Insights](../app/proactive-diagnostics.md) w Azure Portal; z alertami skonfigurowanymi w [sekcji alerty (klasyczne)](./alerts-classic.overview.md) w Azure Portal

Wszystkie klasyczne systemy monitorowania i powiadamiania, w tym odpowiednie dla [interfejsu API](/rest/api/monitor/alertrules), [programu PowerShell](./alerts-classic-portal.md), [interfejsu wiersza polecenia](./alerts-classic-portal.md), [strony Azure Portal](./alerts-classic-portal.md)i [szablonu zasobów](./alerts-enable-template.md) , pozostaną dostępne do końca sierpnia 2019. 

Na koniec sierpnia 2019 w Azure Monitor:

- Usługa klasycznego monitorowania i alertów zostanie wycofana i nie będzie już dostępna do tworzenia nowych reguł alertów.
- Wszelkie reguły alertów, które nadal istnieją w alertach (klasyczny) poza sierpnia 2019, będą nadal wykonywane i wyzwalane, ale nie będą dostępne do modyfikacji.
- Od września 2019 reguły alertów w ramach klasycznego monitorowania & alertów, które mogą zostać zmigrowane, zostaną automatycznie przeniesione przez firmę Microsoft do ich odpowiedników w nowej platformie Azure monitor w fazach obejmujących kilka tygodni. Proces ten będzie bezproblemowe bez przestojów, a klienci nie będą mogli korzystać z monitorowania.
- Reguły alertów migrowane do nowych platform alertów zapewniają pokrycie monitorowania tak, jak wcześniej, ale będą powiadamiane przy użyciu nowych ładunków. Wszystkie adresy e-mail, punkty końcowe elementu webhook lub łącze aplikacji logiki skojarzone z klasyczną regułą alertów będą przenoszone po migracji, ale mogą nie działać poprawnie, ponieważ obciążenie alertów będzie inne na nowej platformie.
- Niektóre [klasyczne reguły alertów, które nie mogą zostać automatycznie zmigrowane](alerts-understand-migration.md#classic-alert-rules-that-will-not-be-migrated) i wymagają ręcznej akcji od użytkowników, będą nadal uruchamiane do czerwca 2020.

> [!IMPORTANT]
> Monitor Microsoft Azure został rozmieszczony w [narzędziu faz, aby wkrótce przeprowadzić migrację](alerts-using-migration-tool.md) swoich klasycznych reguł alertów na nową platformę. I uruchom ją przez wymuszenie dla wszystkich klasycznych reguł alertów, które nadal istnieją i można migrować, rozpoczynając od września 2019. Klienci będą musieli zapewnić, że Automatyzacja korzystający z klasycznej reguły alertów jest dostosowywana do obsługi nowego ładunku z [ujednoliconych metryk i alertów w Application Insights](#unified-metrics-and-alerts-in-application-insights) lub [ujednolicone metryki i alerty dla innych zasobów platformy Azure](#unified-metrics-and-alerts-for-other-azure-resources), po migracji z klasycznych reguł alertów. Aby uzyskać więcej informacji, zobacz [Przygotowywanie do migracji klasycznej reguły alertów](alerts-prepare-migration.md)

Ten artykuł będzie aktualizowany w sposób ciągły wraz z linkami & szczegóły dotyczące nowej funkcji alertów & monitorowania platformy Azure, a także dostępności narzędzi do ułatwienia użytkownikom wdrażania nowej platformy Azure Monitor.

## <a name="pricing-for-migrated-alert-rules"></a>Cennik dla zmigrowanych reguł alertów

Wdrażamy narzędzie do migracji w celu ułatwienia migracji Azure Monitor [klasycznych alertów](./alerts-classic.overview.md) do nowego środowiska alertów. Zmigrowane reguły alertów i odpowiadające im zmigrowane grupy akcji (poczta e-mail, element webhook lub LogicApp) pozostaną bezpłatne. Funkcja z alertami klasycznymi, w tym możliwość edytowania progu, typu agregacji i stopnia szczegółowości agregacji, będzie nadal dostępna bezpłatnie z zmigrowanymi regułami alertów. Jednak w przypadku edytowania zmigrowanej reguły alertów w celu korzystania z dowolnej z nowych funkcji platformy alertów, powiadomień lub typów akcji zostanie naliczona odpowiednia opłata. Aby uzyskać więcej informacji na temat cen reguł alertów i powiadomień, zobacz [Azure monitor Cennik](https://azure.microsoft.com/pricing/details/monitor/).

Poniżej przedstawiono przykłady sytuacji, w których nastąpi naliczanie opłat za regułę alertu:

- Każda nowa (niemigrowana) reguła alertu utworzona poza wolne jednostki na nowej platformie Azure Monitor
- Wszelkie dane pozyskane i zachowane poza bezpłatnymi jednostkami zawartymi w Azure Monitor
- Wszystkie testy sieci Web wielotestowego wykonywane przez Application Insights
- Wszystkie metryki niestandardowe przechowywane poza bezpłatnymi jednostkami zawartymi w Azure Monitor
- Wszystkie zmigrowane reguły alertów, które są edytowane w celu używania nowszych funkcji alertu metryki, takich jak częstotliwość, wiele zasobów/wymiary, [dynamiczne progi](alerts-dynamic-thresholds.md), zmiana zasobu/sygnał i tak dalej.
- Wszystkie zmigrowane grupy akcji, które są edytowane w celu korzystania z nowszych powiadomień lub typy akcji, takich jak wiadomości SMS, połączenia głosowe i/lub integracja narzędzia ITSM.

## <a name="next-steps"></a>Następne kroki

* Poznaj [nowe ujednolicone Azure monitor](../overview.md).
* Dowiedz się więcej o nowych [alertach platformy Azure](./alerts-overview.md).

