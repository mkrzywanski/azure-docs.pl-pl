---
title: Reguły zapory Event Hubs platformy Azure | Microsoft Docs
description: Użyj reguł zapory, aby zezwolić na połączenia z określonych adresów IP z platformą Azure Event Hubs.
ms.topic: article
ms.date: 07/16/2020
ms.openlocfilehash: 8ba327c78888d9e5f38693ae08a2022b2d1543a4
ms.sourcegitcommit: 1b2d1755b2bf85f97b27e8fbec2ffc2fcd345120
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/04/2020
ms.locfileid: "87553125"
---
# <a name="allow-access-to-azure-event-hubs-namespaces-from-specific-ip-addresses-or-ranges"></a>Zezwalaj na dostęp do przestrzeni nazw platformy Azure Event Hubs z określonych adresów IP lub zakresów
Domyślnie obszary nazw Event Hubs są dostępne z Internetu, o ile żądanie zawiera prawidłowe uwierzytelnianie i autoryzację. Za pomocą zapory IP można ograniczyć ją do tylko zestawu adresów IPv4 lub zakresów adresów IPv4 w notacji [CIDR (bez klas routingu między domenami)](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) .

Ta funkcja jest przydatna w scenariuszach, w których usługa Azure Event Hubs powinna być dostępna tylko z niektórych dobrze znanych lokacji. Reguły zapory umożliwiają konfigurowanie reguł w celu akceptowania ruchu pochodzącego z określonych adresów IPv4. Na przykład jeśli używasz Event Hubs z usługą [Azure Express Route][express-route], możesz utworzyć **regułę zapory** , aby zezwolić na ruch tylko z adresów IP infrastruktury lokalnej. 

>[!WARNING]
> Włączenie filtrowania adresów IP może uniemożliwić innym usługom platformy Azure współdziałanie z Event Hubs.
>
> Zaufane usługi firmy Microsoft nie są obsługiwane w przypadku implementacji sieci wirtualnych.
>
> Typowe scenariusze platformy Azure, które nie współpracują z sieciami wirtualnymi (należy zauważyć, że lista **nie** jest wyczerpująca) —
> - Usługa Azure Stream Analytics
> - Trasy usługi Azure IoT Hub
> - Device Explorer usługi Azure IoT
>
> Następujące usługi firmy Microsoft muszą znajdować się w sieci wirtualnej
> - Aplikacje internetowe platformy Azure
> - Azure Functions
> - Azure Monitor (ustawienie diagnostyczne)


## <a name="ip-firewall-rules"></a>Reguły zapory adresów IP
Reguły zapory adresów IP są stosowane na poziomie przestrzeni nazw Event Hubs. W związku z tym reguły są stosowane do wszystkich połączeń z klientów przy użyciu dowolnego obsługiwanego protokołu. Wszystkie próby połączenia z adresu IP, które nie pasują do dozwolonej reguły adresów IP w przestrzeni nazw Event Hubs, są odrzucane jako nieautoryzowane. Odpowiedź nie zawiera wzmianki o regule adresów IP. Reguły filtrowania adresów IP są stosowane w podanej kolejności, a pierwsza reguła zgodna z adresem IP określa akcję Akceptuj lub Odrzuć.

## <a name="use-azure-portal"></a>Korzystanie z witryny Azure Portal
W tej sekcji pokazano, jak za pomocą Azure Portal utworzyć reguły zapory IP dla Event Hubs przestrzeni nazw. 

1. Przejdź do **przestrzeni nazw Event Hubs** w [Azure Portal](https://portal.azure.com).
4. Wybierz pozycję **Sieć** w obszarze **Ustawienia** w menu po lewej stronie. 

    > [!NOTE]
    > Karta **Sieć** jest wyświetlana tylko dla **standardowych** lub **dedykowanych** przestrzeni nazw. 

    Domyślnie wybrana jest opcja **wybrane sieci** . Jeśli nie określisz reguły zapory adresu IP lub nie dodasz sieci wirtualnej na tej stronie, można uzyskać dostęp do przestrzeni nazw za pośrednictwem publicznej sieci Internet (przy użyciu klucza dostępu). 

    :::image type="content" source="./media/event-hubs-firewall/selected-networks.png" alt-text="Karta sieci — opcja wybrane sieci" lightbox="./media/event-hubs-firewall/selected-networks.png":::    

    W przypadku wybrania opcji **wszystkie sieci** centrum zdarzeń akceptuje połączenia z dowolnego adresu IP (przy użyciu klucza dostępu). To ustawienie jest równoważne z regułą akceptującą zakres adresów IP 0.0.0.0/0. 

    ![Zapora — wybrana opcja Wszystkie sieci](./media/event-hubs-firewall/firewall-all-networks-selected.png)
1. Aby ograniczyć dostęp do określonych adresów IP, upewnij się, że wybrano opcję **wybrane sieci** . W sekcji **Zapora** wykonaj następujące kroki:
    1. Wybierz opcję **Dodaj adres IP klienta** , aby zapewnić bieżącemu adresowi IP klienta dostęp do przestrzeni nazw. 
    2. W polu **zakres adresów**wprowadź określony adres IPv4 lub zakres adresów IPv4 w notacji CIDR. 
    3. Określ, czy chcesz **zezwolić zaufanym usługom firmy Microsoft na ominięcie tej zapory**. 

        ![Zapora — wybrana opcja Wszystkie sieci](./media/event-hubs-firewall/firewall-selected-networks-trusted-access-disabled.png)
3. Wybierz pozycję **Zapisz** na pasku narzędzi, aby zapisać ustawienia. Poczekaj kilka minut, aż potwierdzenie będzie widoczne na powiadomieniach portalu.

    > [!NOTE]
    > Aby ograniczyć dostęp do określonych sieci wirtualnych, zobacz [Zezwalanie na dostęp z określonych sieci](event-hubs-service-endpoints.md).

## <a name="use-resource-manager-template"></a>Używanie szablonu usługi Resource Manager

> [!IMPORTANT]
> Reguły zapory są obsługiwane w warstwach **standardowa** i **dedykowana** Event Hubs. Nie są obsługiwane w warstwie Podstawowa.

Poniższy szablon Menedżer zasobów umożliwia dodanie reguły filtru IP do istniejącej przestrzeni nazw Event Hubs.

Parametry szablonu:

- **ipMask** to pojedynczy adres IPv4 lub blok adresów IP w notacji CIDR. Na przykład w notacji CIDR 70.37.104.0/24 reprezentuje adresy IPv4 256 z 70.37.104.0 do 70.37.104.255, z 24 wskazujące liczbę znaczących bitów prefiksu dla zakresu.

> [!NOTE]
> Chociaż nie ma możliwych reguł Odmów, szablon Azure Resource Manager ma ustawioną akcję domyślną **"Zezwalaj"** , która nie ogranicza połączeń.
> Podczas tworzenia reguł Virtual Network lub zapór należy zmienić wartość ***"DefaultAction"***
> 
> wniosek
> ```json
> "defaultAction": "Allow"
> ```
> na
> ```json
> "defaultAction": "Deny"
> ```
>

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "eventhubNamespaceName": {
        "type": "string",
        "metadata": {
          "description": "Name of the Event Hubs namespace"
        }
      },
      "location": {
        "type": "string",
        "metadata": {
          "description": "Location for Namespace"
        }
      }
    },
    "variables": {
      "namespaceNetworkRuleSetName": "[concat(parameters('eventhubNamespaceName'), concat('/', 'default'))]",
    },
    "resources": [
      {
        "apiVersion": "2018-01-01-preview",
        "name": "[parameters('eventhubNamespaceName')]",
        "type": "Microsoft.EventHub/namespaces",
        "location": "[parameters('location')]",
        "sku": {
          "name": "Standard",
          "tier": "Standard"
        },
        "properties": { }
      },
      {
        "apiVersion": "2018-01-01-preview",
        "name": "[variables('namespaceNetworkRuleSetName')]",
        "type": "Microsoft.EventHub/namespaces/networkruleset",
        "dependsOn": [
          "[concat('Microsoft.EventHub/namespaces/', parameters('eventhubNamespaceName'))]"
        ],
        "properties": {
          "virtualNetworkRules": [<YOUR EXISTING VIRTUAL NETWORK RULES>],
          "ipRules": 
          [
            {
                "ipMask":"10.1.1.1",
                "action":"Allow"
            },
            {
                "ipMask":"11.0.0.0/24",
                "action":"Allow"
            }
          ],
          "trustedServiceAccessEnabled": false,
          "defaultAction": "Deny"
        }
      }
    ],
    "outputs": { }
  }
```

Aby wdrożyć szablon, postępuj zgodnie z instrukcjami dotyczącymi [Azure Resource Manager][lnk-deploy].

## <a name="next-steps"></a>Następne kroki

Aby ograniczyć dostęp do Event Hubs do sieci wirtualnych platformy Azure, Skorzystaj z następującego linku:

- [Virtual Network punkty końcowe usługi dla Event Hubs][lnk-vnet]

<!-- Links -->

[express-route]:  /azure/expressroute/expressroute-faqs#supported-services
[lnk-deploy]: ../azure-resource-manager/templates/deploy-powershell.md
[lnk-vnet]: event-hubs-service-endpoints.md
