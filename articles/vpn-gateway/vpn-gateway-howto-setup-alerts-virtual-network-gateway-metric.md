---
title: Konfigurowanie alertów w usłudze Azure VPN Gateway metryki
description: Kroki konfigurowania alertów na VPN Gateway metryki
services: vpn-gateway
author: kumudD
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 04/22/2019
ms.author: alzam
ms.openlocfilehash: c2c44720853500ad6a9a4af1c8d93d6c8bc3eea2
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/23/2020
ms.locfileid: "87075530"
---
# <a name="set-up-alerts-on-vpn-gateway-metrics"></a>Konfigurowanie alertów dotyczących metryk VPN Gateway

Ten artykuł ułatwia Konfigurowanie alertów dotyczących usługi Azure VPN Gateway Metrics. Azure Monitor umożliwia Konfigurowanie alertów dla zasobów platformy Azure. Można skonfigurować alerty dla bram sieci wirtualnej typu "VPN".


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


## <a name="set-up-azure-monitor-alerts-based-on-metrics-by-using-the-azure-portal"></a><a name="setup"></a>Konfigurowanie alertów Azure Monitor w oparciu o metryki przy użyciu Azure Portal

Poniższe przykładowe kroki spowodują utworzenie alertu na bramie dla:

- **Metryka:** TunnelAverageBandwidth
- **Warunek:** Przepustowość > 10 bajtów/s
- **Okno:** 5 minut
- **Akcja alertu:** Adres e-mail



1. Przejdź do zasobu bramy sieci wirtualnej i wybierz pozycję **alerty** na karcie **monitorowanie** . Następnie utwórz nową regułę alertu lub edytuj istniejącą regułę alertu.

   ![Wybory dotyczące tworzenia reguły alertu](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric/metric-alert1.png "Utwórz")

2. Wybierz bramę sieci VPN jako zasób.

   ![Przycisk Wybierz i Brama sieci VPN na liście zasobów](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric/metric-alert2.png "Wybierz pozycję")

3. Wybierz metrykę, która ma zostać skonfigurowana dla alertu.

   ![Wybrana Metryka na liście metryk](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric/metric-alert3.png "Wybierz pozycję")
4. Skonfiguruj logikę sygnału. Istnieją trzy składniki:

    a. **Wymiary**: Jeśli Metryka ma wymiary, można wybrać określone wartości wymiaru, aby alert obliczał tylko dane tego wymiaru. Są one opcjonalne.

    b. **Warunek**: jest to operacja szacowania wartości metryki.

    c. **Czas**: Określ stopień szczegółowości danych metryki oraz okres, w którym ma zostać obliczony alert.

   ![Szczegóły dotyczące konfigurowania logiki sygnałów](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric/metric-alert4.png "Wybierz pozycję")

5. Aby wyświetlić skonfigurowane reguły, wybierz pozycję **Zarządzaj regułami alertów**.

   ![Przycisk służący do zarządzania regułami alertów](./media/vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric/metric-alert8.png "Wybierz pozycję")

## <a name="next-steps"></a>Następne kroki

Aby skonfigurować alerty dotyczące dzienników zasobów tunelu, zobacz [Konfigurowanie alertów dotyczących dzienników zasobów VPN Gateway](vpn-gateway-howto-setup-alerts-virtual-network-gateway-log.md).
