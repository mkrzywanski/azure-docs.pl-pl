---
title: Często zadawane pytania dotyczące platformy Azure Network Watcher | Microsoft Docs
description: Ten artykuł zawiera odpowiedzi na często zadawane pytania dotyczące usługi Azure Network Watcher.
services: network-watcher
documentationcenter: na
author: damendo
manager: ''
editor: ''
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/10/2019
ms.author: damendo
ms.openlocfilehash: b48aab918b477f5c689a50ca476b0b1336642f0f
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "77471860"
---
# <a name="frequently-asked-questions-faq-about-azure-network-watcher"></a>Często zadawane pytania dotyczące usługi Azure Network Watcher
Usługa [Azure Network Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) udostępnia zestaw narzędzi do monitorowania, diagnozowania, wyświetlania metryk i włączania i wyłączania dzienników dla zasobów w sieci wirtualnej platformy Azure. W tym artykule znajdują się odpowiedzi na często zadawane pytania dotyczące usługi.

## <a name="general"></a>Ogólne

### <a name="what-is-network-watcher"></a>Co to jest usługa Network Watcher?
Network Watcher służy do monitorowania i naprawiania kondycji sieci składników IaaS (infrastruktura jako usługa), w tym Virtual Machines, sieci wirtualnych, bram aplikacji, modułów równoważenia obciążenia i innych zasobów w sieci wirtualnej platformy Azure. Nie jest to rozwiązanie do monitorowania infrastruktury PaaS (platforma jako usługa) ani pobierania analiz sieci Web i aplikacji mobilnych.

### <a name="what-tools-does-network-watcher-provide"></a>Jakie narzędzia udostępnia Network Watcher?
Network Watcher oferuje trzy główne zestawy możliwości
* Monitorowanie
  * [Widok topologii](https://docs.microsoft.com/azure/network-watcher/view-network-topology) przedstawia zasoby w sieci wirtualnej i relacje między nimi.
  * [Monitor połączeń](https://docs.microsoft.com/azure/network-watcher/connection-monitor) umożliwia monitorowanie łączności i opóźnień między maszyną wirtualną a innym zasobem sieciowym.
  * [Monitor wydajności sieci](https://docs.microsoft.com/azure/azure-monitor/insights/network-performance-monitor) umożliwia monitorowanie połączeń i opóźnień w różnych architekturach sieci hybrydowych, obwodach usługi ExpressRoute oraz punktach końcowych usług i aplikacji.  
* Diagnostyka
  * [Weryfikacja przepływu IP](https://docs.microsoft.com/azure/network-watcher/network-watcher-ip-flow-verify-overview) pozwala wykrywać problemy z filtrowaniem ruchu na poziomie maszyny wirtualnej.
  * [Następny przeskok](https://docs.microsoft.com/azure/network-watcher/network-watcher-next-hop-overview) pomaga weryfikować trasy ruchu i wykrywać problemy z routingiem.
  * [Rozwiązywanie problemów z połączeniami](https://docs.microsoft.com/azure/network-watcher/network-watcher-connectivity-portal) umożliwia jednorazowe i jednokrotne Sprawdzanie połączeń między maszyną wirtualną a innym zasobem sieciowym.
  * [Przechwytywanie pakietów](https://docs.microsoft.com/azure/network-watcher/network-watcher-packet-capture-overview) umożliwia Przechwytywanie całego ruchu na maszynie wirtualnej w sieci wirtualnej.
  * [Rozwiązywanie problemów z siecią VPN](https://docs.microsoft.com/azure/network-watcher/network-watcher-troubleshoot-overview) powoduje uruchomienie wielu testów diagnostycznych na BRAMACH sieci VPN i połączeniach w celu ułatwienia debugowania problemów.
* Rejestrowanie
  * [Dzienniki przepływu sieciowej grupy zabezpieczeń](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview) pozwalają rejestrować cały ruch w [sieciowych grupach zabezpieczeń (sieciowych grup zabezpieczeń)](https://docs.microsoft.com/azure/virtual-network/security-overview)
  * [Analiza ruchu](https://docs.microsoft.com/azure/network-watcher/traffic-analytics) przetwarza dane dziennika przepływu sieciowej grupy zabezpieczeń, dzięki czemu można wizualizować, wysyłać zapytania, analizować i zrozumieć ruch sieciowy.


Aby uzyskać szczegółowe informacje, zobacz [stronę omówienie Network Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview).


### <a name="how-does-network-watcher-pricing-work"></a>Jak działa Network Watcher cena?
Odwiedź [stronę cennika](https://azure.microsoft.com/pricing/details/network-watcher/) Network Watcher składników i ich cen.

### <a name="which-regions-is-network-watcher-supportedavailable-in"></a>Które regiony są Network Watcher obsługiwane/dostępne w?
Najnowszą dostępność regionalną można wyświetlić na [stronie dostępności usługi platformy Azure](https://azure.microsoft.com/global-infrastructure/services/?products=network-watcher)

### <a name="which-permissions-are-needed-to-use-network-watcher"></a>Które uprawnienia są konieczne do korzystania z Network Watcher?
Zapoznaj się z listą [uprawnień RBAC wymaganych do korzystania z Network Watcher](https://docs.microsoft.com/azure/network-watcher/required-rbac-permissions). W przypadku wdrażania zasobów wymagane są uprawnienia współautora do NetworkWatcherRG (patrz poniżej).

### <a name="how-do-i-enable-network-watcher"></a>Jak mogę włączyć usługę Network Watcher?
Usługa Network Watcher jest [włączona automatycznie](https://azure.microsoft.com/updates/azure-network-watcher-will-be-enabled-by-default-for-subscriptions-containing-virtual-networks/) dla każdej subskrypcji.

### <a name="what-is-the-network-watcher-deployment-model"></a>Co to jest Network Watcher model wdrażania?
Network Watcher zasób nadrzędny jest wdrażany z unikatowym wystąpieniem w każdym regionie. Format nazewnictwa: NetworkWatcher_RegionName. Przykład: NetworkWatcher_centralus jest zasobem Network Watcher dla regionu "środkowe stany USA".

### <a name="what-is-the-networkwatcherrg"></a>Co to jest NetworkWatcherRG?
Zasoby Network Watcher znajdują się w ukrytej grupie zasobów **NetworkWatcherRG** , która jest tworzona automatycznie. Na przykład zasób dzienniki przepływu sieciowej grupy zabezpieczeń jest zasobem podrzędnym Network Watcher i jest włączony w NetworkWatcherRG.

### <a name="why-do-i-need-to-install-the-network-watcher-extension"></a>Dlaczego należy zainstalować rozszerzenie Network Watcher? 
Rozszerzenie Network Watcher jest wymagane dla każdej funkcji, która musi generować lub przechwytywać ruch z maszyny wirtualnej. 

### <a name="which-features-require-the-network-watcher-extension"></a>Które funkcje wymagają rozszerzenia Network Watcher?
Funkcja przechwytywania pakietów, rozwiązywanie problemów z połączeniami i monitorem połączeń wymaga obecności rozszerzenia Network Watcher.

### <a name="what-are-resource-limits-on-network-watcher"></a>Co to są limity zasobów na Network Watcher?
Wszystkie limity można znaleźć na stronie [limity usługi](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#network-watcher-limits) .  

### <a name="why-is-only-one-instance-of-network-watcher-allowed-per-region"></a>Dlaczego jest dozwolone tylko jedno wystąpienie Network Watcher na region? 
Aby subskrypcja usługi mogła działać, Network Watcher należy ją włączyć, ponieważ nie jest to limit usług.

### <a name="how-can-i-manage-the-network-watcher-resource"></a>Jak mogę zarządzać zasobem Network Watcher? 
Zasób Network Watcher reprezentuje usługę zaplecza dla Network Watcher i jest w pełni zarządzana przez platformę Azure. Klienci nie muszą zarządzać nimi. Operacje, takie jak Move, nie są obsługiwane dla zasobu. Można jednak [usunąć zasób](https://docs.microsoft.com/azure/network-watcher/network-watcher-create#delete-a-network-watcher-in-the-portal). 

## <a name="nsg-flow-logs"></a>Dzienniki przepływu sieciowej grupy zabezpieczeń

### <a name="what-does-nsg-flow-logs-do"></a>Co robią dzienniki przepływu sieciowej grupy zabezpieczeń?
Zasoby sieciowe platformy Azure można łączyć i zarządzać nimi za pomocą [sieciowych grup zabezpieczeń (sieciowych grup zabezpieczeń)](https://docs.microsoft.com/azure/virtual-network/security-overview). Dzienniki przepływu sieciowej grupy zabezpieczeń umożliwiają rejestrowanie 5-informacje o przepływie krotki na cały ruch przez sieciowych grup zabezpieczeń. Dzienniki nieprzetworzonych przepływów są zapisywane na koncie usługi Azure Storage, z poziomu którego mogą być przetwarzane, analizowane, badane lub eksportowane zgodnie z wymaganiami.

### <a name="how-do-i-use-nsg-flow-logs-with-a-storage-account-behind-a-firewall"></a>Jak mogę użyć dzienników przepływu sieciowej grupy zabezpieczeń z kontem magazynu za zaporą?

Aby użyć konta magazynu za zaporą, musisz podać wyjątek dla zaufanych usług firmy Microsoft, aby uzyskać dostęp do konta magazynu:

* Przejdź do konta magazynu, wpisując nazwę konta magazynu w wyszukiwaniu globalnym w portalu lub na [stronie konta magazynu](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResource/resourceType/Microsoft.Storage%2FStorageAccounts) .
* W sekcji **USTAWIENIA** wybierz pozycję **Zapory i sieci wirtualne**
* W obszarze "Zezwalaj na dostęp z" Wybierz pozycję **wybrane sieci**. Następnie w obszarze **wyjątki**zaznacz pole wyboru obok pozycji **"Zezwalaj zaufanym usługom firmy Microsoft na dostęp do tego konta magazynu"** . 
* Jeśli jest ona już zaznaczona, nie trzeba wprowadzać żadnych zmian.  
* Znajdź swój docelowy sieciowej grupy zabezpieczeń na [stronie Przegląd dzienników przepływów sieciowej grupy zabezpieczeń](https://ms.portal.azure.com/#blade/Microsoft_Azure_Network/NetworkWatcherMenuBlade/flowLogs) i Włącz dzienniki przepływu sieciowej grupy zabezpieczeń z wybranym powyższym kontem magazynu.

Możesz sprawdzić dzienniki magazynu po kilku minutach — powinna być widoczna zaktualizowana sygnatura czasowa lub utworzony nowy plik JSON.

### <a name="how-do-i-use-nsg-flow-logs-with-a-storage-account-behind-a-service-endpoint"></a>Jak mogę użyć dzienników przepływu sieciowej grupy zabezpieczeń z kontem magazynu za punktem końcowym usługi?

Dzienniki przepływu sieciowej grupy zabezpieczeń są zgodne z punktami końcowymi usługi, bez konieczności dodatkowej konfiguracji. Zapoznaj się z [samouczkiem dotyczącym włączania punktów końcowych usługi](https://docs.microsoft.com/azure/virtual-network/tutorial-restrict-network-access-to-resources#enable-a-service-endpoint) w sieci wirtualnej.


### <a name="what-is-the-difference-between-flow-logs-versions-1--2"></a>Jaka jest różnica między dziennikami przepływów w wersjach 1 & 2?
Dzienniki przepływów w wersji 2 wprowadzają koncepcję *stanu przepływu* & przechowuje informacje o transmitowanych bajtach i pakietach. [Przeczytaj więcej](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview#log-file).

## <a name="next-steps"></a>Następne kroki
 - Przejdź do [strony omówienia dokumentacji](https://docs.microsoft.com/azure/network-watcher/) dotyczącej niektórych samouczków, aby rozpocząć pracę z Network Watcher.
