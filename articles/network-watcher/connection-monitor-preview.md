---
title: Monitor połączeń (wersja zapoznawcza) | Microsoft Docs
description: Dowiedz się, jak używać monitora połączeń (wersja zapoznawcza) do monitorowania komunikacji sieciowej w środowisku rozproszonym.
services: network-watcher
documentationcenter: na
author: vinynigam
manager: agummadi
editor: ''
tags: azure-resource-manager
ms.service: network-watcher
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/27/2020
ms.author: vinigam
ms.custom: mvc
ms.openlocfilehash: 0cb51cd224145e7fe359e2b14a87ed2b87b18c26
ms.sourcegitcommit: 97a0d868b9d36072ec5e872b3c77fa33b9ce7194
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/04/2020
ms.locfileid: "87563028"
---
# <a name="network-connectivity-monitoring-with-connection-monitor-preview"></a>Monitorowanie łączności sieciowej z monitorem połączeń (wersja zapoznawcza)

Monitor połączeń (wersja zapoznawcza) zapewnia ujednolicone kompleksowe monitorowanie połączeń w usłudze Azure Network Watcher. Funkcja monitor połączeń (wersja zapoznawcza) obsługuje wdrożenia hybrydowe i chmurowe platformy Azure. Network Watcher udostępnia narzędzia do monitorowania, diagnozowania i wyświetlania metryk związanych z łącznością dla wdrożeń platformy Azure.

Poniżej przedstawiono niektóre przypadki użycia monitora połączeń (wersja zapoznawcza):

- Maszyna wirtualna serwera frontonu sieci Web komunikuje się z maszyną wirtualną serwera bazy danych w aplikacji wielowarstwowej. Chcesz sprawdzić łączność sieciową między dwiema maszynami wirtualnymi.
- Chcesz, aby maszyny wirtualne w regionie Wschodnie stany USA mogli wysyłać polecenia ping do maszyn wirtualnych w regionie Środkowe stany USA, a chcesz porównać opóźnienia sieci między regionami.
- Masz wiele lokalnych witryn biurowych w Seattle, Waszyngton i w Ashburn, Wirginia. Twoje witryny pakietu Office łączą się z adresami URL pakietu Office 365. Dla użytkowników adresów URL pakietu Office 365 Porównaj opóźnienia między Seattle i Ashburn.
- Aplikacja hybrydowa wymaga połączenia z punktem końcowym usługi Azure Storage. Lokacja lokalna i aplikacja platformy Azure nawiązują połączenie z tym samym punktem końcowym usługi Azure Storage. Chcesz porównać opóźnienia lokacji lokalnej z opóźnieniami aplikacji platformy Azure.
- Chcesz sprawdzić łączność między konfiguracjami lokalnymi i maszynami wirtualnymi platformy Azure, które obsługują aplikację w chmurze.

W fazie zapoznawczej monitor połączenia łączy najlepsze dwie funkcje: funkcja [monitor połączeń](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview#monitor-communication-between-a-virtual-machine-and-an-endpoint) Network Watcher i funkcja [monitor łączności usługi](https://docs.microsoft.com/azure/azure-monitor/insights/network-performance-monitor-service-connectivity) Network Performance Monitor (npm).

Poniżej przedstawiono niektóre zalety usługi Connection monitor (wersja zapoznawcza):

* Ujednolicone, intuicyjne środowisko dla platform Azure i wymagania dotyczące monitorowania hybrydowego
* Międzyregionowe Monitorowanie łączności między obszarami roboczymi
* Wyższe częstotliwości sondowania i lepszy wgląd w wydajność sieci
* Szybsze generowanie alertów dla wdrożeń hybrydowych
* Obsługa sprawdzeń łączności opartych na protokole HTTP, TCP i ICMP 
* Metryki i Log Analytics obsługujące konfiguracje testów platformy Azure i innych niż platformy Azure

![Diagram przedstawiający sposób interakcji monitora połączeń z maszynami wirtualnymi platformy Azure, hostami spoza platformy Azure, punktami końcowymi i lokalizacjami przechowywania danych](./media/connection-monitor-2-preview/hero-graphic.png)

Aby rozpocząć korzystanie z narzędzia Monitor połączeń (wersja zapoznawcza) do monitorowania, wykonaj następujące kroki: 

1. Zainstaluj agentów monitorowania.
1. Włącz Network Watcher w ramach subskrypcji.
1. Utwórz monitor połączeń.
1. Konfigurowanie analizy danych i alertów.
1. Diagnozuj problemy w sieci.

Poniższe sekcje zawierają szczegółowe informacje dotyczące tych kroków.

## <a name="install-monitoring-agents"></a>Zainstaluj agentów monitorowania

Monitor połączeń opiera się na lekkich plikach wykonywalnych w celu uruchomienia kontroli łączności.  Obsługuje ona sprawdzanie łączności ze środowiskami platformy Azure i środowiskami lokalnymi. Plik wykonywalny, którego używasz, zależy od tego, czy maszyna wirtualna jest hostowana na platformie Azure, czy lokalnie.

### <a name="agents-for-azure-virtual-machines"></a>Agenci usługi Azure Virtual Machines

Aby monitor połączeń rozpoznawał maszyny wirtualne platformy Azure jako źródła monitorowania, należy zainstalować na nich rozszerzenie maszyny wirtualnej agenta Network Watcher. To rozszerzenie jest również znane jako *rozszerzenie Network Watcher*. Usługa Azure Virtual Machines wymaga rozszerzenia, aby wyzwolić kompleksowe monitorowanie i inne zaawansowane funkcje. 

Rozszerzenie Network Watcher można zainstalować podczas [tworzenia maszyny wirtualnej](https://docs.microsoft.com/azure/network-watcher/connection-monitor#create-the-first-vm). Można również oddzielnie instalować, konfigurować i rozwiązywać problemy z rozszerzeniem Network Watcher dla systemów [Linux](https://docs.microsoft.com/azure/virtual-machines/extensions/network-watcher-linux) i [Windows](https://docs.microsoft.com/azure/virtual-machines/extensions/network-watcher-windows).

Reguły sieciowej grupy zabezpieczeń (sieciowej grupy zabezpieczeń) lub zapory mogą blokować komunikację między lokalizacją źródłową i docelową. Monitor połączeń wykrywa ten problem i wyświetla go jako komunikat diagnostyczny w topologii. Aby włączyć monitorowanie połączeń, upewnij się, że reguły sieciowej grupy zabezpieczeń i zapory zezwalają na pakiety przez TCP lub ICMP między źródłem a miejscem docelowym.

### <a name="agents-for-on-premises-machines"></a>Agenci dla maszyn lokalnych

Aby monitor połączeń mógł rozpoznać maszyny lokalne jako źródła monitorowania, Zainstaluj agenta Log Analytics na komputerach. Następnie Włącz Network Performance Monitor rozwiązanie. Ci agenci są połączeni z obszarami roboczymi Log Analytics, więc musisz skonfigurować identyfikator obszaru roboczego i klucz podstawowy, aby umożliwić agentom rozpoczęcie monitorowania.

Aby zainstalować agenta Log Analytics dla maszyn z systemem Windows, zobacz [Azure monitor rozszerzenie maszyny wirtualnej dla systemu Windows](https://docs.microsoft.com/azure/virtual-machines/extensions/oms-windows).

Jeśli ścieżka zawiera zapory lub wirtualne urządzenia sieciowe (urządzeń WUS), upewnij się, że lokalizacja docelowa jest osiągalna.

## <a name="enable-network-watcher-on-your-subscription"></a>Włącz Network Watcher w ramach subskrypcji

Wszystkie subskrypcje z siecią wirtualną są włączane przy użyciu Network Watcher. Podczas tworzenia sieci wirtualnej w ramach subskrypcji Network Watcher jest automatycznie włączana w regionie i subskrypcji sieci wirtualnej. To automatyczne włączenie nie wpływa na zasoby ani nie powoduje naliczania opłat. Upewnij się, że Network Watcher nie jest jawnie wyłączona w Twojej subskrypcji. 

Aby uzyskać więcej informacji, zobacz [włączanie Network Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-create).

## <a name="create-a-connection-monitor"></a>Tworzenie monitora połączeń 

Monitor połączeń monitoruje komunikację w regularnych odstępach czasu. Informuje o zmianach w zakresie osiągalności i opóźnień. Możesz również sprawdzić bieżącą i historyczną topologię sieci między agentami źródłowymi i docelowymi punktami końcowymi.

Źródłami mogą być maszyny wirtualne platformy Azure lub maszyny lokalne z zainstalowanym agentem monitorowania. Docelowymi punktami końcowymi mogą być adresy URL pakietu Office 365, adresy URL Dynamics 365, niestandardowe adresy URL, identyfikatory zasobów maszyn wirtualnych platformy Azure, IPv4, IPv6, nazwy FQDN lub dowolna nazwa domeny.

### <a name="access-connection-monitor-preview"></a>Monitor połączenia dostępu (wersja zapoznawcza)

1. Na stronie głównej Azure Portal przejdź do **Network Watcher**.
1. Po lewej stronie w sekcji **monitorowanie** wybierz pozycję **monitor połączeń (wersja zapoznawcza)**.
1. Zobaczysz wszystkie monitory połączeń, które zostały utworzone w monitorze połączeń (wersja zapoznawcza). Aby wyświetlić monitory połączeń, które zostały utworzone w klasycznym środowisku monitora połączeń, przejdź do karty **monitor połączeń** .

    ![Zrzut ekranu przedstawiający monitory połączeń, które zostały utworzone w monitorze połączeń (wersja zapoznawcza)](./media/connection-monitor-2-preview/cm-resource-view.png)


### <a name="create-a-connection-monitor"></a>Tworzenie monitora połączeń

W monitorach połączeń utworzonych w monitorze połączeń (wersja zapoznawcza) można dodawać zarówno maszyny lokalne, jak i maszyny wirtualne platformy Azure jako źródła. Te monitory połączeń mogą również monitorować łączność z punktami końcowymi. Punkty końcowe mogą znajdować się na platformie Azure lub dowolnym innym adresem URL lub adresie IP.

Monitor połączeń (wersja zapoznawcza) zawiera następujące jednostki:

* **Zasób monitora połączeń** — zasób platformy Azure specyficzny dla regionu. Wszystkie poniższe jednostki są właściwościami zasobu monitora połączeń.
* **Endpoint** — Źródło lub miejsce docelowe, które uczestniczy w sprawdzaniu łączności. Przykładowe punkty końcowe obejmują maszyny wirtualne platformy Azure, agentów lokalnych, adresy URL i adresy IP.
* **Konfiguracja testu** — Konfiguracja specyficzna dla protokołu dla testu. Na podstawie wybranego protokołu można zdefiniować port, progi, częstotliwość testów i inne parametry.
* **Grupa testowa** — grupa zawierająca źródłowe punkty końcowe, docelowe punkty końcowe i konfiguracje testów. Monitor połączeń może zawierać więcej niż jedną grupę testową.
* **Test** — połączenie źródłowego punktu końcowego, docelowego punktu końcowego i konfiguracji testu. Test to najbardziej szczegółowy poziom, na którym dane monitorowania są dostępne. Dane monitorowania obejmują procent testów zakończonych niepowodzeniem i czas błądzenia (RTT).

 ![Diagram przedstawiający monitor połączeń, który definiuje relację między grupami testów i testami](./media/connection-monitor-2-preview/cm-tg-2.png)

Podgląd monitora połączeń można utworzyć przy użyciu [Azure Portal](connection-monitor-preview-create-using-portal.md) lub [ARMClient](connection-monitor-preview-create-using-arm-client.md)

Wszystkie źródła, miejsca docelowe i konfiguracje testów dodawane do grupy testowej są podzielone na poszczególne testy. Oto przykład sposobu, w jaki źródła i miejsca docelowe są podzielone:

* Grupa testowa: TG1
* Źródła: 3 (A, B, C)
* Miejsca docelowe: 2 (D, E)
* Konfiguracje testów: 2 (Konfiguracja 1, konfiguracja 2)
* Łączna liczba utworzonych testów: 12

| Numer testu | Element źródłowy | Element docelowy | Konfiguracja testu |
| --- | --- | --- | --- |
| 1 | A | D | Konfiguracja 1 |
| 2 | A | D | Konfiguracja 2 |
| 3 | A | E | Konfiguracja 1 |
| 4 | A | E | Konfiguracja 2 |
| 5 | B | D | Konfiguracja 1 |
| 6 | B | D | Konfiguracja 2 |
| 7 | B | E | Konfiguracja 1 |
| 8 | B | E | Konfiguracja 2 |
| 9 | C | D | Konfiguracja 1 |
| 10 | C | D | Konfiguracja 2 |
| 11 | C | E | Konfiguracja 1 |
| 12 | C | E | Konfiguracja 2 |

### <a name="scale-limits"></a>Limity skalowania

Monitory połączeń mają następujące limity skali:

* Maksymalna liczba monitorów połączeń na subskrypcję na region: 100
* Maksymalna liczba grup testów na monitor połączeń: 20
* Maksymalna liczba źródeł i miejsc docelowych na monitor połączeń: 100
* Maksymalna liczba konfiguracji testu na monitor połączenia: 
    * 20 za pośrednictwem ARMClient
    * 2 za pośrednictwem Azure Portal

## <a name="analyze-monitoring-data-and-set-alerts"></a>Analizowanie danych monitorowania i Ustawianie alertów

Po utworzeniu monitora połączeń źródła sprawdzają łączność z miejscem docelowym na podstawie konfiguracji testu.

### <a name="checks-in-a-test"></a>Sprawdza w teście

W zależności od protokołu, który został wybrany w konfiguracji testu, monitor połączeń (wersja zapoznawcza) uruchamia serię testów dla pary Source-Destination. Kontrole są przeprowadzane zgodnie z wybraną częstotliwością testu.

W przypadku korzystania z protokołu HTTP usługa oblicza liczbę odpowiedzi HTTP, które zwróciły kod odpowiedzi. Wynik określa procent testów zakończonych niepowodzeniem. Aby obliczyć RTT, usługa mierzy czas między wywołaniem HTTP a odpowiedzią.

W przypadku korzystania z protokołu TCP lub ICMP usługa oblicza procent utraty pakietów, aby określić procent niezakończonych testów. Aby obliczyć RTT, usługa mierzy czas potrzebny do otrzymania potwierdzenia (ACK) dla wysłanych pakietów. W przypadku włączenia danych traceroute dla testów sieci można zobaczyć utratę skoku i opóźnienie dla sieci lokalnej.

### <a name="states-of-a-test"></a>Stany testu

Na podstawie danych zwracanych przez testy testy mogą mieć następujące stany:

* Wartości **Pass** — rzeczywiste dla procentu nieudanych testów i RTT znajdują się w określonych progach.
* **Niepowodzenie** — wartości rzeczywiste dla procentu nieudanych testów lub RTT przekroczyły określone progi. Jeśli nie określono progu, test osiągnie stan niepowodzenia, gdy procent testów zakończonych niepowodzeniem wynosi 100.
* **Ostrzeżenie** — nie określono kryteriów dla procentu niezakończonych testów. W przypadku braku określonych kryteriów monitor połączeń (wersja zapoznawcza) automatycznie przypisuje próg. Po przekroczeniu tego progu stan testu zmieni się na ostrzeżenie.

### <a name="data-collection-analysis-and-alerts"></a>Zbieranie danych, analiza i alerty

Dane zbierane przez Monitor połączeń (wersja zapoznawcza) są przechowywane w obszarze roboczym Log Analytics. Ten obszar roboczy jest skonfigurowany podczas tworzenia monitora połączeń. 

Dane monitorowania są również dostępne w metrykach Azure Monitor. Możesz użyć Log Analytics, aby zachować swoje dane monitorowania tak długo, jak chcesz. Azure Monitor przechowuje metryki tylko przez 30 dni. 

[Na podstawie danych można ustawiać alerty oparte na metrykach](https://azure.microsoft.com/blog/monitor-at-scale-in-azure-monitor-with-multi-resource-metric-alerts/).

#### <a name="monitoring-dashboards"></a>Monitorowanie pulpitów nawigacyjnych

Na pulpitach nawigacyjnych monitorowania zostanie wyświetlona lista monitorów połączeń, do których można uzyskać dostęp do subskrypcji, regionów, sygnatur czasowych, źródeł i typów docelowych.

Po przejściu do monitora połączeń (wersja zapoznawcza) z Network Watcher można wyświetlić dane według:

* **Monitor połączeń** — lista wszystkich monitorów połączeń utworzonych dla subskrypcji, regionów, sygnatur czasowych, źródeł i typów docelowych. Ten widok jest domyślny.
* **Grupy testów** — lista wszystkich grup testowych utworzonych dla subskrypcji, regionów, sygnatur czasowych, źródeł i typów docelowych. Te grupy testowe nie są filtrowane według monitorów połączeń.
* **Test** — lista wszystkich testów, które są uruchamiane dla subskrypcji, regionów, sygnatur czasowych, źródeł i typów docelowych. Testy te nie są filtrowane według monitorów połączeń ani grup testowych.

Na poniższej ilustracji trzy widoki danych są wskazywane przez strzałkę 1.

Na pulpicie nawigacyjnym można rozwinąć każdy monitor połączeń, aby wyświetlić jego grupy testowe. Następnie można rozwinąć każdą grupę testową, aby zobaczyć testy, które w niej działają. 

Listę można filtrować na podstawie:

* **Filtry najwyższego poziomu** — wybierz subskrypcje, regiony, źródła sygnatur czasowych i typy docelowe. Zobacz pole 2 na poniższej ilustracji.
* **Filtry oparte na stanie** — Filtruj według stanu monitora połączenia, grupy testowej lub testu. Zobacz strzałkę 3 na poniższej ilustracji.
* **Filtry niestandardowe** — wybierz **pozycję Zaznacz wszystko** , aby wykonać wyszukiwanie ogólne. Aby wyszukać określoną jednostkę, wybierz pozycję z listy rozwijanej. Zobacz strzałkę 4 na poniższej ilustracji.

![Zrzut ekranu przedstawiający sposób filtrowania widoków monitorów połączeń, grup testowych i testów w monitorze połączeń (wersja zapoznawcza)](./media/connection-monitor-2-preview/cm-view.png)

Na przykład aby zobaczyć wszystkie testy w monitorze połączeń (wersja zapoznawcza), gdzie źródłowy adres IP to 10.192.64.56:
1. Zmień widok na **test**.
1. W polu wyszukiwania wpisz *10.192.64.56*
1. Z listy rozwijanej wybierz pozycję **źródła**.

Aby wyświetlić tylko testy zakończone niepowodzeniem w monitorze połączeń (wersja zapoznawcza), gdzie źródłowy adres IP to 10.192.64.56:
1. Zmień widok na **test**.
1. W przypadku filtru opartego na stanie wybierz pozycję **Niepowodzenie**.
1. W polu wyszukiwania wpisz *10.192.64.56*
1. Z listy rozwijanej wybierz pozycję **źródła**.

Aby wyświetlić tylko testy zakończone niepowodzeniem w monitorze połączeń (wersja zapoznawcza), gdzie miejsce docelowe to outlook.office365.com:
1. Zmień widok na **test**.
1. W przypadku filtru opartego na stanie wybierz pozycję **Niepowodzenie**.
1. W polu wyszukiwania wprowadź *Outlook.office365.com*
1. Z listy rozwijanej wybierz pozycję **miejsca docelowe**.

   ![Zrzut ekranu przedstawiający widok, który jest filtrowany do wyświetlania tylko testów zakończonych niepowodzeniem dla miejsca docelowego Outlook.Office365.com](./media/connection-monitor-2-preview/tests-view.png)

Aby wyświetlić trendy w RTT i procent nieudanych testów dla monitora połączeń:
1. Wybierz monitor połączeń, który chcesz zbadać. Domyślnie dane monitorowania są zorganizowane według grupy testowej.

   ![Zrzut ekranu przedstawiający metryki monitora połączeń wyświetlane przez grupę testową](./media/connection-monitor-2-preview/cm-drill-landing.png)

1. Wybierz grupę testową, którą chcesz zbadać.

   ![Zrzut ekranu przedstawiający, gdzie wybrać grupę testową](./media/connection-monitor-2-preview/cm-drill-select-tg.png)

    Zobaczysz pięć pierwszych testów zakończonych niepowodzeniem w grupie testowej w oparciu o RTT lub procent testów zakończonych niepowodzeniem. Dla każdego testu zobaczysz linie RTT i trend dla procentu niezakończonych testów.
1. Wybierz test z listy lub wybierz inny test do zbadania. W przypadku interwału czasowego i procentu nieudanych testów zobaczysz wartości progowe i rzeczywiste. W przypadku czasu RTT zobaczysz wartości progowe, średnie, minimum i maksimum.

   ![Zrzut ekranu przedstawiający wyniki testu dla RTT i procent testów zakończonych niepowodzeniem](./media/connection-monitor-2-preview/cm-drill-charts.png)

1. Zmień przedział czasu, aby wyświetlić więcej danych.
1. Zmień widok, aby wyświetlić źródła, miejsca docelowe lub konfiguracje testowe. 
1. Wybierz źródło na podstawie testów zakończonych niepowodzeniem i zbadaj pięć pierwszych testów zakończonych niepowodzeniem. Na przykład wybierz **Widok według**  >  **źródeł** i **Widok według**  >  **miejsc docelowych** , aby zbadać odpowiednie testy w monitorze połączeń.

   ![Zrzut ekranu przedstawiający metryki wydajności dla pięciu pierwszych testów zakończonych niepowodzeniem](./media/connection-monitor-2-preview/cm-drill-select-source.png)

Aby wyświetlić trendy w RTT i procent nieudanych testów dla grupy testowej:

1. Wybierz grupę testową, którą chcesz zbadać. 

    Domyślnie dane monitorowania są uporządkowane według źródeł, miejsc docelowych i konfiguracji testów (testy). Później można zmienić widok z grup testowych do źródeł, miejsc docelowych lub konfiguracji testów. Następnie wybierz jednostkę, aby zbadać pięć pierwszych testów zakończonych niepowodzeniem. Na przykład Zmień widok na źródła i miejsca docelowe, aby zbadać odpowiednie testy w wybranym monitorze połączeń.
1. Wybierz test, który chcesz zbadać.

   ![Zrzut ekranu przedstawiający miejsce wyboru testu](./media/connection-monitor-2-preview/tg-drill.png)

    Dla danego interwału czasowego i dla procentu nieudanych testów zobaczysz wartości progowe i rzeczywiste wartości. W przypadku czasu RTT widoczne są wartości progowe, średnie, minimum i maksimum. Wyświetlane są również wyzwolone alerty dla wybranego testu.
1. Zmień przedział czasu, aby wyświetlić więcej danych.

Aby wyświetlić trendy w RTT i procent testów zakończonych niepowodzeniem dla testu:
1. Wybierz konfigurację źródłową, docelową i testową, którą chcesz zbadać.

    Dla interwału czasowego i dla procentu nieudanych testów zobaczysz wartości progowe i rzeczywiste wartości. W przypadku czasu RTT widoczne są wartości progowe, średnie, minimum i maksimum. Wyświetlane są również wyzwolone alerty dla wybranego testu.

   ![Zrzut ekranu przedstawiający metryki dla testu](./media/connection-monitor-2-preview/test-drill.png)

1. Aby wyświetlić topologię sieci, wybierz opcję **topologia**.

   ![Zrzut ekranu przedstawiający kartę topologia sieci](./media/connection-monitor-2-preview/test-topo.png)

1. Aby zobaczyć zidentyfikowane problemy, w topologii wybierz dowolny przeskok w ścieżce. (Te przeskoki to zasoby platformy Azure). Ta funkcja nie jest obecnie dostępna dla sieci lokalnych.

   ![Zrzut ekranu przedstawiający wybrany link przeskoku na karcie topologia](./media/connection-monitor-2-preview/test-topo-hop.png)

#### <a name="log-queries-in-log-analytics"></a>Rejestruj zapytania w Log Analytics

Użyj Log Analytics, aby utworzyć niestandardowe widoki danych monitorowania. Wszystkie dane wyświetlane przez interfejs użytkownika znajdują się w Log Analytics. Możesz interaktywnie analizować dane w repozytorium. Skorelowanie danych z Agent Health lub innych rozwiązań opartych na Log Analytics. Wyeksportuj dane do programu Excel lub Power BI lub Utwórz link możliwy do udostępnienia.

#### <a name="metrics-in-azure-monitor"></a>Metryki w usłudze Azure Monitor

W monitorach połączeń utworzonych przed rozpoczęciem korzystania z monitora połączeń (wersja zapoznawcza) są dostępne wszystkie cztery metryki:% sond nie powiodło się, AverageRoundtripMs, ChecksFailedPercent (wersja zapoznawcza) i RoundTripTimeMs (wersja zapoznawcza). W monitorach połączeń utworzonych w środowisku monitor połączeń (wersja zapoznawcza) dane są dostępne tylko dla metryk oznaczonych za pomocą *(wersja zapoznawcza)*.

![Zrzut ekranu przedstawiający metryki w monitorze połączeń (wersja zapoznawcza)](./media/connection-monitor-2-preview/monitor-metrics.png)

Korzystając z metryk, ustaw typ zasobu jako Microsoft. Network/networkWatchers/connectionMonitors

| Metryka | Nazwa wyświetlana | Jednostka | Typ agregacji | Opis | Wymiary |
| --- | --- | --- | --- | --- | --- |
| ProbesFailedPercent | % Sond nie powiodło się | Procent | Średnia | Procent sond monitorowania łączności nie powiódł się. | Brak wymiarów |
| AverageRoundtripMs | Średni czas błądzenia (MS) | ) | Średnia | Średni czas RTT sieci dla sond monitorowania łączności przesyłanych między źródłem a miejscem docelowym. |             Brak wymiarów |
| ChecksFailedPercent (wersja zapoznawcza) | % Sprawdzenia nie powiodło się (wersja zapoznawcza) | Procent | Średnia | Procent testów zakończonych niepowodzeniem dla testu. | ConnectionMonitorResourceId <br>SourceAddress <br>SourceName <br>Identyfikator sourceresourceid <br>SourceType <br>Protokół <br>DestinationAddress <br>Obiekt docelowy <br>DestinationResourceId <br>DestinationType <br>DestinationPort <br>TestGroupName <br>TestConfigurationName <br>Region |
| RoundTripTimeMs (wersja zapoznawcza) | Czas błądzenia (MS) (wersja zapoznawcza) | ) | Średnia | Czas RTT dla czeków wysyłanych między źródłem a miejscem docelowym. Ta wartość nie jest średnia. | ConnectionMonitorResourceId <br>SourceAddress <br>SourceName <br>Identyfikator sourceresourceid <br>SourceType <br>Protokół <br>DestinationAddress <br>Obiekt docelowy <br>DestinationResourceId <br>DestinationType <br>DestinationPort <br>TestGroupName <br>TestConfigurationName <br>Region |

#### <a name="metric-alerts-in-azure-monitor"></a>Alerty metryk w Azure Monitor

Aby utworzyć alert w Azure Monitor:

1. Wybierz zasób monitor połączeń, który został utworzony w monitorze połączeń (wersja zapoznawcza).
1. Upewnij się, że **Metryka** jest wyświetlana jako typ sygnału dla monitora połączenia.
1. W polu **Dodaj warunek**dla **nazwy sygnału**wybierz pozycję **ChecksFailedPercent (wersja zapoznawcza)** lub **RoundTripTimeMs (wersja zapoznawcza)**.
1. W obszarze **Typ sygnału**wybierz pozycję **metryki**. Na przykład wybierz pozycję **ChecksFailedPercent (wersja zapoznawcza)**.
1. Zostaną wyświetlone wszystkie wymiary metryki. Wybierz nazwę wymiaru i wartość wymiaru. Na przykład wybierz pozycję **adres źródłowy** , a następnie wprowadź adres IP dowolnego źródła w monitorze połączenia.
1. W obszarze **logika alertu**podaj następujące informacje:
   * **Typ warunku**: **statyczny**.
   * **Warunek** i **próg**.
   * **Stopień szczegółowości agregacji i częstotliwość oceny**: Monitor połączeń (wersja zapoznawcza) aktualizuje dane co minutę.
1. W obszarze **Akcje**wybierz grupę akcji.
1. Podaj szczegóły alertu.
1. Utwórz regułę alertu.

   ![Zrzut ekranu przedstawiający obszar Tworzenie reguły w Azure Monitor; Wyróżniono "adres źródłowy" i "nazwa punktu końcowego źródła"](./media/connection-monitor-2-preview/mdm-alerts.jpg)

## <a name="diagnose-issues-in-your-network"></a>Diagnozowanie problemów w sieci

Monitor połączeń (wersja zapoznawcza) ułatwia diagnozowanie problemów z monitorem połączeń i siecią. Problemy w sieci hybrydowej są wykrywane przez zainstalowane wcześniej agenci Log Analytics. Problemy na platformie Azure są wykrywane przez rozszerzenie Network Watcher. 

Problemy w sieci platformy Azure można wyświetlić w topologii sieci.

W przypadku sieci, których źródła są lokalnymi maszynami wirtualnymi, można wykryć następujące problemy:

* Upłynął limit czasu żądania.
* Punkt końcowy nie został rozpoznany przez system DNS — tymczasowy lub trwały. Nieprawidłowy adres URL.
* Nie znaleziono hostów.
* Źródło nie może połączyć się z miejscem docelowym. Element docelowy jest nieosiągalny za poorednictwem protokołu ICMP.
* Problemy związane z certyfikatem: 
    * Certyfikat klienta jest wymagany do uwierzytelnienia agenta. 
    * Lista relokacji certyfikatów jest niedostępna. 
    * Nazwa hosta punktu końcowego nie jest zgodna z podmiotem lub alternatywną nazwą podmiotu certyfikatu. 
    * Brak certyfikatu głównego w magazynie zaufanych urzędów certyfikacji komputera lokalnego. 
    * Certyfikat SSL wygasł, nieprawidłowy, odwołany lub niezgodny.

W przypadku sieci, których źródła są maszynami wirtualnymi platformy Azure, można wykryć następujące problemy:

* Problemy z agentem:
    * Agent został zatrzymany.
    * Rozpoznawanie nazw DNS nie powiodło się.
    * Brak aplikacji lub odbiornika nasłuchujących na porcie docelowym.
    * Nie można otworzyć gniazda.
* Problemy ze stanem maszyny wirtualnej: 
    * Uruchamianie
    * Zatrzymywanie
    * Zatrzymano
    * Cofanie przydziału
    * Cofnięto przydział
    * Ponowny rozruch
    * Nie przydzielono
* Brak wpisu tabeli ARP.
* Ruch został zablokowany z powodu lokalnych problemów z zaporą lub reguł sieciowej grupy zabezpieczeń.
* Problemy z bramą sieci wirtualnej: 
    * Brak tras.
    * Tunel między dwoma bramami jest odłączony lub nie istnieje.
    * Druga Brama nie została znaleziona przez tunel.
    * Nie znaleziono informacji o komunikacji równorzędnej.
* Brak trasy w przeglądarce Microsoft Edge.
* Ruch zatrzymany z powodu tras systemowych lub UDR.
* Protokół BGP nie jest włączony dla połączenia bramy.
* Sonda DIP jest wyłączona w module równoważenia obciążenia.
