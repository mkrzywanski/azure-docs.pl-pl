---
title: Dostęp do plików w magazynie w usłudze SQL na żądanie (wersja zapoznawcza)
description: Opisuje wykonywanie zapytań dotyczących plików magazynu za pomocą zasobów SQL na żądanie (wersja zapoznawcza) w programie Synapse SQL.
services: synapse-analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: sql
ms.date: 04/19/2020
ms.author: v-stazar
ms.reviewer: jrasnick, carlrab
ms.openlocfilehash: 3c33e2152fc120d406886d89adda26603126a8ba
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/31/2020
ms.locfileid: "87483556"
---
# <a name="access-external-storage-in-synapse-sql-on-demand"></a>Dostęp do magazynu zewnętrznego w programie Synapse SQL (na żądanie)

W tym dokumencie opisano, jak użytkownik może odczytywać dane z plików przechowywanych w usłudze Azure Storage w programie Synapse SQL (na żądanie). Użytkownicy mają następujące opcje dostępu do magazynu:

- Funkcja [OPENROWSET](develop-openrowset.md) , która umożliwia wykonywanie zapytań ad hoc w plikach w usłudze Azure Storage.
- [Zewnętrzna tabela](develop-tables-external-tables.md) , która jest wstępnie zdefiniowaną strukturą danych utworzoną na podstawie zestawu plików zewnętrznych.

Użytkownik może używać [różnych metod uwierzytelniania](develop-storage-files-storage-access-control.md) , takich jak uwierzytelnianie w usłudze Azure AD Passthrough (domyślnie dla podmiotów zabezpieczeń usługi Azure AD) i uwierzytelniania SAS (domyślnie dla podmiotów zabezpieczeń SQL).

## <a name="openrowset"></a>OPENROWSET

Funkcja [OPENROWSET](develop-openrowset.md) umożliwia użytkownikowi odczytywanie plików z usługi Azure Storage.

### <a name="query-files-using-openrowset"></a>Wykonywanie zapytań dotyczących plików przy użyciu funkcji OPENROWSET

Funkcja OPENROWSET umożliwia użytkownikom wysyłanie zapytań do zewnętrznych plików w usłudze Azure Storage, jeśli mają dostęp do magazynu. Aby można było odczytać zawartość plików w usłudze Azure Storage, użytkownik, który jest połączony z punktem końcowym usługi Synapse SQL na żądanie, powinien użyć następującego zapytania:

```sql
SELECT * FROM
 OPENROWSET(BULK 'https://<storage_account>.dfs.core.windows.net/<container>/<path>/*.parquet', format= 'parquet') as rows
```

Użytkownik może uzyskać dostęp do magazynu przy użyciu następujących reguł dostępu:

- Użytkownik usługi Azure AD — funkcja OPENROWSET będzie używać tożsamości elementu wywołującego usługi Azure AD do uzyskiwania dostępu do usługi Azure Storage lub dostępu do magazynu z dostępem anonimowym.
- Użytkownik SQL — funkcja OPENROWSET będzie uzyskiwać dostęp do magazynu z dostępem anonimowym.

Podmioty zabezpieczeń SQL mogą również używać funkcji OPENROWSET do bezpośredniego zapytania o pliki chronione za pomocą tokenów SAS lub zarządzanej tożsamości obszaru roboczego. Jeśli użytkownik SQL wykona tę funkcję, użytkownik zaawansowany z `ALTER ANY CREDENTIAL` uprawnieniami musi utworzyć poświadczenia z zakresem serwera, które pasują do adresu URL w funkcji (przy użyciu nazwy magazynu i kontenera), a uprawnienia udzielane ODniesień dla tego poświadczenia do obiektu wywołującego funkcji OPENROWSET:

```sql
EXECUTE AS somepoweruser

CREATE CREDENTIAL [https://<storage_account>.dfs.core.windows.net/<container>]
 WITH IDENTITY = 'SHARED ACCESS SIGNATURE', SECRET = 'sas token';

GRANT REFERENCES CREDENTIAL::[https://<storage_account>.dfs.core.windows.net/<container>] TO sqluser
```

Jeśli nie ma poświadczeń na poziomie serwera pasujących do adresu URL lub użytkownik SQL nie ma uprawnień odwołuje się do tego poświadczenia, zostanie zwrócony błąd. Podmioty zabezpieczeń SQL nie mogą personifikować się przy użyciu tożsamości usługi Azure AD.

> [!NOTE]
> Ta wersja usługi OPENROWSET została zaprojektowana z myślą o szybkiej i łatwej eksploracji danych przy użyciu domyślnego uwierzytelniania. Aby wykorzystać personifikację lub tożsamość zarządzaną, użyj funkcji OPENROWSET ze źródłem danych opisanym w następnej sekcji.

### <a name="query-data-sources-using-openrowset"></a>Tworzenie zapytań o źródła danych przy użyciu funkcji OPENROWSET

Funkcja OPENROWSET umożliwia użytkownikowi wykonywanie zapytań dotyczących plików umieszczonych w niezależnym zewnętrznym źródle danych:

```sql
SELECT * FROM
 OPENROWSET(BULK 'file/path/*.parquet',
 DATASOURCE = MyAzureInvoices,
 FORMAT= 'parquet') as rows
```

Użytkownik zaawansowany z KONTROLKą bazy danych musi utworzyć poświadczenie o zakresie bazy danych, które będzie używane w celu uzyskania dostępu do magazynu i zewnętrznego źródła danych, które określa adres URL źródła danych i poświadczenia, które powinny być używane:

```sql
CREATE DATABASE SCOPED CREDENTIAL AccessAzureInvoices
 WITH IDENTITY = 'SHARED ACCESS SIGNATURE',
 SECRET = '******srt=sco&amp;sp=rwac&amp;se=2017-02-01T00:55:34Z&amp;st=201********' ;

CREATE EXTERNAL DATA SOURCE MyAzureInvoices
 WITH ( LOCATION = 'https://<storage_account>.dfs.core.windows.net/<container>/<path>/' ,
 CREDENTIAL = AccessAzureInvoices) ;
```

Poświadczenie o zakresie bazy danych określa, jak uzyskać dostęp do plików w źródle danych, do którego się odwołuje (obecnie jest to tożsamość SAS i zarządzana).

Obiekt wywołujący musi mieć jedno z następujących uprawnień, aby wykonać funkcję OPENROWSET:

- Jedno z uprawnień do wykonania funkcji OPENROWSET:
  - `ADMINISTER BULK OPERATIONS`umożliwia logowanie do wykonywania funkcji OPENROWSET.
  - `ADMINISTER DATABASE BULK OPERATIONS`umożliwia użytkownikowi z zakresem bazy danych wykonywanie funkcji OPENROWSET.
- Odwołuje się do poświadczenia w zakresie bazy danych do poświadczeń, do których odwołuje się zewnętrzne źródło danych

#### <a name="access-anonymous-data-sources"></a>Dostęp do anonimowych źródeł danych

Użytkownik może utworzyć zewnętrzne źródło danych bez poświadczeń, które odwołują się do magazynu dostępu publicznego lub uwierzytelniania przy użyciu usługi Azure AD Passthrough:

```sql
CREATE EXTERNAL DATA SOURCE MyAzureInvoices
 WITH ( LOCATION = 'https://<storage_account>.dfs.core.windows.net/<container>/<path>') ;
```

## <a name="external-table"></a>TABELA ZEWNĘTRZNA

Użytkownik z uprawnieniami do odczytu tabeli może uzyskać dostęp do zewnętrznych plików przy użyciu zewnętrznej tabeli utworzonej w oparciu o zestaw plików i folderów usługi Azure Storage.

Użytkownik, który ma [uprawnienia do tworzenia tabeli zewnętrznej](https://docs.microsoft.com/sql/t-sql/statements/create-external-table-transact-sql?view=sql-server-ver15#permissions) (na przykład CREATE TABLE i zmiany poświadczeń lub odwołania do bazy danych), może użyć następującego skryptu, aby utworzyć tabelę na podstawie źródła danych usługi Azure Storage:

```sql
CREATE EXTERNAL TABLE [dbo].[DimProductexternal]
( ProductKey int, ProductLabel nvarchar, ProductName nvarchar )
WITH
(
LOCATION='/DimProduct/year=*/month=*' ,
DATA_SOURCE = AzureDataLakeStore ,
FILE_FORMAT = TextFileFormat
) ;
```

Użytkownik z uprawnieniami do sterowania BAZAmi danych musi utworzyć poświadczenie o zakresie bazy danych, które będzie używane w celu uzyskania dostępu do magazynu i zewnętrznego źródła danych, które określa adres URL źródła danych i poświadczenia, które powinny być używane:

```sql
CREATE DATABASE SCOPED CREDENTIAL cred
 WITH IDENTITY = 'SHARED ACCESS SIGNATURE',
 SECRET = '******srt=sco&sp=rwac&se=2017-02-01T00:55:34Z&st=201********' ;

CREATE EXTERNAL DATA SOURCE AzureDataLakeStore
 WITH ( LOCATION = 'https://<storage_account>.dfs.core.windows.net/<container>/<path>' ,
 CREDENTIAL = cred
 ) ;
```

Poświadczenie o zakresie bazy danych określa, jak uzyskać dostęp do plików w źródle danych, którego dotyczy odwołanie.

### <a name="read-external-files-with-external-table"></a>Odczytaj pliki zewnętrzne z TABELą ZEWNĘTRZną

TABELA zewnętrzna umożliwia odczytywanie danych z plików, do których odwołuje się źródło danych za pomocą standardowej instrukcji SELECT języka SQL:

```sql
SELECT *
FROM dbo.DimProductsExternal
```

Obiekt wywołujący musi mieć następujące uprawnienia do odczytu danych:
- `SELECT`uprawnienie do tabeli zewnętrznej
- `REFERENCES DATABASE SCOPED CREDENTIAL`uprawnienie, jeśli `DATA SOURCE` ma`CREDENTIAL`

## <a name="permissions"></a>Uprawnienia

Poniższa tabela zawiera listę wymaganych uprawnień do operacji wymienionych powyżej.

| Zapytanie | Wymagane uprawnienia|
| --- | --- |
| OPENROWSET (BULK) bez źródła danych | `ADMINISTER BULK OPERATIONS`, `ADMINISTER DATABASE BULK OPERATIONS` lub logowanie SQL musi zawierać poświadczenie odwołania:: \<URL> dla magazynu chronionego przez sygnaturę dostępu współdzielonego |
| OPENROWSET (BULK) ze źródłem danych bez poświadczeń | `ADMINISTER BULK OPERATIONS`lub `ADMINISTER DATABASE BULK OPERATIONS` , |
| OPENROWSET (BULK) z elementem DataSource z poświadczeniem | `REFERENCES DATABASE SCOPED CREDENTIAL`i jeden z `ADMINISTER BULK OPERATIONS` lub`ADMINISTER DATABASE BULK OPERATIONS` |
| UTWÓRZ ZEWNĘTRZNE ŹRÓDŁO DANYCH | `ALTER ANY EXTERNAL DATA SOURCE` i `REFERENCES DATABASE SCOPED CREDENTIAL` |
| TWORZENIE TABELI ZEWNĘTRZNEJ | `CREATE TABLE`, `ALTER ANY SCHEMA` , `ALTER ANY EXTERNAL FILE FORMAT` i`ALTER ANY EXTERNAL DATA SOURCE` |
| WYBIERZ Z TABELI ZEWNĘTRZNEJ | `SELECT TABLE` i `REFERENCES DATABASE SCOPED CREDENTIAL` |
| CETAS | Aby utworzyć tabelę, `CREATE TABLE` , `ALTER ANY SCHEMA` , `ALTER ANY DATA SOURCE` i `ALTER ANY EXTERNAL FILE FORMAT` . Aby odczytywać dane: `ADMINISTER BULK OPERATIONS` lub `REFERENCES CREDENTIAL` lub `SELECT TABLE` dla każdej tabeli/widoku/funkcji w programie Query + R/w pozwoleniu na magazyn |

## <a name="next-steps"></a>Następne kroki

Teraz można przystąpić do dalszej pracy z następującymi artykułami:

- [Wykonywanie zapytań dotyczących danych w magazynie](query-data-storage.md)

- [Wykonywanie zapytań względem pliku CSV](query-single-csv-file.md)

- [Wykonywanie zapytań względem folderów i wielu plików](query-folders-multiple-csv-files.md)

- [Kwerenda określonych plików](query-specific-files.md)

- [Wykonywanie zapytań względem plików Parquet](query-parquet-files.md)

- [Typy zagnieżdżone zapytania](query-parquet-nested-types.md)

- [Wykonywanie zapytań względem plików JSON](query-json-files.md)

- [Tworzenie widoków i korzystanie z nich](create-use-views.md)
