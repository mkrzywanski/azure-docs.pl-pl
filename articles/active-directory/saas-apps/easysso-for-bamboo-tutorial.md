---
title: 'Samouczek: Azure Active Directory integrację logowania jednokrotnego (SSO) z usługą EasySSO dla Bamboo | Microsoft Docs'
description: Dowiedz się, jak skonfigurować Logowanie jednokrotne między Azure Active Directory i EasySSO dla Bamboo.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: f594613d-0cd8-4046-a7c5-2da2971afecd
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 05/28/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7e10daab315fd20fb1f82ada2e17cef28bb6045f
ms.sourcegitcommit: 32592ba24c93aa9249f9bd1193ff157235f66d7e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/01/2020
ms.locfileid: "85608810"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-easysso-for-bamboo"></a>Samouczek: Azure Active Directory integrację logowania jednokrotnego (SSO) z usługą EasySSO dla Bamboo

W tym samouczku dowiesz się, jak zintegrować usługę EasySSO for Bamboo z usługą Azure Active Directory (Azure AD). Po zintegrowaniu usługi EasySSO for Bamboo z usługą Azure AD można:

* Kontrolka w usłudze Azure AD, która ma dostęp do Bamboo.
* Zezwól użytkownikom na automatyczne logowanie się do usługi Bamboo przy użyciu kont w usłudze Azure AD.
* Zarządzaj kontami w jednej centralnej lokalizacji — Azure Portal.

Aby dowiedzieć się więcej o integracji aplikacji SaaS z usługą Azure AD, zobacz [co to jest dostęp do aplikacji i logowanie jednokrotne przy użyciu Azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on).

## <a name="prerequisites"></a>Wymagania wstępne

Aby rozpocząć, potrzebne są następujące elementy:

* Subskrypcja usługi Azure AD. Jeśli nie masz subskrypcji, możesz uzyskać [bezpłatne konto](https://azure.microsoft.com/free/).
* EasySSO dla subskrypcji z obsługą logowania jednokrotnego (SSO) Bamboo.

## <a name="scenario-description"></a>Opis scenariusza

W tym samouczku skonfigurujesz i testujesz Logowanie jednokrotne usługi Azure AD w środowisku testowym.

* Usługa EasySSO dla Bamboo obsługuje usługi **SP i dostawcy tożsamości** zainicjowane przez usługę SSO
* EasySSO dla Bamboo obsługuje inicjowanie aprowizacji użytkowników **just in Time**
* Po skonfigurowaniu usługi EasySSO dla Bamboo można wymusić kontrolę sesji, która chroni eksfiltracji i niefiltrowanie danych poufnych w organizacji w czasie rzeczywistym. Kontrolka sesji rozszerzy od dostępu warunkowego. [Dowiedz się, jak wymuszać kontrolę sesji za pomocą Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).

## <a name="adding-easysso-for-bamboo-from-the-gallery"></a>Dodawanie EasySSO dla Bamboo z galerii

Aby skonfigurować integrację EasySSO dla Bamboo z usługą Azure AD, musisz dodać EasySSO dla Bamboo z galerii do listy zarządzanych aplikacji SaaS.

1. Zaloguj się do [Azure Portal](https://portal.azure.com) przy użyciu konta służbowego lub konto Microsoft prywatnego.
1. W okienku nawigacji po lewej stronie wybierz usługę **Azure Active Directory** .
1. Przejdź do **aplikacji przedsiębiorstwa** , a następnie wybierz pozycję **wszystkie aplikacje**.
1. Aby dodać nową aplikację, wybierz pozycję **Nowa aplikacja**.
1. W sekcji **Dodaj z galerii** wpisz **EasySSO dla Bamboo** w polu wyszukiwania.
1. Wybierz pozycję **EasySSO for Bamboo** from the Results panel, a następnie Dodaj aplikację. Poczekaj kilka sekund, gdy aplikacja zostanie dodana do dzierżawy.


## <a name="configure-and-test-azure-ad-single-sign-on-for-easysso-for-bamboo"></a>Skonfiguruj i przetestuj Logowanie jednokrotne usługi Azure AD dla EasySSO dla Bamboo

Skonfiguruj i przetestuj Logowanie jednokrotne usługi Azure AD za pomocą EasySSO dla Bamboo przy użyciu użytkownika testowego o nazwie **B. Simon**. Aby logowanie jednokrotne działało, należy ustanowić relację linku między użytkownikiem usługi Azure AD i powiązanym użytkownikiem w EasySSO dla Bamboo.

Aby skonfigurować i przetestować Logowanie jednokrotne usługi Azure AD za pomocą EasySSO dla Bamboo, wykonaj następujące bloki konstrukcyjne:

1. **[Skonfiguruj Logowanie jednokrotne usługi Azure AD](#configure-azure-ad-sso)** , aby umożliwić użytkownikom korzystanie z tej funkcji.
    1. **[Utwórz użytkownika testowego usługi Azure AD](#create-an-azure-ad-test-user)** — aby przetestować Logowanie jednokrotne w usłudze Azure AD za pomocą usługi B. Simon.
    1. **[Przypisz użytkownika testowego usługi Azure AD](#assign-the-azure-ad-test-user)** — aby umożliwić usłudze B. Simon korzystanie z logowania jednokrotnego w usłudze Azure AD.
1. **[Skonfiguruj EasySSO dla logowania jednokrotnego](#configure-easysso-for-bamboo-sso)** w usłudze Bamboo, aby skonfigurować ustawienia logowania jednokrotnego na stronie aplikacji.
    1. **[Utwórz EasySSO dla Bamboo użytkownika testowego](#create-easysso-for-bamboo-test-user)** , aby uzyskać odpowiedni odpowiednik B. Simon w EasySSO dla Bamboo, który jest połączony z reprezentacją użytkownika w usłudze Azure AD.
1. **[Przetestuj Logowanie jednokrotne](#test-sso)** — aby sprawdzić, czy konfiguracja działa.

## <a name="configure-azure-ad-sso"></a>Konfigurowanie rejestracji jednokrotnej w usłudze Azure AD

Wykonaj następujące kroki, aby włączyć logowanie jednokrotne usługi Azure AD w Azure Portal.

1. W [Azure Portal](https://portal.azure.com/)na stronie **EasySSO for Bamboo** Application Integration Znajdź sekcję **Zarządzanie** i wybierz pozycję **Logowanie jednokrotne**.
1. Na stronie **Wybierz metodę logowania jednokrotnego** wybierz pozycję **SAML**.
1. Na stronie **Konfigurowanie logowania jednokrotnego przy użyciu języka SAML** kliknij ikonę Edytuj/pióro, aby określić **podstawową konfigurację języka SAML** , aby edytować ustawienia.

   ![Edycja podstawowej konfiguracji protokołu SAML](common/edit-urls.png)

1. Jeśli chcesz skonfigurować aplikację w trybie inicjalizacji **dostawcy tożsamości** , w sekcji **Podstawowa konfiguracja SAML** wprowadź wartości dla następujących pól:

    a. W polu tekstowym **Identyfikator** wpisz adres URL, używając następującego wzorca:`https://<server-base-url>/plugins/servlet/easysso/saml`

    b. W polu tekstowym **Adres URL odpowiedzi** wpisz adres URL, korzystając z następującego wzorca: `https://<server-base-url>/plugins/servlet/easysso/saml`

1. Kliknij pozycję **Ustaw dodatkowe adresy URL** i wykonaj następujące kroki, jeśli chcesz skonfigurować aplikację w trybie inicjowania programu **SP** :

    W polu tekstowym **Adres URL logowania** wpisz adres URL, korzystając z następującego wzorca: `https://<server-base-url>/login.jsp`

    > [!NOTE]
    > Te wartości nie są prawdziwe. Należy je zastąpić rzeczywistymi wartościami identyfikatora, adresu URL odpowiedzi i adresu URL logowania. Skontaktuj się z [zespołem pomocy technicznej EasySSO](mailto:support@techtime.co.nz) , aby uzyskać te wartości w razie wątpliwości. Przydatne mogą się również okazać wzorce przedstawione w sekcji **Podstawowa konfiguracja protokołu SAML** w witrynie Azure Portal.

1. EasySSO for Bamboo oczekuje potwierdzeń SAML w określonym formacie, co wymaga dodania niestandardowych mapowań atrybutów do konfiguracji atrybutów tokenu SAML. Poniższy zrzut ekranu przedstawia listę atrybutów domyślnych.

    ![image (obraz)](common/default-attributes.png)

1. Oprócz powyższych, EasySSO for Bamboo oczekuje kilku atrybutów do przekazania z powrotem w odpowiedzi SAML, które przedstawiono poniżej. Te atrybuty są również wstępnie wypełnione, ale można je sprawdzić zgodnie z wymaganiami.
    
    | Nazwa | Atrybut źródłowy |
    | ---------------| --------- |
    | urn: identyfikator OID: 0.9.2342.19200300.100.1.1 | user.userprincipalname |
    | urn:oid:0.9.2342.19200300.100.1.3 | user.mail |
    | urn: identyfikator OID: 2.16.840.1.113730.3.1.241 | user.displayname |
    | urn:oid:2.5.4.4 | user.surname |
    | urn:oid:2.5.4.42 | user.givenname |
    
    W przypadku, gdy użytkownicy usługi Azure AD mają skonfigurowaną opcję **sAMAccountName** , należy zamapować **nazwę urn: OID: 0.9.2342.19200300.100.1.1** na atrybut **sAMAccountName** .
    
1. Na stronie **Konfigurowanie logowania jednokrotnego za pomocą języka SAML** w sekcji **certyfikat podpisywania SAML** kliknij pozycję **Pobierz** linki dla opcji **certyfikat (base64)** lub **XML metadanych Federacji** i Zapisz lub wszystkie na komputerze. Będzie ona potrzebna później, aby skonfigurować Bamboo EasySSO.

    ![Link do pobierania certyfikatu](./media/easysso-for-bamboo-tutorial/certificate.png)
    
    Jeśli planujesz ręcznie wykonać EasySSO konfigurację Bamboo z certyfikatem, musisz także skopiować **adres URL logowania** i identyfikator usługi **Azure AD** z poniższej sekcji i zapisać je na komputerze.

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

W tej sekcji włączysz usługę B. Simon, aby korzystać z logowania jednokrotnego na platformie Azure przez przyznanie dostępu do EasySSO dla Bamboo.

1. W Azure Portal wybierz pozycję **aplikacje dla przedsiębiorstw**, a następnie wybierz pozycję **wszystkie aplikacje**.
1. Na liście Aplikacje wybierz pozycję **EasySSO dla Bamboo**.
1. Na stronie Przegląd aplikacji Znajdź sekcję **Zarządzanie** i wybierz pozycję **Użytkownicy i grupy**.

   ![Link „Użytkownicy i grupy”](common/users-groups-blade.png)

1. Wybierz pozycję **Dodaj użytkownika**, a następnie w oknie dialogowym **Dodawanie przypisania** wybierz pozycję **Użytkownicy i grupy** .

    ![Link Dodaj użytkownika](common/add-assign-user.png)

1. W oknie dialogowym **Użytkownicy i grupy** wybierz pozycję **B. Simon** z listy Użytkownicy, a następnie kliknij przycisk **Wybierz** w dolnej części ekranu.
1. Jeśli oczekujesz dowolnej wartości roli w potwierdzeniu SAML, w oknie dialogowym **Wybierz rolę** wybierz odpowiednią rolę dla użytkownika z listy, a następnie kliknij przycisk **Wybierz** w dolnej części ekranu.
1. W oknie dialogowym **Dodawanie przypisania** kliknij przycisk **Przypisz** .

## <a name="configure-easysso-for-bamboo-sso"></a>Konfigurowanie EasySSO na potrzeby logowania jednokrotnego Bamboo

1. Zaloguj się do swojego wystąpienia programu Atlassian Bamboo z uprawnieniami administratora i przejdź do sekcji **Zarządzanie aplikacjami** . 

    ![Zarządzanie aplikacjami](./media/easysso-for-bamboo-tutorial/bamboo-admin-1.png)

2. Po lewej stronie Znajdź **EasySSO** i kliknij go.

    ![Łatwe logowanie jednokrotne](./media/easysso-for-bamboo-tutorial/bamboo-admin-2.png)

3. Wybierz opcję **SAML** . Spowoduje to przejście do sekcji Konfiguracja protokołu SAML.

    ![SAML](./media/easysso-for-bamboo-tutorial/bamboo-admin-3.png)

4. Wybierz kartę **Certyfikaty** u góry, a zostanie wyświetlony następujący ekran:

    ![Adres URL metadanych](./media/easysso-for-bamboo-tutorial/bamboo-admin-4.png)

5. Teraz Zlokalizuj **certyfikat (base64)** lub **plik metadanych** zapisany we wcześniejszych krokach konfiguracji **rejestracji jednokrotnej usługi Azure AD** . Dostępne są następujące opcje:

    a. Użyj pobranego **pliku metadanych** Federacji aplikacji do pliku lokalnego na komputerze. Wybierz przycisk radiowy **Przekaż** i postępuj zgodnie z oknem dialogowym przekazywania pliku określonym dla danego systemu operacyjnego

    **ORAZ**

    b. Otwórz **plik metadanych** Federacji aplikacji, aby zobaczyć zawartość (w dowolnym edytorze tekstu) i skopiować ją do Schowka. Wybierz opcję **dane wejściowe** i wklej zawartość schowka do pola tekstowego.
 
    **ORAZ**

    c.  Konfiguracja w pełni ręczna. Otwórz certyfikat Federacji aplikacji **(base64)** , aby zobaczyć zawartość (w dowolnym edytorze tekstu) i skopiować ją do Schowka. Wklej ją do pola tekstowego **certyfikaty podpisywania tokenu dostawcy tożsamości** . Następnie przejdź do karty **Ogólne** i wypełnij **pola adres URL** i **Identyfikator jednostki** z odpowiednimi wartościami dla **adresu URL logowania** i **identyfikatora usługi Azure AD** , które zostały zapisane wcześniej.
 
6. Kliknij przycisk **Zapisz** u dołu strony. Zostanie wyświetlona zawartość metadanych lub plików certyfikatów przeanalizowanych w pola konfiguracji. Konfiguracja EasySSO dla Bamboo została ukończona.

7. W celu uzyskania najlepszego środowiska testowania przejdź do karty **& działanie** i zaznacz opcję **przycisk Zaloguj SAML** . Spowoduje to włączenie osobnego przycisku na ekranie logowania Bamboo w celu przetestowania kompleksowej integracji protokołu SAML usługi Azure AD. Ten przycisk można opuścić i skonfigurować jego położenie, kolor oraz tłumaczenie dla trybu produkcji.

    ![Wygląd &](./media/easysso-for-bamboo-tutorial/bamboo-admin-5.png)

    > [!NOTE]
    > Jeśli masz jakieś problemy, skontaktuj się z [zespołem pomocy technicznej EasySSO](mailto:support@techtime.co.nz).

### <a name="create-easysso-for-bamboo-test-user"></a>Utwórz EasySSO dla użytkownika testowego Bamboo

W tej sekcji użytkownik o nazwie Britta Simon jest tworzony w Bamboo. EasySSO dla Bamboo obsługuje Inicjowanie obsługi użytkowników just in Time, która jest domyślnie **wyłączona** . Aby włączyć Inicjowanie obsługi użytkowników, należy jawnie zaznaczyć opcję **Utwórz użytkownika przy pomyślnym logowaniu** w sekcji Ogólne konfiguracji wtyczki EasySSO. Jeśli użytkownik nie istnieje jeszcze w usłudze Bamboo, zostanie utworzony nowy po uwierzytelnieniu.

Jeśli jednak użytkownik nie chce włączać automatycznego inicjowania obsługi użytkowników przy pierwszym logowaniu użytkownika, musi istnieć w katalogach użytkowników zaplecza wystąpienie Bamboo wykorzystuje, takie jak LDAP lub Atlassian.

![Aprowizowanie użytkowników](./media/easysso-for-bamboo-tutorial/bamboo-admin-6.png)

## <a name="test-sso"></a>Testuj Logowanie jednokrotne 

### <a name="idp-initiated-workflow"></a>Dostawcy tożsamości — przepływ pracy zainicjowany

W tej sekcji przetestujesz konfigurację logowania jednokrotnego usługi Azure AD przy użyciu panelu dostępu.

Po kliknięciu kafelka EasySSO dla Bamboo w panelu dostępu należy automatycznie zalogować się do wystąpienia Bamboo, dla którego skonfigurowano Logowanie jednokrotne. Aby uzyskać więcej informacji na temat panelu dostępu, zobacz [wprowadzenie do panelu dostępu](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

### <a name="sp-initiated-workflow"></a>Przepływ pracy zainicjowany przez program SP

W tej sekcji przetestujesz konfigurację logowania jednokrotnego usługi Azure AD za pomocą przycisku Bamboo **SAML login** .

![Logowanie użytkownika w języku SAML](./media/easysso-for-bamboo-tutorial/bamboo-admin-7.png)

W tym scenariuszu przyjęto, że **przycisk Zaloguj SAML** został włączony na karcie **& wygląd** na stronie Konfiguracja Bamboo EasySSO (Zobacz powyżej). Otwórz adres URL logowania do Bamboo w trybie incognito przeglądarki, aby uniknąć zakłóceń w istniejących sesjach. Kliknij przycisk **zalogowanie SAML** i nastąpi przekierowanie do przepływu uwierzytelniania użytkownika usługi Azure AD. Po pomyślnym zakończeniu nastąpi przekierowanie z powrotem do wystąpienia Bamboo jako uwierzytelniony użytkownik za pośrednictwem protokołu SAML.

Istnieje możliwość, że po przekierowaniu z powrotem z usługi Azure AD może wystąpić Poniższy ekran

![Ekran niepowodzeń EasySSO](./media/easysso-for-bamboo-tutorial/bamboo-admin-8.png)

W takim przypadku musisz postępować zgodnie z [instrukcjami na tej stronie]( https://techtime.co.nz/display/TECHTIME/EasySSO+How+to+get+the+logs#EasySSOHowtogetthelogs-RETRIEVINGTHELOGS) , aby uzyskać dostęp do pliku **Atlassian-Bamboo. log** . Szczegóły błędu będą dostępne przez identyfikator odwołania znaleziony na stronie błędu EasySSO.

Jeśli masz problemy z tworzeniem komunikatów dziennika, skontaktuj się z [zespołem pomocy technicznej EasySSO](mailto:support@techtime.co.nz).

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Lista samouczków dotyczących integrowania aplikacji SaaS z usługą Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Co to jest dostęp warunkowy w Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Wypróbuj EasySSO dla Bamboo z usługą Azure AD](https://aad.portal.azure.com/)

- [Co to jest kontrola sesji w Microsoft Cloud App Security?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Jak chronić EasySSO dla Bamboo z zaawansowaną widocznością i kontrolkami](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)