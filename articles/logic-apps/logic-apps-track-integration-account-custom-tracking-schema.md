---
title: Niestandardowe schematy śledzenia dla komunikatów B2B
description: Tworzenie niestandardowych schematów śledzenia do monitorowania komunikatów B2B w Azure Logic Apps
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, logicappspm
ms.topic: article
ms.date: 01/01/2020
ms.openlocfilehash: c82f9cbfaf2e23ddaa5e4b05f4aac4795d3e16a9
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "76903053"
---
# <a name="create-custom-tracking-schemas-that-monitor-end-to-end-workflows-in-azure-logic-a"></a>Tworzenie niestandardowych schematów śledzenia, które monitorują kompleksowe przepływy pracy w usłudze Azure Logic A

Azure Logic Apps ma wbudowane śledzenie, które można włączyć dla części przepływu pracy. Można jednak skonfigurować niestandardowe śledzenie, które rejestruje zdarzenia od początku do końca przepływów pracy, na przykład przepływy pracy, które obejmują aplikację logiki, BizTalk Server, SQL Server lub dowolną inną warstwę. Ten artykuł zawiera kod niestandardowy, którego można użyć w warstwach poza aplikacją logiki.

## <a name="custom-tracking-schema"></a>Niestandardowy schemat śledzenia

```json
{
   "sourceType": "",
   "source": {
      "workflow": {
         "systemId": ""
      },
      "runInstance": {
         "runId": ""
      },
      "operation": {
         "operationName": "",
         "repeatItemScopeName": "",
         "repeatItemIndex": ,
         "trackingId": "",
         "correlationId": "",
         "clientRequestId": ""
      }
   },
   "events": [
      {
         "eventLevel": "",
         "eventTime": "",
         "recordType": "",
         "record": {}
      }
   ]
}
```

| Właściwość | Wymagany | Typ | Opis |
|----------|----------|------|-------------|
| sourceType | Tak | String | Typ źródła przebiegu z tymi dozwolonymi wartościami: `Microsoft.Logic/workflows` ,`custom` |
| source | Tak | Ciąg lub JToken | Jeśli typ źródła to `Microsoft.Logic/workflows` , informacje źródłowe muszą być zgodne z tym schematem. Jeśli typem źródła jest `custom` , schemat jest JToken. |
| systemId | Tak | String | Identyfikator systemu aplikacji logiki |
| runId | Tak | String | Identyfikator przebiegu aplikacji logiki |
| operationName | Tak | String | Nazwa operacji, na przykład akcja lub wyzwalacz |
| repeatItemScopeName | Tak | String | Powtórz nazwę elementu, jeśli akcja znajduje się wewnątrz `foreach` `until` pętli lub |
| repeatItemIndex | Tak | Integer | Wskazuje, że akcja znajduje się wewnątrz `foreach` `until` pętli lub i jest numerem indeksu powtarzanego elementu. |
| trackingId | Nie | String | Identyfikator śledzenia do skorelowania komunikatów |
| correlationId | Nie | String | Identyfikator korelacji do skorelowania komunikatów |
| Identyfikatorem żądania klienta | Nie | String | Klient może wypełnić tę właściwość w celu skorelowania komunikatów |
| eventLevel | Tak | String | Poziom zdarzenia |
| eventTime | Tak | DateTime | Godzina zdarzenia w formacie UTC: *RRRR-MM-DDTgg: mm: SS. 00000Z* |
| recordType | Tak | String | Typ rekordu śledzenia z tą dozwoloną wartością:`custom` |
| rejestrowanie | Tak | JToken | Typ rekordu niestandardowego z tylko formatem JToken |
|||||

## <a name="b2b-protocol-tracking-schemas"></a>Schematy śledzenia protokołu B2B

Aby uzyskać informacje o schematach śledzenia protokołu B2B, zobacz:

* [Schematy śledzenia AS2](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)
* [Schematy śledzenia X12](logic-apps-track-integration-account-x12-tracking-schema.md)

## <a name="next-steps"></a>Następne kroki

* Dowiedz się więcej o [monitorowaniu komunikatów B2B przy użyciu dzienników Azure monitor](../logic-apps/monitor-b2b-messages-log-analytics.md)