---
title: Wymagania dotyczące sieci
description: Wymagania sieciowe i najlepsze praktyki sieciowe umożliwiające optymalne środowisko
author: florianborn71
ms.author: flborn
ms.date: 02/13/2020
ms.topic: reference
ms.openlocfilehash: fe684d15e2ce9b8d302db4eb6bd31dd1416abf21
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "83196546"
---
# <a name="network-requirements"></a>Wymagania dotyczące sieci

Stabilne połączenie sieciowe o małym opóźnieniu z centrum danych platformy Azure ma kluczowe znaczenie dla dobrego środowiska użytkownika w przypadku renderowania zdalnego na platformie Azure. Słabe warunki w sieci mogą spowodować przerwanie połączeń, niestabilne, zakłócenia lub Hologramy, a także zauważalne opóźnienia podczas aktualizowania wykresu sceny po stronie serwera.

## <a name="guidelines-for-network-connectivity"></a>Wskazówki dotyczące łączności sieciowej

Dokładne wymagania dotyczące sieci są zależne od konkretnego przypadku użycia, takich jak liczba i częstotliwość modyfikacji zdalnego wykresu sceny, a także złożoność renderowanego widoku, ale istnieje kilka wytycznych, które zapewniają, że środowisko jest równie dobre:

* Łączność z Internetem musi obsługiwać co najmniej **40 MB/s na poziomie podrzędnym** i **5 MB/s** dla jednej sesji użytkownika na potrzeby renderowania zdalnego na platformie Azure, przy założeniu, że nie istnieje konkurencyjny ruch sieciowy. Zalecamy wyższe stawki za lepsze środowiska. W przypadku większej liczby użytkowników w tej samej sieci te wymagania są skalowane odpowiednio.
* Użycie **pasma Wi-Fi 5 GHz** zwykle daje lepsze wyniki niż pasmo Wi-Fi 2,4 GHz, chociaż obie powinny być wykonane.
* Jeśli w pobliżu istnieją inne sieci Wi-Fi, należy unikać używania kanałów sieci Wi-Fi używanych przez te inne sieci. Możesz użyć narzędzi do skanowania sieci, takich jak [WifiInfoView](https://www.nirsoft.net/utils/wifi_information_view.html) , aby sprawdzić, czy kanały używane przez sieć Wi-Fi są bezpłatne.
* **Należy ściśle unikać korzystania z funkcji powtarzania Wi-Fi** lub przekazywania sieci LAN przez sieć PowerLine.
* W tej samej sieci Wi-Fi nie **należy unikać konkurowania ruchu sieciowego intensywnie wykorzystującego przepustowość** , takiego jak wideo czy przesyłanie strumieniowe.
* Posiadanie **dobrej siły sygnału sieci Wi-Fi** jest niezbędne. Jeśli to możliwe, pozostaw blisko punktu dostępu Wi-Fi i unikaj przeszkód między urządzeniem klienckim a punktami dostępu.
* Upewnij się, że zawsze nawiązujesz połączenie z **najbliższym centrum danych platformy Azure** w Twoim [regionie](regions.md). Im bliżej centrum danych, tym niższe opóźnienie sieci, które ma ogromny wpływ na stabilność z hologramów.

> [!NOTE]
> Przepustowość w trybie podrzędnym jest głównie zużywana przez strumień wideo, który z kolei jest podzielony między informacje o kolorach i głębi (zarówno 60 Hz, stereo).

## <a name="network-performance-tests"></a>Testy wydajności sieci

Jeśli chcesz uzyskać początkowe informacje o tym, czy jakość łączności sieciowej wystarcza do uruchomienia renderowania zdalnego na platformie Azure, dostępne są narzędzia online, których można użyć. Zdecydowanie zalecamy uruchamianie tych narzędzi online z poziomu odpowiednio zaawansowanego laptopa podłączonego do tej samej sieci Wi-Fi, co urządzenie, na którym planujesz uruchomić aplikację kliencką zdalnego renderowania platformy Azure. Wyniki uzyskane z uruchamiania testów na telefonie komórkowym lub HoloLens2 są zwykle mniej użyteczne, ponieważ sprawdzono, aby pokazać znaczną odmianę urządzeń punktu końcowego z niską kontrolą. Lokalizacja, w której umieszcza się laptop powinien znajdować się w tym samym miejscu, w którym oczekuje się, że używasz urządzenia z uruchomioną aplikacją kliencką renderowania zdalnego platformy Azure.

Poniżej przedstawiono kilka prostych kroków służących do szybkiego testowania łączności sieciowej:

1. **Uruchom narzędzie do testowania sieci, takie jak www.speedtest.net, aby uzyskać dane o ogólnym opóźnieniu i przepustowości pasma/podrzędnego połączenia sieciowego.**
Wybierz serwer najbliższy i uruchom test. Serwer nie będzie centrum danych platformy Azure, z którym zostanie nawiązane zdalne renderowanie przy użyciu platformy Azure, a dane wynikowe są nadal przydatne do zrozumienia wydajności połączenia internetowego i sieci Wi-Fi.
   * **Minimalne wymaganie** dla zdalnego renderowania na platformie Azure: około 40 MB/s, nadrzędny i 5 MB/s.
   * **Zalecane** w przypadku renderowania zdalnego na platformie Azure: około 100 MB/s, nadrzędny i 10 MB/s.
Zalecamy przeprowadzenie testu wiele razy i podjęcie najgorszych wyników.
1. **Użyj narzędzia takiego jak www.azurespeed.com, które mierzy opóźnienie w centrach danych platformy Azure**. Wybierz centrum danych platformy Azure obsługiwane przez zdalne renderowanie na platformie Azure, które jest najbliżej siebie (zobacz [Obsługiwane regiony](regions.md)) i uruchom **test opóźnienia**. Jeśli występują zmiany w liczbie widoczne, podawanie wyników trochę czasu.
   * **Minimalne wymaganie** dla zdalnego renderowania na platformie Azure: opóźnienie powinno być stale mniejsze niż 100 ms.
   * **Zalecane** dla zdalnego renderowania na platformie Azure: opóźnienie powinno być stale mniejsze niż 70 MS.

Chociaż małe opóźnienia nie są gwarancją, że zdalne renderowanie na platformie Azure będzie dobrze działać w sieci, zazwyczaj zobaczymy, że te testy zakończyły się pomyślnie.
Jeśli napotykasz artefakty, takie jak niestabilne, zakłócenia lub hologramy przeskoków podczas uruchamiania zdalnego renderowania platformy Azure, zapoznaj się z [przewodnikiem rozwiązywania problemów](../resources/troubleshoot.md).

## <a name="next-steps"></a>Następne kroki

* [Szybki Start: renderowanie modelu przy użyciu aparatu Unity](../quickstarts/render-model.md)
