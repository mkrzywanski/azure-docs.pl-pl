---
title: Multi-Factor Authentication platformy Azure dla Twojej organizacji — Azure Active Directory
description: Dowiedz się więcej o dostępnych funkcjach Multi-Factor Authentication platformy Azure dla Twojej organizacji w oparciu o model licencji
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 03/18/2020
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 38f3a6d9cea1aa1ebcb76f61882dcf2615dc4832
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "85554642"
---
# <a name="overview-of-azure-multi-factor-authentication-for-your-organization"></a>Omówienie Multi-Factor Authentication platformy Azure dla Twojej organizacji

Istnieje wiele sposobów włączania usługi Azure Multi-Factor Authentication dla użytkowników Azure Active Directory (AD) na podstawie licencji należących do organizacji. 

![Zbadaj sygnały i wymuś uwierzytelnianie MFA w razie konieczności](./media/concept-fundamentals-mfa-get-started/verify-signals-and-perform-mfa-if-required.png)

W oparciu o nasze studia Twoje konto ma więcej niż 99,9% mniej więcej w przypadku korzystania z uwierzytelniania wieloskładnikowego (MFA).

W jaki sposób Twoja organizacja włączy uwierzytelnianie wieloskładnikowe nawet bezpłatnie, zanim staną się statystyką?

## <a name="free-option"></a>Opcja bezpłatna

Klienci korzystający z bezpłatnych korzyści płynących z usługi Azure AD mogą używać [ustawień domyślnych zabezpieczeń](../fundamentals/concept-fundamentals-security-defaults.md) w celu włączenia uwierzytelniania wieloskładnikowego w swoich środowiskach.

## <a name="microsoft-365-business-e3-or-e5"></a>Microsoft 365 Business, E3 lub E5

W przypadku klientów z pakietem Office 365 dostępne są dwie opcje:

* Usługa Azure Multi-Factor Authentication jest włączona lub wyłączona dla wszystkich użytkowników w przypadku wszystkich zdarzeń związanych z logowaniem. Nie ma możliwości włączania uwierzytelniania wieloskładnikowego tylko dla podzbioru użytkowników lub tylko w określonych scenariuszach. Zarządzanie odbywa się za pomocą portalu pakietu Office 365. 
* Aby ulepszyć środowisko użytkownika, należy przeprowadzić uaktualnienie do Azure AD — wersja Premium P1 lub P2 i korzystać z dostępu warunkowego. Aby uzyskać więcej informacji, zobacz Zabezpieczanie zasobów pakietu Office 365 przy użyciu usługi uwierzytelniania wieloskładnikowego.

## <a name="azure-ad-premium-p1"></a>Usługa Azure AD — warstwa Premium P1

W przypadku klientów mających Azure AD — wersja Premium P1 lub podobne licencje zawierające te funkcje, takie jak Enterprise Mobility + Security E3, Microsoft 365 F1 lub Microsoft 365 E3: 

Korzystanie z [dostępu warunkowego usługi Azure AD](../authentication/tutorial-enable-azure-mfa.md) w celu monitowania użytkowników o uwierzytelnianie wieloskładnikowe w określonych scenariuszach lub zdarzeniach spełniających wymagania biznesowe.

## <a name="azure-ad-premium-p2"></a>Usługa Azure AD — warstwa Premium P2

W przypadku klientów mających Azure AD — wersja Premium P2 lub podobne licencje zawierające te funkcje, takie jak Enterprise Mobility + Security E5 lub Microsoft 365 E5: 

Zapewnia najmocniejszą pozycję zabezpieczeń i ulepszone środowisko użytkownika. Dodaje [dostęp warunkowy oparty na ryzyku](../conditional-access/howto-conditional-access-policy-risk.md) do funkcji Azure AD — wersja Premium P1, które dostosowują się do wzorców użytkownika i minimalizują komunikaty uwierzytelniania wieloskładnikowego.

## <a name="authentication-methods"></a>Metody uwierzytelniania

| Metoda | Domyślne ustawienia zabezpieczeń | Wszystkie inne metody |
| --- | --- | --- |
| Powiadomienie za poorednictwem aplikacji mobilnej | X | X |
| Kod weryfikacyjny z aplikacji mobilnej lub tokenu sprzętowego |   | X |
| Wiadomość SMS na telefon |   | X |
| Wywołanie telefonu |   | X |

## <a name="next-steps"></a>Następne kroki

Aby rozpocząć, zapoznaj się z samouczkiem [dotyczącym zabezpieczania zdarzeń logowania użytkowników przy użyciu usługi Azure Multi-Factor Authentication](../authentication/tutorial-enable-azure-mfa.md).

Aby uzyskać więcej informacji na temat licencjonowania, zobacz [funkcje i licencje dla platformy Azure Multi-Factor Authentication](../authentication/concept-mfa-licensing.md).
