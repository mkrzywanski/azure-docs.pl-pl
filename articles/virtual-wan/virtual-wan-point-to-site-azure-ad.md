---
title: 'Konfigurowanie uwierzytelniania usługi Azure AD dla połączenia sieci VPN użytkownika: wirtualna sieć WAN'
description: Dowiedz się, jak skonfigurować uwierzytelnianie Azure Active Directory dla sieci VPN użytkownika.
services: virtual-wan
author: kumudD
ms.service: virtual-wan
ms.topic: how-to
ms.date: 03/17/2020
ms.author: alzam
ms.openlocfilehash: 2028cae4908214db28de2545f02f5f2997eeb8af
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/23/2020
ms.locfileid: "87077474"
---
# <a name="configure-azure-active-directory-authentication-for-user-vpn"></a>Konfigurowanie uwierzytelniania Azure Active Directory dla sieci VPN użytkownika

W tym artykule opisano sposób konfigurowania uwierzytelniania usługi Azure AD dla sieci VPN użytkownika w wirtualnej sieci WAN w celu łączenia się z zasobami na platformie Azure za pośrednictwem połączenia sieci VPN OpenVPN. Uwierzytelnianie Azure Active Directory jest dostępne tylko dla bram korzystających z protokołu OpenVPN i klientów z systemem Windows.

Ten typ połączenia wymaga skonfigurowania klienta na komputerze klienckim. Aby uzyskać więcej informacji na temat usługi Virtual WAN, zobacz [Omówienie usługi Virtual WAN](virtual-wan-about.md).

W tym artykule omówiono sposób wykonywania następujących zadań:

> [!div class="checklist"]
> * Tworzenie sieci WAN
> * Tworzenie koncentratora
> * Tworzenie konfiguracji P2S
> * Pobieranie profilu klienta VPN
> * Stosowanie konfiguracji P2S do koncentratora
> * Łączenie sieci wirtualnej z koncentratorem
> * Pobieranie i stosowanie konfiguracji klienta sieci VPN
> * Wyświetlanie wirtualnej sieci WAN

![Diagram usługi Virtual WAN](./media/virtual-wan-about/virtualwanp2s.png)

## <a name="before-you-begin"></a>Przed rozpoczęciem

Przed rozpoczęciem konfiguracji sprawdź, czy są spełnione następujące kryteria:

* Masz sieć wirtualną, z którą chcesz nawiązać połączenie. Sprawdź, czy żadna z podsieci sieci lokalnych nie nakłada się na sieci wirtualne, z którymi chcesz nawiązać połączenie. Aby utworzyć sieć wirtualną w Azure Portal, zobacz [Przewodnik Szybki Start](../virtual-network/quick-create-portal.md).

* Twoja sieć wirtualna nie ma żadnych bram sieci wirtualnej. Jeśli sieć wirtualna ma bramę (VPN lub ExpressRoute), należy usunąć wszystkie bramy. Ta konfiguracja wymaga, aby w zamian były połączone sieci wirtualne z bramą wirtualnego centrum sieci WAN.

* Uzyskaj zakres adresów IP w regionie koncentratora. Centrum jest siecią wirtualną, która jest tworzona i używana przez wirtualną sieć WAN. Zakres adresów określony dla centrum nie może pokrywać się z żadną z istniejących sieci wirtualnych, z którymi się łączysz. Nie może również pokrywać się z zakresami adresów, z którymi łączysz się lokalnie. Jeśli nie znasz zakresów adresów IP znajdujących się w konfiguracji sieci lokalnej, koordynuj się z osobą, która może podać te szczegóły.

* Jeśli nie masz subskrypcji platformy Azure, utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="create-a-virtual-wan"></a><a name="wan"></a>Tworzenie wirtualnej sieci WAN

Przejdź w przeglądarce do witryny [Azure Portal](https://portal.azure.com) i zaloguj się przy użyciu konta platformy Azure.

1. Przejdź do strony wirtualna sieć WAN. W portalu kliknij pozycję **+Utwórz zasób**. W polu wyszukiwania wpisz **wirtualną sieć WAN** i wybierz pozycję Wprowadź.
2. Z wyników wybierz pozycję **wirtualna sieć WAN** . Na stronie wirtualna sieć WAN kliknij przycisk **Utwórz** , aby otworzyć stronę tworzenie sieci WAN.
3. Na stronie **Tworzenie sieci WAN** na karcie **podstawowe** wypełnij następujące pola:

   ![Virtual WAN](./media/virtual-wan-point-to-site-azure-ad/vwan.png)

   * **Subskrypcja** — wybierz subskrypcję, która ma być używana.
   * **Grupa zasobów** — Utwórz nową lub Użyj istniejącej.
   * **Lokalizacja grupy zasobów** — wybierz lokalizację zasobu z listy rozwijanej. Sieć WAN to zasób globalny i nie znajduje się w konkretnym regionie. Konieczne jest jednak wybranie regionu, aby łatwiej zarządzać utworzonym zasobem sieci WAN i go lokalizować.
   * **Nazwa** — wpisz nazwę, która ma zostać wywołana w sieci WAN.
   * **Typ:** Standardowa. W przypadku tworzenia podstawowej sieci WAN można utworzyć tylko podstawowe centrum. Centra podstawowe obsługują tylko połączenia sieci VPN typu lokacja-lokacja.
4. Po zakończeniu wypełniania pól wybierz pozycję **Przegląd + Utwórz**.
5. Po zakończeniu walidacji wybierz pozycję **Utwórz** , aby utworzyć wirtualną sieć WAN.

## <a name="create-an-empty-virtual-hub"></a><a name="site"></a>Tworzenie pustego koncentratora wirtualnego

1. W obszarze wirtualne sieci WAN wybierz pozycję centra i kliknij pozycję **+ nowe centrum**.

   ![nowa lokacja](media/virtual-wan-point-to-site-azure-ad/hub1.jpg)
2. Na stronie Tworzenie wirtualnego centrum Wypełnij poniższe pola.

   **Region** — wybierz region, w którym chcesz wdrożyć koncentrator wirtualny.

   **Nazwa** — wprowadź nazwę, która ma być wywoływana z koncentratorem wirtualnym.

   **Prywatna przestrzeń adresowa centrum** — zakres adresów centrum w notacji CIDR.

   ![nowa lokacja](media/virtual-wan-point-to-site-azure-ad/hub2.jpg)  
3. Kliknij pozycję **Przejrzyj i utwórz**.
4. Na stronie **Walidacja została przeniesiona** kliknij przycisk **Utwórz**.

## <a name="create-a-new-p2s-configuration"></a><a name="site"></a>Utwórz nową konfigurację P2S

Konfiguracja P2S definiuje parametry służące do łączenia z klientami zdalnymi.

1. W obszarze wirtualnej sieci WAN wybierz pozycję **konfiguracje sieci VPN użytkownika**.

   ![Nowa konfiguracja](media/virtual-wan-point-to-site-azure-ad/aadportal1.jpg)

2. Kliknij pozycję **+ Utwórz konfigurację sieci VPN użytkownika**.

   ![Nowa konfiguracja](media/virtual-wan-point-to-site-azure-ad/aadportal2.jpg)

3. Wprowadź informacje i kliknij przycisk **Utwórz** .

   ![Nowa konfiguracja](media/virtual-wan-point-to-site-azure-ad/aadportal3.jpg)

## <a name="edit-hub-assignment"></a><a name="hub"></a>Edytowanie przypisania koncentratora

1. Przejdź do bloku **centra** w wirtualnej sieci WAN.
2. Wybierz centrum, do którego chcesz skojarzyć konfigurację serwera sieci VPN, a następnie kliknij przycisk wielokropka (...).

   ![nowa lokacja](media/virtual-wan-point-to-site-azure-ad/p2s4.jpg)
3. Kliknij pozycję **Edytuj koncentrator wirtualny**.
4. Zaznacz pole wyboru **Uwzględnij bramę punkt-lokacja** i wybierz żądaną **jednostkę skalowania bramy** .

   ![nowa lokacja](media/virtual-wan-point-to-site-azure-ad/p2s2.jpg)
5. Wprowadź **pulę adresów** , z której będą przypisywane adresy IP klientów sieci VPN.
6. Kliknij pozycję **Potwierdź**.
7. Wykonanie operacji może potrwać do 30 minut.

## <a name="download-vpn-profile"></a><a name="device"></a>Pobieranie profilu sieci VPN

Użyj profilu sieci VPN, aby skonfigurować klientów.

1. Na stronie wirtualnej sieci WAN kliknij pozycję **konfiguracje sieci VPN użytkownika**.
2. W górnej części strony kliknij pozycję **Pobierz konfigurację sieci VPN użytkownika**.
3. Po zakończeniu tworzenia pliku możesz kliknąć link, aby go pobrać.
4. Użyj pliku profilu, aby skonfigurować klientów sieci VPN.

## <a name="configure-user-vpn-clients"></a>Konfigurowanie klientów VPN użytkowników

Aby nawiązać połączenie, należy pobrać klienta sieci VPN platformy Azure i zaimportować profil klienta sieci VPN, który został pobrany w poprzednich krokach na każdym komputerze, który chce nawiązać połączenie z siecią wirtualną.

> [!NOTE]
> Uwierzytelnianie za pomocą usługi Azure AD jest obsługiwane tylko dla &reg; połączeń protokołu OpenVPN.
>

#### <a name="to-download-the-azure-vpn-client"></a>Aby pobrać klienta sieci VPN platformy Azure

Użyj tego [linku](https://www.microsoft.com/p/azure-vpn-client-preview/9np355qt2sqb?rtc=1&activetab=pivot:overviewtab) , aby pobrać klienta sieci VPN platformy Azure.

#### <a name="to-import-a-client-profile"></a><a name="import"></a>Aby zaimportować profil klienta

1. Na stronie wybierz pozycję **Importuj**.

    ![import](./media/virtual-wan-point-to-site-azure-ad/import/import1.jpg)

2. Przejdź do pliku XML profilu i wybierz go. Po wybraniu pliku wybierz pozycję **Otwórz**.

    ![import](./media/virtual-wan-point-to-site-azure-ad/import/import2.jpg)

3. Określ nazwę profilu i wybierz pozycję **Zapisz**.

    ![import](./media/virtual-wan-point-to-site-azure-ad/import/import3.jpg)

4. Wybierz pozycję **Połącz** , aby nawiązać połączenie z siecią VPN.

    ![import](./media/virtual-wan-point-to-site-azure-ad/import/import4.jpg)

5. Po nawiązaniu połączenia ikona zmieni kolor na zielony i powiedzie **się.**

    ![import](./media/virtual-wan-point-to-site-azure-ad/import/import5.jpg)

#### <a name="to-delete-a-client-profile"></a><a name="delete"></a>Aby usunąć profil klienta

1. Wybierz wielokropek (...) obok profilu klienta, który chcesz usunąć. Następnie wybierz pozycję **Usuń**.

    ![usunięcie](./media/virtual-wan-point-to-site-azure-ad/delete/delete1.jpg)

2. Wybierz pozycję **Usuń** , aby usunąć.

    ![usunięcie](./media/virtual-wan-point-to-site-azure-ad/delete/delete2.jpg)

#### <a name="diagnose-connection-issues"></a><a name="diagnose"></a>Diagnozuj problemy z połączeniem

1. Aby zdiagnozować problemy z połączeniem, można użyć narzędzia do **diagnozowania** . Wybierz wielokropek (...) obok połączenia sieci VPN, które chcesz zdiagnozować, aby wyświetlić menu. Następnie wybierz pozycję **Diagnozuj**.

    ![Sprawdź](./media/virtual-wan-point-to-site-azure-ad/diagnose/diagnose1.jpg)

2. Na stronie **Właściwości połączenia** wybierz pozycję **Uruchom diagnostykę**.

    ![Sprawdź](./media/virtual-wan-point-to-site-azure-ad/diagnose/diagnose2.jpg)

3. Zaloguj się przy użyciu swoich poświadczeń.

    ![Sprawdź](./media/virtual-wan-point-to-site-azure-ad/diagnose/diagnose3.jpg)

4. Wyświetl wyniki diagnostyki.

    ![Sprawdź](./media/virtual-wan-point-to-site-azure-ad/diagnose/diagnose4.jpg)

## <a name="view-your-virtual-wan"></a><a name="viewwan"></a>Wyświetlanie wirtualnej sieci WAN

1. Przejdź do wirtualnej sieci WAN.
2. Na stronie Przegląd każdy punkt na mapie przedstawia koncentrator.
3. W sekcji dotyczącej koncentratorów i połączeń możesz wyświetlić stan koncentratora, lokację, region, stan połączenia sieci VPN oraz bajty przychodzące i wychodzące.


## <a name="clean-up-resources"></a><a name="cleanup"></a>Czyszczenie zasobów

Gdy grupa zasobów i zawarte w niej zasoby nie będą już potrzebne, można je usunąć za pomocą polecenia [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup). Zastąp wartość „myResourceGroup” nazwą grupy zasobów, a następnie uruchom następujące polecenie programu PowerShell:

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji na temat usługi Virtual WAN, zobacz stronę [Omówienie usługi Virtual WAN](virtual-wan-about.md).
