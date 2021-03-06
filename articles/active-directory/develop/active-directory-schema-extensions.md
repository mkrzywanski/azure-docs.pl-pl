---
title: Używanie atrybutów rozszerzenia schematu usługi Azure AD w oświadczeniach
titleSuffix: Microsoft identity platform
description: Opisuje sposób używania atrybutów rozszerzenia schematu katalogu do wysyłania danych użytkownika do aplikacji w oświadczeniach tokenów.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.custom: aaddev
ms.workload: identity
ms.topic: how-to
ms.date: 07/29/2020
ms.author: ryanwi
ms.reviewer: paulgarn, hirsin, jeedes, luleon
ms.openlocfilehash: cd21ef8d697570afb2109bb56d552284c03fd9a2
ms.sourcegitcommit: 1b2d1755b2bf85f97b27e8fbec2ffc2fcd345120
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/04/2020
ms.locfileid: "87552785"
---
# <a name="using-directory-schema-extension-attributes-in-claims"></a>Używanie atrybutów rozszerzenia schematu katalogu w oświadczeniach

Atrybuty rozszerzenia schematu katalogu umożliwiają przechowywanie dodatkowych danych w Azure Active Directory obiektów użytkowników i innych obiektów katalogów, takich jak grupy, szczegóły dzierżawy, nazwy główne usługi.  Do emitowania oświadczeń do aplikacji można używać tylko atrybutów rozszerzenia dla obiektów użytkownika. W tym artykule opisano, jak używać atrybutów rozszerzenia schematu katalogu do wysyłania danych użytkownika do aplikacji w oświadczeniach tokenów.

> [!NOTE]
> Microsoft Graph oferuje dwa inne mechanizmy rozszerzenia do dostosowywania obiektów grafów. Są one znane jako Microsoft Graph otwieranie rozszerzeń i Microsoft Graph rozszerzeń schematu. Szczegółowe informacje znajdują się w [dokumentacji Microsoft Graph](/graph/extensibility-overview) . Dane przechowywane w obiektach Microsoft Graph przy użyciu tych funkcji nie są dostępne jako źródła oświadczeń w tokenach.

Atrybuty rozszerzenia schematu katalogu są zawsze skojarzone z aplikacją w dzierżawie i są przywoływane przez identyfikator aplikacji *w swojej* nazwie.

Identyfikator atrybutu rozszerzenia schematu katalogu ma postać *Extension_xxxxxxxxx_AttributeName*.  Gdzie *xxxxxxxxx* jest identyfikatorem *aplikacji, dla* którego zdefiniowano rozszerzenie.

## <a name="registering-and-using-directory-schema-extensions"></a>Rejestrowanie i używanie rozszerzeń schematu katalogu
Atrybuty rozszerzenia schematu katalogu mogą być rejestrowane i wypełniane na jeden z dwóch sposobów:

- Konfigurując program AD Connect w celu ich tworzenia i synchronizowania danych z lokalnej usługi AD. Zobacz [Azure AD Connect rozszerzenia katalogu synchronizacji](/azure/active-directory/hybrid/how-to-connect-sync-feature-directory-extensions).
- Przy użyciu Microsoft Graph do rejestrowania, ustawiania wartości i odczytywania z atrybutów rozszerzenia schematu katalogów [rozszerzenia schematu katalogu | Interfejs API programu Graph koncepcje](/previous-versions/azure/ad/graph/howto/azure-ad-graph-api-directory-schema-extensions) i/lub program PowerShell + [Zarządzanie atrybutami rozszerzenia za pomocą poleceń cmdlet programu PowerShell AzureAD](/powershell/azure/active-directory/using-extension-attributes-sample?view=azureadps-2.0).

### <a name="emitting-claims-with-data-from-directory-schema-extension-attributes-created-with-ad-connect"></a>Emitowanie oświadczeń z danymi z atrybutów rozszerzenia schematu katalogu utworzonych przy użyciu programu AD Connect
Atrybuty rozszerzenia schematu katalogu utworzone i zsynchronizowane przy użyciu programu AD Connect są zawsze skojarzone z IDENTYFIKATORem aplikacji używanym przez program AD Connect. Mogą one służyć jako źródło dla oświadczeń zarówno przez skonfigurowanie ich jako oświadczeń w konfiguracji **aplikacji przedsiębiorstwa** w interfejsie użytkownika portalu dla aplikacji SAML zarejestrowanych przy użyciu galerii lub środowiska konfiguracji aplikacji spoza galerii w obszarze **aplikacje dla przedsiębiorstw**, a także za pośrednictwem zasad mapowania oświadczeń dla aplikacji zarejestrowanych za pośrednictwem środowiska rejestracji aplikacji.  Gdy atrybut rozszerzenia katalogu utworzony za pośrednictwem programu AD Connect znajduje się w katalogu, zostanie wyświetlony w interfejsie użytkownika konfiguracji oświadczeń logowania jednokrotnego SAML.

### <a name="emitting-claims-with-data-from-directory-schema-extension-attributes-created-for-an-application-using-graph-or-powershell"></a>Emitowanie oświadczeń z danymi z atrybutów rozszerzenia schematu katalogu utworzonych dla aplikacji przy użyciu programu Graph lub programu PowerShell
Jeśli atrybut rozszerzenia schematu katalogu jest zarejestrowany dla aplikacji przy użyciu programu Microsoft Graph lub PowerShell (za pomocą początkowej konfiguracji aplikacji lub kroku aprowizacji dla wystąpienia), ta sama aplikacja może zostać skonfigurowana w Azure Active Directory do odbierania danych w tym atrybucie z obiektu użytkownika w ramach roszczeń po zalogowaniu się użytkownika.  Aplikację można skonfigurować tak, aby odbierać dane w rozszerzeniach schematu katalogu, które są zarejestrowane w tej samej aplikacji przy użyciu [opcjonalnych oświadczeń](active-directory-optional-claims.md#configuring-directory-extension-optional-claims).  Można je ustawić w manifeście aplikacji.  Dzięki temu aplikacja wielodostępna może rejestrować atrybuty rozszerzenia schematu katalogu do własnego użytku. Gdy aplikacja jest obsługiwana w dzierżawie, skojarzone rozszerzenia schematu katalogu stają się dostępne do ustawienia dla użytkowników w tej dzierżawie i używane.  Po jego skonfigurowaniu w dzierżawie i udzieleniu zgody można go użyć do przechowywania i pobierania danych za pośrednictwem grafu oraz do mapowania oświadczeń w tokenach platforma Microsoft Identity platform emituje do aplikacji.

Atrybuty rozszerzenia schematu katalogu mogą być rejestrowane i wypełniane dla dowolnej aplikacji.

Jeśli aplikacja musi wysyłać oświadczenia z danymi z atrybutu rozszerzenia zarejestrowanego w innej aplikacji, należy użyć [zasad mapowania oświadczeń](active-directory-claims-mapping.md) do mapowania atrybutu rozszerzenia na oświadczenie.  Typowym wzorcem do zarządzania atrybutami rozszerzenia schematu katalogu jest utworzenie aplikacji, która jest punktem rejestracji dla wszystkich potrzebnych rozszerzeń schematu.  Nie musi to być prawdziwa aplikacja, a ta technika oznacza, że wszystkie rozszerzenia mają ten sam identyfikator aplikacji w nazwie.

Na przykład poniżej przedstawiono zasady mapowania oświadczeń do emisji pojedynczego oświadczenia z atrybutu rozszerzenia schematu katalogu w tokenie OAuth/OIDC:

```json
{
    "ClaimsMappingPolicy": {
        "Version": 1,
        "IncludeBasicClaimSet": "false",
        "ClaimsSchema": [{
                "Source": "User",
                "ExtensionID": "extension_xxxxxxx_test",
                "JWTClaimType": "http://schemas.contoso.com/identity/claims/exampleclaim"
            }, 
        ]
    }
}
```

Gdzie *xxxxxxx* to identyfikator aplikacji, w której zostało zarejestrowane rozszerzenie.

> [!TIP]
> Podczas ustawiania atrybutów rozszerzenia katalogu dla obiektów należy pamiętać o spójności wielkości liter.  Nazwy atrybutów rozszerzeń nie uwzględniają wielkości liter podczas konfigurowania, ale w przypadku odczytywania z katalogu przez usługę tokenów rozróżniana jest wielkość liter.  Jeśli atrybut rozszerzenia jest ustawiony na obiekcie użytkownika o nazwie "LegacyId" i w innym obiekcie użytkownika o nazwie "LegacyId", gdy atrybut jest mapowany do roszczeń przy użyciu nazwy "LegacyId", dane zostaną pomyślnie pobrane i zostanie zawarte w tokenie dla pierwszego użytkownika, ale nie w drugim.
>
> Parametr "ID" w schemacie oświadczeń używanym dla wbudowanych atrybutów katalogu ma wartość "ExtensionID" dla atrybutów rozszerzenia katalogu.

## <a name="next-steps"></a>Następne kroki
- Dowiedz się [, jak dodać niestandardowe lub dodatkowe oświadczenia do tokenów tokenów sieci Web SAML 2,0 i JSON (JWT)](active-directory-optional-claims.md). 
- Dowiedz się, jak [dostosować oświadczenia emitowane w tokenach dla określonej aplikacji](active-directory-claims-mapping.md).