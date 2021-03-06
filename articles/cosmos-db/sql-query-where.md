---
title: Klauzula WHERE w Azure Cosmos DB
description: Informacje o klauzuli WHERE języka SQL dla Azure Cosmos DB
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/06/2020
ms.author: tisande
ms.openlocfilehash: ceffb203ccc2cca1ff6e1c53644cde955c2e0acb
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/20/2020
ms.locfileid: "86523506"
---
# <a name="where-clause-in-azure-cosmos-db"></a>Klauzula WHERE w Azure Cosmos DB

Opcjonalna klauzula WHERE ( `WHERE <filter_condition>` ) określa warunek (y), które muszą spełniać źródłowe elementy JSON dla zapytania, aby uwzględnić je w wynikach. Element JSON musi oszacować określone warunki, aby można je było `true` uwzględnić w wyniku. Warstwa indeksu używa klauzuli WHERE do określenia najmniejszego podzestawu elementów źródłowych, które mogą być częścią wyniku.
  
## <a name="syntax"></a>Składnia
  
```sql  
WHERE <filter_condition>  
<filter_condition> ::= <scalar_expression>  
  
```  
  
## <a name="arguments"></a>Argumenty

- `<filter_condition>`  
  
   Określa warunek, który ma zostać spełniony dla dokumentów, które mają zostać zwrócone.  
  
- `<scalar_expression>`  
  
   Wyrażenie reprezentujące wartość, która ma zostać obliczona. Aby uzyskać szczegółowe informacje, zobacz [wyrażenia skalarne](sql-query-scalar-expressions.md) .  
  
## <a name="remarks"></a>Uwagi
  
  Aby dokument był zwracany wyrażenie określone jako warunek filtru musi mieć wartość true. Tylko wartość logiczna `true` będzie spełniać warunek, żadna inna wartość: undefined, null, false, Number, array lub Object nie spełnia warunku.

  Jeśli klucz partycji zostanie uwzględniony w `WHERE` klauzuli w ramach filtra równości, zapytanie zostanie automatycznie przefiltrowane na odpowiednie partycje.

## <a name="examples"></a>Przykłady

Następujące zapytanie żąda elementów, które zawierają `id` Właściwość, której wartość to `AndersenFamily` . Wyklucza wszelkie elementy, które nie mają `id` właściwości lub których wartość nie jest zgodna `AndersenFamily` .

```sql
    SELECT f.address
    FROM Families f
    WHERE f.id = "AndersenFamily"
```

Wyniki są następujące:

```json
    [{
      "address": {
        "state": "WA",
        "county": "King",
        "city": "Seattle"
      }
    }]
```

### <a name="scalar-expressions-in-the-where-clause"></a>Wyrażenia skalarne w klauzuli WHERE

W poprzednim przykładzie pokazano proste zapytanie dotyczące równości. Interfejs API SQL obsługuje również różne [wyrażenia skalarne](sql-query-scalar-expressions.md). Najczęściej używane są wyrażenia binarne i jednoargumentowe. Odwołania do właściwości z obiektu JSON źródła są również prawidłowymi wyrażeniami.

Można użyć następujących obsługiwanych operatorów binarnych:  

|**Typ operatora**  | **Wartości** |
|---------|---------|
|Arytmetyczny | +,-,*,/,% |
|Bitowy    | \|, &, ^, <<, >>, >>> (wypełnienie zerami, przesunięcie w prawo) |
|Logiczny    | AND, OR, NOT      |
|Porównanie | =, !=, &lt;, &gt;, &lt;=, &gt;=, <> |
|String (ciąg)     |  \|\| (łączenie) |

Następujące zapytania używają operatorów binarnych:

```sql
    SELECT *
    FROM Families.children[0] c
    WHERE c.grade % 2 = 1     -- matching grades == 5, 1

    SELECT *
    FROM Families.children[0] c
    WHERE c.grade ^ 4 = 1    -- matching grades == 5

    SELECT *
    FROM Families.children[0] c
    WHERE c.grade >= 5    -- matching grades == 5
```

Można również użyć operatorów jednoargumentowych +,-, ~, a nie w zapytaniach, jak pokazano w następujących przykładach:

```sql
    SELECT *
    FROM Families.children[0] c
    WHERE NOT(c.grade = 5)  -- matching grades == 1

    SELECT *
    FROM Families.children[0] c
    WHERE (-c.grade = -5)  -- matching grades == 5
```

Można również użyć odwołań do właściwości w zapytaniach. Na przykład `SELECT * FROM Families f WHERE f.isRegistered` zwraca element JSON zawierający Właściwość `isRegistered` o wartości równej `true` . Każda inna wartość, taka jak,,,,, `false` `null` `Undefined` `<number>` `<string>` `<object>` lub `<array>` , wyklucza element z wyniku. Ponadto można użyć `IS_DEFINED` funkcji sprawdzania typu w celu wykonywania zapytań na podstawie obecności lub braku danej właściwości JSON. Na przykład `SELECT * FROM Families f WHERE NOT IS_DEFINED(f.isRegistered)` zwraca dowolny element JSON, który nie ma wartości dla `isRegistered` .

## <a name="next-steps"></a>Następne kroki

- [Rozpoczęcie pracy](sql-query-getting-started.md)
- [Słowo kluczowe IN](sql-query-keywords.md#in)
- [Klauzula FROM](sql-query-from.md)
