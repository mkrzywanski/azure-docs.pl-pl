---
title: Używanie etykiet zapytań w Synapse SQL
description: Zawarta w tym artykule są istotne porady dotyczące korzystania z etykiet zapytań w programie SQL Synapse.
services: synapse-analytics
author: filippopovic
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/15/2020
ms.author: fipopovi
ms.reviewer: jrasnick
ms.custom: ''
ms.openlocfilehash: 47b476cbc6997ca5ec63968bdc269e2273662100
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "81430034"
---
# <a name="use-query-labels-in-synapse-sql"></a>Używanie etykiet zapytań w Synapse SQL
Zawarta w tym artykule są istotne porady dotyczące korzystania z etykiet zapytań w programie SQL Synapse.

> [!NOTE]
> SQL na żądanie (wersja zapoznawcza) nie obsługuje zapytań etykietowania.

## <a name="what-are-query-labels"></a>Co to są etykiety zapytań
Pula SQL obsługuje koncepcję o nazwie etykiety zapytań. Przed przejściem do dowolnej głębokości Przyjrzyjmy się przykładowi:

```sql
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query Label')
;
```

Ostatni wiersz określa ciąg "My Query Label" w zapytaniu. Ten tag jest szczególnie przydatny, ponieważ etykieta jest w trakcie wykonywania zapytania przez widoków DMV. Wykonywanie zapytań dotyczących etykiet zapewnia mechanizm lokalizowania zapytań dotyczących problemów i ułatwia identyfikowanie postępu przez ELT uruchomienie.

Dobre konwencje nazewnictwa są najbardziej przydatne. Na przykład uruchomienie etykiety z projektem, procedurą, INSTRUKCJą lub KOMENTARZEm jednoznacznie identyfikuje zapytanie między całym kodem w kontroli źródła.

Następujące zapytanie używa dynamicznego widoku zarządzania do wyszukiwania według etykiety:

```sql
SELECT  *
FROM    sys.dm_pdw_exec_requests r
WHERE   r.[label] = 'My Query Label'
;
```

> [!NOTE]
> Podczas wykonywania zapytań należy umieścić nawiasy kwadratowe lub podwójne cudzysłowy wokół etykiety wyrazu. Etykieta jest słowem zastrzeżonym i powoduje błąd, gdy nie zostanie on rozdzielony. 
> 
> 

## <a name="next-steps"></a>Następne kroki
Aby uzyskać więcej porad programistycznych, zobacz [Omówienie projektowania](develop-overview.md).


