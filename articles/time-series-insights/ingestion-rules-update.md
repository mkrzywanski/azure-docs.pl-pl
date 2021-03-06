---
title: Nadchodzące zmiany w regułach pozyskiwania i spłaszczania w Azure Time Series Insights Gen2 | Microsoft Docs
description: Zmiany reguły pozyskiwania
ms.service: time-series-insights
services: time-series-insights
author: lyrana
ms.author: lyhughes
manager: dpalled
ms.workload: big-data
ms.topic: conceptual
ms.date: 06/16/2020
ms.custom: lyhughes
ms.openlocfilehash: f667ca5ad82182fcf40d5c1fbb325f2ea99a7e08
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/20/2020
ms.locfileid: "86495112"
---
# <a name="upcoming-changes-to-the-json-flattening-and-escaping-rules-for-new-environments"></a>Nadchodzące zmiany reguł spłaszczania i ucieczki JSON dla nowych środowisk

**Te zmiany zostaną zastosowane do *nowo utworzonych* Azure Time Series Insights środowiska Gen2. Te zmiany nie dotyczą środowisk Gen1.**

Środowisko Azure Time Series Insights Gen2 tworzy dynamicznie kolumny magazynu zgodnie z określonym zestawem konwencji nazewnictwa. W przypadku pozyskania zdarzenia zestaw reguł jest stosowany do nazw ładunku i właściwości JSON. Zmiany sposobu spłaszczania i przechowywania danych JSON zaczną obowiązywać w przypadku nowych środowisk Azure Time Series Insights Gen2 w lipcu 2020. Ta zmiana ma wpływ na następujące sytuacje:

* Jeśli ładunek JSON zawiera obiekty zagnieżdżone
*  Jeśli ładunek JSON zawiera tablice
*  Jeśli używasz jednego z następujących czterech znaków specjalnych w nazwie właściwości JSON: [\. '
*  Jeśli co najmniej jedna z właściwości identyfikatora TS znajduje się w obiekcie zagnieżdżonym.

Jeśli tworzysz nowe środowisko i co najmniej jeden z powyższych przypadków ma zastosowanie do ładunku zdarzenia, dane są spłaszczone i przechowywane inaczej. Poniżej znajduje się podsumowanie zmian:

| Bieżąca reguła | Nowa reguła | Przykładowy kod JSON | Nazwa poprzedniej kolumny | Nazwa nowej kolumny
|---|---| ---| ---|  ---|
| Zagnieżdżony kod JSON jest spłaszczony przy użyciu znaku podkreślenia jako delineator |Zagnieżdżony kod JSON jest spłaszczony przy użyciu okresu jako delineator  | ``{"series" : { "value" : 19.338 }}`` | `series_value_double` |`series.value_double` |
| Znaki specjalne nie są wyprowadzane | Nazwy właściwości JSON, które zawierają znaki specjalne. [\ i "są wyprowadzane przy użyciu [" i "]. W obrębie ["i"] istnieje dodatkowe anulowanie apostrofów i ukośników odwrotnych. Pojedynczy cytat zostanie zapisany jako \' , a ukośnik odwrotny zostanie zapisany jako\\\  | ```"Foo's Law Value": "17.139999389648"``` | `Foo's Law Value_double` | `['Foo\'s Law Value']_double` | 
| Tablice elementów podstawowych są przechowywane jako ciąg | Tablice typów pierwotnych są przechowywane jako typ dynamiczny  | `"values": [154, 149, 147]` | `values_string`  | `values_dynamic` |
Tablice obiektów są zawsze spłaszczane, generując wiele zdarzeń | Jeśli obiekty w tablicy nie mają odpowiednio identyfikatora TS lub sygnatur czasowych, tablica obiektów jest przechowywana w całości jako typ dynamiczny | `"values": [{"foo" : 140}, {"bar" : 149}]` | `values_foo_long | values_bar_long` | `values_dynamic` |

## <a name="recommended-changes-for-new-environments"></a>Zalecane zmiany dla nowych środowisk

#### <a name="if-your-ts-id-andor-timestamp-property-is-nested-within-an-object"></a>Jeśli identyfikator TS i/lub właściwość sygnatury czasowej są zagnieżdżone w obiekcie:

*  Wszystkie nowe wdrożenia będą musiały pasować do nowych reguł pozyskiwania. Na przykład jeśli identyfikator TS jest `telemetry_tagId` wymagany do zaktualizowania szablonów usługi ARM lub zautomatyzowanego wdrażania skryptów w celu skonfigurowania `telemetry.tagId` go jako środowiska TS ID. Ta zmiana jest wymagana dla sygnatur czasowych źródła zdarzeń w zagnieżdżonym kodzie JSON.

 #### <a name="if-your-payload-contains-nested-json-or-special-characters-and-you-automate-authoring-time-series-model-variable-expressions"></a>Jeśli ładunek zawiera zagnieżdżony kod JSON lub znaki specjalne i automatyzuje tworzenie wyrażeń zmiennych [modelu szeregów czasowych](.\time-series-insights-update-tsm.md)

*  Zaktualizuj kod klienta wykonujący [TypesBatchPut](https://docs.microsoft.com/rest/api/time-series-insights/dataaccessgen2/timeseriestypes/executebatch#typesbatchput) w celu dopasowania do nowych reguł pozyskiwania. Na przykład poprzednie [wyrażenie szeregów czasowych](https://docs.microsoft.com/rest/api/time-series-insights/preview#time-series-expression-and-syntax) `"value": {"tsx": "$event.series_value.Double"}` należy zaktualizować do jednej z poniższych opcji:
    * `"value": {"tsx": "$event.series.value.Double"}`
    * `"value": {"tsx": "$event['series']['value'].Double"}`


## <a name="next-steps"></a>Następne kroki

- Przeczytaj [Azure Time Series Insights Gen2 Storage i](./time-series-insights-update-storage-ingress.md)transfery danych przychodzących.

- Dowiedz się więcej na temat wykonywania zapytań dotyczących danych za pomocą [interfejsów API zapytań szeregów czasowych](./concepts-query-overview.md).

- Przeczytaj więcej na temat [nowej składni wyrażeń szeregów czasowych](https://docs.microsoft.com/rest/api/time-series-insights/preview#time-series-expression-and-syntax).

