---
title: Konfigurowanie replikacji danych — Azure Database for MariaDB
description: W tym artykule opisano sposób konfigurowania replikacja typu data-in w Azure Database for MariaDB.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: how-to
ms.date: 6/11/2020
ms.openlocfilehash: 623c072cb8cb2c7fb1b9b6ec7d3ea661302d5e6a
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/08/2020
ms.locfileid: "86104677"
---
# <a name="configure-data-in-replication-in-azure-database-for-mariadb"></a>Konfigurowanie replikacja typu data-in w Azure Database for MariaDB

W tym artykule opisano sposób konfigurowania [replikacja typu Data-in](concepts-data-in-replication.md) w Azure Database for MariaDB przez skonfigurowanie serwerów głównych i replik. W tym artykule założono, że masz pewne doświadczenie w korzystaniu z serwerów i baz danych MariaDB.

Aby utworzyć replikę w usłudze Azure Database for MariaDB, [replikacja typu Data-in](concepts-data-in-replication.md) synchronizuje dane z lokalnego serwera MariaDB, w maszynach wirtualnych lub w usługach bazy danych w chmurze. Replikacja typu data-in jest wykonywana za pomocą technologii replikacji opartej na pozycji w pliku dziennika binarnego (binlog) natywnej dla programu MariaDB. Aby dowiedzieć się więcej na temat replikacji binlog, zobacz [Omówienie replikacji binlog](https://mariadb.com/kb/en/library/replication-overview/).

Przed wykonaniem kroków opisanych w tym artykule Przejrzyj [ograniczenia i wymagania](concepts-data-in-replication.md#limitations-and-considerations) replikacji danych.

> [!NOTE]
> Jeśli serwer główny jest w wersji 10,2 lub nowszej, zalecamy skonfigurowanie replikacja typu data-in przy użyciu [globalnego identyfikatora transakcji](https://mariadb.com/kb/en/library/gtid/).


## <a name="create-a-mariadb-server-to-use-as-a-replica"></a>Utwórz serwer MariaDB, który ma być używany jako replika

1. Utwórz nowy serwer Azure Database for MariaDB (na przykład replica.mariadb.database.azure.com). Serwer jest serwerem repliki w replikacja typu data-in.

    Aby dowiedzieć się więcej o tworzeniu serwera, zobacz [Tworzenie serwera Azure Database for MariaDB przy użyciu Azure Portal](quickstart-create-mariadb-server-database-using-azure-portal.md).

   > [!IMPORTANT]
   > Należy utworzyć serwer Azure Database for MariaDB w warstwach cenowych Ogólnego przeznaczenia lub zoptymalizowanych pod kątem pamięci.

2. Utwórz identyczne konta użytkowników i odpowiednie uprawnienia.
    
    Konta użytkowników nie są replikowane z serwera głównego do serwera repliki. Aby zapewnić użytkownikom dostęp do serwera repliki, należy ręcznie utworzyć wszystkie konta i odpowiednie uprawnienia na nowo utworzonym serwerze Azure Database for MariaDB.

3. Dodaj adres IP serwera głównego do reguł zapory repliki. 

   Zaktualizuj reguły zapory za pomocą [witryny Azure Portal](howto-manage-firewall-portal.md) lub [interfejsu wiersza polecenia platformy Azure](howto-manage-firewall-cli.md).

> [!NOTE]
> Komunikacja bezpłatna bez opłat
>
> Firma Microsoft obsługuje różnorodne i dołączane środowiska. Ten artykuł zawiera odwołania do programu Word _podrzędny_. Przewodnik po [stylu firmy Microsoft dla komunikacji bezpłatnej](https://github.com/MicrosoftDocs/microsoft-style-guide/blob/master/styleguide/bias-free-communication.md) jest rozpoznawany jako wykluczony wyraz. Słowo jest używane w tym artykule w celu zapewnienia spójności, ponieważ jest to obecnie słowo, które jest wyświetlane w oprogramowaniu. W przypadku zaktualizowania oprogramowania w celu usunięcia wyrazu ten artykuł zostanie zaktualizowany w celu wyrównania.
>

## <a name="configure-the-master-server"></a>Konfigurowanie serwera głównego

Poniższe kroki przygotowują i skonfigurują serwer MariaDB hostowany lokalnie, na maszynie wirtualnej lub w usłudze bazy danych w chmurze dla replikacja typu data-in. Serwer MariaDB jest serwerem głównym w replikacja typu data-in.

1. Przed kontynuowaniem Przejrzyj [wymagania dotyczące serwera głównego](concepts-data-in-replication.md#requirements) . 

   Na przykład upewnij się, że serwer główny zezwala na ruch przychodzący i wychodzący na porcie 3306 i że serwer główny ma **publiczny adres IP**, usługa DNS jest publicznie dostępna lub ma w pełni kwalifikowaną nazwę domeny (FQDN). 
   
   Przetestuj łączność z serwerem głównym, próbując nawiązać połączenie za pomocą narzędzia, takiego jak wiersz polecenia MySQL hostowany na innym komputerze lub z [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) dostępnego w Azure Portal.

2. Włącz rejestrowanie plików binarnych.
    
    Aby sprawdzić, czy na wzorcu jest włączone rejestrowanie plików binarnych, wprowadź następujące polecenie:

   ```sql
   SHOW VARIABLES LIKE 'log_bin';
   ```

   Jeśli zmienna [`log_bin`](https://mariadb.com/kb/en/library/replication-and-binary-log-server-system-variables/#log_bin) zwraca wartość `ON` , rejestrowanie binarne jest włączone na serwerze.

   Jeśli `log_bin` zwraca wartość `OFF` , edytuj plik **My. cnf** , aby `log_bin=ON` włączyć rejestrowanie binarne. Aby zmiana zaczęła obowiązywać, należy ponownie uruchomić serwer.

3. Skonfiguruj ustawienia serwera głównego.

    Replikacja typu data-in wymaga, `lower_case_table_names` Aby parametr był spójny między serwerem głównym a serwerami repliki. `lower_case_table_names`Parametr jest domyślnie ustawiony na wartość `1` Azure Database for MariaDB.

   ```sql
   SET GLOBAL lower_case_table_names = 1;
   ```

4. Utwórz nową rolę replikacji i Skonfiguruj uprawnienia.

   Utwórz konto użytkownika na serwerze głównym, który został skonfigurowany z uprawnieniami replikacji. Konto można utworzyć przy użyciu poleceń SQL lub MySQL Workbench. Jeśli planujesz replikację przy użyciu protokołu SSL, musisz go określić podczas tworzenia konta użytkownika.
   
   Aby dowiedzieć się, jak dodać konta użytkowników na serwerze głównym, zapoznaj się z [dokumentacją MariaDB](https://mariadb.com/kb/en/library/create-user/).

   Przy użyciu następujących poleceń Nowa rola replikacji może uzyskać dostęp do serwera głównego z dowolnego komputera, a nie tylko z komputera, który hostuje wzorzec. W przypadku tego dostępu określ **syncuser \@ "%"** w poleceniu, aby utworzyć użytkownika.
   
   Aby dowiedzieć się więcej na temat dokumentacji MariaDB, zobacz [Określanie nazw kont](https://mariadb.com/kb/en/library/create-user/#account-names).

   **Polecenie SQL**

   - Replikacja przy użyciu protokołu SSL

       Aby wymagać protokołu SSL dla wszystkich połączeń użytkowników, wprowadź następujące polecenie, aby utworzyć użytkownika:

       ```sql
       CREATE USER 'syncuser'@'%' IDENTIFIED BY 'yourpassword';
       GRANT REPLICATION SLAVE ON *.* TO ' syncuser'@'%' REQUIRE SSL;
       ```

   - Replikacja bez protokołu SSL

       Jeśli protokół SSL nie jest wymagany dla wszystkich połączeń, wprowadź następujące polecenie, aby utworzyć użytkownika:
    
       ```sql
       CREATE USER 'syncuser'@'%' IDENTIFIED BY 'yourpassword';
       GRANT REPLICATION SLAVE ON *.* TO ' syncuser'@'%';
       ```

   **MySQL Workbench**

   Aby utworzyć rolę replikacji w programie MySQL Workbench, w okienku **Zarządzanie** wybierz pozycję **Użytkownicy i uprawnienia**. Następnie wybierz pozycję **Dodaj konto**.
 
   ![Użytkownicy i uprawnienia](./media/howto-data-in-replication/users_privileges.png)

   Wprowadź nazwę użytkownika w polu **Nazwa logowania** .

   ![Użytkownik synchronizacji](./media/howto-data-in-replication/syncuser.png)
 
   Wybierz panel **role administracyjne** , a następnie na liście **uprawnień globalnych**wybierz pozycję **replikacja podrzędna**. Wybierz pozycję **Zastosuj** , aby utworzyć rolę replikacji.

   ![Replikacja podrzędna](./media/howto-data-in-replication/replicationslave.png)


5. Ustaw serwer główny do trybu tylko do odczytu.

   Przed zazrzutem bazy danych serwer musi być umieszczony w trybie tylko do odczytu. W trybie tylko do odczytu główny serwer nie może przetwarzać żadnych transakcji zapisu. Aby uniknąć wpływu na działalność biznesową, Zaplanuj okno tylko do odczytu w czasie poza szczytem.

   ```sql
   FLUSH TABLES WITH READ LOCK;
   SET GLOBAL read_only = ON;
   ```

6. Pobierz bieżącą nazwę pliku dziennika binarnego i przesunięcia.

   Aby określić bieżącą nazwę pliku dziennika binarnego i przesunięcia, uruchom polecenie [`show master status`](https://mariadb.com/kb/en/library/show-master-status/) .
    
   ```sql
   show master status;
   ```
   Wyniki powinny wyglądać podobnie do poniższej tabeli:
   
   ![Wyniki stanu głównego](./media/howto-data-in-replication/masterstatus.png)

   Zanotuj nazwę pliku binarnego, ponieważ zostanie ona użyta w dalszych krokach.
   
7. Pobierz pozycję GTID (opcjonalnie wymagana do replikacji z GTID).

   Uruchom funkcję [`BINLOG_GTID_POS`](https://mariadb.com/kb/en/library/binlog_gtid_pos/) w celu uzyskania pozycji GTID dla odpowiedniej nazwy pliku binlog i przesunięcia.
  
    ```sql
    select BINLOG_GTID_POS('<binlog file name>', <binlog offset>);
    ```
 

## <a name="dump-and-restore-the-master-server"></a>Zrzuć i przywróć serwer główny

1. Zrzuć wszystkie bazy danych z serwera głównego.

   Użyj mysqldump, aby zrzucić wszystkie bazy danych z serwera głównego. Nie jest konieczne zrzucanie biblioteki MySQL i biblioteki testowej.

    Aby uzyskać więcej informacji, zobacz temat [Zrzuć i Przywróć](howto-migrate-dump-restore.md).

2. Ustaw serwer główny na tryb do odczytu i zapisu.

   Po zrzucie bazy danych Zmień serwer główny MariaDB z powrotem na tryb odczytu/zapisu.

   ```sql
   SET GLOBAL read_only = OFF;
   UNLOCK TABLES;
   ```

3. Przywróć plik zrzutu na nowy serwer.

   Przywróć plik zrzutu do serwera utworzonego w usłudze Azure Database for MariaDB. Zapoznaj się z tematem [zrzuć & Restore](howto-migrate-dump-restore.md) , aby przywrócić plik zrzutu do serwera MariaDB.

   Jeśli plik zrzutu jest duży, przekaż go do maszyny wirtualnej na platformie Azure w tym samym regionie, w którym znajduje się serwer repliki. Przywróć go na serwerze Azure Database for MariaDB z maszyny wirtualnej.

## <a name="link-the-master-and-replica-servers-to-start-data-in-replication"></a>Łączenie serwerów głównych i replik w celu uruchomienia replikacja typu data-in

1. Ustaw serwer główny.

   Wszystkie funkcje replikacja typu data-in są wykonywane przez procedury składowane. Wszystkie procedury można znaleźć w [replikacja typu Data-in procedurach składowanych](reference-data-in-stored-procedures.md). Procedury składowane można uruchamiać w programie MySQL Shell lub MySQL Workbench.

   Aby połączyć dwa serwery i rozpocząć replikację, zaloguj się do docelowego serwera repliki w usłudze Azure DB dla MariaDB. Następnie ustaw wystąpienie zewnętrzne jako serwer główny przy użyciu `mysql.az_replication_change_master` `mysql.az_replication_change_master_with_gtid` procedury składowanej lub na serwerze usługi Azure DB dla MariaDB.

   ```sql
   CALL mysql.az_replication_change_master('<master_host>', '<master_user>', '<master_password>', 3306, '<master_log_file>', <master_log_pos>, '<master_ssl_ca>');
   ```
   
   lub
   
   ```sql
   CALL mysql.az_replication_change_master_with_gtid('<master_host>', '<master_user>', '<master_password>', 3306, '<master_gtid_pos>', '<master_ssl_ca>');
   ```

   - master_host: Nazwa hosta serwera głównego
   - master_user: Nazwa użytkownika serwera głównego
   - master_password: hasło dla serwera głównego
   - master_log_file: nie uruchomiono binarnej nazwy pliku dziennika`show master status`
   - master_log_pos: uruchamianie binarnej lokalizacji dziennika`show master status`
   - master_gtid_pos: GTID położenie z uruchamiania`select BINLOG_GTID_POS('<binlog file name>', <binlog offset>);`
   - master_ssl_ca: kontekst certyfikatu urzędu certyfikacji. Jeśli nie używasz protokołu SSL, Przekaż pusty ciąg. *
    
    
    * Zalecamy przekazywanie w master_ssl_ca parametr jako zmienna. Więcej informacji można znaleźć w poniższych przykładach.

   **Przykłady**

   - Replikacja przy użyciu protokołu SSL

       Utwórz zmienną `@cert` , uruchamiając następujące polecenia:

       ```sql
       SET @cert = '-----BEGIN CERTIFICATE-----
       PLACE YOUR PUBLIC KEY CERTIFICATE\'S CONTEXT HERE
       -----END CERTIFICATE-----'
       ```

       Replikacja przy użyciu protokołu SSL jest skonfigurowana między serwerem głównym hostowanym w domenie companya.com i serwerem repliki hostowanym w Azure Database for MariaDB. Ta procedura składowana jest uruchamiana w replice.
    
       ```sql
       CALL mysql.az_replication_change_master('master.companya.com', 'syncuser', 'P@ssword!', 3306, 'mariadb-bin.000016', 475, @cert);
       ```
   - Replikacja bez protokołu SSL

       Replikacja bez protokołu SSL jest skonfigurowana między serwerem głównym hostowanym w domenie companya.com i serwerem repliki hostowanym w Azure Database for MariaDB. Ta procedura składowana jest uruchamiana w replice.

       ```sql
       CALL mysql.az_replication_change_master('master.companya.com', 'syncuser', 'P@ssword!', 3306, 'mariadb-bin.000016', 475, '');
       ```

2. Rozpocznij replikację.

   Wywołaj `mysql.az_replication_start` procedurę składowaną, aby rozpocząć replikację.

   ```sql
   CALL mysql.az_replication_start;
   ```

3. Sprawdź stan replikacji.

   Wywołaj [`show slave status`](https://mariadb.com/kb/en/library/show-slave-status/) polecenie na serwerze repliki, aby wyświetlić stan replikacji.
    
   ```sql
   show slave status;
   ```

   Jeśli `Slave_IO_Running` i `Slave_SQL_Running` są w stanie `yes` , a wartość `Seconds_Behind_Master` to `0` , działa replikacja. `Seconds_Behind_Master`wskazuje, jak późna jest replika. Jeśli ta wartość nie jest `0` , replika przetwarza aktualizacje.

4. Zaktualizuj odpowiednie zmienne serwera, aby zapewnić bezpieczeństwo replikacji danych (wymagane tylko w przypadku replikacji bez GTID).
    
    Ze względu na ograniczenie replikacji natywnej w MariaDB należy ustawić [`sync_master_info`](https://mariadb.com/kb/en/library/replication-and-binary-log-system-variables/#sync_master_info) i [`sync_relay_log_info`](https://mariadb.com/kb/en/library/replication-and-binary-log-system-variables/#sync_relay_log_info) zmienne w replikacji bez scenariusza GTID.

    Sprawdź, czy serwer podrzędny `sync_master_info` i `sync_relay_log_info` zmienne, aby upewnić się, że replikacja danych jest stabilna, i Ustaw zmienne na `1` .
    
## <a name="other-stored-procedures"></a>Inne procedury składowane

### <a name="stop-replication"></a>Zatrzymywanie replikacji

Aby zatrzymać replikację między serwerem głównym a programem repliki, należy użyć następującej procedury składowanej:

```sql
CALL mysql.az_replication_stop;
```

### <a name="remove-the-replication-relationship"></a>Usuń relację replikacji

Aby usunąć relację między serwerem głównym a programem repliki, należy użyć następującej procedury składowanej:

```sql
CALL mysql.az_replication_remove_master;
```

### <a name="skip-the-replication-error"></a>Pomiń błąd replikacji

Aby pominąć błąd replikacji i zezwolić na replikację, należy użyć następującej procedury składowanej:
    
```sql
CALL mysql.az_replication_skip_counter;
```

## <a name="next-steps"></a>Następne kroki
Dowiedz się więcej na temat [replikacja typu data-in](concepts-data-in-replication.md) Azure Database for MariaDB.
