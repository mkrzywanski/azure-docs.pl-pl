---
title: Najlepsze rozwiązania dotyczące programowania Synapse SQL
description: Zalecenia i najlepsze rozwiązania, które należy znać podczas opracowywania programu SQL Synapse.
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/15/2020
ms.author: xiaoyul
ms.reviewer: igorstan
ms.openlocfilehash: ff4781109b2572d5555ec0a03c65359ef5a89d8d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "85482517"
---
# <a name="development-best-practices-for-synapse-sql"></a>Najlepsze rozwiązania dotyczące programowania Synapse SQL
W tym artykule opisano wskazówki i najlepsze rozwiązania w zakresie tworzenia rozwiązań magazynu danych. 

## <a name="sql-pool-development-best-practices"></a>Najlepsze rozwiązania dotyczące programowania puli SQL

### <a name="reduce-cost-with-pause-and-scale"></a>Obniżenie kosztów dzięki wstrzymaniu i skalowaniu

Aby uzyskać więcej informacji dotyczących obniżania kosztów za pomocą wstrzymywania i skalowania, zobacz [Zarządzanie obliczeniami](../sql-data-warehouse/sql-data-warehouse-manage-compute-overview.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json).

### <a name="maintain-statistics"></a>Prowadzenie statystyk

Pamiętaj o aktualizowaniu statystyk codziennie lub po każdym załadowaniu.  Zawsze istnieje możliwość wypracowania kompromisu pomiędzy wydajnością a kosztami tworzenia i aktualizowania statystyk. Jeśli okaże się, że obsługa wszystkich statystyk trwa zbyt długo, należy bardziej wybiórczo o tym, które kolumny mają statystykę lub które kolumny wymagają częstego aktualizowania.  

Na przykład możesz chcieć zaktualizować kolumny dat, w których można codziennie dodawać nowe wartości. 

> [!NOTE]
> Dzięki temu można uzyskać statystykę na temat kolumn związanych z sprzężeniami, kolumnami używanymi w klauzuli WHERE oraz kolumnami, które znajdują się w grupie według.

Zobacz też [Zarządzanie statystykami tabel](develop-tables-statistics.md), [CREATE STATISTICS](/sql/t-sql/statements/create-statistics-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)i [Update Statistics](/sql/t-sql/statements/update-statistics-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest).

### <a name="hash-distribute-large-tables"></a>Dystrybucja dużych tabel z użyciem skrótów

Domyślnym sposobem dystrybucji tabel jest działanie okrężne.  Dzięki temu użytkownicy mogą łatwo rozpocząć tworzenie tabel bez konieczności podejmowania decyzji o sposobie dystrybuowania ich tabel.  Tabele działające w trybie okrężnym mogą być wystarczająco wydajne w przypadku niektórych obciążeń. Jednak w większości przypadków wybranie kolumny dystrybucji będzie dużo lepsze.  

Najbardziej typowym przykładem sytuacji, w której zastosowanie dla tabeli dystrybucji według kolumny przyniesie znacznie lepsze wyniki niż zastosowanie tabeli z działaniem okrężnym, jest połączenie dwóch dużych tabel faktów.  

Na przykład jeśli masz tabelę Orders, która jest dystrybuowana przez order_id, a tabela transakcji, która jest również dystrybuowana przez order_id, po dołączeniu tabeli Orders do tabeli transakcji w order_id, to zapytanie zostanie przekazane do kwerendy przekazującej. 

Oznacza to, że eliminujemy operacje przenoszenia danych.  Mniej kroków to szybsze kwerendy.  Mniejsza konieczność przenoszenia danych wpływa także na skrócenie czasu działania kwerend.

> [!TIP]
> Podczas ładowania dystrybuowanej tabeli należy upewnić się, że dane przychodzące nie są sortowane według klucza dystrybucji, ponieważ mogłoby to spowolnić ładowanie.  

Zobacz poniższe linki, aby uzyskać dodatkowe informacje o tym, jak wybór kolumny dystrybucji może poprawić wydajność, a także określić sposób definiowania rozproszonej tabeli w klauzuli WITH instrukcji CREATE TABLES.

Zobacz również [Omówienie tabeli](develop-tables-overview.md), [dystrybucji tabel](../sql-data-warehouse/sql-data-warehouse-tables-distribute.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json), [wybierania dystrybucji tabel](https://blogs.msdn.microsoft.com/sqlcat/20../../choosing-hash-distributed-table-vs-round-robin-distributed-table-in-azure-sql-dw-service/), [CREATE TABLE](/sql/t-sql/statements/create-table-azure-sql-data-warehouse?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)i [CREATE TABLE jako wybrane](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest).

### <a name="do-not-over-partition"></a>Unikanie nadmiernego partycjonowania
Gdy Partycjonowanie danych może być skuteczne do obsługi danych za pomocą przełączania partycji lub optymalizowania skanowania za pomocą eliminacji partycji, zbyt wiele partycji może spowalniać zapytania.  Często wysoce ziarnista strategia partycjonowania, która może być dobrze włączona SQL Server może nie współpracować z pulą SQL.  

> [!NOTE]
> Często wysoce ziarnista strategia partycjonowania, która może być dobrze włączona SQL Server może nie współpracować z pulą SQL.  

Zbyt duża liczba partycji danych może także zmniejszyć skuteczność indeksów klastrowanego magazynu kolumn, jeśli każda partycja ma mniej niż milion wierszy. Pula SQL dzieli Twoje dane na 60 baz danych. 

Dlatego, jeśli utworzysz tabelę z 100 partycji, wynik będzie wynosić 6000 partycji.  Każde obciążenie jest inne, więc warto eksperymentować z podziałem na partycje — w ten sposób można przekonać się, jakie rozwiązanie sprawdzi się najlepiej w przypadku danego obciążenia.  

Jedną z opcji, które należy wziąć pod uwagę, jest użycie stopnia szczegółowości, który jest mniejszy niż to, co może być w SQL Server.  Można na przykład rozważyć wykorzystanie partycji cotygodniowych lub comiesięcznych zamiast partycji codziennych.

Zobacz też [partycjonowanie tabel](../sql-data-warehouse/sql-data-warehouse-tables-partition.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json).

### <a name="minimize-transaction-sizes"></a>Minimalizowanie rozmiarów transakcji

Instrukcje INSERT, UPDATE i DELETE działają w obrębie transakcji i muszą zostać wycofane, jeśli zakończą się niepowodzeniem.  Aby zminimalizować ryzyko długiego czasu wycofywania, warto minimalizować rozmiary transakcji, gdy tylko jest to możliwe.  Można to zrobić poprzez podział instrukcji INSERT, UPDATE i DELETE na części.  

Na przykład jeśli masz WSTAWIENIE, którego oczekujesz na 1 godzinę, możesz przerwać Wstawianie do czterech części, a tym samym skrócić każdy przebieg do 15 minut.

> [!TIP]
> Stosuj szczególne przypadki minimalnego rejestrowania, takie jak CTAS, TRUNCATE, DROP TABLE lub INSERT, do opróżniania tabel, aby zmniejszyć ryzyko wycofywania.  

Innym sposobem na eliminację procesu wycofywania zmian jest korzystanie z operacji z właściwościami Tylko metadane, takich jak przełączanie partycji pod kątem zarządzania danymi.  

Na przykład zamiast wykonywać instrukcję DELETE w celu usunięcia z tabeli wszystkich wierszy, dla których wartość data_zamowienia to październik 2001, można rozdzielić dane na miesiące, aby następnie przełączyć wybraną partycję na pustą partycję z innej tabeli (zobacz przykłady instrukcji ALTER TABLE).  

W przypadku tabel niepartycjonowanych zamiast korzystać z instrukcji DELETE, należy rozważyć użycie instrukcji CTAS do zapisu danych, które mają zostać zachowane w tabeli.  Jeśli wykonanie instrukcji CTAS trwa tyle samo czasu, to na jej korzyść nadal przemawia znacznie większe bezpieczeństwo. Jej uruchomienie wiąże się z minimalnym rejestrowaniem, przez co operacja może w razie potrzeby zostać szybko anulowana.

Zobacz też [Omówienie transakcji](develop-transactions.md), [Optymalizowanie transakcji](../sql-data-warehouse/sql-data-warehouse-develop-best-practices-transactions.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json), [partycjonowanie tabel](../sql-data-warehouse/sql-data-warehouse-tables-partition.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json), [TRUNCATE TABLE](/sql/t-sql/statements/truncate-table-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), [ALTER TABLE](/sql/t-sql/statements/alter-table-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest)i [CREATE TABLE as Select (CTAs)](../sql-data-warehouse/sql-data-warehouse-develop-ctas.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json).

### <a name="use-the-smallest-possible-column-size"></a>Użycie możliwie najmniejszego rozmiaru kolumny

Użycie podczas definiowania kwerendy DDL najmniejszego typu danych, który umożliwi obsługę danych, zwiększy wydajność kwerendy.  Jest to szczególnie ważne w przypadku kolumn CHAR i VARCHAR.  

Jeśli najdłuższa wartość w kolumnie ma 25 znaków, należy zdefiniować typ kolumny jako VARCHAR(25).  Należy unikać domyślnego definiowania wszystkich kolumn znaków jako kolumn długich wartości.  Ponadto należy unikać stosowania kolumn NVARCHAR, jeśli zastosowanie typu VARCHAR spełni wymagania danego zastosowania.

Zobacz także [Omówienie tabel](develop-tables-overview.md), [typy danych tabeli](develop-tables-data-types.md)i [CREATE TABLE](/sql/t-sql/statements/create-table-azure-sql-data-warehouse?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest).

### <a name="optimize-clustered-columnstore-tables"></a>Optymalizowanie tabel klastrowanego magazynu kolumn

Klastrowane indeksy magazynu kolumn to jeden z najbardziej wydajnych sposobów przechowywania danych w puli SQL.  Domyślnie tabele w puli SQL są tworzone jako klastrowane magazynu kolumn.  

Dla uzyskania najlepszej wydajności kwerend w odniesieniu do tabel magazynu kolumn ważne jest zapewnienie dobrej jakości segmentów.  Jeśli wiersze są zapisywane w tabelach magazynu kolumn przy dużym wykorzystaniu pamięci, może to spowodować obniżenie jakości segmentów w magazynie kolumn.  

Jakość segmentu określa się na podstawie liczby wierszy w skompresowanej grupie wierszy.  Zobacz [przyczyny słabej jakości indeksu magazynu kolumn](../sql-data-warehouse/sql-data-warehouse-tables-index.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json#causes-of-poor-columnstore-index-quality) i [indeksów tabeli](../sql-data-warehouse/sql-data-warehouse-tables-index.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) , aby uzyskać instrukcje krok po kroku dotyczące wykrywania i poprawiania jakości segmentu dla klastrowanych tabel magazynu kolumn.  

Ponieważ duże jakości segmenty magazynu kolumn są ważne, dobrym pomysłem jest użycie identyfikatorów użytkowników, które znajdują się w średniej lub dużej klasie zasobów do ładowania danych. W przypadku korzystania z niższych [jednostek magazynu danych](resource-consumption-models.md) do użytkownika ładującego należy przypisać większą klasę zasobów.

Ponieważ tabele magazynu kolumn zwykle nie przepychają danych do skompresowanego segmentu magazynu kolumn, dopóki nie będzie więcej niż 1 000 000 wierszy na tabelę, a każda tabela puli SQL zostanie podzielona na 60 tabel, tabele magazynu kolumn nie będą korzystać z zapytania, chyba że tabela ma więcej niż 60 000 000 wierszy.  

> [!TIP]
> W przypadku tabel zawierających mniej niż 60 000 000 wierszy posiadanie indeksu magazynu kolumn może nie być najlepszym rozwiązaniem.  

Ponadto w przypadku partycjonowania danych warto wziąć pod uwagę, że każda partycja będzie musiała mieć milion wierszy, aby można było odnieść korzyść z zastosowania klastrowanego indeksu magazynu kolumn.  Jeśli tabela ma 100 partycji, będzie musiała mieć co najmniej 6 000 000 000 wierszy do skorzystania z magazynu kolumn klastrowanych (60 distributions *100 partycje* 1 000 000 wiersze).  

Jeśli tabela nie zawiera 6 000 000 000 wierszy, zmniejsz liczbę partycji lub Rozważ użycie w zamian tabeli sterty.  Może być również warto eksperymentować, aby sprawdzić, czy lepsza wydajność może być uzyskana przy użyciu tabeli sterty z indeksami pomocniczymi, a nie z tabeli magazynu kolumn.

Podczas wykonywania zapytania odnoszącego się do tabeli magazynu kolumn kwerendy będą uruchamiane szybciej, jeśli wybrane zostaną tylko niezbędne kolumny.  

Zobacz również [indeksy tabel](../sql-data-warehouse/sql-data-warehouse-tables-index.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json), [Przewodnik po indeksach magazynu kolumn](/sql/relational-databases/indexes/columnstore-indexes-overview?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest), ponowne [Kompilowanie indeksów magazynu kolumn](../sql-data-warehouse/sql-data-warehouse-tables-index.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json#rebuilding-indexes-to-improve-segment-quality).

## <a name="sql-on-demand-development-best-practices"></a>Najlepsze rozwiązania dotyczące programowania na żądanie w języku SQL

### <a name="general-considerations"></a>Zagadnienia ogólne

Usługa SQL na żądanie umożliwia wykonywanie zapytań dotyczących plików na kontach usługi Azure Storage. Nie ma ona lokalnego magazynu ani możliwości pozyskiwania, co oznacza, że wszystkie pliki docelowe zapytań są zewnętrzne na żądanie. W związku z tym wszystkie informacje dotyczące odczytywania plików z magazynu mogą mieć wpływ na wydajność zapytań.

### <a name="colocate-azure-storage-account-and-sql-on-demand"></a>Lokalizowanie konta usługi Azure Storage i SQL na żądanie

Aby zminimalizować opóźnienie, umieść konto usługi Azure Storage i punkt końcowy na żądanie SQL. Konta magazynu i punkty końcowe inicjowane podczas tworzenia obszaru roboczego znajdują się w tym samym regionie.

Aby uzyskać optymalną wydajność, Jeśli uzyskujesz dostęp do innych kont magazynu za pomocą programu SQL na żądanie, upewnij się, że znajdują się one w tym samym regionie. W przeciwnym razie nastąpi zwiększone opóźnienie transferu sieciowego danych z regionu zdalnego do regionu punktu końcowego.

### <a name="azure-storage-throttling"></a>Ograniczanie usługi Azure Storage

Wiele aplikacji i usług może uzyskać dostęp do konta magazynu. Gdy połączone operacje we/wy są generowane przez aplikacje, usługi i obciążenie na żądanie SQL, przekraczają limity konta magazynu. W przypadku ograniczenia przepustowości magazynu występuje znaczny negatywny wpływ na wydajność zapytań.

Po wykryciu ograniczenia przepustowości SQL na żądanie ma wbudowaną obsługę tego scenariusza. Program SQL na żądanie będzie przesyłał żądania do magazynu w wolniejszym tempie, dopóki ograniczanie zostanie rozwiązane. 

Jednak w celu zapewnienia optymalnego wykonywania zapytań zaleca się, aby nie naciskać konta magazynu z innymi obciążeniami podczas wykonywania zapytania.

### <a name="prepare-files-for-querying"></a>Przygotuj pliki do zapytania

Jeśli to możliwe, można przygotować pliki w celu uzyskania lepszej wydajności:

- Konwertuj CSV na Parquet — Parquet jest formatem kolumnowym. Ponieważ jest ona skompresowana, ma mniejsze rozmiary plików niż pliki CSV z tymi samymi danymi, a na żądanie musi być krótszy czas i liczba żądań magazynu, aby je odczytać.
- Jeśli zapytanie odwołuje się do pojedynczego dużego pliku, można je podzielić na wiele mniejszych plików.
- Spróbuj zachować rozmiar pliku CSV poniżej 10 GB.
- Preferowane jest posiadanie plików o równym rozmiarze dla pojedynczej ścieżki OPENROWSET lub lokalizacji tabeli zewnętrznej.
- Podziel dane przez przechowywanie partycji w różnych folderach lub nazwach plików — zaznacz [opcję Użyj funkcji filename i FilePath, aby określić docelowe partycje](#use-fileinfo-and-filepath-functions-to-target-specific-partitions).

### <a name="use-fileinfo-and-filepath-functions-to-target-specific-partitions"></a>Używanie funkcji FileInfo i FilePath do określonych partycji

Dane często są zorganizowane w partycjach. Można wydać instrukcję SQL na żądanie, aby wykonywać zapytania dotyczące określonych folderów i plików. Spowoduje to zmniejszenie liczby plików i ilości danych, które zapytanie musi odczytać i przetworzyć. 

W związku z tym osiągniesz lepszą wydajność. Aby uzyskać więcej informacji, zapoznaj się z funkcjami [filename](query-data-storage.md#filename-function) i [FilePath](query-data-storage.md#filepath-function) i przykładami dotyczącymi [zapytań określonych plików](query-specific-files.md).

Jeśli dane w magazynie nie są partycjonowane, rozważ ich partycjonowanie, aby można było używać tych funkcji do optymalizowania zapytań przeznaczonych dla tych plików.

Podczas [wykonywania zapytania dotyczącego Apache Spark partycjonowania dla tabel zewnętrznych platformy Azure Synapse](develop-storage-files-spark-tables.md) z bazy danych SQL na żądanie, zapytanie będzie automatycznie kierować tylko pliki, które są zbędne.

### <a name="use-cetas-to-enhance-query-performance-and-joins"></a>Korzystanie z CETAS w celu zwiększenia wydajności zapytań i sprzężeń

[CETAS](develop-tables-cetas.md) to jedna z najważniejszych funkcji dostępnych w programie SQL na żądanie. CETAS to równoległa operacja, która tworzy metadane tabeli zewnętrznej i eksportuje wynik zapytania SELECT do zestawu plików na koncie magazynu.

Można użyć CETAS do przechowywania często używanych części zapytań, takich jak sprzężone tabele odwołań, do nowego zestawu plików. Później można przyłączyć się do tej pojedynczej tabeli zewnętrznej zamiast powtarzających się wspólnych sprzężeń w wielu zapytaniach. 

Ponieważ CETAS generuje pliki Parquet, statystyki zostaną automatycznie utworzone, gdy pierwsze zapytanie jest przeznaczone dla tej tabeli zewnętrznej i zostanie zwiększona wydajność.

### <a name="next-steps"></a>Następne kroki

Jeśli potrzebujesz informacji, które nie zostały podane w tym artykule, użyj "Wyszukaj dokumenty" po lewej stronie tej strony, aby wyszukać wszystkie dokumenty w puli SQL.  [Microsoft Q&stronie pytania dla puli SQL](https://docs.microsoft.com/answers/topics/azure-synapse-analytics.html) jest miejscem, w którym można zadawać pytania do innych użytkowników i grupy produktów w puli SQL.  

Firma Microsoft aktywnie monitoruje to forum, aby mieć pewność, że użytkownicy uzyskują odpowiedzi od innych użytkowników lub pracowników firmy Microsoft.  Jeśli wolisz zadać pytania na Stack Overflow, masz również [Forum usługi Azure SQL pool Stack Overflow](https://stackoverflow.com/questions/tagged/azure-sqldw).
 
