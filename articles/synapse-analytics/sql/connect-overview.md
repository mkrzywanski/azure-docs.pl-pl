---
title: Nawiązywanie połączenia z usługą Synapse SQL
description: Połączono z usługą SQL Synapse.
services: synapse-analytics
author: azaricstefan
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: v-stazar
ms.reviewer: jrasnick
ms.openlocfilehash: f09f9a503348efc51fb50c283e7fe856869e0dd5
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/12/2020
ms.locfileid: "83198516"
---
# <a name="connect-to-synapse-sql"></a>Nawiązywanie połączenia z usługą Synapse SQL
Połącz się z funkcją SQL Synapse w usłudze Azure Synapse Analytics.

## <a name="supported-tools-for-sql-on-demand-preview"></a>Obsługiwane narzędzia dla SQL na żądanie (wersja zapoznawcza)

[Azure Data Studio](/sql/azure-data-studio/download-azure-data-studio) jest w pełni obsługiwana począwszy od wersji 1.18.0. Program SSMS jest częściowo obsługiwany począwszy od wersji 18,5, można go używać do nawiązywania połączeń i tylko zapytań.

> [!NOTE]
> Jeśli logowanie za pomocą usługi AAD ma otwarte połączenie przez ponad 1 godzinę w czasie wykonywania zapytania, wszelkie zapytania, które opierają się na usłudze AAD, zakończą się niepowodzeniem. Obejmuje to wysyłanie zapytań do magazynu przy użyciu przekazywania usługi AAD i instrukcji, które współpracują z usługą AAD (na przykład CREATE EXTERNAL PROVIDER). Ma to wpływ na wszystkie narzędzia, które utrzymują otwarte połączenia, takie jak w edytorze zapytań w programie SSMS i ADS. Nie dotyczy narzędzi otwierających nowe połączenia do wykonywania zapytań, takich jak Synapse Studio.

> Aby rozwiązać ten problem, możesz uruchomić ponownie narzędzie SSMS lub połączyć się i rozłączyć w usłudze ADS. 

## <a name="find-your-server-name"></a>Znajdowanie nazwy serwera

Nazwa serwera dla puli SQL w następującym przykładzie to: showdemoweu.sql.azuresynapse.net.
Nazwa serwera dla SQL na żądanie w następującym przykładzie to: showdemoweu-ondemand.sql.azuresynapse.net.

Aby znaleźć w pełni kwalifikowaną nazwę serwera:

1. Przejdź do [Azure Portal](https://portal.azure.com).
2. Kliknij pozycję **Synapse obszary robocze**.
3. Kliknij obszar roboczy, z którym chcesz nawiązać połączenie.
4. Przejdź do omówienia.
5. Znajdź pełną nazwę serwera.

## <a name="sql-pool"></a>**Pula SQL**

![Pełna nazwa serwera](./media/connect-overview/server-connect-example.png)

## <a name="sql-on-demand"></a>**SQL na żądanie**

![Pełna nazwa serwera SQL na żądanie](./media/connect-overview/server-connect-example-sqlod.png)

## <a name="supported-drivers-and-connection-strings"></a>Obsługiwane sterowniki i parametry połączenia
Synapse SQL obsługuje [ADO.NET](https://msdn.microsoft.com/library/e80y5yhx(v=vs.110).aspx), [ODBC](https://msdn.microsoft.com/library/jj730314.aspx), [php](https://msdn.microsoft.com/library/cc296172.aspx?f=255&MSPPError=-2147217396)i [JDBC](https://msdn.microsoft.com/library/mt484311(v=sql.110).aspx). Aby znaleźć najnowszą wersję i dokumentację, kliknij jeden z powyższych sterowników. Aby automatycznie wygenerować parametry połączenia dla sterownika, którego używasz z Azure Portal, kliknij pozycję **Pokaż parametry połączenia bazy danych** w poprzednim przykładzie. Poniżej przedstawiono również przykłady parametrów połączenia dla każdego sterownika.

> [!NOTE]
> Rozważ ustawienie limitu czasu połączenia na wartość 300 sekund, aby połączenie nie zostało zakończone mimo krótkich okresów niedostępności.

### <a name="adonet-connection-string-example"></a>Przykład parametrów połączenia sterownika ADO.NET

```csharp
Server=tcp:{your_server}.sql.azuresynapse.net,1433;Database={your_database};User ID={your_user_name};Password={your_password_here};Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;
```

### <a name="odbc-connection-string-example"></a>Przykład parametrów połączenia sterownika ODBC

```csharp
Driver={SQL Server Native Client 11.0};Server=tcp:{your_server}.sql.azuresynapse.net,1433;Database={your_database};Uid={your_user_name};Pwd={your_password_here};Encrypt=yes;TrustServerCertificate=no;Connection Timeout=30;
```

### <a name="php-connection-string-example"></a>Przykład parametrów połączenia sterownika PHP

```PHP
Server: {your_server}.sql.azuresynapse.net,1433 \r\nSQL Database: {your_database}\r\nUser Name: {your_user_name}\r\n\r\nPHP Data Objects(PDO) Sample Code:\r\n\r\ntry {\r\n   $conn = new PDO ( \"sqlsrv:server = tcp:{your_server}.sql.azuresynapse.net,1433; Database = {your_database}\", \"{your_user_name}\", \"{your_password_here}\");\r\n    $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );\r\n}\r\ncatch ( PDOException $e ) {\r\n   print( \"Error connecting to SQL Server.\" );\r\n   die(print_r($e));\r\n}\r\n\rSQL Server Extension Sample Code:\r\n\r\n$connectionInfo = array(\"UID\" => \"{your_user_name}\", \"pwd\" => \"{your_password_here}\", \"Database\" => \"{your_database}\", \"LoginTimeout\" => 30, \"Encrypt\" => 1, \"TrustServerCertificate\" => 0);\r\n$serverName = \"tcp:{your_server}.sql.azuresynapse.net,1433\";\r\n$conn = sqlsrv_connect($serverName, $connectionInfo);
```

### <a name="jdbc-connection-string-example"></a>Przykład parametrów połączenia sterownika JDBC

```Java
jdbc:sqlserver://yourserver.sql.azuresynapse.net:1433;database=yourdatabase;user={your_user_name};password={your_password_here};encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.sql.azuresynapse.net;loginTimeout=30;
```

## <a name="connection-settings"></a>Ustawienia połączenia
Synapse SQL Standard podczas tworzenia połączeń i obiektów. Tych ustawień nie można zastąpić i obejmują one:

| Ustawienia bazy danych | Wartość |
|:--- |:--- |
| [ANSI_NULLS](/sql/t-sql/statements/set-ansi-nulls-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |ON |
| [QUOTED_IDENTIFIERS](/sql/t-sql/statements/set-quoted-identifier-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |ON |
| [DATEFORMAT](/sql/t-sql/statements/set-dateformat-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |mdy |
| [DATEFIRST](/sql/t-sql/statements/set-datefirst-transact-sql?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json&view=azure-sqldw-latest) |7 |

## <a name="recommendations"></a>Zalecenia

W celu wykonywania zapytań **na żądanie SQL** zalecane są narzędzia [Azure Data Studio](get-started-azure-data-studio.md) i Azure Synapse Studio.

## <a name="next-steps"></a>Następne kroki
Aby nawiązać połączenie i rozpocząć tworzenie zapytań przy użyciu programu Visual Studio, zobacz artykuł [Query with Visual Studio](../sql-data-warehouse/sql-data-warehouse-query-visual-studio.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json) (Wykonywanie zapytań przy użyciu programu Visual Studio). Aby dowiedzieć się więcej na temat opcji uwierzytelniania, zobacz [uwierzytelnianie w programie SQL Synapse](../sql-data-warehouse/sql-data-warehouse-authentication.md?toc=/azure/synapse-analytics/toc.json&bc=/azure/synapse-analytics/breadcrumb/toc.json).