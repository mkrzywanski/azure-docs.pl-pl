---
title: 'Szybki Start: bezpośredni ruch internetowy przy użyciu programu PowerShell'
titleSuffix: Azure Application Gateway
description: Dowiedz się, jak za pomocą Azure PowerShell utworzyć Application Gateway platformy Azure, która kieruje ruch internetowy do maszyn wirtualnych w puli zaplecza.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: quickstart
ms.date: 04/15/2020
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: 3e1ca14d967b0e88ea7eb559fd9962a3824ff9b0
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/29/2020
ms.locfileid: "81406210"
---
# <a name="quickstart-direct-web-traffic-with-azure-application-gateway-using-azure-powershell"></a>Szybki Start: bezpośredni ruch internetowy za pomocą usługi Azure Application Gateway przy użyciu Azure PowerShell

W tym przewodniku szybki start użyjesz Azure PowerShell, aby utworzyć bramę aplikacji. Następnie przetestuj go, aby upewnić się, że działa prawidłowo. 

Aplikacja Application Gateway kieruje ruch sieci Web aplikacji do określonych zasobów w puli zaplecza. Można przypisywać odbiorniki do portów, tworzyć reguły i dodawać zasoby do puli zaplecza. Dla uproszczenia w tym artykule użyto prostej konfiguracji z publicznym adresem IP frontonu, podstawowego odbiornika do hostowania pojedynczej lokacji bramy aplikacji, podstawowej reguły routingu żądań i dwóch maszyn wirtualnych w puli zaplecza.

Możesz również ukończyć ten przewodnik Szybki Start przy użyciu [interfejsu wiersza polecenia platformy Azure](quick-create-cli.md) lub [Azure Portal](quick-create-portal.md).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Wymagania wstępne

- Konto platformy Azure z aktywną subskrypcją. [Utwórz konto bezpłatnie](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- [Azure PowerShell w wersji 1.0.0 lub nowszej](/powershell/azure/install-az-ps) (w przypadku uruchamiania Azure PowerShell lokalnie).

## <a name="connect-to-azure"></a>Nawiązywanie połączenia z usługą Azure

Aby nawiązać połączenie z platformą Azure, uruchom `Connect-AzAccount`polecenie.

## <a name="create-a-resource-group"></a>Tworzenie grupy zasobów

Na platformie Azure możesz przydzielić powiązane zasoby do grupy zasobów. Możesz użyć istniejącej grupy zasobów lub utworzyć nową.

Aby utworzyć nową grupę zasobów, użyj `New-AzResourceGroup` polecenia cmdlet: 

```azurepowershell-interactive
New-AzResourceGroup -Name myResourceGroupAG -Location eastus
```
## <a name="create-network-resources"></a>Tworzenie zasobów sieciowych

Do komunikacji między tworzonymi zasobami platforma Azure potrzebuje sieci wirtualnej.  Podsieć bramy aplikacji może zawierać tylko bramy aplikacji. Inne zasoby nie są dozwolone.  Można utworzyć nową podsieć dla Application Gateway lub użyć istniejącej. W tym przykładzie utworzysz dwie podsieci w tym przykładzie: jeden dla bramy aplikacji, a drugi dla serwerów wewnętrznej bazy danych. Można skonfigurować adres IP frontonu Application Gateway publiczny lub prywatny zgodnie z Twoim przypadkiem użycia. W tym przykładzie wybrano publiczny adres IP frontonu.

1. Utwórz konfiguracje podsieci przy użyciu `New-AzVirtualNetworkSubnetConfig`polecenia.
2. Utwórz sieć wirtualną z konfiguracjami podsieci za pomocą `New-AzVirtualNetwork`polecenia. 
3. Utwórz publiczny adres IP za pomocą `New-AzPublicIpAddress`. 

```azurepowershell-interactive
$agSubnetConfig = New-AzVirtualNetworkSubnetConfig `
  -Name myAGSubnet `
  -AddressPrefix 10.0.1.0/24
$backendSubnetConfig = New-AzVirtualNetworkSubnetConfig `
  -Name myBackendSubnet `
  -AddressPrefix 10.0.2.0/24
New-AzVirtualNetwork `
  -ResourceGroupName myResourceGroupAG `
  -Location eastus `
  -Name myVNet `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $agSubnetConfig, $backendSubnetConfig
New-AzPublicIpAddress `
  -ResourceGroupName myResourceGroupAG `
  -Location eastus `
  -Name myAGPublicIPAddress `
  -AllocationMethod Static `
  -Sku Standard
```
## <a name="create-an-application-gateway"></a>Tworzenie bramy aplikacji

### <a name="create-the-ip-configurations-and-frontend-port"></a>Tworzenie konfiguracji adresów IP i portu frontonu

1. Użyj `New-AzApplicationGatewayIPConfiguration` , aby utworzyć konfigurację, która kojarzy utworzoną podsieć z bramą aplikacji. 
2. Użyj `New-AzApplicationGatewayFrontendIPConfig` , aby utworzyć konfigurację, która przypisuje publiczny adres IP, który został wcześniej utworzony do bramy aplikacji. 
3. Użyj `New-AzApplicationGatewayFrontendPort` , aby przypisać port 80 w celu uzyskania dostępu do bramy aplikacji.

```azurepowershell-interactive
$vnet   = Get-AzVirtualNetwork -ResourceGroupName myResourceGroupAG -Name myVNet
$subnet = Get-AzVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name myAGSubnet
$pip    = Get-AzPublicIPAddress -ResourceGroupName myResourceGroupAG -Name myAGPublicIPAddress 
$gipconfig = New-AzApplicationGatewayIPConfiguration `
  -Name myAGIPConfig `
  -Subnet $subnet
$fipconfig = New-AzApplicationGatewayFrontendIPConfig `
  -Name myAGFrontendIPConfig `
  -PublicIPAddress $pip
$frontendport = New-AzApplicationGatewayFrontendPort `
  -Name myFrontendPort `
  -Port 80
```

### <a name="create-the-backend-pool"></a>Tworzenie puli zaplecza

1. Użyj `New-AzApplicationGatewayBackendAddressPool` , aby utworzyć pulę zaplecza dla bramy aplikacji. Pula zaplecza będzie teraz pusta. Gdy tworzysz karty sieciowe serwera wewnętrznej bazy danych w następnej sekcji, dodasz je do puli zaplecza.
2. Skonfiguruj ustawienia puli zaplecza przy użyciu programu `New-AzApplicationGatewayBackendHttpSetting`.

```azurepowershell-interactive
$backendPool = New-AzApplicationGatewayBackendAddressPool `
  -Name myAGBackendPool
$poolSettings = New-AzApplicationGatewayBackendHttpSetting `
  -Name myPoolSettings `
  -Port 80 `
  -Protocol Http `
  -CookieBasedAffinity Enabled `
  -RequestTimeout 30
```

### <a name="create-the-listener-and-add-a-rule"></a>Tworzenie odbiornika i dodawanie reguły

Platforma Azure wymaga odbiornika, aby brama aplikacji mogła właściwie kierować ruch do puli zaplecza. Platforma Azure wymaga również reguły, aby odbiornik wiedział, której puli zaplecza używać dla ruchu przychodzącego. 

1. Utwórz odbiornik przy użyciu `New-AzApplicationGatewayHttpListener` programu z utworzoną wcześniej konfiguracją frontonu i frontonem. 
2. Służy `New-AzApplicationGatewayRequestRoutingRule` do tworzenia reguły o nazwie *RULE1*. 

```azurepowershell-interactive
$defaultlistener = New-AzApplicationGatewayHttpListener `
  -Name myAGListener `
  -Protocol Http `
  -FrontendIPConfiguration $fipconfig `
  -FrontendPort $frontendport
$frontendRule = New-AzApplicationGatewayRequestRoutingRule `
  -Name rule1 `
  -RuleType Basic `
  -HttpListener $defaultlistener `
  -BackendAddressPool $backendPool `
  -BackendHttpSettings $poolSettings
```

### <a name="create-the-application-gateway"></a>Tworzenie bramy aplikacji

Teraz po utworzeniu niezbędnych zasobów pomocniczych utwórz bramę aplikacji:

1. Użyj `New-AzApplicationGatewaySku` , aby określić parametry dla bramy aplikacji.
2. Użyj `New-AzApplicationGateway` , aby utworzyć bramę aplikacji.

```azurepowershell-interactive
$sku = New-AzApplicationGatewaySku `
  -Name Standard_v2 `
  -Tier Standard_v2 `
  -Capacity 2
New-AzApplicationGateway `
  -Name myAppGateway `
  -ResourceGroupName myResourceGroupAG `
  -Location eastus `
  -BackendAddressPools $backendPool `
  -BackendHttpSettingsCollection $poolSettings `
  -FrontendIpConfigurations $fipconfig `
  -GatewayIpConfigurations $gipconfig `
  -FrontendPorts $frontendport `
  -HttpListeners $defaultlistener `
  -RequestRoutingRules $frontendRule `
  -Sku $sku
```

### <a name="backend-servers"></a>Serwery zaplecza

Teraz, gdy utworzono Application Gateway, Utwórz maszyny wirtualne zaplecza, które będą hostować witryny sieci Web. Zaplecze może składać się z kart sieciowych, zestawów skalowania maszyn wirtualnych, publicznych adresów IP, wewnętrznych adresów IP, w pełni kwalifikowanych nazw domen (FQDN) i wielodostępnych zaplecza, takich jak Azure App Service. W tym przykładzie utworzysz dwie maszyny wirtualne platformy Azure, które będą używane jako serwery zaplecza dla bramy aplikacji. Zainstalujesz również usługi IIS na maszynach wirtualnych, aby sprawdzić, czy platforma Azure pomyślnie utworzyła bramę aplikacji.

#### <a name="create-two-virtual-machines"></a>Tworzenie dwóch maszyn wirtualnych

1. Pobierz ostatnio utworzoną Application Gateway konfigurację puli zaplecza przy `Get-AzApplicationGatewayBackendAddressPool`użyciu programu.
2. Utwórz interfejs sieciowy przy użyciu `New-AzNetworkInterface`.
3. Utwórz konfigurację maszyny wirtualnej za pomocą `New-AzVMConfig`programu.
4. Utwórz maszynę wirtualną za `New-AzVM`pomocą programu.

Po uruchomieniu następującego przykładowego kodu w celu utworzenia maszyn wirtualnych na platformie Azure zostanie wyświetlony monit o podanie poświadczeń. Wprowadź *azureuser* dla nazwy użytkownika i hasła:
    
```azurepowershell-interactive
$appgw = Get-AzApplicationGateway -ResourceGroupName myResourceGroupAG -Name myAppGateway
$backendPool = Get-AzApplicationGatewayBackendAddressPool -Name myAGBackendPool -ApplicationGateway $appgw
$vnet   = Get-AzVirtualNetwork -ResourceGroupName myResourceGroupAG -Name myVNet
$subnet = Get-AzVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name myBackendSubnet
$cred = Get-Credential
for ($i=1; $i -le 2; $i++)
{
  $nic = New-AzNetworkInterface `
    -Name myNic$i `
    -ResourceGroupName myResourceGroupAG `
    -Location EastUS `
    -Subnet $subnet `
    -ApplicationGatewayBackendAddressPool $backendpool
  $vm = New-AzVMConfig `
    -VMName myVM$i `
    -VMSize Standard_DS2_v2
  Set-AzVMOperatingSystem `
    -VM $vm `
    -Windows `
    -ComputerName myVM$i `
    -Credential $cred
  Set-AzVMSourceImage `
    -VM $vm `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter `
    -Version latest
  Add-AzVMNetworkInterface `
    -VM $vm `
    -Id $nic.Id
  Set-AzVMBootDiagnostic `
    -VM $vm `
    -Disable
  New-AzVM -ResourceGroupName myResourceGroupAG -Location EastUS -VM $vm
  Set-AzVMExtension `
    -ResourceGroupName myResourceGroupAG `
    -ExtensionName IIS `
    -VMName myVM$i `
    -Publisher Microsoft.Compute `
    -ExtensionType CustomScriptExtension `
    -TypeHandlerVersion 1.4 `
    -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}' `
    -Location EastUS
}
```

## <a name="test-the-application-gateway"></a>Testowanie bramy aplikacji

Mimo że zainstalowanie usług IIS nie jest wymagane do utworzenia bramy aplikacji, zainstalowano je w ramach tego przewodnika Szybki start, aby sprawdzić, czy platforma Azure pomyślnie utworzyła bramę aplikacji. Użyj usług do przetestowania bramy aplikacji:

1. Uruchom `Get-AzPublicIPAddress` , aby uzyskać publiczny adres IP bramy aplikacji. 
2. Skopiuj i wklej publiczny adres IP na pasku adresu przeglądarki. Po odświeżeniu przeglądarki powinna zostać wyświetlona nazwa maszyny wirtualnej. Prawidłowa odpowiedź weryfikuje, czy Brama aplikacji została pomyślnie utworzona i może pomyślnie nawiązać połączenie z zapleczem.

```azurepowershell-interactive
Get-AzPublicIPAddress -ResourceGroupName myResourceGroupAG -Name myAGPublicIPAddress
```

![Testowanie bramy aplikacji](./media/quick-create-powershell/application-gateway-iistest.png)


## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Gdy nie potrzebujesz już zasobów utworzonych przy użyciu bramy aplikacji, Usuń grupę zasobów. Po usunięciu grupy zasobów należy również usunąć bramę aplikacji i wszystkie powiązane z nią zasoby. 

Aby usunąć grupę zasobów, wywołaj `Remove-AzResourceGroup` polecenie cmdlet:

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroupAG
```

## <a name="next-steps"></a>Następne kroki

> [!div class="nextstepaction"]
> [Zarządzanie ruchem internetowym przy użyciu bramy aplikacji za pomocą programu Azure PowerShell](./tutorial-manage-web-traffic-powershell.md)

