---
title: Kolokacja tabeli — przeskalowanie (Citus) — Azure Database for PostgreSQL
description: Jak przechowywać powiązane informacje razem z szybszymi zapytaniami
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 05/06/2019
ms.openlocfilehash: 7e4073ec45f4c21f33d20924a9948e72f961c7f8
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "74967341"
---
# <a name="table-colocation-in-azure-database-for-postgresql--hyperscale-citus"></a>Współlokalizacja tabeli w Azure Database for PostgreSQL — funkcja do skalowania (Citus)

Wspólna lokalizacja oznacza przechowywanie powiązanych informacji w tych samych węzłach. Zapytania mogą szybko przechodzić, gdy wszystkie niezbędne dane są dostępne bez żadnego ruchu sieciowego. Współlokalizowanie powiązanych danych w różnych węzłach pozwala wydajnie uruchamiać zapytania w każdym węźle.

## <a name="data-colocation-for-hash-distributed-tables"></a>Współlokalizacja danych dla tabel dystrybuowanych przez funkcję tworzenia skrótów

W Azure Database for PostgreSQL — Citus, wiersz jest przechowywany w fragmentu, jeśli skrót wartości w kolumnie dystrybucja mieści się w zakresie wartości skrótu fragmentu. Fragmentów z tym samym zakresem wyznaczania wartości skrótu są zawsze umieszczane w tym samym węźle. Wiersze z równymi wartościami kolumny dystrybucji są zawsze w tym samym węźle między tabelami.

![Fragmentów](media/concepts-hyperscale-colocation/colocation-shards.png)

## <a name="a-practical-example-of-colocation"></a>Praktyczny przykład wspólnej lokalizacji

Weź pod uwagę następujące tabele, które mogą być częścią wielodostępnego SaaS Web Analytics:

```sql
CREATE TABLE event (
  tenant_id int,
  event_id bigint,
  page_id int,
  payload jsonb,
  primary key (tenant_id, event_id)
);

CREATE TABLE page (
  tenant_id int,
  page_id int,
  path text,
  primary key (tenant_id, page_id)
);
```

Teraz chcemy odpowiedzieć na zapytania, które mogą być wystawiane przez pulpit nawigacyjny dostępny dla klienta. Przykładowe zapytanie to "Zwróć liczbę wizyt w ubiegłym tygodniu dla wszystkich stron zaczynających się od"/blog "w dzierżawie szóstej."

Jeśli nasze dane były w opcji wdrożenia na jednym serwerze, możemy łatwo wyrazić nasze zapytanie przy użyciu bogatego zestawu operacji relacyjnych oferowanych przez SQL:

```sql
SELECT page_id, count(event_id)
FROM
  page
LEFT JOIN  (
  SELECT * FROM event
  WHERE (payload->>'time')::timestamptz >= now() - interval '1 week'
) recent
USING (tenant_id, page_id)
WHERE tenant_id = 6 AND path LIKE '/blog%'
GROUP BY page_id;
```

Tak długo, jak [zestaw roboczy](https://en.wikipedia.org/wiki/Working_set) dla tej kwerendy mieści się w pamięci, jest to odpowiednie rozwiązanie w tabeli na jednym serwerze. Rozważmy możliwości skalowania modelu danych za pomocą opcji wdrożenia Citus.

### <a name="distribute-tables-by-id"></a>Dystrybuuj tabele według identyfikatora

Zapytania na jednym serwerze zaczynają spowalniać działanie, ponieważ liczba dzierżawców i danych przechowywanych dla każdej dzierżawy rośnie. Zestaw roboczy przestanie być dopasowany do pamięci, a procesor CPU stał się wąskim gardłem.

W takim przypadku możemy fragmentu dane między wieloma węzłami przy użyciu funkcji Citus. Pierwszy i najważniejszy wybór, które należy podjąć, gdy zdecydujemy, że fragmentu jest kolumną dystrybucji. Zacznijmy od algorytmie wyboru użycia `event_id` dla tabeli zdarzeń i `page_id` dla `page` tabeli:

```sql
-- naively use event_id and page_id as distribution columns

SELECT create_distributed_table('event', 'event_id');
SELECT create_distributed_table('page', 'page_id');
```

Gdy dane są rozproszone dla różnych procesów roboczych, nie można wykonać sprzężenia, tak jak w przypadku jednego węzła PostgreSQL. Zamiast tego musimy wydać dwie zapytania:

```sql
-- (Q1) get the relevant page_ids
SELECT page_id FROM page WHERE path LIKE '/blog%' AND tenant_id = 6;

-- (Q2) get the counts
SELECT page_id, count(*) AS count
FROM event
WHERE page_id IN (/*…page IDs from first query…*/)
  AND tenant_id = 6
  AND (payload->>'time')::date >= now() - interval '1 week'
GROUP BY page_id ORDER BY count DESC LIMIT 10;
```

Następnie wyniki dwóch kroków muszą być połączone przez aplikację.

Uruchomienie zapytań musi zajrzeć do danych w fragmentów rozproszeniu między węzłami.

![Niewydajne zapytania](media/concepts-hyperscale-colocation/colocation-inefficient-queries.png)

W takim przypadku Dystrybucja danych tworzy znaczące wady:

-   Narzutowanie zapytania o poszczególne fragmentu i uruchamianie wielu zapytań.
-   Narzuty z Q1 zwracają wiele wierszy do klienta.
-   Q2 jest duży.
-   Konieczność zapisu zapytań w wielu krokach wymaga wprowadzenia zmian w aplikacji.

Dane są rozproszone, więc zapytania mogą być równoległe. Jest to przydatne tylko wtedy, gdy ilość pracy wykonywanej przez zapytanie jest znacznie większa niż obciążenie związane z wykonywaniem zapytań dotyczących wielu fragmentów.

### <a name="distribute-tables-by-tenant"></a>Dystrybuowanie tabel według dzierżawy

W obszarze skalowanie (Citus) wiersze o tej samej wartości kolumny dystrybucji są gwarantowane w tym samym węźle. Od początku możemy utworzyć nasze tabele `tenant_id` jako kolumnę dystrybucji.

```sql
-- co-locate tables by using a common distribution column
SELECT create_distributed_table('event', 'tenant_id');
SELECT create_distributed_table('page', 'tenant_id', colocate_with => 'event');
```

Teraz funkcja wieloskalowania (Citus) może odpowiedzieć na oryginalne zapytanie pojedynczego serwera bez modyfikacji (Q1):

```sql
SELECT page_id, count(event_id)
FROM
  page
LEFT JOIN  (
  SELECT * FROM event
  WHERE (payload->>'time')::timestamptz >= now() - interval '1 week'
) recent
USING (tenant_id, page_id)
WHERE tenant_id = 6 AND path LIKE '/blog%'
GROUP BY page_id;
```

Ze względu na filtr i sprzężenie w tenant_id, funkcja Citus (fragmentów) wie, że całe zapytanie może być odpowiedzią przy użyciu zestawu współdzielonego elementu, który zawiera dane dla danej dzierżawy. Pojedynczy węzeł PostgreSQL może odpowiedzieć na zapytanie w jednym kroku.

![Lepsze zapytanie](media/concepts-hyperscale-colocation/colocation-better-query.png)

W niektórych przypadkach zapytania i schematy tabeli muszą zostać zmienione w celu uwzględnienia identyfikatora dzierżawy w unikatowych ograniczeniach i warunkach sprzężenia. Ta zmiana jest zwykle prosta.

## <a name="next-steps"></a>Następne kroki

- Zobacz, jak dane dzierżawy znajdują się w [samouczku dotyczącym wielu dzierżawców](tutorial-design-database-hyperscale-multi-tenant.md).
