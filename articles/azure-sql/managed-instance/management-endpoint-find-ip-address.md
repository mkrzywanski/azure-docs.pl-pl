---
title: Odnajdź adres IP punktu końcowego zarządzania
titleSuffix: Azure SQL Managed Instance
description: Dowiedz się, jak uzyskać publiczny adres IP punktu zarządzania wystąpienia zarządzanego usługi Azure SQL i zweryfikować jego wbudowaną ochronę zapory
services: sql-database
ms.service: sql-managed-instance
ms.subservice: operations
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein, carlrab
ms.date: 12/04/2018
ms.openlocfilehash: 40a44fe46cf38c633380c4c353960cc4e11f2f3d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "84708726"
---
# <a name="determine-the-management-endpoint-ip-address---azure-sql-managed-instance"></a>Określanie adresu IP punktu końcowego zarządzania — wystąpienie zarządzane Azure SQL 
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

Klaster wirtualny wystąpienia zarządzanego Azure SQL zawiera punkt końcowy zarządzania wykorzystywany przez platformę Azure do operacji zarządzania. Punkt końcowy zarządzania jest chroniony za pomocą wbudowanej zapory na poziomie sieci i weryfikacji certyfikatu wzajemnego na poziomie aplikacji. Możesz określić adres IP punktu końcowego zarządzania, ale nie możesz uzyskać dostępu do tego punktu końcowego.

Aby określić adres IP zarządzania, wykonaj [wyszukiwanie DNS](/windows-server/administration/windows-commands/nslookup) na nazwie FQDN wystąpienia zarządzanego SQL: `mi-name.zone_id.database.windows.net` . Spowoduje to zwrócenie wpisu DNS, który jest taki sam `trx.region-a.worker.vnet.database.windows.net` . Następnie można wykonać wyszukiwanie DNS dla tej nazwy FQDN z usuniętym elementem ". VNET". Spowoduje to zwrócenie adresu IP zarządzania. 

Ten kod programu PowerShell wykona wszystkie czynności w przypadku zamiany na \<MI FQDN\> wpis DNS wystąpienia zarządzanego SQL: `mi-name.zone_id.database.windows.net`
  
``` powershell
  $MIFQDN = "<MI FQDN>"
  resolve-dnsname $MIFQDN | select -first 1  | %{ resolve-dnsname $_.NameHost.Replace(".vnet","")}
```

Aby uzyskać więcej informacji na temat zarządzania wystąpieniem i łącznością SQL, zobacz [Architektura łączności wystąpienia zarządzanego Azure SQL](connectivity-architecture-overview.md).
