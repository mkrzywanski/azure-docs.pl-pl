---
title: Operacje języka DDL w Azure Cosmos DB interfejs API Cassandra z platformy Spark
description: W tym artykule opisano operacje dotyczące przestrzeni kluczy i języka DDL w tabeli dotyczącej Azure Cosmos DB interfejs API Cassandra z platformy Spark.
author: kanshiG
ms.author: govindk
ms.reviewer: sngun
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: how-to
ms.date: 09/24/2018
ms.openlocfilehash: 30cac5894998ca2bb9c37217820e1000ed97ba5d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "85260568"
---
# <a name="ddl-operations-in-azure-cosmos-db-cassandra-api-from-spark"></a>Operacje języka DDL w Azure Cosmos DB interfejs API Cassandra z platformy Spark

W tym artykule opisano operacje dotyczące przestrzeni kluczy i języka DDL w tabeli dotyczącej Azure Cosmos DB interfejs API Cassandra z platformy Spark.

## <a name="cassandra-api-related-configuration"></a>Konfiguracja związana z interfejs API Cassandra 

```scala
import org.apache.spark.sql.cassandra._

//Spark connector
import com.datastax.spark.connector._
import com.datastax.spark.connector.cql.CassandraConnector

//CosmosDB library for multiple retry
import com.microsoft.azure.cosmosdb.cassandra

//Connection-related
spark.conf.set("spark.cassandra.connection.host","YOUR_ACCOUNT_NAME.cassandra.cosmosdb.azure.com")
spark.conf.set("spark.cassandra.connection.port","10350")
spark.conf.set("spark.cassandra.connection.ssl.enabled","true")
spark.conf.set("spark.cassandra.auth.username","YOUR_ACCOUNT_NAME")
spark.conf.set("spark.cassandra.auth.password","YOUR_ACCOUNT_KEY")
spark.conf.set("spark.cassandra.connection.factory", "com.microsoft.azure.cosmosdb.cassandra.CosmosDbConnectionFactory")

//Throughput-related...adjust as needed
spark.conf.set("spark.cassandra.output.batch.size.rows", "1")
spark.conf.set("spark.cassandra.connection.connections_per_executor_max", "10")
spark.conf.set("spark.cassandra.output.concurrent.writes", "1000")
spark.conf.set("spark.cassandra.concurrent.reads", "512")
spark.conf.set("spark.cassandra.output.batch.grouping.buffer.size", "1000")
spark.conf.set("spark.cassandra.connection.keep_alive_ms", "600000000")
```

## <a name="keyspace-ddl-operations"></a>Operacje języka DDL dotyczące przestrzeni kluczy

### <a name="create-a-keyspace"></a>Utwórz miejsce na dysku

```scala
//Cassandra connector instance
val cdbConnector = CassandraConnector(sc)

// Create keyspace
cdbConnector.withSessionDo(session => session.execute("CREATE KEYSPACE IF NOT EXISTS books_ks WITH REPLICATION = {'class': 'SimpleStrategy', 'replication_factor': 1 } "))
```

#### <a name="validate-in-cqlsh"></a>Weryfikuj w cqlsh

Uruchom następujące polecenie w cqlsh i zobaczysz wcześniej utworzoną przestrzeń kluczy.

```bash
DESCRIBE keyspaces;
```

### <a name="drop-a-keyspace"></a>Porzuć miejsce na dysku

```scala
val cdbConnector = CassandraConnector(sc)
cdbConnector.withSessionDo(session => session.execute("DROP KEYSPACE books_ks"))
```

#### <a name="validate-in-cqlsh"></a>Weryfikuj w cqlsh

```bash
DESCRIBE keyspaces;
```
## <a name="table-ddl-operations"></a>Operacje na tabelach w języku DDL

**Zagadnienia dotyczące**  

- Przepływność można przypisywać na poziomie tabeli przy użyciu instrukcji CREATE TABLE.  
- Jeden klucz partycji może przechowywać 20 GB danych.  
- Jeden rekord może przechowywać maksymalnie 2 MB danych.  
- Jeden zakres kluczy partycji może przechowywać wiele kluczy partycji.

### <a name="create-a-table"></a>Tworzenie tabeli

```scala
val cdbConnector = CassandraConnector(sc)
cdbConnector.withSessionDo(session => session.execute("CREATE TABLE IF NOT EXISTS books_ks.books(book_id TEXT PRIMARY KEY,book_author TEXT, book_name TEXT,book_pub_year INT,book_price FLOAT) WITH cosmosdb_provisioned_throughput=4000 , WITH default_time_to_live=630720000;"))
```

#### <a name="validate-in-cqlsh"></a>Weryfikuj w cqlsh

Uruchom następujące polecenie w cqlsh i Wyświetl tabelę o nazwie "Books: 

```bash
USE books_ks;
DESCRIBE books;
```

W danych wyjściowych poprzedniego polecenia nie są wyświetlane wartości przepływności aprowizacji i domyślne ustawienia czasu wygaśnięcia, ale te wartości można uzyskać z portalu.

### <a name="alter-table"></a>ALTER TABLE

Poniższe wartości można zmienić za pomocą polecenia ALTER TABLE:

* Elastyczna przepływność 
* wartość czasu wygaśnięcia
<br>Zmiany w kolumnach nie są obecnie obsługiwane.

```scala
val cdbConnector = CassandraConnector(sc)
cdbConnector.withSessionDo(session => session.execute("ALTER TABLE books_ks.books WITH cosmosdb_provisioned_throughput=8000, WITH default_time_to_live=0;"))
```

### <a name="drop-table"></a>Usuń tabelę

```scala
val cdbConnector = CassandraConnector(sc)
cdbConnector.withSessionDo(session => session.execute("DROP TABLE IF EXISTS books_ks.books;"))
```

#### <a name="validate-in-cqlsh"></a>Weryfikuj w cqlsh

Uruchom następujące polecenie w cqlsh i sprawdź, czy tabela "książki" nie jest już dostępna:

```bash
USE books_ks;
DESCRIBE tables;
```

## <a name="next-steps"></a>Następne kroki

Po utworzeniu przestrzeni kluczy i tabeli należy wykonać czynności opisane w następujących artykułach CRUD i nie tylko:
 
* [Operacje tworzenia/wstawiania](cassandra-spark-create-ops.md)  
* [Operacje odczytu](cassandra-spark-read-ops.md)  
* [Operacje upsert](cassandra-spark-upsert-ops.md)  
* [Operacje usuwania](cassandra-spark-delete-ops.md)  
* [Operacje agregacji](cassandra-spark-aggregation-ops.md)  
* [Operacje kopiowania tabel](cassandra-spark-table-copy-ops.md)  
