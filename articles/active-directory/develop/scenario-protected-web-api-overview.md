---
title: Chroniony internetowy interfejs API — Omówienie
titleSuffix: Microsoft identity platform
description: Dowiedz się, jak utworzyć chroniony internetowy interfejs API (omówienie).
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 07/14/2020
ms.author: jmprieur
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: 4e530f76c8301dc74f73b675befa6f0710aedab7
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/23/2020
ms.locfileid: "87026632"
---
# <a name="scenario-protected-web-api"></a>Scenariusz: chroniony internetowy interfejs API

W tym scenariuszu dowiesz się, jak uwidocznić internetowy interfejs API. Dowiesz się również, jak chronić internetowy interfejs API, tak aby tylko uwierzytelnieni użytkownicy mieli do niego dostęp.

Aby korzystać z internetowego interfejsu API, należy włączyć użytkowników uwierzytelnionych zarówno przy użyciu kont służbowych, jak i na kontach osobistych firmy Microsoft.

## <a name="prerequisites"></a>Wymagania wstępne

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="specifics"></a>Szczegółowych informacji

Poniżej przedstawiono szczegółowe informacje, które należy znać, aby chronić interfejsy API sieci Web:

- Rejestracja aplikacji musi uwidaczniać co najmniej jeden *zakres* lub jedną *rolę aplikacji*.
  - Zakresy są udostępniane przez interfejsy API sieci Web, które są wywoływane w imieniu użytkownika.
  - Role aplikacji są udostępniane przez interfejsy API sieci Web wywoływane przez aplikacje demona (które wywołuje internetowy interfejs API w ich imieniu).
- W przypadku tworzenia nowej rejestracji aplikacji internetowego interfejsu API wybierz [wersję tokenu dostępu](reference-app-manifest.md#accesstokenacceptedversion-attribute) zaakceptowaną przez internetowy interfejs API `2` . W przypadku starszych interfejsów API sieci Web zaakceptowana wersja tokenu może być `null` , ale ta wartość ogranicza odbiorców logowania tylko do organizacji, a osobiste konta Microsoft (MSA) nie będą obsługiwane.
- Konfiguracja kodu dla internetowego interfejsu API musi weryfikować token używany podczas wywoływania internetowego interfejsu API.
- Kod w akcjach kontrolera musi sprawdzać poprawność ról lub zakresów w tokenie.

## <a name="next-steps"></a>Następne kroki

> [!div class="nextstepaction"]
> [Rejestracja aplikacji](scenario-protected-web-api-app-registration.md)
