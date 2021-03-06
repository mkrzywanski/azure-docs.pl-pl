---
title: Przepływy danych mapowania
description: Omówienie mapowania przepływów danych w Azure Data Factory
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 06/09/2020
ms.openlocfilehash: 850879675d4554329f24c86f2ac28660b303084c
ms.sourcegitcommit: 5f7b75e32222fe20ac68a053d141a0adbd16b347
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/31/2020
ms.locfileid: "87475570"
---
# <a name="what-are-mapping-data-flows"></a>Czym są przepływy danych mapowania?

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Mapowanie przepływów danych to wizualnie zaprojektowane przekształcenia danych w Azure Data Factory. Przepływy danych umożliwiają inżynierom danych Tworzenie logiki transformacji danych graficznych bez pisania kodu. Wyniki przepływów danych są wykonywane jako działania w ramach potoków Azure Data Factory, które korzystają ze skalowania Apache Spark klastrów. Działania związane z przepływem danych mogą być realizowane za pośrednictwem istniejących Data Factory planowania, kontroli, przepływu i monitorowania.

Mapowanie przepływów danych zapewnia całkowicie wizualizację, bez konieczności kodowania. Przepływy danych są uruchamiane w klastrze wykonywania w celu przetwarzania danych skalowanych w poziomie. Azure Data Factory obsługuje wszystkie tłumaczenia kodu, optymalizację ścieżki i wykonywanie zadań przepływu danych.

![Architektura](media/data-flow/adf-data-flows.png "Architektura")

## <a name="getting-started"></a>Wprowadzenie

Aby utworzyć przepływ danych, wybierz znak plus w obszarze **zasoby fabryki**, a następnie wybierz pozycję **przepływ danych**. 

![Nowy przepływ danych](media/data-flow/newdataflow2.png "Nowy przepływ danych")

Ta akcja spowoduje przejście do kanwy przepływu danych, w której można utworzyć logikę transformacji. Wybierz pozycję **Dodaj źródło** , aby rozpocząć konfigurowanie transformacji źródłowej. Aby uzyskać więcej informacji, zobacz [Źródło transformacji](data-flow-source.md).

## <a name="data-flow-canvas"></a>Kanwa przepływu danych

Kanwa przepływu danych jest podzielony na trzy części: górny pasek, wykres i panel konfiguracja. 

![Kanwa](media/data-flow/canvas1.png "Kanwa")

### <a name="graph"></a>Graph

Wykres przedstawia strumień transformacji. Pokazuje on dane źródłowe w miarę ich przepływu w jednym lub większej liczbie zlewów. Aby dodać nowe źródło, wybierz pozycję **Dodaj źródło**. Aby dodać nową transformację, wybierz znak plus w prawym dolnym rogu istniejącej transformacji.

![Kanwa](media/data-flow/canvas2.png "Kanwa")

### <a name="azure-integration-runtime-data-flow-properties"></a>Właściwości przepływu danych środowiska Azure Integration Runtime

![Przycisk Debuguj](media/data-flow/debugbutton.png "Przycisk Debuguj")

Po rozpoczęciu pracy z przepływami danych w podajniku APD należy włączyć przełącznik "Debuguj" dla przepływów danych w górnej części interfejsu użytkownika przeglądarki. To powoduje, że klaster Spark ma być używany na potrzeby debugowania interaktywnego, podglądów danych i wykonań debugowania potoku. Możesz ustawić rozmiar używanego klastra, wybierając niestandardową [Azure Integration Runtime](concepts-integration-runtime.md). Sesja debugowania pozostaje aktywna przez nawet 60 minut od ostatniej wersji zapoznawczej danych lub ostatniego wykonania potoku debugowania.

Gdy operacjonalizować potoki z działaniami przepływu danych, funkcja ADF używa Azure Integration Runtime skojarzonego z [działaniem](control-flow-execute-data-flow-activity.md) we właściwości "Run on".

Domyślny Azure Integration Runtime to niewielki klaster jednordzeniowego pojedynczego procesu roboczego, który umożliwia podgląd danych i szybkie wykonywanie potoków debugowania przy minimalnych kosztach. Ustaw większą konfigurację Azure IR, jeśli wykonujesz operacje na dużych zestawach danych.

Można wydać polecenie ADF, aby zachować pulę zasobów klastra (maszyn wirtualnych) przez ustawienie czasu wygaśnięcia we właściwościach przepływu danych Azure IR. Ta akcja powoduje przyspieszenie wykonywania zadań w kolejnych działaniach.

#### <a name="azure-integration-runtime-and-data-flow-strategies"></a>Infrastruktura Azure Integration Runtime i strategie przepływu danych

##### <a name="execute-data-flows-in-parallel"></a>Równoległe wykonywanie przepływów danych

Jeśli przepływy danych są wykonywane równolegle, moduł ADF uruchamia oddzielne klastry usługi Spark dla każdego wykonywania działań na podstawie ustawień w Azure Integration Runtime dołączone do poszczególnych działań. Aby zaprojektować wykonywanie równoległe w potokach ADF, należy dodać działania przepływu danych bez ograniczeń pierwszeństwa w interfejsie użytkownika.

Z tych trzech opcji, ta opcja może być wykonywana w najkrótszym czasie. Jednak każdy przepływ danych równoległych wykonuje się w tym samym czasie w oddzielnych klastrach, więc porządkowanie zdarzeń jest niedeterministyczne.

Jeśli wykonujesz działania przepływu danych równolegle wewnątrz potoków, zaleca się, aby nie używać czasu wygaśnięcia. Ta akcja polega na tym, że równoległe wykonywanie przepływu danych jednocześnie przy użyciu tego samego Azure Integration Runtime powoduje wystąpienie wielu wystąpień puli dla fabryki danych.

##### <a name="overload-single-data-flow"></a>Przeciążanie pojedynczego przepływu danych

Jeśli umieścisz całą logikę wewnątrz pojedynczego przepływu danych, moduł ADF wykonuje ten sam kontekst wykonywania zadania w jednym wystąpieniu klastra Spark.

Ta opcja może być trudniejsza do obserwowania i rozwiązywania problemów, ponieważ reguły biznesowe i logika biznesowa mogą być Jumbled razem. Ta opcja nie zapewnia również dużej ilości użyteczności.

##### <a name="execute-data-flows-sequentially"></a>Wykonywanie przepływów danych sekwencyjnie

Jeśli wykonujesz działania przepływu danych w sekwencji w potoku i ustawisz wartość czasu wygaśnięcia dla konfiguracji Azure IR, usługa ADF użyje ponownie zasobów obliczeniowych (maszyn wirtualnych), co spowoduje szybsze wykonywanie kolejnych operacji. W każdym wykonaniu będzie nadal wyświetlany nowy kontekst platformy Spark.

Z tych trzech opcji ta akcja może zająć najdłuższy czas. Ale zapewnia czyste rozdzielenie operacji logicznych w każdym kroku przepływu danych.

### <a name="configuration-panel"></a>Panel konfiguracji

Panel konfiguracja przedstawia ustawienia specyficzne dla aktualnie wybranego przekształcenia. Jeśli żadna transformacja nie jest zaznaczona, przepływ danych zostanie wyświetlony. W ogólnej konfiguracji przepływu danych można edytować nazwę i opis na karcie **Ogólne** lub dodać parametry za pomocą karty **Parametry** . Aby uzyskać więcej informacji, zobacz [Mapowanie parametrów przepływu danych](parameters-data-flow.md).

Każda transformacja zawiera co najmniej cztery karty konfiguracyjne.

#### <a name="transformation-settings"></a>Ustawienia transformacji

Pierwsza karta w okienku Konfiguracja każdej transformacji zawiera ustawienia specyficzne dla tej transformacji. Aby uzyskać więcej informacji, zobacz stronę dokumentacji przekształcenia.

![Karta Ustawienia źródła](media/data-flow/source1.png "Karta Ustawienia źródła")

#### <a name="optimize"></a>Optymalizacja

Karta **Optymalizacja** zawiera ustawienia umożliwiające skonfigurowanie schematów partycjonowania. Aby dowiedzieć się więcej na temat optymalizowania przepływów danych, zobacz [Przewodnik dotyczący wydajności przepływu danych](concepts-data-flow-performance.md).

![Optymalizacja](media/data-flow/optimize.png "Optymalizacja")

#### <a name="inspect"></a>Skontrol

Karta **Inspekcja** umożliwia wyświetlenie metadanych strumienia danych, który jest przekształcany. Możesz zobaczyć liczby kolumn, kolumny zmienione, kolumny dodane, typy danych, kolejność kolumn i odwołania do kolumn. **Inspekcja** to widok metadanych w trybie tylko do odczytu. Nie musisz mieć włączonego trybu debugowania, aby wyświetlić metadane w okienku **Inspekcja** .

![Skontrol](media/data-flow/inspect1.png "Skontrol")

Gdy zmienisz kształt danych za pomocą transformacji, przepływ zmian metadanych zostanie wyświetlony w okienku **Inspekcja** . Jeśli w transformacji źródłowej nie ma zdefiniowanego schematu, metadane nie będą widoczne w okienku **Inspekcja** . Brak metadanych jest powszechny w scenariuszach dryfowania schematu.

#### <a name="data-preview"></a>Podgląd danych

Jeśli tryb debugowania jest włączony, karta **Podgląd danych** zapewnia interaktywną migawkę danych w każdej transformacji. Aby uzyskać więcej informacji, zobacz [Podgląd danych w trybie debugowania](concepts-data-flow-debug-mode.md#data-preview).

### <a name="top-bar"></a>Górny pasek

Górny pasek zawiera akcje, które mają wpływ na cały przepływ danych, takie jak zapisywanie i walidacja. Można również przełączać się między trybami wykresu i konfiguracji za pomocą przycisków **Pokaż wykres** i **Ukryj wykres** .

![Ukryj wykres](media/data-flow/hideg.png "Ukryj wykres")

W przypadku ukrycia grafu można przeglądać węzły transformacji później za pomocą przycisków **Wstecz** i **dalej** .

![Przyciski poprzednie i następne](media/data-flow/showhide.png "przyciski poprzednie i następne")

## <a name="next-steps"></a>Następne kroki

* Dowiedz się, jak utworzyć [transformację źródłową](data-flow-source.md).
* Dowiedz się, jak tworzyć przepływy danych w [trybie debugowania](concepts-data-flow-debug-mode.md).
