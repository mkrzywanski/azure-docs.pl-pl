---
title: Dostęp warunkowy — Blokuj dostęp Azure Active Directory
description: Utwórz niestandardowe zasady dostępu warunkowego do
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: how-to
ms.date: 05/26/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb,
ms.collection: M365-identity-device-management
ms.openlocfilehash: b3ee7287f2a5cf9491ae91d434caf2f653c853a3
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "83995313"
---
# <a name="conditional-access-block-access"></a>Dostęp warunkowy: Blokuj dostęp

W przypadku organizacji z podejściem do migracji w chmurze można użyć opcji Blokuj wszystkie zasady. 

> [!CAUTION]
> Błędna konfiguracja zasad blokowania może prowadzić do zablokowania organizacji Azure Portal.

Takie zasady mogą mieć niezamierzone skutki uboczne. Poprawne testowanie i walidacja są istotne przed włączeniem. Administratorzy powinni korzystać z narzędzi, takich jak [tryb tylko raport dostęp warunkowy](concept-conditional-access-report-only.md) i [Narzędzie What If w przypadku dostępu warunkowego](what-if-tool.md) podczas wprowadzania zmian.

## <a name="user-exclusions"></a>Wykluczenia użytkowników

Zasady dostępu warunkowego to zaawansowane narzędzia, dlatego zalecamy wykluczenie następujących kont z zasad:

* **Dostęp awaryjny** lub konta z **przerwaniem** do blokowania kont w całej dzierżawie. W mało prawdopodobnym scenariuszu wszyscy administratorzy są Zablokowani z Twojej dzierżawy, w celu zalogowania się do dzierżawy można użyć konta administratora z dostępem awaryjnym.
   * Więcej informacji można znaleźć w artykule [Zarządzanie kontami dostępu awaryjnego w usłudze Azure AD](../users-groups-roles/directory-emergency-access.md).
* **Konta usług** i jednostki **usługi**, takie jak konto synchronizacji Azure AD Connect. Konta usług są kontami nieinteraktywnymi, które nie są powiązane z żadnym konkretnym użytkownikiem. Są one zwykle używane przez usługi zaplecza umożliwiające programistyczny dostęp do aplikacji, ale są również używane do logowania się do systemów w celach administracyjnych. Konta usług, takie jak te, powinny być wykluczone, ponieważ nie można programowo zakończyć usługi MFA. Wywołania wykonywane przez jednostki usługi nie są blokowane przez dostęp warunkowy.
   * Jeśli w organizacji są używane te konta w skryptach lub kodzie, należy rozważyć zastępowanie ich [tożsamościami zarządzanymi](../managed-identities-azure-resources/overview.md). W ramach tymczasowego obejścia można wykluczyć te określone konta z zasad linii bazowej.

## <a name="create-a-conditional-access-policy"></a>Tworzenie zasad dostępu warunkowego

Poniższe kroki ułatwią utworzenie zasad dostępu warunkowego w celu zablokowania dostępu do wszystkich aplikacji z wyjątkiem [pakietu Office 365](concept-conditional-access-cloud-apps.md#office-365-preview) , jeśli użytkownicy nie znajdują się w zaufanej sieci. Te zasady są umieszczane w [trybie tylko do raportowania](howto-conditional-access-report-only.md) w celu uruchomienia, aby administratorzy mogli ustalić wpływ, jaki będzie miał na istniejących użytkowników. Gdy Administratorzy są niewoli, że zasady są stosowane w miarę ich założeń, mogą przełączać **je do programu**.

Pierwsze zasady blokują dostęp do wszystkich aplikacji z wyjątkiem aplikacji pakietu Office 365, jeśli nie znajduje się w zaufanej lokalizacji.

1. Zaloguj się do **Azure Portal** jako Administrator globalny, administrator zabezpieczeń lub administrator dostępu warunkowego.
1. Przejdź do **Azure Active Directory**  >  **Security**  >  **dostępu warunkowego**zabezpieczeń.
1. Wybierz pozycję **nowe zasady**.
1. Nadaj zasadom nazwę. Firma Microsoft zaleca, aby organizacje utworzyły znaczący Standard nazw swoich zasad.
1. W obszarze **Przypisania** wybierz pozycję **Użytkownicy i grupy**.
   1. W obszarze **dołączanie**wybierz pozycję **Wszyscy użytkownicy**.
   1. W obszarze **Wyklucz**wybierz pozycję **Użytkownicy i grupy** , a następnie wybierz opcję dostęp awaryjny lub konta w ramach swojej organizacji. 
   1. Wybierz pozycję **Gotowe**.
1. W obszarze **aplikacje lub akcje w chmurze**wybierz następujące opcje:
   1. W obszarze **dołączanie**wybierz pozycję **wszystkie aplikacje w chmurze**.
   1. W obszarze **Wyklucz**wybierz pozycję **Office 365 (wersja zapoznawcza)**, wybierz pozycję **Wybierz**, a następnie wybierz pozycję **gotowe**.
1. W **warunkach**:
   1. W **Conditions**obszarze  >  **Lokalizacja**warunków.
      1. Ustaw **wartość** **tak**
      1. W obszarze **dołączanie**wybierz **dowolną lokalizację**.
      1. W obszarze **Wyklucz**wybierz opcję **wszystkie Zaufane lokalizacje**.
      1. Wybierz pozycję **Gotowe**.
   1. W obszarze **aplikacje klienckie (wersja zapoznawcza)** **Ustaw wartość** **tak**, **a następnie wybierz**pozycję **gotowe**.
1. W obszarze **Kontrola dostępu**  >  **Przyznaj**wybierz pozycję **Blokuj dostęp**, a następnie wybierz pozycję **Wybierz**.
1. Potwierdź ustawienia i ustaw opcję **Włącz zasady** **tylko na raport**.
1. Wybierz pozycję **Utwórz** , aby utworzyć zasady.

Poniżej utworzono drugie zasady, które wymagają uwierzytelniania wieloskładnikowego lub zgodnego urządzenia dla użytkowników pakietu Office 365.

1. Wybierz pozycję **nowe zasady**.
1. Nadaj zasadom nazwę. Firma Microsoft zaleca, aby organizacje utworzyły znaczący Standard nazw swoich zasad.
1. W obszarze **Przypisania** wybierz pozycję **Użytkownicy i grupy**.
   1. W obszarze **dołączanie**wybierz pozycję **Wszyscy użytkownicy**.
   1. W obszarze **Wyklucz**wybierz pozycję **Użytkownicy i grupy** , a następnie wybierz opcję dostęp awaryjny lub konta w ramach swojej organizacji. 
   1. Wybierz pozycję **Gotowe**.
1. W obszarze **aplikacje lub akcje w chmurze**  >  **Uwzględnij**wybierz pozycję **Wybierz aplikacje**, wybierz **Office 365 (wersja zapoznawcza)** i wybierz pozycję **Wybierz**, a następnie pozycję **gotowe**.
1. W obszarze **Kontrola dostępu**  >  **Przyznaj**wybierz pozycję **Udziel dostępu**.
   1. Wybierz opcję **Wymagaj uwierzytelniania wieloskładnikowego** i **Wymagaj, aby urządzenie było oznaczone jako zgodne** wybierz pozycję **Wybierz**.
   1. Upewnij się, że wybrano **wszystkie wybrane kontrolki** .
   1. Wybierz pozycję **Wybierz**.
1. Potwierdź ustawienia i ustaw opcję **Włącz zasady** **tylko na raport**.
1. Wybierz pozycję **Utwórz** , aby utworzyć zasady.

## <a name="next-steps"></a>Następne kroki

[Wspólne zasady dostępu warunkowego](concept-conditional-access-policy-common.md)

[Określanie wpływu przy użyciu trybu tylko Raport z dostępem warunkowym](howto-conditional-access-report-only.md)

[Symulowanie zachowania logowania za pomocą narzędzia What If dostępu warunkowego](troubleshoot-conditional-access-what-if.md)
