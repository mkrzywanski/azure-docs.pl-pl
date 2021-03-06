---
title: Wskazówki dotyczące projektowania tabeli usługi Azure Storage | Microsoft Docs
description: Zaprojektuj usługę tabel platformy Azure w celu wydajnego obsługi operacji odczytu.
services: storage
author: SnehaGunda
ms.service: storage
ms.topic: article
ms.date: 04/23/2018
ms.author: sngun
ms.subservice: tables
ms.openlocfilehash: d056d29469ad9a60fceeee307aca3c0e1319283c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "61269851"
---
# <a name="guidelines-for-table-design"></a>Wytyczne dotyczące projektu tabel

Projektowanie tabel do użycia w usłudze Azure Storage Table Service różni się od zagadnień związanych z projektowaniem relacyjnej bazy danych. W tym artykule opisano wskazówki dotyczące projektowania rozwiązania Table service w celu zapewnienia wydajnego odczytu i zwiększenia wydajności.

## <a name="design-your-table-service-solution-to-be-read-efficient"></a>Zaprojektuj rozwiązanie do Table service pod kątem sprawności odczytu

* ***Projekt do wykonywania zapytań w aplikacjach z dużym obciążeniem.*** Podczas projektowania tabel należy zastanowić się nad zapytaniami (zwłaszcza z uwzględnieniem opóźnień), które zostaną wykonane przed zawieszeniem się, jak należy zaktualizować jednostki. Zwykle jest to wydajne i wydajne rozwiązanie.  
* ***W zapytaniach Określ zarówno PartitionKey, jak i RowKey.*** *Zapytania punktowe* , takie jak te, to najbardziej wydajne zapytania dotyczące usługi Table Service.  
* ***Rozważ przechowywanie zduplikowanych kopii jednostek.*** Magazyn tabel jest tani, dlatego należy rozważyć przechowywanie tej samej jednostki wielokrotnie (z różnymi kluczami) w celu umożliwienia wydajniejszych zapytań.  
* ***Rozważ wyprowadzenie denormalizacji danych.*** Magazyn tabel jest tani, dlatego należy rozważyć denormalizację danych. Na przykład, przechowuj jednostki podsumowujące, tak aby zapytania dotyczące danych agregowanych musieli uzyskać dostęp do pojedynczej jednostki.  
* ***Użyj wartości klucza złożonego.*** Jedyne klucze są **PartitionKey** i **RowKey**. Na przykład użyj wartości klucza złożonego, aby włączyć alternatywne ścieżki dostępu do jednostek.  
* ***Użyj projekcji zapytania.*** Możesz zmniejszyć ilość danych przesyłanych przez sieć za pomocą zapytań, które wybierają tylko potrzebne pola.  

## <a name="design-your-table-service-solution-to-be-write-efficient"></a>Zaprojektuj rozwiązanie do Table service, aby zapewnić wysoką wydajność zapisu  

* ***Nie twórz partycji aktywnych.*** Wybierz klucze, które umożliwiają rozproszenie żądań w wielu partycjach w dowolnym momencie.  
* ***Unikaj wzrostu ruchu.*** Wygładź ruch w rozsądnym czasie i unikaj wzrostu ruchu.
* ***Niekoniecznie utwórz oddzielną tabelę dla każdego typu jednostki.*** Gdy wymagane są transakcje niepodzielne między typami jednostek, można przechowywać te wiele typów jednostek w tej samej partycji w tej samej tabeli.
* ***Należy wziąć pod uwagę maksymalną przepływność, która ma zostać osiągnięta.*** Należy pamiętać o obiektach docelowych skalowalności Table service i upewnić się, że projekt nie spowoduje przekroczenia ich.  

Po przeczytaniu tego przewodnika zobaczysz przykłady, które stosują wszystkie te zasady do ćwiczeń. 

## <a name="next-steps"></a>Następne kroki

- [Wzorce projektowe tabel](table-storage-design-patterns.md)
- [Projektowanie pod kątem wykonywania zapytań](table-storage-design-for-query.md)
- [Szyfruj dane tabeli](table-storage-design-encrypt-data.md)
- [Projektowanie pod kątem modyfikacji danych](table-storage-design-for-modification.md)
