---
title: Zabezpieczenia w Azure Database for PostgreSQL — pojedynczy serwer
description: Omówienie funkcji zabezpieczeń w ramach Azure Database for PostgreSQL-jednego serwera.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 11/22/2019
ms.openlocfilehash: 02dc9e1ad9ee46b1a400e44b6ef737e70571a17a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "75972584"
---
# <a name="security-in-azure-database-for-postgresql---single-server"></a>Zabezpieczenia w Azure Database for PostgreSQL — pojedynczy serwer

Istnieje wiele warstw zabezpieczeń, które są dostępne do ochrony danych na serwerze Azure Database for PostgreSQL. W tym artykule opisano opcje zabezpieczeń.

## <a name="information-protection-and-encryption"></a>Ochrona informacji i szyfrowanie

### <a name="in-transit"></a>W trakcie przesyłania
Azure Database for PostgreSQL zabezpiecza dane przez szyfrowanie danych podczas przesyłania z Transport Layer Security. Szyfrowanie (SSL/TLS) jest domyślnie wymuszane.

### <a name="at-rest"></a>W spoczynku
Usługa Azure Database for PostgreSQL używa zatwierdzonego modułu kryptograficznego FIPS 140-2 do szyfrowania magazynu danych w spoczynku. Dane, w tym kopie zapasowe, są szyfrowane na dysku, z wyjątkiem plików tymczasowych utworzonych podczas wykonywania zapytań. Usługa używa szyfru AES 256-bit zawartego w szyfrowaniu usługi Azure Storage, a klucze są zarządzane przez system. Szyfrowanie magazynu jest zawsze włączone i nie można go wyłączyć.


## <a name="network-security"></a>Bezpieczeństwo sieci
Połączenia z serwerem Azure Database for PostgreSQL są najpierw kierowane przez bramę regionalną. Brama ma publicznie dostępny adres IP, podczas gdy adresy IP serwera są chronione. Więcej informacji o bramie można znaleźć w artykule dotyczącym [architektury łączności](concepts-connectivity-architecture.md).  

Nowo utworzony serwer Azure Database for PostgreSQL ma zaporę, która blokuje wszystkie połączenia zewnętrzne. Chociaż docierają one do bramy, nie mogą łączyć się z serwerem. 

### <a name="ip-firewall-rules"></a>Reguły zapory adresów IP
Reguły zapory adresów IP umożliwiają dostęp do serwerów na podstawie źródłowego adresu IP każdego żądania. Aby uzyskać więcej informacji, zobacz [Omówienie reguł zapory](concepts-firewall-rules.md) .

### <a name="virtual-network-firewall-rules"></a>Reguły zapory sieci wirtualnej
Punkty końcowe usługi sieci wirtualnej zwiększają łączność sieci wirtualnej przez sieć szkieletową platformy Azure. Korzystając z reguł sieci wirtualnej, można włączyć serwer Azure Database for PostgreSQL, aby zezwalać na połączenia z wybranych podsieci w sieci wirtualnej. Aby uzyskać więcej informacji, zobacz [Omówienie punktu końcowego usługi sieci wirtualnej](concepts-data-access-and-security-vnet.md).

### <a name="private-ip"></a>Prywatny adres IP
Link prywatny umożliwia nawiązanie połączenia z Azure Database for PostgreSQL pojedynczym serwerem na platformie Azure za pośrednictwem prywatnego punktu końcowego. Link prywatny platformy Azure zasadniczo zapewnia usługi platformy Azure w ramach prywatnego Virtual Network (VNet). Dostęp do zasobów PaaS można uzyskać przy użyciu prywatnego adresu IP, podobnie jak w przypadku każdego innego zasobu w sieci wirtualnej. Aby uzyskać więcej informacji, zobacz [link prywatny — Omówienie](concepts-data-access-and-security-private-link.md)


## <a name="access-management"></a>Zarządzanie dostępem

Podczas tworzenia serwera Azure Database for PostgreSQL należy podać poświadczenia dla roli administratora. Ta rola administratora może służyć do tworzenia dodatkowych [ról PostgreSQL](https://www.postgresql.org/docs/current/user-manag.html).

Możesz również nawiązać połączenie z serwerem przy użyciu [uwierzytelniania Azure Active Directory (AAD)](concepts-aad-authentication.md).


## <a name="threat-protection"></a>Ochrona przed zagrożeniami

Możesz zrezygnować z [zaawansowaną ochronę przed zagrożeniami](concepts-data-access-and-security-threat-protection.md) , która wykrywa nietypowe działania wskazujące nietypową i potencjalnie szkodliwe próby dostępu do serwerów lub korzystania z nich.

[Rejestrowanie inspekcji](concepts-audit.md) jest dostępne do śledzenia aktywności w bazach danych. 


## <a name="next-steps"></a>Następne kroki
- Włącz reguły zapory dla [adresów IP](concepts-firewall-rules.md) lub [sieci wirtualnych](concepts-data-access-and-security-vnet.md)
- Informacje o [uwierzytelnianiu Azure Active Directory](concepts-aad-authentication.md) w programie Azure Database for PostgreSQL
