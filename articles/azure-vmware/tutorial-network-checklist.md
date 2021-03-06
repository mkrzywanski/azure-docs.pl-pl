---
title: 'Samouczek: Lista kontrolna sieci'
description: Wymagania wstępne dotyczące sieci i szczegółowe informacje dotyczące łączności sieciowej i portów sieciowych
ms.topic: tutorial
ms.date: 05/04/2020
ms.openlocfilehash: 42bd579a455c7efe3b8f6e4d4154e687726adb1d
ms.sourcegitcommit: d9cd51c3a7ac46f256db575c1dfe1303b6460d04
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/04/2020
ms.locfileid: "82740150"
---
# <a name="networking-checklist-for-azure-vmware-solution-avs"></a>Lista kontrolna sieci dla rozwiązania Azure VMware (Automatyczna synchronizacja)

Rozwiązanie VMware dla platformy Azure oferuje środowisko chmury prywatnej VMware, które jest dostępne dla użytkowników i aplikacji z lokalnych środowisk lub zasobów opartych na platformie Azure. Łączność jest dostarczana za pośrednictwem usług sieciowych, takich jak Azure ExpressRoute i połączenia sieci VPN, i będzie wymagać określonych zakresów adresów sieciowych i portów zapory na potrzeby włączania usług. Ten artykuł zawiera informacje niezbędne do poprawnego skonfigurowania sieci do pracy z funkcją automatycznej synchronizacji.

W ramach tego samouczka nauczysz się, jak:

> [!div class="checklist"]
> * Wymagania dotyczące łączności sieciowej
> * Usługa DHCP w wersji zaautomatycznej

## <a name="network-connectivity"></a>Łączność sieciowa

Chmura prywatna automatycznej synchronizacji jest połączona z siecią wirtualną platformy Azure przy użyciu połączenia usługi Azure ExpressRoute. Ta wysoka przepustowość, połączenie o małym opóźnieniu pozwala uzyskiwać dostęp do usług uruchomionych w ramach subskrypcji platformy Azure w środowisku chmury prywatnej.

Automatyczna synchronizacja chmur prywatnych wymaga co najmniej bloku `/22` adresów sieciowych CIDR dla podsieci, jak pokazano poniżej. Ta sieć uzupełnia sieci lokalne. Aby można było połączyć się ze środowiskami lokalnymi i sieciami wirtualnymi, musi to być nienakładający się blok adresów sieciowych.

Przykładowy `/22` blok adresów sieciowych CIDR:`10.10.0.0/22`

Podsieci:

| Użycie sieci             | Podsieć | Przykład        |
| ------------------------- | ------ | -------------- |
| Zarządzanie chmurą prywatną            | `/24`    | `10.10.0.0/24`   |
| Sieć vMotion       | `/24`    | `10.10.1.0/24`   |
| Obciążenia maszyn wirtualnych | `/24`   | `10.10.2.0/24`   |
| Komunikacja równorzędna ExpressRoute | `/24`    | `10.10.3.8/30`   |

### <a name="network-ports-required-to-communicate-with-the-service"></a>Porty sieciowe wymagane do komunikowania się z usługą

Element źródłowy|Element docelowy|Protocol (Protokół) |Port |Opis  |Komentarz
---|-----|:-----:|:-----:|-----|-----
Serwer DNS w chmurze prywatnej  |Lokalny serwer DNS  |UDP |53|Klient DNS — żądania przesyłane dalej z komputera vCenter do wszelkich lokalnych zapytań DNS (Sprawdź poniższą sekcję DNS) |
Lokalny serwer DNS  |Serwer DNS w chmurze prywatnej  |UDP |53|Klient DNS — żądania przesyłane dalej z usług lokalnych do serwerów DNS w chmurze prywatnej (Sprawdź poniższą sekcję DNS)
Sieć lokalna  |Prywatny serwer vCenter w chmurze  |TCP (HTTP)  |80|vCenter Server wymaga portu 80 dla bezpośrednich połączeń HTTP.Port 80 przekierowuje żądania do portu HTTPS 443. To przekierowanie jest pomocne, jeśli `http://server` używasz zamiast `https://server`.  <br><br>Usługa WS-Management (wymaga również otwarcia portu 443) <br><br>Jeśli używasz niestandardowej bazy danych Microsoft SQL, a nie bazy danych SQL Server 2008 na vCenter Server, port 80 jest używany przez usługi SQL Reporting. Po zainstalowaniu vCenter Server Instalator będzie monitował o zmianę portu HTTP dla vCenter Server. Aby zapewnić pomyślną instalację, zmień port HTTP vCenter Server na wartość niestandardową.Program Microsoft Internet Information Services (IIS) również używa portu 80. Zobacz konflikt między vCenter Server i IIS dla portu 80.
Sieć zarządzania chmurą prywatną |Lokalna usługa Active Directory  |TCP  |389|Ten port musi być otwarty na lokalnym i wszystkich zdalnych wystąpieniach vCenter Server. Ten port jest numerem portu LDAP dla usług katalogowych dla grupy vCenter Server. System vCenter Server należy powiązać z portem 389, nawet jeśli to wystąpienie vCenter Server nie jest przyłączane do grupy trybu połączonego. Jeśli na tym porcie jest uruchomiona inna usługa, może być zalecane jej usunięcie lub zmianę portu na inny port. Usługę LDAP można uruchomić na dowolnym porcie z prze1025 do 65535.Jeśli to wystąpienie pełni rolę Active Directory Microsoft Windows, Zmień numer portu z 389 na dostępny port z 1025 do 65535. Ten port jest opcjonalny — w celu skonfigurowania lokalnej usługi AD jako źródła tożsamości w chmurze prywatnej vCenter
Sieć lokalna  |Prywatny serwer vCenter w chmurze  |TCP (HTTPS)  |443|Ten port umożliwia dostęp do programu vCenter z sieci lokalnej.Domyślny port wykorzystywany przez system vCenter Server do nasłuchiwania połączeń z klienta vSphere. Aby umożliwić systemowi vCenter Server odbierania danych z klienta vSphere, otwórz port 443 w zaporze. System vCenter Server używa również portu 443 do monitorowania transferu danych z klientów SDK.Ten port jest również używany dla następujących usług: WS-Management (wymaga to również otwarcia portu 80). vSphere dostęp klienta do programu vSphere Update Manager. Połączenia klienta zarządzania siecią innej firmy z usługą vCenter Server. Klienci zarządzania siecią innych firm uzyskują dostęp do hostów. 
Przeglądarka internetowa  | Hybrydowy Menedżer chmury  | TCP (https) | 9443 | Interfejs zarządzania urządzeniami wirtualnymi w chmurze hybrydowej dla konfiguracji systemu hybrydowego programu Cloud Manager.
Sieć administracyjna  | Hybrydowy Menedżer chmury |SSH |22| Dostęp SSH do administratora do hybrydowego Menedżera chmurowego.
HCM |   Brama chmury|TCP (HTTPS) |8123| Wyślij instrukcje usługi replikacji opartej na hoście do bramy chmury hybrydowej.
HCM |   Brama chmury  |    HTTP TCP (HTTPS) |    9443  |  Wyślij instrukcje dotyczące zarządzania do lokalnej bramy chmury hybrydowej przy użyciu interfejsu API REST.
Brama chmury|      L2C |    TCP (HTTP)  |   443 |   Wyślij instrukcje dotyczące zarządzania z bramy Cloud Gateway do L2C, gdy L2C używa tej samej ścieżki co Brama chmury hybrydowej.
Brama chmury |     Hosty ESXi |     TCP |    80 902  |   Zarządzanie i wdrażanie OVF
Brama w chmurze (lokalna)|     Brama chmury (zdalna)|     UDP  |   4500 | Wymagane w przypadku protokołu IPSEC<br>   Internet Key Exchange (IKEv2) do hermetyzacji obciążeń dla tunelu dwukierunkowego. Translacja adresów sieciowych — przechodzenie (NAT-T) jest również obsługiwane.
Brama w chmurze (lokalna)  |   Brama chmury (zdalna)  |   UDP  |   500   | Wymagane w przypadku protokołu IPSEC<br> Internet Key Exchange (ISAKMP) dla tunelu dwukierunkowego.
Lokalna sieć vCenter|      Sieć zarządzania chmurą prywatną|      TCP  |    8000    |  vMotion maszyn wirtualnych z lokalnego programu vCenter do chmury prywatnej — vCenter   |     

## <a name="dhcp"></a>DHCP

Aplikacje i obciążenia działające w środowisku chmury prywatnej wymagają rozpoznawania nazw i usług DHCP do wyszukiwania i przypisywania adresów IP. Aby zapewnić te usługi, wymagana jest właściwa infrastruktura DHCP i DNS. Można skonfigurować maszynę wirtualną, aby udostępnić te usługi w środowisku chmury prywatnej.  
Zaleca się używanie usługi DHCP, która jest wbudowana w NSX lub użycie lokalnego serwera DHCP w chmurze prywatnej zamiast routingu ruchu DHCP z powrotem do sieci lokalnej.

W tym samouczku przedstawiono następujące informacje:

> [!div class="checklist"]
> * Wymagania dotyczące łączności sieciowej
> * Usługa DHCP w wersji zaautomatycznej

## <a name="next-steps"></a>Następne kroki

Po umieszczeniu odpowiedniej sieci w miejscu przejdź do następnego samouczka, aby utworzyć chmurę prywatną do automatycznej synchronizacji.

> [!div class="nextstepaction"]
> [Samouczek: Tworzenie chmury prywatnej do automatycznej synchronizacji](tutorial-create-private-cloud.md)
