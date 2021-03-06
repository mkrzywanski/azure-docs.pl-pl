---
title: Uaktualnianie do usługi Azure serwer proxy aplikacji usługi Azure AD | Microsoft Docs
description: Wybierz, które rozwiązanie proxy jest najlepsze w przypadku uaktualniania z programu Microsoft Forefront lub ujednoliconej bramy dostępu.
services: active-directory
documentationcenter: ''
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/17/2019
ms.author: kenwith
ms.reviewer: harshja
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: ef1ffb5ec0b39c5331afbc55d99268d9a3997e62
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "84764455"
---
# <a name="compare-remote-access-solutions"></a>Porównywanie rozwiązań dostępu zdalnego

Serwer proxy aplikacji usługi Azure Active Directory jest jednym z dwóch rozwiązań dostępu zdalnego oferowanych przez firmę Microsoft. Druga to serwer proxy aplikacji sieci Web, wersja lokalna. Te dwa rozwiązania zastępują wcześniejsze produkty oferowane przez firmę Microsoft: Microsoft Forefront Threat Management Gateway (TMG) i Unified Access Gateway (UAG). Skorzystaj z tego artykułu, aby zrozumieć, jak te cztery rozwiązania porównują ze sobą. Aby nadal korzystać z przestarzałych rozwiązań TMG lub UAG, Skorzystaj z tego artykułu w celu zaplanowania migracji do jednego z serwerów proxy aplikacji. 


## <a name="feature-comparison"></a>Porównanie funkcji

Skorzystaj z tej tabeli, aby zrozumieć, jak Brama zarządzania zagrożeniami (TMG), Unified Access Gateway (UAG), serwer proxy aplikacji sieci Web (WAP) i usługa Azure serwer proxy aplikacji usługi Azure AD (AP) porównują się ze sobą.

| Cecha | TMG | UAG | WAP | Interfejs API |
| ------- | --- | --- | --- | --- |
| Uwierzytelnianie certyfikatu | Tak | Tak | - | - |
| Selektywne publikowanie aplikacji przeglądarki | Tak | Tak | Tak | Tak |
| Wstępne uwierzytelnianie i logowanie jednokrotne | Tak | Tak | Tak | Tak | 
| Zapora warstwy 2/3 | Tak | Tak | - | - |
| Przekazywanie możliwości serwera proxy | Tak | - | - | - |
| Możliwości sieci VPN | Tak | Tak | - | - |
| Obsługa zaawansowanych protokołów | - | Tak | Tak, w przypadku uruchamiania za pośrednictwem protokołu HTTP | Tak, w przypadku uruchamiania za pośrednictwem protokołu HTTP lub za pośrednictwem bramy Pulpit zdalny |
| Służy jako serwer proxy usług ADFS | - | Tak | Tak | - |
| Jeden portal na potrzeby dostępu do aplikacji | - | Tak | - | Tak |
| Tłumaczenie łącza treści odpowiedzi | Tak | Tak | - | Tak | 
| Uwierzytelnianie z nagłówkami | - | Tak | - | Tak, z PingAccess | 
| Zabezpieczenia w skali chmury | - | - | - | Tak | 
| Dostęp warunkowy | - | Tak | - | Tak |
| Brak składników w strefie zdemilitaryzowana (DMZ) | - | - | - | Tak |
| Brak połączeń przychodzących | - | - | - | Tak |

W przypadku większości scenariuszy zalecamy korzystanie z platformy Azure serwer proxy aplikacji usługi Azure AD jako nowoczesnego rozwiązania. Serwer proxy aplikacji sieci Web jest preferowany tylko w scenariuszach, które wymagają serwera proxy dla AD FS i nie można używać domen niestandardowych w programie Azure Active Directory. 

Usługa Azure serwer proxy aplikacji usługi Azure AD oferuje unikatowe korzyści w porównaniu do podobnych produktów, w tym:

- Rozszerzanie usługi Azure AD do zasobów lokalnych
   - Zabezpieczenia i ochrona w skali chmury
   - Funkcje, takie jak dostęp warunkowy i Multi-Factor Authentication, można łatwo włączyć
- Brak składników w strefie zdemilitaryzowana
- Nie są wymagane żadne połączenia przychodzące
- Jeden panel dostępu, do którego użytkownicy mogą przejść dla wszystkich swoich aplikacji, w tym usługi O365, zintegrowane aplikacje SaaS w usłudze Azure AD i lokalne aplikacje sieci Web. 


## <a name="next-steps"></a>Następne kroki

- [Korzystanie z aplikacji usługi Azure AD w celu zapewnienia bezpiecznego dostępu zdalnego do aplikacji lokalnych](application-proxy.md)
