---
title: Dokumentacja zestawu SDK czytnika immersyjny
titleSuffix: Azure Cognitive Services
description: Zestaw SDK czytnika immersyjny zawiera bibliotekę języka JavaScript, która umożliwia integrację czytnika immersyjny z aplikacją.
services: cognitive-services
author: metanMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: reference
ms.date: 06/20/2019
ms.author: metan
ms.openlocfilehash: 6dfcd8d56232f893f881f310b33f3f849e2364a7
ms.sourcegitcommit: 1d9f7368fa3dadedcc133e175e5a4ede003a8413
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/27/2020
ms.locfileid: "85475955"
---
# <a name="immersive-reader-javascript-sdk-reference-v11"></a>Dokumentacja zestawu SDK języka JavaScript czytnika immersyjny (v 1.1)

Zestaw SDK czytnika immersyjny zawiera bibliotekę języka JavaScript, która umożliwia integrację czytnika immersyjny z aplikacją.

## <a name="functions"></a>Funkcje

Zestaw SDK udostępnia funkcje:

- [`ImmersiveReader.launchAsync(token, subdomain, content, options)`](#launchasync)

- [`ImmersiveReader.close()`](#close)

- [`ImmersiveReader.renderButtons(options)`](#renderbuttons)

## <a name="launchasync"></a>launchAsync

Uruchamia czytnik immersyjny w `iframe` aplikacji sieci Web. Należy pamiętać, że rozmiar zawartości jest ograniczony do maksymalnie 50 MB.

```typescript
launchAsync(token: string, subdomain: string, content: Content, options?: Options): Promise<LaunchResponse>;
```

### <a name="parameters"></a>Parametry

| Nazwa | Typ | Opis |
| ---- | ---- |------------ |
| `token` | ciąg | Token uwierzytelniania usługi Azure AD. |
| `subdomain` | ciąg | Niestandardowa poddomena zasobu czytnika immersyjny na platformie Azure. |
| `content` | [Zawartość](#content) | Obiekt zawierający zawartość, która ma zostać pokazana w czytniku immersyjny. |
| `options` | [Opcje](#options) | Opcje konfigurowania niektórych zachowań czytnika immersyjny. Opcjonalny. |

### <a name="returns"></a>Zwraca

Zwraca obiekt `Promise<LaunchResponse>` , który jest rozpoznawany, gdy czytnik immersyjny jest ładowany. Jest to `Promise` rozwiązanie rozpoznawane jako [`LaunchResponse`](#launchresponse) obiekt.

### <a name="exceptions"></a>Wyjątki

Zwracana wartość `Promise` zostanie odrzucona z [`Error`](#error) obiektem, jeśli czytnik immersyjny nie zostanie załadowany. Aby uzyskać więcej informacji, zobacz [kody błędów](#error-codes).

## <a name="close"></a>close

Zamyka czytnik immersyjny.

Przykładowy przypadek użycia tej funkcji to jeśli przycisk Zakończ jest ukryty przez ustawienie ```hideExitButton: true``` w [opcjach](#options). Następnie inny przycisk (na przykład strzałka wstecz nagłówka komórkowego) może wywołać tę ```close``` funkcję, gdy zostanie kliknięty.

```typescript
close(): void;
```

## <a name="renderbuttons"></a>renderButtons

Ta funkcja stylów i aktualizuje elementy przycisku czytnika immersyjny dokumentu. Jeśli ```options.elements``` jest podany, ta funkcja będzie renderować przyciski w ```options.elements``` . W przeciwnym razie przyciski będą renderowane w obrębie elementów dokumentu, które mają klasę ```immersive-reader-button``` .

Ta funkcja jest automatycznie wywoływana przez zestaw SDK podczas ładowania okna.

Zobacz [atrybuty opcjonalne](#optional-attributes) , aby uzyskać więcej opcji renderowania.

```typescript
renderButtons(options?: RenderButtonsOptions): void;
```

### <a name="parameters"></a>Parametry

| Nazwa | Typ | Opis |
| ---- | ---- |------------ |
| `options` | [RenderButtonsOptions](#renderbuttonsoptions) | Opcje konfigurowania niektórych zachowań funkcji renderButtons. Opcjonalny. |

## <a name="types"></a>Typy

### <a name="content"></a>Zawartość

Zawiera zawartość, która ma zostać pokazana w czytniku immersyjny.

```typescript
{
    title?: string;    // Title text shown at the top of the Immersive Reader (optional)
    chunks: Chunk[];   // Array of chunks
}
```

### <a name="chunk"></a>Fragment

Pojedynczy fragment danych, który zostanie przesłany do zawartości czytnika immersyjny.

```typescript
{
    content: string;        // Plain text string
    lang?: string;          // Language of the text, e.g. en, es-ES (optional). Language will be detected automatically if not specified.
    mimeType?: string;      // MIME type of the content (optional). Currently 'text/plain', 'application/mathml+xml', and 'text/html' are supported. Defaults to 'text/plain' if not specified.
}
```

#### <a name="supported-mime-types"></a>Obsługiwane typy MIME

| Typ MIME | Opis |
| --------- | ----------- |
| tekst/zwykły | Zwykły tekst. |
| text/html | Zawartość HTML. [Dowiedz się więcej](#html-support)|
| Application/MathML + XML | Język matematycznych znaczników (MathML). [Dowiedz się więcej](./how-to/display-math.md).
| ument aplikacji/vnd.openxmlformats-officedocument.wordprocessingml.doc | Dokument formatu programu Microsoft Word. docx.

### <a name="options"></a>Opcje

Zawiera właściwości, które konfigurują pewne zachowania czytnika immersyjny.

```typescript
{
    uiLang?: string;           // Language of the UI, e.g. en, es-ES (optional). Defaults to browser language if not specified.
    timeout?: number;          // Duration (in milliseconds) before launchAsync fails with a timeout error (default is 15000 ms).
    uiZIndex?: number;         // Z-index of the iframe that will be created (default is 1000).
    useWebview?: boolean;      // Use a webview tag instead of an iframe, for compatibility with Chrome Apps (default is false).
    onExit?: () => any;        // Executes when the Immersive Reader exits.
    customDomain?: string;     // Reserved for internal use. Custom domain where the Immersive Reader webapp is hosted (default is null).
    allowFullscreen?: boolean; // The ability to toggle fullscreen (default is true).
    hideExitButton?: boolean;  // Whether or not to hide the Immersive Reader's exit button arrow (default is false). This should only be true if there is an alternative mechanism provided to exit the Immersive Reader (e.g a mobile toolbar's back arrow).
    cookiePolicy?: CookiePolicy; // Setting for the Immersive Reader's cookie usage (default is CookiePolicy.Disable). It's the responsibility of the host application to obtain any necessary user consent in accordance with EU Cookie Compliance Policy.
    disableFirstRun?: boolean; // Disable the first run experience.
    readAloudOptions?: ReadAloudOptions; // Options to configure Read Aloud.
    translationOptions?: TranslationOptions; // Options to configure translation.
    displayOptions?: DisplayOptions; // Options to configure text size, font, etc.
    preferences?: string; // String returned from onPreferencesChanged representing the user's preferences in the Immersive Reader.
    onPreferencesChanged?: (value: string) => any; // Executes when the user's preferences have changed.
}
```

```typescript
enum CookiePolicy { Disable, Enable }
```

```typescript
type ReadAloudOptions = {
    voice?: string;      // Voice, either 'male' or 'female'. Note that not all languages support both genders.
    speed?: number;      // Playback speed, must be between 0.5 and 2.5, inclusive.
    autoplay?: boolean;  // Automatically start Read Aloud when the Immersive Reader loads.
};
```

> [!NOTE]
> Ze względu na ograniczenia przeglądarki funkcja autoodtwarzania nie jest obsługiwana w przeglądarce Safari.

```typescript
type TranslationOptions = {
    language: string;                         // Set the translation language, e.g. fr-FR, es-MX, zh-Hans-CN. Required to automatically enable word or document translation.
    autoEnableDocumentTranslation?: boolean;  // Automatically translate the entire document.
    autoEnableWordTranslation?: boolean;      // Automatically enable word translation.
};
```

```typescript
type DisplayOptions = {
    textSize?: number;          // Valid values are 14, 20, 28, 36, 42, 48, 56, 64, 72, 84, 96.
    increaseSpacing?: boolean;  // Set whether increased spacing is enabled.
    fontFamily?: string;        // Valid values are 'Calibri', 'ComicSans', and 'Sitka'.
};
```

### <a name="launchresponse"></a>LaunchResponse

Zawiera odpowiedź z wywołania do `ImmersiveReader.launchAsync` . Należy zauważyć, że odwołanie do programu zawierającego `iframe` czytnik immersyjny jest dostępne za pośrednictwem `container.firstChild` .

```typescript
{
    container: HTMLDivElement;    // HTML element which contains the Immersive Reader iframe
    sessionId: string;            // Globally unique identifier for this session, used for debugging
}
```
 
### <a name="error"></a>Błąd

Zawiera informacje o błędzie.

```typescript
{
    code: string;    // One of a set of error codes
    message: string; // Human-readable representation of the error
}
```

#### <a name="error-codes"></a>Kody błędów

| Kod | Opis |
| ---- | ----------- |
| BadArgument | Podany argument jest nieprawidłowy `message` . Aby uzyskać szczegółowe informacje, zobacz. |
| Limit czasu | Nie można załadować czytnika immersyjny w określonym limicie czasu. |
| TokenExpired | Podany token wygasł. |
| Ograniczone | Przekroczono limit liczby wywołań. |

### <a name="renderbuttonsoptions"></a>RenderButtonsOptions

Opcje renderowania przycisków czytnika szczegółowego.

```typescript
{
    elements: HTMLDivElement[];    // Elements to render the Immersive Reader buttons in
}
```

## <a name="launching-the-immersive-reader"></a>Uruchamianie czytnika immersyjny

Zestaw SDK zawiera domyślne style dla przycisku umożliwiającego uruchomienie czytnika immersyjny. Użyj `immersive-reader-button` atrybutu Class, aby włączyć ten styl. Aby uzyskać więcej informacji, zobacz [ten artykuł](./how-to-customize-launch-button.md) .

```html
<div class='immersive-reader-button'></div>
```

### <a name="optional-attributes"></a>Atrybuty opcjonalne

Użyj następujących atrybutów, aby skonfigurować wygląd i działanie przycisku.

| Atrybut | Opis |
| --------- | ----------- |
| `data-button-style` | Ustawia styl przycisku. Może być `icon` , `text` lub `iconAndText` . Wartość domyślna to `icon` . |
| `data-locale` | Ustawia ustawienia regionalne. Na przykład: `en-US` lub `fr-FR`. Domyślnie jest używany język angielski `en` . |
| `data-icon-px-size` | Ustawia rozmiar ikony w pikselach. Wartość domyślna to 20px. |

## <a name="html-support"></a>Obsługa HTML

| HTML | Obsługiwana zawartość |
| --------- | ----------- |
| Style czcionki | Pogrubienie, kursywa, podkreślenie, kod, przekreślenie, indeks górny, dolny indeks |
| Listy nieuporządkowane | Dysk, okrąg, kwadrat |
| Uporządkowane listy | Dziesiętny, górny, Dolny, niższy niż alfa, wielkie litery, małe litery |

Nieobsługiwane Tagi będą renderowane w sposób porównywalny. Obrazy i tabele nie są obecnie obsługiwane.

## <a name="browser-support"></a>Obsługa przeglądarki

Najnowsze wersje następujących przeglądarek są używane w celu uzyskania najlepszego środowiska z czytnikiem immersyjny.

* Microsoft Edge
* Internet Explorer 11
* Google Chrome
* Mozilla Firefox
* Apple Safari

## <a name="next-steps"></a>Następne kroki

* Eksplorowanie [zestawu SDK czytnika immersyjny w witrynie GitHub](https://github.com/microsoft/immersive-reader-sdk)
* [Szybki Start: Tworzenie aplikacji sieci Web, która uruchamia czytnik immersyjny (C#)](./quickstarts/client-libraries.md?pivots=programming-language-csharp)
