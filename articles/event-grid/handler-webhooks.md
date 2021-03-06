---
title: Elementy webhook jako programy obsługi zdarzeń dla zdarzeń Azure Event Grid
description: Opisuje, w jaki sposób można używać elementów webhook jako programów obsługi zdarzeń dla zdarzeń Azure Event Grid. Elementy Runbook Azure Automation i Aplikacje logiki są obsługiwane jako programy obsługi zdarzeń za pośrednictwem elementów webhook.
ms.topic: conceptual
ms.date: 07/07/2020
ms.openlocfilehash: 3dc75b15c55c82b6164e77f0ac93a87bced94a64
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/08/2020
ms.locfileid: "86105612"
---
# <a name="webhooks-automation-runbooks-logic-apps-as-event-handlers-for-azure-event-grid-events"></a>Webhook, elementy Runbook usługi Automation, Logic Apps jako programy obsługi zdarzeń dla zdarzeń Azure Event Grid
Program obsługi zdarzeń jest miejscem, w którym zdarzenie jest wysyłane. Procedura obsługi wykonuje kilka dalszych działań w celu przetworzenia zdarzenia. Kilka usług platformy Azure jest automatycznie konfigurowanych do obsługi zdarzeń. Możesz również użyć dowolnego elementu webhook do obsługi zdarzeń. Elementy webhook nie muszą być hostowane na platformie Azure, aby obsługiwać zdarzenia. Event Grid obsługuje tylko punkty końcowe elementu webhook protokołu HTTPS.

> [!NOTE]
> Elementy Runbook Azure Automation i Aplikacje logiki są obsługiwane jako programy obsługi zdarzeń za pośrednictwem elementów webhook. 

## <a name="webhooks"></a>Elementy webhook
Zobacz następujące artykuły, aby zapoznać się z omówieniem i przykładami używania elementów webhook jako programów obsługi zdarzeń. 

|Tytuł  |Opis  |
|---------|---------|
| Szybki Start: Tworzenie i kierowanie zdarzeń niestandardowych za pomocą [interfejsu wiersza polecenia platformy Azure](custom-event-quickstart.md), [programu PowerShell](custom-event-quickstart-powershell.md)i [portalu](custom-event-quickstart-portal.md). | Pokazuje, jak wysyłać zdarzenia niestandardowe do elementu webhook. |
| Szybki Start: kierowanie zdarzeń usługi BLOB Storage do niestandardowego punktu końcowego sieci Web przy użyciu [interfejsu wiersza polecenia platformy Azure](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json), [programu PowerShell](../storage/blobs/storage-blob-event-quickstart-powershell.md?toc=%2fazure%2fevent-grid%2ftoc.json)i [portalu](blob-event-quickstart-portal.md). | Pokazuje, jak wysyłać zdarzenia magazynu obiektów BLOB do elementu webhook. |
| [Szybki Start: wysyłanie zdarzeń rejestru kontenera](../container-registry/container-registry-event-grid-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Pokazuje, w jaki sposób używać interfejsu wiersza polecenia platformy Azure do wysyłania zdarzeń Container Registry. |
| [Przegląd: odbieranie zdarzeń do punktu końcowego HTTP](receive-events.md) | Opisuje sposób sprawdzania poprawności punktu końcowego HTTP w celu odbierania zdarzeń z subskrypcji zdarzeń oraz odbierania i deserializacji zdarzeń. |


## <a name="azure-automation"></a>Azure Automation
Zdarzenia można przetwarzać za pomocą Azure Automation elementów Runbook. Przetwarzanie zdarzeń przy użyciu zautomatyzowanych elementów Runbook jest obsługiwane przez elementy webhook. Tworzysz element webhook dla elementu Runbook, a następnie użyj procedury obsługi elementu webhook. Przykład można znaleźć na poniższym samouczku: 

|Tytuł  |Opis  |
|---------|---------|
|[Samouczek: Azure Automation z Event Grid i Microsoft Teams](ensure-tags-exists-on-new-virtual-machines.md) |Utwórz maszynę wirtualną, która wysyła zdarzenie. Zdarzenie wyzwala element Runbook usługi Automation, który oznacza maszynę wirtualną, i wyzwala komunikat wysyłany do kanału Microsoft Teams. |


## <a name="logic-apps"></a>Logic Apps
Użyj **Logic Apps** , aby zaimplementować procesy biznesowe do przetwarzania zdarzeń Event Grid. Nie utworzysz elementu webhook jawnie w tym scenariuszu. Element webhook jest tworzony automatycznie podczas konfigurowania aplikacji logiki do obsługi zdarzeń z Event Grid. Przykłady można znaleźć w następujących samouczkach: 

|Tytuł  |Opis  |
|---------|---------|
| [Samouczek: monitorowanie zmian maszyny wirtualnej przy użyciu Azure Event Grid i Logic Apps](monitor-virtual-machine-changes-event-grid-logic-app.md) | Aplikacja logiki monitoruje zmiany w maszynie wirtualnej i wysyła wiadomości e-mail o tych zmianach. |
| [Samouczek: wysyłanie powiadomień e-mail dotyczących zdarzeń usługi Azure IoT Hub przy użyciu Logic Apps](publish-iot-hub-events-to-logic-apps.md) | Aplikacja logiki wysyła wiadomość e-mail z powiadomieniem za każdym razem, gdy urządzenie zostanie dodane do centrum IoT. |
| [Samouczek: reagowanie na zdarzenia Azure Service Bus odbierane za pośrednictwem Azure Event Grid przy użyciu Azure Functions i Azure Logic Apps](../service-bus-messaging/service-bus-to-event-grid-integration-example.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Event Grid wysyła wiadomości z tematu Service Bus do aplikacji funkcji i aplikacji logiki. |

## <a name="rest-example-for-put"></a>Przykład REST (na potrzeby PUT)

```json
{
    "properties": 
    {
        "destination": 
        {
            "endpointType": "WebHook",
            "properties": 
            {
                "endpointUrl": "<WEB HOOK URL>",
                "maxEventsPerBatch": 1,
                "preferredBatchSizeInKilobytes": 64
            }
        },
        "eventDeliverySchema": "EventGridSchema"
    }
}
```

## <a name="next-steps"></a>Następne kroki
Listę obsługiwanych programów obsługi zdarzeń zawiera artykuł [obsługi zdarzeń](event-handlers.md) . 
