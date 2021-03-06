---
title: Azure DDoS Protection — Omówienie
description: Dowiedz się, jak standard Azure DDoS Protection, w połączeniu z najlepszymi rozwiązaniami dotyczącymi projektowania aplikacji, zapewnia ochronę przed atakami DDoS.
services: virtual-network
documentationcenter: na
author: KumudD
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/22/2020
ms.author: kumud
ms.openlocfilehash: e92f3b5d8c48c783648285c9a03d8cb8d391a5ac
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/28/2020
ms.locfileid: "87289614"
---
# <a name="azure-ddos-protection-standard-overview"></a>Azure DDoS Protection — Omówienie

Ataki typu „rozproszona odmowa usługi” (Distributed Denial of Service, DDoS) należą do największych obaw związanych z dostępnością i zabezpieczeniami wśród klientów, którzy planują przeniesienie swoich aplikacji do chmury. Atak DDoS próbuje wymusić wyczerpanie zasobów aplikacji, co sprawia, że aplikacja jest niedostępna dla uprawnionych użytkowników. Celem ataku DDoS może być dowolny punkt końcowy publicznie dostępny za pośrednictwem Internetu.

Usługa Azure DDoS Protection, w połączeniu z najlepszymi rozwiązaniami dotyczącymi projektowania aplikacji, zapewnia ochronę przed atakami na DDoS. Usługa Azure DDoS Protection udostępnia następujące warstwy usług:

- **Podstawowa**: automatyczne włączenie w ramach platformy Azure. Zawsze włączone monitorowanie ruchu oraz łagodzenie typowych ataków na poziomie sieci w czasie rzeczywistym zapewnia te same zabezpieczenia wykorzystywane przez Usługi online firmy Microsoft.Cała Skala sieci globalnej platformy Azure może służyć do dystrybucji i ograniczania ruchu ataków między regionami.Zapewniona jest ochrona dla [publicznych adresów IP](virtual-network-public-ip-address.md)platformy Azure IPv4 i IPv6.
- **Standard**: oferuje dodatkowe możliwości ograniczania funkcjonalności w ramach podstawowej warstwy usług, które są dostosowane specjalnie do zasobów usługi Azure Virtual Network. Standard DDoS Protection jest prosty do włączenia i nie wymaga żadnych zmian w aplikacji. Zasady ochrony są dostosowywane za pośrednictwem dedykowanych algorytmów monitorowania ruchu i uczenia maszynowego. Zasady są stosowane do publicznych adresów IP skojarzonych z zasobami wdrożonymi w sieciach wirtualnych, takich jak Azure Load Balancer, Application Gateway platformy Azure i wystąpienia Service Fabric platformy Azure, ale ta ochrona nie dotyczy środowisk App Service.Dane telemetryczne w czasie rzeczywistym są dostępne za pośrednictwem widoków Azure Monitor podczas ataku oraz dla historii. Zaawansowana analiza łagodzenia ataków jest dostępna za pośrednictwem ustawień diagnostycznych. Ochronę warstwy aplikacji można dodać za pośrednictwem [zapory aplikacji sieci Web platformy azure Application Gateway](../application-gateway//application-gateway-web-application-firewall-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) lub przez zainstalowanie zapory innej firmy w witrynie Azure Marketplace. Zapewniona jest ochrona dla [publicznych adresów IP](virtual-network-public-ip-address.md)platformy Azure IPv4 i IPv6.

|Cechy                                         |DDoS Protection podstawowa                 |DDoS Protection Standard                      |
|------------------------------------------------|--------------------------------------|----------------------------------------------|
|Monitorowanie aktywnego ruchu & zawsze włączone wykrywanie |Tak                                   |Tak                                           |
|Automatyczne łagodzenie ataków                    |Tak                                   |Tak                                           |
|Gwarancja dostępności                          |Region świadczenia usługi Azure                          |Aplikacja                                   |
|Zasady ograniczania                             |Dostrojony dla woluminu obszaru ruchu platformy Azure |Dostrojony dla woluminu ruchu aplikacji          |
|Metryki & alerty                                |Nie                                    |Metryki ataku w czasie rzeczywistym & dzienników zasobów za pośrednictwem Azure Monitor                                 |
|Raporty ograniczenia                              |Nie                                    |Publikowanie raportów ograniczenia ataków                |
|Dzienniki przepływów łagodzenia                            |Nie                                    |Strumień dziennika NRT na potrzeby integracji usługi SIEM           |
|Dostosowanie zasad ograniczenia                 |Nie                                    |Zaangażowanie ekspertów DDoS                           |
|Pomoc techniczna                                         |Najlepszy nakład pracy                           |Dostęp do ekspertów DDoS podczas aktywnego ataku|
|Umowa SLA                                             |Region świadczenia usługi Azure                          |Gwarancja aplikacji & kosztów       |
|Cennik                                         |Bezpłatna                                  |Miesięczne użycie &                         |

## <a name="types-of-ddos-attacks-that-ddos-protection-standard-mitigates"></a>Typy ataków DDoS, które DDoS Protection standardowe ograniczenia

Standard DDoS Protection może ograniczyć ataki następujących typów:

- **Ataki pomiarowe**: celem ataku jest zalanie warstwy sieci przy użyciu dużej ilości pozornie wiarygodnego ruchu. Obejmuje to zalewania protokołu UDP, zalewania wzpełnień oraz inne zalewania pakietów sfałszowanych. DDoS Protection Standard ogranicza te potencjalne ataki z obsługą wielodostępności przez przeabsorbowanie i ich kontrolę przy użyciu globalnej skali sieci platformy Azure.
- **Ataki protokołów**: te ataki sprawiają, że element docelowy jest niedostępny, wykorzystując słabą lukę w stosie protokołu warstwy 3 i 4. Obejmuje to ataki, ataki na zalanie się w przypadku zalewania i ataków. DDoS Protection Standard ogranicza te ataki, różnią się od złośliwego i wiarygodnego ruchu, współpracując z klientem i blokując złośliwy ruch. 
- **Ataki warstwowe zasobów (aplikacji)**: te ataki są docelowymi pakietami aplikacji sieci Web, aby zakłócać przekazywanie danych między hostami. Ataki obejmują naruszenia protokołu HTTP, wstrzykiwanie kodu SQL, skrypty między lokacjami i inne ataki warstwy 7. Użyj zapory aplikacji sieci Web, takiej jak [Zapora aplikacji sieci Web](../application-gateway/application-gateway-web-application-firewall-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json)platformy Azure Application Gateway, a także DDoS Protection Standard, aby zapewnić ochronę przed atakami. Istnieją także oferty zapory aplikacji sieci Web innych firm dostępne w [portalu Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps?page=1&search=web%20application%20firewall).

DDoS Protection Standard chroni zasoby w sieci wirtualnej łącznie z publicznymi adresami IP skojarzonymi z maszynami wirtualnymi, modułami równoważenia obciążenia i bramami aplikacji. W połączeniu z zaporą aplikacji sieci Web Application Gateway lub zaporą aplikacji sieci Web innej firmy wdrożoną w sieci wirtualnej z publicznym adresem IP, DDoS Protection Standard może zapewnić pełną możliwość ograniczenia warstwy 3 do warstwy 7.

## <a name="ddos-protection-standard-features"></a>DDoS Protection funkcje standardowe

![Funkcja DDoS](./media/ddos-protection-overview/ddosfeatures.png)

Standardowe funkcje DDoS Protection obejmują:

- **Integracja z platformą natywną:** Natywnie zintegrowane z platformą Azure. Obejmuje konfigurację za pomocą Azure Portal. W standardzie DDoS Protection są poznanie zasobów i konfiguracji zasobów.
- **Ochrona klucza:** Uproszczona konfiguracja natychmiast chroni wszystkie zasoby w sieci wirtualnej zaraz po włączeniu DDoS Protection Standard. Nie jest wymagana żadna interwencja ani definicja użytkownika. Program DDoS Protection Standard natychmiast i automatycznie ogranicza atak po jego wykryciu.
- **Zawsze włączone monitorowanie ruchu:** Wzorce ruchu aplikacji są monitorowane przez 24 godziny dziennie, 7 dni w tygodniu, szukając wskaźników ataków DDoS. Środki zaradcze są wykonywane w przypadku przekroczenia zasad ochrony.
- **Dostrajanie adaptacyjne:** Profilowanie ruchu inteligentnego uzyskuje informacje o ruchu aplikacji w czasie i wybiera i aktualizuje profil, który jest najbardziej odpowiedni dla Twojej usługi. Profil dostosowuje się w miarę zmiany ruchu w czasie.
- **Ochrona wielowarstwowa:** Zapewnia pełną ochronę DDoS stosu, gdy jest używana z zaporą aplikacji sieci Web.
- **Rozbudowana Skala łagodzenia:** Ponad 60 różnych typów ataków można ograniczyć, korzystając z globalnej pojemności, aby chronić przed największym znanymi atakami DDoS.
- **Analiza ataków:** Uzyskaj szczegółowe raporty w ciągu pięciu minut podczas ataku i kompletne podsumowanie po zakończeniu ataku. Dzienniki przepływów ograniczenia przesyłania strumieniowego do systemu informacji o zabezpieczeniach w trybie offline i zarządzania zdarzeniami (SIEM) na potrzeby monitorowania niemal w czasie rzeczywistym podczas ataku.
- **Metryki ataków:** Podsumowania metryk z każdego ataku są dostępne za pośrednictwem Azure Monitor.
- **Alerty ataków:** Alerty można skonfigurować przy uruchamianiu i zatrzymywaniu ataku oraz przez czas trwania ataku przy użyciu wbudowanych metryk ataku. Alerty integrują się z oprogramowaniem operacyjnym, takimi jak dzienniki monitora Microsoft Azure, Splunk, Azure Storage, Poczta E-mail i Azure Portal.
- **Gwarancja kosztów:** Kredyty na korzystanie z usług transferu i skalowanie w poziomie aplikacji dla udokumentowanych ataków DDoS.

## <a name="ddos-protection-standard-mitigation"></a>DDoS Protection ograniczenia standardowego

DDoS Protection monitoruje rzeczywiste wykorzystanie ruchu i stale porównuje go z progami zdefiniowanymi w zasadach DDoS. Po przekroczeniu progu ruchu środki zaradcze DDoS są inicjowane automatycznie. Gdy ruch wraca poniżej wartości progowej, środki zaradcze zostaną usunięte.

![Ograniczanie ryzyka](./media/ddos-protection-overview/mitigation.png)

Podczas rozwiązywania problemów ruch wysyłany do chronionego zasobu jest przekierowywany przez usługę ochrony DDoS i wykonywane jest kilka operacji sprawdzania, takich jak następujące sprawdzenia:

- Upewnij się, że pakiety są zgodne ze specyfikacjami internetowymi i nie są źle sformułowane.
- Współdziałanie z klientem w celu ustalenia, czy ruch jest potencjalnie sfałszowanym pakietem (np. SYN auth lub SYN cookie lub przez porzucanie pakietu dla źródła w celu ponownego przesłania go).
- Oceń liczbę pakietów, jeśli nie można wykonać żadnej innej metody wymuszania.

Usługa DDoS Protection blokuje ruch w ramach ataku, a pozostały ruch przekazuje do miejsca docelowego. W ciągu kilku minut od wykrycia ataku otrzymasz powiadomienie przy użyciu metryk usługi Azure Monitor. Konfigurując funkcję rejestrowania na DDoS Protection standardową telemetrię, możesz napisać dzienniki, aby wyświetlić dostępne opcje dla przyszłej analizy. Dane metryk w Azure Monitor dla DDoS Protection standard są przechowywane przez 30 dni.

Firma Microsoft połączyła się z [chmurą BreakingPoint](https://www.ixiacom.com/products/breakingpoint-cloud) , aby utworzyć interfejs, w którym można generować ruch dla publicznych adresów IP z włączoną obsługą DDoS Protection na potrzeby symulacji. Symulacja chmury w punkcie przerwania umożliwia:

- Sprawdź, jak Microsoft Azure DDoS Protection Standard chroni zasoby platformy Azure przed atakami DDoS
- Optymalizowanie procesu reagowania na zdarzenia w ramach ataku DDoS
- Zgodność dokumentu DDoS
- Uczenie zespołów zabezpieczeń sieci

## <a name="next-steps"></a>Następne kroki

- [Konfigurowanie DDoS Protection Standard](manage-ddos-protection.md)
