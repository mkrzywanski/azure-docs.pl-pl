---
title: Skonfiguruj zachowanie sesji — Azure Active Directory B2C | Microsoft Docs
description: Skonfiguruj zachowanie sesji w Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 07/30/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: c47b0249824a3683b8f24cc01cc5eea5ccf32585
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/31/2020
ms.locfileid: "87482672"
---
# <a name="configure-session-behavior-in-azure-active-directory-b2c"></a>Konfigurowanie zachowania sesji w Azure Active Directory B2C

Usługa logowania jednokrotnego [(SSO)](session-overview.md) w programie Azure Active Directory B2C (Azure AD B2C) umożliwia administratorowi sterowanie interakcją z użytkownikiem po jego uwierzytelnieniu. Administrator może na przykład określić, czy jest wyświetlany wybór dostawców tożsamości, czy też należy wprowadzić ponownie szczegóły konta. W tym artykule opisano sposób konfigurowania ustawień logowania jednokrotnego dla Azure AD B2C.

## <a name="session-behavior-properties"></a>Właściwości zachowania sesji

Aby zarządzać sesjami aplikacji sieci Web, można użyć następujących właściwości:

- **Okres istnienia sesji aplikacji sieci Web (w minutach)** — okres istnienia pliku cookie sesji usługi Azure AD B2C's w przeglądarce użytkownika po pomyślnym uwierzytelnieniu.
    - Wartość domyślna = 1440 min.
    - Minimum (włącznie) = 15 minut.
    - Maksimum (włącznie) = 1440 min.
- **Limit czasu sesji aplikacji sieci Web** — [Typ wygaśnięcia sesji](session-overview.md#session-expiry-type), wartość *krocząca*lub *bezwzględna*. 
- **Konfiguracja logowania** jednokrotnego — [zakres sesji](session-overview.md#session-scope) zachowań logowania jednokrotnego (SSO) w wielu aplikacjach i przepływach użytkowników w dzierżawie Azure AD B2C.


## <a name="configure-the-properties"></a>Konfigurowanie właściwości

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).
2. Upewnij się, że używasz katalogu, który zawiera dzierżawę Azure AD B2C, wybierając w górnym menu pozycję **katalog i subskrypcja** , a następnie wybierz katalog zawierający dzierżawcę Azure AD B2C.
3. Wybierz pozycję **Wszystkie usługi** w lewym górnym rogu witryny Azure Portal, a następnie wyszukaj i wybierz usługę **Azure AD B2C**.
4. Wybierz pozycję **przepływy użytkownika**.
5. Otwórz wcześniej utworzony przepływ użytkownika.
6. Wybierz pozycję **Właściwości**.
7. Skonfiguruj **okres istnienia sesji aplikacji sieci Web (minuty)**, **limit czasu sesji aplikacji sieci Web**, **konfigurację logowania**jednokrotnego i **Wymagaj tokenu identyfikatora w żądaniach wylogowywania** zgodnie z potrzebami.

    ![Ustawienia właściwości zachowanie sesji w Azure Portal](./media/session-behavior/session-behavior.png)

8. Kliknij pozycję **Zapisz**.

## <a name="next-steps"></a>Następne kroki

- Dowiedz się więcej o [sesji Azure AD B2C](session-overview.md).