---
title: Rejestrowanie publicznej aplikacji klienckiej w usłudze Azure AD — Azure API for FHIR
description: W tym artykule wyjaśniono, jak zarejestrować publiczną aplikację kliencką w Azure Active Directory, w przygotowaniu do wdrażania interfejsu API FHIR na platformie Azure.
services: healthcare-apis
author: hansenms
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: conceptual
ms.date: 02/07/2019
ms.author: mihansen
ms.openlocfilehash: 5aa9e5a33dbe66e3ebd787decfa3a520454fc6f6
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "84871792"
---
# <a name="register-a-public-client-application-in-azure-active-directory"></a>Zarejestruj publiczną aplikację kliencką w Azure Active Directory

W tym artykule dowiesz się, jak zarejestrować aplikację publiczną w Azure Active Directory.  

Rejestracje aplikacji klienckich są Azure Active Directory reprezentacje aplikacji, które mogą uwierzytelniać i żądać uprawnień interfejsu API w imieniu użytkownika. Klienci publiczni są aplikacjami, takimi jak aplikacje mobilne i aplikacje JavaScript jednostronicowe, które nie mogą zachować tajemnicy tajnych. Ta procedura jest podobna do [rejestracji klienta poufnego](register-confidential-azure-ad-client-app.md), ale ponieważ klienci publiczni nie mogą być Zaufani do przechowywania klucza tajnego aplikacji, nie ma potrzeby dodawania go.

## <a name="app-registrations-in-azure-portal"></a>Rejestracje aplikacji w Azure Portal

1. W witrynie [Azure Portal](https://portal.azure.com) w panelu nawigacyjnym po lewej stronie kliknij pozycję **Azure Active Directory**.

2. W bloku **Azure Active Directory** kliknij pozycję **rejestracje aplikacji**:

    ![Azure Portal. Rejestracja nowej aplikacji.](media/how-to-aad/portal-aad-new-app-registration.png)

3. Kliknij **nową rejestrację**.

## <a name="application-registration-overview"></a>Przegląd rejestracji aplikacji

1. Nadaj aplikacji nazwę wyświetlaną.

2. Podaj adres URL odpowiedzi. Adres URL odpowiedzi to miejsce, w którym kody uwierzytelniania zostaną zwrócone do aplikacji klienckiej. Możesz dodać więcej adresów URL odpowiedzi i edytować je później.

    ![Azure Portal. Nowa rejestracja aplikacji publicznej.](media/how-to-aad/portal-aad-register-new-app-registration-PUB-CLIENT-NAME.png)

## <a name="api-permissions"></a>Uprawnienia aplikacji

Podobnie jak w przypadku [poufnej aplikacji klienckiej](register-confidential-azure-ad-client-app.md), należy wybrać uprawnienia interfejsu API, które ta aplikacja powinna być w stanie żądać w imieniu użytkowników:

1. Otwórz **uprawnienia interfejsu API**.

    Jeśli używasz interfejsu API platformy Azure dla usługi FHIR, dodasz uprawnienie do interfejsów API usługi Azure opieki zdrowotnej, wyszukując interfejsy API usługi Azure opieki zdrowotnej w obszarze **interfejsy API używane przez moją organizację** (Poniższy obraz).
    
    Jeśli odwołujesz się do innej aplikacji zasobów, wybierz [rejestrację aplikacji FHIR interfejsu API](register-resource-azure-ad-client-app.md) utworzoną wcześniej w obszarze **Moje interfejsy API**:

    ![Azure Portal. Nowe publiczne uprawnienia interfejsu API — usługa Azure API for FHIR — domyślne](media/public-client-app/api-permissions.png)


2. Wybierz uprawnienia, do których aplikacja ma mieć możliwość żądania: ![ Azure Portal. Uprawnienia aplikacji](media/public-client-app/app-permissions.png)

## <a name="validate-fhir-server-authority"></a>Weryfikuj urząd serwera FHIR
Jeśli aplikacja zarejestrowana w tym artykule i serwerze FHIR znajdują się w tej samej dzierżawie usługi Azure AD, warto przejoć do następnych kroków.

W przypadku skonfigurowania aplikacji klienckiej w innej dzierżawie usługi Azure AD z poziomu serwera FHIR należy zaktualizować **Urząd**. W interfejsie API platformy Azure dla FHIR ustaw pozycję urząd w obszarze Ustawienia--> uwierzytelnianie. Ustaw dla urzędu **https://login.microsoftonline.com/\<TENANT-ID>** .

## <a name="next-steps"></a>Następne kroki

W tym artykule przedstawiono sposób rejestrowania publicznej aplikacji klienckiej w Azure Active Directory. Następnie przetestuj dostęp do serwera FHIR za pomocą programu Poster.
 
>[!div class="nextstepaction"]
>[Dostęp do interfejsu API platformy Azure dla usługi FHIR za pomocą programu Poster](access-fhir-postman-tutorial.md)
