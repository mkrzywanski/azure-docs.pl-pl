---
title: Microsoft Identity platform Android — szybki start | Azure
description: Dowiedz się, jak aplikacje dla systemu Android mogą wywołać interfejs API, który wymaga tokenów dostępu w punkcie końcowym platformy tożsamości firmy Microsoft.
services: active-directory
author: mmacy
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: quickstart
ms.workload: identity
ms.date: 10/15/2019
ms.author: marsma
ms.custom: aaddev, identityplatformtop40, scenarios:getting-started, languages:Android
ms.openlocfilehash: 9afb5b7602b220c25d919f8fe0773d5cfa143d89
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/29/2020
ms.locfileid: "80991198"
---
# <a name="quickstart-sign-in-users-and-call-the-microsoft-graph-api-from-an-android-app"></a>Szybki start: logowanie użytkowników i wywoływanie interfejsu API programu Microsoft Graph z poziomu aplikacji dla systemu Android

Ten przewodnik Szybki Start używa przykładu kodu, aby zademonstrować, jak aplikacja systemu Android może zalogować się do konta osobistego, służbowego lub szkolnego za pomocą platformy tożsamości firmy Microsoft, a następnie uzyskać token dostępu i wywołać interfejs API Microsoft Graph. (Zobacz [, jak działa przykład](#how-the-sample-works) dla ilustracji).

Aplikacje muszą być reprezentowane przez obiekt aplikacji w Azure Active Directory, aby platforma tożsamości firmy Microsoft mogła udostępniać tokeny aplikacji.

> [!div renderon="docs"]
> Jako wygoda, przykładowy kod zawiera domyślne `redirect_uri` wstępnie skonfigurowane w `AndroidManifest.xml` pliku, aby nie trzeba było najpierw zarejestrować własnego obiektu aplikacji. `redirect_uri` Jest częściowo oparta na kluczu podpisywania Twojej aplikacji. Przykładowy projekt jest wstępnie skonfigurowany przy użyciu klucza podpisywania, aby zapewnić, że dane `redirect_uri` będą działały. Aby dowiedzieć się więcej na temat rejestrowania obiektu aplikacji i integrowania go z aplikacją, zapoznaj się z samouczkiem [Logowanie użytkowników i Wywołaj Microsoft Graph z aplikacji dla systemu Android](tutorial-v2-android.md) .


> [!NOTE]
> **Wymagania wstępne**
> * Android Studio 
> * System Android 16 +

> [!div class="sxs-lookup" renderon="portal"]
> ### <a name="step-1-configure-your-application-in-the-azure-portal"></a>Krok 1. Konfigurowanie aplikacji w witrynie Azure Portal 
>  Aby przykład kodu dla tego przewodnika Szybki Start działał, należy dodać identyfikator URI przekierowania zgodny z brokerem uwierzytelniania.
> > [!div renderon="portal" id="makechanges" class="nextstepaction"]
> > [Wprowadź zmiany automatycznie]()
>
> > [!div id="appconfigured" class="alert alert-info"]
> > ![Already configured](media/quickstart-v2-android/green-check.png) (Już skonfigurowano) Twoja aplikacja została skonfigurowana za pomocą tych atrybutów
>
> ### <a name="step-2-download-the-project"></a>Krok 2. Pobieranie projektu 
> [!div class="sxs-lookup" renderon="portal"]
> Uruchom projekt przy użyciu Android Studio.
> [!div renderon="portal" id="autoupdate" class="nextstepaction"]
> [Pobierz przykład kodu](https://github.com/Azure-Samples/ms-identity-android-java/archive/master.zip)
>
> [!div class="sxs-lookup" renderon="portal"]
> ### <a name="step-3-your-app-is-configured-and-ready-to-run"></a>Krok 3. Twoja aplikacja jest skonfigurowana i gotowa do uruchomienia
> Twój projekt został skonfigurowany z wartościami właściwości aplikacji i jest gotowy do uruchomienia. 
> Przykładowa aplikacja zostanie uruchomiona na ekranie **trybu pojedynczego konta** . Domyślny zakres, **User. Read**, jest dostarczany domyślnie, który jest używany podczas odczytywania własnych danych profilu podczas wywołania interfejsu API Microsoft Graph. Adres URL wywołania interfejsu API Microsoft Graph jest udostępniany domyślnie. Jeśli chcesz, możesz zmienić oba te elementy.
>
> ![Przykładowa aplikacja MSAL przedstawiająca użycie jednego i wielu kont](./media/quickstart-v2-android/quickstart-sample-app.png)
>
> Użyj menu aplikacji do zmiany między trybami jednego i wielu kont.
>
> W trybie jednego konta zaloguj się przy użyciu konta służbowego:
>
> 1. Wybierz pozycję **Pobierz dane grafu interaktywnie** , aby monitować użytkownika o poświadczenia. Wyniki wywołania interfejsu API Microsoft Graph będą widoczne w dolnej części ekranu.
> 2. Po zalogowaniu wybierz pozycję **Pobierz dane grafu w trybie dyskretnym** , aby wywołać interfejs API Microsoft Graph bez monitowania użytkownika o podanie poświadczeń ponownie. Wyniki wywołania interfejsu API Microsoft Graph będą widoczne w dolnej części ekranu.
>
> W trybie wielu kont można powtórzyć te same czynności.  Ponadto możesz usunąć zalogowane konto, co spowoduje również usunięcie buforowanych tokenów dla tego konta.

> [!div class="sxs-lookup" renderon="portal"]
> > [!NOTE]
> > `Enter_the_Supported_Account_Info_Here`

> [!div renderon="docs"]
> ## <a name="step-1-get-the-sample-app"></a>Krok 1. Uzyskiwanie przykładowej aplikacji
>
> [Pobierz kod](https://github.com/Azure-Samples/ms-identity-android-java/archive/master.zip).
>
> ## <a name="step-2-run-the-sample-app"></a>Krok 2. Uruchamianie przykładowej aplikacji
>
> Wybierz emulator lub urządzenie fizyczne, z listy rozwijanej **dostępne urządzenia** Android Studio i uruchom aplikację.
>
> Przykładowa aplikacja zostanie uruchomiona na ekranie **trybu pojedynczego konta** . Domyślny zakres, **User. Read**, jest dostarczany domyślnie, który jest używany podczas odczytywania własnych danych profilu podczas wywołania interfejsu API Microsoft Graph. Adres URL wywołania interfejsu API Microsoft Graph jest udostępniany domyślnie. Jeśli chcesz, możesz zmienić oba te elementy.
>
> ![Przykładowa aplikacja MSAL przedstawiająca użycie jednego i wielu kont](./media/quickstart-v2-android/quickstart-sample-app.png)
>
> Użyj menu aplikacji do zmiany między trybami jednego i wielu kont.
>
> W trybie jednego konta zaloguj się przy użyciu konta służbowego:
>
> 1. Wybierz pozycję **Pobierz dane grafu interaktywnie** , aby monitować użytkownika o poświadczenia. Wyniki wywołania interfejsu API Microsoft Graph będą widoczne w dolnej części ekranu.
> 2. Po zalogowaniu wybierz pozycję **Pobierz dane grafu w trybie dyskretnym** , aby wywołać interfejs API Microsoft Graph bez monitowania użytkownika o podanie poświadczeń ponownie. Wyniki wywołania interfejsu API Microsoft Graph będą widoczne w dolnej części ekranu.
>
> W trybie wielu kont można powtórzyć te same czynności.  Ponadto możesz usunąć zalogowane konto, co spowoduje również usunięcie buforowanych tokenów dla tego konta.

## <a name="how-the-sample-works"></a>Jak działa przykład
![Zrzut ekranu przedstawiający przykładową aplikację](media/quickstart-v2-android/android-intro.svg)


Kod jest zorganizowany w fragmenty, które pokazują, jak napisać jedną i wiele kont aplikacji MSAL. Pliki kodu są zorganizowane w następujący sposób:

| Plik  | Demonstracje  |
|---------|---------|
| MainActivity | Zarządza interfejsem użytkownika |
| MSGraphRequestWrapper  | Wywołuje interfejs API Microsoft Graph przy użyciu tokenu dostarczonego przez MSAL |
| MultipleAccountModeFragment  | Inicjuje aplikację wielokontową, ładuje konto użytkownika i pobiera token do wywołania interfejsu API Microsoft Graph |
| SingleAccountModeFragment | Inicjuje aplikację pojedynczego konta, ładuje konto użytkownika i pobiera token do wywołania interfejsu API Microsoft Graph |
| res/auth_config_multiple_account. JSON  | Plik konfiguracji z wieloma kontami |
| res/auth_config_single_account. JSON  | Plik konfiguracji pojedynczego konta |
| Gradle scripts/Build. klasy (moduł: App) | Zależności biblioteki MSAL są tutaj dodawane |

Teraz będziemy bardziej szczegółowo przeglądać te pliki i wywoływać kod specyficzny dla MSAL w każdym z nich.

### <a name="adding-msal-to-the-app"></a>Dodawanie MSAL do aplikacji

MSAL ([com. Microsoft. Identity. Client](https://javadoc.io/doc/com.microsoft.identity.client/msal)) to biblioteka służąca do logowania użytkowników i żądania tokenów używanych w celu uzyskania dostępu do interfejsu API chronionego przez platformę tożsamości firmy Microsoft. Gradle 3.0 + instaluje bibliotekę po dodaniu następującej do **Gradle skryptów** > **Build. Gradle (module: App)** w obszarze **zależności**:

```gradle  
implementation 'com.microsoft.identity.client:msal:1.+'
```

Można to zobaczyć w przykładowym projekcie w Build. Gradle (moduł: aplikacja):

```java
dependencies {
    ...
    implementation 'com.microsoft.identity.client:msal:1.+'
    ...
}
```

Powoduje to, że Gradle pobrać i skompilować MSAL z Maven Central.

### <a name="msal-imports"></a>MSAL Importy

Importy istotne dla biblioteki MSAL są `com.microsoft.identity.client.*`.  Na przykład zobaczysz, `import com.microsoft.identity.client.PublicClientApplication;` która jest przestrzenią nazw dla `PublicClientApplication` klasy, która reprezentuje publiczną aplikację kliencką.

### <a name="singleaccountmodefragmentjava"></a>SingleAccountModeFragment. Java

Ten plik pokazuje, jak utworzyć aplikację MSAL z jednym kontem i wywołać interfejs API Microsoft Graph.

Aplikacje pojedynczego konta są używane tylko przez jednego użytkownika.  Na przykład możesz mieć tylko jedno konto, za pomocą którego możesz zalogować się do aplikacji mapowania.

#### <a name="single-account-msal-initialization"></a>Inicjalizacja MSAL jednego konta

W `auth_config_single_account.json`programie w `onCreateView()`programie jest tworzone pojedyncze `PublicClientApplication` konto przy użyciu informacji konfiguracyjnych przechowywanych w `auth_config_single_account.json` pliku.  W ten sposób można zainicjować bibliotekę MSAL do użycia w aplikacji MSAL pojedynczego konta:

```java
...
// Creates a PublicClientApplication object with res/raw/auth_config_single_account.json
PublicClientApplication.createSingleAccountPublicClientApplication(getContext(),
        R.raw.auth_config_single_account,
        new IPublicClientApplication.ISingleAccountApplicationCreatedListener() {
            @Override
            public void onCreated(ISingleAccountPublicClientApplication application) {
                /**
                 * This test app assumes that the app is only going to support one account.
                 * This requires "account_mode" : "SINGLE" in the config json file.
                 **/
                mSingleAccountApp = application;
                loadAccount();
            }

            @Override
            public void onError(MsalException exception) {
                displayError(exception);
            }
        });
```

#### <a name="sign-in-a-user"></a>Zaloguj użytkownika

W `SingleAccountModeFragment.java`programie kod służący do logowania użytkownika znajduje się `initializeUI()`w programie obsługi `signInButton` kliknij.

Wywołaj `signIn()` przed próbą uzyskania tokenów. `signIn()`zachowuje się tak, `acquireToken()` jakby jest wywoływana, co spowoduje wyświetlenie interakcyjnego monitu użytkownika o zalogowanie się.

Logowanie użytkownika jest operacją asynchroniczną. Jest przesyłane wywołanie zwrotne, które wywołuje interfejs API Microsoft Graph i aktualizuje interfejs użytkownika po zalogowaniu się użytkownika:

```java
mSingleAccountApp.signIn(getActivity(), null, getScopes(), getAuthInteractiveCallback());
```

#### <a name="sign-out-a-user"></a>Wylogowywanie użytkownika

W `SingleAccountModeFragment.java`programie kod do wylogowania użytkownika `initializeUI()`należy do programu obsługi `signOutButton` kliknij.  Podpisywanie użytkownika jest operacją asynchroniczną. Podpisywanie użytkownika spowoduje również wyczyszczenie pamięci podręcznej tokenów dla tego konta. Zostanie utworzone wywołanie zwrotne, aby zaktualizować interfejs użytkownika po wylogowaniu się konta użytkownika:

```java
mSingleAccountApp.signOut(new ISingleAccountPublicClientApplication.SignOutCallback() {
    @Override
    public void onSignOut() {
        updateUI(null);
        performOperationOnSignOut();
    }

    @Override
    public void onError(@NonNull MsalException exception) {
        displayError(exception);
    }
});
```

#### <a name="get-a-token-interactively-or-silently"></a>Uzyskaj token interaktywnie lub w trybie dyskretnym

Aby przedstawić najmniejszą liczbę monitów dla użytkownika, zazwyczaj otrzymujesz token w trybie dyskretnym. Następnie, jeśli wystąpi błąd, spróbuj uzyskać dostęp do tokenu interaktywnie. Gdy aplikacja jest wywoływana `signIn()`po raz pierwszy, efektywnie działa jako wywołanie `acquireToken()`, co spowoduje wyświetlenie monitu o podanie poświadczeń użytkownika.

Niektóre sytuacje, w których użytkownik może zostać poproszony o wybranie konta, wprowadzenie ich poświadczeń lub zgoda na uprawnienia wymagane przez aplikację:

* Przy pierwszym logowaniu użytkownika do aplikacji
* Jeśli użytkownik resetuje hasło, musi wprowadzić swoje poświadczenia
* Jeśli wyrażanie zgody zostało odwołane
* Jeśli aplikacja jawnie wymaga zgody
* Gdy aplikacja żąda dostępu do zasobu po raz pierwszy
* Gdy wymagane jest uwierzytelnianie wieloskładnikowe lub inne zasady dostępu warunkowego

Kod umożliwiający interaktywny dostęp do tokenu, który jest z INTERFEJSem użytkownika, który będzie obejmował użytkownika, znajduje `SingleAccountModeFragment.java`się w `initializeUI()`, w, `callGraphApiInteractiveButton` w, w programie obsługi kliknij:

```java
/**
 * If acquireTokenSilent() returns an error that requires an interaction (MsalUiRequiredException),
 * invoke acquireToken() to have the user resolve the interrupt interactively.
 *
 * Some example scenarios are
 *  - password change
 *  - the resource you're acquiring a token for has a stricter set of requirement than your Single Sign-On refresh token.
 *  - you're introducing a new scope which the user has never consented for.
 **/
mSingleAccountApp.acquireToken(getActivity(), getScopes(), getAuthInteractiveCallback());
```

Jeśli użytkownik jest już zalogowany, `acquireTokenSilentAsync()` umożliwia aplikacjom zażądanie tokenów dyskretnie, jak `initializeUI()`pokazano w, `callGraphApiSilentButton` w programie obsługi kliknij:

```java
/**
 * Once you've signed the user in,
 * you can perform acquireTokenSilent to obtain resources without interrupting the user.
 **/
  mSingleAccountApp.acquireTokenSilentAsync(getScopes(), AUTHORITY, getAuthSilentCallback());
```

#### <a name="load-an-account"></a>Załaduj konto

Kod do załadowania konta znajduje się w `SingleAccountModeFragment.java` programie `loadAccount()`.  Załadowanie konta użytkownika jest operacją asynchroniczną, więc wywołania zwrotne do obsłużenia, gdy następuje załadowanie konta, zmiany lub błąd, są przesyłane do MSAL.  Poniższy kod obsługuje `onAccountChanged()`również, który występuje po usunięciu konta, zmieni się na inne konto i tak dalej.

```java
private void loadAccount() {
    ...

    mSingleAccountApp.getCurrentAccountAsync(new ISingleAccountPublicClientApplication.CurrentAccountCallback() {
        @Override
        public void onAccountLoaded(@Nullable IAccount activeAccount) {
            // You can use the account data to update your UI or your app database.
            updateUI(activeAccount);
        }

        @Override
        public void onAccountChanged(@Nullable IAccount priorAccount, @Nullable IAccount currentAccount) {
            if (currentAccount == null) {
                // Perform a cleanup task as the signed-in account changed.
                performOperationOnSignOut();
            }
        }

        @Override
        public void onError(@NonNull MsalException exception) {
            displayError(exception);
        }
    });
```

#### <a name="call-microsoft-graph"></a>Microsoft Graph wywołania

Gdy użytkownik jest zalogowany, wywołanie do Microsoft Graph jest nawiązywane za pośrednictwem żądania HTTP, `callGraphAPI()` przez który jest zdefiniowany `SingleAccountModeFragment.java`w. Ta funkcja to otoka, która upraszcza przykład, wykonując pewne zadania, takie jak pobieranie tokenu dostępu z `authenticationResult` i pakowanie wywołania do MSGraphRequestWrapper i wyświetlanie wyników wywołania.

```java
private void callGraphAPI(final IAuthenticationResult authenticationResult) {
    MSGraphRequestWrapper.callGraphAPIUsingVolley(
            getContext(),
            graphResourceTextView.getText().toString(),
            authenticationResult.getAccessToken(),
            new Response.Listener<JSONObject>() {
                @Override
                public void onResponse(JSONObject response) {
                    /* Successfully called graph, process data and send to UI */
                    ...
                }
            },
            new Response.ErrorListener() {
                @Override
                public void onErrorResponse(VolleyError error) {
                    ...
                }
            });
}
```

### <a name="auth_config_single_accountjson"></a>plik auth_config_single_account. JSON

Jest to plik konfiguracji aplikacji MSAL, który używa jednego konta.

Wyjaśnienie tych pól można znaleźć w temacie [Omówienie pliku konfiguracji MSAL systemu Android](msal-configuration.md) .

Zanotuj obecność `"account_mode" : "SINGLE"`, która konfiguruje tę aplikację w taki sposób, aby korzystała z jednego konta.

`"client_id"`jest wstępnie skonfigurowana do korzystania z rejestracji obiektu aplikacji obsługiwanej przez firmę Microsoft.
`"redirect_uri"`jest wstępnie skonfigurowany do korzystania z klucza podpisywania dostarczonego z przykładem kodu.

```json
{
  "client_id" : "0984a7b6-bc13-4141-8b0d-8f767e136bb7",
  "authorization_user_agent" : "DEFAULT",
  "redirect_uri" : "msauth://com.azuresamples.msalandroidapp/1wIqXSqBj7w%2Bh11ZifsnqwgyKrY%3D",
  "account_mode" : "SINGLE",
  "broker_redirect_uri_registered": true,
  "authorities" : [
    {
      "type": "AAD",
      "audience": {
        "type": "AzureADandPersonalMicrosoftAccount",
        "tenant_id": "common"
      }
    }
  ]
}
```

### <a name="multipleaccountmodefragmentjava"></a>MultipleAccountModeFragment. Java

Ten plik pokazuje, jak utworzyć aplikację MSAL z wieloma kontami i wywołać interfejs API Microsoft Graph.

Przykładem aplikacji z wieloma kontami jest aplikacja poczty, która umożliwia współdziałanie z wieloma kontami użytkowników, takimi jak konto służbowe i konto osobiste.

#### <a name="multiple-account-msal-initialization"></a>Inicjowanie wielu kont MSAL

W `MultipleAccountModeFragment.java` pliku w programie `onCreateView()`zostanie utworzony obiekt aplikacji z wieloma kontami`IMultipleAccountPublicClientApplication`() przy użyciu informacji konfiguracyjnych przechowywanych w `auth_config_multiple_account.json file`:

```java
// Creates a PublicClientApplication object with res/raw/auth_config_multiple_account.json
PublicClientApplication.createMultipleAccountPublicClientApplication(getContext(),
        R.raw.auth_config_multiple_account,
        new IPublicClientApplication.IMultipleAccountApplicationCreatedListener() {
            @Override
            public void onCreated(IMultipleAccountPublicClientApplication application) {
                mMultipleAccountApp = application;
                loadAccounts();
            }

            @Override
            public void onError(MsalException exception) {
                ...
            }
        });
```

Utworzony `MultipleAccountPublicClientApplication` obiekt jest przechowywany w zmiennej składowej klasy, dzięki czemu może służyć do współdziałania z biblioteką MSAL w celu uzyskania tokenów i załadowania i usunięcia konta użytkownika.

#### <a name="load-an-account"></a>Załaduj konto

Aplikacje z wieloma kontami `getAccounts()` zwykle są wywoływane w celu wybrania konta do użycia dla operacji MSAL. Kod do załadowania konta znajduje się w `MultipleAccountModeFragment.java` pliku. `loadAccounts()`  Ładowanie konta użytkownika jest operacją asynchroniczną. Wywołanie zwrotne obsługuje sytuacje, w których konto jest załadowane, zmienia się lub występuje błąd.

```java
/**
 * Load currently signed-in accounts, if there's any.
 **/
private void loadAccounts() {
    if (mMultipleAccountApp == null) {
        return;
    }

    mMultipleAccountApp.getAccounts(new IPublicClientApplication.LoadAccountsCallback() {
        @Override
        public void onTaskCompleted(final List<IAccount> result) {
            // You can use the account data to update your UI or your app database.
            accountList = result;
            updateUI(accountList);
        }

        @Override
        public void onError(MsalException exception) {
            displayError(exception);
        }
    });
}
```

#### <a name="get-a-token-interactively-or-silently"></a>Uzyskaj token interaktywnie lub w trybie dyskretnym

Niektóre sytuacje, w których użytkownik może zostać poproszony o wybranie konta, wprowadzenie ich poświadczeń lub zgoda na uprawnienia wymagane przez aplikację:

* Gdy nowi użytkownicy logują się do aplikacji po raz pierwszy.
* Jeśli użytkownik resetuje hasło, musi wprowadzić swoje poświadczenia 
* Jeśli wyrażanie zgody zostało odwołane 
* Jeśli aplikacja jawnie wymaga zgody 
* Gdy aplikacja żąda dostępu do zasobu po raz pierwszy
* Gdy wymagane jest uwierzytelnianie wieloskładnikowe lub inne zasady dostępu warunkowego

Wiele aplikacji konta powinna zwykle uzyskiwać tokeny interaktywnie, czyli interfejsem użytkownika, który obejmuje użytkownika, z wywołaniem do `acquireToken()`.  Kod umożliwiający interaktywne uzyskanie tokenu znajduje się w `MultipleAccountModeFragment.java` pliku w programie `initializeUI()`obsługi `callGraphApiInteractiveButton` kliknij:

```java
/**
 * Acquire token interactively. It will also create an account object for the silent call as a result (to be obtained by getAccount()).
 *
 * If acquireTokenSilent() returns an error that requires an interaction,
 * invoke acquireToken() to have the user resolve the interrupt interactively.
 *
 * Some example scenarios are
 *  - password change
 *  - the resource you're acquiring a token for has a stricter set of requirement than your SSO refresh token.
 *  - you're introducing a new scope which the user has never consented for.
 **/
mMultipleAccountApp.acquireToken(getActivity(), getScopes(), getAuthInteractiveCallback());
```

Aplikacje nie powinny wymagać od użytkownika logowania się za każdym razem, gdy żądają tokenu. Jeśli użytkownik `acquireTokenSilentAsync()` jest już zalogowany, umożliwia aplikacjom żądanie tokenów bez monitowania użytkownika, jak pokazano w `MultipleAccountModeFragment.java` pliku, w`initializeUI()` obszarze procedury obsługi `callGraphApiSilentButton` kliknij:

```java
/**
 * Performs acquireToken without interrupting the user.
 *
 * This requires an account object of the account you're obtaining a token for.
 * (can be obtained via getAccount()).
 */
mMultipleAccountApp.acquireTokenSilentAsync(getScopes(),
    accountList.get(accountListSpinner.getSelectedItemPosition()),
    AUTHORITY,
    getAuthSilentCallback());
```

#### <a name="remove-an-account"></a>Usuwanie konta

Kod służący do usuwania konta i wszystkie tokeny w pamięci podręcznej dla konta, `MultipleAccountModeFragment.java` znajduje się `initializeUI()` w pliku w programie obsługi dla przycisku Usuń konto. Aby można było usunąć konto, wymagany jest obiekt konta, który uzyskano z metod MSAL, takich jak `getAccounts()` i `acquireToken()`. Ponieważ usunięcie konta jest operacją asynchroniczną, podano `onRemoved` wywołanie zwrotne w celu zaktualizowania interfejsu użytkownika.

```java
/**
 * Removes the selected account and cached tokens from this app (or device, if the device is in shared mode).
 **/
mMultipleAccountApp.removeAccount(accountList.get(accountListSpinner.getSelectedItemPosition()),
        new IMultipleAccountPublicClientApplication.RemoveAccountCallback() {
            @Override
            public void onRemoved() {
                ...
                /* Reload account asynchronously to get the up-to-date list. */
                loadAccounts();
            }

            @Override
            public void onError(@NonNull MsalException exception) {
                displayError(exception);
            }
        });
```

### <a name="auth_config_multiple_accountjson"></a>plik auth_config_multiple_account. JSON

Jest to plik konfiguracji aplikacji MSAL, który używa wielu kont.

Aby uzyskać wyjaśnienie różnych pól, zobacz [Opis pliku konfiguracji MSAL systemu Android](msal-configuration.md) .

W przeciwieństwie do pliku konfiguracji [auth_config_single_account. JSON](#auth_config_single_accountjson) , ten plik konfiguracji `"account_mode" : "MULTIPLE"` jest w `"account_mode" : "SINGLE"` zamian, ponieważ jest to aplikacja z wieloma kontami.

`"client_id"`jest wstępnie skonfigurowana do korzystania z rejestracji obiektu aplikacji obsługiwanej przez firmę Microsoft.
`"redirect_uri"`jest wstępnie skonfigurowany do korzystania z klucza podpisywania dostarczonego z przykładem kodu.

```json
{
  "client_id" : "0984a7b6-bc13-4141-8b0d-8f767e136bb7",
  "authorization_user_agent" : "DEFAULT",
  "redirect_uri" : "msauth://com.azuresamples.msalandroidapp/1wIqXSqBj7w%2Bh11ZifsnqwgyKrY%3D",
  "account_mode" : "MULTIPLE",
  "broker_redirect_uri_registered": true,
  "authorities" : [
    {
      "type": "AAD",
      "audience": {
        "type": "AzureADandPersonalMicrosoftAccount",
        "tenant_id": "common"
      }
    }
  ]
}
```

## <a name="next-steps"></a>Następne kroki

### <a name="learn-the-steps-to-create-the-application-used-in-this-quickstart"></a>Dowiedz się więcej na temat czynności, które są wymagane to utworzenia aplikacji użytej w tym przewodniku Szybki start

Wypróbuj [użytkowników logowania i Wywołaj Microsoft Graph z samouczka aplikacji dla systemu Android](tutorial-v2-android.md) , aby zapoznać się z przewodnikiem krok po kroku tworzenia aplikacji systemu Android, która pobiera token dostępu i używa jej do wywoływania interfejsu API Microsoft Graph.

> [!div class="nextstepaction"]
> [Samouczek: wywołanie interfejsu API programu Graph dla systemu Android](https://docs.microsoft.com/azure/active-directory/develop/guidedsetups/active-directory-android)

### <a name="msal-for-android-library-wiki"></a>Strona typu wiki: biblioteka MSAL dla systemu Android

Przeczytaj więcej informacji na temat biblioteki MSAL dla systemu Android:

> [!div class="nextstepaction"]
> [Strona typu wiki: biblioteka MSAL dla systemu Android](https://github.com/AzureAD/microsoft-authentication-library-for-android/wiki)

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]
