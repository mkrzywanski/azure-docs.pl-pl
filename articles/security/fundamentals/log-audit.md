---
title: Rejestrowanie i Inspekcja zabezpieczeń platformy Azure | Microsoft Docs
description: Poznaj dzienniki dostępne na platformie Azure i szczegółowe informacje o zabezpieczeniach, które możesz uzyskać.
services: security
documentationcenter: na
author: TerryLanfear
manager: rkarlin
editor: TomSh
ms.assetid: ''
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/31/2019
ms.author: terrylan
ms.openlocfilehash: bd0f42507e22559690e2682a391c53b9c090aa6c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "75750790"
---
# <a name="azure-security-logging-and-auditing"></a>Rejestrowanie i inspekcja zabezpieczeń na platformie Azure

System Azure oferuje szeroką gamę konfigurowalnych opcji inspekcji zabezpieczeń i rejestrowania, które ułatwiają identyfikację luk w zabezpieczeniach i mechanizmach zabezpieczeń. W tym artykule omówiono generowanie, zbieranie i analizowanie dzienników zabezpieczeń z usług hostowanych na platformie Azure.

> [!Note]
> Niektóre zalecenia przedstawione w tym artykule mogą spowodować zwiększenie użycia zasobów danych, sieci lub obliczeń oraz zwiększyć koszty licencji lub subskrypcji.

## <a name="types-of-logs-in-azure"></a>Typy dzienników na platformie Azure

Aplikacje w chmurze są złożone z wieloma ruchomymi częściami. Dane rejestrowania mogą zapewnić wgląd w aplikacje i pomóc Ci:

- Rozwiązywanie problemów z poprzednimi problemami lub blokowanie potencjalnych
- Zwiększenie wydajności lub utrzymania aplikacji
- Automatyzuj akcje, które w przeciwnym razie wymagają interwencji ręcznej

Dzienniki platformy Azure są podzielone na następujące typy:
* **Dzienniki kontroli i zarządzania** zawierają informacje dotyczące Azure Resource Manager operacji tworzenia, aktualizowania i usuwania. Aby uzyskać więcej informacji, zobacz [dzienniki aktywności platformy Azure](../../azure-monitor/platform/platform-logs-overview.md).

* **Dzienniki płaszczyzny danych** zawierają informacje o zdarzeniach wywoływanych w ramach użycia zasobów platformy Azure. Przykładami tego typu dziennika są dzienniki systemu zdarzeń systemu Windows, zabezpieczeń i aplikacji na maszynie wirtualnej (VM) oraz [dzienniki diagnostyczne](../../azure-monitor/platform/platform-logs-overview.md) skonfigurowane za pomocą Azure monitor.

* **Przetwarzane zdarzenia** zawierają informacje o przeanalizowanych zdarzeniach/alertach, które zostały przetworzone w Twoim imieniu. Przykłady tego typu są [Azure Security Center alertów](../../security-center/security-center-managing-and-responding-alerts.md) , w których [Azure Security Center](../../security-center/security-center-intro.md) przetworzył i przeanalizować subskrypcję i oferuje zwięzłe alerty zabezpieczeń.

Poniższa tabela zawiera listę najważniejszych typów dzienników dostępnych na platformie Azure:

| Kategoria dziennika | Typ dziennika | Użycie | Integracja |
| ------------ | -------- | ------ | ----------- |
|[Dzienniki aktywności](../../azure-monitor/platform/platform-logs-overview.md)|Zdarzenia płaszczyzny kontroli dla zasobów Azure Resource Manager|  Zapewnia wgląd w operacje wykonywane względem zasobów w ramach subskrypcji.|    Interfejs API REST, [Azure monitor](../../azure-monitor/platform/platform-logs-overview.md)|
|[Dzienniki zasobów platformy Azure](../../azure-monitor/platform/platform-logs-overview.md)|Częste dane dotyczące operacji Azure Resource Manager zasobów w subskrypcji|   Zapewnia wgląd w operacje wykonywane przez sam zasób.| Azure Monitor|
|[Raporty w usłudze Azure Active Directory](../../active-directory/reports-monitoring/overview-reports.md)|Dzienniki i raporty | Zgłasza działania związane z logowaniem użytkowników i informacje o aktywności systemu dotyczące zarządzania użytkownikami i grupami.|[Interfejs API programu Graph](../../active-directory/develop/active-directory-graph-api-quickstart.md)|
|[Maszyny wirtualne i usługi w chmurze](../../azure-monitor/learn/quick-collect-azurevm.md)|Usługa Dziennik zdarzeń systemu Windows i dziennik systemowy Linux|  Przechwytuje dane systemowe i rejestruje dane na maszynach wirtualnych i przesyła je do wybranego konta magazynu.|   Windows (z systemem Windows Diagnostyka Azure [[funkcji wad](../../monitoring-and-diagnostics/azure-diagnostics.md)] Storage) i Linux w systemie Azure monitor|
|[analityka magazynu platformy Azure](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics)|Rejestrowanie magazynu udostępnia dane metryk dla konta magazynu|Zapewnia wgląd w żądania śledzenia, analizuje trendy użycia i diagnozuje problemy związane z kontem magazynu.|   Interfejs API REST lub [Biblioteka kliencka](https://msdn.microsoft.com/library/azure/mt347887.aspx)|
|[Dzienniki przepływu sieciowej grupy zabezpieczeń (sieciowej grupy zabezpieczeń)](../../network-watcher/network-watcher-nsg-flow-logging-overview.md)|Format JSON, pokazuje przepływy wychodzące i przychodzące dla każdej reguły|Wyświetla informacje dotyczące ruchu przychodzącego i wychodzącego IP za pomocą sieciowej grupy zabezpieczeń.|[Azure Network Watcher](../../network-watcher/network-watcher-monitoring-overview.md)|
|[Application Insights](../../azure-monitor/app/app-insights-overview.md)|Dzienniki, wyjątki i Diagnostyka niestandardowa|  Zapewnia usługę programu Application Performance Monitoring (APM) dla deweloperów sieci Web na wielu platformach.| Interfejs API REST, [Power BI](https://powerbi.microsoft.com/documentation/powerbi-azure-and-power-bi/)|
|[Przetwarzanie alertów dotyczących danych/zabezpieczeń](../../security-center/security-center-intro.md)|  Alerty dotyczące Azure Security Center, alerty dzienników Azure Monitor|    Zawiera informacje o zabezpieczeniach i alertach.|  Interfejsy API REST, JSON|

## <a name="log-integration-with-on-premises-siem-systems"></a>Integracja dzienników z lokalnymi systemami SIEM
[Integracja Security Centerych alertów](../../security-center/security-center-export-data-to-siem.md) omawia sposób synchronizowania alertów Security Center, zdarzeń zabezpieczeń maszyn wirtualnych zbieranych przez dzienniki diagnostyki platformy Azure i dzienników inspekcji platformy Azure z dziennikami Azure monitor lub rozwiązaniem Siem.

## <a name="next-steps"></a>Następne kroki

- [Inspekcja i rejestrowanie](management-monitoring-overview.md): Ochrona danych przez utrzymywanie wglądu i szybkie reagowanie na alerty zabezpieczeń.

- [Rejestrowanie zabezpieczeń i zbieranie dzienników na platformie Azure](https://azure.microsoft.com/resources/videos/security-logging-and-audit-log-collection/): Wymuś te ustawienia, aby upewnić się, że wystąpienia platformy Azure będą zbierać poprawne dzienniki zabezpieczeń i inspekcji.

- [Skonfiguruj ustawienia inspekcji dla zbioru witryn](https://support.office.com/article/Configure-audit-settings-for-a-site-collection-A9920C97-38C0-44F2-8BCB-4CF1E2AE22D2?ui=&rs=&ad=US): Jeśli jesteś administratorem zbioru witryn, Pobierz historię akcji poszczególnych użytkowników i historię akcji podejmowanych w określonym zakresie dat.

- [Przeszukaj dziennik inspekcji w centrum zgodności z pakietem office 365 security &](https://support.office.com/article/Search-the-audit-log-in-the-Office-365-Security-Compliance-Center-0d4d0f35-390b-4518-800e-0c7ec95e946c?ui=&rs=&ad=US): Użyj Centrum zgodności pakietu Office 365 Security &, aby przeszukać ujednolicony dziennik inspekcji i wyświetlić aktywność użytkowników i administratorów w organizacji pakietu Office 365.
