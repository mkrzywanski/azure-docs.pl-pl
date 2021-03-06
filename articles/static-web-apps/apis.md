---
title: Obsługa interfejsu API w usłudze Azure static Web Apps z Azure Functions
description: Dowiedz się, jakie funkcje interfejsu API Azure static Web Apps obsługuje
services: static-web-apps
author: craigshoemaker
ms.service: static-web-apps
ms.topic: conceptual
ms.date: 05/08/2020
ms.author: cshoe
ms.openlocfilehash: f5f40a615bc5faab6265f42d0728403e2735aa0f
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "84791626"
---
# <a name="api-support-in-azure-static-web-apps-preview-with-azure-functions"></a>Obsługa interfejsu API w usłudze Azure static Web Apps Preview przy użyciu Azure Functions

Statyczne Web Apps platformy Azure udostępnia bezserwerowe punkty końcowe interfejsu API za pośrednictwem [Azure Functions](../azure-functions/functions-overview.md). Wykorzystując Azure Functions, interfejsy API dynamicznie skalowane na żądanie i obejmują następujące funkcje:

- **Zintegrowane zabezpieczenia** dzięki bezpośredniemu dostępowi do [uwierzytelniania użytkowników i danych autoryzacji opartych na rolach](user-information.md) .
- **Bezproblemowy Routing** , który sprawia, że trasa _interfejsu API_ jest bezpiecznie dostępna dla aplikacji sieci Web bez konieczności stosowania niestandardowych reguł CORS.
- **Azure Functions** wersja 3 zgodna z Node.js 12.
- **Wyzwalacze protokołu HTTP** i powiązania wyjściowe.

## <a name="configuration"></a>Konfigurowanie

Punkty końcowe interfejsu API są dostępne dla aplikacji sieci Web za pomocą trasy _interfejsu API_ . Po naprawieniu tej trasy masz kontrolę nad folderem, w którym znajduje się skojarzona aplikacja Azure Functions. Tę lokalizację można zmienić, [edytując plik YAML przepływu pracy](github-actions-workflow.md#build-and-deploy) znajdujący się w folderze _. GitHub/_ Workflows repozytorium.

## <a name="constraints"></a>Ograniczenia

Usługa Azure static Web Apps udostępnia interfejs API za pomocą Azure Functions. Możliwości Azure Functions są skoncentrowane na określonym zestawie funkcji, które umożliwiają tworzenie interfejsu API dla aplikacji sieci Web i Zezwalanie aplikacji sieci Web na bezpieczne łączenie się z interfejsem API. Te funkcje są dostępne z pewnymi ograniczeniami, w tym:

- Prefiks trasy interfejsu API musi być _interfejsem API_.
- Aplikacja funkcji API musi znajdować się w języku JavaScript.
- Reguły tras dla funkcji API obsługują tylko [przekierowywanie](routes.md#redirects) i [Zabezpieczanie tras z rolami](routes.md#securing-routes-with-roles).
- Wyzwalacze i powiązania są ograniczone do [protokołu HTTP](../azure-functions/functions-bindings-http-webhook.md).
  - Wszystkie inne [Azure Functions wyzwalacze i powiązania](../azure-functions/functions-triggers-bindings.md#supported-bindings) z wyjątkiem powiązań wyjściowych są ograniczone.
- Dzienniki są dostępne tylko wtedy, gdy dodasz [Application Insights](../azure-functions/functions-monitoring.md) do aplikacji funkcji.

## <a name="next-steps"></a>Następne kroki

> [!div class="nextstepaction"]
> [Dodawanie interfejsu API](add-api.md)
