---
title: Wzorce kolumn w przepływie danych mapowania Azure Data Factory
description: Tworzenie uogólnionych wzorców transformacji danych przy użyciu wzorców kolumn w Azure Data Factory mapowania przepływów danych
author: kromerm
ms.author: makromer
ms.reviewer: daperlov
ms.service: data-factory
ms.topic: conceptual
ms.date: 10/21/2019
ms.openlocfilehash: aacec8830948e08f66d71da88897670f7ef43788
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "81606134"
---
# <a name="using-column-patterns-in-mapping-data-flow"></a>Używanie wzorców kolumn w mapowaniu przepływu danych

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Niektóre przekształcenia przepływu danych mapowania umożliwiają odwoływanie się do kolumn szablonu na podstawie wzorców zamiast zakodowanych nazw kolumn. Takie dopasowanie jest znane jako *wzorce kolumn*. Możesz definiować wzorce, aby dopasować kolumny na podstawie nazwy, typu danych, strumienia lub pozycji, zamiast wymagać dokładnej nazwy pól. Istnieją dwa scenariusze, w których są przydatne wzorce kolumn:

* Jeśli przychodzące pola źródłowe często zmieniają się, na przykład w przypadku zmiany kolumn w plikach tekstowych lub bazach danych NoSQL. Ten scenariusz jest nazywany [dryfem schematu](concepts-data-flow-schema-drift.md).
* Jeśli chcesz wykonać wspólną operację na dużej grupie kolumn. Na przykład, chcemy rzutować każdą kolumnę, która ma wartość "Total" w nazwie kolumny na podwójną.

Wzorce kolumn są obecnie dostępne w transformacje kolumn pochodnych, agregacji, wyboru i ujścia.

## <a name="column-patterns-in-derived-column-and-aggregate"></a>Wzorce kolumn w kolumnie pochodnej i agregowanie

Aby dodać wzorzec kolumny w kolumnie pochodnej lub karcie agregowania transformacji agregowanej, kliknij ikonę znaku plus z prawej strony istniejącej kolumny. Wybierz pozycję **Dodaj wzorzec kolumny**. 

![wzorce kolumn](media/data-flow/columnpattern.png "Wzorce kolumn")

Użyj [konstruktora wyrażeń](concepts-data-flow-expression-builder.md) , aby wprowadzić warunek dopasowania. Utwórz wyrażenie logiczne, które pasuje do kolumn na podstawie `name` `type` kolumny,, `stream` i `position` . Wzorzec będzie miał wpływ na wszystkie kolumny, przenoszące lub zdefiniowane, gdzie warunek zwraca wartość true.

Dwa pola wyrażenia poniżej warunku Match określają nowe nazwy i wartości kolumn, których dotyczy problem. Służy `$$` do odwoływania się do istniejącej wartości dopasowanego pola. Pole wyrażenia Left definiuje nazwę i prawo pole wyrażenia definiuje wartość.

![wzorce kolumn](media/data-flow/columnpattern2.png "Wzorce kolumn")

Powyższy wzorzec kolumny jest zgodny z każdą kolumną typu Double i tworzy jedną kolumnę agregującą dla dopasowania. Nazwa nowej kolumny jest dopasowaną nazwą kolumny połączonej z "_total". Wartość nowej kolumny to zaokrąglona, zagregowana suma istniejącej wartości podwójnej precyzji.

Aby sprawdzić, czy odpowiedni warunek jest poprawny, można sprawdzić poprawność schematu wyjściowego zdefiniowanych kolumn na karcie **Inspekcja** lub uzyskać migawkę danych na karcie **Podgląd danych** . 

![wzorce kolumn](media/data-flow/columnpattern3.png "Wzorce kolumn")

## <a name="rule-based-mapping-in-select-and-sink"></a>Mapowanie oparte na regułach w SELECT i ujścia

Podczas mapowania kolumn w źródle i wybierania transformacji można dodać stałe mapowania lub mapowania oparte na regułach. Dopasowanie na podstawie `name` kolumn, `type` , `stream` i `position` . Można mieć dowolną kombinację stałych i mapowań opartych na regułach. Domyślnie wszystkie projekcje zawierające więcej niż 50 kolumn będą domyślnie mapowane na podstawie reguły, które pasują do każdej kolumny i wyprowadzają wydaną nazwę. 

Aby dodać mapowanie oparte na regułach, kliknij przycisk **Dodaj mapowanie** i wybierz **Mapowanie oparte na regułach**.

![Mapowanie oparte na regułach](media/data-flow/rule2.png "Mapowanie oparte na regułach")

Każde mapowanie oparte na regułach wymaga dwóch danych wejściowych: warunek, dla którego należy dopasować i co należy nazwać każdej mapowanej kolumny. Obie wartości są zwracane za pośrednictwem [konstruktora wyrażeń](concepts-data-flow-expression-builder.md). W polu wyrażenia po lewej stronie wprowadź warunek dopasowania wartości logicznej. W polu wyrażenie po prawej stronie Określ, do czego zostanie zamapowana pasująca kolumna.

![Mapowanie oparte na regułach](media/data-flow/rule-based-mapping.png "Mapowanie oparte na regułach")

Użyj `$$` składni, aby odwołać się do nazwy wejściowej dopasowanej kolumny. Używając powyższego obrazu na przykład, Załóżmy, że użytkownik chce dopasować wszystkie kolumny ciągów, których nazwy są krótsze niż sześć znaków. Jeśli jedna kolumna przychodząca `test` ma nazwę, wyrażenie zmieni `$$ + '_short'` nazwę kolumny `test_short` . Jeśli jest to jedyne mapowanie, które istnieje, wszystkie kolumny, które nie spełniają warunku, zostaną usunięte z danych, które zostały wydane.

Wzorce pasują do kolumn przeznaczonych i zdefiniowanych. Aby zobaczyć, które zdefiniowane kolumny są mapowane przez regułę, kliknij ikonę okularów obok reguły. Sprawdź dane wyjściowe przy użyciu podglądu danych.

### <a name="regex-mapping"></a>Mapowanie wyrażenia regularnego

Jeśli klikniesz ikonę z dolnym cudzysłowem, możesz określić warunek mapowania wyrażenia regularnego. Warunek mapowania wyrażenia regularnego pasuje do wszystkich nazw kolumn, które pasują do określonego warunku wyrażenia regularnego. Można go używać w połączeniu z standardowymi mapowaniami opartymi na regułach.

![Mapowanie oparte na regułach](media/data-flow/regex-matching.png "Mapowanie oparte na regułach")

Powyższy przykład pasuje do wzorca wyrażenia regularnego `(r)` lub nazwy kolumny zawierającej małe litery r. Podobnie jak w przypadku standardowego mapowania opartego na regułach, wszystkie dopasowane kolumny są modyfikowane przez warunek po prawej stronie `$$` składni.

### <a name="rule-based-hierarchies"></a>Hierarchie oparte na regułach

Jeśli zdefiniowana projekcja ma hierarchię, można użyć mapowania opartego na regułach, aby zmapować podkolumny hierarchii. Określ warunek dopasowania i kolumnę złożoną, której kolumny mają być mapowane. Każda dopasowana Podkolumna zostanie wykorzystana przy użyciu reguły "name as" określonej po prawej stronie.

![Mapowanie oparte na regułach](media/data-flow/rule-based-hierarchy.png "Mapowanie oparte na regułach")

Powyższy przykład dopasowuje wszystkie podkolumny złożonej kolumny `a` . `a`zawiera dwie podkolumny `b` i `c` . Schemat danych wyjściowych będzie zawierać dwie kolumny `b` , a `c` jako warunek "name as" `$$` .

## <a name="pattern-matching-expression-values"></a>Wartości wyrażeń dopasowania do wzorca.

* `$$`tłumaczy na nazwę lub wartość każdego dopasowania w czasie wykonywania
* `name`reprezentuje nazwę każdej kolumny przychodzącej
* `type`reprezentuje typ danych każdej kolumny przychodzącej
* `stream`reprezentuje nazwę skojarzoną z każdym strumieniem lub transformację w przepływie
* `position`jest pozycją porządkową kolumn w przepływie danych

## <a name="next-steps"></a>Następne kroki
* Dowiedz się więcej o [języku wyrażeń](data-flow-expression-functions.md) przepływu danych mapowania dla transformacji danych
* Używanie wzorców kolumn w [transformację ujścia](data-flow-sink.md) i [Wybieranie transformacji](data-flow-select.md) z mapowaniem opartym na regułach
