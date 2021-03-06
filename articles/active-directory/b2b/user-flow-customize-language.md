---
title: Dostosowywanie języka w przepływach użytkowników usługi Azure AD
description: Dowiedz się więcej o dostosowywaniu środowiska w przepływach użytkownika.
services: active-directory
author: msmimart
manager: celestedg
ms.service: active-directory
ms.subservice: B2B
ms.topic: how-to
ms.date: 05/06/2020
ms.author: mimart
ms.reviewer: elisolMS
ms.collection: M365-identity-device-management
ms.openlocfilehash: abd6423ebbdba11cd1b0e0c2d00cfd36aa745e72
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "85551155"
---
# <a name="language-customization-in-azure-active-directory-preview"></a>Dostosowanie języka w Azure Active Directory (wersja zapoznawcza)

> [!NOTE]
> Rejestracja samoobsługowa jest publiczną funkcją w wersji zapoznawczej Azure Active Directory. Aby uzyskać więcej informacji na temat wersji zapoznawczych, zobacz [dodatkowe warunki użytkowania wersji](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)zapoznawczych Microsoft Azure.

Dostosowanie języka w programie Azure Active Directory (Azure AD) pozwala przepływom użytkownika dostosowywać się do potrzeb użytkowników w różnych językach. Firma Microsoft udostępnia tłumaczenia dla [języków 36](#supported-languages). Nawet jeśli środowisko jest dostarczane tylko dla jednego języka, można dostosować nazwy atrybutów na stronie kolekcji atrybutów.

## <a name="how-language-customization-works"></a>Jak działa dostosowanie języka

Domyślnie dostosowanie języka jest włączone dla użytkowników logujących się, aby zapewnić spójne środowisko rejestracji. Za pomocą języków można modyfikować ciągi wyświetlane użytkownikom w ramach procesu zbierania atrybutów podczas tworzenia konta.

> [!NOTE]
> Jeśli używasz niestandardowych atrybutów użytkownika, musisz podać własne tłumaczenia. Aby uzyskać więcej informacji, zobacz [Dostosowywanie ciągów](#customize-your-strings).

## <a name="customize-your-strings"></a>Dostosowywanie ciągów

Dostosowanie języka umożliwia dostosowanie dowolnego ciągu w przepływie użytkownika.

1. Zaloguj się do [Azure Portal](https://portal.azure.com) jako administrator usługi Azure AD.
2. W obszarze **usługi platformy Azure**wybierz pozycję **Azure Active Directory**.
3. W menu po lewej stronie wybierz pozycję **tożsamości zewnętrzne**.
4. Wybierz pozycję **przepływy użytkownika (wersja zapoznawcza)**.
3. Wybierz przepływ użytkownika, który chcesz włączyć dla tłumaczeń.
4. Wybierz **Języki**.
5. Na stronie **Języki** dla przepływu użytkownika wybierz język, który chcesz dostosować.
6. Rozwiń **stronę kolekcji atrybutów**.
7. Wybierz pozycję **Pobierz domyślne** (lub **Pobierz zastąpienia** , jeśli wcześniej edytowano ten język).

Te kroki zapewniają plik JSON, którego można użyć do rozpoczęcia edytowania ciągów.

### <a name="change-any-string-on-the-page"></a>Zmień dowolny ciąg na stronie

1. Otwórz plik JSON pobrany z poprzednich instrukcji w edytorze JSON.
1. Znajdź element, który chcesz zmienić. Możesz znaleźć `StringId` ciąg, którego szukasz, lub poszukać `Value` atrybutu, który ma zostać zmieniony.
1. Zaktualizuj `Value` atrybut o dane, które mają być wyświetlane.
1. Dla każdego ciągu, który chcesz zmienić, przejdź `Override` do `true` .
1. Zapisz plik i przekaż zmiany. (Formant przekazywania można znaleźć w tym samym miejscu, w którym został pobrany plik JSON).

> [!IMPORTANT]
> Jeśli musisz przesłonić ciąg, upewnij się, że wartość jest ustawiona `Override` na `true` . Jeśli wartość nie zostanie zmieniona, wpis zostanie zignorowany.

### <a name="change-extension-attributes"></a>Zmień atrybuty rozszerzenia

Jeśli chcesz zmienić ciąg dla niestandardowego atrybutu użytkownika lub chcesz dodać go do pliku JSON, jest w następującym formacie:

```JSON
{
  "LocalizedStrings": [
    {
      "ElementType": "ClaimType",
      "ElementId": "extension_<ExtensionAttribute>",
      "StringId": "DisplayName",
      "Override": true,
      "Value": "<ExtensionAttributeValue>"
    }
    [...]
}
```

Zamień `<ExtensionAttribute>` na nazwę niestandardowego atrybutu użytkownika.

Zamień na `<ExtensionAttributeValue>` Nowy ciąg, który ma być wyświetlany.

### <a name="provide-a-list-of-values-by-using-localizedcollections"></a>Podaj listę wartości za pomocą LocalizedCollections

Jeśli chcesz podać listę wartości odpowiedzi, musisz utworzyć `LocalizedCollections` atrybut. `LocalizedCollections`jest tablicą `Name` par i `Value` . Kolejność dla elementów będzie kolejność, w jakiej są wyświetlane. Aby dodać `LocalizedCollections` , użyj następującego formatu:

```JSON
{
  "LocalizedStrings": [...],
  "LocalizedCollections": [{
      "ElementType":"ClaimType",
      "ElementId":"<UserAttribute>",
      "TargetCollection":"Restriction",
      "Override": true,
      "Items":[
           {
                "Name":"<Response1>",
                "Value":"<Value1>"
           },
           {
                "Name":"<Response2>",
                "Value":"<Value2>"
           }
     ]
  }]
}
```

* `ElementId`jest atrybutem użytkownika, do którego ten `LocalizedCollections` atrybut jest odpowiedzią.
* `Name`jest wartością pokazywaną użytkownikowi.
* `Value`to co jest zwracane w ramach żądania, gdy ta opcja jest zaznaczona.

### <a name="upload-your-changes"></a>Przekaż zmiany

1. Po wprowadzeniu zmian w pliku JSON Wróć do swojej dzierżawy.
1. Wybierz pozycję **przepływy użytkownika** i kliknij przepływ użytkownika, który chcesz włączyć dla tłumaczeń.
1. Wybierz **Języki**.
1. Wybierz język, w którym chcesz przeprowadzić tłumaczenie.
1. Wybierz **stronę kolekcji atrybutów**.
1. Wybierz ikonę folderu i wybierz plik JSON do przekazania.

Zmiany są zapisywane automatycznie w przepływie użytkownika.

## <a name="additional-information"></a>Dodatkowe informacje

### <a name="page-ui-customization-labels-as-overrides"></a>Etykiety dostosowywania interfejsu użytkownika strony jako zastąpienia

Po włączeniu dostosowywania języka poprzednie zmiany w etykietach korzystających z dostosowywania interfejsu użytkownika są utrwalane w pliku JSON dla języka angielskiego (pl). Można nadal zmieniać etykiety i inne ciągi przez przekazywanie zasobów języka w celu dostosowania języka.

### <a name="up-to-date-translations"></a>Aktualne tłumaczenia

Firma Microsoft dokłada starań, aby zapewnić najbardziej aktualne tłumaczenia do użycia. Firma Microsoft stale ulepsza tłumaczenia i zapewnia ich zgodność. Firma Microsoft zidentyfikuje usterki i zmiany w globalnej terminologii oraz wprowadzi aktualizacje, które bezproblemowo pracują w przepływie użytkownika.

### <a name="support-for-right-to-left-languages"></a>Obsługa języków pisanych od prawej do lewej

Firma Microsoft obecnie nie obsługuje języków pisanych od prawej do lewej. Można to zrobić za pomocą niestandardowych ustawień regionalnych i CSS, aby zmienić sposób wyświetlania ciągów. Jeśli potrzebujesz tej funkcji, zagłosuj na opinię na temat [platformy Azure](https://feedback.azure.com/forums/169401-azure-active-directory/suggestions/19393000-provide-language-support-for-right-to-left-languag).

### <a name="social-identity-provider-translations"></a>Tłumaczenia dostawcy tożsamości społecznościowej

Firma Microsoft udostępnia `ui_locales` parametr OIDC na potrzeby logowania do sieci społecznościowych. Jednak niektórzy dostawcy tożsamości społecznościowych, w tym Facebook i Google, nie honorli.

### <a name="browser-behavior"></a>Zachowanie przeglądarki

Programy Chrome i Firefox żądają swojego języka zestawu. Jeśli jest to obsługiwany język, jest on wyświetlany przed ustawieniem domyślnym. Przeglądarka Microsoft Edge obecnie nie żąda języka i przechodzi bezpośrednio do języka domyślnego.

## <a name="supported-languages"></a>Obsługiwane języki

Usługa Azure AD obejmuje obsługę następujących języków. Języki przepływu użytkownika są udostępniane przez usługę Azure AD. Języki powiadomień usługi uwierzytelniania wieloskładnikowego (MFA) są udostępniane przez [usługę Azure MFA](https://docs.microsoft.com/azure/active-directory/authentication/concept-mfa-howitworks).

| Język              | Kod języka | Przepływy użytkowników         | Powiadomienia usługi MFA  |
|-----------------------| :-----------: | :----------------: | :----------------: |
| Arabski                | ty            | ![nie](./media/user-flow-customize-language/no.png) | ![tak](./media/user-flow-customize-language/yes.png) |
| Bułgarski             | BG            | ![nie](./media/user-flow-customize-language/no.png) | ![tak](./media/user-flow-customize-language/yes.png) |
| Języku                | mld USD            | ![tak](./media/user-flow-customize-language/yes.png) | ![nie](./media/user-flow-customize-language/no.png) |
| Kataloński               | urzędu certyfikacji            | ![nie](./media/user-flow-customize-language/no.png) | ![tak](./media/user-flow-customize-language/yes.png) |
| Czeski                 | Rejestr            | ![tak](./media/user-flow-customize-language/yes.png) | ![tak](./media/user-flow-customize-language/yes.png) |
| Duński                | da            | ![tak](./media/user-flow-customize-language/yes.png) | ![tak](./media/user-flow-customize-language/yes.png) |
| Niemiecki                | de            | ![tak](./media/user-flow-customize-language/yes.png) | ![tak](./media/user-flow-customize-language/yes.png) |
| Grecki                 | Colon            | ![tak](./media/user-flow-customize-language/yes.png) | ![tak](./media/user-flow-customize-language/yes.png) |
| Angielski               | pl            | ![tak](./media/user-flow-customize-language/yes.png) | ![tak](./media/user-flow-customize-language/yes.png) |
| Hiszpański               | Tak            | ![tak](./media/user-flow-customize-language/yes.png) | ![tak](./media/user-flow-customize-language/yes.png) |
| Estoński              | staw            | ![nie](./media/user-flow-customize-language/no.png) | ![tak](./media/user-flow-customize-language/yes.png) |
| Baskijski                | Europejska            | ![nie](./media/user-flow-customize-language/no.png) | ![tak](./media/user-flow-customize-language/yes.png) |
| Fiński               | fi            | ![tak](./media/user-flow-customize-language/yes.png) | ![tak](./media/user-flow-customize-language/yes.png) |
| Francuski                | fr            | ![tak](./media/user-flow-customize-language/yes.png) | ![tak](./media/user-flow-customize-language/yes.png) |
| Galicyjski              | GL            | ![nie](./media/user-flow-customize-language/no.png) | ![tak](./media/user-flow-customize-language/yes.png) |
| Gudżarati              | gu            | ![tak](./media/user-flow-customize-language/yes.png) | ![nie](./media/user-flow-customize-language/no.png) |
| Hebrajski                | Przewodniczący            | ![nie](./media/user-flow-customize-language/no.png) | ![tak](./media/user-flow-customize-language/yes.png) |
| Hindi                 | Cześć            | ![tak](./media/user-flow-customize-language/yes.png) | ![tak](./media/user-flow-customize-language/yes.png) |
| Chorwacki              | godz.            | ![tak](./media/user-flow-customize-language/yes.png) | ![tak](./media/user-flow-customize-language/yes.png) |
| Węgierski             | Węgry            | ![tak](./media/user-flow-customize-language/yes.png) | ![tak](./media/user-flow-customize-language/yes.png) |
| Indonezyjski            | identyfikator            | ![nie](./media/user-flow-customize-language/no.png) | ![tak](./media/user-flow-customize-language/yes.png) |
| Włoski               | it            | ![tak](./media/user-flow-customize-language/yes.png) | ![tak](./media/user-flow-customize-language/yes.png) |
| Japoński              | ja            | ![tak](./media/user-flow-customize-language/yes.png) | ![tak](./media/user-flow-customize-language/yes.png) |
| Kazachski                | kk            | ![nie](./media/user-flow-customize-language/no.png) | ![tak](./media/user-flow-customize-language/yes.png) |
| Kannada               | kN            | ![tak](./media/user-flow-customize-language/yes.png) | ![nie](./media/user-flow-customize-language/no.png) |
| Koreański                | Ko            | ![tak](./media/user-flow-customize-language/yes.png) | ![tak](./media/user-flow-customize-language/yes.png) |
| Litewski            | lt            | ![nie](./media/user-flow-customize-language/no.png) | ![tak](./media/user-flow-customize-language/yes.png) |
| Łotewski               | LV            | ![nie](./media/user-flow-customize-language/no.png) | ![tak](./media/user-flow-customize-language/yes.png) |
| Malayalam             | ml            | ![tak](./media/user-flow-customize-language/yes.png) | ![nie](./media/user-flow-customize-language/no.png) |
| Marathi               | Pan            | ![tak](./media/user-flow-customize-language/yes.png) | ![nie](./media/user-flow-customize-language/no.png) |
| Malajski                 | Arial            | ![tak](./media/user-flow-customize-language/yes.png) | ![tak](./media/user-flow-customize-language/yes.png) |
| Norweski (Bokmal)      | sesja            | ![tak](./media/user-flow-customize-language/yes.png) | ![nie](./media/user-flow-customize-language/no.png) |
| Niderlandzki                 | nl            | ![tak](./media/user-flow-customize-language/yes.png) | ![tak](./media/user-flow-customize-language/yes.png) |
| Norweski             | nie            | ![nie](./media/user-flow-customize-language/no.png) | ![tak](./media/user-flow-customize-language/yes.png) |
| Pendżabski               | biur            | ![tak](./media/user-flow-customize-language/yes.png) | ![nie](./media/user-flow-customize-language/no.png) |
| Polski                | zysków            | ![tak](./media/user-flow-customize-language/yes.png) | ![tak](./media/user-flow-customize-language/yes.png) |
| Portugalski (Brazylia)   | pt-br         | ![tak](./media/user-flow-customize-language/yes.png) | ![tak](./media/user-flow-customize-language/yes.png) |
| Portugalski (Portugalia) | pt-pt         | ![tak](./media/user-flow-customize-language/yes.png) | ![tak](./media/user-flow-customize-language/yes.png) |
| Rumuński              | ro            | ![tak](./media/user-flow-customize-language/yes.png) | ![tak](./media/user-flow-customize-language/yes.png) |
| Rosyjski               | ru            | ![tak](./media/user-flow-customize-language/yes.png) | ![tak](./media/user-flow-customize-language/yes.png) |
| Słowacki                | SK            | ![tak](./media/user-flow-customize-language/yes.png) | ![tak](./media/user-flow-customize-language/yes.png) |
| Słoweński             | SL            | ![nie](./media/user-flow-customize-language/no.png) | ![tak](./media/user-flow-customize-language/yes.png) |
| Serbski – cyrylica    | Wirtualizacja SR-cryl-CS    | ![nie](./media/user-flow-customize-language/no.png) | ![tak](./media/user-flow-customize-language/yes.png) |
| Serbski – łaciński       | Wirtualizacja sr-latn-cs    | ![nie](./media/user-flow-customize-language/no.png) | ![tak](./media/user-flow-customize-language/yes.png) |
| Szwedzki               | sv            | ![tak](./media/user-flow-customize-language/yes.png) | ![tak](./media/user-flow-customize-language/yes.png) |
| Tamilski                 | Ta            | ![tak](./media/user-flow-customize-language/yes.png) | ![nie](./media/user-flow-customize-language/no.png) |
| Telugu                | Usuń            | ![tak](./media/user-flow-customize-language/yes.png) | ![nie](./media/user-flow-customize-language/no.png) |
| Tajlandzki                  | th            | ![tak](./media/user-flow-customize-language/yes.png) | ![tak](./media/user-flow-customize-language/yes.png) |
| Turecki               | zdawczy            | ![tak](./media/user-flow-customize-language/yes.png) | ![tak](./media/user-flow-customize-language/yes.png) |
| Ukraiński             | Południowe Zjednoczone Królestwo            | ![nie](./media/user-flow-customize-language/no.png) | ![tak](./media/user-flow-customize-language/yes.png) |
| Wietnamski            | VI            | ![nie](./media/user-flow-customize-language/no.png) | ![tak](./media/user-flow-customize-language/yes.png) |
| Chiński – uproszczony  | zh-Hans       | ![tak](./media/user-flow-customize-language/yes.png) | ![tak](./media/user-flow-customize-language/yes.png) |
| Chiński – tradycyjny | zh-Hant       | ![tak](./media/user-flow-customize-language/yes.png) | ![tak](./media/user-flow-customize-language/yes.png) |