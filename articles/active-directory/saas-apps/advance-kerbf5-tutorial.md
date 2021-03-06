---
title: 'Samouczek: Azure Active Directory Integracja z logowaniem jednokrotnym za pomocą klawisza F5 | Microsoft Docs'
description: Dowiedz się, jak skonfigurować Logowanie jednokrotne między Azure Active Directory i F5.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 9c5fb47a-1c5d-437a-b4c1-dbf739eaf5e3
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 11/11/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3e9a112f3f8cb7ba22201f4e5f05f21af30df039
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/23/2020
ms.locfileid: "87018641"
---
# <a name="tutorial-azure-active-directory-ad-single-sign-on-sso-integration-with-f5"></a>Samouczek: integracja logowania jednokrotnego (SSO) w usłudze Azure Active Directory (AD) za pomocą klawisza F5

W tym samouczku dowiesz się, jak zintegrować F5 z Azure Active Directory (Azure AD). Po zintegrowaniu z usługą Azure AD za pomocą klawisza F5 można:

* Kontrolka w usłudze Azure AD, która ma dostęp do klawisza F5.
* Zezwól użytkownikom na automatyczne logowanie do programu F5 przy użyciu kont usługi Azure AD.
* Zarządzaj kontami w jednej centralnej lokalizacji — Azure Portal.

Aby dowiedzieć się więcej o integracji aplikacji SaaS z usługą Azure AD, zobacz [co to jest dostęp do aplikacji i logowanie jednokrotne przy użyciu Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Wymagania wstępne

Aby rozpocząć, potrzebne są następujące elementy:

* Subskrypcja usługi Azure AD. Jeśli nie masz subskrypcji, możesz uzyskać [bezpłatne konto](https://azure.microsoft.com/free/).
* Subskrypcja z włączonym logowaniem jednokrotnym (SSO).

## <a name="scenario-description"></a>Opis scenariusza

W tym samouczku skonfigurujesz i testujesz Logowanie jednokrotne usługi Azure AD w środowisku testowym.

* Klawisz F5 obsługuje funkcję SSO zainicjowaną przez usługę **SP i dostawcy tożsamości**
* F5 SSO można skonfigurować na trzy różne sposoby.

- [Konfigurowanie logowania jednokrotnego w przypadku zaawansowanej aplikacji Kerberos](#configure-f5-single-sign-on-for-advanced-kerberos-application)

- [Konfigurowanie logowania jednokrotnego dla aplikacji na podstawie nagłówka](headerf5-tutorial.md)

- [Konfigurowanie logowania jednokrotnego dla aplikacji Kerberos](kerbf5-tutorial.md)

## <a name="adding-f5-from-the-gallery"></a>Dodawanie F5 z galerii

Aby skonfigurować integrację klawisza F5 z usługą Azure AD, musisz dodać F5 z galerii do listy zarządzanych aplikacji SaaS.

1. Zaloguj się do [Azure Portal](https://portal.azure.com) przy użyciu konta służbowego lub konto Microsoft prywatnego.
1. W okienku nawigacji po lewej stronie wybierz usługę **Azure Active Directory** .
1. Przejdź do **aplikacji przedsiębiorstwa** , a następnie wybierz pozycję **wszystkie aplikacje**.
1. Aby dodać nową aplikację, wybierz pozycję **Nowa aplikacja**.
1. W sekcji **Dodaj z galerii** wpisz **F5** w polu wyszukiwania.
1. Wybierz pozycję **F5** z panelu wyniki, a następnie Dodaj aplikację. Poczekaj kilka sekund, gdy aplikacja zostanie dodana do dzierżawy.

## <a name="configure-and-test-azure-ad-single-sign-on-for-f5"></a>Skonfiguruj i przetestuj Logowanie jednokrotne w usłudze Azure AD dla F5

Skonfiguruj i przetestuj Logowanie jednokrotne usługi Azure AD za pomocą klawisza F5 przy użyciu użytkownika testowego o nazwie **B. Simon**. Aby logowanie jednokrotne działało, należy ustanowić relację linku między użytkownikiem usługi Azure AD i powiązanym użytkownikiem w F5.

Aby skonfigurować i przetestować Logowanie jednokrotne usługi Azure AD za pomocą klawisza F5, wykonaj następujące bloki konstrukcyjne:

1. **[Skonfiguruj Logowanie jednokrotne usługi Azure AD](#configure-azure-ad-sso)** , aby umożliwić użytkownikom korzystanie z tej funkcji.
    1. **[Utwórz użytkownika testowego usługi Azure AD](#create-an-azure-ad-test-user)** — aby przetestować Logowanie jednokrotne w usłudze Azure AD za pomocą usługi B. Simon.
    1. **[Przypisz użytkownika testowego usługi Azure AD](#assign-the-azure-ad-test-user)** — aby umożliwić usłudze B. Simon korzystanie z logowania jednokrotnego w usłudze Azure AD.
1. **[Skonfiguruj F5-SSO](#configure-f5-sso)** , aby skonfigurować ustawienia logowania jednokrotnego na stronie aplikacji.
    1. **[Utwórz użytkownika testowego F5](#create-f5-test-user)** , aby dysponować odpowiednikiem B. Simon w F5, która jest połączona z reprezentacją użytkownika w usłudze Azure AD.
1. **[Przetestuj Logowanie jednokrotne](#test-sso)** — aby sprawdzić, czy konfiguracja działa.

## <a name="configure-azure-ad-sso"></a>Konfigurowanie rejestracji jednokrotnej w usłudze Azure AD

Wykonaj następujące kroki, aby włączyć logowanie jednokrotne usługi Azure AD w Azure Portal.

1. W [Azure Portal](https://portal.azure.com/)na stronie integracja z aplikacją **F5** Znajdź sekcję **Zarządzanie** i wybierz pozycję **Logowanie jednokrotne**.
1. Na stronie **Wybierz metodę logowania jednokrotnego** wybierz pozycję **SAML**.
1. Na stronie **Konfigurowanie logowania jednokrotnego przy użyciu języka SAML** kliknij ikonę Edytuj/pióro, aby określić **podstawową konfigurację języka SAML** , aby edytować ustawienia.

   ![Edycja podstawowej konfiguracji protokołu SAML](common/edit-urls.png)

1. Jeśli chcesz skonfigurować aplikację w trybie inicjalizacji **dostawcy tożsamości** , w sekcji **Podstawowa konfiguracja SAML** wprowadź wartości dla następujących pól:

    a. W polu tekstowym **Identyfikator** wpisz adres URL, używając następującego wzorca:`https://<YourCustomFQDN>.f5.com/`

    b. W polu tekstowym **Adres URL odpowiedzi** wpisz adres URL, korzystając z następującego wzorca: `https://<YourCustomFQDN>.f5.com/`

1. Kliknij pozycję **Ustaw dodatkowe adresy URL** i wykonaj następujące kroki, jeśli chcesz skonfigurować aplikację w trybie inicjowania programu **SP** :

    W polu tekstowym **Adres URL logowania** wpisz adres URL, korzystając z następującego wzorca: `https://<YourCustomFQDN>.f5.com/`

    > [!NOTE]
    > Te wartości nie są prawdziwe. Należy je zastąpić rzeczywistymi wartościami identyfikatora, adresu URL odpowiedzi i adresu URL logowania. Skontaktuj się z [zespołem obsługi klienta F5](https://support.f5.com/csp/knowledge-center/software/BIG-IP?module=BIG-IP%20APM45) , aby uzyskać te wartości. Przydatne mogą się również okazać wzorce przedstawione w sekcji **Podstawowa konfiguracja protokołu SAML** w witrynie Azure Portal.

1. Na stronie **Konfigurowanie logowania jednokrotnego przy użyciu protokołu SAML** w sekcji **certyfikat podpisywania SAML** Znajdź **plik XML metadanych Federacji** i wybierz pozycję **Pobierz** , aby pobrać certyfikat i zapisać go na komputerze.

    ![Link do pobierania certyfikatu](common/metadataxml.png)

1. W sekcji **Konfigurowanie F5** skopiuj odpowiednie adresy URL na podstawie wymagań.

    ![Kopiowanie adresów URL konfiguracji](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Tworzenie użytkownika testowego usługi Azure AD

W tej sekcji utworzysz użytkownika testowego w Azure Portal o nazwie B. Simon.

1. W lewym okienku w Azure Portal wybierz pozycję **Azure Active Directory**, wybierz pozycję **Użytkownicy**, a następnie wybierz pozycję **Wszyscy użytkownicy**.
1. Wybierz pozycję **nowy użytkownik** w górnej części ekranu.
1. We właściwościach **użytkownika** wykonaj następujące kroki:
   1. W polu **Nazwa** wprowadź wartość `B.Simon`.  
   1. W polu **Nazwa użytkownika** wprowadź wartość username@companydomain.extension . Na przykład `B.Simon@contoso.com`.
   1. Zaznacz pole wyboru **Pokaż hasło** i zanotuj wartość wyświetlaną w polu **Hasło**.
   1. Kliknij pozycję **Utwórz**.

### <a name="assign-the-azure-ad-test-user"></a>Przypisywanie użytkownika testowego usługi Azure AD

W tej sekcji włączysz usługę B. Simon, aby korzystać z logowania jednokrotnego na platformie Azure przez przyznanie dostępu do klawisza F5.

1. W Azure Portal wybierz pozycję **aplikacje dla przedsiębiorstw**, a następnie wybierz pozycję **wszystkie aplikacje**.
1. Na liście Aplikacje wybierz pozycję **F5**.
1. Na stronie Przegląd aplikacji Znajdź sekcję **Zarządzanie** i wybierz pozycję **Użytkownicy i grupy**.

   ![Link „Użytkownicy i grupy”](common/users-groups-blade.png)

1. Wybierz pozycję **Dodaj użytkownika**, a następnie w oknie dialogowym **Dodawanie przypisania** wybierz pozycję **Użytkownicy i grupy** .

    ![Link Dodaj użytkownika](common/add-assign-user.png)

1. W oknie dialogowym **Użytkownicy i grupy** wybierz pozycję **B. Simon** z listy Użytkownicy, a następnie kliknij przycisk **Wybierz** w dolnej części ekranu.
1. Jeśli oczekujesz dowolnej wartości roli w potwierdzeniu SAML, w oknie dialogowym **Wybierz rolę** wybierz odpowiednią rolę dla użytkownika z listy, a następnie kliknij przycisk **Wybierz** w dolnej części ekranu.
1. W oknie dialogowym **Dodawanie przypisania** kliknij przycisk **Przypisz** .

## <a name="configure-f5-sso"></a>Konfigurowanie rejestracji jednokrotnej F5

- [Konfigurowanie logowania jednokrotnego dla aplikacji na podstawie nagłówka](headerf5-tutorial.md)

- [Konfigurowanie logowania jednokrotnego dla aplikacji Kerberos](kerbf5-tutorial.md)

### <a name="configure-f5-single-sign-on-for-advanced-kerberos-application"></a>Konfigurowanie logowania jednokrotnego w przypadku zaawansowanej aplikacji Kerberos

1. Otwórz nowe okno przeglądarki sieci Web i zaloguj się do witryny firmy F5 (Advanced Kerberos) jako administrator i wykonaj następujące czynności:

1. Należy zaimportować certyfikat metadanych do F5 (Advanced Kerberos), który będzie używany później w procesie instalacji. Przejdź do pozycji **System > Certificate management > Traffic Certificate management >> listy certyfikatów SSL**. Kliknij pozycję **Importuj** w prawym górnym rogu.

    ![Konfiguracja F5 (Advanced Kerberos)](./media/advance-kerbf5-tutorial/configure01.png)
 
1. Aby skonfigurować dostawcy tożsamości SAML, przejdź do pozycji **dostęp > federacyjnego > dostawcy usługi saml > utwórz > z metadanych**.

    ![Konfiguracja F5 (Advanced Kerberos)](./media/advance-kerbf5-tutorial/configure02.png)

    ![Konfiguracja F5 (Advanced Kerberos)](./media/advance-kerbf5-tutorial/configure03.png)
 
    ![Konfiguracja F5 (Advanced Kerberos)](./media/advance-kerbf5-tutorial/configure04.png)

    ![Konfiguracja F5 (Advanced Kerberos)](./media/advance-kerbf5-tutorial/configure05.png)
 
1. Określ certyfikat przekazany z zadania 3

    ![Konfiguracja F5 (Advanced Kerberos)](./media/advance-kerbf5-tutorial/configure06.png)

    ![Konfiguracja F5 (Advanced Kerberos)](./media/advance-kerbf5-tutorial/configure07.png)

 1. Aby skonfigurować protokół SAML SP, przejdź do obszaru **dostępu > federacyjnego > saml > usługi w usłudze SP > Utwórz**.

    ![Konfiguracja F5 (Advanced Kerberos)](./media/advance-kerbf5-tutorial/configure08.png)
 
1. Kliknij przycisk **OK**.

1. Wybierz konfigurację SP i kliknij pozycję **Powiązywanie/Usuwanie powiązania dostawcy tożsamości łączników**.

     ![Konfiguracja F5 (Advanced Kerberos)](./media/advance-kerbf5-tutorial/configure09.png)
 
 
1. Kliknij przycisk **Dodaj nowy wiersz** i wybierz **zewnętrzny łącznik dostawcy tożsamości** utworzony w poprzednim kroku.

    ![Konfiguracja F5 (Advanced Kerberos)](./media/advance-kerbf5-tutorial/configure10.png)
 
1. Do konfigurowania logowania jednokrotnego protokołu Kerberos **> dostępu > protokołu Kerberos** przy użyciu logowania jednokrotnego

    >[!Note]
    >Konieczne będzie utworzenie i określenie konta delegowania protokołu Kerberos. Odwołaj się do KCD sekcji (zobacz dodatek dla odwołań do zmiennych)

    • Źródło nazwy użytkownika`session.saml.last.attr.name.http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`

    • Źródło obszaru użytkownika`session.logon.last.domain`

    ![Konfiguracja F5 (Advanced Kerberos)](./media/advance-kerbf5-tutorial/configure11.png)

1. Aby skonfigurować profil dostępu, **> dostępu do profilu/zasad > profilu dostępu (dla zasad sesji)**.

    ![Konfiguracja F5 (Advanced Kerberos)](./media/advance-kerbf5-tutorial/configure12.png)

    ![Konfiguracja F5 (Advanced Kerberos)](./media/advance-kerbf5-tutorial/configure13.png)

    ![Konfiguracja F5 (Advanced Kerberos)](./media/advance-kerbf5-tutorial/configure14.png)

    ![Konfiguracja F5 (Advanced Kerberos)](./media/advance-kerbf5-tutorial/configure15.png)

    ![Konfiguracja F5 (Advanced Kerberos)](./media/advance-kerbf5-tutorial/configure16.png)
 
    * Session. Logon. Last. usernameUPN Expr {[mcget {Session. SAML. Last. Identity}]}

    * preprezentacja tekstu Session. AD. lastactualdomain. Live

    ![Konfiguracja F5 (Advanced Kerberos)](./media/advance-kerbf5-tutorial/configure17.png)

    * (userPrincipalName =% {Session. Logon. Last. usernameUPN})

    ![Konfiguracja F5 (Advanced Kerberos)](./media/advance-kerbf5-tutorial/configure18.png)

    ![Konfiguracja F5 (Advanced Kerberos)](./media/advance-kerbf5-tutorial/configure19.png)

    * Session. Logon. Last. username Expr {"[mcget {Session. AD. Last. ATTR. sAMAccountName}]"}

    ![Konfiguracja F5 (Advanced Kerberos)](./media/advance-kerbf5-tutorial/configure20.png)

    * mcget {Session. Logon. Last. username}
    * mcget {Session. Logon. Last. Password}

1. Aby dodać nowy węzeł, przejdź do obszaru **ruch lokalny > węzły > listy węzłów > +**.

    ![Konfiguracja F5 (Advanced Kerberos)](./media/advance-kerbf5-tutorial/configure21.png)
 
1. Aby utworzyć nową pulę, przejdź do obszaru **ruch lokalny > pule > liście puli > Utwórz**.

     ![Konfiguracja F5 (Advanced Kerberos)](./media/advance-kerbf5-tutorial/configure22.png)

 1. Aby utworzyć nowy serwer wirtualny, przejdź do obszaru **ruch lokalny > serwery wirtualne > listy serwerów wirtualnych > +**.

    ![Konfiguracja F5 (Advanced Kerberos)](./media/advance-kerbf5-tutorial/configure23.png)

1. Określ Profil dostępu utworzony w poprzednim kroku.

    ![Konfiguracja F5 (Advanced Kerberos)](./media/advance-kerbf5-tutorial/configure24.png) 

### <a name="setting-up-kerberos-delegation"></a>Konfigurowanie delegowania protokołu Kerberos 

>[!Note]
>Więcej szczegółów można znaleźć [tutaj](https://www.f5.com/pdf/deployment-guides/kerberos-constrained-delegation-dg.pdf)

* **Krok 1. Tworzenie konta delegowania**

    * Przykład
    ```
    Domain Name : superdemo.live
    Sam Account Name : big-ipuser

    New-ADUser -Name "APM Delegation Account" -UserPrincipalName host/big-ipuser.superdemo.live@superdemo.live -SamAccountName "big-ipuser" -PasswordNeverExpires $true -Enabled $true -AccountPassword (Read-Host -AsSecureString "Password!1234")
    ```

* **Krok 2. Ustawianie nazwy SPN (na koncie delegowania APM)**

    *  Przykład
    ```
    setspn –A host/big-ipuser.superdemo.live big-ipuser
    ```

* **Krok 3: delegowanie nazwy SPN (dla konta App Service)**

    * Skonfiguruj odpowiednie delegowanie dla konta delegowania F5.
    * W poniższym przykładzie konto delegowania APM jest konfigurowane do KCD for FRP-APP1. predemonstracyjne. Live App.

        ![Konfiguracja F5 (Advanced Kerberos)](./media/advance-kerbf5-tutorial/configure25.png)

1. Podaj szczegóły, jak wspomniano w powyższym dokumencie referencyjnym w [tym](https://techdocs.f5.com/kb/en-us/products/big-ip_apm/manuals/product/apm-authentication-single-sign-on-11-5-0/2.html) temacie

1. Dodatek — mapowania zmiennych adresów IP w języku SAML – F5 pokazane poniżej:

    ![Konfiguracja F5 (Advanced Kerberos)](./media/advance-kerbf5-tutorial/configure26.png)

    ![Konfiguracja F5 (Advanced Kerberos)](./media/advance-kerbf5-tutorial/configure27.png) 

1. Poniżej znajduje się pełna lista domyślnych atrybutów SAML. Dana wartośćname jest reprezentowana przy użyciu następującego ciągu.
`session.saml.last.attr.name.http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname`

| Sesja | Atrybut |
| -- | -- |
| eb46b6b6. Session. SAML. Last. assertionID | `<TENANT ID>` |
| eb46b6b6. Session. SAML. Last. assertionIssueInstant  | `<ID>` |
| eb46b6b6. Session. SAML. Last. assertionIssuer | `https://sts.windows.net/<TENANT ID>`/ |
| eb46b6b6. Session. SAML. Last. ATTR. Name. http: \/ /schemas.Microsoft.com/Claims/authnmethodsreferences | `http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password` |
| eb46b6b6. Session. SAML. Last. ATTR. Name. http: \/ /schemas.Microsoft.com/identity/claims/DisplayName | user0 |
| eb46b6b6. Session. SAML. Last. ATTR. Name. http: \/ /schemas.Microsoft.com/identity/claims/IdentityProvider | `https://sts.windows.net/<TENANT ID>/` |
| eb46b6b6. Session. SAML. Last. ATTR. Name. http: \/ /schemas.Microsoft.com/identity/claims/objectidentifier | `<TENANT ID>` |
| eb46b6b6. Session. SAML. Last. ATTR. Name. http: \/ /schemas.Microsoft.com/identity/claims/tenantid | `<TENANT ID>` |
| eb46b6b6. Session. SAML. Last. ATTR. Name. http: \/ /schemas.xmlSOAP.org/ws/2005/05/identity/claims/EmailAddress | `user0@superdemo.live` |
| eb46b6b6. Session. SAML. Last. ATTR. Name. http: \/ /schemas.xmlSOAP.org/ws/2005/05/identity/claims/givenName | user0 |
| eb46b6b6. Session. SAML. Last. ATTR. Name. http: \/ /schemas.xmlSOAP.org/ws/2005/05/identity/claims/Name | `user0@superdemo.live` |
| eb46b6b6. Session. SAML. Last. ATTR. Name. http: \/ /schemas.xmlSOAP.org/ws/2005/05/identity/claims/surname | 0 |
| eb46b6b6. Session. SAML. Last. odbiorca | `https://kerbapp.superdemo.live` |
| eb46b6b6. Session. SAML. Last. authNContextClassRef | urn: języka Oasis: names: TC: SAML: 2.0: AC: klasy: hasło |
| eb46b6b6. Session. SAML. Last. authNInstant | `<ID>` |
| eb46b6b6. Session. SAML. Last. Identity | `user0@superdemo.live` |
| eb46b6b6. Session. SAML. Last. inResponseTo | `<TENANT ID>` |
| eb46b6b6. Session. SAML. Last. nameIDValue | `user0@superdemo.live` |
| eb46b6b6. Session. SAML. Last. nameIdFormat | urn: języka Oasis: names: TC: SAML: 1.1: NameID-format: emailAddress |
| eb46b6b6. Session. SAML. Last. responseDestination | `https://kerbapp.superdemo.live/saml/sp/profile/post/acs` |
| eb46b6b6. Session. SAML. Last. responseId | `<TENANT ID>` |
| eb46b6b6. Session. SAML. Last. responseIssueInstant | `<ID>` |
| eb46b6b6. Session. SAML. Last. responseIssuer | `https://sts.windows.net/<TENANT ID>/` |
| eb46b6b6. Session. SAML. Last. Result | 1 |
| eb46b6b6. Session. SAML. Last. samlVersion | 2,0 |
| eb46b6b6. Session. SAML. Last. sessionIndex | `<TENANT ID>` |
| eb46b6b6. Session. SAML. Last. statusValue | urn: języka Oasis: names: TC: SAML: 2.0: status: sukces |
| eb46b6b6. Session. SAML. Last. subjectConfirmDataNotOnOrAfter | `<ID>` |
| eb46b6b6. Session. SAML. Last. subjectConfirmDataRecipient | `https://kerbapp.superdemo.live/saml/sp/profile/post/acs` |
| eb46b6b6. Session. SAML. Last. subjectConfirmMethod | urn: języka Oasis: names: TC: SAML: 2.0: cm: Bearer |
| eb46b6b6. Session. SAML. Last. validityNotBefore | `<ID>` |
| eb46b6b6. Session. SAML. Last. validityNotOnOrAfter | `<ID>` |

### <a name="create-f5-test-user"></a>Utwórz użytkownika testowego F5

W tej sekcji utworzysz użytkownika o nazwie B. Simon w F5. Pracuj z [zespołem pomocy technicznej dla klientów F5](https://support.f5.com/csp/knowledge-center/software/BIG-IP?module=BIG-IP%20APM45) , aby dodać użytkowników na platformie F5. Użytkownicy muszą być utworzeni i aktywowani przed rozpoczęciem korzystania z logowania jednokrotnego. 

## <a name="test-sso"></a>Testuj Logowanie jednokrotne 

W tej sekcji przetestujesz konfigurację logowania jednokrotnego usługi Azure AD przy użyciu panelu dostępu.

Po kliknięciu kafelka F5 w panelu dostępu należy automatycznie zalogować się do F5, dla którego skonfigurowano Logowanie jednokrotne. Aby uzyskać więcej informacji na temat panelu dostępu, zobacz [wprowadzenie do panelu dostępu](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Lista samouczków dotyczących integrowania aplikacji SaaS z usługą Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Co to jest dostęp warunkowy w Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Wypróbuj usługę Azure AD](https://aad.portal.azure.com/)

- [Konfigurowanie logowania jednokrotnego dla aplikacji na podstawie nagłówka](headerf5-tutorial.md)

- [Konfigurowanie logowania jednokrotnego dla aplikacji Kerberos](kerbf5-tutorial.md)

