---
title: Dodawanie niestandardowych zatwierdzeń do samoobsługowego przepływu rejestracji — Azure AD
description: Dodaj łączniki interfejsu API dla niestandardowych przepływów pracy zatwierdzania w ramach samoobsługowego rejestrowania tożsamości, Azure Active Directory (Azure AD)
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: how-to
ms.date: 06/16/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6d1a4495b1d637b1cf8592f8c17e63ad456ea3c4
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/23/2020
ms.locfileid: "87027465"
---
# <a name="add-a-custom-approval-workflow-to-self-service-sign-up"></a>Dodawanie niestandardowego przepływu pracy zatwierdzenia do rejestracji samoobsługowej

[Łączniki interfejsu API](api-connectors-overview.md)umożliwiają integrację z własnymi niestandardowymi przepływami pracy zatwierdzania za pomocą rejestracji samoobsługowej, dzięki czemu można zarządzać kontami użytkowników-Gości tworzonych w dzierżawie.

Ten artykuł zawiera przykład integracji z systemem zatwierdzania. W tym przykładzie przepływ użytkownika samoobsługowego tworzenia konta zbiera dane użytkownika podczas procesu rejestracji i przekazuje go do systemu zatwierdzania. Następnie system zatwierdzania może:

- Automatycznie Zatwierdź użytkownika i zezwól usłudze Azure AD na utworzenie konta użytkownika.
- Wyzwól przegląd ręczny. Jeśli żądanie zostało zatwierdzone, system zatwierdzania używa Microsoft Graph do aprowizacji konta użytkownika. System zatwierdzania umożliwia również powiadomienie użytkownika o utworzeniu konta.

## <a name="register-an-application-for-your-approval-system"></a>Rejestrowanie aplikacji dla systemu zatwierdzania

Musisz zarejestrować system zatwierdzania jako aplikację w dzierżawie usługi Azure AD, aby można było uwierzytelnić się w usłudze Azure AD i mieć uprawnienia do tworzenia użytkowników. Dowiedz się więcej [na temat podstawy uwierzytelniania i autoryzacji dla Microsoft Graph](https://docs.microsoft.com/graph/auth/auth-concepts).

1. Zaloguj się do [Azure Portal](https://portal.azure.com) jako administrator usługi Azure AD.
2. W obszarze **usługi platformy Azure**wybierz pozycję **Azure Active Directory**.
3. W menu po lewej stronie wybierz pozycję **rejestracje aplikacji**, a następnie wybierz pozycję **Nowa rejestracja**.
4. Wprowadź **nazwę** aplikacji, na przykład _zatwierdzenia rejestracji_.

   <!-- ![Register an application for the approval system](./self-service-sign-up-add-approvals/approvals/register-an-approvals-application.png) -->

5. Wybierz pozycję **Rejestruj**. W ustawieniach domyślnych można pozostawić inne pola.

   ![Rejestrowanie strony aplikacji](media/self-service-sign-up-add-approvals/register-approvals-app.png)

6. W obszarze **Zarządzaj** w menu po lewej stronie wybierz pozycję **uprawnienia interfejsu API**, a następnie wybierz pozycję **Dodaj uprawnienie**.
7. Na stronie **uprawnienia interfejsu API żądania** wybierz pozycję **Microsoft Graph**, a następnie wybierz pozycję **uprawnienia aplikacji**.
8. W obszarze **Wybierz uprawnienia**rozwiń pozycję **użytkownik**, a następnie zaznacz pole wyboru **User. ReadWrite. All** . To uprawnienie umożliwia systemowi zatwierdzania Tworzenie użytkownika po zatwierdzeniu. Następnie wybierz pozycję **Dodaj uprawnienia**.

   ![Rejestrowanie strony aplikacji](media/self-service-sign-up-add-approvals/request-api-permissions.png)

9. Na stronie **uprawnienia interfejsu API** wybierz opcję **Przyznaj zgodę administratora na nazwę dzierżawy**, a następnie wybierz pozycję **tak**.
10. W obszarze **Zarządzaj** w menu po lewej stronie wybierz pozycję **Certyfikaty & wpisy tajne**, a następnie wybierz pozycję **nowy klucz tajny klienta**.
11. Wprowadź **Opis** wpisu tajnego, na przykład _zatwierdzenie klucza tajnego klienta_i wybierz czas **wygaśnięcia**klucza tajnego klienta. Następnie wybierz pozycję **Dodaj**.
12. Skopiuj wartość klucza tajnego klienta.

    ![Skopiuj klucz tajny klienta do użycia w systemie zatwierdzania](media/self-service-sign-up-add-approvals/client-secret-value-copy.png)

13. Skonfiguruj system zatwierdzania tak, aby używał **identyfikatora aplikacji** jako identyfikatora klienta i **klucza tajnego klienta** wygenerowanego do uwierzytelniania w usłudze Azure AD.

## <a name="create-the-api-connectors"></a>Tworzenie łączników interfejsu API

Następnie utworzysz [Łączniki interfejsu API](self-service-sign-up-add-api-connector.md#create-an-api-connector) dla przepływu użytkownika samoobsługowego tworzenia konta. Interfejs API systemu zatwierdzania wymaga dwóch łączników i odpowiednich punktów końcowych, takich jak Przykłady przedstawione poniżej. Te łączniki interfejsu API są następujące:

- **Sprawdź stan zatwierdzenia**. Wyślij wywołanie do systemu zatwierdzania natychmiast po zalogowaniu się użytkownika przy użyciu dostawcy tożsamości w celu sprawdzenia, czy użytkownik ma istniejące żądanie zatwierdzenia lub zostało już odrzucone. Jeśli system zatwierdzania tylko podejmuje decyzje dotyczące automatycznego zatwierdzania, ten łącznik interfejsu API może nie być wymagany. Przykład łącznika interfejsu API "Sprawdzanie stanu zatwierdzenia".

  ![Sprawdź konfigurację łącznika interfejsu API stanu zatwierdzenia](./media/self-service-sign-up-add-approvals/check-approval-status-api-connector-config-alt.png)

- **Żądaj zatwierdzenia** — Wyślij wywołanie do systemu zatwierdzania po zakończeniu przez użytkownika strony kolekcji atrybutów, ale przed utworzeniem konta użytkownika, aby zażądać zatwierdzenia. Żądanie zatwierdzenia można automatycznie udzielić lub ręcznie przejrzeć. Przykład łącznika interfejsu API "Żądaj zatwierdzenia". Wybierz wszelkie **oświadczenia, które mają zostać wysłane** przez system zatwierdzania, aby podjąć decyzję o zatwierdzeniu.

  ![Zażądaj konfiguracji łącznika interfejsu API zatwierdzania](./media/self-service-sign-up-add-approvals/create-approval-request-api-connector-config-alt.png)

Aby utworzyć te łączniki, wykonaj kroki opisane w temacie [Tworzenie łącznika interfejsu API](self-service-sign-up-add-api-connector.md#create-an-api-connector).

## <a name="enable-the-api-connectors-in-a-user-flow"></a>Włączanie łączników interfejsu API w przepływie użytkownika

Teraz można dodać łączniki interfejsu API do samoobsługowego przepływu użytkownika przy użyciu następujących kroków:

1. Zaloguj się do [Azure Portal](https://portal.azure.com/) jako administrator usługi Azure AD.
2. W obszarze **usługi platformy Azure**wybierz pozycję **Azure Active Directory**.
3. W menu po lewej stronie wybierz pozycję **tożsamości zewnętrzne**.
4. Wybierz pozycję **przepływy użytkownika (wersja zapoznawcza)**, a następnie wybierz przepływ użytkownika, dla którego chcesz włączyć łącznik interfejsu API.
5. Wybierz pozycję **Łączniki interfejsu API**, a następnie wybierz punkty końcowe interfejsu API, które mają być wywoływane, w następujących krokach przepływu użytkownika:

   - **Po zalogowaniu się przy użyciu dostawcy tożsamości**: wybierz łącznik interfejsu API stanu zatwierdzenia, na przykład _Sprawdź stan zatwierdzenia_.
   - **Przed utworzeniem użytkownika**: wybierz łącznik interfejsu API żądania zatwierdzenia, na przykład _Zażądaj zatwierdzenia_.

   ![Dodawanie interfejsów API do przepływu użytkownika](./media/self-service-sign-up-add-approvals/api-connectors-user-flow-api.png)

6. Wybierz pozycję **Zapisz**.

## <a name="control-the-sign-up-flow-with-api-responses"></a>Sterowanie przepływem rejestracji przy użyciu odpowiedzi interfejsu API

System zatwierdzania może używać [typów odpowiedzi interfejsu API](self-service-sign-up-add-api-connector.md#expected-response-types-from-the-web-api) z dwóch punktów końcowych interfejsu API do sterowania przepływem rejestracji.

### <a name="request-and-responses-for-the-check-approval-status-api-connector"></a>Żądanie i odpowiedzi dotyczące łącznika interfejsu API "Sprawdzanie stanu zatwierdzenia"

Przykład żądania odebranego przez interfejs API z łącznika interfejsu API "Sprawdzanie stanu zatwierdzenia":

```http
POST <Approvals-API-endpoint>
Content-type: application/json

{
 "email": "johnsmith@outlook.com",
 "identities": [
     {
     "signInType":"federated",
     "issuer":"facebook.com",
     "issuerAssignedId":"0123456789"
     }
 ],
 "ui_locales":"en-US"
}
```

#### <a name="continuation-response-for-check-approval-status"></a>Niedalsza odpowiedź dla "Sprawdź stan zatwierdzenia"

Punkt końcowy interfejsu API **sprawdzania stanu zatwierdzenia** powinien zwrócić odpowiedź kontynuacji, jeśli:

- Użytkownik nie zażądał wcześniej zatwierdzenia.

Przykład odpowiedzi kontynuacji:

```http
HTTP/1.1 200 OK
Content-type: application/json

{
    "version": "1.0.0",
    "action": "Continue"
}
```

#### <a name="blocking-response-for-check-approval-status"></a>Blokowanie odpowiedzi dla opcji "Sprawdź stan zatwierdzenia"

Punkt końcowy interfejsu API **sprawdzania stanu zatwierdzenia** powinien zwrócić odpowiedź blokującą, jeśli:

- Oczekiwanie na zatwierdzenie przez użytkownika.
- Użytkownik został odrzucony i nie powinien mieć uprawnień do ponownego żądania zatwierdzenia.

Poniżej przedstawiono przykłady blokowania odpowiedzi:

```http
HTTP/1.1 200 OK
Content-type: application/json

{
    "version": "1.0.0",
    "action": "ShowBlockPage",
    "userMessage": "Your access request is already processing. You'll be notified when your request has been approved.",
    "code": "CONTOSO-APPROVAL-PENDING"
}
```

```http
HTTP/1.1 200 OK
Content-type: application/json

{
    "version": "1.0.0",
    "action": "ShowBlockPage",
    "userMessage": "Your sign up request has been denied. Please contact an administrator if you believe this is an error",
    "code": "CONTOSO-APPROVAL-DENIED"
}
```

### <a name="request-and-responses-for-the-request-approval-api-connector"></a>Żądanie i odpowiedzi dla łącznika interfejsu API "Żądaj zatwierdzenia"

Przykład żądania HTTP odebranego przez interfejs API z łącznika interfejsu API "Żądaj zatwierdzenia":

```http
POST <Approvals-API-endpoint>
Content-type: application/json

{
 "email": "johnsmith@outlook.com",
 "identities": [
     {
     "signInType":"federated",
     "issuer":"facebook.com",
     "issuerAssignedId":"0123456789"
     }
 ],
 "displayName": "John Smith",
 "city": "Redmond",
 "extension_<extensions-app-id>_CustomAttribute": "custom attribute value",
 "ui_locales":"en-US"
}
```

#### <a name="continuation-response-for-request-approval"></a>Dalsza odpowiedź dla "żądania zatwierdzenia"

Punkt końcowy interfejsu API **zatwierdzania żądań** powinien zwrócić odpowiedź kontynuacji, jeśli:

- Użytkownik może zostać **_automatycznie zatwierdzony_**.

Przykład odpowiedzi kontynuacji:

```http
HTTP/1.1 200 OK
Content-type: application/json

{
    "version": "1.0.0",
    "action": "Continue"
}
```

> [!IMPORTANT]
> W przypadku odebrania odpowiedzi na kontynuację usługa Azure AD tworzy konto użytkownika i kieruje użytkownika do aplikacji.

#### <a name="blocking-response-for-request-approval"></a>Blokowanie odpowiedzi dla "Żądaj zatwierdzenia"

Punkt końcowy interfejsu API **zatwierdzania żądań** powinien zwrócić odpowiedź blokującą, jeśli:

- Żądanie zatwierdzenia przez użytkownika zostało utworzone i trwa oczekiwanie.
- Żądanie zatwierdzenia użytkownika zostało automatycznie odrzucone.

Poniżej przedstawiono przykłady blokowania odpowiedzi:

```http
HTTP/1.1 200 OK
Content-type: application/json

{
    "version": "1.0.0",
    "action": "ShowBlockPage",
    "userMessage": "Your account is now waiting for approval. You'll be notified when your request has been approved.",
    "code": "CONTOSO-APPROVAL-REQUESTED"
}
```

```http
HTTP/1.1 200 OK
Content-type: application/json

{
    "version": "1.0.0",
    "action": "ShowBlockPage",
    "userMessage": "Your sign up request has been denied. Please contact an administrator if you believe this is an error",
    "code": "CONTOSO-APPROVAL-AUTO-DENIED"
}
```

`userMessage`W odpowiedzi zostanie wyświetlony użytkownik, na przykład:

![Przykład oczekującej strony zatwierdzania](./media/self-service-sign-up-add-approvals/approval-pending.png)

## <a name="user-account-creation-after-manual-approval"></a>Tworzenie konta użytkownika po zatwierdzeniu ręcznym

Po uzyskaniu zatwierdzenia ręcznego system zatwierdzania niestandardowego tworzy konto [użytkownika](https://docs.microsoft.com/graph/azuread-users-concept-overview) przy użyciu [Microsoft Graph](https://docs.microsoft.com/graph/use-the-api). Sposób, w jaki system zatwierdzania, zależy od dostawcy tożsamości, który był używany przez użytkownika.

### <a name="for-a-federated-google-or-facebook-user"></a>Dla federacyjnego użytkownika usługi Google lub Facebook

> [!IMPORTANT]
> System zatwierdzania powinien jawnie sprawdzić `identities` , czy `identities[0]` `identities[0].issuer` istnieją i czy ma `identities[0].issuer` wartość "Facebook" lub "Google", aby użyć tej metody.

Jeśli użytkownik zalogował się przy użyciu konta Google lub Facebook, można użyć [interfejsu API tworzenia użytkownika](https://docs.microsoft.com/graph/api/user-post-users?view=graph-rest-1.0&tabs=http).

1. System zatwierdzania odbiera żądanie HTTP z przepływu użytkownika.

```http
POST <Approvals-API-endpoint>
Content-type: application/json

{
 "email": "johnsmith@outlook.com",
 "identities": [
     {
     "signInType":"federated",
     "issuer":"facebook.com",
     "issuerAssignedId":"0123456789"
     }
 ],
 "displayName": "John Smith",
 "city": "Redmond",
 "extension_<extensions-app-id>_CustomAttribute": "custom attribute value",
 "ui_locales":"en-US"
}
```

2. System zatwierdzania używa Microsoft Graph do tworzenia konta użytkownika.

```http
POST https://graph.microsoft.com/v1.0/users
Content-type: application/json

{
 "userPrincipalName": "johnsmith_outlook.com#EXT@contoso.onmicrosoft.com",
 "accountEnabled": true,
 "mail": "johnsmith@outlook.com",
 "userType": "Guest",
 "identities": [
     {
     "signInType":"federated",
     "issuer":"facebook.com",
     "issuerAssignedId":"0123456789"
     }
 ],
 "displayName": "John Smith",
 "city": "Redmond",
 "extension_<extensions-app-id>_CustomAttribute": "custom attribute value"
}
```

| Parametr                                           | Wymagane | Opis                                                                                                                                                            |
| --------------------------------------------------- | -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| userPrincipalName                                   | Tak      | Mogą być generowane przez pobranie `email` zgłoszenia do interfejsu API, zastąpienie `@` znaku znakiem `_` i wstępnie do `#EXT@<tenant-name>.onmicrosoft.com` . |
| accountEnabled                                      | Tak      | Musi być ustawiony na `true` .                                                                                                                                                 |
| mail (poczta)                                                | Tak      | Odpowiednik `email` żądania wysłanego do interfejsu API.                                                                                                               |
| userType                                            | Tak      | Musi być `Guest` . Wyznacza tego użytkownika jako użytkownika-gościa.                                                                                                                 |
| osob                                          | Tak      | Informacje o tożsamości federacyjnej.                                                                                                                                    |
| \<otherBuiltInAttribute>                            | Nie       | Inne wbudowane atrybuty, takie jak `displayName` , `city` i inne. Nazwy parametrów są takie same jak parametry wysyłane przez łącznik interfejsu API.                            |
| \<extension\_\{extensions-app-id}\_CustomAttribute> | Nie       | Atrybuty niestandardowe użytkownika. Nazwy parametrów są takie same jak parametry wysyłane przez łącznik interfejsu API.                                                            |

### <a name="for-a-federated-azure-active-directory-user"></a>Dla użytkownika federacyjnego Azure Active Directory

Jeśli użytkownik zaloguje się za pomocą konta Azure Active Directory federacyjnego, należy użyć [interfejsu API zaproszenia](https://docs.microsoft.com/graph/api/invitation-post?view=graph-rest-1.0) do utworzenia użytkownika, a następnie opcjonalnie [zaktualizować interfejs API aktualizacji użytkownika](https://docs.microsoft.com/graph/api/user-update?view=graph-rest-1.0) , aby przypisać więcej atrybutów do użytkownika.

1. System zatwierdzania odbiera żądanie HTTP z przepływu użytkownika.

```http
POST <Approvals-API-endpoint>
Content-type: application/json

{
 "email": "johnsmith@fabrikam.onmicrosoft.com",
 "displayName": "John Smith",
 "city": "Redmond",
 "extension_<extensions-app-id>_CustomAttribute": "custom attribute value",
 "ui_locales":"en-US"
}
```

2. System zatwierdzania tworzy zaproszenie przy użyciu `email` dostarczonego przez łącznik interfejsu API.

```http
POST https://graph.microsoft.com/v1.0/invitations
Content-type: application/json

{
    "invitedUserEmailAddress":"johnsmith@fabrikam.onmicrosoft.com",
    "inviteRedirectUrl" : "https://myapp.com"
}
```

Przykład odpowiedzi:

```http
HTTP/1.1 201 OK
Content-type: application/json

{
    ...
    "invitedUser": {
        "id": "<generated-user-guid>"
    }
}
```

3. System zatwierdzania używa identyfikatora zaproszonego użytkownika do zaktualizowania konta użytkownika za pomocą zebranych atrybutów użytkownika (opcjonalnie).

```http
PATCH https://graph.microsoft.com/v1.0/users/<generated-user-guid>
Content-type: application/json

{
    "displayName": "John Smith",
    "city": "Redmond",
    "extension_<extensions-app-id>_AttributeName": "custom attribute value"
}
```

## <a name="next-steps"></a>Następne kroki

- Rozpocznij pracę z naszymi [przykładami szybkiego startu usługi Azure Functions](code-samples-self-service-sign-up.md#api-connector-azure-function-quickstarts).
- Zapoznaj [się z przykładem samoobsługowego rejestrowania się dla użytkowników-Gości przy użyciu ręcznego zatwierdzania](code-samples-self-service-sign-up.md#custom-approval-workflows). 
