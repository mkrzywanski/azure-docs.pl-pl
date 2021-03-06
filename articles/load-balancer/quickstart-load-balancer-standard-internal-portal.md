---
title: 'Szybki Start: Tworzenie wewnętrznego modułu równoważenia obciążenia — Azure Portal'
titleSuffix: Azure Load Balancer
description: Ten przewodnik Szybki Start przedstawia sposób tworzenia wewnętrznego modułu równoważenia obciążenia przy użyciu Azure Portal.
services: load-balancer
documentationcenter: na
author: asudbring
manager: KumudD
Customer intent: I want to create a internal load balancer so that I can load balance internal traffic to VMs.
ms.service: load-balancer
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/30/2020
ms.author: allensu
ms.custom: mvc
ms.openlocfilehash: fb0a5c06c8b7bdbb62e937e865e6bb2fd4000f2d
ms.sourcegitcommit: f988fc0f13266cea6e86ce618f2b511ce69bbb96
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/31/2020
ms.locfileid: "87462478"
---
# <a name="quickstart-create-an-internal-load-balancer-to-load-balance-vms-using-the-azure-portal"></a>Szybki Start: Tworzenie wewnętrznego modułu równoważenia obciążenia w celu równoważenia obciążenia maszyn wirtualnych przy użyciu Azure Portal

Rozpocznij pracę z Azure Load Balancer przy użyciu Azure Portal, aby utworzyć wewnętrzny moduł równoważenia obciążenia i dwie maszyny wirtualne.

## <a name="prerequisites"></a>Wymagania wstępne

- Konto platformy Azure z aktywną subskrypcją. [Utwórz konto bezpłatnie](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="sign-in-to-azure"></a>Logowanie do platformy Azure

Zaloguj się do witryny Azure Portal pod adresem [https://portal.azure.com](https://portal.azure.com).

---

# <a name="option-1-default-create-a-internal-load-balancer-standard-sku"></a>[Opcja 1 (domyślnie): Tworzenie wewnętrznego modułu równoważenia obciążenia (standardowa jednostka SKU)](#tab/option-1-create-internal-load-balancer-standard)

>[!NOTE]
>Moduł równoważenia obciążenia w warstwie Standardowa jest zalecany w przypadku obciążeń produkcyjnych.  Aby uzyskać więcej informacji o jednostkach SKU, zobacz **[Azure Load Balancer SKU](skus.md)**.

W tej sekcji utworzysz moduł równoważenia obciążenia, który równoważy obciążenie maszyn wirtualnych. 

Można utworzyć publiczny moduł równoważenia obciążenia lub wewnętrzny moduł równoważenia obciążenia. 

Podczas tworzenia wewnętrznego modułu równoważenia obciążenia Sieć wirtualna jest konfigurowana jako sieć dla modułu równoważenia obciążenia. 

Prywatny adres IP w sieci wirtualnej jest skonfigurowany jako fronton (domyślnie **LoadBalancerFrontend** ) dla usługi równoważenia obciążenia. 

Adres IP frontonu może być **statyczny** lub **dynamiczny**.

## <a name="virtual-network-and-parameters"></a>Sieć wirtualna i parametry

W tej sekcji zastąpisz parametry w krokach poniższymi informacjami:

| Parametr                   | Wartość                |
|-----------------------------|----------------------|
| **\<resource-group-name>**  | myResourceGroupLB |
| **\<virtual-network-name>** | myVNet          |
| **\<region-name>**          | West Europe      |
| **\<IPv4-address-space>**   | 10.1.0.0 \ 16          |
| **\<subnet-name>**          | myBackendSubnet        |
| **\<subnet-address-range>** | 10.1.0.0 \ 24          |

[!INCLUDE [virtual-networks-create-new](../../includes/virtual-networks-create-new.md)]

## <a name="create-load-balancer"></a>Tworzenie modułu równoważenia obciążenia

1. W lewym górnym rogu ekranu wybierz pozycję **Utwórz zasób zasobów**  >  **Networking**  >  **Load Balancer**.

2. Na karcie **podstawy** na stronie **Tworzenie modułu równoważenia obciążenia** wprowadź lub wybierz następujące informacje: 

    | Ustawienie                 | Wartość                                              |
    | ---                     | ---                                                |
    | Subskrypcja               | Wybierz subskrypcję.    |    
    | Grupa zasobów         | Wybierz **myResourceGroupLB** utworzone w poprzednim kroku.|
    | Nazwa                   | Wprowadź **myLoadBalancer**                                   |
    | Region         | Wybierz pozycję **Europa Zachodnia**.                                        |
    | Typ          | wybierz pozycję **Wewnętrzny**.                                        |
    | SKU           | Wybierz pozycję **standardowa** |
    | Sieć wirtualna | Wybierz **myVNet** utworzone w poprzednim kroku. |
    | Podsieć  | Wybierz **myBackendSubnet** utworzone w poprzednim kroku. |
    | Przypisanie adresu IP | wybierz pozycję **Dynamiczne**. |
    | Strefa dostępności | Wybierz **strefę nadmiarową** |

3. Zaakceptuj wartości domyślne pozostałych ustawień, a następnie wybierz pozycję **Przegląd + Utwórz**.

4. Na karcie **Recenzja i tworzenie** wybierz pozycję **Utwórz**.   
    
    :::image type="content" source="./media/quickstart-load-balancer-standard-internal-portal/create-standard-internal-load-balancer.png" alt-text="Tworzenie standardowego wewnętrznego modułu równoważenia obciążenia" border="true":::
 
## <a name="create-load-balancer-resources"></a>Tworzenie zasobów modułu równoważenia obciążenia

W tej sekcji skonfigurujesz:

* Ustawienia usługi równoważenia obciążenia dla puli adresów zaplecza.
* Sonda kondycji.
* Reguła modułu równoważenia obciążenia.

### <a name="create-a-backend-pool"></a>Tworzenie puli zaplecza

Pula adresów zaplecza zawiera adresy IP wirtualnych (kart sieciowych) podłączonych do modułu równoważenia obciążenia. 

Utwórz pulę adresów zaplecza **myBackendPool** , aby uwzględnić maszyny wirtualne na potrzeby ruchu internetowego związanego z równoważeniem obciążenia.

1. W menu po lewej stronie wybierz pozycję Wszystkie **usługi** , wybierz pozycję **wszystkie zasoby**, a następnie wybierz pozycję **myLoadBalancer** z listy zasoby.

2. W obszarze **Ustawienia**wybierz pozycję **Pule zaplecza**, a następnie wybierz pozycję **Dodaj**.

3. Na stronie **Dodawanie puli zaplecza** wpisz nazwę **myBackendPool**, jako nazwę puli zaplecza, a następnie wybierz pozycję **Dodaj**.

### <a name="create-a-health-probe"></a>Tworzenie sondy kondycji

Moduł równoważenia obciążenia monitoruje stan aplikacji przy użyciu sondy kondycji. 

Sonda kondycji dodaje lub usuwa maszyny wirtualne z modułu równoważenia obciążenia na podstawie odpowiedzi na testy kondycji. 

Utwórz sondę kondycji o nazwie **myHealthProbe**, aby monitorować kondycję maszyn wirtualnych.

1. W menu po lewej stronie wybierz pozycję Wszystkie **usługi** , wybierz pozycję **wszystkie zasoby**, a następnie wybierz pozycję **myLoadBalancer** z listy zasoby.

2. W obszarze **Ustawienia**wybierz pozycję **sondy kondycji**, a następnie wybierz pozycję **Dodaj**.
    
    | Ustawienie | Wartość |
    | ------- | ----- |
    | Nazwa | Wprowadź **myHealthProbe**. |
    | Protokół | Wybierz pozycję **http**. |
    | Port | Wprowadź **80**.|
    | Interwał | Wprowadź **15** dla liczby **interwałów** (w sekundach) między kolejnymi próbami sondowania. |
    | Próg złej kondycji | Wybierz **2** dla liczby **progów złej kondycji** lub kolejnych niepowodzeń sondy, które muszą wystąpić, zanim maszyna wirtualna zostanie uznana za złą.|
    | | |

3. Pozostaw pozostałe wartości domyślne i wybierz **przycisk OK**.

### <a name="create-a-load-balancer-rule"></a>Tworzenie reguły modułu równoważenia obciążenia

Reguła modułu równoważenia obciążenia służy do definiowania sposobu dystrybucji ruchu do maszyn wirtualnych. Należy zdefiniować konfigurację adresu IP frontonu dla ruchu przychodzącego i pulę adresów IP zaplecza do odbierania ruchu. Port źródłowy i docelowy są zdefiniowane w regule. 

W tej sekcji utworzysz regułę modułu równoważenia obciążenia:

* O nazwie **myHTTPRule**.
* Na frontonie o nazwie **LoadBalancerFrontEnd**.
* Nasłuchiwanie na **porcie 80**.
* Kieruje ruch o zrównoważonym obciążeniu do zaplecza o nazwie **myBackendPool** na **porcie 80**.

1. W menu po lewej stronie wybierz pozycję Wszystkie **usługi** , wybierz pozycję **wszystkie zasoby**, a następnie wybierz pozycję **myLoadBalancer** z listy zasoby.

2. W obszarze **Ustawienia**wybierz pozycję **reguły równoważenia obciążenia**, a następnie wybierz pozycję **Dodaj**.

3. Użyj tych wartości, aby skonfigurować regułę równoważenia obciążenia:
    
    | Ustawienie | Wartość |
    | ------- | ----- |
    | Nazwa | Wprowadź **myHTTPRule**. |
    | Wersja protokołu IP | Wybierz **protokół IPv4** |
    | Adres IP frontonu | Wybierz **LoadBalancerFrontEnd** |
    | Protokół | Wybierz pozycję **TCP**. |
    | Port | Wprowadź **80**.|
    | Port zaplecza | Wprowadź **80**. |
    | Pula zaplecza | Wybierz pozycję **myBackendPool**.|
    | Sonda kondycji | Wybierz pozycję **myHealthProbe**. |
    | Utwórz niejawne reguły wychodzące | Wybierz pozycję **Nie**.

4. Pozostaw pozostałe wartości domyślne, a następnie wybierz przycisk **OK**.

## <a name="create-backend-servers"></a>Tworzenie serwerów zaplecza

W tej sekcji omówiono następujące zagadnienia:

* Utwórz dwie maszyny wirtualne dla puli zaplecza modułu równoważenia obciążenia.
* Zainstaluj usługi IIS na maszynach wirtualnych w celu przetestowania modułu równoważenia obciążenia.

### <a name="create-virtual-machines"></a>Tworzenie maszyn wirtualnych

W tej sekcji utworzysz dwie maszyny wirtualne (**myVM1** i **myVM2**) ze standardowym publicznym adresem IP w dwóch strefach (**Strefa 1** i **strefa 2**). 

Te maszyny wirtualne są dodawane do puli zaplecza modułu równoważenia obciążenia, który został utworzony wcześniej.

1. W lewym górnym rogu portalu wybierz pozycję **Utwórz zasób**  >  **obliczeniowy**  >  **maszyny wirtualnej**. 
   
2. W obszarze **Utwórz maszynę wirtualną**wpisz lub wybierz wartości z karty **podstawowe** :

    | Ustawienie | Wartość                                          |
    |-----------------------|----------------------------------|
    | **Szczegóły projektu** |  |
    | Subskrypcja | Wybierz subskrypcję platformy Azure |
    | Grupa zasobów | Wybierz **myResourceGroupLB** |
    | **Szczegóły wystąpienia** |  |
    | Nazwa maszyny wirtualnej | Wprowadź **myVM1** |
    | Region | Wybierz **Europa Zachodnia** |
    | Opcje dostępności | Wybierz **strefy dostępności** |
    | Strefa dostępności | Wybierz **1** |
    | Image (Obraz) | Wybierz pozycję **Windows Server 2019 Datacenter** |
    | Wystąpienie usługi Azure Spot | Wybierz pozycję **nie** |
    | Rozmiar | Wybierz rozmiar maszyny wirtualnej lub ustaw ustawienie domyślne |
    | **Konto administratora** |  |
    | Nazwa użytkownika | Wprowadź nazwę użytkownika |
    | Hasło | Wprowadź hasło |
    | Potwierdź hasło | Wprowadź ponownie hasło |

3. Wybierz kartę **Sieć** lub wybierz pozycję **Dalej: Dyski**, a następnie pozycję **Dalej: Sieć**.
  
4. Na karcie Sieć wybierz lub wprowadź:

    | Ustawienie | Wartość |
    |-|-|
    | **Interfejs sieciowy** |  |
    | Sieć wirtualna | **myVNet** |
    | Podsieć | **myBackendSubnet** |
    | Publiczny adres IP | Zaakceptuj wartość domyślną **myVM-IP**. </br> Adres IP automatycznie będzie adresem IP standardowej jednostki SKU w Strefa 1. |
    | Grupa zabezpieczeń sieci karty sieciowej | Wybierz pozycję **Zaawansowane**|
    | Konfigurowanie sieciowej grupy zabezpieczeń | Wybierz pozycję**Utwórz nowy**. </br> W **grupie Tworzenie zabezpieczeń sieci**wprowadź **MyNSG** w polu **Nazwa**. </br> Wybierz **przycisk OK** |
    | **Równoważenie obciążenia**  |
    | Umieścić tę maszynę wirtualną za istniejącym rozwiązaniem równoważenia obciążenia? | Wybierz opcję **tak** |
    | **Ustawienia równoważenia obciążenia** |
    | Opcje równoważenia obciążenia | Wybieranie **usługi Azure Load Balancing** |
    | Wybierz moduł równoważenia obciążenia | Wybierz **myLoadBalancer**  |
    | Wybierz pulę zaplecza | Wybierz **myBackendPool** |

5. Wybierz kartę **Zarządzanie** lub wybierz pozycję **Dalej** > **Zarządzanie**.

6. Na karcie **Zarządzanie** wybierz lub wprowadź:
    
    | Ustawienie | Wartość |
    |-|-|
    | **Monitorowanie** |  |
    | Diagnostyka rozruchu | Wybierz pozycję **wyłączone** |
   
7. Wybierz pozycję **Przegląd + utwórz**. 
  
8. Przejrzyj ustawienia, a następnie wybierz pozycję **Utwórz**.

9. Wykonaj kroki od 1 do 8, aby utworzyć jedną dodatkową maszynę wirtualną z następującymi wartościami, a wszystkie inne ustawienia tak samo jak **myVM1**:

    | Ustawienie | MW 2|
    | ------- | ----- |
    | Nazwa |  **myVM2** |
    | Strefa dostępności | **2** |
    | Sieciowa grupa zabezpieczeń | Wybierz istniejący **myNSG**|


# <a name="option-2-create-a-internal-load-balancer-basic-sku"></a>[Opcja 2: Tworzenie wewnętrznego modułu równoważenia obciążenia (podstawowa jednostka SKU)](#tab/option-1-create-internal-load-balancer-basic)

>[!NOTE]
>Moduł równoważenia obciążenia w warstwie Standardowa jest zalecany w przypadku obciążeń produkcyjnych.  Aby uzyskać więcej informacji o jednostkach SKU, zobacz **[Azure Load Balancer SKU](skus.md)**.

W tej sekcji utworzysz moduł równoważenia obciążenia, który równoważy obciążenie maszyn wirtualnych. 

Można utworzyć publiczny moduł równoważenia obciążenia lub wewnętrzny moduł równoważenia obciążenia. 

Podczas tworzenia wewnętrznego modułu równoważenia obciążenia Sieć wirtualna jest konfigurowana jako sieć dla modułu równoważenia obciążenia. 

Prywatny adres IP w sieci wirtualnej jest skonfigurowany jako fronton (domyślnie **LoadBalancerFrontend** ) dla usługi równoważenia obciążenia. 

Adres IP frontonu może być **statyczny** lub **dynamiczny**.

## <a name="virtual-network-and-parameters"></a>Sieć wirtualna i parametry

W tej sekcji zastąpisz parametry w krokach poniższymi informacjami:

| Parametr                   | Wartość                |
|-----------------------------|----------------------|
| **\<resource-group-name>**  | myResourceGroupLB |
| **\<virtual-network-name>** | myVNet          |
| **\<region-name>**          | West Europe      |
| **\<IPv4-address-space>**   | 10.1.0.0 \ 16          |
| **\<subnet-name>**          | myBackendSubnet        |
| **\<subnet-address-range>** | 10.1.0.0 \ 24          |

[!INCLUDE [virtual-networks-create-new](../../includes/virtual-networks-create-new.md)]

## <a name="create-load-balancer"></a>Tworzenie modułu równoważenia obciążenia

1. W lewym górnym rogu ekranu wybierz pozycję **Utwórz zasób zasobów**  >  **Networking**  >  **Load Balancer**.

2. Na karcie **podstawy** na stronie **Tworzenie modułu równoważenia obciążenia** wprowadź lub wybierz następujące informacje: 

    | Ustawienie                 | Wartość                                              |
    | ---                     | ---                                                |
    | Subskrypcja               | Wybierz subskrypcję.    |    
    | Grupa zasobów         | Wybierz **myResourceGroupLB** utworzone w poprzednim kroku.|
    | Nazwa                   | Wprowadź **myLoadBalancer**                                   |
    | Region         | Wybierz pozycję **Europa Zachodnia**.                                        |
    | Typ          | wybierz pozycję **Wewnętrzny**.                                        |
    | SKU           | Wybierz pozycję **podstawowa** |
    | Sieć wirtualna | Wybierz **myVNet** utworzone w poprzednim kroku. |
    | Podsieć  | Wybierz **myBackendSubnet** utworzone w poprzednim kroku. |
    | Przypisanie adresu IP | wybierz pozycję **Dynamiczne**. |

3. Zaakceptuj wartości domyślne pozostałych ustawień, a następnie wybierz pozycję **Przegląd + Utwórz**.

4. Na karcie **Recenzja i tworzenie** wybierz pozycję **Utwórz**.   

    :::image type="content" source="./media/quickstart-load-balancer-standard-internal-portal/create-basic-internal-load-balancer.png" alt-text="Tworzenie standardowego wewnętrznego modułu równoważenia obciążenia" border="true":::

## <a name="create-load-balancer-resources"></a>Tworzenie zasobów modułu równoważenia obciążenia

W tej sekcji skonfigurujesz:

* Ustawienia usługi równoważenia obciążenia dla puli adresów zaplecza.
* Sonda kondycji.
* Reguła modułu równoważenia obciążenia.

### <a name="create-a-backend-pool"></a>Tworzenie puli zaplecza

Pula adresów zaplecza zawiera adresy IP wirtualnych (kart sieciowych) podłączonych do modułu równoważenia obciążenia. 

Utwórz pulę adresów zaplecza **myBackendPool** , aby uwzględnić maszyny wirtualne na potrzeby ruchu internetowego związanego z równoważeniem obciążenia.

1. W menu po lewej stronie wybierz pozycję Wszystkie **usługi** , wybierz pozycję **wszystkie zasoby**, a następnie wybierz pozycję **myLoadBalancer** z listy zasoby.

2. W obszarze **Ustawienia**wybierz pozycję **Pule zaplecza**, a następnie wybierz pozycję **Dodaj**.

3. Na stronie **Dodawanie puli zaplecza** wprowadź lub wybierz:
    
    | Ustawienie | Wartość |
    | ------- | ----- |
    | Nazwa | Wprowadź **myBackendPool**. |
    | Sieć wirtualna | Wybierz pozycję **myVNet**. |
    | Skojarzone z | Wybierz **maszyny wirtualne** |

4. Wybierz pozycję **Dodaj**.

### <a name="create-a-health-probe"></a>Tworzenie sondy kondycji

Moduł równoważenia obciążenia monitoruje stan aplikacji przy użyciu sondy kondycji. 

Sonda kondycji dodaje lub usuwa maszyny wirtualne z modułu równoważenia obciążenia na podstawie odpowiedzi na testy kondycji. 

Utwórz sondę kondycji o nazwie **myHealthProbe**, aby monitorować kondycję maszyn wirtualnych.

1. W menu po lewej stronie wybierz pozycję Wszystkie **usługi** , wybierz pozycję **wszystkie zasoby**, a następnie wybierz pozycję **myLoadBalancer** z listy zasoby.

2. W obszarze **Ustawienia**wybierz pozycję **sondy kondycji**, a następnie wybierz pozycję **Dodaj**.
    
    | Ustawienie | Wartość |
    | ------- | ----- |
    | Nazwa | Wprowadź **myHealthProbe**. |
    | Protokół | Wybierz pozycję **http**. |
    | Port | Wprowadź **80**.|
    | Ścieżka | Wejść**/** |
    | Interwał | Wprowadź **15** dla liczby **interwałów** (w sekundach) między kolejnymi próbami sondowania. |
    | Próg złej kondycji | Wybierz **2** dla liczby **progów złej kondycji** lub kolejnych niepowodzeń sondy, które muszą wystąpić, zanim maszyna wirtualna zostanie uznana za złą.|

3. Wybierz przycisk **OK**.

### <a name="create-a-load-balancer-rule"></a>Tworzenie reguły modułu równoważenia obciążenia

Reguła modułu równoważenia obciążenia służy do definiowania sposobu dystrybucji ruchu do maszyn wirtualnych. Należy zdefiniować konfigurację adresu IP frontonu dla ruchu przychodzącego i pulę adresów IP zaplecza do odbierania ruchu. Port źródłowy i docelowy są zdefiniowane w regule. 

W tej sekcji utworzysz regułę modułu równoważenia obciążenia:

* O nazwie **myHTTPRule**.
* Na frontonie o nazwie **LoadBalancerFrontEnd**.
* Nasłuchiwanie na **porcie 80**.
* Kieruje ruch o zrównoważonym obciążeniu do zaplecza o nazwie **myBackendPool** na **porcie 80**.

1. W menu po lewej stronie wybierz pozycję Wszystkie **usługi** , wybierz pozycję **wszystkie zasoby**, a następnie wybierz pozycję **myLoadBalancer** z listy zasoby.

2. W obszarze **Ustawienia**wybierz pozycję **reguły równoważenia obciążenia**, a następnie wybierz pozycję **Dodaj**.

3. Użyj tych wartości, aby skonfigurować regułę równoważenia obciążenia:
    
    | Ustawienie | Wartość |
    | ------- | ----- |
    | Nazwa | Wprowadź **myHTTPRule**. |
    | Wersja protokołu IP | Wybierz **protokół IPv4** |
    | Adres IP frontonu | Wybierz **LoadBalancerFrontEnd** |
    | Protokół | Wybierz pozycję **TCP**. |
    | Port | Wprowadź **80**.|
    | Port zaplecza | Wprowadź **80**. |
    | Pula zaplecza | Wybierz pozycję **myBackendPool**.|
    | Sonda kondycji | Wybierz pozycję **myHealthProbe**. |
 
4. Pozostaw pozostałe wartości domyślne, a następnie wybierz przycisk **OK**.

## <a name="create-backend-servers"></a>Tworzenie serwerów zaplecza

W tej sekcji omówiono następujące zagadnienia:

* Utwórz dwie maszyny wirtualne dla puli zaplecza modułu równoważenia obciążenia.
* Utwórz zestaw dostępności dla maszyn wirtualnych.
* Zainstaluj usługi IIS na maszynach wirtualnych w celu przetestowania modułu równoważenia obciążenia.

### <a name="create-virtual-machines"></a>Tworzenie maszyn wirtualnych

Jednostki SKU publicznego adresu IP i jednostki SKU modułu równoważenia obciążenia muszą być zgodne. W przypadku podstawowego modułu równoważenia obciążenia Użyj maszyn wirtualnych z podstawowymi adresami IP w puli zaplecza. 

W tej sekcji utworzysz dwie maszyny wirtualne (**myVM1**i **myVM2**) z podstawowym publicznym adresem IP.  

Dwie maszyny wirtualne zostaną dodane do zestawu dostępności o nazwie **myAvailabilitySet**.

Te maszyny wirtualne są dodawane do puli zaplecza modułu równoważenia obciążenia, który został utworzony wcześniej.

1. W lewym górnym rogu portalu wybierz pozycję **Utwórz zasób**  >  **obliczeniowy**  >  **maszyny wirtualnej**. 
   
2. W obszarze **Utwórz maszynę wirtualną**wpisz lub wybierz wartości z karty **podstawowe** :

    | Ustawienie | Wartość                                          |
    |-----------------------|----------------------------------|
    | **Szczegóły projektu** |  |
    | Subskrypcja | Wybierz subskrypcję platformy Azure |
    | Grupa zasobów | Wybierz **myResourceGroupLB** |
    | **Szczegóły wystąpienia** |  |
    | Nazwa maszyny wirtualnej | Wprowadź **myVM1** |
    | Region | Wybierz **Europa Zachodnia** |
    | Opcje dostępności | Wybierz pozycję **Zestaw dostępności** |
    | Zestaw dostępności | Wybierz pozycję**Utwórz nowy**. </br> Wprowadź **myAvailabilitySet** w polu **Nazwa**. </br> Wybierz **przycisk OK** |
    | Image (Obraz) | **Windows Server 2019 Datacenter** |
    | Wystąpienie usługi Azure Spot | Wybierz pozycję **nie** |
    | Rozmiar | Wybierz rozmiar maszyny wirtualnej lub ustaw ustawienie domyślne |
    | **Konto administratora** |  |
    | Nazwa użytkownika | Wprowadź nazwę użytkownika |
    | Hasło | Wprowadź hasło |
    | Potwierdź hasło | Wprowadź ponownie hasło |

3. Wybierz kartę **Sieć** lub wybierz pozycję **Dalej: Dyski**, a następnie pozycję **Dalej: Sieć**.
  
4. Na karcie Sieć wybierz lub wprowadź:

    | Ustawienie | Wartość |
    |-|-|
    | **Interfejs sieciowy** |  |
    | Sieć wirtualna | Wybierz **myVNet** |
    | Podsieć | Wybierz **myBackendSubnet** |
    | Publiczny adres IP | Wybierz pozycję **Utwórz nowy** </br> Wprowadź **myVM-IP** w polu Nazwa. </br> Wybierz **przycisk OK** |
    | Grupa zabezpieczeń sieci karty sieciowej | Wybierz pozycję **Zaawansowane**|
    | Konfigurowanie sieciowej grupy zabezpieczeń | Wybierz pozycję**Utwórz nowy**. </br> W **grupie Tworzenie zabezpieczeń sieci**wprowadź **MyNSG** w polu **Nazwa**. </br> Wybierz **przycisk OK** |
    | **Równoważenie obciążenia**  |
    | Umieścić tę maszynę wirtualną za istniejącym rozwiązaniem równoważenia obciążenia? | Wybierz pozycję **nie** |
 
5. Wybierz kartę **Zarządzanie** lub wybierz pozycję **Dalej** > **Zarządzanie**.

6. Na karcie **Zarządzanie** wybierz lub wprowadź:
    | Ustawienie | Wartość |
    |-|-|
    | **Monitorowanie** | |
    | Diagnostyka rozruchu | Wybierz pozycję **wyłączone** |

7. Wybierz pozycję **Przegląd + utwórz**. 
  
8. Przejrzyj ustawienia, a następnie wybierz pozycję **Utwórz**.

9. Wykonaj kroki od 1 do 8, aby utworzyć jedną dodatkową maszynę wirtualną z następującymi wartościami, a wszystkie inne ustawienia tak samo jak **myVM1**:

    | Ustawienie | MW 2 |
    | ------- | ----- |
    | Nazwa |  **myVM2** |
    | Zestaw dostępności| Wybierz **myAvailabilitySet** |
    | Sieciowa grupa zabezpieczeń | Wybierz istniejący **myNSG**|

### <a name="add-virtual-machines-to-the-backend-pool"></a>Dodawanie maszyn wirtualnych do puli zaplecza

Maszyny wirtualne utworzone w poprzednich krokach należy dodać do puli zaplecza **myLoadBalancer**.

1. W menu po lewej stronie wybierz pozycję Wszystkie **usługi** , wybierz pozycję **wszystkie zasoby**, a następnie wybierz pozycję **myLoadBalancer** z listy zasoby.

2. W obszarze **Ustawienia**wybierz pozycję **Pule zaplecza**, a następnie wybierz pozycję **myBackendPool**.

3. Wybierz **maszyny wirtualne** **powiązane**z.

4. W sekcji **maszyny wirtualne** wybierz pozycję **+ Dodaj**.

5. Zaznacz pola obok pozycji **myVM1** i **myVM2**.

6. Wybierz pozycję **Dodaj**.

7. Wybierz pozycję **Zapisz**.
---

## <a name="create-test-virtual-machine"></a>Utwórz testową maszynę wirtualną

W tej sekcji utworzysz maszynę wirtualną o nazwie **myTestVM**.  Ta maszyna wirtualna zostanie użyta do przetestowania konfiguracji modułu równoważenia obciążenia.

1. W lewym górnym rogu portalu wybierz pozycję **Utwórz zasób**  >  **obliczeniowy**  >  **maszyny wirtualnej**. 
   
2. W obszarze **Utwórz maszynę wirtualną**wpisz lub wybierz wartości z karty **podstawowe** :

    | Ustawienie | Wartość                                          |
    |-----------------------|----------------------------------|
    | **Szczegóły projektu** |  |
    | Subskrypcja | Wybierz subskrypcję platformy Azure |
    | Grupa zasobów | Wybierz **myResourceGroupLB** |
    | **Szczegóły wystąpienia** |  |
    | Nazwa maszyny wirtualnej | Wprowadź **myTestVM** |
    | Region | Wybierz **Europa Zachodnia** |
    | Opcje dostępności | Nie wybieraj **nadmiarowości infrastruktury** |
    | Strefa dostępności | Wybierz **strefę nadmiarową** |
    | Image (Obraz) | Wybierz pozycję **Windows Server 2019 Datacenter** |
    | Wystąpienie usługi Azure Spot | Wybierz pozycję **nie** |
    | Rozmiar | Wybierz rozmiar maszyny wirtualnej lub ustaw ustawienie domyślne |
    | **Konto administratora** |  |
    | Nazwa użytkownika | Wprowadź nazwę użytkownika |
    | Hasło | Wprowadź hasło |
    | Potwierdź hasło | Wprowadź ponownie hasło |

3. Wybierz kartę **Sieć** lub wybierz pozycję **Dalej: Dyski**, a następnie pozycję **Dalej: Sieć**.
  
4. Na karcie Sieć wybierz lub wprowadź:

    | Ustawienie | Wartość |
    |-|-|
    | **Interfejs sieciowy** |  |
    | Sieć wirtualna | **myVNet** |
    | Podsieć | **myBackendSubnet** |
    | Publiczny adres IP | Zaakceptuj wartość domyślną **myTestVM-IP**. |
    | Grupa zabezpieczeń sieci karty sieciowej | Wybierz pozycję **Zaawansowane**|
    | Konfigurowanie sieciowej grupy zabezpieczeń | Wybierz **MyNSG** utworzone w poprzednim kroku.|
    
5. Wybierz kartę **Zarządzanie** lub wybierz pozycję **Dalej** > **Zarządzanie**.

6. Na karcie **Zarządzanie** wybierz lub wprowadź:
    
    | Ustawienie | Wartość |
    |-|-|
    | **Monitorowanie** |  |
    | Diagnostyka rozruchu | Wybierz pozycję **wyłączone** |
   
7. Wybierz pozycję **Przegląd + utwórz**. 
  
8. Przejrzyj ustawienia, a następnie wybierz pozycję **Utwórz**.

## <a name="install-iis"></a>Instalowanie usług IIS

1. Wybierz pozycję **wszystkie usługi** w menu po lewej stronie, wybierz pozycję **wszystkie zasoby**, a następnie na liście zasobów wybierz pozycję **myVM1** , która znajduje się w grupie zasobów **myResourceGroupLB** .

2. Na stronie **Przegląd** wybierz pozycję **Połącz** , aby pobrać plik RDP dla maszyny wirtualnej.

3. Otwórz plik RDP.

4. Zaloguj się do maszyny wirtualnej przy użyciu poświadczeń podanych podczas jej tworzenia.

5. Na pulpicie serwera przejdź do **narzędzi administracyjnych systemu Windows** > **programu Windows PowerShell**.

6. W oknie programu PowerShell uruchom następujące polecenia, aby:

    * Zainstaluj serwer IIS
    * Usuń domyślny plik iisstart.htm
    * Dodaj nowy plik iisstart.htm, który wyświetla nazwę maszyny wirtualnej:

   ```powershell
    
    # install IIS server role
    Install-WindowsFeature -name Web-Server -IncludeManagementTools
    
    # remove default htm file
    remove-item  C:\inetpub\wwwroot\iisstart.htm
    
    # Add a new htm file that displays server name
    Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Hello World from " + $env:computername)
   ```
7. Zamknij sesję protokołu RDP na maszynie wirtualnej **myVM1**.

8. Powtórz kroki od 1 do 6, aby zainstalować usługi IIS i zaktualizowany plik iisstart.htm na maszynie wirtualnej **myVM2**.

## <a name="test-the-load-balancer"></a>Testowanie modułu równoważenia obciążenia

1. Znajdź prywatny adres IP dla modułu równoważenia obciążenia na ekranie **Przegląd** . Wybierz pozycję **wszystkie usługi** w menu po lewej stronie, wybierz pozycję **wszystkie zasoby**, a następnie wybierz pozycję **myLoadBalancer**.

2. Zanotuj lub skopiuj adres obok **prywatnego adresu IP** w **omówieniu** **myLoadBalancer**.

3. Wybierz pozycję **wszystkie usługi** w menu po lewej stronie, wybierz pozycję **wszystkie zasoby**, a następnie na liście zasobów wybierz pozycję **myTestVM** , która znajduje się w grupie zasobów **myResourceGroupLB** .

4. Na stronie **Przegląd** wybierz pozycję **Połącz** , aby pobrać plik RDP dla maszyny wirtualnej.

5. Otwórz plik RDP.

6. Zaloguj się do maszyny wirtualnej przy użyciu poświadczeń podanych podczas jej tworzenia.

7. Otwórz program **Internet Explorer** w systemie **myTestVM**.

4. Skopiuj prywatny adres IP, a następnie wklej go na pasku adresu przeglądarki. W przeglądarce jest wyświetlana domyślna strona internetowego serwera usług IIS.

    :::image type="content" source="./media/quickstart-load-balancer-standard-internal-portal/load-balancer-test.png" alt-text="Tworzenie standardowego wewnętrznego modułu równoważenia obciążenia" border="true":::
   
Aby zobaczyć, jak moduł równoważenia obciążenia dystrybuuje ruch między wszystkimi trzema maszynami wirtualnymi, można dostosować domyślną stronę każdego z serwerów sieci Web usług IIS na maszynie wirtualnej, a następnie wymusić odświeżenie przeglądarki sieci Web na komputerze klienckim.

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Gdy grupa zasobów, moduł równoważenia obciążenia i wszystkie pokrewne zasoby nie będą już potrzebne, usuń je. W tym celu wybierz grupę zasobów **myResourceGroupLB** , która zawiera zasoby, a następnie wybierz pozycję **Usuń**.

## <a name="next-steps"></a>Następne kroki

W ramach tego przewodnika Szybki start wykonasz następujące czynności:

* Utworzono wewnętrzną Load Balancer w warstwie Standardowa lub podstawowa platformy Azure
* Dołączono 2 maszyny wirtualne do modułu równoważenia obciążenia.
* Skonfigurowano regułę ruchu modułu równoważenia obciążenia, sondę kondycji, a następnie przetestowano moduł równoważenia obciążenia. 

Aby dowiedzieć się więcej na temat Azure Load Balancer, przejdź do [co Azure Load Balancer?](load-balancer-overview.md) i [Load Balancer często zadawanych pytań](load-balancer-faqs.md).

Dowiedz się więcej na temat [stref Load Balancer i dostępności](load-balancer-standard-availability-zones.md).
