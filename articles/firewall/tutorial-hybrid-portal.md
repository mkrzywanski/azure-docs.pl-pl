---
title: 'Samouczek: wdrażanie i Konfigurowanie zapory platformy Azure w sieci hybrydowej przy użyciu Azure Portal'
description: W tym samouczku dowiesz się, jak wdrożyć i skonfigurować zaporę platformy Azure przy użyciu Azure Portal.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: tutorial
ms.date: 03/24/2020
ms.author: victorh
customer intent: As an administrator, I want to control network access from an on-premises network to an Azure virtual network.
ms.openlocfilehash: 5ba9bb723ab7b052440eea2ac509692200b80f6e
ms.sourcegitcommit: 4ac596f284a239a9b3d8ed42f89ed546290f4128
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/12/2020
ms.locfileid: "84750699"
---
# <a name="tutorial-deploy-and-configure-azure-firewall-in-a-hybrid-network-using-the-azure-portal"></a>Samouczek: wdrażanie i Konfigurowanie zapory platformy Azure w sieci hybrydowej przy użyciu Azure Portal

W przypadku łączenia sieci lokalnej z siecią wirtualną platformy Azure w celu utworzenia sieci hybrydowej ważną częścią ogólnego planu zabezpieczeń jest możliwość kontrolowania dostępu do zasobów sieciowych platformy Azure.

Aby kontrolować dostęp do sieci hybrydowej korzystającej z reguł, które definiują dozwolony i zabroniony ruch sieciowy, możesz użyć usługi Azure Firewall.

W tym samouczku zostaną utworzone trzy sieci wirtualne:

- **VNet-Hub** — w tej sieci wirtualnej znajduje się zapora.
- **VNet-Spoke** — sieć wirtualna będąca szprychą reprezentuje pakiet roboczy na platformie Azure.
- **VNet-Onprem** — lokalna sieć wirtualna reprezentuje sieć lokalną. W rzeczywistym wdrożeniu może ono być połączone przez połączenie sieci VPN lub ExpressRoute. Dla ułatwienia w tym samouczku zostanie wykorzystane połączenie za pośrednictwem bramy VPN Gateway, a do reprezentowania sieci lokalnej zostanie wykorzystana sieć wirtualna zlokalizowana na platformie Azure.

![Zapora w sieci hybrydowej](media/tutorial-hybrid-ps/hybrid-network-firewall.png)

Z tego samouczka dowiesz się, jak wykonywać następujące czynności:

> [!div class="checklist"]
> * Deklarowanie zmiennych
> * Tworzenie sieci wirtualnej koncentratora zapory
> * Tworzenie sieci wirtualnej będącej szprychą
> * Tworzenie lokalnej sieci wirtualnej
> * Konfigurowanie i wdrażanie zapory
> * Tworzenie i łączenie bram sieci VPN
> * Komunikacja równorzędna pomiędzy sieciami wirtualnymi koncentratora i szprychy
> * Tworzenie tras
> * Tworzenie maszyn wirtualnych
> * Testowanie zapory

Jeśli chcesz użyć Azure PowerShell zamiast tego wykonać tę procedurę, zobacz [wdrażanie i Konfigurowanie zapory platformy Azure w sieci hybrydowej przy użyciu Azure PowerShell](tutorial-hybrid-ps.md).

## <a name="prerequisites"></a>Wymagania wstępne

Sieć hybrydowa używa modelu architektury Hub i szprych do kierowania ruchu między usługą Azure sieci wirtualnych i sieciami lokalnymi. Architektura gwiazdy i szprych ma następujące wymagania:

- Ustaw **AllowGatewayTransit** podczas komunikacji równorzędnej między koncentratorem a siecią wirtualną-szprych. W architekturze sieci typu Hub i szprych, tranzyt bramy umożliwia sieciom wirtualnym współużytkowanie bramy sieci VPN w centrum, a nie wdrażanie bram sieci VPN w każdej sieci wirtualnej szprych. 

   Ponadto trasy do sieci wirtualnych podłączonych do bramy lub sieci lokalnych będą automatycznie propagowane do tabel routingu dla równorzędnych sieci wirtualnych przy użyciu tranzytu bramy. Aby uzyskać więcej informacji, zobacz [Konfigurowanie tranzytu bramy sieci VPN dla komunikacji równorzędnej sieci wirtualnych](../vpn-gateway/vpn-gateway-peering-gateway-transit.md).

- Ustaw **useremotegateways o wartości** podczas komunikacji równorzędnej między sieciami wirtualnymi i koncentratorem. Jeśli ustawiono opcję **useremotegateways o wartości** i **AllowGatewayTransit** na zdalnej komunikacji równorzędnej, Sieć wirtualna szprych używa bram zdalnej sieci wirtualnej do tranzytu.
- Aby skierować ruch podsieci szprych przez zaporę koncentratora, można użyć trasy zdefiniowanej przez użytkownika (UDR), która wskazuje zaporę z wyłączoną opcją **propagacji trasy bramy sieci wirtualnej** . Opcja wyłączania **propagacji trasy bramy sieci wirtualnej** uniemożliwia dystrybucję tras do podsieci szprych. Zapobiega to wyznaniom tras spowodowanych konfliktami z UDR. Jeśli chcesz zachować **propagację trasy bramy sieci wirtualnej** , upewnij się, że określone trasy są zdefiniowane dla zapory, aby przesłonić te, które są publikowane z lokalizacji lokalnej za pośrednictwem protokołu BGP.
- Skonfiguruj UDR w podsieci bramy centrum, która wskazuje adres IP zapory w następnym przeskoku do sieci szprych. W podsieci usługi Azure Firewall nie jest wymagana trasa zdefiniowana przez użytkownika, ponieważ uzyskuje ona informacje o trasach na podstawie protokołu BGP.

Zapoznaj się z sekcją [Tworzenie tras](#create-the-routes) w tym samouczku, aby zobaczyć, jak te trasy zostały utworzone.

>[!NOTE]
>Zapora platformy Azure musi mieć bezpośrednią łączność z Internetem. Jeśli AzureFirewallSubnet nauczy trasy domyślnej do sieci lokalnej za pośrednictwem protokołu BGP, należy przesłonić ten element przy użyciu wartości 0.0.0.0/0 UDR z wartością **NextHopType** ustawioną jako **Internet** w celu utrzymania bezpośredniej łączności z Internetem.
>
>Zaporę platformy Azure można skonfigurować do obsługi wymuszonego tunelowania. Aby uzyskać więcej informacji, zobacz [tunelowanie wymuszone przez zaporę platformy Azure](forced-tunneling.md).

>[!NOTE]
>Ruch między wirtualnymi sieciami równorzędnymi połączonymi bezpośrednio jest kierowany bezpośrednio nawet wtedy, gdy trasa zdefiniowana przez użytkownika wskazuje usługę Azure Firewall jako bramę domyślną. Aby w tym scenariuszu wysyłać ruch między podsieciami do zapory, trasa zdefiniowana przez użytkownika musi jawnie zawierać prefiks podsieci docelowej w obu podsieciach.

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="create-the-firewall-hub-virtual-network"></a>Tworzenie sieci wirtualnej koncentratora zapory

Najpierw utwórz grupę zasobów zawierającą zasoby do celów tego samouczka:

1. Zaloguj się do witryny Azure Portal pod adresem [https://portal.azure.com](https://portal.azure.com).
2. Na stronie głównej Azure Portal wybierz pozycję **grupy zasobów**  >  **Dodaj**.
3. W obszarze **Nazwa grupy zasobów**wpisz polecenie **PD-hybrydowy-test**.
4. W polu **Subskrypcja** wybierz subskrypcję.
5. W **obszarze region**wybierz pozycję **Wschodnie stany USA**. Wszystkie zasoby, które tworzysz później, muszą znajdować się w tej samej lokalizacji.
6. Wybierz pozycję **Recenzja + Utwórz**.
7. Wybierz przycisk **Utwórz**.

Teraz Utwórz sieć wirtualną:

> [!NOTE]
> Rozmiar podsieci AzureFirewallSubnet to/26. Aby uzyskać więcej informacji o rozmiarze podsieci, zobacz [często zadawane pytania dotyczące zapory platformy Azure](firewall-faq.md#why-does-azure-firewall-need-a-26-subnet-size).

1. Na stronie głównej Azure Portal wybierz pozycję **Utwórz zasób**.
2. W obszarze **Sieć**wybierz pozycję **Sieć wirtualna**.
4. W obszarze **Nazwa wpisz nazwę** **koncentratora sieci wirtualnej**.
5. W **polu przestrzeń adresowa**wpisz **10.5.0.0/16**.
6. W polu **Subskrypcja** wybierz subskrypcję.
7. W obszarze **Grupa zasobów**wybierz opcję **PD-hybrydowy-test**.
8. W obszarze **Lokalizacja**wybierz pozycję **Wschodnie stany USA**.
9. W obszarze **Podsieci** w polu **Nazwa** wpisz wartość **AzureFirewallSubnet**. Zapora będzie znajdować się w tej podsieci, a nazwą podsieci **musi** być AzureFirewallSubnet.
10. W obszarze **zakres adresów**wpisz **10.5.0.0/26**. 
11. Zaakceptuj inne ustawienia domyślne, a następnie wybierz pozycję **Utwórz**.

## <a name="create-the-spoke-virtual-network"></a>Tworzenie sieci wirtualnej będącej szprychą

1. Na stronie głównej Azure Portal wybierz pozycję **Utwórz zasób**.
2. W obszarze **Sieć**wybierz pozycję **Sieć wirtualna**.
4. W obszarze **Nazwa**wpisz **VNET-szprych**.
5. W **polu przestrzeń adresowa**wpisz **10.6.0.0/16**.
6. W polu **Subskrypcja** wybierz subskrypcję.
7. W obszarze **Grupa zasobów**wybierz opcję **PD-hybrydowy-test**.
8. W polu **Lokalizacja** wybierz tę samą lokalizację, która była używana poprzednio.
9. W obszarze **Podsieć** w polu **Nazwa** wpisz **SN-Workload**.
10. W obszarze **zakres adresów**wpisz **10.6.0.0/24**.
11. Zaakceptuj inne ustawienia domyślne, a następnie wybierz pozycję **Utwórz**.

## <a name="create-the-on-premises-virtual-network"></a>Tworzenie lokalnej sieci wirtualnej

1. Na stronie głównej Azure Portal wybierz pozycję **Utwórz zasób**.
2. W obszarze **Sieć**wybierz pozycję **Sieć wirtualna**.
4. W obszarze **Nazwa**wpisz **VNET-lokalnego**.
5. W polu **Przestrzeń adresowa** wpisz wartość **192.168.0.0/16**.
6. W polu **Subskrypcja** wybierz subskrypcję.
7. W obszarze **Grupa zasobów**wybierz opcję **PD-hybrydowy-test**.
8. W polu **Lokalizacja** wybierz tę samą lokalizację, która była używana poprzednio.
9. W obszarze **podsieć**w polu **Nazwa** wpisz **SN-Corp**.
10. W polu **Zakres adresów** wpisz wartość **192.168.1.0/24**.
11. Zaakceptuj inne ustawienia domyślne, a następnie wybierz pozycję **Utwórz**.

Teraz Utwórz drugą podsieć dla bramy.

1. Na stronie **Sieć wirtualna — lokalnego** wybierz pozycję **podsieci**.
2. Wybierz pozycję **+ podsieć**.
3. W obszarze **Nazwa**wpisz **GatewaySubnet**.
4. W obszarze **zakres adresów (blok CIDR)** wpisz **192.168.2.0/24**.
5. Wybierz przycisk **OK**.

## <a name="configure-and-deploy-the-firewall"></a>Konfigurowanie i wdrażanie zapory

Teraz Wdróż zaporę w sieci wirtualnej centrum zapory.

1. Na stronie głównej Azure Portal wybierz pozycję **Utwórz zasób**.
2. W lewej kolumnie Wybierz pozycję **Sieć**, a następnie wybierz pozycję **Zapora**.
4. Na stronie **Tworzenie zapory** strony skorzystaj z poniższej tabeli, aby skonfigurować zaporę:

   |Ustawienie  |Wartość  |
   |---------|---------|
   |Subskrypcja     |\<your subscription\>|
   |Grupa zasobów     |**PD-test hybrydowy** |
   |Nazwa     |**AzFW01**|
   |Lokalizacja     |Wybierz tę samą lokalizację, której użyto poprzednio|
   |Wybieranie sieci wirtualnej     |**Use Existing** (Użyj istniejącej):<br> **Sieć wirtualna — koncentrator**|
   |Publiczny adres IP     |Utwórz nowy: <br>**Nazwa**  -  **PD-PIP**. |

5. Wybierz pozycję **Przegląd + utwórz**.
6. Przejrzyj podsumowanie, a następnie wybierz pozycję **Utwórz** , aby utworzyć zaporę.

   Wdrożenie może potrwać kilka minut.
7. Po zakończeniu wdrażania przejdź do grupy zasobów Zapora **PD-hybrydowy-test** , a następnie wybierz zaporę **AzFW01** .
8. Zanotuj prywatny adres IP. Użyjesz go później podczas tworzenia trasy domyślnej.

### <a name="configure-network-rules"></a>Konfigurowanie reguł sieci

Najpierw Dodaj regułę sieciową, aby zezwolić na ruch w sieci Web.

1. Na stronie **AzFW01** wybierz pozycję **reguły**.
2. Wybierz kartę **Kolekcja reguł sieci** .
3. Wybierz pozycję **Dodaj kolekcję reguł sieci**.
4. W obszarze **Nazwa**wpisz **RCNet01**.
5. Dla **priorytetu**wpisz **100**.
6. W polu **Akcja** wybierz opcję **Zezwalaj**.
6. W obszarze **reguły**, w polu **Nazwa**wpisz **AllowWeb**.
7. W polu **Protokół** wybierz **TCP**.
8. W obszarze **Typ źródła**wybierz pozycję **adres IP**.
9. W obszarze **Źródło**wpisz **192.168.1.0/24**.
10. W obszarze **adres docelowy**wpisz **10.6.0.0/16**
11. W przypadku **portów docelowych**wpisz **80**.

Teraz Dodaj regułę zezwalającą na ruch RDP.

W drugim wierszu reguły wpisz następujące informacje:

1. **Name**Wpisz **AllowRDP**.
2. W polu **Protokół** wybierz **TCP**.
3. W obszarze **Typ źródła**wybierz pozycję **adres IP**.
4. W obszarze **Źródło**wpisz **192.168.1.0/24**.
5. W obszarze **adres docelowy**wpisz **10.6.0.0/16**
6. W przypadku **portów docelowych**wpisz **3389**.
7. Wybierz pozycję **Dodaj**.

## <a name="create-and-connect-the-vpn-gateways"></a>Tworzenie i łączenie bram sieci VPN

Sieć wirtualna koncentratora i lokalna sieć wirtualna są połączone za pośrednictwem bram sieci VPN.

### <a name="create-a-vpn-gateway-for-the-hub-virtual-network"></a>Tworzenie bramy sieci VPN dla sieci wirtualnej koncentratora

Teraz utwórz bramę sieci VPN dla sieci wirtualnej koncentratora. Konfiguracje połączeń między sieciami wymagają zastosowania wartości RouteBased obiektu VpnType. Tworzenie bramy sieci VPN może potrwać 45 minut lub dłużej, w zależności od wybranej jednostki SKU bramy sieci VPN.

1. Na stronie głównej Azure Portal wybierz pozycję **Utwórz zasób**.
2. W polu tekstowym Wyszukaj wpisz **Brama sieci wirtualnej** i naciśnij klawisz **Enter**.
3. Wybierz pozycję **Brama sieci wirtualnej**, a następnie wybierz pozycję **Utwórz**.
4. W obszarze **Nazwa**wpisz **GW-Hub**.
5. W polu **region**wybierz ten sam region, który był wcześniej używany.
6. W obszarze **Typ bramy**wybierz pozycję **Sieć VPN**.
7. W obszarze **Typ sieci VPN**wybierz pozycję **oparta na trasach**.
8. W obszarze **jednostka SKU**wybierz pozycję **podstawowa**.
9. W obszarze **Sieć wirtualna**wybierz opcję **koncentrator**wirtualny.
10. W obszarze **publiczny adres IP**wybierz pozycję **Utwórz nowy**, a następnie w polu Nazwa wpisz **VNET-Hub-GW-PIP** .
11. Zaakceptuj pozostałe wartości domyślne, a następnie wybierz pozycję **Recenzja + Utwórz**.
12. Przejrzyj konfigurację, a następnie wybierz pozycję **Utwórz**.

### <a name="create-a-vpn-gateway-for-the-on-premises-virtual-network"></a>Tworzenie bramy sieci VPN dla lokalnej sieci wirtualnej

Teraz utwórz bramę sieci VPN dla lokalnej sieci wirtualnej. Konfiguracje połączeń między sieciami wymagają zastosowania wartości RouteBased obiektu VpnType. Tworzenie bramy sieci VPN może potrwać 45 minut lub dłużej, w zależności od wybranej jednostki SKU bramy sieci VPN.

1. Na stronie głównej Azure Portal wybierz pozycję **Utwórz zasób**.
2. W polu tekstowym Wyszukaj wpisz **Brama sieci wirtualnej** i naciśnij klawisz **Enter**.
3. Wybierz pozycję **Brama sieci wirtualnej**, a następnie wybierz pozycję **Utwórz**.
4. W obszarze **Nazwa**wpisz **GW-lokalnego**.
5. W polu **region**wybierz ten sam region, który był wcześniej używany.
6. W obszarze **Typ bramy**wybierz pozycję **Sieć VPN**.
7. W obszarze **Typ sieci VPN**wybierz pozycję **oparta na trasach**.
8. W obszarze **jednostka SKU**wybierz pozycję **podstawowa**.
9. W obszarze **Sieć wirtualna**wybierz pozycję **VNET-lokalnego**.
10. W polu **publiczny adres IP**wybierz pozycję **Utwórz nowy**, a następnie wpisz **VNET-lokalnego-GW-PIP** jako nazwę.
11. Zaakceptuj pozostałe wartości domyślne, a następnie wybierz pozycję **Recenzja + Utwórz**.
12. Przejrzyj konfigurację, a następnie wybierz pozycję **Utwórz**.

### <a name="create-the-vpn-connections"></a>Tworzenie połączeń sieci VPN

Teraz można tworzyć połączenia sieci VPN między centrami centralnymi i lokalnymi.

W tym kroku utworzysz połączenie z sieci wirtualnej koncentratora do lokalnej sieci wirtualnej. W przykładach zastosowano odwołania do klucza współużytkowanego. Możesz wybrać własne wartości dla klucza współużytkowanego. Ważne jest, aby klucz współużytkowany był zgodny z obydwoma połączeniami. Tworzenie połączenia może nieco potrwać.

1. Otwórz grupę zasobów **PD-hybrydowy-test** i wybierz bramę bramy **GW-Hub** .
2. Wybierz pozycję **połączenia** w lewej kolumnie.
3. Wybierz pozycję **Dodaj**.
4. Nazwa połączenia wpisz **Hub-to-lokalnego**.
5. Wybierz pozycję **Sieć wirtualna-sieć wirtualna** dla **typu połączenia**.
6. W przypadku **drugiej bramy sieci wirtualnej**wybierz pozycję **GW-lokalnego**.
7. Dla **klucza współużytkowanego (PSK)** wpisz **AzureA1b2C3**.
8. Wybierz przycisk **OK**.

Utwórz połączenie z lokalnej sieci wirtualnej do sieci wirtualnej koncentratora. Ten krok jest podobny do poprzedniego, jednak w tym przypadku tworzysz połączenie z sieci VNet-Onprem do sieci VNet-hub. Upewnij się, że klucze współużytkowane są zgodne. Po kilku minutach połączenie zostanie ustanowione.

1. Otwórz grupę zasobów **PD-hybrydowy-test** i wybierz bramę **GW-lokalnego** Gateway.
2. Wybierz pozycję **połączenia** w lewej kolumnie.
3. Wybierz pozycję **Dodaj**.
4. Nazwa połączenia wpisz **lokalnego-to-Hub**.
5. Wybierz pozycję **Sieć wirtualna-sieć wirtualna** dla **typu połączenia**.
6. W przypadku **drugiej bramy sieci wirtualnej**wybierz pozycję **GW-Hub**.
7. Dla **klucza współużytkowanego (PSK)** wpisz **AzureA1b2C3**.
8. Wybierz przycisk **OK**.


#### <a name="verify-the-connection"></a>Weryfikowanie połączenia

Po około pięciu minutach lub tak, stan obu połączeń powinien być **połączony**.

![Połączenia bramy](media/tutorial-hybrid-portal/gateway-connections.png)

## <a name="peer-the-hub-and-spoke-virtual-networks"></a>Komunikacja równorzędna pomiędzy sieciami wirtualnymi koncentratora i szprychy

Teraz nawiąż komunikację równorzędną pomiędzy siecią wirtualną koncentratora i siecią wirtualną będącą szprychą.

1. Otwórz grupę zasobów **PD-hybrydowy-test** i wybierz sieć wirtualną **koncentratora** sieci wirtualnej.
2. W lewej kolumnie Wybierz pozycję **Komunikacja równorzędna**.
3. Wybierz pozycję **Dodaj**.
4. W obszarze **Nazwa**wpisz **HubtoSpoke**.
5. Dla **sieci wirtualnej**wybierz opcję Sieć wirtualna **-szprycha**
6. Aby uzyskać nazwę komunikacji równorzędnej z VNetSpoke do koncentratora sieci wirtualnej, wpisz **SpoketoHub**.
7. Wybierz pozycję **Zezwalaj na tranzyt bramy**.
8. Wybierz przycisk **OK**.

### <a name="configure-additional-settings-for-the-spoketohub-peering"></a>Konfigurowanie dodatkowych ustawień komunikacji równorzędnej SpoketoHub

Należy włączyć **ruch przekierowany dalej** w komunikacji równorzędnej SpoketoHub.

1. Otwórz grupę zasobów **PD-hybrydowy-test** i wybierz **sieć wirtualną sieci wirtualnej.**
2. W lewej kolumnie Wybierz pozycję **Komunikacja równorzędna**.
3. Wybierz komunikację równorzędną **SpoketoHub** .
4. W obszarze **Zezwalaj na ruch przesłany z sieci wirtualnej do sieci wirtualnej — szprych**wybierz pozycję **włączone**.
5. Wybierz pozycję **Zapisz**.

## <a name="create-the-routes"></a>Tworzenie tras

Następnie należy utworzyć kilka tras:

- Trasa z podsieci bramy koncentratora do podsieci będącej szprychą za pośrednictwem adresu IP zapory
- Trasa domyślna z podsieci będącej szprychą za pośrednictwem adresu IP zapory

1. Na stronie głównej Azure Portal wybierz pozycję **Utwórz zasób**.
2. W polu tekstowym Wyszukaj wpisz ciąg **Route Table** i naciśnij klawisz **Enter**.
3. Wybierz pozycję **tabela tras**.
4. Wybierz przycisk **Utwórz**.
5. W polu Nazwa wpisz **UDR-Hub-szprych**.
6. Wybierz z grupy zasobów opcję **PD-hybrydowy-test** .
8. W polu **Lokalizacja** wybierz tę samą lokalizację, która była używana poprzednio.
9. Wybierz przycisk **Utwórz**.
10. Po utworzeniu tabeli tras wybierz ją, aby otworzyć stronę tabela tras.
11. W lewej kolumnie Wybierz pozycję **trasy** .
12. Wybierz pozycję **Dodaj**.
13. Dla nazwy trasy wpisz **ToSpoke**.
14. Dla prefiksu adresu wpisz **10.6.0.0/16**.
15. W polu Typ następnego przeskoku wybierz pozycję **urządzenie wirtualne**.
16. W polu adres następnego przeskoku wpisz zanotowany wcześniej prywatny adres IP zapory.
17. Wybierz przycisk **OK**.

Teraz Skojarz trasę z podsiecią.

1. Na stronie **UDR-Hub-szprych-Routes** wybierz pozycję **podsieci**.
2. Wybierz pozycję **Skojarz**.
3. Wybierz pozycję **Wybierz sieć wirtualną**.
4. Wybierz pozycję **Sieć wirtualna — koncentrator**.
5. Wybierz pozycję **GatewaySubnet**.
6. Wybierz przycisk **OK**.

Teraz Utwórz trasę domyślną z podsieci szprych.

1. Na stronie głównej Azure Portal wybierz pozycję **Utwórz zasób**.
2. W polu tekstowym Wyszukaj wpisz ciąg **Route Table** i naciśnij klawisz **Enter**.
3. Wybierz pozycję **tabela tras**.
5. Wybierz przycisk **Utwórz**.
6. W polu Nazwa wpisz **UDR-DG**.
7. Wybierz z grupy zasobów opcję **PD-hybrydowy-test** .
8. W polu **Lokalizacja** wybierz tę samą lokalizację, która była używana poprzednio.
4. W przypadku **propagacji trasy bramy sieci wirtualnej**wybierz pozycję **wyłączone**.
1. Wybierz przycisk **Utwórz**.
2. Po utworzeniu tabeli tras wybierz ją, aby otworzyć stronę tabela tras.
3. W lewej kolumnie Wybierz pozycję **trasy** .
4. Wybierz pozycję **Dodaj**.
5. Dla nazwy trasy wpisz **ToHub**.
6. Dla prefiksu adresu wpisz **0.0.0.0/0**.
7. W polu Typ następnego przeskoku wybierz pozycję **urządzenie wirtualne**.
8. W polu adres następnego przeskoku wpisz zanotowany wcześniej prywatny adres IP zapory.
9. Wybierz przycisk **OK**.

Teraz Skojarz trasę z podsiecią.

1. Na stronie **UDR-DG-Routes** wybierz pozycję **podsieci**.
2. Wybierz pozycję **Skojarz**.
3. Wybierz pozycję **Wybierz sieć wirtualną**.
4. Wybierz pozycję **Sieć wirtualna-szprycha**.
5. Wybierz pozycję **SN-obciążenie**.
6. Wybierz przycisk **OK**.

## <a name="create-virtual-machines"></a>Tworzenie maszyn wirtualnych

Teraz utwórz maszyny wirtualne pakietu roboczego szprychy i sieci lokalnej, a następnie umieść je w odpowiednich podsieciach.

### <a name="create-the-workload-virtual-machine"></a>Tworzenie maszyny wirtualnej pakietu roboczego

Tworzenie maszyny wirtualnej w sieci wirtualnej szprych, uruchamianie usług IIS bez publicznego adresu IP.

1. Na stronie głównej Azure Portal wybierz pozycję **Utwórz zasób**.
2. W obszarze **popularne**wybierz pozycję **Windows Server 2016 Datacenter**.
3. Wprowadź poniższe wartości dla maszyny wirtualnej:
    - **Grupa zasobów** — wybierz opcję **PD-hybrydowy-test**.
    - **Nazwa maszyny wirtualnej**: *VM-szprych-01*.
    - **Region — w** tym samym regionie, który jest używany wcześniej.
    - **Nazwa użytkownika**: *azureuser*.
    - **Hasło**: *Azure123456!*
4. Wybierz pozycję **Dalej: Dyski**.
5. Zaakceptuj wartości domyślne i wybierz pozycję **Dalej: sieć**.
6. Wybierz pozycję Sieć wirtualna **-szprycha** dla sieci wirtualnej, a podsieć jest **obciążeniem SN**.
7. W obszarze **publiczny adres IP**wybierz pozycję **Brak**.
8. W przypadku **publicznych portów ruchu przychodzącego**wybierz opcję **Zezwalaj na wybrane porty**, a następnie wybierz pozycję **http (80)** i **protokół RDP (3389).**
9. Wybierz pozycję **Dalej: Zarządzanie**.
10. W obszarze **Diagnostyka rozruchu**wybierz pozycję **wyłączone**.
11. Wybierz pozycję **Przegląd + Utwórz**, przejrzyj ustawienia na stronie Podsumowanie, a następnie wybierz pozycję **Utwórz**.

### <a name="install-iis"></a>Instalowanie usług IIS

1. W Azure Portal Otwórz Cloud Shell i upewnij się, że jest ustawiony na program **PowerShell**.
2. Uruchom następujące polecenie, aby zainstalować usługi IIS na maszynie wirtualnej i w razie potrzeby zmienić lokalizację:

   ```azurepowershell-interactive
   Set-AzVMExtension `
           -ResourceGroupName FW-Hybrid-Test `
           -ExtensionName IIS `
           -VMName VM-Spoke-01 `
           -Publisher Microsoft.Compute `
           -ExtensionType CustomScriptExtension `
           -TypeHandlerVersion 1.4 `
           -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell      Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}' `
           -Location EastUS
   ```

### <a name="create-the-on-premises-virtual-machine"></a>Tworzenie maszyny wirtualnej w środowisku lokalnym

Jest to maszyna wirtualna, która jest używana do nawiązywania połączenia przy użyciu Pulpit zdalny z publicznym adresem IP. Z tego miejsca nawiążesz następnie połączenie z serwerem lokalnym za pośrednictwem zapory.

1. Na stronie głównej Azure Portal wybierz pozycję **Utwórz zasób**.
2. W obszarze **popularne**wybierz pozycję **Windows Server 2016 Datacenter**.
3. Wprowadź poniższe wartości dla maszyny wirtualnej:
    - **Grupa zasobów** — wybierz pozycję istniejące, a następnie wybierz pozycję **PD-hybrydowy-test**.
    - **Nazwa**  -  maszyny wirtualnej *Maszyna wirtualna — lokalnego*.
    - **Region — w** tym samym regionie, który jest używany wcześniej.
    - **Nazwa użytkownika**: *azureuser*.
    - **Hasło**: *Azure123456!*.
4. Wybierz pozycję **Dalej: Dyski**.
5. Zaakceptuj wartości domyślne i wybierz pozycję **Dalej: sieć**.
6. Wybierz pozycję Sieć wirtualna **— lokalnego** dla sieci wirtualnej, a podsieć to **SN-Corp**.
7. W przypadku **publicznych portów ruchu przychodzącego**wybierz opcję **Zezwalaj na wybrane porty**, a następnie wybierz pozycję **RDP (3389).**
8. Wybierz pozycję **Dalej: Zarządzanie**.
9. W obszarze **Diagnostyka rozruchu**wybierz pozycję **wyłączone**.
10. Wybierz pozycję **Przegląd + Utwórz**, przejrzyj ustawienia na stronie Podsumowanie, a następnie wybierz pozycję **Utwórz**.

## <a name="test-the-firewall"></a>Testowanie zapory

1. Najpierw Zanotuj prywatny adres IP maszyny wirtualnej **VM-szprych-01** .

2. W witrynie Azure Portal połącz się z maszyną wirtualną **VM-Onprem**.
<!---2. Open a Windows PowerShell command prompt on **VM-Onprem**, and ping the private IP for **VM-spoke-01**.

   You should get a reply.--->
3. Otwórz przeglądarkę internetową w obszarze **VM-lokalnego**, a następnie przejdź do http:// \<VM-spoke-01 private IP\> .

   Powinna zostać wyświetlona strona sieci Web **VM-szprych-01** : ![ Strona sieci Web VM-szprych-01](media/tutorial-hybrid-portal/VM-Spoke-01-web.png)

4. Z maszyny wirtualnej **VM-lokalnego** Otwórz pulpit zdalny do **maszyny wirtualnej-szprych-01** z prywatnym adresem IP.

   Połączenie powinno być pomyślne i powinno być możliwe zalogowanie się.

Teraz sprawdzono, że reguły zapory działają:

<!---- You can ping the server on the spoke VNet.--->
- Możesz przeglądać serwer internetowy w sieci wirtualnej będącej szprychą.
- Możesz nawiązać połączenie z serwerem w sieci wirtualnej będącej szprychą, korzystając z protokołu RDP.

Następnie zmień ustawienie akcji kolekcji reguł sieci zapory na **Odmów**, aby sprawdzić, czy reguły zapory działają zgodnie z oczekiwaniami.

1. Wybierz zaporę **AzFW01** .
2. Wybierz pozycję **reguły**.
3. Wybierz kartę **Kolekcja reguł sieci** i wybierz kolekcję reguł **RCNet01** .
4. W obszarze **Akcja**wybierz pozycję **Odmów**.
5. Wybierz pozycję **Zapisz**.

Zamknij wszystkie zdalne pulpity, zanim zaczniesz testować zmienione zasady. Teraz ponownie uruchom testy. Tym razem wszystkie powinny zakończyć się niepowodzeniem.

## <a name="clean-up-resources"></a>Czyszczenie zasobów

Możesz zachować zasoby zapory na potrzeby kolejnego samouczka, a jeśli nie będą już potrzebne, możesz usunąć grupę zasobów **FW-Hybrid-Test**, aby usunąć wszystkie zasoby związane z zaporą.

## <a name="next-steps"></a>Następne kroki

Następnie możesz monitorować dzienniki usługi Azure Firewall.

> [!div class="nextstepaction"]
> [Samouczek: monitorowanie dzienników usługi Azure Firewall](./tutorial-diagnostics.md)
