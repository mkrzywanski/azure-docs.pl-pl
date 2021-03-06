---
title: Rozwiązywanie problemów z błędami w interfejsie API raportowania Azure Active Directory | Microsoft Docs
description: Zapewnia rozwiązanie błędów podczas wywoływania interfejsów API raportowania Azure Active Directory.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 0030c5a4-16f0-46f4-ad30-782e7fea7e40
ms.service: active-directory
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: eec5c5a3d810fdd2d561313e3a355e872fb525c2
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "85608096"
---
# <a name="troubleshoot-errors-in-azure-active-directory-reporting-api"></a>Rozwiązywanie problemów z błędami w interfejsie API raportowania Azure Active Directory

W tym artykule wymieniono typowe komunikaty o błędach, które można napotkać podczas uzyskiwania dostępu do raportów aktywności przy użyciu interfejsu API Microsoft Graph i kroki dla ich rozdzielczości.

### <a name="500-http-internal-server-error-while-accessing-microsoft-graph-v2-endpoint"></a>Błąd wewnętrzny serwera HTTP 500 podczas uzyskiwania dostępu do punktu końcowego Microsoft Graph v2

Obecnie nie obsługujemy punktu końcowego Microsoft Graph v2 — upewnij się, że uzyskujesz dostęp do dzienników aktywności przy użyciu punktu końcowego Microsoft Graph v1.

### <a name="error-neither-tenant-is-b2c-or-tenant-doesnt-have-premium-license"></a>Błąd: żaden dzierżawca nie jest B2C lub dzierżawa nie ma licencji Premium

Uzyskiwanie dostępu do raportów logowania wymaga licencji Azure Active Directory Premium 1 (P1). Jeśli ten komunikat o błędzie jest wyświetlany podczas uzyskiwania dostępu do logowania, upewnij się, że dzierżawa jest licencjonowana przy użyciu licencji usługi Azure AD P1.

### <a name="error-user-is-not-in-the-allowed-roles"></a>Błąd: użytkownik nie znajduje się w dozwolonych rolach 

Jeśli ten komunikat o błędzie jest wyświetlany podczas próby dostępu do dzienników inspekcji lub logowania przy użyciu interfejsu API, upewnij się, że konto należy do roli **czytelnik zabezpieczeń** lub **czytelnik raportu** w dzierżawie Azure Active Directory. 

### <a name="error-application-missing-aad-read-directory-data-permission"></a>Błąd: brak uprawnienia do odczytu danych katalogu usługi AAD w aplikacji 

Wykonaj kroki opisane w sekcji [wymagania wstępne, aby uzyskać dostęp do interfejsu API raportowania Azure Active Directory](howto-configure-prerequisites-for-reporting-api.md) , aby upewnić się, że aplikacja działa z odpowiednim zestawem uprawnień. 

### <a name="error-application-missing-microsoft-graph-api-read-all-audit-log-data-permission"></a>Błąd: brak aplikacji Microsoft Graph interfejs API "Odczyt wszystkich danych dziennika inspekcji"

Wykonaj kroki opisane w sekcji [wymagania wstępne, aby uzyskać dostęp do interfejsu API raportowania Azure Active Directory](howto-configure-prerequisites-for-reporting-api.md) , aby upewnić się, że aplikacja działa z odpowiednim zestawem uprawnień. 

## <a name="next-steps"></a>Następne kroki

[Użyj dokumentacji dotyczącej](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/directoryaudit) 
 interfejsu API inspekcji [Korzystanie z interfejsu API raportów działań związanych z logowaniem](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/signin)
