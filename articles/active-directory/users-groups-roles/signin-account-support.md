---
title: Czy moja strona logowania do usługi Azure AD akceptuje konta Microsoft | Microsoft Docs
description: Jak komunikaty na ekranie odzwierciedlają wyszukiwanie nazw użytkowników podczas logowania
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: overview
ms.date: 11/08/2019
ms.author: curtand
ms.reviewer: kexia
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 81ba7f77bb3396f49c3cf41d2266567aa48a8257
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "84731362"
---
# <a name="sign-in-options-for-microsoft-accounts-in-azure-active-directory"></a>Opcje logowania dla kont Microsoft w Azure Active Directory

Strona logowania Microsoft 365 dla Azure Active Directory (Azure AD) obsługuje konta służbowe i konta Microsoft, ale w zależności od sytuacji użytkownika może to być jeden lub drugi lub oba te elementy. Na przykład strona logowania usługi Azure AD obsługuje:

* Aplikacje akceptujące logowania z obu typów kont
* Organizacje akceptujące Gości

## <a name="identification"></a>Identyfikacja
Możesz określić, czy strona logowania używana przez organizację obsługuje konta Microsoft, przeglądając tekst wskazówki w polu Nazwa użytkownika. Jeśli tekst wskazówki brzmi "Poczta E-mail, telefon lub Skype", Strona logowania obsługuje konta Microsoft.

![Różnica między stronami logowania na koncie](./media/signin-account-support/ui-prompt.png)

[Dodatkowe opcje logowania działają tylko dla osobistych kont Microsoft](https://azure.microsoft.com/updates/microsoft-account-signin-options/ ) , ale nie można ich używać do logowania się do zasobów konta służbowego.

## <a name="next-steps"></a>Następne kroki

[Dostosuj znakowanie logowania](../fundamentals/add-custom-domain.md)