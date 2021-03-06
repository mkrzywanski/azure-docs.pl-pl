---
title: 'Application Insights: Języki, platformy i integracje | Microsoft Docs'
description: Języki, platformy i integracje dostępne dla Application Insights
ms.topic: conceptual
ms.date: 07/18/2019
ms.reviewer: olegan
ms.openlocfilehash: 35dc6c5146edd13309a42702d1bc247333ff0fd7
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/28/2020
ms.locfileid: "87322452"
---
# <a name="supported-languages"></a>Obsługiwane języki

* [C#|VB (.NET)](./asp-net.md)
* [Java](./java-get-started.md)
* [JavaScript](./javascript.md)
* [Node.JS](./nodejs.md)
* [Python](./opencensus-python.md)

## <a name="supported-platforms-and-frameworks"></a>Obsługiwane platformy i struktury

### <a name="instrumentation-for-already-deployed-applications-codeless-agent-based"></a>Instrumentacja dla już wdrożonych aplikacji (bez kodu, oparta na agentach)
* [Maszyny wirtualne platformy Azure i zestawy skalowania maszyn wirtualnych platformy Azure](./azure-vm-vmss-apps.md)
* [Azure App Service](./azure-web-apps.md)
* [ASP.NET — dla aplikacji, które już działają](./monitor-performance-live-website-now.md)
* [Cloud Services platformy Azure](./cloudservices.md), w tym role sieci Web i procesu roboczego
* [Azure Functions](../../azure-functions/functions-monitoring.md)
### <a name="instrumentation-through-code-sdks"></a>Instrumentacja poprzez kod (SDK)
* [ASP.NET](./asp-net.md)
* [ASP.NET Core](./asp-net-core.md)
* [Android](../learn/mobile-center-quickstart.md) (App Center)
* [iOS](../learn/mobile-center-quickstart.md) (App Center)
* [Java EE](./java-get-started.md)
* [Node.JS](https://www.npmjs.com/package/applicationinsights)
* [Python](./opencensus-python.md)
* [Aplikacja uniwersalna systemu Windows](../learn/mobile-center-quickstart.md) (App Center)
* [Aplikacje klasyczne, usługi i role procesów roboczych systemu Windows](./windows-desktop.md)

## <a name="logging-frameworks"></a>Struktury rejestrowania
* [ILogger](./ilogger.md)
* [Log4Net, NLog lub System.Diagnostics.Trace](./asp-net-trace-logs.md)
* [Java, Log4J lub Logback](./java-trace-logs.md)
* [Dodatek LogStash](https://github.com/Azure/azure-diagnostics-tools/tree/master/Logstash/logstash-output-applicationinsights)
* [Azure Monitor](/archive/blogs/msoms/application-insights-connector-in-oms)

## <a name="export-and-data-analysis"></a>Eksportowanie i analiza danych
* [Power BI](https://powerbi.microsoft.com/blog/explore-your-application-insights-data-with-power-bi/)
* [Stream Analytics](./export-power-bi.md)

## <a name="unsupported-sdks"></a>Nieobsługiwane zestawy SDK
Mamy świadomość, że istnieją różne zestawy SDK obsługiwane przez społeczność. Jednak Azure Monitor zapewnia pomoc techniczną tylko w przypadku korzystania z obsługiwanych zestawów SDK wymienionych na tej stronie. Nieustannie oceniamy możliwości rozszerzania pomocy technicznej dla innych języków. Aby uzyskać najnowsze informacje o zestawie SDK, postępuj zgodnie z naszą stroną [anonsów usługi GitHub](https://github.com/microsoft/ApplicationInsights-Announcements/issues) . 

