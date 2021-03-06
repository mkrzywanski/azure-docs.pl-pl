---
title: Tworzenie wielodostępnego demona korzystającego z punktu końcowego platformy tożsamości firmy Microsoft
description: W tym samouczku dowiesz się, jak wywoływać interfejs API sieci Web ASP.NET chroniony przez Azure Active Directory z aplikacji klasycznej systemu Windows (WPF). Klient WPF uwierzytelnia użytkownika, żąda tokenu dostępu i wywołuje internetowy interfejs API.
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: tutorial
ms.workload: identity
ms.date: 12/10/2019
ms.author: jmprieur
ms.custom: aaddev, identityplatformtop40, scenarios:getting-started, languages:ASP.NET
ms.openlocfilehash: b63aa2b2d98a12246d0dc2c35e015da872caff28
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/19/2020
ms.locfileid: "83641100"
---
# <a name="tutorial-build-a-multitenant-daemon-that-uses-the-microsoft-identity-platform-endpoint"></a>Samouczek: Tworzenie demona wielodostępnego, która używa punktu końcowego platformy tożsamości firmy Microsoft

W ramach tego samouczka nauczysz się używać platformy tożsamości firmy Microsoft do uzyskiwania dostępu do danych klientów firmy Microsoft w długotrwałym, nieinteraktywnym procesie. Demona Przykładowa używa [przydzielenia poświadczeń klienta OAuth2](v2-oauth2-client-creds-grant-flow.md) w celu uzyskania tokenu dostępu. Demon używa tokenu do wywoływania [Microsoft Graph](https://graph.microsoft.io) i dostępu do danych organizacji.

> [!div class="checklist"]
> * Integrowanie aplikacji demona z platformą tożsamości firmy Microsoft
> * Przyznawanie uprawnień aplikacji bezpośrednio do aplikacji przez administratora
> * Uzyskiwanie tokenu dostępu w celu wywołania interfejsu API Microsoft Graph
> * Wywołaj interfejs API Microsoft Graph.

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem Utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) .

Aplikacja została skompilowana jako aplikacja ASP.NET MVC. Używa ona oprogramowania OWIN OpenID Connect Connecter do logowania użytkowników.

Składnik "Demon" w tym przykładzie jest kontrolerem interfejsu API, `SyncController.cs` . Gdy kontroler jest wywoływany, pobiera listę użytkowników w dzierżawie Azure Active Directory klienta (Azure AD) z Microsoft Graph. `SyncController.cs`jest wyzwalany przez wywołanie AJAX w aplikacji sieci Web. Używa [biblioteki Microsoft Authentication Library (MSAL) dla platformy .NET](msal-overview.md) , aby uzyskać token dostępu dla Microsoft Graph.

>[!NOTE]
> Jeśli dopiero zaczynasz pracę z platformą tożsamości firmy Microsoft, zalecamy rozpoczęcie pracy z programem [.NET Core DAEMON — szybki](quickstart-v2-netcore-daemon.md)Start.

## <a name="scenario"></a>Scenariusz

Ponieważ aplikacja jest aplikacją wielodostępnymi dla klientów firmy Microsoft, musi zapewnić klientom możliwość "rejestracji" lub "łączenia" aplikacji z danymi firmowymi. Podczas przepływu połączenia administrator firmy najpierw przyznaje *uprawnienia aplikacji* bezpośrednio do aplikacji, dzięki czemu może uzyskiwać dostęp do danych firmowych w sposób nieinteraktywny, bez obecności zalogowanego użytkownika. Większość logiki w tym przykładzie przedstawia sposób osiągnięcia tego przepływu połączenia przy użyciu punktu końcowego [zgody](v2-permissions-and-consent.md#using-the-admin-consent-endpoint) na platformę tożsamości.

![Topologia](./media/tutorial-v2-aspnet-daemon-webapp/topology.png)

Aby uzyskać więcej informacji na temat pojęć użytych w tym przykładzie, Przeczytaj [dokumentację protokołu poświadczeń klienta dla punktu końcowego platformy tożsamości](v2-oauth2-client-creds-grant-flow.md).

## <a name="prerequisites"></a>Wymagania wstępne

Aby uruchomić przykład w tym przewodniku Szybki Start, potrzebne są:

- [Program Visual Studio 2017 lub 2019](https://visualstudio.microsoft.com/downloads/).
- Dzierżawa usługi Azure AD. Aby uzyskać więcej informacji, zobacz [jak uzyskać dzierżawę usługi Azure AD](quickstart-create-new-tenant.md).
- Co najmniej jedno konto użytkownika w dzierżawie usługi Azure AD. Ten przykład nie będzie działał z konto Microsoft (dawniej konto usługi Windows Live). Jeśli zalogowano się do [Azure Portal](https://portal.azure.com) za pomocą konto Microsoft i nigdy nie utworzono konta użytkownika w katalogu, należy to zrobić teraz.

## <a name="clone-or-download-this-repository"></a>Klonuj lub pobrać to repozytorium

W powłoce lub wierszu polecenia wprowadź następujące polecenie:

```Shell
git clone https://github.com/Azure-Samples/active-directory-dotnet-daemon-v2.git
```

Lub [Pobierz przykład w pliku zip](https://github.com/Azure-Samples/ms-identity-aspnet-daemon-webapp/archive/master.zip).

## <a name="register-your-application"></a>Rejestrowanie aplikacji

Ten przykład ma jeden projekt. Aby zarejestrować aplikację w dzierżawie usługi Azure AD, możesz:

- Wykonaj kroki opisane w temacie [Rejestrowanie przykładu w dzierżawie Azure Active Directory](#register-your-application) i [Skonfiguruj przykład do korzystania z dzierżawy usługi Azure AD](#choose-the-azure-ad-tenant).
- Użyj skryptów programu PowerShell, które:
  - *Automatycznie* Twórz aplikacje usługi Azure AD i powiązane obiekty (hasła, uprawnienia, zależności).
  - Zmodyfikuj pliki konfiguracyjne projektów programu Visual Studio.

Jeśli chcesz użyć automatyzacji:

1. W systemie Windows uruchom program PowerShell i przejdź do katalogu głównego sklonowanego katalogu.
1. Uruchom następujące polecenie:

   ```PowerShell
   Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope Process -Force
   ```

1. Uruchom skrypt, aby utworzyć aplikację usługi Azure AD i odpowiednio skonfigurować kod przykładowej aplikacji:

   ```PowerShell
   .\AppCreationScripts\Configure.ps1
   ```

   Inne sposoby uruchamiania skryptów są opisane w [skryptach tworzenia aplikacji](https://github.com/Azure-Samples/ms-identity-aspnet-daemon-webapp/blob/master/AppCreationScripts/AppCreationScripts.md).

1. Otwórz rozwiązanie Visual Studio i wybierz pozycję **Uruchom** , aby uruchomić kod.

Jeśli nie chcesz używać automatyzacji, wykonaj kroki opisane w poniższych sekcjach.

### <a name="choose-the-azure-ad-tenant"></a>Wybierz dzierżawę usługi Azure AD

1. Zaloguj się do [Azure Portal](https://portal.azure.com) przy użyciu konta służbowego lub konto Microsoft prywatnego.
1. Jeśli konto należy do więcej niż jednej dzierżawy usługi Azure AD, wybierz swój profil w menu w górnej części strony, a następnie wybierz pozycję **Przełącz katalog**.
1. Zmień sesję portalu na żądaną dzierżawę usługi Azure AD.

### <a name="register-the-client-app-dotnet-web-daemon-v2"></a>Rejestrowanie aplikacji klienckiej (dotnet-Web-DAEMON-v2)

1. Przejdź do strony [rejestracje aplikacji](https://go.microsoft.com/fwlink/?linkid=2083908) na platformie tożsamości firmy Microsoft dla deweloperów.
1. Wybierz pozycję **Nowa rejestracja**.
1. Po wyświetleniu strony **Rejestrowanie aplikacji** podaj informacje dotyczące rejestracji aplikacji:
   - W sekcji **Nazwa** wprowadź zrozumiałą nazwę aplikacji, która będzie wyświetlana użytkownikom aplikacji. Na przykład wprowadź polecenie **dotnet-Web-DAEMON-v2**.
   - W sekcji **obsługiwane typy kont** wybierz pozycję **konta w dowolnym katalogu organizacyjnym**.
   - W sekcji **Identyfikator URI przekierowania (opcjonalnie)** wybierz pozycję **Sieć Web** w polu kombi i wprowadź następujące identyfikatory URI przekierowania:
       - **https://localhost:44316/**
       - **https://localhost:44316/Account/GrantPermissions**

     Jeśli istnieje więcej niż dwa identyfikatory URI przekierowania, należy dodać je z karty **uwierzytelnianie** później, po pomyślnym utworzeniu aplikacji.
1. Wybierz pozycję **Zarejestruj**, aby utworzyć aplikację.
1. Na stronie **Przegląd** aplikacji Znajdź wartość **Identyfikator aplikacji (klienta)** i Zapisz ją jako nowszą. Będzie ona potrzebna do skonfigurowania pliku konfiguracji programu Visual Studio dla tego projektu.
1. Na liście stron dla aplikacji wybierz pozycję **Uwierzytelnianie**. Następnie:
   - W sekcji **Ustawienia zaawansowane** Ustaw **adres URL wylogowywania** na **https://localhost:44316/Account/EndSession** .
   - W sekcji **Ustawienia zaawansowane**  >  **niejawne przyznanie** wybierz pozycję **tokeny dostępu** i **tokeny identyfikatorów**. Ten przykład wymaga, aby [przepływ niejawnego przydzielenia](v2-oauth2-implicit-grant-flow.md) był włączony do logowania użytkownika i wywoływania interfejsu API.
1. Wybierz pozycję **Zapisz**.
1. Na stronie **certyfikaty & wpisy tajne** w sekcji **klucze tajne klienta** wybierz pozycję **nowy klucz tajny klienta**. Następnie:

   1. Wprowadź opis klucza (na przykład **klucz tajny aplikacji**),
   1. Wybierz kluczowy okres trwania z przedziału **1 roku**, **w ciągu 2 lat**lub **nigdy nie wygasa**.
   1. Wybierz przycisk **Add** (Dodaj).
   1. Gdy zostanie wyświetlona wartość klucza, skopiuj ją i Zapisz w bezpiecznej lokalizacji. Ten klucz będzie potrzebny później, aby skonfigurować projekt w programie Visual Studio. Nie będzie można go ponownie wyświetlić ani pobrać w inny sposób.
1. Na liście stron dla aplikacji wybierz pozycję **uprawnienia interfejsu API**. Następnie:
   1. Wybierz przycisk **Dodaj uprawnienia**.
   1. Upewnij się, że wybrano kartę **interfejsy API firmy Microsoft** .
   1. W sekcji **najczęściej używane interfejsy API firmy Microsoft** wybierz pozycję **Microsoft Graph**.
   1. W sekcji **uprawnienia aplikacji** upewnij się, że wybrano odpowiednie uprawnienia: **User. Read. All**.
   1. Wybierz przycisk **Dodaj uprawnienia** .

## <a name="configure-the-sample-to-use-your-azure-ad-tenant"></a>Konfigurowanie przykładu do korzystania z dzierżawy usługi Azure AD

W poniższych krokach **ClientID** jest taki sam jak "Identyfikator aplikacji" lub **AppID**.

Otwórz rozwiązanie w programie Visual Studio, aby skonfigurować projekty.

### <a name="configure-the-client-project"></a>Konfigurowanie projektu klienta

W przypadku użycia skryptów Instalatora zostaną zastosowane następujące zmiany.

1. Otwórz plik **UserSync\Web.config** .
1. Znajdź klucz aplikacji **Ida: ClientId**. Zastąp istniejącą wartość IDENTYFIKATORem aplikacji aplikacji **dotnet-Web-DAEMON-v2** skopiowanej z Azure Portal.
1. Znajdź klucz aplikacji **Ida: ClientSecret**. Zastąp istniejącą wartość kluczem zapisanym podczas tworzenia aplikacji **dotnet-Web-DAEMON-v2** w Azure Portal.

## <a name="run-the-sample"></a>Uruchamianie aplikacji przykładowej

Wyczyść rozwiązanie, ponownie skompiluj rozwiązanie, uruchom aplikację UserSync, a następnie zaloguj się jako administrator w dzierżawie usługi Azure AD. Jeśli nie masz dzierżawy usługi Azure AD do testowania, możesz [wykonać te instrukcje](quickstart-create-new-tenant.md) , aby je uzyskać.

Po zalogowaniu aplikacja najpierw wyświetli monit o podanie uprawnień do zalogowania się i odczytania profilu użytkownika. Dzięki temu aplikacja może mieć pewność, że jesteś użytkownikiem biznesowym.

![Zgoda użytkownika](./media/tutorial-v2-aspnet-daemon-webapp/firstconsent.png)

Następnie aplikacja próbuje zsynchronizować listę użytkowników z dzierżawy usługi Azure AD za pośrednictwem Microsoft Graph. Jeśli nie jest to możliwe, prosi użytkownika (administratora dzierżawy) o połączenie dzierżawy z aplikacją.

Następnie aplikacja prosi o uprawnienie do odczytu listy użytkowników w Twojej dzierżawie.

![zgoda administratora](./media/tutorial-v2-aspnet-daemon-webapp/adminconsent.PNG)

Po udzieleniu uprawnienia nastąpi wylogowanie z aplikacji. Ten wylogowanie gwarantuje, że wszystkie istniejące tokeny dostępu dla Microsoft Graph zostaną usunięte z pamięci podręcznej tokenów. Po ponownym zalogowaniu uzyskany świeży token będzie miał odpowiednie uprawnienia do wykonywania wywołań do Microsoft Graph.


Po udzieleniu uprawnienia aplikacja może następnie wysyłać zapytania dla użytkowników w dowolnym momencie. Można to sprawdzić, wybierając przycisk **Synchronizuj użytkowników** i odświeżając listę użytkowników. Spróbuj dodać lub usunąć użytkownika i ponownie zsynchronizować listę. (Pamiętaj jednak, że aplikacja synchronizuje tylko pierwszą stronę użytkowników).

## <a name="about-the-code"></a>Informacje o kodzie

Odpowiedni kod dla tego przykładu znajduje się w następujących plikach:

- **App_Start \Startup.auth.cs**, **Controllers\AccountController.cs**: wstępne logowanie. W szczególności akcje na kontrolerze mają atrybut **Autoryzuj** , który wymusza Logowanie użytkownika. Aplikacja używa [przepływu kodu autoryzacji](v2-oauth2-auth-code-flow.md) do logowania użytkownika.
- **Controllers\SyncController.cs**: Synchronizowanie listy użytkowników z lokalnym magazynem w pamięci.
- **Controllers\UserController.cs**: Wyświetlanie listy użytkowników z lokalnego magazynu w pamięci.
- **Controllers\AccountController.cs**: uzyskiwanie uprawnień od administratora dzierżawy przy użyciu punktu końcowego zgody administratora.

## <a name="re-create-the-sample-app"></a>Ponowne tworzenie przykładowej aplikacji

1. W programie Visual Studio Utwórz nowy projekt **aplikacji sieci Web ASP.NET (.NET Framework)** w **języku Visual C#** .
1. Na następnym ekranie wybierz szablon projektu **MVC** . Należy również dodać folder i podstawowe odwołania dla **interfejsu API sieci Web**, ponieważ kontroler internetowego interfejsu API zostanie dodany później. Pozostaw wybrany tryb uwierzytelniania projektu jako domyślny: **Brak uwierzytelniania**.
1. Wybierz projekt w oknie **Eksplorator rozwiązań** i wybierz klawisz **F4** .
1. We właściwościach projektu ustaw wartość **SSL** na **true**. Zanotuj informacje w **adresie URL protokołu SSL**. Będzie on potrzebny podczas konfigurowania rejestracji tej aplikacji w Azure Portal.
1. Dodaj następujące pakiety NuGet oprogramowania pośredniczącego ASP.NET OWIN:
   - Microsoft. Owin. Security. ActiveDirectory
   - Microsoft.Owin.Security.Cookies
   - Microsoft.Owin.Host.SystemWeb
   - Microsoft. IdentityModel. Protocol. Extensions
   - Microsoft.Owin.Security.OpenIdConnect
   - Microsoft. Identity. Client
1. W folderze **App_Start** :
   1. Utwórz klasę o nazwie **Startup.auth.cs**.
   1. Usuń **. App_Start** z nazwy przestrzeni nazw.
   1. Zastąp kod klasy **uruchomieniowej** kodem z tego samego pliku przykładowej aplikacji.
   Upewnij się, że cała definicja klasy została uwzględniona. Definicja zostanie zmieniona z **uruchamiania klasy publicznej** na **Uruchamianie publicznej klasy częściowej.**
1. W **Startup.auth.cs**, Rozwiąż brakujące odwołania przez dodanie instrukcji **using** zgodnie z sugestią w programie Visual Studio IntelliSense.
1. Kliknij prawym przyciskiem myszy projekt, wybierz polecenie **Dodaj**, a następnie wybierz pozycję **Klasa**.
1. W polu wyszukiwania wprowadź **Owin**. **Klasa uruchomieniowa Owin** jest wyświetlana jako zaznaczenie. Zaznacz ją i nadaj jej nazwę **Startup.cs**.
1. W **Startup.cs**Zastąp kod klasy **uruchomieniowej** kodem z tego samego pliku przykładowej aplikacji. Należy pamiętać, że definicja zmiany z **klasy publicznej rozpoczyna** się na **publiczne uruchamianie częściowej klasy**.
1. W folderze **modele** Dodaj nową klasę o nazwie **MsGraphUser.cs**. Zastąp implementację zawartością pliku o tej samej nazwie z przykładu.
1. Dodaj nowy **kontroler MVC 5 — puste** wystąpienie o nazwie **elementu AccountController**. Zastąp implementację zawartością pliku o tej samej nazwie z przykładu.
1. Dodaj nowy **kontroler MVC 5 — puste** wystąpienie o nazwie **UserController**. Zastąp implementację zawartością pliku o tej samej nazwie z przykładu.
1. Dodaj nowy **kontroler Web API 2 — puste** wystąpienie o nazwie **SyncController**. Zastąp implementację zawartością pliku o tej samej nazwie z przykładu.
1. W przypadku interfejsu użytkownika w folderze **Views\Account** Dodaj trzy **puste (bez modelu) wystąpienia widoków** o nazwie **GrantPermissions**, **index**i **UserMismatch**. Dodaj i jeden nazwany **indeks** w folderze **Views\User** Zastąp implementację zawartością pliku o tej samej nazwie z przykładu.
1. Aktualizowanie **współużytkowanego \_ układu. cshtml** i **Home\Index.cshtml** w celu poprawnego łączenia różnych widoków.

## <a name="deploy-the-sample-to-azure"></a>Wdróż przykład na platformie Azure

Ten projekt zawiera projekty aplikacji sieci Web i interfejsów API sieci Web. W celu wdrożenia ich w usłudze Azure Websites wykonaj następujące czynności:

1. Utwórz witrynę sieci Web platformy Azure.
1. Publikowanie aplikacji sieci Web i interfejsów API sieci Web w witrynie sieci Web.
1. Zaktualizuj klientów, aby wywoływać witrynę sieci Web, a nie IIS Express.

### <a name="create-and-publish-dotnet-web-daemon-v2-to-an-azure-website"></a>Tworzenie i publikowanie dotnet-Web-DAEMON-v2 w witrynie sieci Web platformy Azure

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).
1. W lewym górnym rogu wybierz pozycję **Utwórz zasób**.
1. Wybierz pozycję **Web**  >  **App**Web, a następnie nadaj nazwę witrynie sieci Web. Na przykład nadaj mu nazwę **dotnet-Web-DAEMON-v2-contoso.azurewebsites.NET**.
1. Wybierz informacje dotyczące **subskrypcji**, **grupy zasobów**i **planu i lokalizacji usługi App Service**. System **operacyjny** to **Windows**i **publikuje** **kod**.
1. Wybierz pozycję **Utwórz** i poczekaj na utworzenie usługi App Service.
1. Po otrzymaniu powiadomienia o **pomyślnym wdrożeniu** wybierz pozycję **Przejdź do zasobu** , aby przejść do nowo utworzonej usługi App Service.
1. Po utworzeniu witryny sieci Web Znajdź ją na **pulpicie nawigacyjnym** , a następnie wybierz ją, aby otworzyć ekran **Przegląd** usługi App Service.
1. Na karcie **Przegląd** usługi App Service Pobierz profil publikowania, wybierając link **Pobierz profil publikowania** i Zapisz go. Można użyć innych mechanizmów wdrażania, takich jak wdrażanie z kontroli źródła.
1. Przejdź do programu Visual Studio, a następnie:
   1. Przejdź do projektu **dotnet-Web-DAEMON-v2** .
   1. Kliknij prawym przyciskiem myszy projekt w Eksplorator rozwiązań, a następnie wybierz pozycję **Publikuj**.
   1. Wybierz pozycję **Importuj profil** na dolnym pasku i zaimportuj wcześniej pobrany profil publikacji.
1. Wybierz pozycję **Konfiguruj**.
1. Na karcie **połączenie** zaktualizuj docelowy adres URL w taki sposób, aby korzystał z protokołu HTTPS. Użyj na przykład nazwy `https://dotnet-web-daemon-v2-contoso.azurewebsites.net`. Wybierz pozycję **Dalej**.
1. Na karcie **Ustawienia** upewnij się, że pole wyboru **Włącz uwierzytelnianie organizacyjne** jest wyczyszczone.
1. Wybierz pozycję **Zapisz**. Wybierz pozycję **Publikuj** na ekranie głównym.

Program Visual Studio opublikuje projekt i automatycznie otworzy przeglądarkę na adres URL projektu. Jeśli zostanie wyświetlona domyślna strona sieci Web projektu, publikacja zakończyła się pomyślnie.

### <a name="update-the-azure-ad-tenant-application-registration-for-dotnet-web-daemon-v2"></a>Aktualizowanie rejestracji aplikacji dzierżawy usługi Azure AD dla programu dotnet-Web-DAEMON-v2

1. Wróć do witryny [Azure Portal](https://portal.azure.com).
1. W okienku po lewej stronie wybierz usługę **Azure Active Directory** a następnie wybierz pozycję **rejestracje aplikacji**.
1. Wybierz aplikację **dotnet-Web-demon-v2** .
1. Na stronie **uwierzytelnianie** dla swojej aplikacji zaktualizuj pola **adresu URL wylogowywania** z adresem usługi. Użyj na przykład nazwy `https://dotnet-web-daemon-v2-contoso.azurewebsites.net`.
1. Z menu **znakowanie** , zaktualizuj **adres URL strony głównej** na adres usługi. Użyj na przykład nazwy `https://dotnet-web-daemon-v2-contoso.azurewebsites.net`.
1. Zapisz konfigurację.
1. Dodaj ten sam adres URL na liście wartości **Authentication**  >  menu**identyfikatorów URI przekierowania** uwierzytelniania. Jeśli masz wiele adresów URL przekierowania, upewnij się, że istnieje nowy wpis, który używa identyfikatora URI usługi App Service dla każdego adresu URL przekierowania.

## <a name="clean-up-resources"></a>Czyszczenie zasobów
Gdy nie jest już potrzebne, Usuń obiekt aplikacji, który został utworzony w kroku [zarejestruj aplikację](#register-your-application) .  Aby usunąć aplikację, postępuj zgodnie z instrukcjami w temacie [usuwanie aplikacji napisanej przez Ciebie lub w organizacji](quickstart-remove-app.md#remove-an-application-authored-by-you-or-your-organization).

## <a name="get-help"></a>Uzyskaj pomoc

Użyj [Stack Overflow](http://stackoverflow.com/questions/tagged/msal) , aby uzyskać pomoc techniczną dla społeczności.
Zadawaj pytania na Stack Overflow pierwsze i Przeglądaj istniejące problemy, aby zobaczyć, czy ktoś zażądał wcześniej pytania.
Upewnij się, że Twoje pytania lub komentarze są oznaczone atrybutem ADAL, "msal" i "dotnet".

Jeśli znajdziesz usterkę w przykładzie, zgłoś problem w usłudze [GitHub](https://github.com/Azure-Samples/ms-identity-aspnet-daemon-webapp/issues).

Jeśli znajdziesz błąd w MSAL.NET, zgłoś problem w usłudze [MSAL.NET GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet/issues).

Aby uzyskać zalecenie, przejdź do [strony głos użytkownika](https://feedback.azure.com/forums/169401-azure-active-directory).

## <a name="next-steps"></a>Następne kroki
Dowiedz się więcej na temat różnych [przepływów uwierzytelniania i scenariuszy aplikacji](authentication-flows-app-scenarios.md) obsługiwanych przez platformę tożsamości firmy Microsoft.

Aby uzyskać więcej informacji, zobacz następującą dokumentację koncepcyjną:

- [Dzierżawa w Azure Active Directory](single-and-multi-tenant-apps.md)
- [Opis środowisk zgody dla aplikacji usługi Azure AD](application-consent-experience.md)
- [Zaloguj dowolnego użytkownika Azure Active Directory przy użyciu wzorca aplikacji wielodostępnej](howto-convert-app-to-be-multi-tenant.md)
- [Zrozumienie zgody użytkownika i administratora](howto-convert-app-to-be-multi-tenant.md#understand-user-and-admin-consent)
- [Obiekty aplikacji i jednostki usługi w usłudze Azure Active Directory](app-objects-and-service-principals.md)
- [Szybki Start: rejestrowanie aplikacji na platformie tożsamości firmy Microsoft](quickstart-register-app.md)
- [Szybki Start: Konfigurowanie aplikacji klienckiej w celu uzyskiwania dostępu do interfejsów API sieci Web](quickstart-configure-app-access-web-apis.md)
- [Uzyskiwanie tokenu dla aplikacji za pomocą przepływów poświadczeń klienta](msal-client-applications.md)

Aby uzyskać prostszy dostęp do wielodostępnej aplikacji demona konsoli, zobacz [prestart demona platformy .NET Core](quickstart-v2-netcore-daemon.md).
