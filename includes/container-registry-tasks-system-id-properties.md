---
title: dołączanie pliku
description: dołączanie pliku
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: include
ms.date: 07/06/2020
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 1b7c8487eb42204f2741679c9ef6eb2717c272cd
ms.sourcegitcommit: bcb962e74ee5302d0b9242b1ee006f769a94cfb8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/07/2020
ms.locfileid: "86057362"
---
W danych wyjściowych polecenia `identity` sekcja pokazuje tożsamość typu `SystemAssigned` jest ustawiana w zadaniu. `principalId`Jest identyfikatorem podmiotu zabezpieczeń dla tożsamości zadania:

```console
[...]
  "identity": {
    "principalId": "xxxxxxxx-2703-42f9-97d0-xxxxxxxxxxxx",
    "tenantId": "xxxxxxxx-86f1-41af-91ab-xxxxxxxxxxxx",
    "type": "SystemAssigned",
    "userAssignedIdentities": null
  },
  "location": "eastus",
[...]
``` 
Użyj polecenia [AZ ACR Task show][az-acr-task-show] , aby zapisać principalId w zmiennej, aby użyć ich w późniejszych poleceniach. Zastąp nazwę zadania i rejestr w następującym poleceniu:

```azurecli
principalID=$(az acr task show \
  --name <task_name> --registry <registry_name> \
  --query identity.principalId --output tsv)
```

<!-- LINKS - Internal -->
[az-acr-task-show]: /cli/azure/acr/task#az-acr-task-show
