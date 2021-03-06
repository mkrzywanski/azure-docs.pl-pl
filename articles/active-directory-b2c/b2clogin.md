---
title: Migrowanie aplikacji i interfejsów API do b2clogin.com
titleSuffix: Azure AD B2C
description: Dowiedz się więcej na temat używania b2clogin.com w adresach URL przekierowania dla Azure Active Directory B2C.
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 07/17/2020
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 79807e8e0f798a73063576a00b8d0c32cdfe5a4b
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/23/2020
ms.locfileid: "87005348"
---
# <a name="set-redirect-urls-to-b2clogincom-for-azure-active-directory-b2c"></a>Ustaw adresy URL przekierowania na b2clogin.com dla Azure Active Directory B2C

Po skonfigurowaniu dostawcy tożsamości na potrzeby rejestracji i logowania w aplikacji Azure Active Directory B2C (Azure AD B2C) musisz określić adres URL przekierowania. Nie należy już odwoływać się do *login.microsoftonline.com* w aplikacjach i interfejsach API. Zamiast tego należy użyć *b2clogin.com* dla wszystkich nowych aplikacji i zmigrować istniejące aplikacje z *login.microsoftonline.com* do *b2clogin.com*.

## <a name="deprecation-of-loginmicrosoftonlinecom"></a>Zaniechano login.microsoftonline.com

W dniu 04 grudnia 2019 ogłoszono zaplanowaną emeryturę za pomoc techniczną login.microsoftonline.com w Azure AD B2C dniu **04 grudnia 2020**:

[Azure Active Directory B2C jest przestarzałe login.microsoftonline.com](https://azure.microsoft.com/updates/b2c-deprecate-msol/)

Wycofanie login.microsoftonline.com ma wpływ na wszystkie dzierżawy Azure AD B2C w dniu 04 grudnia 2020, dostarczając istniejących dzierżawców o jeden (1) rok, aby przeprowadzić migrację do b2clogin.com. Nowi dzierżawcy utworzone po 04 grudnia 2019 nie będą akceptować żądań od login.microsoftonline.com. Wszystkie funkcje pozostają takie same w punkcie końcowym b2clogin.com.

Zaniechanie login.microsoftonline.com nie ma wpływu na dzierżawy Azure Active Directory. Ta zmiana dotyczy tylko dzierżawców Azure Active Directory B2C.

## <a name="benefits-of-b2clogincom"></a>Zalety b2clogin.com

Jeśli używasz *b2clogin.com* jako adresu URL przekierowania:

* Zajęte miejsce w nagłówku pliku cookie przez usługi firmy Microsoft jest ograniczone.
* Adresy URL przekierowań nie muszą już zawierać odwołania do firmy Microsoft.
* Kod po stronie klienta JavaScript jest obsługiwany (obecnie w [wersji zapoznawczej](user-flow-javascript-overview.md)) na niestandardowych stronach. Ze względu na ograniczenia zabezpieczeń, kod JavaScript i elementy formularza HTML są usuwane z niestandardowych stron, jeśli używasz *login.microsoftonline.com*.

## <a name="overview-of-required-changes"></a>Przegląd wymaganych zmian

Istnieje kilka modyfikacji, które trzeba wykonać, aby przeprowadzić migrację aplikacji do *b2clogin.com*:

* Zmień adres URL przekierowania w aplikacjach dostawcy tożsamości, aby odwoływała się do *b2clogin.com*.
* Zaktualizuj aplikacje Azure AD B2C, aby używać *b2clogin.com* w ich przepływie użytkownika i odwołaniach do punktów końcowych tokenu.
* Zaktualizuj wszystkie **dozwolone źródła** zdefiniowane w ustawieniach mechanizmu CORS do [dostosowywania interfejsu użytkownika](custom-policy-ui-customization.md).

## <a name="change-identity-provider-redirect-urls"></a>Zmienianie adresów URL przekierowań dostawcy tożsamości

W każdej witrynie sieci Web dostawcy tożsamości, w której została utworzona aplikacja, Zmień wszystkie Zaufane adresy URL, aby przekierować do `your-tenant-name.b2clogin.com` *login.microsoftonline.com*.

Istnieją dwa formaty, których można użyć w odniesieniu do adresów URL przekierowania b2clogin.com. Pierwsza z nich to korzyść, która nie ma "firmy Microsoft" pojawia się w dowolnym miejscu w adresie URL przy użyciu identyfikatora dzierżawy (GUID) zamiast nazwy domeny dzierżawcy:

```
https://{your-tenant-name}.b2clogin.com/{your-tenant-id}/oauth2/authresp
```

Druga opcja używa nazwy domeny dzierżawy w postaci `your-tenant-name.onmicrosoft.com` . Na przykład:

```
https://{your-tenant-name}.b2clogin.com/{your-tenant-name}.onmicrosoft.com/oauth2/authresp
```

Dla obu formatów:

* Zamień `{your-tenant-name}` na nazwę dzierżawy Azure AD B2C.
* Usuń `/te` , jeśli istnieje w adresie URL.

## <a name="update-your-applications-and-apis"></a>Aktualizowanie aplikacji i interfejsów API

Kod w aplikacjach i interfejsach API z obsługą Azure AD B2C mogą odwoływać się do `login.microsoftonline.com` programu w kilku miejscach. Na przykład kod może zawierać odwołania do przepływów użytkowników i punktów końcowych tokenu. W zamian należy zaktualizować następujące informacje `your-tenant-name.b2clogin.com` :

* Punkt końcowy autoryzacji
* Punkt końcowy tokenu
* Wystawca tokenu

Na przykład punkt końcowy urzędu certyfikacji dla zasad rejestracji/logowania w firmie Contoso będzie teraz:

```
https://contosob2c.b2clogin.com/00000000-0000-0000-0000-000000000000/B2C_1_signupsignin1
```

Aby uzyskać informacje na temat migrowania aplikacji sieci Web opartych na OWIN do usługi b2clogin.com, zobacz [Migrowanie internetowego interfejsu API opartego na bazie danych Owin do programu b2clogin.com](multiple-token-endpoints.md).

Aby przeprowadzić migrację interfejsów API usługi Azure API Management chronionych przez usługę Azure AD B2C, zobacz sekcję [Migrowanie do b2clogin.com](secure-api-management.md#migrate-to-b2clogincom) w temacie [Zabezpieczanie interfejsu API API Management platformy Azure przy użyciu programu Azure AD B2C](secure-api-management.md).

## <a name="microsoft-authentication-library-msal"></a>Biblioteka uwierzytelniania firmy Microsoft (MSAL)

### <a name="msalnet-validateauthority-property"></a>Właściwość MSAL.NET ValidateAuthority

Jeśli używasz programu [MSAL.NET][msal-dotnet] v2 lub starszego, ustaw właściwość **ValidateAuthority** na `false` Tworzenie wystąpienia klienta, aby umożliwić przekierowywanie do *b2clogin.com*. Ustawienie tej wartości `false` nie jest wymagane dla MSAL.NET v3 i nowszych.

```csharp
ConfidentialClientApplication client = new ConfidentialClientApplication(...); // Can also be PublicClientApplication
client.ValidateAuthority = false; // MSAL.NET v2 and earlier **ONLY**
```

### <a name="msal-for-javascript-validateauthority-property"></a>MSAL dla właściwości validateAuthority języka JavaScript

Jeśli używasz [MSAL for JavaScript][msal-js] v 1.2.2 lub starszej wersji, ustaw właściwość **validateAuthority** na `false` .

```JavaScript
// MSAL.js v1.2.2 and earlier
this.clientApplication = new UserAgentApplication(
  env.auth.clientId,
  env.auth.loginAuthority,
  this.authCallback.bind(this),
  {
    validateAuthority: false // Required in MSAL.js v1.2.2 and earlier **ONLY**
  }
);
```

Jeśli ustawisz `validateAuthority: true` w MSAL.js 1.3.0 + (wartość domyślna), należy również określić prawidłowy Wystawca tokenu z `knownAuthorities` :

```JavaScript
// MSAL.js v1.3.0+
this.clientApplication = new UserAgentApplication(
  env.auth.clientId,
  env.auth.loginAuthority,
  this.authCallback.bind(this),
  {
    validateAuthority: true, // Supported in MSAL.js v1.3.0+
    knownAuthorities: ['tenant-name.b2clogin.com'] // Required if validateAuthority: true
  }
);
```

## <a name="next-steps"></a>Następne kroki

Aby uzyskać informacje na temat migrowania aplikacji sieci Web opartych na OWIN do usługi b2clogin.com, zobacz [Migrowanie internetowego interfejsu API opartego na bazie danych Owin do programu b2clogin.com](multiple-token-endpoints.md).

Aby przeprowadzić migrację interfejsów API usługi Azure API Management chronionych przez usługę Azure AD B2C, zobacz sekcję [Migrowanie do b2clogin.com](secure-api-management.md#migrate-to-b2clogincom) w temacie [Zabezpieczanie interfejsu API API Management platformy Azure przy użyciu programu Azure AD B2C](secure-api-management.md).

<!-- LINKS - External -->
[msal-dotnet]: https://github.com/AzureAD/microsoft-authentication-library-for-dotnet
[msal-dotnet-b2c]: https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/wiki/AAD-B2C-specifics
[msal-js]: https://github.com/AzureAD/microsoft-authentication-library-for-js
[msal-js-b2c]: ../active-directory/develop/msal-b2c-overview.md
