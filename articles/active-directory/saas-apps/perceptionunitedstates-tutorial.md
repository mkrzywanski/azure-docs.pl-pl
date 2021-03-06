---
title: 'Samouczek: integracja Azure Active Directory z percepcją Stany Zjednoczone (UltiPro) | Microsoft Docs'
description: Dowiedz się, jak skonfigurować Logowanie jednokrotne między Azure Active Directory i percepcją Stany Zjednoczone (UltiPro).
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: b4a8f026-cb5f-41eb-9680-68eddc33565e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/25/2019
ms.author: jeedes
ms.openlocfilehash: e9ba42f780c93486409077383750d0635637e99b
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/29/2020
ms.locfileid: "67094840"
---
# <a name="tutorial-azure-active-directory-integration-with-perception-united-states-non-ultipro"></a>Samouczek: integracja Azure Active Directory z percepcją Stany Zjednoczone (UltiPro)

W tym samouczku dowiesz się, jak zintegrować Stany Zjednoczone percepcję (UltiPro) z Azure Active Directory (Azure AD).
Integrowanie Stany Zjednoczone percepcji (UltiPro) z usługą Azure AD zapewnia następujące korzyści:

* Możesz kontrolować w usłudze Azure AD, kto ma dostęp do percepcji Stany Zjednoczone (UltiPro).
* Możesz pozwolić użytkownikom na automatyczne logowanie się do percepcji Stany Zjednoczone (UltiPro) (Logowanie jednokrotne) przy użyciu kont usługi Azure AD.
* Możesz zarządzać swoimi kontami w jednej centralnej lokalizacji — witrynie Azure Portal.

Jeśli chcesz dowiedzieć się więcej na temat integracji aplikacji SaaS z usługą Azure AD, zobacz [Co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem [Utwórz bezpłatne konto](https://azure.microsoft.com/free/) .

## <a name="prerequisites"></a>Wymagania wstępne

Aby skonfigurować integrację usługi Azure AD z percepcją Stany Zjednoczone (UltiPro), potrzebne są następujące elementy:

* Subskrypcja usługi Azure AD. Jeśli nie masz środowiska usługi Azure AD, możesz skorzystać z miesięcznej wersji próbnej [tutaj](https://azure.microsoft.com/pricing/free-trial/)
* Stany Zjednoczone percepcji — subskrypcja z włączonym logowaniem jednokrotnym (UltiPro)

## <a name="scenario-description"></a>Opis scenariusza

W tym samouczku skonfigurujesz i przetestujesz logowanie jednokrotne usługi Azure AD w środowisku testowym.

* Stany Zjednoczone percepcji (inne niż UltiPro) obsługuje **dostawcy tożsamości** zainicjowane przez logowanie jednokrotne

## <a name="adding-perception-united-states-non-ultipro-from-the-gallery"></a>Dodawanie percepcji Stany Zjednoczone (UltiPro) z galerii

Aby skonfigurować integrację percepcji Stany Zjednoczone (UltiPro) z usługą Azure AD, musisz dodać percepcję Stany Zjednoczone (UltiPro) z galerii do listy zarządzanych aplikacji SaaS.

**Aby dodać percepcję Stany Zjednoczone (UltiPro) z galerii, wykonaj następujące czynności:**

1. W witrynie **[Azure Portal](https://portal.azure.com)** w panelu nawigacyjnym po lewej stronie kliknij ikonę usługi **Azure Active Directory**.

    ![Przycisk Azure Active Directory](common/select-azuread.png)

2. Przejdź do grupy **Aplikacje dla przedsiębiorstw** i wybierz opcję **Wszystkie aplikacje**.

    ![Blok Aplikacje dla przedsiębiorstw](common/enterprise-applications.png)

3. Aby dodać nową aplikację, kliknij przycisk **Nowa aplikacja** w górnej części okna dialogowego.

    ![Przycisk Nowa aplikacja](common/add-new-app.png)

4. W polu wyszukiwania wpisz **percepcję Stany Zjednoczone (UltiPro)**, wybierz pozycję **percepcja Stany Zjednoczone (UltiPro)** w panelu wyników, a następnie kliknij przycisk **Dodaj** , aby dodać aplikację.

     ![Stany Zjednoczone percepcji (inne niż UltiPro) na liście wyników](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurowanie i testowanie logowania jednokrotnego usługi Azure AD

W tej sekcji konfigurujesz i testujesz Logowanie jednokrotne w usłudze Azure AD z percepcją Stany Zjednoczone (UltiPro) na podstawie użytkownika testowego o nazwie **Britta Simon**.
Aby logowanie jednokrotne działało, należy ustanowić relację linku między użytkownikiem usługi Azure AD i powiązanym użytkownikiem w Stany Zjednoczone percepcji (UltiPro).

Aby skonfigurować i przetestować Logowanie jednokrotne w usłudze Azure AD z percepcją Stany Zjednoczone (UltiPro), należy wykonać następujące bloki konstrukcyjne:

1. **[Konfigurowanie logowania jednokrotnego usługi Azure AD](#configure-azure-ad-single-sign-on)** — aby umożliwić użytkownikom korzystanie z tej funkcji.
2. **[Skonfiguruj Stany Zjednoczone percepcji (UltiPro) logowanie](#configure-perception-united-states-non-ultipro-single-sign-on)** jednokrotne — aby skonfigurować ustawienia logowania jednokrotnego na stronie aplikacji.
3. **[Tworzenie użytkownika testowego usługi Azure AD](#create-an-azure-ad-test-user)** — aby przetestować logowanie jednokrotne usługi Azure AD z użytkownikiem Britta Simon.
4. **[Przypisywanie użytkownika testowego usługi Azure AD](#assign-the-azure-ad-test-user)** — aby umożliwić użytkownikowi Britta Simon korzystanie z logowania jednokrotnego usługi Azure AD.
5. **[Twórz percepcję Stany Zjednoczone użytkownika testowego (UltiPro)](#create-perception-united-states-non-ultipro-test-user)** , aby uzyskać odpowiednik Britta Simon w postrzeganiu Stany Zjednoczone (nie UltiPro), który jest połączony z reprezentacją użytkownika w usłudze Azure AD.
6. **[Testowanie logowania jednokrotnego](#test-single-sign-on)** — aby sprawdzić, czy konfiguracja działa.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurowanie logowania jednokrotnego usługi Azure AD

W tej sekcji włączysz logowanie jednokrotne usługi Azure AD w witrynie Azure Portal.

Aby skonfigurować Logowanie jednokrotne w usłudze Azure AD z percepcją Stany Zjednoczone (UltiPro), wykonaj następujące czynności:

1. W [Azure Portal](https://portal.azure.com/)na stronie integracja aplikacji **Stany Zjednoczone (UltiPro)** wybierz pozycję **Logowanie jednokrotne**.

    ![Link do konfigurowania logowania jednokrotnego](common/select-sso.png)

2. W oknie dialogowym **Wybieranie metody logowania jednokrotnego** wybierz tryb **SAML/WS-Fed**, aby włączyć logowanie jednokrotne.

    ![Wybieranie trybu logowania jednokrotnego](common/select-saml-option.png)

3. Na stronie **Konfigurowanie logowania jednokrotnego za pomocą protokołu SAML** kliknij ikonę **Edytuj**, aby otworzyć okno dialogowe **Podstawowa konfiguracja protokołu SAML**.

    ![Edycja podstawowej konfiguracji protokołu SAML](common/edit-urls.png)

4. Na stronie **Konfigurowanie logowania jednokrotnego za pomocą protokołu SAML** wykonaj następujące kroki:

    ![Postrzeganie Stany Zjednoczone (nie UltiPro) domeny i adresów URL Logowanie jednokrotne](common/idp-intiated.png)

    a. W polu tekstowym **Identyfikator** wpisz adres URL:`https://perception.kanjoya.com/sp`

    b. W polu tekstowym **Adres URL odpowiedzi** wpisz adres URL, korzystając z następującego wzorca: `https://perception.kanjoya.com/sso?idp=<entity_id>`

    c. **Postrzeganie Stany Zjednoczone (UltiPro)** wymaga, aby wartość **identyfikatora usługi Azure AD** była <entity_id>, którą otrzymasz od sekcji **Ustawianie percepcji Stany Zjednoczone (innej niż UltiPro)** , aby była zakodowana przy użyciu identyfikatora URI. Aby uzyskać wartość zakodowaną URI, użyj następującego linku: **http://www.url-encode-decode.com/**.

    d. Po uzyskaniu wartości zakodowanej URI połącz ją z **adresem URL odpowiedzi** , jak wspomniano poniżej —

    `https://perception.kanjoya.com/sso?idp=<URI encooded entity_id>`
    
    e. Wklej powyższą wartość w polu tekstowym **adres URL odpowiedzi** .

5. Na stronie **Konfigurowanie logowania jednokrotnego za pomocą protokołu SAML** w sekcji **Certyfikat podpisywania SAML** kliknij link **Pobierz**, aby pobrać **kod XML metadanych federacji** na podstawie podanych opcji zgodnie z wymaganiami i zapisać go na komputerze.

    ![Link do pobierania certyfikatu](common/metadataxml.png)

6. W sekcji **Ustawianie percepcji Stany Zjednoczone (UltiPro)** skopiuj odpowiednie adresy URL zgodnie z wymaganiami.

    ![Kopiowanie adresów URL konfiguracji](common/copy-configuration-urls.png)

    a. Adres URL logowania

    b. Identyfikator usługi Azure AD

    c. Adres URL wylogowywania   

### <a name="configure-perception-united-states-non-ultipro-single-sign-on"></a>Skonfiguruj Stany Zjednoczone percepcji (UltiPro) Logowanie jednokrotne

1. W innym oknie przeglądarki Zaloguj się w witrynie firmowej Stany Zjednoczone (UltiPro) jako administrator.

2. Na głównym pasku narzędzi kliknij pozycję **Ustawienia konta**.

    ![Stany Zjednoczone użytkownik (inny niż UltiPro)](./media/perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_user.png)

3. Na stronie **Ustawienia konta** wykonaj następujące czynności:

    ![Stany Zjednoczone użytkownik (inny niż UltiPro)](./media/perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_account.png)

    a. W polu tekstowym **Nazwa firmy** wpisz nazwę **firmy**.
    
    b. W polu tekstowym **nazwa konta** wpisz nazwę **konta**.

    c. W polu tekstowym **adres E-mail odpowiedzi domyślnej** wpisz prawidłowy **adres e-mail**.

    d. Wybierz pozycję **dostawca tożsamości logowania jednokrotnego** jako element **SAML 2,0**.

4. Na stronie **Konfiguracja logowania jednokrotnego** wykonaj następujące czynności:

    ![Stany Zjednoczone percepcji (UltiPro) SSOConfig](./media/perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_ssoconfig.png)

    a. Wybierz pozycję **SAML NameID Type** jako **wiadomość e-mail**.

    b. W polu tekstowym **nazwa konfiguracji logowania jednokrotnego** wpisz nazwę **konfiguracji**.
    
    c. W polu tekstowym **Nazwa dostawcy tożsamości** wklej wartość identyfikatora usługi **Azure AD**, która została skopiowana z Azure Portal. 

    d. W polu **tekstowym domena SAML**wprowadź domenę, taką @contoso.comjak.

    e. Kliknij **ponownie przycisk Przekaż** , aby przekazać plik **XML metadanych** .

    f. Kliknij przycisk **Update** (Aktualizuj).

### <a name="create-an-azure-ad-test-user"></a>Tworzenie użytkownika testowego usługi Azure AD 

W tej sekcji w witrynie Azure Portal utworzysz użytkownika testowego o nazwie Britta Simon.

1. W witrynie Azure Portal w okienku po lewej stronie wybierz pozycję **Azure Active Directory**, wybierz opcję **Użytkownicy**, a następnie wybierz pozycję **Wszyscy użytkownicy**.

    ![Linki „Użytkownicy i grupy” i „Wszyscy użytkownicy”](common/users.png)

2. Wybierz pozycję **nowy użytkownik** w górnej części ekranu.

    ![Przycisk Nowy użytkownik](common/new-user.png)

3. We właściwościach użytkownika wykonaj następujące kroki.

    ![Okno dialogowe Użytkownik](common/user-properties.png)

    a. W polu **Nazwa** wprowadź **BrittaSimon**.
  
    b. W polu **Nazwa użytkownika** wpisz brittasimon@yourcompanydomain.extension. Na przykład: BrittaSimon@contoso.com

    c. Zaznacz pole wyboru **Pokaż hasło** i zanotuj wartość wyświetlaną w polu Hasło.

    d. Kliknij przycisk **Utwórz**.

### <a name="assign-the-azure-ad-test-user"></a>Przypisywanie użytkownika testowego usługi Azure AD

W tej sekcji Britta Simon do korzystania z logowania jednokrotnego na platformie Azure przez przyznanie dostępu Stany Zjednoczone percepcji (UltiPro).

1. W Azure Portal wybierz pozycję **aplikacje dla przedsiębiorstw**, wybierz pozycję **wszystkie aplikacje**, a następnie wybierz pozycję **percepcja Stany Zjednoczone (nie UltiPro)**.

    ![Blok Aplikacje dla przedsiębiorstw](common/enterprise-applications.png)

2. Na liście Aplikacje wybierz pozycję **percepcja Stany Zjednoczone (nie UltiPro)**.

    ![Stany Zjednoczone percepcji (inne niż UltiPro) na liście aplikacji](common/all-applications.png)

3. W menu po lewej stronie wybierz pozycję **Użytkownicy i grupy**.

    ![Link „Użytkownicy i grupy”](common/users-groups-blade.png)

4. Kliknij przycisk **Dodaj użytkownika**, a następnie wybierz pozycję **Użytkownicy i grupy** w oknie dialogowym **Dodawanie przypisania**.

    ![Okienko Dodawanie przypisania](common/add-assign-user.png)

5. W oknie dialogowym **Użytkownicy i grupy** wybierz użytkownika **Britta Simon** na liście użytkowników, a następnie kliknij przycisk **Wybierz** u dołu ekranu.

6. Jeśli oczekujesz, że masz dowolną wartość roli w potwierdzeniu SAML, w oknie dialogowym **Wybierz rolę** wybierz odpowiednią rolę dla użytkownika z listy, a następnie kliknij przycisk **Wybierz** w dolnej części ekranu.

7. W oknie dialogowym **Dodawanie przypisania** kliknij przycisk **Przypisz**.

### <a name="create-perception-united-states-non-ultipro-test-user"></a>Utwórz użytkownika testowego Stany Zjednoczone (nie UltiPro)

W tej sekcji utworzysz użytkownika o nazwie Britta Simon w polu percepcja Stany Zjednoczone (bez UltiPro). Pracuj z [zespołem pomocy technicznej Stany Zjednoczone (UltiPro)](https://www.ultimatesoftware.com/Contact/ContactUs) , aby dodać użytkowników na platformie percepcji Stany Zjednoczone (nie UltiPro).

### <a name="test-single-sign-on"></a>Testowanie logowania jednokrotnego 

W tej sekcji przetestujesz konfigurację logowania jednokrotnego usługi Azure AD przy użyciu panelu dostępu.

Po kliknięciu kafelka Stany Zjednoczone (bez UltiPro) w panelu dostępu należy automatycznie zalogować się do Stany Zjednoczone percepcji (UltiPro), dla którego skonfigurowano Logowanie jednokrotne. Aby uzyskać więcej informacji na temat panelu dostępu, zobacz [wprowadzenie do panelu dostępu](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Lista samouczków dotyczących sposobu integrowania aplikacji SaaS z usługą Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Co to jest dostęp do aplikacji i logowanie jednokrotne za pomocą Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Co to jest dostęp warunkowy w Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

