---
title: Wyświetl metryki usługi Azure VPN Gateway
description: Kroki umożliwiające wyświetlenie metryk VPN Gateway
services: vpn-gateway
author: kumudD
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 07/12/2020
ms.author: alzam
ms.openlocfilehash: b3a79b8101a55eaf401c20cb118be3b0796b7aca
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/20/2020
ms.locfileid: "86532149"
---
# <a name="view-vpn-gateway-metrics"></a>Wyświetl metryki VPN Gateway

Bramy sieci VPN platformy Azure można monitorować przy użyciu Azure Monitor. W tym artykule omówiono metryki, które są dostępne w portalu. Metryki są lekkie i obsługują scenariusze niemal w czasie rzeczywistym, dzięki czemu mogą być używane do tworzenia alertów i szybkiego wykrywania problemów.


|**Metryka**   | **Jednostka** | **Poziom szczegółowości** | **Opis** | 
|---       | ---        | ---       | ---            | ---       |
|**AverageBandwidth**| Bajty/s  | 5 min| Średnie wykorzystanie łącznej przepustowości wszystkich połączeń między lokacjami w bramie.     |
|**P2SBandwidth**| Bajty/s  | 1 minuta  | Średnie wykorzystanie przepustowości połączonej wszystkich połączeń punkt-lokacja na bramie.    |
|**P2SConnectionCount**| Liczba  | 1 minuta  | Liczba połączeń punkt-lokacja na bramie.   |
|**TunnelAverageBandwidth** | Bajty/s    | 5 min  | Średnie wykorzystanie przepustowości przez tunele utworzone na bramie. |
|**TunnelEgressBytes** | Bajty | 5 min | Ruch wychodzący w tunelach utworzonych na bramie.   |
|**TunnelEgressPackets** | Liczba | 5 min | Liczba wychodzących pakietów w tunelach utworzonych na bramie.   |
|**TunnelEgressPacketDropTSMismatch** | Liczba | 5 min | Liczba pakietów wychodzących usuniętych w tunelach spowodowanych przez niezgodność selektora ruchu. |
|**TunnelIngressBytes** | Bajty | 5 min | Ruch przychodzący w tunelach utworzonych na bramie.   |
|**TunnelIngressPackets** | Liczba | 5 min | Liczba pakietów przychodzących w tunelach utworzonych na bramie.   |
|**TunnelIngressPacketDropTSMismatch** | Liczba | 5 min | Liczba pakietów przychodzących porzuconych na tunelach spowodowanych przez niezgodność selektora ruchu. |

## <a name="the-following-steps-help-you-locate-and-view-metrics"></a>Poniższe kroki ułatwiają lokalizowanie i wyświetlanie metryk:

1. Przejdź do zasobu bramy sieci wirtualnej w portalu
2. Wybierz pozycję **Przegląd** , aby wyświetlić metryki łącznego ruchu przychodzącego i wychodzącego tunelu.

   ![Wybory dotyczące tworzenia reguły alertu](./media/vpn-gateway-howto-view-virtual-network-gateway-metrics/overview.png "Widok")

3. Aby wyświetlić wszystkie inne metryki wymienione powyżej. Kliknij sekcję **metryki** w obszarze zasób bramy sieci wirtualnej i wybierz metrykę z listy rozwijanej.

   ![Przycisk Wybierz i Brama sieci VPN na liście zasobów](./media/vpn-gateway-howto-view-virtual-network-gateway-metrics/metrics.png "Wybierz pozycję")

## <a name="next-steps"></a>Następne kroki

Aby skonfigurować alerty dotyczące metryk tuneli, zobacz [Konfigurowanie alertów na VPN Gateway metryki](vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric.md).

Aby skonfigurować alerty dotyczące dzienników zasobów tunelu, zobacz [Konfigurowanie alertów dotyczących dzienników zasobów VPN Gateway](vpn-gateway-howto-setup-alerts-virtual-network-gateway-log.md).
