---
title: Punkty przywracania zdefiniowane przez użytkownika
description: Jak utworzyć punkt przywracania dla puli SQL.
services: synapse-analytics
author: anumjs
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: sql-dw
ms.date: 07/03/2019
ms.author: anjangsh
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 66a2dad9396e8bf7c8ef49db529f7a5486cc8a8f
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/23/2020
ms.locfileid: "87089211"
---
# <a name="user-defined-restore-points"></a>Punkty przywracania zdefiniowane przez użytkownika

W tym artykule opisano tworzenie nowego punktu przywracania zdefiniowanego przez użytkownika dla puli SQL w usłudze Azure Synapse Analytics przy użyciu programu PowerShell i Azure Portal.

## <a name="create-user-defined-restore-points-through-powershell"></a>Tworzenie punktów przywracania zdefiniowanych przez użytkownika za poorednictwem programu PowerShell

Aby utworzyć punkt przywracania zdefiniowany przez użytkownika, należy użyć polecenia cmdlet [New-AzSqlDatabaseRestorePoint](/powershell/module/az.sql/new-azsqldatabaserestorepoint?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) programu PowerShell.

1. Przed rozpoczęciem upewnij się, że [zainstalowano Azure PowerShell](/powershell/azure/?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json).
2. Otwórz program PowerShell.
3. Połącz się z kontem platformy Azure i Wyświetl listę wszystkich subskrypcji skojarzonych z Twoim kontem.
4. Wybierz subskrypcję zawierającą bazę danych, która ma zostać przywrócona.
5. Utwórz punkt przywracania natychmiastowej kopii magazynu danych.

```Powershell

$SubscriptionName="<YourSubscriptionName>"
$ResourceGroupName="<YourResourceGroupName>"
$ServerName="<YourServerNameWithoutURLSuffixSeeNote>"  # Without database.windows.net
$DatabaseName="<YourDatabaseName>"
$Label = "<YourRestorePointLabel>"

Connect-AzAccount
Get-AzSubscription
Select-AzSubscription -SubscriptionName $SubscriptionName

# Create a restore point of the original database
New-AzSqlDatabaseRestorePoint -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName -RestorePointLabel $Label

```

6. Zapoznaj się z listą wszystkich istniejących punktów przywracania.

```Powershell
# List all restore points
Get-AzSqlDatabaseRestorePoints -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName
```

## <a name="create-user-defined-restore-points-through-the-azure-portal"></a>Utwórz punkty przywracania zdefiniowane przez użytkownika za pomocą Azure Portal

Punkty przywracania zdefiniowane przez użytkownika mogą być również tworzone za poorednictwem Azure Portal.

1. Zaloguj się do konta [Azure Portal](https://portal.azure.com/) .

2. Przejdź do puli SQL, dla której chcesz utworzyć punkt przywracania.

3. Wybierz pozycję **Przegląd** w okienku po lewej stronie, a następnie wybierz pozycję **+ nowy punkt przywracania**. Jeśli przycisk Nowy punkt przywracania nie jest włączony, upewnij się, że Pula SQL nie jest wstrzymana.

    ![Nowy punkt przywracania](./media/sql-data-warehouse-restore-points/creating-restore-point-01.png)

4. Określ nazwę punktu przywracania zdefiniowanego przez użytkownika, a następnie kliknij przycisk **Zastosuj**. Punkty przywracania zdefiniowane przez użytkownika mają domyślny okres przechowywania wynoszący siedem dni.

    ![Nazwa punktu przywracania](./media/sql-data-warehouse-restore-points/creating-restore-point-11.png)

## <a name="next-steps"></a>Następne kroki

- [Przywracanie istniejącej puli SQL](sql-data-warehouse-restore-active-paused-dw.md)
- [Przywracanie usuniętej puli SQL](sql-data-warehouse-restore-deleted-dw.md)
- [Przywracanie z puli SQL geograficznej kopii zapasowej](sql-data-warehouse-restore-from-geo-backup.md)

