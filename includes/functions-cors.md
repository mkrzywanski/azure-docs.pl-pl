---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 05/06/2020
ms.author: glenga
ms.openlocfilehash: a5f829fc0e153c3a427fd1c7cc7d5c613cd624f6
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "83648244"
---
Azure Functions obsługuje udostępnianie zasobów między źródłami (CORS). Mechanizm CORS jest konfigurowany [w portalu](../articles/azure-functions/functions-how-to-use-azure-function-app-settings.md#cors) i za pomocą [interfejsu wiersza polecenia platformy Azure](/cli/azure/functionapp/cors). Lista źródeł dozwolonych CORS jest stosowana na poziomie aplikacji funkcji. Po włączeniu funkcji CORS odpowiedzi zawierają `Access-Control-Allow-Origin` nagłówek. Aby uzyskać więcej informacji, zobacz temat [Współużytkowanie zasobów między źródłami](../articles/azure-functions/functions-how-to-use-azure-function-app-settings.md#cors). 