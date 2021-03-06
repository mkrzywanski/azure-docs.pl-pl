---
title: Ocena modeli uczenia maszynowego z PRZEWIDYWANIAmi
description: Dowiedz się, jak wypróbować modele uczenia maszynowego przy użyciu funkcji przewidywania T-SQL w Synapse SQL.
services: synapse-analytics
author: anumjs
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 07/21/2020
ms.author: anjangsh
ms.reviewer: jrasnick
ms.custom: azure-synapse
ms.openlocfilehash: d28f37e6020e3a3165b012548868a3ec798651c2
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/28/2020
ms.locfileid: "87294372"
---
# <a name="score-machine-learning-models-with-predict"></a>Ocena modeli uczenia maszynowego z PRZEWIDYWANIAmi

Synapse SQL zapewnia możliwość oceny modeli uczenia maszynowego przy użyciu znanego języka T-SQL. Dzięki [przewidywaniu](https://docs.microsoft.com/sql/t-sql/queries/predict-transact-sql?view=azure-sqldw-latest)w języku T-SQL można przenieść istniejące modele uczenia maszynowego z danymi historycznymi i przedstawić je w ramach bezpiecznych granic magazynu danych. Funkcja przewidywania przyjmuje model [ONNX (Open neuronowych Network Exchange)](https://onnx.ai/) i danych jako dane wejściowe. Ta funkcja eliminuje etap przechodzenia cennych danych poza magazyn danych na potrzeby oceniania. Celem jest umożliwienie specjalistom ds. danych łatwego wdrażania modeli uczenia maszynowego przy użyciu znanego interfejsu T-SQL, a także bezproblemowe współdziałanie z analitykami danych, którzy pracują z właściwą strukturą dla swoich zadań.

> [!NOTE]
> Ta funkcja nie jest obecnie obsługiwana w programie SQL na żądanie.

Funkcja wymaga, aby model był szkolony poza programem SQL Synapse. Po skompilowaniu modelu załaduj go do magazynu danych i zapoznaj się z składnią przewidywania T-SQL, aby uzyskać wgląd w dane.

![predictoverview](./media/sql-data-warehouse-predict/datawarehouse-overview.png)

## <a name="training-the-model"></a>Uczenie modelu

Synapse SQL oczekuje wstępnie nauczonego modelu. Podczas uczenia modelu uczenia maszynowego, który jest używany do wykonywania prognoz w programie SQL Synapse, należy pamiętać o następujących kwestiach.

- Synapse SQL obsługuje tylko modele formatu ONNX. ONNX to format modelu "open source", który umożliwia wymianę modeli między różnymi platformami w celu zapewnienia współdziałania. Istniejące modele można przekonwertować na format ONNX przy użyciu struktur, które obsługują ją natywnie lub konwertują dostępne pakiety. Na przykład [skryptu sklearn-Onnx](https://github.com/onnx/sklearn-onnx) Package Convert scikit-Poznaj modele do Onnx. [Repozytorium GitHub ONNX](https://github.com/onnx/tutorials#converting-to-onnx-format) zawiera listę obsługiwanych platform i przykładów.

   Jeśli używasz [zautomatyzowanej](https://docs.microsoft.com/azure/machine-learning/concept-automated-ml) tablicy do szkoleń, upewnij się, że parametr *ENABLE_ONNX_COMPATIBLE_MODELS* ma wartość true, aby utworzyć model formatu Onnx. [Zautomatyzowany Machine Learning Notes](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification-bank-marketing-all-features/auto-ml-classification-bank-marketing-all-features.ipynb) zawiera przykład użycia programu AutoML do utworzenia modelu uczenia maszynowego w formacie ONNX.

- Dla danych wejściowych są obsługiwane następujące typy danych:
    - int, bigint, Real, float
    - char, varchar, nvarchar

- Dane oceniania muszą być w tym samym formacie co dane szkoleniowe. Złożone typy danych, takie jak tablice wielowymiarowe, nie są obsługiwane przez PRZEWIDYWANie. Dlatego w przypadku szkolenia upewnij się, że każde wejście modelu odpowiada pojedynczej kolumnie tabeli oceniania zamiast przekazywania pojedynczej tablicy zawierającej wszystkie dane wejściowe.

- Upewnij się, że nazwy i typy danych wejścia modelu są zgodne z nazwami kolumn i typami danych nowych danych prognozowania. Wizualizacja modelu ONNX przy użyciu różnych dostępnych narzędzi "open source" w trybie online może ułatwić debugowanie.

## <a name="loading-the-model"></a>Ładowanie modelu

Model jest przechowywany w tabeli użytkownika programu SQL Synapse jako ciąg szesnastkowy. Dodatkowe kolumny, takie jak ID i Description, można dodać w tabeli model, aby zidentyfikować model. Użyj wartości varbinary (max) jako typu danych kolumny model. Oto przykład kodu dla tabeli, która może być używana do przechowywania modeli:

```sql
-- Sample table schema for storing a model and related data
CREATE TABLE [dbo].[Models]
(
    [Id] [int] IDENTITY(1,1) NOT NULL,
    [Model] [varbinary](max) NULL,
    [Description] [varchar](200) NULL
)
WITH
(
    DISTRIBUTION = ROUND_ROBIN,
    HEAP
)
GO

```

Gdy model zostanie przekonwertowany na ciąg szesnastkowy, a określona definicja tabeli, użyj [polecenia copy](https://docs.microsoft.com/sql/t-sql/statements/copy-into-transact-sql?view=azure-sqldw-latest) lub MyBase, aby załadować model z tabeli SQL Synapse. Poniższy przykład kodu używa polecenia Copy do załadowania modelu.

```sql
-- Copy command to load hexadecimal string of the model from Azure Data Lake storage location
COPY INTO [Models] (Model)
FROM '<enter your storage location>'
WITH (
    FILE_TYPE = 'CSV',
    CREDENTIAL=(IDENTITY= 'Shared Access Signature', SECRET='<enter your storage key here>')
)
```

## <a name="scoring-the-model"></a>Ocenianie modelu

Po załadowaniu modelu i danych w magazynie danych należy użyć funkcji **przewidywania T-SQL** , aby wypróbować model. Upewnij się, że nowe dane wejściowe są w tym samym formacie co dane szkoleniowe używane do kompilowania modelu. PRZEWIDYWANie w języku T-SQL pobiera dwa dane wejściowe: model i nowe danych wejściowych oceniania, a następnie generuje nowe kolumny dla danych wyjściowych. Model może być określony jako zmienna, literał lub sub_query skalarna. Użyj opcji [WITH common_table_expression](https://docs.microsoft.com/sql/t-sql/queries/with-common-table-expression-transact-sql?view=sql-server-ver15) , aby określić nazwany zestaw wyników dla parametru Data.

W poniższym przykładzie pokazano przykładowe zapytanie przy użyciu funkcji przewidywania. Zostanie utworzona dodatkowa kolumna o *wartości Nazwa i* typ danych *zmiennoprzecinkowej* zawierającej wyniki przewidywania. Wszystkie kolumny danych wejściowych, jak również kolumny prognozowania danych wyjściowych, są dostępne do wyświetlania z instrukcją SELECT. Aby uzyskać więcej informacji, zobacz [przewidywania (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/queries/predict-transact-sql?view=azure-sqldw-latest).

```sql
-- Query for ML predictions
SELECT d.*, p.Score
FROM PREDICT(MODEL = (SELECT Model FROM Models WHERE Id = 1),
DATA = dbo.mytable AS d) WITH (Score float) AS p;
```

## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się więcej na temat funkcji przewidywania, zobacz [przewidywania (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/queries/predict-transact-sql?view=azure-sqldw-latest).
