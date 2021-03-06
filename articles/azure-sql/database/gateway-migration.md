---
title: Powiadomienie o migracji ruchu bramy
description: Artykuł zawiera powiadomienie dla użytkowników o migracji adresów IP bramy Azure SQL Database
services: sql-database
ms.service: sql-db-mi
ms.subservice: service
ms.custom: sqldbrb=1 
ms.topic: conceptual
author: rohitnayakmsft
ms.author: rohitna
ms.reviewer: vanto
ms.date: 07/01/2019
ms.openlocfilehash: 22bfab5b9f00a392054fa1aef6a93195180fd968
ms.sourcegitcommit: f353fe5acd9698aa31631f38dd32790d889b4dbb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/29/2020
ms.locfileid: "87373491"
---
# <a name="azure-sql-database-traffic-migration-to-newer-gateways"></a>Azure SQL Database migrację ruchu do nowszych bram
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

Ze względu na to, że infrastruktura platformy Azure jest ulepszona, firma Microsoft okresowo odświeża sprzęt, aby zapewnić najlepszą obsługę klienta. W najbliższych miesiącach planuje Dodawanie bram opartych na nowszych generacjach sprzętu, migrowaniu ruchu do nich i ostatecznie likwidowanie bram opartych na starszym sprzęcie w niektórych regionach.  

Klienci będą powiadamiani za pośrednictwem poczty e-mail i w Azure Portal również z wyprzedzeniem o wszelkich zmianach bram dostępnych w poszczególnych regionach. Najbardziej aktualne informacje będą przechowywane w tabeli [adresy IP bramy Azure SQL Database](connectivity-architecture.md#gateway-ip-addresses) .

## <a name="status-updates"></a>Aktualizacje stanu

# <a name="in-progress"></a>[W toku](#tab/in-progress-ip)
### <a name="september-2020"></a>Wrzesień 2020

Nowe bramy SQL są dodawane do następujących regionów:

- Europa Północna: 13.74.104.113 
- Stany USA 2 zachodni: 40.78.248.10 
- Europa Zachodnia: 52.236.184.163 
- Południowo-środkowe stany USA: 20.45.121.1, 20.49.88.1 

Istniejące bramy SQL będą uruchamiać akceptowanie ruchu w następujących regionach:
- Japonia Wschodnia: 40.79.184.8, 40.79.192.5

Te bramy SQL zaczynają akceptować ruch klienta 1 września 2020. 

### <a name="august-2020"></a>2020 sierpnia

Nowe bramy SQL są dodawane do następujących regionów:

- Australia Wschodnia: 13.70.112.9
- Kanada Środkowa: 52.246.152.0, 20.38.144.1 
- Zachodnie stany USA 2:40.78.240.8

Te bramy SQL zaczynają akceptować ruch klienta w dniu 10 sierpnia 2020. 

# <a name="completed"></a>[Ukończone](#tab/completed-ip)

Następujące migracje bramy zostały ukończone: 

### <a name="october-2019"></a>Październik 2019 r.
- Brazil South
- Zachodnie stany USA
- West Europe
- East US
- Central US
- Azja Południowo-Wschodnia
- South Central US
- Europa Północna
- Północno-środkowe stany USA
- Japonia Zachodnia
- Japan East
- Wschodnie stany USA 2
- Azja Wschodnia

---

## <a name="impact-of-this-change"></a>Wpływ tej zmiany

Migracja ruchu może zmienić publiczny adres IP rozpoznawany przez system DNS dla bazy danych w Azure SQL Database.
Może mieć to wpływ na następujące działania:

- Sztywno zakodowany adres IP dla każdej konkretnej bramy w zaporze lokalnej
- Istnieją podsieci używające programu Microsoft. SQL jako punktu końcowego usługi, ale nie mogą komunikować się z adresami IP bramy
- Używanie [nadmiarowej konfiguracji strefy](high-availability-sla.md#zone-redundant-configuration) dla bazy danych

Nie będzie to miało wpływu na następujące warunki:

- Przekierowanie jako zasada połączenia
- Połączenia do SQL Database z platformy Azure i używanie tagów usługi
- Połączenia utworzone przy użyciu obsługiwanych wersji sterownika JDBC dla SQL Server nie będą miały wpływu. Aby uzyskać obsługiwane wersje JDBC, zobacz artykuł [pobieranie sterownika programu Microsoft JDBC dla SQL Server](/sql/connect/jdbc/download-microsoft-jdbc-driver-for-sql-server).

## <a name="what-to-do-you-do-if-youre-affected"></a>Co zrobić, jeśli chcesz to zrobić

Zalecamy Zezwalanie na ruch wychodzący do adresów IP dla wszystkich [adresów IP bramy](connectivity-architecture.md#gateway-ip-addresses) w regionie na porcie TCP 1433 i zakres portów 11000-11999. To zalecenie dotyczy klientów łączących się z lokalnymi, a także połączeń za pośrednictwem punktów końcowych usługi. Aby uzyskać więcej informacji na temat zakresów portów, zobacz [zasady połączeń](connectivity-architecture.md#connection-policy).

Połączenia wykonane z aplikacji przy użyciu sterownika programu Microsoft JDBC w wersji 4,0 mogą kończyć się niepowodzeniem weryfikacji certyfikatu. Niższe wersje programu Microsoft JDBC korzystają z nazwy pospolitej (CN) w polu podmiotu certyfikatu. Środki zaradcze polegają na zapewnieniu, że właściwość hostNameInCertificate jest ustawiona na *. database.windows.net. Aby uzyskać więcej informacji na temat sposobu ustawiania właściwości hostNameInCertificate, zobacz [nawiązywanie połączenia z szyfrowaniem](/sql/connect/jdbc/connecting-with-ssl-encryption).

Jeśli powyższe środki zaradcze nie działają, należy wysłać żądanie pomocy technicznej dla SQL Database lub wystąpienia zarządzanego SQL przy użyciu następującego adresu URL:https://aka.ms/getazuresupport

## <a name="next-steps"></a>Następne kroki

- Dowiedz się więcej o [architekturze łączności usługi Azure SQL](connectivity-architecture.md)
