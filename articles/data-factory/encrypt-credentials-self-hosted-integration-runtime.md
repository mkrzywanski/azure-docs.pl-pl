---
title: Szyfruj poświadczenia w Azure Data Factory
description: Dowiedz się, jak szyfrować i przechowywać poświadczenia dla lokalnych magazynów danych na komputerze przy użyciu własnego środowiska Integration Runtime.
services: data-factory
documentationcenter: ''
author: nabhishek
manager: anandsub
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/15/2018
ms.author: abnarain
ms.openlocfilehash: cd775c5a3bf367600a4537a9409a9bb8f902f588
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "82628984"
---
# <a name="encrypt-credentials-for-on-premises-data-stores-in-azure-data-factory"></a>Szyfruj poświadczenia dla lokalnych magazynów danych w Azure Data Factory

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Poświadczenia dla lokalnych magazynów danych (połączonych usług z informacjami poufnymi) można zaszyfrować i zapisać na komputerze przy użyciu własnego środowiska Integration Runtime. 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Przekaż plik definicji JSON z poświadczeniami do <br/>Polecenie cmdlet [**New-AzDataFactoryV2LinkedServiceEncryptedCredential**](/powershell/module/az.datafactory/New-AzDataFactoryV2LinkedServiceEncryptedCredential) w celu utworzenia pliku wyjściowego JSON z zaszyfrowanymi poświadczeniami. Następnie użyj zaktualizowanej definicji JSON, aby utworzyć połączone usługi.

## <a name="author-sql-server-linked-service"></a>Tworzenie połączonej usługi SQL Server
Utwórz plik JSON o nazwie **SqlServerLinkedService.js** w pliku w folderze o następującej zawartości:  

`<servername>` `<databasename>` `<username>` `<password>` Przed zapisaniem pliku Zastąp wartości,, i wartościami dla SQL Server. I Zastąp ciąg `<integration runtime name>` nazwą Twojego środowiska Integration Runtime. 

```json
{
    "properties": {
        "type": "SqlServer",
        "typeProperties": {
            "connectionString": "Server=<servername>;Database=<databasename>;User ID=<username>;Password=<password>;Timeout=60"
        },
        "connectVia": {
            "type": "integrationRuntimeReference",
            "referenceName": "<integration runtime name>"
        },
        "name": "SqlServerLinkedService"
    }
}
```

## <a name="encrypt-credentials"></a>Szyfrowanie poświadczeń
Aby zaszyfrować poufne dane z ładunku JSON w lokalnym środowisku Integration Runtime, uruchom polecenie **New-AzDataFactoryV2LinkedServiceEncryptedCredential**i przekaż ładunek JSON. To polecenie cmdlet zapewnia szyfrowanie poświadczeń przy użyciu funkcji DPAPI i przechowywanie ich lokalnie na własnym węźle środowiska Integration Runtime. Ładunek wyjściowy zawierający zaszyfrowane odwołanie do poświadczenia może zostać przekierowany do innego pliku JSON (w tym przypadku "encryptedLinkedService.json").

```powershell
New-AzDataFactoryV2LinkedServiceEncryptedCredential -DataFactoryName $dataFactoryName -ResourceGroupName $ResourceGroupName -Name "SqlServerLinkedService" -DefinitionFile ".\SQLServerLinkedService.json" > encryptedSQLServerLinkedService.json
```

## <a name="use-the-json-with-encrypted-credentials"></a>Używanie pliku JSON z zaszyfrowanymi poświadczeniami
Teraz Użyj wyjściowego pliku JSON z poprzedniego polecenia zawierającego zaszyfrowane poświadczenia, aby skonfigurować **SqlServerLinkedService**.

```powershell
Set-AzDataFactoryV2LinkedService -DataFactoryName $dataFactoryName -ResourceGroupName $ResourceGroupName -Name "EncryptedSqlServerLinkedService" -DefinitionFile ".\encryptedSqlServerLinkedService.json" 
```

## <a name="next-steps"></a>Następne kroki
Informacje o kwestiach dotyczących zabezpieczeń dotyczących przenoszenia danych znajdują się w temacie [zagadnienia dotyczące zabezpieczeń przenoszenia danych](data-movement-security-considerations.md).

