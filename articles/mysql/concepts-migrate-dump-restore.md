---
title: Migrowanie przy użyciu zrzutów i przywracania Azure Database for MySQL
description: W tym artykule opisano dwa typowe sposoby tworzenia kopii zapasowych i przywracania baz danych w Azure Database for MySQL przy użyciu narzędzi takich jak mysqldump, MySQL Workbench i PHPMyAdmin.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 2/27/2020
ms.openlocfilehash: c30faa31f6f733f80d4bfd5184c09d9fdbd6f389
ms.sourcegitcommit: f684589322633f1a0fafb627a03498b148b0d521
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/06/2020
ms.locfileid: "85971185"
---
# <a name="migrate-your-mysql-database-to-azure-database-for-mysql-using-dump-and-restore"></a>Migrowanie bazy danych MySQL do usługi Azure Database for MySQL przy użyciu zrzutu i przywracania
W tym artykule opisano dwa typowe sposoby tworzenia kopii zapasowych i przywracania baz danych w Azure Database for MySQL
- Zrzuć i Przywróć z wiersza polecenia (przy użyciu mysqldump) 
- Zrzuć i Przywróć przy użyciu PHPMyAdmin 

## <a name="before-you-begin"></a>Przed rozpoczęciem
Aby krokowo korzystać z tego przewodnika, musisz mieć:
- [tworzenie serwera usługi Azure Database for MySQL za pomocą witryny Azure Portal](quickstart-create-mysql-server-database-using-azure-portal.md)
- Narzędzie wiersza polecenia [mysqldump](https://dev.mysql.com/doc/refman/5.7/en/mysqldump.html) zainstalowane na komputerze.
- Narzędzia MySQL Workbench [MySQL Workbench Download](https://dev.mysql.com/downloads/workbench/) lub inne narzędzie MySQL innej firmy do wykonywania poleceń zrzutów i przywracania.

Jeśli chcesz przeprowadzić migrację dużych baz danych o rozmiarze bazy danych więcej niż 1 TBs, możesz rozważyć użycie narzędzi społeczności, takich jak redumper/OnLoad, które obsługują eksport równoległy i import. Równoległy zrzut i przywracanie mogą znacznie skrócić czas migracji dużych baz danych. W naszym [blogu techcommunity](https://techcommunity.microsoft.com/t5/azure-database-for-mysql/best-practices-for-migrating-large-databases-to-azure-database/ba-p/1362699) można zapoznać się z najlepszymi rozwiązaniami dotyczącymi migrowania dużych baz danych do usługi Azure Database for MySQL przy użyciu narzędzi do zrzutówer/narzędzia do ładowania.

## <a name="use-common-tools"></a>Korzystanie z popularnych narzędzi
Używaj popularnych narzędzi i narzędzi, takich jak MySQL Workbench lub mysqldump, aby zdalnie łączyć i przywracać dane w Azure Database for MySQL. Użyj tych narzędzi na komputerze klienckim z połączeniem internetowym, aby nawiązać połączenie z Azure Database for MySQL. Użyj połączenia szyfrowanego protokołem SSL, aby uzyskać najlepsze rozwiązania w zakresie zabezpieczeń, zobacz również [Konfigurowanie łączności SSL w Azure Database for MySQL](concepts-ssl-connection-security.md). Nie ma potrzeby przenoszenia plików zrzutu do żadnej specjalnej lokalizacji w chmurze podczas migracji do Azure Database for MySQL. 

## <a name="common-uses-for-dump-and-restore"></a>Typowe zastosowania zrzutów i przywracania
Możesz użyć narzędzi MySQL, takich jak mysqldump i mysqlpump, aby zrzucić i ładować bazy danych do bazy danych Azure MySQL w kilku typowych scenariuszach. W innych scenariuszach zamiast tego można użyć metody [importu i eksportu](concepts-migrate-import-export.md) .

- Podczas migrowania całej bazy danych używaj zrzutów baz danych. To zalecenie jest przechowywane podczas przesuwania dużej ilości danych MySQL lub w przypadku, gdy chcesz zminimalizować przerwy w działaniu usługi dla witryn lub aplikacji na żywo. 
-  Upewnij się, że podczas ładowania danych do usługi Azure Database for MySQL wszystkie tabele w bazie danych korzystają z aparatu magazynu InnoDB. Azure Database for MySQL obsługuje tylko aparat magazynu InnoDB i w związku z tym nie obsługuje alternatywnych aparatów pamięci masowej. Jeśli tabele są skonfigurowane z innymi aparatami magazynu, przed rozpoczęciem migracji do Azure Database for MySQL należy je przekonwertować na format aparatu InnoDB.
   Jeśli na przykład masz witrynę WordPress lub WebApp przy użyciu tabel MyISAM, najpierw przekonwertuj te tabele przez Migrowanie do formatu InnoDB przed przywróceniem do Azure Database for MySQL. Użyj klauzuli, `ENGINE=InnoDB` Aby ustawić aparat używany podczas tworzenia nowej tabeli, a następnie Przenieś dane do zgodnej tabeli przed przywróceniem. 

   ```sql
   INSERT INTO innodb_table SELECT * FROM myisam_table ORDER BY primary_key_columns
   ```
- Aby uniknąć problemów ze zgodnością, upewnij się, że w przypadku zrzucania baz danych w systemach źródłowym i docelowym jest używana ta sama wersja programu MySQL. Na przykład jeśli istniejący serwer MySQL jest w wersji 5,7, należy przeprowadzić migrację do Azure Database for MySQL skonfigurowany do uruchamiania wersji 5,7. Polecenie nie działa `mysql_upgrade` na serwerze Azure Database for MySQL i nie jest obsługiwane. Jeśli musisz przeprowadzić uaktualnienie między wersjami programu MySQL, najpierw wykonaj zrzut lub wyeksportuj niższą wersję bazy danych do nowszej wersji programu MySQL w Twoim środowisku. Następnie uruchom `mysql_upgrade` polecenie, przed podjęciem próby migracji do Azure Database for MySQL.

## <a name="performance-considerations"></a>Zagadnienia dotyczące wydajności
Aby zoptymalizować wydajność, należy wziąć pod uwagę następujące kwestie w przypadku zatopienia dużych baz danych:
-   Użyj `exclude-triggers` opcji w mysqldump, gdy zrzucane są bazy danych. Wyklucz wyzwalacze z plików zrzutów, aby uniknąć uruchamiania poleceń wyzwalacza podczas przywracania danych. 
-   Użyj `single-transaction` opcji, aby ustawić tryb izolacji transakcji na powtarzalne odczytywanie i wysyłanie instrukcji SQL Start Transaction do serwera przed zatopieniem danych. Zatopienie wielu tabel w ramach jednej transakcji powoduje, że niektóre dodatkowe magazyny mają być zużywane podczas przywracania. `single-transaction`Opcja i `lock-tables` opcja wykluczają się wzajemnie, ponieważ tabele blokad powodują niejawne zatwierdzenie oczekujących transakcji. Aby zrzucić duże tabele, Połącz `single-transaction` opcję z `quick` opcją. 
-   Użyj `extended-insert` składni wielowierszowej, która zawiera kilka list wartości. Powoduje to zmniejszenie pliku zrzutu i przyspieszenie operacji wstawiania podczas ponownego ładowania pliku.
-  Użyj `order-by-primary` opcji w mysqldump, gdy zrzucane są bazy danych, aby dane były określane w kolejności klucza podstawowego.
-   Użyj `disable-keys` opcji w mysqldump, gdy zrzucasz dane, aby wyłączyć ograniczenia klucza obcego przed załadowaniem. Wyłączenie kontroli kluczy obcych zapewnia wzrost wydajności. Włącz ograniczenia i sprawdź dane po załadowaniu, aby zapewnić integralność referencyjną.
-   W razie potrzeby użyj tabel partycjonowanych.
-   Równoległe ładowanie danych. Należy unikać zbyt dużej liczby równoległości, która spowodowałaby osiągnięcie limitu zasobów i monitorowanie zasobów przy użyciu metryk dostępnych w Azure Portal. 
-   Użyj `defer-table-indexes` opcji w mysqlpump podczas zrzucania baz danych, dzięki czemu tworzenie indeksów odbywa się po załadowaniu danych tabeli.
-   Użyj `skip-definer` opcji w mysqlpump, aby pominąć definicje i klauzule zabezpieczeń SQL z instrukcji CREATE dla widoków i procedur składowanych.  Po ponownym załadowaniu pliku zrzutu tworzone są obiekty korzystające z domyślnych definicji i wartości zabezpieczeń SQL.
-   Skopiuj pliki kopii zapasowej do magazynu lub obiektów blob platformy Azure, a następnie wykonaj przywracanie z tego miejsca, które powinny być znacznie szybsze niż przeprowadzenie przywracania przez Internet.

## <a name="create-a-backup-file-from-the-command-line-using-mysqldump"></a>Tworzenie pliku kopii zapasowej z wiersza polecenia przy użyciu mysqldump
Aby utworzyć kopię zapasową istniejącej bazy danych MySQL na serwerze lokalnym lub na maszynie wirtualnej, uruchom następujące polecenie: 
```bash
$ mysqldump --opt -u [uname] -p[pass] [dbname] > [backupfile.sql]
```

Parametry, które należy podać:
- [uname] Nazwa użytkownika bazy danych 
- chodzenia Hasło do bazy danych (należy zauważyć, że nie ma spacji między-p i hasłem) 
- Name Nazwa bazy danych 
- [backupfile. SQL] nazwa pliku kopii zapasowej bazy danych 
- [--opt] Opcja mysqldump 

Na przykład, aby utworzyć kopię zapasową bazy danych o nazwie "TestDB" na serwerze MySQL przy użyciu nazwy użytkownika "Użytkownik testowy" i bez hasła do pliku testdb_backup. SQL, użyj następującego polecenia. Polecenie tworzy kopię zapasową `testdb` bazy danych w pliku o nazwie `testdb_backup.sql` , który zawiera wszystkie instrukcje SQL wymagane do ponownego utworzenia bazy danych. Upewnij się, że nazwa użytkownika "Użytkownik testowy" ma co najmniej uprawnienie Wybieranie dla tabel z zrzutem, Pokaż widok dla widoków z zrzutem, WYZWÓL wyzwalacze wyzwalane i Zablokuj tabele, jeśli opcja--Single-Transaction nie jest używana.

```bash
GRANT SELECT, LOCK TABLES, SHOW VIEW ON *.* TO 'testuser'@'hostname' IDENTIFIED BY 'password';
```

```bash
$ mysqldump -u root -p testdb > testdb_backup.sql
```
Aby wybrać określone tabele w bazie danych, aby utworzyć kopię zapasową, należy wyświetlić listę nazw tabel rozdzielonych spacjami. Na przykład, aby utworzyć kopię zapasową tylko tabel Tabela1 i tabela2 z elementu "TestDB", wykonaj następujące czynności: 
```bash
$ mysqldump -u root -p testdb table1 table2 > testdb_tables_backup.sql
```
Aby utworzyć kopię zapasową więcej niż jednej bazy danych, należy użyć przełącznika--Database i wyświetlić listę nazw baz danych rozdzielonych spacjami. 
```bash
$ mysqldump -u root -p --databases testdb1 testdb3 testdb5 > testdb135_backup.sql 
```

## <a name="create-a-database-on-the-target-azure-database-for-mysql-server"></a>Tworzenie bazy danych na serwerze docelowym Azure Database for MySQL
Utwórz pustą bazę danych na docelowym serwerze Azure Database for MySQL, na którym chcesz przeprowadzić migrację danych. Utwórz bazę danych za pomocą narzędzia, takiego jak MySQL Workbench. Baza danych może mieć taką samą nazwę, jak baza danych, która zawiera dane z danymi zrzutu, lub można utworzyć bazę danych o innej nazwie.

Aby nawiązać połączenie, Znajdź informacje o połączeniu w **przeglądzie** Azure Database for MySQL.

![Znajdź informacje o połączeniu w Azure Portal](./media/concepts-migrate-dump-restore/1_server-overview-name-login.png)

Dodaj informacje o połączeniu do usługi MySQL Workbench.

![Parametry połączenia MySQL Workbench](./media/concepts-migrate-dump-restore/2_setup-new-connection.png)

## <a name="preparing-the-target-azure-database-for-mysql-server-for-fast-data-loads"></a>Przygotowywanie docelowego serwera Azure Database for MySQL na potrzeby szybkiego ładowania danych
Aby przygotować docelowy serwer Azure Database for MySQL do szybszego ładowania danych, należy zmienić następujące parametry serwera i konfigurację.
- max_allowed_packet — ustaw na 1073741824 (tj. 1 GB), aby zapobiec wszelkim problemom z przepełnieniem spowodowanym długimi wierszami.
- slow_query_log — ustaw wartość wyłączone, aby wyłączyć dziennik wolnych zapytań. Pozwoli to wyeliminować obciążenie spowodowane przez wolne rejestrowanie zapytań podczas ładowania danych.
- query_store_capture_mode — ustaw wartość Brak, aby wyłączyć magazyn zapytań. Pozwoli to wyeliminować obciążenie spowodowane przez działania próbkowania według magazynu zapytań.
- innodb_buffer_pool_size — Skaluj serwer w górę do 32 rdzeń wirtualny pamięci podręcznej zoptymalizowanej od warstwy cenowej portalu podczas migracji, aby zwiększyć innodb_buffer_pool_size. Innodb_buffer_pool_size można zwiększyć tylko poprzez skalowanie w górę obliczeń dla serwera Azure Database for MySQL.
- innodb_io_capacity & innodb_io_capacity_max — Zmień na 9000 z parametrów serwera w Azure Portal, aby zwiększyć użycie operacji we/wy w celu optymalizacji pod kątem szybkości migracji.
- innodb_write_io_threads & innodb_write_io_threads — Zmień na 4 z parametrów serwera w programie Azure Portal, aby zwiększyć szybkość migracji.
- Skalowanie w górę warstwy magazynowania — liczba operacji we/wy dla serwera Azure Database for MySQL przyrostowo rośnie wraz ze wzrostem warstwy magazynowania. W przypadku szybszych obciążeń warto zwiększyć warstwę magazynowania, aby zwiększyć liczbę operacji we/wy zainicjowanej. Należy pamiętać, że magazyn można skalować w górę, a nie w dół.

Po zakończeniu migracji można przywrócić jej poprzednie wartości parametrów serwera i konfiguracji warstwy obliczeniowej. 

## <a name="restore-your-mysql-database-using-command-line-or-mysql-workbench"></a>Przywracanie bazy danych MySQL przy użyciu wiersza polecenia lub MySQL Workbench
Po utworzeniu docelowej bazy danych można użyć polecenia MySQL lub MySQL Workbench do przywrócenia danych do konkretnej nowo utworzonej bazy danych z pliku zrzutu.
```bash
mysql -h [hostname] -u [uname] -p[pass] [db_to_restore] < [backupfile.sql]
```
W tym przykładzie Przywróć dane do nowo utworzonej bazy danych na docelowym serwerze Azure Database for MySQL.
```bash
$ mysql -h mydemoserver.mysql.database.azure.com -u myadmin@mydemoserver -p testdb < testdb_backup.sql
```
## <a name="export-using-phpmyadmin"></a>Eksportuj przy użyciu PHPMyAdmin
W celu wyeksportowania można użyć typowego narzędzia phpMyAdmin, które mogło już być zainstalowane lokalnie w danym środowisku. Aby wyeksportować bazę danych MySQL przy użyciu PHPMyAdmin:
1. Otwórz phpMyAdmin.
2. Wybierz bazę danych. Kliknij nazwę bazy danych na liście po lewej stronie. 
3. Kliknij link **Eksportuj** . Zostanie wyświetlona nowa strona umożliwiająca wyświetlenie zrzutu bazy danych.
4. W obszarze eksport kliknij link **Zaznacz wszystko** , aby wybrać tabele w bazie danych. 
5. W obszarze Opcje SQL kliknij odpowiednie opcje. 
6. Kliknij opcję **Zapisz jako plik** i odpowiednią opcję kompresji, a następnie kliknij przycisk **Przejdź** . Zostanie wyświetlone okno dialogowe z monitem o zapisanie pliku lokalnie.

## <a name="import-using-phpmyadmin"></a>Importuj przy użyciu PHPMyAdmin
Importowanie bazy danych jest podobne do eksportowania. Wykonaj następujące czynności:
1. Otwórz phpMyAdmin. 
2. Na stronie Konfiguracja phpMyAdmin kliknij przycisk **Dodaj** , aby dodać serwer Azure Database for MySQL. Podaj szczegóły połączenia i informacje logowania.
3. Utwórz odpowiednio nazwę bazy danych i wybierz ją po lewej stronie ekranu. Aby ponownie zapisać istniejącą bazę danych, kliknij nazwę bazy danych, zaznacz wszystkie pola wyboru obok nazwy tabeli, a następnie wybierz pozycję **Usuń, aby usunąć** istniejące tabele. 
4. Kliknij link **SQL** , aby wyświetlić stronę, na której można wpisywać polecenia SQL, lub Przekaż plik SQL. 
5. Użyj przycisku **Przeglądaj** , aby znaleźć plik bazy danych. 
6. Kliknij przycisk **Przejdź** , aby wyeksportować kopię zapasową, wykonać polecenia SQL i ponownie utworzyć bazę danych.

## <a name="known-issues"></a>Znane problemy
Aby uzyskać znane problemy, porady i wskazówki, zalecamy zapoznanie się z [blogiem usługi techcommunity](https://techcommunity.microsoft.com/t5/azure-database-for-mysql/tips-and-tricks-in-using-mysqldump-and-mysql-restore-to-azure/ba-p/916912).

## <a name="next-steps"></a>Następne kroki
- [Połącz aplikacje z Azure Database for MySQL](./howto-connection-string.md).
- Aby uzyskać więcej informacji na temat migrowania baz danych do Azure Database for MySQL, zobacz [Przewodnik po migracji bazy danych](https://aka.ms/datamigration).
