---
title: Samouczek — Tworzenie bramy i zarządzanie nią za pomocą usługi Azure VPN Gateway
description: Samouczek — tworzenie bramy sieci VPN i zarządzanie nią przy użyciu modułu Azure PowerShell
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: tutorial
ms.date: 03/11/2020
ms.author: cherylmc
ms.openlocfilehash: 66efa0f2922e70908616c7c447d782efee8f6b1b
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/29/2020
ms.locfileid: "79137181"
---
# <a name="tutorial-create-and-manage-a-vpn-gateway-using-powershell"></a>Samouczek: Tworzenie bramy sieci VPN i zarządzanie nią przy użyciu programu PowerShell

Bramy sieci VPN na platformie Azure zapewniają łączność między lokalizacjami klienta a platformą Azure. Ten samouczek obejmuje podstawowe etapy wdrażania bramy sieci VPN, takie jak tworzenie bramy i zarządzanie nią. Omawiane kwestie:

> [!div class="checklist"]
> * Tworzenie bramy sieci VPN
> * Wyświetlanie publicznego adresu IP
> * Zmiana rozmiaru bramy sieci VPN
> * Resetowanie bramy VPN Gateway

Poniższy diagram przedstawia sieć wirtualną oraz bramę sieci VPN utworzoną w ramach tego samouczka.

![Sieć wirtualna i brama sieci VPN](./media/vpn-gateway-tutorial-create-gateway-powershell/vnet1-gateway.png)

### <a name="working-with-azure-cloud-shell-and-azure-powershell"></a>Praca z Azure Cloud Shell i Azure PowerShell

[!INCLUDE [working with cloud shell](../../includes/vpn-gateway-cloud-shell-powershell.md)]

## <a name="common-network-parameter-values"></a>Częste wartości parametrów sieciowych

Poniżej znajdują się wartości parametrów używane w tym samouczku. W przykładach zmienne są tłumaczone na następujące:

```
#$RG1         = The name of the resource group
#$VNet1       = The name of the virtual network
#$Location1   = The location region
#$FESubnet1   = The name of the first subnet
#$BESubnet1   = The name of the second subnet
#$VNet1Prefix = The address range for the virtual network
#$FEPrefix1   = Addresses for the first subnet
#$BEPrefix1   = Addresses for the second subnet
#$GwPrefix1   = Addresses for the GatewaySubnet
#$VNet1ASN    = ASN for the virtual network
#$DNS1        = The IP address of the DNS server you want to use for name resolution
#$Gw1         = The name of the virtual network gateway
#$GwIP1       = The public IP address for the virtual network gateway
#$GwIPConf1   = The name of the IP configuration
```

Zmień poniższe wartości na podstawie konfiguracji środowiska i sieci, a następnie skopiuj je i wklej, aby ustawić zmienne na potrzeby tego samouczka. Jeśli upłynie limit czasu sesji usługi Cloud Shell lub konieczne jest użycie innego okna programu PowerShell, skopiuj i wklej zmienne do nowej sesji i kontynuuj pracę z tym samouczkiem.

```azurepowershell-interactive
$RG1         = "TestRG1"
$VNet1       = "VNet1"
$Location1   = "East US"
$FESubnet1   = "FrontEnd"
$BESubnet1   = "Backend"
$VNet1Prefix = "10.1.0.0/16"
$FEPrefix1   = "10.1.0.0/24"
$BEPrefix1   = "10.1.1.0/24"
$GwPrefix1   = "10.1.255.0/27"
$VNet1ASN    = 65010
$DNS1        = "8.8.8.8"
$Gw1         = "VNet1GW"
$GwIP1       = "VNet1GWIP"
$GwIPConf1   = "gwipconf1"
```

## <a name="create-a-resource-group"></a>Tworzenie grupy zasobów

Utwórz grupę zasobów za pomocą polecenia [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup). Grupa zasobów platformy Azure to logiczny kontener przeznaczony do wdrażania zasobów platformy Azure i zarządzania nimi. Najpierw należy utworzyć grupę zasobów. W poniższym przykładzie grupa zasobów o nazwie *TestRG1* została utworzona w regionie *Wschodnie stany USA*:

```azurepowershell-interactive
New-AzResourceGroup -ResourceGroupName $RG1 -Location $Location1
```

## <a name="create-a-virtual-network"></a>Tworzenie sieci wirtualnej

Brama sieci VPN na platformie Azure zapewnia łączność między lokalizacjami oraz funkcjonalność serwera VPN P2S w sieci wirtualnej. Dodaj bramę sieci VPN do istniejącej sieci wirtualnej lub utwórz nową sieć wirtualną i bramę. Należy zauważyć, że w przykładzie określono nazwę podsieci bramy. Należy zawsze określić nazwę podsieci bramy jako "GatewaySubnet", aby działała prawidłowo. Ten przykład umożliwia utworzenie nowej sieci wirtualnej z trzema podsieciami: frontonu, zaplecza i GatewaySubnet przy użyciu polecenia [New-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/new-azvirtualnetworksubnetconfig) i [New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork):

```azurepowershell-interactive
$fesub1 = New-AzVirtualNetworkSubnetConfig -Name $FESubnet1 -AddressPrefix $FEPrefix1
$besub1 = New-AzVirtualNetworkSubnetConfig -Name $BESubnet1 -AddressPrefix $BEPrefix1
$gwsub1 = New-AzVirtualNetworkSubnetConfig -Name GatewaySubnet -AddressPrefix $GwPrefix1
$vnet   = New-AzVirtualNetwork `
            -Name $VNet1 `
            -ResourceGroupName $RG1 `
            -Location $Location1 `
            -AddressPrefix $VNet1Prefix `
            -Subnet $fesub1,$besub1,$gwsub1
```

## <a name="request-a-public-ip-address-for-the-vpn-gateway"></a>Żądanie publicznego adresu IP dla bramy sieci VPN

Bramy sieci VPN na platformie Azure komunikują się z lokalnymi urządzeniami sieci VPN przez Internet, by przeprowadzić negocjacje IKE (Internet Key Exchange) oraz utworzyć tunele protokołu IPsec. Utwórz publiczny adres IP i przypisz go do bramy sieci VPN za pomocą poleceń [New-AzPublicIpAddress](/powershell/module/az.network/new-azpublicipaddress) oraz [New-AzPublicIpAddress](/powershell/module/az.network/new-azvirtualnetworkgatewayipconfig), jak pokazano w poniższym przykładzie:

> [!IMPORTANT]
> Obecnie bramy mogą korzystać tylko z dynamicznych publicznych adresów IP. Statyczne adresy IP nie są obsługiwane przez bramy sieci VPN na platformie Azure.

```azurepowershell-interactive
$gwpip    = New-AzPublicIpAddress -Name $GwIP1 -ResourceGroupName $RG1 `
              -Location $Location1 -AllocationMethod Dynamic
$subnet   = Get-AzVirtualNetworkSubnetConfig -Name 'GatewaySubnet' `
              -VirtualNetwork $vnet
$gwipconf = New-AzVirtualNetworkGatewayIpConfig -Name $GwIPConf1 `
              -Subnet $subnet -PublicIpAddress $gwpip
```

## <a name="create-a-vpn-gateway"></a>Tworzenie bramy sieci VPN

Tworzenie bramy sieci VPN może potrwać 45 minut lub dłużej. Po zakończeniu tworzenia bramy możesz utworzyć połączenie między swoją siecią wirtualną a inną siecią wirtualną albo lokalizacją lokalną. Utwórz bramę sieci VPN za pomocą polecenia cmdlet [New-AzVirtualNetworkGateway](/powershell/module/az.network/New-azVirtualNetworkGateway).

```azurepowershell-interactive
New-AzVirtualNetworkGateway -Name $Gw1 -ResourceGroupName $RG1 `
  -Location $Location1 -IpConfigurations $gwipconf -GatewayType Vpn `
  -VpnType RouteBased -GatewaySku VpnGw1
```

Wartości parametrów klucza:
* GatewayType: użyj **Vpn** w przypadku połączeń między lokacjami oraz między sieciami wirtualnymi
* VpnType: użyj **RouteBased**, by umożliwić interakcję z większą liczbą urządzeń VPN oraz korzystanie z większej liczby funkcji routingu
* GatewaySku: **VpnGw1** jest wartością domyślną; Zmień ją na inną jednostkę SKU VpnGw, jeśli potrzebujesz większej przepływności lub większej liczby połączeń. Aby uzyskać więcej informacji, zobacz [Gateway SKUs](vpn-gateway-about-vpn-gateway-settings.md#gwsku) (Jednostki SKU bramy).

Jeśli używasz TryIt, sesja może przekroczyć limit czasu. Tak jest ok. Brama zostanie utworzona.

Po zakończeniu tworzenia bramy możesz utworzyć połączenie między swoją siecią wirtualną a inną siecią wirtualną albo lokalizacją lokalną. Możesz również skonfigurować połączenie P2S komputera klienckiego ze swoją siecią lokalną.

## <a name="view-the-gateway-public-ip-address"></a>Wyświetlanie publicznego adresu IP bramy

Jeśli znasz nazwę publicznego adresu IP, za pomocą polecenia [Get-AzPublicIpAddress](https://docs.microsoft.com/powershell/module/az.network/get-azpublicipaddress) możesz wyświetlić publiczny adres IP przypisany do bramy.

Jeśli upłynął limit czasu sesji, skopiuj wspólne parametry sieci z początkowej części tego samouczka do nowej sesji i kontynuuj pracę.

```azurepowershell-interactive
$myGwIp = Get-AzPublicIpAddress -Name $GwIP1 -ResourceGroup $RG1
$myGwIp.IpAddress
```

## <a name="resize-a-gateway"></a>Zmienianie rozmiaru bramy

Możesz zmienić jednostkę SKU po utworzeniu bramy sieci VPN. Różne jednostki SKU bramy obsługują różne specyfikacje, takie jak przepływność, liczba połączeń itp. W poniższym przykładzie zastosowano [zmianę rozmiaru AzVirtualNetworkGateway](/powershell/module/az.network/Resize-azVirtualNetworkGateway) , aby zmienić rozmiar bramy z VpnGw1 na VpnGw2. Aby uzyskać więcej informacji, zobacz [Gateway SKUs](vpn-gateway-about-vpn-gateway-settings.md#gwsku) (Jednostki SKU bramy).

```azurepowershell-interactive
$gateway = Get-AzVirtualNetworkGateway -Name $Gw1 -ResourceGroup $RG1
Resize-AzVirtualNetworkGateway -GatewaySku VpnGw2 -VirtualNetworkGateway $gateway
```

Zmiana rozmiaru bramy sieci VPN również trwa około 30–45 minut, jednak ta operacja **nie** przerywa ani nie usuwa istniejących połączeń i konfiguracji.

## <a name="reset-a-gateway"></a>Resetowanie bramy

W ramach rozwiązywania problemów możesz zresetować bramę sieci VPN na platformie Azure, by wymusić ponowne uruchomienie konfiguracji tuneli protokołu IPsec/IKE. Aby zresetować bramę, użyj polecenia [Reset-AzVirtualNetworkGateway](/powershell/module/az.network/Reset-azVirtualNetworkGateway).

```azurepowershell-interactive
$gateway = Get-AzVirtualNetworkGateway -Name $Gw1 -ResourceGroup $RG1
Reset-AzVirtualNetworkGateway -VirtualNetworkGateway $gateway
```

Aby uzyskać więcej informacji, zobacz temat [Resetowanie bramy VPN Gateway](vpn-gateway-resetgw-classic.md).

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Jeśli przejdziesz do [następnego samouczka](vpn-gateway-tutorial-vpnconnection-powershell.md), chcesz zachować te zasoby, ponieważ są one wymagane.

Jeśli jednak brama jest częścią wdrożenia prototypowego lub służącego do weryfikacji koncepcji, możesz usunąć grupę zasobów, bramę sieci VPN i wszystkie powiązane zasoby za pomocą polecenia [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup).

```azurepowershell-interactive
Remove-AzResourceGroup -Name $RG1
```

## <a name="next-steps"></a>Następne kroki

W niniejszym samouczku zawarto informacje dotyczące podstawowych zadań tworzenia bramy sieci VPN i zarządzania nią, takie jak:

> [!div class="checklist"]
> * Tworzenie bramy sieci VPN
> * Wyświetlanie publicznego adresu IP
> * Zmiana rozmiaru bramy sieci VPN
> * Resetowanie bramy VPN Gateway

Przejdź do następujących samouczków, aby dowiedzieć się więcej na temat połączeń S2S, między sieciami wirtualnymi i P2S.

> [!div class="nextstepaction"]
> * [Tworzenie połączeń S2S](vpn-gateway-tutorial-vpnconnection-powershell.md)
> * [Tworzenie połączeń między sieciami wirtualnymi](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
> * [Tworzenie połączeń P2S](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
