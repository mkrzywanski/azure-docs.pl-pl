---
title: IS_ARRAY w języku zapytań Azure Cosmos DB
description: Dowiedz się więcej na temat funkcji systemu SQL IS_ARRAY w Azure Cosmos DB.
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 09/13/2019
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: f5867850db6eb3d6552bc129cca3708ef7747072
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "78303889"
---
# <a name="is_array-azure-cosmos-db"></a>IS_ARRAY (Azure Cosmos DB)
 Zwraca wartość logiczną wskazującą, czy typ określonego wyrażenia jest tablicą.  
  
## <a name="syntax"></a>Składnia
  
```sql
IS_ARRAY(<expr>)  
```  
  
## <a name="arguments"></a>Argumenty
  
*wyrażenie*  
   To dowolne wyrażenie.  
  
## <a name="return-types"></a>Typy zwracane
  
  Zwraca wyrażenie logiczne.  
  
## <a name="examples"></a>Przykłady
  
  Poniższy przykład sprawdza obiekty typu Boolean, Number, String, null, Object, Array i undefined przy użyciu `IS_ARRAY` funkcji.  
  
```sql
SELECT   
 IS_ARRAY(true) AS isArray1,   
 IS_ARRAY(1) AS isArray2,  
 IS_ARRAY("value") AS isArray3,  
 IS_ARRAY(null) AS isArray4,  
 IS_ARRAY({prop: "value"}) AS isArray5,   
 IS_ARRAY([1, 2, 3]) AS isArray6,  
 IS_ARRAY({prop: "value"}.prop2) AS isArray7  
```  
  
 Tutaj znajduje się zestaw wyników.  
  
```json
[{"isArray1":false,"isArray2":false,"isArray3":false,"isArray4":false,"isArray5":false,"isArray6":true,"isArray7":false}]
```  

## <a name="remarks"></a>Uwagi

Ta funkcja systemowa będzie korzystać z [indeksu zakresu](index-policy.md#includeexclude-strategy).

## <a name="next-steps"></a>Następne kroki

- [Funkcje sprawdzania typu Azure Cosmos DB](sql-query-type-checking-functions.md)
- [Azure Cosmos DB funkcje systemowe](sql-query-system-functions.md)
- [Wprowadzenie do usługi Azure Cosmos DB](introduction.md)
