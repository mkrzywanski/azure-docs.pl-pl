---
title: Kopiowanie i Przekształcanie danych w Azure SQL Database
description: Dowiedz się, jak kopiować dane do i z Azure SQL Database oraz przekształcać dane w Azure SQL Database przy użyciu Azure Data Factory.
services: data-factory
ms.author: jingwang
author: linda33wj
manager: shwang
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 08/03/2020
ms.openlocfilehash: 3b5382b1de76532d83f526ba87a0a8239c20db0c
ms.sourcegitcommit: 3d56d25d9cf9d3d42600db3e9364a5730e80fa4a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/03/2020
ms.locfileid: "87530779"
---
# <a name="copy-and-transform-data-in-azure-sql-database-by-using-azure-data-factory"></a>Kopiowanie i Przekształcanie danych w Azure SQL Database przy użyciu Azure Data Factory

> [!div class="op_single_selector" title1="Wybierz używaną wersję Azure Data Factory:"]
>
> - [Wersja 1](v1/data-factory-azure-sql-connector.md)
> - [Bieżąca wersja](connector-azure-sql-database.md)

[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

W tym artykule opisano sposób używania działania kopiowania w Azure Data Factory do kopiowania danych z i do Azure SQL Database oraz do przekształcania danych w Azure SQL Database. Aby dowiedzieć się więcej na temat Azure Data Factory, Przeczytaj [artykuł wprowadzający](introduction.md).

## <a name="supported-capabilities"></a>Obsługiwane możliwości

Ten łącznik Azure SQL Database jest obsługiwany dla następujących działań:

- [Działanie kopiowania](copy-activity-overview.md) z [obsługiwaną tabelą macierzy źródłowej/ujścia](copy-activity-overview.md)
- [Mapowanie przepływu danych](concepts-data-flow-overview.md)
- [Działanie Lookup](control-flow-lookup-activity.md)
- [Działanie GetMetadata](control-flow-get-metadata-activity.md)

W przypadku działania kopiowania ten łącznik Azure SQL Database obsługuje następujące funkcje:

- Kopiowanie danych przy użyciu uwierzytelniania SQL i usługi Azure Active Directory (Azure AD) uwierzytelnianie tokenu aplikacji z jednostką usług lub tożsamościami zarządzanymi dla zasobów platformy Azure.
- Jako źródło, pobieranie danych przy użyciu zapytania SQL lub procedury składowanej.
- Jako ujścia, automatyczne tworzenie tabeli docelowej, jeśli nie istnieje, na podstawie schematu źródłowego; Dołączanie danych do tabeli lub wywoływanie procedury składowanej z logiką niestandardową podczas kopiowania.

>[!NOTE]
> [Always Encrypted](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine?view=azuresqldb-current) Azure SQL Database nie jest teraz obsługiwana przez ten łącznik. Aby obejść ten krok, można użyć [ogólnego łącznika ODBC](connector-odbc.md) i sterownika SQL Server ODBC za pośrednictwem własnego środowiska Integration Runtime. Dowiedz się więcej z sekcji [używanie Always Encrypted](#using-always-encrypted) . 

> [!IMPORTANT]
> W przypadku kopiowania danych przy użyciu środowiska Azure Integration Runtime Skonfiguruj [regułę zapory na poziomie serwera](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) tak, aby usługi platformy Azure mogły uzyskiwać dostęp do serwera.
> W przypadku kopiowania danych przy użyciu własnego środowiska Integration Runtime Skonfiguruj zaporę tak, aby zezwalała na odpowiedni zakres adresów IP. Ten zakres obejmuje adres IP maszyny, który jest używany do nawiązywania połączenia z Azure SQL Database.

## <a name="get-started"></a>Rozpoczęcie pracy

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Poniższe sekcje zawierają szczegółowe informacje o właściwościach, które są używane do definiowania jednostek Azure Data Factory specyficznych dla łącznika Azure SQL Database.

## <a name="linked-service-properties"></a>Właściwości połączonej usługi

Te właściwości są obsługiwane dla Azure SQL Database połączonej usługi:

| Właściwość | Opis | Wymagane |
|:--- |:--- |:--- |
| typ | Właściwość **Type** musi być ustawiona na wartość **AzureSqlDatabase**. | Tak |
| Parametry połączenia | Określ informacje, które są konieczne do nawiązania połączenia z wystąpieniem Azure SQL Database dla właściwości **ConnectionString** . <br/>Możesz również umieścić hasło lub klucz jednostki usługi w Azure Key Vault. Jeśli jest to uwierzytelnianie SQL, należy ściągnąć `password` konfigurację z parametrów połączenia. Aby uzyskać więcej informacji, zobacz przykład JSON po zalogowaniu do tabeli i [przechowywania w Azure Key Vault](store-credentials-in-key-vault.md). | Tak |
| servicePrincipalId | Określ identyfikator klienta aplikacji. | Tak, w przypadku korzystania z uwierzytelniania usługi Azure AD za pomocą nazwy głównej usługi |
| servicePrincipalKey | Określ klucz aplikacji. Oznacz to pole jako element **SecureString** , aby bezpiecznie przechowywać go w Azure Data Factory lub [odwołać się do wpisu tajnego przechowywanego w Azure Key Vault](store-credentials-in-key-vault.md). | Tak, w przypadku korzystania z uwierzytelniania usługi Azure AD za pomocą nazwy głównej usługi |
| dzierżaw | Określ informacje o dzierżawie, takie jak nazwa domeny lub identyfikator dzierżawy, w której znajduje się aplikacja. Pobierz ją przez umieszczenie kursora myszy w prawym górnym rogu Azure Portal. | Tak, w przypadku korzystania z uwierzytelniania usługi Azure AD za pomocą nazwy głównej usługi |
| Właściwością connectvia | To [środowisko Integration Runtime](concepts-integration-runtime.md) służy do nawiązywania połączenia z magazynem danych. Jeśli magazyn danych znajduje się w sieci prywatnej, możesz użyć środowiska Azure Integration Runtime lub własnego środowiska Integration Runtime. Jeśli nie zostanie określony, zostanie użyta domyślna usługa Azure Integration Runtime. | Nie |

W przypadku różnych typów uwierzytelniania zapoznaj się z poniższymi sekcjami dotyczącymi wymagań wstępnych i próbek JSON odpowiednio:

- [Uwierzytelnianie SQL](#sql-authentication)
- [Uwierzytelnianie tokenu aplikacji usługi Azure AD: Nazwa główna usługi](#service-principal-authentication)
- [Uwierzytelnianie tokenu aplikacji usługi Azure AD: zarządzane tożsamości dla zasobów platformy Azure](#managed-identity)

>[!TIP]
>Jeśli wystąpi błąd z kodem błędu "UserErrorFailedToConnectToSqlServer" i komunikatem "limit sesji dla bazy danych to XXX i został osiągnięty," Dodaj `Pooling=false` do parametrów połączenia i spróbuj ponownie.

### <a name="sql-authentication"></a>Uwierzytelnianie SQL

**Przykład: używanie uwierzytelniania SQL**

```json
{
    "name": "AzureSqlDbLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
            "connectionString": "Data Source=tcp:<servername>.database.windows.net,1433;Initial Catalog=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Przykład: hasło w Azure Key Vault**

```json
{
    "name": "AzureSqlDbLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
            "connectionString": "Data Source=tcp:<servername>.database.windows.net,1433;Initial Catalog=<databasename>;User ID=<username>@<servername>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30",
            "password": {
                "type": "AzureKeyVaultSecret",
                "store": {
                    "referenceName": "<Azure Key Vault linked service name>",
                    "type": "LinkedServiceReference"
                },
                "secretName": "<secretName>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="service-principal-authentication"></a>Uwierzytelnianie jednostki usługi

Aby skorzystać z uwierzytelniania tokena aplikacji opartego na jednostce usługi Azure AD, wykonaj następujące kroki:

1. [Utwórz aplikację Azure Active Directoryową](../active-directory/develop/howto-create-service-principal-portal.md#register-an-application-with-azure-ad-and-create-a-service-principal) z Azure Portal. Zanotuj nazwę aplikacji oraz następujące wartości, które definiują połączoną usługę:

    - Identyfikator aplikacji
    - Klucz aplikacji
    - Identyfikator dzierżawy

2. Jeśli jeszcze tego nie zrobiono, [Zapewnij administratorowi Azure Active Directory](../azure-sql/database/authentication-aad-configure.md#provision-azure-ad-admin-sql-database) dla serwera na Azure Portal. Administrator usługi Azure AD musi być użytkownikiem usługi Azure AD lub grupą usługi Azure AD, ale nie może być główną usługą. Ten krok jest wykonywany w celu utworzenia użytkownika zawartej bazy danych dla jednostki usługi przy użyciu tożsamości usługi Azure AD.

3. [Utwórz użytkowników zawartej bazy danych](../azure-sql/database/authentication-aad-configure.md#create-contained-users-mapped-to-azure-ad-identities) dla jednostki usługi. Nawiąż połączenie z bazą danych z lub, do której chcesz skopiować dane przy użyciu narzędzi takich jak SQL Server Management Studio, z tożsamością usługi Azure AD, która ma co najmniej zmienione uprawnienia użytkownika. Uruchom następujące T-SQL:
  
    ```sql
    CREATE USER [your application name] FROM EXTERNAL PROVIDER;
    ```

4. Przyznaj jednostce usługi odpowiednie uprawnienia, które zwykle są przeznaczone dla użytkowników SQL lub innych. Uruchom następujący kod. Aby uzyskać więcej informacji, zobacz [ten dokument](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addrolemember-transact-sql?view=sql-server-2017).

    ```sql
    ALTER ROLE [role name] ADD MEMBER [your application name];
    ```

5. Skonfiguruj połączoną usługę Azure SQL Database w Azure Data Factory.

#### <a name="linked-service-example-that-uses-service-principal-authentication"></a>Przykład połączonej usługi korzystającej z uwierzytelniania jednostki usługi

```json
{
    "name": "AzureSqlDbLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
            "connectionString": "Data Source=tcp:<servername>.database.windows.net,1433;Initial Catalog=<databasename>;Connection Timeout=30",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": {
                "type": "SecureString",
                "value": "<service principal key>"
            },
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="managed-identities-for-azure-resources-authentication"></a><a name="managed-identity"></a>Zarządzane tożsamości na potrzeby uwierzytelniania zasobów platformy Azure

Fabryka danych może być skojarzona z [zarządzaną tożsamością dla zasobów platformy Azure](data-factory-service-identity.md) , które reprezentują określoną fabrykę danych. Tej tożsamości zarządzanej można użyć do uwierzytelniania Azure SQL Database. Wydaną fabrykę mogą uzyskać dostęp do danych z lub do bazy danych lub z niej kopiować przy użyciu tej tożsamości.

Aby korzystać z uwierzytelniania tożsamości zarządzanej, wykonaj następujące kroki.

1. Jeśli jeszcze tego nie zrobiono, [Zapewnij administratorowi Azure Active Directory](../azure-sql/database/authentication-aad-configure.md#provision-azure-ad-admin-sql-database) dla serwera na Azure Portal. Administrator usługi Azure AD może być użytkownikiem usługi Azure AD lub grupą usługi Azure AD. Jeśli grupa ma zarządzaną tożsamość rolę administratora, Pomiń kroki 3 i 4. Administrator ma pełny dostęp do bazy danych programu.

2. [Utwórz użytkowników zawartej bazy danych](../azure-sql/database/authentication-aad-configure.md#create-contained-users-mapped-to-azure-ad-identities) dla Azure Data Factory tożsamości zarządzanej. Nawiąż połączenie z bazą danych z lub, do której chcesz skopiować dane przy użyciu narzędzi takich jak SQL Server Management Studio, z tożsamością usługi Azure AD, która ma co najmniej zmienione uprawnienia użytkownika. Uruchom następujące T-SQL:
  
    ```sql
    CREATE USER [your Data Factory name] FROM EXTERNAL PROVIDER;
    ```

3. Przyznaj Data Factoryj zarządzanej tożsamości, które są potrzebne w zwykły sposób dla użytkowników SQL i innych. Uruchom następujący kod. Aby uzyskać więcej informacji, zobacz [ten dokument](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addrolemember-transact-sql?view=sql-server-2017).

    ```sql
    ALTER ROLE [role name] ADD MEMBER [your Data Factory name];
    ```

4. Skonfiguruj połączoną usługę Azure SQL Database w Azure Data Factory.

**Przykład**

```json
{
    "name": "AzureSqlDbLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "typeProperties": {
            "connectionString": "Data Source=tcp:<servername>.database.windows.net,1433;Initial Catalog=<databasename>;Connection Timeout=30"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Właściwości zestawu danych

Aby uzyskać pełną listę sekcji i właściwości dostępnych do definiowania zestawów danych, zobacz [zestawy danych](https://docs.microsoft.com/azure/data-factory/concepts-datasets-linked-services).

Następujące właściwości są obsługiwane dla Azure SQL Database zestawu danych:

| Właściwość | Opis | Wymagane |
|:--- |:--- |:--- |
| typ | Właściwość **Type** zestawu danych musi być ustawiona na wartość **wartość azuresqltable**. | Tak |
| schema | Nazwa schematu. |Nie dla źródła, tak dla ujścia  |
| table | Nazwa tabeli/widoku. |Nie dla źródła, tak dla ujścia  |
| tableName | Nazwa tabeli/widoku ze schematem. Ta właściwość jest obsługiwana w celu zapewnienia zgodności z poprzednimi wersjami. W przypadku nowych obciążeń Użyj `schema` i `table` . | Nie dla źródła, tak dla ujścia |

### <a name="dataset-properties-example"></a>Przykład właściwości zestawu danych

```json
{
    "name": "AzureSQLDbDataset",
    "properties":
    {
        "type": "AzureSqlTable",
        "linkedServiceName": {
            "referenceName": "<Azure SQL Database linked service name>",
            "type": "LinkedServiceReference"
        },
        "schema": [ < physical schema, optional, retrievable during authoring > ],
        "typeProperties": {
            "schema": "<schema_name>",
            "table": "<table_name>"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Właściwości działania kopiowania

Aby uzyskać pełną listę sekcji i właściwości dostępnych do definiowania działań, zobacz [potoki](concepts-pipelines-activities.md). Ta sekcja zawiera listę właściwości obsługiwanych przez źródło i ujścia Azure SQL Database.

### <a name="azure-sql-database-as-the-source"></a>Azure SQL Database jako źródło

Aby skopiować dane z Azure SQL Database, w sekcji **Źródło** działania kopiowania są obsługiwane następujące właściwości:

| Właściwość | Opis | Wymagane |
|:--- |:--- |:--- |
| typ | Właściwość **Type** źródła działania Copy musi być ustawiona na wartość **AzureSqlSource**. Typ "sqlsource" jest nadal obsługiwany w celu zapewnienia zgodności z poprzednimi wersjami. | Tak |
| sqlReaderQuery | Ta właściwość używa niestandardowego zapytania SQL do odczytywania danych. Może to być na przykład `select * from MyTable`. | Nie |
| sqlReaderStoredProcedureName | Nazwa procedury składowanej, która odczytuje dane z tabeli źródłowej. Ostatnia instrukcja SQL musi być instrukcją SELECT w procedurze składowanej. | Nie |
| storedProcedureParameters | Parametry procedury składowanej.<br/>Dozwolone wartości to pary nazw lub wartości. Nazwy i wielkość liter parametrów muszą być zgodne z nazwami i wielkością liter parametrów procedury składowanej. | Nie |
| isolationLevel | Określa zachowanie blokowania transakcji dla źródła SQL. Dozwolone wartości to: **READCOMMITTED**, **READUNCOMMITTED**, **REPEATABLEREAD**, **Serializable**, **migawka**. Jeśli nie zostanie określony, używany jest domyślny poziom izolacji bazy danych. Aby uzyskać więcej informacji, zapoznaj się z [tym dokumentem](https://docs.microsoft.com/dotnet/api/system.data.isolationlevel) . | Nie |

**Punkty do uwagi:**

- Jeśli **sqlReaderQuery** jest określony dla **AzureSqlSource**, działanie Copy uruchamia to zapytanie względem źródła Azure SQL Database, aby uzyskać dane. Można również określić procedurę składowaną, określając **sqlReaderStoredProcedureName** i **storedProcedureParameters** , jeśli procedura składowana pobiera parametry.
- Jeśli nie określisz opcji **sqlReaderQuery** ani **sqlReaderStoredProcedureName**, kolumny zdefiniowane w sekcji "Structure" w kodzie JSON zestawu danych są używane do konstruowania zapytania. Zapytanie `select column1, column2 from mytable` jest uruchamiane względem Azure SQL Database. Jeśli definicja zestawu danych nie ma "struktury", wszystkie kolumny są wybierane z tabeli.

#### <a name="sql-query-example"></a>Przykład zapytania SQL

```json
"activities":[
    {
        "name": "CopyFromAzureSQLDatabase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Azure SQL Database input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "AzureSqlSource",
                "sqlReaderQuery": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

#### <a name="stored-procedure-example"></a>Przykład procedury składowanej

```json
"activities":[
    {
        "name": "CopyFromAzureSQLDatabase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Azure SQL Database input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "AzureSqlSource",
                "sqlReaderStoredProcedureName": "CopyTestSrcStoredProcedureWithParameters",
                "storedProcedureParameters": {
                    "stringData": { "value": "str3" },
                    "identifier": { "value": "$$Text.Format('{0:yyyy}', <datetime parameter>)", "type": "Int"}
                }
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="stored-procedure-definition"></a>Definicja procedury składowanej

```sql
CREATE PROCEDURE CopyTestSrcStoredProcedureWithParameters
(
    @stringData varchar(20),
    @identifier int
)
AS
SET NOCOUNT ON;
BEGIN
     select *
     from dbo.UnitTestSrcTable
     where dbo.UnitTestSrcTable.stringData != stringData
    and dbo.UnitTestSrcTable.identifier != identifier
END
GO
```

### <a name="azure-sql-database-as-the-sink"></a>Azure SQL Database jako ujścia

> [!TIP]
> Dowiedz się więcej o obsługiwanych zachowaniach zapisu, konfiguracjach i najlepszych rozwiązaniach od [najlepszych rozwiązań dotyczących ładowania danych do Azure SQL Database](#best-practice-for-loading-data-into-azure-sql-database).

Aby skopiować dane do Azure SQL Database, w sekcji **ujścia** działania kopiowania są obsługiwane następujące właściwości:

| Właściwość | Opis | Wymagane |
|:--- |:--- |:--- |
| typ | Właściwość **Type** ujścia działania Copy musi być ustawiona na wartość **AzureSqlSink**. Typ "sqlsink" jest nadal obsługiwany w celu zapewnienia zgodności z poprzednimi wersjami. | Tak |
| preCopyScript | Określ zapytanie SQL dla działania kopiowania, które ma zostać uruchomione przed zapisaniem danych w Azure SQL Database. Jest on wywoływany tylko raz dla każdego przebiegu kopiowania. Ta właściwość służy do czyszczenia wstępnie załadowanych danych. | Nie |
| tableOption | Określa, czy [tabela ujścia ma być automatycznie tworzona,](copy-activity-overview.md#auto-create-sink-tables) Jeśli nie istnieje na podstawie schematu źródłowego. <br>Funkcja autotworzenia tabeli nie jest obsługiwana, gdy obiekt ujścia określa procedurę przechowywaną. <br>Dozwolone wartości to: `none` (domyślnie), `autoCreate` . | Nie |
| sqlWriterStoredProcedureName | Nazwa procedury składowanej, która definiuje sposób zastosowania danych źródłowych do tabeli docelowej. <br/>Ta procedura składowana jest *wywoływana na partię*. W przypadku operacji, które są uruchamiane tylko raz i nie mają niczego do wykonania z danymi źródłowymi, na przykład Usuń lub Obetnij, użyj `preCopyScript` właściwości.<br>Zobacz przykład od [wywołania procedury składowanej z ujścia bazy danych SQL](#invoke-a-stored-procedure-from-a-sql-sink). | Nie |
| storedProcedureTableTypeParameterName |Nazwa parametru typu tabeli określona w procedurze składowanej.  |Nie |
| sqlWriterTableType |Nazwa typu tabeli, która ma zostać użyta w procedurze składowanej. Działanie kopiowania sprawia, że dane są dostępne w tabeli tymczasowej z tym typem tabeli. Kod procedury składowanej może następnie scalić dane, które są kopiowane z istniejącymi danymi. |Nie |
| storedProcedureParameters |Parametry procedury składowanej.<br/>Dozwolone wartości to pary nazw i wartości. Nazwy i wielkość liter parametrów muszą być zgodne z nazwami i wielkością liter parametrów procedury składowanej. | Nie |
| writeBatchSize | Liczba wierszy do wstawienia do tabeli SQL *na partię*.<br/> Dozwolona wartość to liczba **całkowita** (liczba wierszy). Domyślnie Azure Data Factory dynamicznie określa odpowiedni rozmiar wsadu na podstawie rozmiaru wiersza. | Nie |
| writeBatchTimeout | Czas oczekiwania na zakończenie operacji wstawiania wsadowego przed przekroczeniem limitu czasu.<br/> Dozwolona wartość to **TimeSpan**. Przykładem jest "00:30:00" (30 minut). | Nie |
| disableMetricsCollection | Data Factory zbiera metryki, takie jak Azure SQL Database DTU na potrzeby optymalizacji wydajności kopiowania i zaleceń. W przypadku tego zachowania należy określić, `true` aby je wyłączyć. | Nie (domyślnie `false` ) |

**Przykład 1: Dołączanie danych**

```json
"activities":[
    {
        "name": "CopyToAzureSQLDatabase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure SQL Database output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "AzureSqlSink",
                "tableOption": "autoCreate",
                "writeBatchSize": 100000
            }
        }
    }
]
```

**Przykład 2: wywoływanie procedury składowanej podczas kopiowania**

Dowiedz się więcej szczegółowo od [wywołania procedury składowanej z ujścia bazy danych SQL](#invoke-a-stored-procedure-from-a-sql-sink).

```json
"activities":[
    {
        "name": "CopyToAzureSQLDatabase",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure SQL Database output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "AzureSqlSink",
                "sqlWriterStoredProcedureName": "CopyTestStoredProcedureWithParameters",
                "storedProcedureTableTypeParameterName": "MyTable",
                "sqlWriterTableType": "MyTableType",
                "storedProcedureParameters": {
                    "identifier": { "value": "1", "type": "Int" },
                    "stringData": { "value": "str1" }
                }
            }
        }
    }
]
```

## <a name="best-practice-for-loading-data-into-azure-sql-database"></a>Najlepsze rozwiązanie w zakresie ładowania danych do Azure SQL Database

Podczas kopiowania danych do Azure SQL Database może być wymagane inne zachowanie zapisu:

- [Dołącz](#append-data): moje dane źródłowe mają tylko nowe rekordy.
- [Upsert](#upsert-data): moje dane źródłowe są wstawiane i aktualizowane.
- [Zastąp](#overwrite-the-entire-table): chcę ponownie załadować całą tabelę wymiarów za każdym razem.
- [Zapisz z logiką niestandardową](#write-data-with-custom-logic): Potrzebuję dodatkowego przetwarzania przed ostatnim wstawieniem do tabeli docelowej.

Zapoznaj się z odpowiednimi sekcjami dotyczącymi konfigurowania programu w Azure Data Factory i najlepszych rozwiązaniach.

### <a name="append-data"></a>Dołącz dane

Dołączanie danych jest domyślnym zachowaniem tego łącznika Azure SQL Database sink. Azure Data Factory wykonuje zbiorcze Wstawianie w celu wydajnego zapisu w tabeli. Można odpowiednio skonfigurować źródło i ujścia w działaniu kopiowania.

### <a name="upsert-data"></a>Wykonywanie operacji upsert dla danych

**Opcja 1:** W przypadku dużej ilości danych do skopiowania można zbiorczo ładować wszystkie rekordy do tabeli przemieszczania za pomocą działania kopiowania, a następnie uruchamiać działanie procedury składowanej w celu zastosowania instrukcji [merge](https://docs.microsoft.com/sql/t-sql/statements/merge-transact-sql?view=azuresqldb-current) lub Insert/Update w jednym zrzucie. 

Działania kopiowania obecnie nie obsługują natywnie ładowania danych do tabeli tymczasowej bazy danych. Istnieje zaawansowany sposób skonfigurowania go przy użyciu kombinacji wielu działań. Zapoznaj się z tematem [optymalizacja Azure SQL Database zbiorczych scenariuszy upsert](https://github.com/scoriani/azuresqlbulkupsert). Poniżej przedstawiono przykład użycia trwałej tabeli jako przejściowej.

Przykładowo w Azure Data Factory można utworzyć potok z **działaniem kopiowania** łańcucha z **działaniem procedury składowanej**. Dawniej kopiuje dane z magazynu źródłowego do Azure SQL Database tabeli tymczasowej, na przykład **UpsertStagingTable**, jako nazwę tabeli w zestawie danych. Następnie drugi wywołuje procedurę przechowywaną w celu scalenia danych źródłowych z tabeli przemieszczania do tabeli docelowej i wyczyszczenia tabeli przemieszczania.

![Upsert](./media/connector-azure-sql-database/azure-sql-database-upsert.png)

W bazie danych Zdefiniuj procedurę składowaną z logiką scalania, taką jak Poniższy przykład, który jest wskazywany przez poprzednią aktywność procedury składowanej. Załóżmy, że element docelowy jest tabelą **marketingową** z trzema kolumnami: **ProfileID**, **State**i **Category**. Wykonaj upsert na podstawie kolumny **ProfileID** .

```sql
CREATE PROCEDURE [dbo].[spMergeData]
AS
BEGIN
   MERGE TargetTable AS target
   USING UpsertStagingTable AS source
   ON (target.[ProfileID] = source.[ProfileID])
   WHEN MATCHED THEN
      UPDATE SET State = source.State
    WHEN NOT matched THEN
       INSERT ([ProfileID], [State], [Category])
      VALUES (source.ProfileID, source.State, source.Category);
    TRUNCATE TABLE UpsertStagingTable
END
```

**Opcja 2:** Można wybrać opcję [wywołania procedury składowanej w ramach działania kopiowania](#invoke-a-stored-procedure-from-a-sql-sink). Takie podejście uruchamia każdą partię (zgodnie z `writeBatchSize` właściwością) w tabeli źródłowej, zamiast używać instrukcji BULK INSERT jako podejścia domyślnego w działaniu kopiowania.

**Opcja 3:** Możesz użyć [przepływu danych mapowania](#sink-transformation) , który oferuje wbudowane metody INSERT/upsert/Update.

### <a name="overwrite-the-entire-table"></a>Zastąp całą tabelę

Właściwość **preCopyScript** można skonfigurować w ujścia działania kopiowania. W tym przypadku dla każdego działania kopiowania, które działa, Azure Data Factory uruchamia najpierw skrypt. Następnie uruchamia kopię, aby wstawić dane. Na przykład aby zastąpić całą tabelę najnowszymi danymi, należy określić skrypt, aby najpierw usunąć wszystkie rekordy przed zbiorczym załadowaniem nowych danych ze źródła.

### <a name="write-data-with-custom-logic"></a>Zapisz dane za pomocą logiki niestandardowej

Kroki zapisu danych za pomocą logiki niestandardowej są podobne do tych opisanych w sekcji [dane upsert](#upsert-data) . Jeśli trzeba zastosować dodatkowe przetwarzanie przed ostatnim wstawieniem danych źródłowych do tabeli docelowej, można załadować do tabeli przemieszczania, a następnie wywołać działanie procedury składowanej lub wywołać procedurę składowaną w ujścia działania kopiowania, aby zastosować dane, lub użyć przepływu danych mapowania.

## <a name="invoke-a-stored-procedure-from-a-sql-sink"></a><a name="invoke-a-stored-procedure-from-a-sql-sink"></a>Wywoływanie procedury składowanej z ujścia SQL

Podczas kopiowania danych do Azure SQL Database można także skonfigurować i wywołać procedurę składowaną określoną przez użytkownika z dodatkowymi parametrami każdej partii tabeli źródłowej. Funkcja procedury składowanej wykorzystuje parametry z [wartościami przechowywanymi w tabeli](https://msdn.microsoft.com/library/bb675163.aspx).

Można użyć procedury składowanej, gdy wbudowane mechanizmy kopiowania nie służą do tego celu. Przykładem jest to, że chcesz zastosować dodatkowe przetwarzanie przed ostatnim wstawieniem danych źródłowych do tabeli docelowej. Niektóre dodatkowe przykłady przetwarzania są potrzebne do scalania kolumn, wyszukiwania dodatkowych wartości i wstawiania do więcej niż jednej tabeli.

Poniższy przykład przedstawia sposób użycia procedury składowanej do wykonania upsert w tabeli w Azure SQL Database. Załóżmy, że dane wejściowe i tabela **marketingu** ujścia mają trzy kolumny: **ProfileID**, **stan**i **Kategoria**. Zrób upsert na podstawie kolumny **ProfileID** i Zastosuj ją tylko dla określonej kategorii o nazwie "Product".

1. W swojej bazie danych Zdefiniuj typ tabeli o tej samej nazwie co **sqlWriterTableType**. Schemat typu tabeli jest taki sam jak schemat zwrócony przez dane wejściowe.

    ```sql
    CREATE TYPE [dbo].[MarketingType] AS TABLE(
        [ProfileID] [varchar](256) NOT NULL,
        [State] [varchar](256) NOT NULL,
        [Category] [varchar](256) NOT NULL
    )
    ```

2. W bazie danych Zdefiniuj procedurę składowaną o takiej samej nazwie jak **sqlWriterStoredProcedureName**. Obsługuje dane wejściowe z określonego źródła i scala do tabeli danych wyjściowych. Nazwa parametru typu tabeli w procedurze składowanej jest taka sama jak **tabelaname** zdefiniowana w zestawie danych.

    ```sql
    CREATE PROCEDURE spOverwriteMarketing @Marketing [dbo].[MarketingType] READONLY, @category varchar(256)
    AS
    BEGIN
    MERGE [dbo].[Marketing] AS target
    USING @Marketing AS source
    ON (target.ProfileID = source.ProfileID and target.Category = @category)
    WHEN MATCHED THEN
        UPDATE SET State = source.State
    WHEN NOT MATCHED THEN
        INSERT (ProfileID, State, Category)
        VALUES (source.ProfileID, source.State, source.Category);
    END
    ```

3. W Azure Data Factory Zdefiniuj sekcję **ujścia SQL** w działaniu kopiowania w następujący sposób:

    ```json
    "sink": {
        "type": "AzureSqlSink",
        "sqlWriterStoredProcedureName": "spOverwriteMarketing",
        "storedProcedureTableTypeParameterName": "Marketing",
        "sqlWriterTableType": "MarketingType",
        "storedProcedureParameters": {
            "category": {
                "value": "ProductA"
            }
        }
    }
    ```

## <a name="mapping-data-flow-properties"></a>Mapowanie właściwości przepływu danych

Podczas przekształcania danych w mapowaniu przepływu danych można odczytywać i zapisywać w tabelach z Azure SQL Database. Aby uzyskać więcej informacji, zobacz [przekształcenie źródłowe](data-flow-source.md) i [przekształcanie ujścia](data-flow-sink.md) w mapowaniu przepływów danych.

### <a name="source-transformation"></a>Transformacja źródła

Ustawienia specyficzne dla Azure SQL Database są dostępne na karcie **Opcje źródła** transformacji źródłowej.

**Dane wejściowe:** Wybierz, czy chcesz wskazać źródło w tabeli (równoważnej ```Select * from <table-name>``` ), czy wprowadzić niestandardowe zapytanie SQL.

**Zapytanie**: w przypadku wybrania zapytania w polu wejściowym wprowadź zapytanie SQL dla źródła. To ustawienie przesłania każdą tabelę, która została wybrana w zestawie danych. Klauzule **order by** nie są obsługiwane w tym miejscu, ale można ustawić pełną instrukcję SELECT FROM. Można również użyć funkcji tabeli zdefiniowanej przez użytkownika. **SELECT * FROM udfGetData ()** to format UDF w języku SQL, który zwraca tabelę. To zapytanie spowoduje utworzenie tabeli źródłowej, której można użyć w przepływie danych. Używanie zapytań jest również doskonałym sposobem zredukowania liczby wierszy do testowania lub wyszukiwania.

- Przykład SQL:```Select * from MyTable where customerId > 1000 and customerId < 2000```

**Rozmiar wsadu**: wprowadź rozmiar partii, aby podzielić duże ilości danych na odczyt.

**Poziom izolacji**: wartość domyślna dla źródeł SQL w mapowaniu przepływu danych jest odczytana. Poziom izolacji można zmienić tutaj na jedną z następujących wartości:

- Odczytaj zatwierdzone
- Odczytaj niezatwierdzone
- Odczyt powtarzalny
- Serializable
- Brak (Ignoruj poziom izolacji)

![Poziom izolacji](media/data-flow/isolationlevel.png "Poziom izolacji")

### <a name="sink-transformation"></a>Przekształcanie ujścia

Ustawienia specyficzne dla Azure SQL Database są dostępne na karcie **Ustawienia** transformacji ujścia.

**Metoda aktualizacji:** Określa, jakie operacje są dozwolone w miejscu docelowym bazy danych. Domyślnie zezwala na operacje wstawiania. Aby zaktualizować, upsert lub usunąć wiersze, przekształcenie ALTER Row jest wymagane do tagowania wierszy dla tych działań. W przypadku aktualizacji, upserts i usunięć należy ustawić kolumnę klucza lub kolumny, aby określić, który wiersz ma być zmieniany.

![Kolumny klucza](media/data-flow/keycolumn.png "Kolumny klucza")

Nazwa kolumny, która jest wybierana jako klucz, będzie używana przez ADF w ramach kolejnej aktualizacji, Upsert, Usuń. W związku z tym należy wybrać kolumnę, która istnieje w mapowaniu ujścia. Jeśli chcesz, aby nie zapisywać wartości w tej kolumnie klucza, kliknij przycisk "Pomiń zapisywanie kolumn kluczy".

**Akcja tabeli:** Określa, czy należy ponownie utworzyć lub usunąć wszystkie wiersze z tabeli docelowej przed zapisem.

- Brak: w tabeli nie zostanie wykonana żadna akcja.
- Utwórz ponownie: tabela zostanie porzucona i utworzona ponownie. Wymagane w przypadku dynamicznego tworzenia nowej tabeli.
- Obcinanie: wszystkie wiersze z tabeli docelowej zostaną usunięte.

**Rozmiar wsadu**: określa, ile wierszy jest zapisywanych w każdym przedziale. Większe rozmiary partii zwiększają optymalizację kompresji i pamięci, ale grozi wyjątkami dotyczącymi pamięci podczas buforowania danych.

**Wstępne i gotowe skrypty SQL**: wprowadź wielowierszowe skrypty SQL, które zostaną wykonane przed (przetwarzanie wstępne) i po wykonaniu (po przetworzeniu) dane są zapisywane w bazie danych ujścia

![wstępne i końcowe skrypty przetwarzania SQL](media/data-flow/prepost1.png "Skrypty przetwarzania SQL")

## <a name="data-type-mapping-for-azure-sql-database"></a>Mapowanie typu danych dla Azure SQL Database

Gdy dane są kopiowane z lub do Azure SQL Database, następujące mapowania są używane z Azure SQL Database typów danych do Azure Data Factory pośrednich typów danych. Aby dowiedzieć się, jak działanie kopiowania mapuje schemat źródłowy i typ danych na ujścia, zobacz [Mapowanie schematu i typu danych](copy-activity-schema-and-type-mapping.md).

| Typ danych Azure SQL Database | Azure Data Factory typ danych pośrednich |
|:--- |:--- |
| bigint |Int64 |
| binarny |Byte [] |
| bit |Wartość logiczna |
| char |String, Char [] |
| date |DateTime |
| Datetime (data/godzina) |DateTime |
| datetime2 |DateTime |
| DateTimeOffset |DateTimeOffset |
| Wartość dziesiętna |Wartość dziesiętna |
| FILESTREAM — atrybut (varbinary (max)) |Byte [] |
| Float |Double |
| image (obraz) |Byte [] |
| int |Int32 |
| pieniędzy |Wartość dziesiętna |
| nchar |String, Char [] |
| ntext |String, Char [] |
| numeryczne |Wartość dziesiętna |
| nvarchar |String, Char [] |
| liczba rzeczywista |Pojedyncze |
| rowversion |Byte [] |
| smalldatetime |DateTime |
| smallint |Int16 |
| smallmoney |Wartość dziesiętna |
| sql_variant |Obiekt |
| tekst |String, Char [] |
| time |przedział_czasu |
| sygnatura czasowa |Byte [] |
| tinyint |Byte |
| uniqueidentifier |Guid (identyfikator GUID) |
| varbinary |Byte [] |
| varchar |String, Char [] |
| xml |Xml |

>[!NOTE]
> W przypadku typów danych, które są mapowane na typ pośredni dziesiętnego, obecnie działanie kopiowania obsługuje dokładność do 28. Jeśli masz dane o dokładności większej niż 28, Rozważ przekonwertowanie na ciąg w kwerendzie SQL.

## <a name="lookup-activity-properties"></a>Właściwości działania Lookup

Aby dowiedzieć się więcej o właściwościach, sprawdź [działanie Lookup (wyszukiwanie](control-flow-lookup-activity.md)).

## <a name="getmetadata-activity-properties"></a>Właściwości działania GetMetadata

Aby uzyskać szczegółowe informacje na temat właściwości, sprawdź [działanie GetMetadata](control-flow-get-metadata-activity.md)

## <a name="using-always-encrypted"></a>Używanie Always Encrypted

Podczas kopiowania danych z/do Azure SQL Database z [Always Encrypted](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-database-engine?view=azuresqldb-current), użyj [ogólnego łącznika ODBC](connector-odbc.md) i SQL Server sterownika ODBC za pośrednictwem samoobsługowego Integration Runtime. Ten łącznik Azure SQL Database nie obsługuje Always Encrypted teraz. 

Więcej szczegółów:

1. Skonfiguruj samodzielny Integration Runtime, jeśli go nie masz. Aby uzyskać szczegółowe informacje, zobacz artykuł [Integration Runtime samodzielny](create-self-hosted-integration-runtime.md) .

2. Pobierz sterownik 64-bitowy ODBC dla SQL Server z tego [miejsca](https://docs.microsoft.com/sql/connect/odbc/download-odbc-driver-for-sql-server?view=azuresqldb-current)i zainstaluj go na maszynie Integration Runtime. Dowiedz się więcej o tym, w jaki sposób ten sterownik działa z [wykorzystaniem Always Encrypted ze sterownikiem ODBC dla SQL Server](https://docs.microsoft.com/sql/connect/odbc/using-always-encrypted-with-the-odbc-driver?view=azuresqldb-current#using-the-azure-key-vault-provider).

3. Utwórz połączoną usługę z typem ODBC, aby połączyć się z bazą danych SQL, zapoznaj się z poniższymi przykładami:

    - Aby używać **uwierzytelniania SQL**: Określ parametry połączenia ODBC poniżej, a następnie wybierz pozycję uwierzytelnianie **podstawowe** , aby ustawić nazwę użytkownika i hasło.

        ```
        Driver={ODBC Driver 17 for SQL Server};Server=<serverName>;Database=<databaseName>;ColumnEncryption=Enabled;KeyStoreAuthentication=KeyVaultClientSecret;KeyStorePrincipalId=<servicePrincipalKey>;KeyStoreSecret=<servicePrincipalKey>
        ```

    - Aby użyć **Data Factory uwierzytelniania tożsamości zarządzanej**: 

        1. Wykonaj te same [wymagania wstępne](#managed-identity) , aby utworzyć użytkownika bazy danych dla zarządzanej tożsamości i udzielić odpowiedniej roli w bazie danych.
        2. W polu połączona usługa Określ parametry połączenia ODBC poniżej, a następnie wybierz opcję uwierzytelnianie **anonimowe** jako parametry połączenia `Authentication=ActiveDirectoryMsi` .

        ```
        Driver={ODBC Driver 17 for SQL Server};Server=<serverName>;Database=<databaseName>;ColumnEncryption=Enabled;KeyStoreAuthentication=KeyVaultClientSecret;KeyStorePrincipalId=<servicePrincipalKey>;KeyStoreSecret=<servicePrincipalKey>; Authentication=ActiveDirectoryMsi;
        ```

4. Utwórz odpowiednio zestaw danych i działanie Copy z typem ODBC. Dowiedz się więcej z artykułu [łącznika ODBC](connector-odbc.md) .

## <a name="next-steps"></a>Następne kroki

Listę magazynów danych obsługiwanych jako źródła i ujścia przez działanie kopiowania w Azure Data Factory można znaleźć w temacie [obsługiwane magazyny i formaty danych](copy-activity-overview.md#supported-data-stores-and-formats).
