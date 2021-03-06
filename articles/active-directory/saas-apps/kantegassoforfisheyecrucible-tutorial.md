---
title: 'Samouczek: integracja Azure Active Directory z logowaniem jednokrotnym w usłudze Kantega dla FishEye/tygla | Microsoft Docs'
description: Dowiedz się, jak skonfigurować Logowanie jednokrotne między Azure Active Directory i Kantega SSO dla FishEye/tygla.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 9fe951fd-1530-4d33-a1a4-390385b99ce9
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/16/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: f262f94c451d5dcffd933bdebb1374b8733b9fd8
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/29/2020
ms.locfileid: "67595169"
---
# <a name="tutorial-azure-active-directory-integration-with-kantega-sso-for-fisheyecrucible"></a>Samouczek: integracja Azure Active Directory z logowaniem jednokrotnym w usłudze Kantega dla FishEye/tygla

W ramach tego samouczka nauczysz się zintegrować usługę Kantega SSO for FishEye/tygiel z usługą Azure Active Directory (Azure AD).
Integrowanie Kantega SSO dla FishEye/tygla z usługą Azure AD zapewnia następujące korzyści:

* Możesz kontrolować w usłudze Azure AD, kto ma dostęp do Kantega SSO dla FishEye/tygiel.
* Możesz włączyć automatyczne logowanie użytkowników, aby Kantega Logowanie jednokrotne w FishEye/tyglu (Logowanie jednokrotne) przy użyciu kont usługi Azure AD.
* Możesz zarządzać swoimi kontami w jednej centralnej lokalizacji — witrynie Azure Portal.

Jeśli chcesz dowiedzieć się więcej na temat integracji aplikacji SaaS z usługą Azure AD, zobacz [Co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem [Utwórz bezpłatne konto](https://azure.microsoft.com/free/) .

## <a name="prerequisites"></a>Wymagania wstępne

Aby skonfigurować integrację usługi Azure AD z logowaniem jednokrotnym w usłudze Kantega dla FishEye/tygla, potrzebne są następujące elementy:

* Subskrypcja usługi Azure AD. Jeśli nie masz środowiska usługi Azure AD, możesz uzyskać [bezpłatne konto](https://azure.microsoft.com/free/)
* Kantega Logowanie jednokrotne dla subskrypcji FishEye/tygiel z włączoną rejestracją jednokrotną

## <a name="scenario-description"></a>Opis scenariusza

W tym samouczku skonfigurujesz i przetestujesz logowanie jednokrotne usługi Azure AD w środowisku testowym.

* Logowanie jednokrotne w usłudze Kantega dla FishEye/tygiel obsługuje zainicjowanie logowania jednokrotnego **z** użyciem

## <a name="adding-kantega-sso-for-fisheyecrucible-from-the-gallery"></a>Dodawanie logowania jednokrotnego Kantega dla FishEye/tygla z galerii

Aby skonfigurować integrację logowania jednokrotnego Kantega dla FishEye/tygla z usługą Azure AD, musisz dodać Kantega SSO dla FishEye/tygiel z galerii do listy zarządzanych aplikacji SaaS.

**Aby dodać Kantega SSO dla FishEye/tygiel z galerii, wykonaj następujące czynności:**

1. W witrynie **[Azure Portal](https://portal.azure.com)** w panelu nawigacyjnym po lewej stronie kliknij ikonę usługi **Azure Active Directory**.

    ![Przycisk Azure Active Directory](common/select-azuread.png)

2. Przejdź do grupy **Aplikacje dla przedsiębiorstw** i wybierz opcję **Wszystkie aplikacje**.

    ![Blok Aplikacje dla przedsiębiorstw](common/enterprise-applications.png)

3. Aby dodać nową aplikację, kliknij przycisk **Nowa aplikacja** w górnej części okna dialogowego.

    ![Przycisk Nowa aplikacja](common/add-new-app.png)

4. W polu wyszukiwania wpisz **KANTEGA SSO for fisheye/tygiel**, wybierz pozycję **Kantega SSO dla fisheye/tygiel** z panelu wyników, a następnie kliknij przycisk **Dodaj** przycisk, aby dodać aplikację.

    ![Kantega Logowanie jednokrotne dla FishEye/tygla na liście wyników](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurowanie i testowanie logowania jednokrotnego usługi Azure AD

W tej sekcji można skonfigurować i przetestować Logowanie jednokrotne usługi Azure AD za pomocą logowania jednoKantegaowego dla FishEye/tygla na podstawie użytkownika testowego o nazwie **Britta Simon**.
Aby logowanie jednokrotne działało, należy ustanowić relację linku między użytkownikiem usługi Azure AD i powiązanym użytkownikiem w usłudze Kantega SSO dla FishEye/tygla.

Aby skonfigurować i przetestować Logowanie jednokrotne w usłudze Azure AD za pomocą usługi Kantega SSO dla FishEye/tygla, należy wykonać następujące bloki konstrukcyjne:

1. **[Konfigurowanie logowania jednokrotnego usługi Azure AD](#configure-azure-ad-single-sign-on)** — aby umożliwić użytkownikom korzystanie z tej funkcji.
2. **[Skonfiguruj logowanie JEDNOkrotne w usłudze Kantega do fisheye/tygiel](#configure-kantega-sso-for-fisheyecrucible-single-sign-on)** jednokrotne — aby skonfigurować ustawienia logowania jednokrotnego na stronie aplikacji.
3. **[Tworzenie użytkownika testowego usługi Azure AD](#create-an-azure-ad-test-user)** — aby przetestować logowanie jednokrotne usługi Azure AD z użytkownikiem Britta Simon.
4. **[Przypisywanie użytkownika testowego usługi Azure AD](#assign-the-azure-ad-test-user)** — aby umożliwić użytkownikowi Britta Simon korzystanie z logowania jednokrotnego usługi Azure AD.
5. **[Utwórz Logowanie jednokrotne w usłudze Kantega dla użytkownika testowego fisheye/tygiel](#create-kantega-sso-for-fisheyecrucible-test-user)** , aby dysponować odpowiednikiem Britta Simon w Kantega SSO dla fisheye/tygiel, który jest połączony z reprezentacją usługi Azure AD użytkownika.
6. **[Testowanie logowania jednokrotnego](#test-single-sign-on)** — aby sprawdzić, czy konfiguracja działa.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurowanie logowania jednokrotnego usługi Azure AD

W tej sekcji włączysz logowanie jednokrotne usługi Azure AD w witrynie Azure Portal.

Aby skonfigurować Logowanie jednokrotne w usłudze Azure AD za pomocą usługi Kantega SSO dla FishEye/tygla, wykonaj następujące czynności:

1. W [Azure Portal](https://portal.azure.com/)na stronie **Kantega Logowanie jednokrotne dla aplikacji fisheye/tygiel** wybierz pozycję **Logowanie**jednokrotne.

    ![Link do konfigurowania logowania jednokrotnego](common/select-sso.png)

2. W oknie dialogowym **Wybieranie metody logowania jednokrotnego** wybierz tryb **SAML/WS-Fed**, aby włączyć logowanie jednokrotne.

    ![Wybieranie trybu logowania jednokrotnego](common/select-saml-option.png)

3. Na stronie **Konfigurowanie logowania jednokrotnego za pomocą protokołu SAML** kliknij ikonę **Edytuj**, aby otworzyć okno dialogowe **Podstawowa konfiguracja protokołu SAML**.

    ![Edycja podstawowej konfiguracji protokołu SAML](common/edit-urls.png)

4. Jeśli chcesz skonfigurować aplikację w trybie inicjalizacji **dostawcy tożsamości** , w sekcji **Podstawowa konfiguracja SAML** wykonaj następujące czynności:

    ![Kantega Logowanie jednokrotne w przypadku domen FishEye/tygiel i adresów URL logowania jednokrotnego](common/idp-intiated.png)

    a. W polu tekstowym **Identyfikator** wpisz adres URL, używając następującego wzorca:`https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

    b. W polu tekstowym **Adres URL odpowiedzi** wpisz adres URL, korzystając z następującego wzorca: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

5. Kliknij pozycję **Ustaw dodatkowe adresy URL** i wykonaj następujące kroki, jeśli chcesz skonfigurować aplikację w trybie inicjowania programu **SP** :

    ![Kantega Logowanie jednokrotne w przypadku domen FishEye/tygiel i adresów URL logowania jednokrotnego](common/metadata-upload-additional-signon.png)

    W polu tekstowym **Adres URL logowania** wpisz adres URL, korzystając z następującego wzorca: `https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

    > [!NOTE]
    > Te wartości nie są prawdziwe. Zastąp je rzeczywistymi wartościami identyfikatora, adresu URL odpowiedzi i adresu URL logowania. Te wartości są odbierane podczas konfiguracji wtyczki FishEye/tygiel, która została omówiona w dalszej części samouczka.

6. Na stronie **Konfigurowanie logowania jednokrotnego za pomocą protokołu SAML** w sekcji **Certyfikat podpisywania SAML** kliknij link **Pobierz**, aby pobrać **kod XML metadanych federacji** na podstawie podanych opcji zgodnie z wymaganiami i zapisać go na komputerze.

    ![Link do pobierania certyfikatu](common/metadataxml.png)

7. W sekcji **set up KANTEGA SSO for fisheye/tygiel** skopiuj odpowiednie adresy URL zgodnie z wymaganiami.

    ![Kopiowanie adresów URL konfiguracji](common/copy-configuration-urls.png)

    a. Adres URL logowania

    b. Identyfikator usługi Azure AD

    c. Adres URL wylogowywania

### <a name="configure-kantega-sso-for-fisheyecrucible-single-sign-on"></a>Konfigurowanie logowania jednokrotnego Kantega do FishEye/tygla Logowanie jednokrotne

1. W innym oknie przeglądarki sieci Web Zaloguj się do serwera lokalnego FishEye/tygiel jako administrator.

1. Zatrzymaj wskaźnik myszy nad ikoną koła zębatego i kliknij pozycję **Dodatki**.

    ![Konfigurowanie logowania jednokrotnego](./media/kantegassoforfisheyecrucible-tutorial/addon1.png)

1. W sekcji Ustawienia systemu kliknij przycisk **Znajdź nowe dodatki**. 

    ![Konfigurowanie logowania jednokrotnego](./media/kantegassoforfisheyecrucible-tutorial/add-on2.png)

1. Wyszukaj **Kantega Logowanie jednokrotne dla tygla** i kliknij przycisk **Instaluj** , aby zainstalować nową wtyczkę SAML.

    ![Konfigurowanie logowania jednokrotnego](./media/kantegassoforfisheyecrucible-tutorial/addon2.png)

1. Rozpocznie się instalacja wtyczki. 

    ![Konfigurowanie logowania jednokrotnego](./media/kantegassoforfisheyecrucible-tutorial/addon33.png)

1. Po zakończeniu instalacji. Kliknij przycisk **Zamknij**.

    ![Konfigurowanie logowania jednokrotnego](./media/kantegassoforfisheyecrucible-tutorial/addon34.png)

1.  Kliknij pozycję **Zarządzaj**.

    ![Konfigurowanie logowania jednokrotnego](./media/kantegassoforfisheyecrucible-tutorial/addon35.png)

1. Kliknij pozycję **Configure** (Konfiguruj), aby skonfigurować nową wtyczkę. 

    ![Konfigurowanie logowania jednokrotnego](./media/kantegassoforfisheyecrucible-tutorial/addon3.png)

1. W sekcji **SAML** . Wybierz pozycję **Azure Active Directory (Azure AD)** z listy rozwijanej **Dodaj dostawcę tożsamości** .

    ![Konfigurowanie logowania jednokrotnego](./media/kantegassoforfisheyecrucible-tutorial/addon4.png)

1. Wybierz poziom subskrypcji jako **podstawowy**.

    ![Konfigurowanie logowania jednokrotnego](./media/kantegassoforfisheyecrucible-tutorial/addon5.png)

1. W sekcji **właściwości aplikacji** wykonaj następujące czynności:

    ![Konfigurowanie logowania jednokrotnego](./media/kantegassoforfisheyecrucible-tutorial/addon6.png)

    a. Skopiuj wartość **Identyfikator URI identyfikatora aplikacji** i użyj jej jako **identyfikatora, adresu URL odpowiedzi i adresu URL logowania** w sekcji **Podstawowa konfiguracja SAML** w Azure Portal.

    b. Kliknij przycisk **Dalej**.

1. W sekcji **Importowanie metadanych** wykonaj następujące czynności:

    ![Konfigurowanie logowania jednokrotnego](./media/kantegassoforfisheyecrucible-tutorial/addon7.png)

    a. Wybierz pozycję **plik metadanych na komputerze**i Przekaż plik metadanych, który został pobrany z Azure Portal.

    b. Kliknij przycisk **Dalej**.

1. W sekcji **Nazwa i lokalizacja logowania jednokrotnego** wykonaj następujące czynności:

    ![Konfigurowanie logowania jednokrotnego](./media/kantegassoforfisheyecrucible-tutorial/addon8.png)

    a. Dodaj nazwę dostawcy tożsamości w polu tekstowym **Nazwa dostawcy tożsamości** (np. Azure AD).

    b. Kliknij przycisk **Dalej**.

1. Sprawdź certyfikat podpisywania i kliknij przycisk **dalej**.   

    ![Konfigurowanie logowania jednokrotnego](./media/kantegassoforfisheyecrucible-tutorial/addon9.png)

1. W sekcji **konta użytkowników fisheye** wykonaj następujące czynności:

    ![Konfigurowanie logowania jednokrotnego](./media/kantegassoforfisheyecrucible-tutorial/addon10.png)

    a. **W razie potrzeby wybierz opcję Utwórz użytkowników w katalogu wewnętrznym fisheye** i wprowadź odpowiednią nazwę grupy dla użytkowników (może to być wiele nie. grup oddzielonych przecinkami).

    b. Kliknij przycisk **Dalej**.

1. Kliknij przycisk **Zakończ**.

    ![Konfigurowanie logowania jednokrotnego](./media/kantegassoforfisheyecrucible-tutorial/addon11.png)

1. W sekcji **znane domeny dla usługi Azure AD** wykonaj następujące czynności:  

    ![Konfigurowanie logowania jednokrotnego](./media/kantegassoforfisheyecrucible-tutorial/addon12.png)

    a. Wybierz opcję **znane domeny** w lewym panelu strony.

    b. Wprowadź nazwę domeny w polu tekstowym **znane domeny** .

    c. Kliknij przycisk **Zapisz**.

### <a name="create-an-azure-ad-test-user"></a>Tworzenie użytkownika testowego usługi Azure AD 

W tej sekcji w witrynie Azure Portal utworzysz użytkownika testowego o nazwie Britta Simon.

1. W witrynie Azure Portal w okienku po lewej stronie wybierz pozycję **Azure Active Directory**, wybierz opcję **Użytkownicy**, a następnie wybierz pozycję **Wszyscy użytkownicy**.

    ![Linki „Użytkownicy i grupy” i „Wszyscy użytkownicy”](common/users.png)

2. Wybierz pozycję **nowy użytkownik** w górnej części ekranu.

    ![Przycisk Nowy użytkownik](common/new-user.png)

3. We właściwościach użytkownika wykonaj następujące kroki.

    ![Okno dialogowe Użytkownik](common/user-properties.png)

    a. W polu **Nazwa** wprowadź **BrittaSimon**.
  
    b. W polu **Nazwa użytkownika** wpisz `brittasimon@yourcompanydomain.extension`. Na przykład: BrittaSimon@contoso.com

    c. Zaznacz pole wyboru **Pokaż hasło** i zanotuj wartość wyświetlaną w polu Hasło.

    d. Kliknij przycisk **Utwórz**.

### <a name="assign-the-azure-ad-test-user"></a>Przypisywanie użytkownika testowego usługi Azure AD

W tej sekcji Britta Simon do korzystania z logowania jednokrotnego na platformie Azure przez przyznanie dostępu do logowania jednokrotnego w Kantega dla FishEye/tygiel.

1. W Azure Portal wybierz pozycję **aplikacje dla przedsiębiorstw**, wybierz pozycję **wszystkie aplikacje**, a następnie wybierz pozycję **Kantega Logowanie jednokrotne dla fisheye/tygiel**.

    ![Blok Aplikacje dla przedsiębiorstw](common/enterprise-applications.png)

2. Na liście Aplikacje wybierz pozycję **KANTEGA SSO dla fisheye/tygiel**.

    ![Link SSO Kantega dla FishEye/tygla na liście aplikacji](common/all-applications.png)

3. W menu po lewej stronie wybierz pozycję **Użytkownicy i grupy**.

    ![Link „Użytkownicy i grupy”](common/users-groups-blade.png)

4. Kliknij przycisk **Dodaj użytkownika**, a następnie wybierz pozycję **Użytkownicy i grupy** w oknie dialogowym **Dodawanie przypisania**.

    ![Okienko Dodawanie przypisania](common/add-assign-user.png)

5. W oknie dialogowym **Użytkownicy i grupy** wybierz użytkownika **Britta Simon** na liście użytkowników, a następnie kliknij przycisk **Wybierz** u dołu ekranu.

6. Jeśli oczekujesz, że masz dowolną wartość roli w potwierdzeniu SAML, w oknie dialogowym **Wybierz rolę** wybierz odpowiednią rolę dla użytkownika z listy, a następnie kliknij przycisk **Wybierz** w dolnej części ekranu.

7. W oknie dialogowym **Dodawanie przypisania** kliknij przycisk **Przypisz**.

### <a name="create-kantega-sso-for-fisheyecrucible-test-user"></a>Utwórz Logowanie jednokrotne Kantega dla użytkownika testowego FishEye/tygiel

Aby umożliwić użytkownikom usługi Azure AD logowanie się do FishEye/tygiel, muszą one być obsługiwane w FishEye/tyglu. W Kantega SSO dla FishEye/tygla Inicjowanie obsługi jest zadaniem ręcznym.

**Aby aprowizować konto użytkownika, wykonaj następujące czynności:**

1. Zaloguj się do swojego tygla na serwerze lokalnym jako administrator.

1. Umieść kursor na koło zębate, a następnie kliknij pozycję **Użytkownicy**.

    ![Dodawanie pracownika](./media/kantegassoforfisheyecrucible-tutorial/user1.png)

1. W obszarze Karta **Użytkownicy** kliknij przycisk **Dodaj użytkownika**.

    ![Dodawanie pracownika](./media/kantegassoforfisheyecrucible-tutorial/user2.png)

1. Na stronie **Dodaj nowego użytkownika** wykonaj następujące czynności:

    ![Dodawanie pracownika](./media/kantegassoforfisheyecrucible-tutorial/user3.png)

    a. W polu tekstowym **Username** (Nazwa użytkownika) wpisz adres e-mail użytkownika, na przykład Brittasimon@contoso.com.

    b. W polu tekstowym **Nazwa wyświetlana** wpisz nazwę wyświetlaną użytkownika, taką jak Britta Simon.

    c. W polu tekstowym **Email address** (Adres e-mail) wpisz adres e-mail użytkownika, na przykład Brittasimon@contoso.com.

    d. W polu tekstowym **Password** (Hasło) wpisz hasło użytkownika.

    e. W polu tekstowym **Potwierdź hasło** ponownie wprowadź hasło użytkownika.

    f. Kliknij pozycję **Dodaj**.

### <a name="test-single-sign-on"></a>Testowanie logowania jednokrotnego

W tej sekcji przetestujesz konfigurację logowania jednokrotnego usługi Azure AD przy użyciu panelu dostępu.

Po kliknięciu kafelka SSO Kantega dla FishEye/tygla w panelu dostępu należy automatycznie zalogować się do logowania jednokrotnego Kantega dla FishEye/tygla, dla którego skonfigurowano Logowanie jednokrotne. Aby uzyskać więcej informacji na temat panelu dostępu, zobacz [wprowadzenie do panelu dostępu](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Lista samouczków dotyczących sposobu integrowania aplikacji SaaS z usługą Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Co to jest dostęp do aplikacji i logowanie jednokrotne za pomocą Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Co to jest dostęp warunkowy w Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)