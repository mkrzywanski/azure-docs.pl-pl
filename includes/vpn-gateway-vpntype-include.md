---
title: dołączanie pliku
description: dołączanie pliku
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: b3907882df09bfae1d6453fbffbd3e7562554f7c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "67183113"
---
* **PolicyBased:** Sieci VPN PolicyBased były wcześniej nazywane statycznymi bramami routingu w klasycznym modelu wdrażania. Sieci VPN oparte na zasadach szyfrują pakiety i kierują je bezpośrednio za pośrednictwem tuneli IPsec na podstawie zasad protokołu IPsec skonfigurowanych przy użyciu kombinacji prefiksów adresów między siecią lokalną i sieci wirtualnej platformy Azure. Zasady (lub selektor ruchu) są zazwyczaj zdefiniowane jako lista dostępu w konfiguracji urządzenia sieci VPN. Wartość dla typu sieci VPN PolicyBased to *PolicyBased*. Korzystając z sieci VPN PolicyBased, należy pamiętać o następujących ograniczeniach:
  
  * Sieci VPN PolicyBased można używać **tylko** w podstawowej jednostce SKU bramy. Ten typ sieci VPN nie jest zgodny z innymi jednostkami SKU bramy.
  * W przypadku korzystania z sieci VPN PolicyBased można używać tylko 1 tunelu.
  * Sieci VPN PolicyBased można używać tylko do połączeń S2S i tylko w przypadku niektórych konfiguracji. Większość konfiguracji VPN Gateway wymaga sieci VPN RouteBased.
* **RouteBased**: sieci VPN RouteBased były wcześniej nazywane dynamicznymi bramami routingu w klasycznym modelu wdrażania. Sieci VPN RouteBased używają "tras" w przypadku przesyłania dalej IP lub tabeli routingu do kierowania pakietów do odpowiednich interfejsów tuneli. W dalszej kolejności interfejsy tuneli szyfrują lub odszyfrowują pakiety wchodzące do tuneli lub wychodzące z nich. Zasady (lub selektor ruchu) dla sieci VPN RouteBased są konfigurowane jako dowolne (lub wieloznaczne karty). Wartość dla typu sieci VPN RouteBased to *RouteBased*.
