---
title: Konfigurowanie tunelu użytkownika z zawsze włączonymi sieciami VPN
titleSuffix: Azure Virtual WAN
description: W tym artykule opisano sposób konfigurowania tunelu użytkownika zawsze włączonej sieci VPN dla wirtualnej sieci WAN
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 06/22/2020
ms.author: cherylmc
ms.openlocfilehash: 03f67053a5a199c8c64efb05d2b6a65ad6707650
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "85564052"
---
# <a name="configure-an-always-on-vpn-user-tunnel-for-virtual-wan"></a>Konfigurowanie tunelu użytkownika usługi Always On VPN dla wirtualnej sieci WAN

[!INCLUDE [intro](../../includes/vpn-gateway-vwan-always-on-intro.md)]

## <a name="prerequisites"></a>Wymagania wstępne

Należy utworzyć konfigurację typu punkt-lokacja i edytować przypisanie koncentratora wirtualnego. Aby uzyskać instrukcje, zobacz następujące sekcje:

* [Tworzenie konfiguracji P2S](virtual-wan-point-to-site-portal.md#p2sconfig)
* [Tworzenie centrum przy użyciu bramy P2S](virtual-wan-point-to-site-portal.md#hub)

## <a name="configure-a-user-tunnel"></a>Konfigurowanie tunelu użytkownika

[!INCLUDE [user tunnel](../../includes/vpn-gateway-vwan-always-on-user.md)]

## <a name="to-remove-a-profile"></a>Aby usunąć profil

Aby usunąć profil, wykonaj następujące czynności:

1. Uruchom następujące polecenie:

   ```powershell
   C:\> Remove-VpnConnection UserTest  
   ```

1. Odłącz połączenie i wyczyść pole wyboru **Połącz automatycznie** .

   ![Czyszczenie](./media/howto-always-on-user-tunnel/disconnect.jpg)

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji na temat wirtualnej sieci WAN, zobacz [często zadawane pytania](virtual-wan-faq.md).
