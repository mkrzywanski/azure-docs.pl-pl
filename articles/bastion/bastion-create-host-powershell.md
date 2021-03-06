---
title: Tworzenie hosta bastionu przy użyciu programu Azure PowerShell | Microsoft Docs
description: W tym artykule dowiesz się, jak utworzyć hosta usługi Azure bastionu
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: how-to
ms.date: 02/03/2020
ms.author: cherylmc
ms.openlocfilehash: c62ac014513f3e93a04008af06ef8ffe5008ed2a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "84744259"
---
# <a name="create-an-azure-bastion-host-using-azure-powershell"></a>Tworzenie hosta usługi Azure bastionu za pomocą Azure PowerShell

W tym artykule opisano sposób tworzenia hosta usługi Azure bastionu przy użyciu programu PowerShell. Po udostępnieniu usługi Azure bastionu w sieci wirtualnej, bezproblemowe środowisko RDP/SSH jest dostępne dla wszystkich maszyn wirtualnych w tej samej sieci wirtualnej. Wdrożenie usługi Azure bastionu odbywa się na sieć wirtualną, a nie na subskrypcję/konto lub maszynę wirtualną.

Opcjonalnie można utworzyć hosta usługi Azure bastionu za pomocą [Azure Portal](bastion-create-host-portal.md).

## <a name="before-you-begin"></a>Przed rozpoczęciem

Sprawdź, czy masz subskrypcję platformy Azure. Jeśli nie masz jeszcze subskrypcji platformy Azure, możesz aktywować [korzyści dla subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details) lub utworzyć [bezpłatne konto](https://azure.microsoft.com/pricing/free-trial).

[!INCLUDE [powershell](../../includes/vpn-gateway-cloud-shell-powershell-about.md)]

## <a name="create-a-bastion-host"></a><a name="createhost"></a>Tworzenie hosta bastionu

Ta sekcja ułatwia tworzenie nowego zasobu usługi Azure bastionu przy użyciu Azure PowerShell.

1. Utwórz sieć wirtualną i podsieć usługi Azure bastionu. Należy utworzyć podsieć usługi Azure bastionu przy użyciu wartości Name **AzureBastionSubnet**. Ta wartość pozwala platformie Azure wiedzieć, która podsieć, do której mają zostać wdrożone zasoby bastionu. Jest to inna niż podsieć bramy. Należy użyć podsieci z co najmniej/27 lub większą podsiecią (/27,/26 itd.). Utwórz **AzureBastionSubnet** bez żadnych tabel tras ani delegowania. Jeśli używasz sieciowych grup zabezpieczeń w **AzureBastionSubnet**, zapoznaj się z artykułem [Pracuj z sieciowych grup zabezpieczeń](bastion-nsg.md) .

   ```azurepowershell-interactive
   $subnetName = "AzureBastionSubnet"
   $subnet = New-AzVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
   $vnet = New-AzVirtualNetwork -Name "myVnet" -ResourceGroupName "myBastionRG" -Location "westeurope" -AddressPrefix 10.0.0.0/16 -Subnet $subnet
   ```

2. Utwórz publiczny adres IP dla usługi Azure bastionu. Publiczny adres IP jest publicznym adresem IP, do którego zostanie uzyskany dostęp do protokołu RDP/SSH (za pośrednictwem portu 443). Publiczny adres IP musi znajdować się w tym samym regionie co tworzony zasób bastionu.

   ```azurepowershell-interactive
   $publicip = New-AzPublicIpAddress -ResourceGroupName "myBastionRG" -name "myPublicIP" -location "westeurope" -AllocationMethod Static -Sku Standard
   ```

3. Utwórz nowy zasób platformy Azure bastionu w AzureBastionSubnet sieci wirtualnej. Utworzenie i wdrożenie zasobu bastionu trwa około 5 minut.

   ```azurepowershell-interactive
   $bastion = New-AzBastion -ResourceGroupName "myBastionRG" -Name "myBastion" -PublicIpAddress $publicip -VirtualNetwork $vnet
   ```

## <a name="next-steps"></a>Następne kroki

* Przeczytaj [często zadawane pytania](bastion-faq.md) dotyczące usługi bastionu, aby uzyskać dodatkowe informacje.

* Aby użyć sieciowych grup zabezpieczeń z podsiecią usługi Azure bastionu, zobacz [Work with sieciowych grup zabezpieczeń](bastion-nsg.md).
