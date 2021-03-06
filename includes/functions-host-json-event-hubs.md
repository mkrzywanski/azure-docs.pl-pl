---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: 2604a1608f21d7239db755027e15b8198fb3f9f2
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "81791669"
---
### <a name="functions-2x-and-higher"></a>Funkcje w wersji 2.x i nowszych

```json
{
    "version": "2.0",
    "extensions": {
        "eventHubs": {
            "batchCheckpointFrequency": 5,
            "eventProcessorOptions": {
                "maxBatchSize": 256,
                "prefetchCount": 512
            }
        }
    }
}  
```

|Właściwość  |Domyślne | Opis |
|---------|---------|---------|
|maxBatchSize|10|Maksymalna liczba zdarzeń odebranych na pętlę odbierania.|
|prefetchCount|300|Domyślna liczba pobrań poprzedzających, używana przez bazowe `EventProcessorHost` .|
|batchCheckpointFrequency|1|Liczba partii zdarzeń do przetworzenia przed utworzeniem punktu kontrolnego kursora centrum EventHub.|

> [!NOTE]
> Aby uzyskać informacje na temat host.jsw Azure Functions 2. x i więcej, zobacz [host.json Reference for Azure Functions](../articles/azure-functions/functions-host-json.md).

### <a name="functions-1x"></a>Functions w wersji 1.x

```json
{
    "eventHub": {
      "maxBatchSize": 64,
      "prefetchCount": 256,
      "batchCheckpointFrequency": 1
    }
}
```

|Właściwość  |Domyślne | Opis |
|---------|---------|---------| 
|maxBatchSize|64|Maksymalna liczba zdarzeń odebranych na pętlę odbierania.|
|prefetchCount|nie dotyczy|Domyślne pobranie wstępne, które będzie używane przez bazowe `EventProcessorHost` .| 
|batchCheckpointFrequency|1|Liczba partii zdarzeń do przetworzenia przed utworzeniem punktu kontrolnego kursora centrum EventHub.| 

> [!NOTE]
> Aby uzyskać informacje na temat host.jsna Azure Functions 1. x, zobacz [host.json Reference for Azure Functions 1. x](../articles/azure-functions/functions-host-json-v1.md).

