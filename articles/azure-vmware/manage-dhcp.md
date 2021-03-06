---
title: Jak zarządzać protokołem DHCP
description: W tym artykule wyjaśniono, jak zarządzać usługą DHCP w rozwiązaniu Azure VMware (Automatyczna synchronizacja)
ms.topic: conceptual
ms.date: 05/04/2020
ms.openlocfilehash: 80791dd2041fb9d6fbc7c67f2d7d7b2d0b6c977e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "84148365"
---
# <a name="how-to-manage-dhcp-in-azure-vmware-solution-avs-preview"></a>Jak zarządzać usługą DHCP w programie Azure VMWare Solution (automatyczna wersja zapoznawcza)

NSX-T zapewnia możliwość konfigurowania protokołu DHCP dla chmury prywatnej. Jeśli planujesz używać NSX-T do hostowania serwera DHCP, zobacz [Tworzenie serwera DHCP](#create-dhcp-server). W przeciwnym razie, jeśli w sieci istnieje zewnętrzny serwer DHCP innej firmy i chcesz przekazać żądania do tego serwera DHCP, zobacz [Tworzenie usługi przekaźnika DHCP](#create-dhcp-relay-service).

## <a name="create-dhcp-server"></a>Utwórz serwer DHCP

Wykonaj następujące kroki, aby skonfigurować serwer DHCP na NSX-T.

W programie NSX Manager przejdź do karty **Sieć** , a następnie wybierz pozycję **DHCP** w obszarze **Zarządzanie IP**. Wybierz przycisk **Dodaj serwer** . Podaj nazwę serwera i adres IP serwera. Po zakończeniu wybierz pozycję **Zapisz**.

:::image type="content" source="./media/manage-dhcp/dhcp-server-settings.png" alt-text="Dodaj serwer DHCP" border="true":::

### <a name="connect-dhcp-server-to-the-tier-1-gateway"></a>Połącz serwer DHCP z bramą warstwy 1.

Wybierz pozycję **bramy warstwy 1**, wybierz bramę i wybierz pozycję **Edytuj** .

:::image type="content" source="./media/manage-dhcp/edit-tier-1-gateway.png" alt-text="Wybierz bramę do użycia" border="true":::

Dodaj podsieć, wybierając pozycję **Brak zestawu alokacji adresów IP**

:::image type="content" source="./media/manage-dhcp/add-subnet.png" alt-text="Dodawanie podsieci" border="true":::

Na następnym ekranie wybierz pozycję **serwer lokalny DHCP** z listy rozwijanej **Typ** . W obszarze **serwer DHCP**wybierz opcję **domyślny serwer DHCP** , a następnie wybierz pozycję **Zapisz**.

:::image type="content" source="./media/manage-dhcp/set-ip-address-management.png" alt-text="Wybieranie opcji serwera DHCP" border="true":::

W oknie **brama warstwy 1** wybierz pozycję **Zapisz**. Na następnym ekranie zobaczysz **zmiany zapisane**, wybierz pozycję **Zamknij edytowanie** , aby zakończyć.

### <a name="add-a-network-segment"></a>Dodawanie segmentu sieci

Po utworzeniu serwera DHCP należy dodać do niego segmenty sieci.

W NSX-T wybierz kartę **Sieć** i wybierz **segmenty** w obszarze **łączność**. Wybierz pozycję **Dodaj segment**. Nazwij segment i połączenie z bramą warstwy 1. Następnie wybierz pozycję **Ustaw podsieci** , aby skonfigurować nową podsieć. 

:::image type="content" source="./media/manage-dhcp/add-segment.png" alt-text="Dodaj nowy segment sieci" border="true":::

W oknie **Ustawianie podsieci** wybierz pozycję **Dodaj podsieć**. Wprowadź adres IP bramy i zakres DHCP, a następnie wybierz pozycję **Dodaj** , a następnie **Zastosuj**

:::image type="content" source="./media/manage-dhcp/add-subnet-segment.png" alt-text="Dodaj segment sieci" border="true":::

Po zakończeniu wybierz pozycję **Zapisz** , aby zakończyć dodawanie segmentu sieci.

:::image type="content" source="./media/manage-dhcp/segments-complete.png" alt-text="kompletne segmenty" border="true":::

## <a name="create-dhcp-relay-service"></a>Tworzenie usługi przekaźnika DHCP

W oknie NXT-T wybierz kartę **Sieć** , a następnie w obszarze **Zarządzanie IP**wybierz pozycję **DHCP**. Wybierz pozycję **Dodaj serwer**. W polu **Typ serwera** wybierz opcję przekaźnik DHCP, a następnie wprowadź nazwę serwera i adres IP serwera przekazywania. Wybierz przycisk **Zapisz**, aby zapisać zmiany.

:::image type="content" source="./media/manage-dhcp/create-dhcp-relay.png" alt-text="Utwórz serwer przekazywania DHCP" border="true":::

Wybierz pozycję **bramy warstwy 1** w obszarze **łączność**. Wybierz wielokropek pionowy w bramie warstwy 1 i wybierz pozycję **Edytuj**.

:::image type="content" source="./media/manage-dhcp/edit-tier-1-gateway-relay.png" alt-text="Edytuj bramę warstwy 1" border="true":::

Wybierz pozycję **Brak zestawu alokacji adresów IP** , aby zdefiniować alokację adresów IP.

:::image type="content" source="./media/manage-dhcp/edit-ip-address-allocation.png" alt-text="Edytuj alokację adresów IP" border="true":::

W oknie dialogowym, w polu **Typ**wybierz opcję **serwer przekaźnika DHCP**. Na liście rozwijanej **przekaźnik DHCP** wybierz serwer przekaźnika DHCP. Po zakończeniu wybierz pozycję **Zapisz** .

:::image type="content" source="./media/manage-dhcp/set-ip-address-management-relay.png" alt-text="Ustawianie zarządzania adresami IP" border="true":::

Określ adres IP zakresu DHCP dla segmentu:

> [!NOTE]
> Ta konfiguracja jest wymagana do realizacji funkcji przekazywania DHCP w segmencie klienta DHCP. 

W obszarze **łączność**wybierz pozycję **segmenty**. Wybierz wielokropek pionowy i wybierz pozycję **Edytuj**. Zamiast tego, jeśli chcesz dodać nowy segment, możesz wybrać pozycję **Dodaj segment** , aby utworzyć nowy segment.

:::image type="content" source="./media/manage-dhcp/edit-segments.png" alt-text="Edytowanie podsieci sieciowej" border="true":::

Dodaj szczegóły dotyczące segmentu. Wybierz wartość w obszarze **podsieci** lub **Ustaw podsieci** , aby dodać lub zmodyfikować podsieć.

:::image type="content" source="./media/manage-dhcp/network-segments.png" alt-text="segmenty sieci" border="true":::

Wybierz wielokropek pionowy i wybierz pozycję **Edytuj**. Jeśli musisz utworzyć nową podsieć, wybierz pozycję **Dodaj podsieć** , aby utworzyć bramę i skonfigurować zakres DHCP. Podaj zakres puli adresów IP i wybierz pozycję **Zastosuj**, a następnie wybierz pozycję **Zapisz** .

:::image type="content" source="./media/manage-dhcp/edit-subnet.png" alt-text="Edytowanie podsieci" border="true":::

Teraz Pula serwerów DHCP jest przypisana do segmentu.

:::image type="content" source="./media/manage-dhcp/assigned-to-segment.png" alt-text="Pula serwerów DHCP przypisana do segmentu" border="true":::
