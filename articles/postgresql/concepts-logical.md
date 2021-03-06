---
title: Dekodowanie logiczne-Azure Database for PostgreSQL-pojedynczy serwer
description: Opisuje logiczne dekodowanie i wal2json na potrzeby przechwytywania zmian danych w ramach jednego serwera Azure Database for PostgreSQL
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 06/22/2020
ms.openlocfilehash: 363c003a915763a7ab1165c2e0d8f945bc3dd510
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "85213690"
---
# <a name="logical-decoding"></a>Dekodowanie logiczne
 
[Dekodowanie logiczne w programie PostgreSQL](https://www.postgresql.org/docs/current/logicaldecoding.html) umożliwia przesyłanie strumieniowe zmian danych do użytkowników zewnętrznych. Dekodowanie logiczne jest popularne w przypadku scenariuszy przesyłania strumieniowego zdarzeń i przechwytywania zmian danych.

Dekodowanie logiczne używa wtyczki wyjściowej do przekonwertowania dziennika zapisu z wyprzedzeniem (WAL) Postgres na format możliwy do odczytu. Azure Database for PostgreSQL udostępnia wtyczki danych wyjściowych [wal2json](https://github.com/eulerto/wal2json), [test_decoding](https://www.postgresql.org/docs/current/test-decoding.html) i pgoutput. pgoutput jest udostępniana przez Postgres z Postgres w wersji 10.

Aby zapoznać się z omówieniem działania dekodowania logicznego Postgres, [odwiedź nasz blog](https://techcommunity.microsoft.com/t5/azure-database-for-postgresql/change-data-capture-in-postgres-how-to-use-logical-decoding-and/ba-p/1396421). 

> [!NOTE]
> Dekodowanie logiczne jest w publicznej wersji zapoznawczej na Azure Database for PostgreSQL-pojedynczym serwerze.


## <a name="set-up-your-server"></a>Skonfiguruj serwer 
Logiczne dekodowanie i [odczytywanie replik](concepts-read-replicas.md) są zależne od Postgres zapisu (WAL) w celu uzyskania informacji. Te dwie funkcje wymagają różnych poziomów rejestrowania z Postgres. Dekodowanie logiczne wymaga wyższego poziomu rejestrowania niż repliki odczytu.

Aby skonfigurować odpowiedni poziom rejestrowania, użyj parametru Obsługa replikacji platformy Azure. Obsługa replikacji platformy Azure ma trzy opcje ustawień:

* **Wyłączone** — umieszcza najniższe informacje w Wal. To ustawienie nie jest dostępne na większości serwerów Azure Database for PostgreSQL.  
* **Replika** — większa niż **wyłączona**. Jest to minimalny poziom rejestrowania, który jest wymagany do działania [replik odczytu](concepts-read-replicas.md) . To ustawienie jest domyślne na większości serwerów.
* **Logiczne** — więcej informacji niż **replika**. Jest to minimalny poziom rejestrowania kodu logicznego do pracy. Odczytaj repliki również działają w tym ustawieniu.

Po zmianie tego parametru należy ponownie uruchomić serwer. Wewnętrznie, ten parametr ustawia parametry Postgres `wal_level` , `max_replication_slots` i `max_wal_senders` .

### <a name="using-azure-cli"></a>Korzystanie z interfejsu wiersza polecenia platformy Azure

1. Ustaw platformę Azure. replication_support na `logical` .
   ```
   az postgres server configuration set --resource-group mygroup --server-name myserver --name azure.replication_support --value logical
   ``` 

2. Uruchom ponownie serwer, aby zastosować zmianę.
   ```
   az postgres server restart --resource-group mygroup --name myserver
   ```

### <a name="using-azure-portal"></a>Korzystanie z witryny Azure Portal

1. Ustaw wartość **logiczna**Obsługa replikacji platformy Azure. Wybierz pozycję **Zapisz**.

   ![Azure Database for PostgreSQL-replikacja — Obsługa replikacji platformy Azure](./media/concepts-logical/replication-support.png)

2. Uruchom ponownie serwer, aby zastosować zmiany, wybierając opcję **tak**.

   ![Azure Database for PostgreSQL — Potwierdź ponowne uruchomienie](./media/concepts-logical/confirm-restart.png)


## <a name="start-logical-decoding"></a>Rozpocznij dekodowanie logiczne

Dekodowanie logiczne może być używane za pośrednictwem protokołu przesyłania strumieniowego lub interfejsu SQL. Obie metody używają [miejsc replikacji](https://www.postgresql.org/docs/current/logicaldecoding-explanation.html#LOGICALDECODING-REPLICATION-SLOTS). Gniazdo reprezentuje strumień zmian z pojedynczej bazy danych.

Korzystanie z gniazda replikacji wymaga uprawnień replikacji Postgres. W tej chwili uprawnienie replikacji jest dostępne tylko dla administratora serwera. 

### <a name="streaming-protocol"></a>Protokół przesyłania strumieniowego
Użycie protokołu przesyłania strumieniowego jest często preferowane. Możesz utworzyć własnego użytkownika/łącznik lub użyć narzędzia, takiego jak [Debezium](https://debezium.io/). 

Zapoznaj się z dokumentacją wal2json, aby zapoznać się [z przykładem przy użyciu protokołu przesyłania strumieniowego z pg_recvlogical](https://github.com/eulerto/wal2json#pg_recvlogical).


### <a name="sql-interface"></a>Interfejs SQL
W poniższym przykładzie użyto interfejsu SQL z wtyczką wal2json.
 
1. Utwórz miejsce.
   ```SQL
   SELECT * FROM pg_create_logical_replication_slot('test_slot', 'wal2json');
   ```
 
2. Wydaj polecenia SQL. Przykład:
   ```SQL
   CREATE TABLE a_table (
      id varchar(40) NOT NULL,
      item varchar(40),
      PRIMARY KEY (id)
   );
   
   INSERT INTO a_table (id, item) VALUES ('id1', 'item1');
   DELETE FROM a_table WHERE id='id1';
   ```

3. Użyj zmian.
   ```SQL
   SELECT data FROM pg_logical_slot_get_changes('test_slot', NULL, NULL, 'pretty-print', '1');
   ```

   Dane wyjściowe będą wyglądać następująco:
   ```
   {
         "change": [
         ]
   }
   {
         "change": [
                  {
                           "kind": "insert",
                           "schema": "public",
                           "table": "a_table",
                           "columnnames": ["id", "item"],
                           "columntypes": ["character varying(40)", "character varying(40)"],
                           "columnvalues": ["id1", "item1"]
                  }
         ]
   }
   {
         "change": [
                  {
                           "kind": "delete",
                           "schema": "public",
                           "table": "a_table",
                           "oldkeys": {
                                 "keynames": ["id"],
                                 "keytypes": ["character varying(40)"],
                                 "keyvalues": ["id1"]
                           }
                  }
         ]
   }
   ```

4. Upuść miejsce po zakończeniu korzystania z niego.
   ```SQL
   SELECT pg_drop_replication_slot('test_slot'); 
   ```


## <a name="monitoring-slots"></a>Gniazda monitorowania

Należy monitorować logiczne dekodowanie. Wszystkie nieużywane gniazda replikacji muszą zostać porzucone. Gniazda znajdują się na Postgres dzienników i odpowiednich wykazów systemowych do momentu odczytania zmian przez konsumenta. Jeśli użytkownik ulegnie awarii lub nie został prawidłowo skonfigurowany, nieużywane dzienniki będą wymagały użycia i wypełnienia magazynu. Ponadto niewykorzystane dzienniki zwiększają ryzyko związane z IDENTYFIKATORem transakcji wraparound. Obie sytuacje mogą spowodować, że serwer stanie się niedostępny. W związku z tym ważne jest, aby gniazda replikacji logicznej były używane w sposób ciągły. Jeśli gniazdo replikacji logicznej nie jest już używane, Porzuć je od razu.

Kolumna "Active" w widoku pg_replication_slots wskazuje, czy klient jest połączony z miejscem.
```SQL
SELECT * FROM pg_replication_slots;
```

[Ustawianie alertów](howto-alert-on-metric.md) dotyczących *używanej przestrzeni dyskowej* oraz *maksymalnego opóźnienia między replikami* w celu powiadomienia, gdy wartości rosną do poprzednich progów normalnych. 

> [!IMPORTANT]
> Należy porzucić nieużywane gniazda replikacji. Niewykonanie tej czynności może prowadzić do niedostępności serwera.

## <a name="how-to-drop-a-slot"></a>Jak usunąć gniazdo
Jeśli nie korzystasz aktywnie z miejsca replikacji, należy je usunąć.

Aby porzucić miejsce replikacji o nazwie `test_slot` przy użyciu języka SQL:
```SQL
SELECT pg_drop_replication_slot('test_slot');
```

> [!IMPORTANT]
> Jeśli zatrzymasz korzystanie z dekodowania logicznego, Zmień platformę Azure. replication_support z powrotem na `replica` lub `off` . Szczegóły WAL przechowywane przez `logical` program są bardziej pełne i powinny być wyłączone, gdy dekodowanie logiczne nie jest używane. 

 
## <a name="next-steps"></a>Następne kroki

* Zapoznaj się z dokumentacją Postgres, aby [dowiedzieć się więcej na temat dekodowania logicznego](https://www.postgresql.org/docs/current/logicaldecoding-explanation.html).
* Skontaktuj się z [naszym zespołem](mailto:AskAzureDBforPostgreSQL@service.microsoft.com) , jeśli masz pytania dotyczące dekodowania logicznego.
* Dowiedz się więcej o [odczytaniu replik](concepts-read-replicas.md).

