---
title: Azure Service Bus-wstrzymywanie jednostek obsługi komunikatów
description: W tym artykule wyjaśniono, jak tymczasowo wstrzymywać i ponownie aktywować Azure Service Bus jednostki komunikatów (kolejki, tematy i subskrypcje).
ms.topic: article
ms.date: 06/23/2020
ms.openlocfilehash: da7c25c8ef9f3daa32984ac26aa62710ab775951
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/23/2020
ms.locfileid: "87038141"
---
# <a name="suspend-and-reactivate-messaging-entities-disable"></a>Wstrzymywanie i ponowne uaktywnianie jednostek obsługi komunikatów (wyłączone)

Kolejki, tematy i subskrypcje mogą być tymczasowo zawieszone. Zawieszenie umieszcza jednostkę w stanie wyłączonym, w którym wszystkie komunikaty są przechowywane w magazynie. Jednak wiadomości nie mogą zostać usunięte ani dodane, a odpowiednie operacje protokołu zwracają błędy.

Zawieszenie jednostki jest zwykle wykonywane z pilnymi przyczynami administracyjnymi. W jednym scenariuszu wdrożono wadliwy odbiornik, który pobiera komunikaty z kolejki, kończy się niepowodzeniem, a następnie nieprawidłowo wypełnia komunikaty i usuwa je. W przypadku zdiagnozowania tego zachowania Kolejka może zostać wyłączona dla odbieranych danych, dopóki nie zostanie wdrożony skorygowany kod i będzie można zapobiec dalszej utracie danych spowodowanych przez błędny kod.

Zawieszenie lub ponowna aktywacja może zostać wykonana przez użytkownika lub przez system. System zawiesza jedynie jednostki z powodu poważnego działania administracyjnego, takiego jak nakroczenie limitu wydatków na subskrypcję. Jednostki wyłączone przez system nie mogą być ponownie uaktywniane przez użytkownika, ale są przywracane po rozdaniu przyczyny zawieszenia.

W portalu, sekcja **Właściwości** dla odpowiedniej jednostki umożliwia zmianę stanu; Poniższy zrzut ekranu przedstawia przełącznik dla kolejki:

![Zrzut ekranu przedstawiający funkcję Service Bus z wyróżnionym ustawieniem właściwości i opcją stanu kolejki ustawioną jako aktywny i wyróżnioną kolorem czerwonym.][1]

Portal zezwala tylko na całkowite wyłączenie kolejek. Operacje wysyłania i odbierania można także wyłączać oddzielnie przy użyciu interfejsów API Service Bus [NamespaceManager](/dotnet/api/microsoft.servicebus.namespacemanager) w zestawie .NET Framework SDK lub z szablonem Azure Resource Manager za pomocą interfejsu wiersza polecenia platformy Azure lub Azure PowerShell.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="suspension-states"></a>Stany zawieszenia

Stany, które można ustawić dla kolejki, to:

-   **Aktywne**: kolejka jest aktywna.
-   **Wyłączone**: kolejka jest wstrzymana.
-   **SendDisabled**: kolejka jest częściowo zawieszona, a odbieranie jest dozwolone.
-   **ReceiveDisabled**: kolejka jest częściowo zawieszona z dozwolonym wysyłaniem.

W przypadku subskrypcji i tematów można ustawić tylko **aktywne** i **wyłączone** .

Wyliczenie [EntityStatus](/dotnet/api/microsoft.servicebus.messaging.entitystatus) definiuje również zestaw stanów przejściowych, które mogą być ustawiane przez system. W poniższym przykładzie pokazano polecenie programu PowerShell do wyłączania kolejki. Polecenie ponownej aktywacji jest równoznaczne z ustawieniem `Status` **aktywny**.

```powershell
$q = Get-AzServiceBusQueue -ResourceGroup mygrp -NamespaceName myns -QueueName myqueue

$q.Status = "Disabled"

Set-AzServiceBusQueue -ResourceGroup mygrp -NamespaceName myns -QueueName myqueue -QueueObj $q
```

## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się więcej na temat Service Bus Messaging, zobacz następujące tematy:

* [Kolejki, tematy i subskrypcje usługi Service Bus](service-bus-queues-topics-subscriptions.md)
* [Wprowadzenie do kolejek usługi Service Bus](service-bus-dotnet-get-started-with-queues.md)
* [Jak używać tematów i subskrypcji usługi Service Bus](service-bus-dotnet-how-to-use-topics-subscriptions.md)

[1]: ./media/entity-suspend/queue-disable.png

