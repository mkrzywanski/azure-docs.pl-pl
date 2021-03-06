---
title: 'Samouczek: integracja Azure Active Directory z programem Palo Alto Networks — interfejs użytkownika administratora | Microsoft Docs'
description: Dowiedz się, jak skonfigurować logowanie jednokrotne między usługą Azure Active Directory i aplikacją Palo Alto Networks - Admin UI.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: a826eaec-15af-4c85-8855-8a3374d1efb9
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 03/12/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: fbfa16223484928dda1004011d2e92295edd8b89
ms.sourcegitcommit: 4042aa8c67afd72823fc412f19c356f2ba0ab554
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/24/2020
ms.locfileid: "85297261"
---
# <a name="tutorial-azure-active-directory-integration-with-palo-alto-networks---admin-ui"></a>Samouczek: integracja Azure Active Directory z programem Palo Alto Networks — interfejs użytkownika administratora

W tym samouczku dowiesz się, jak zintegrować aplikację Palo Alto Networks - Admin UI z usługą Azure Active Directory (Azure AD).
Zintegrowanie aplikacji Palo Alto Networks - Admin UI z usługą Azure AD zapewnia następujące korzyści:

* W usłudze Azure AD możesz kontrolować, kto ma dostęp do aplikacji Palo Alto Networks - Admin UI.
* Możesz umożliwić swoim użytkownikom automatyczne logowanie do aplikacji Palo Alto Networks - Admin UI (logowanie jednokrotne) przy użyciu kont usługi Azure AD.
* Możesz zarządzać swoimi kontami w jednej centralnej lokalizacji — witrynie Azure Portal.

Jeśli chcesz dowiedzieć się więcej na temat integracji aplikacji SaaS z usługą Azure AD, zobacz [Co to jest dostęp do aplikacji i logowanie jednokrotne z usługą Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on).
Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem [Utwórz bezpłatne konto](https://azure.microsoft.com/free/) .

## <a name="prerequisites"></a>Wymagania wstępne

Aby skonfigurować integrację usługi Azure AD z aplikacją Palo Alto Networks - Admin UI, potrzebne są następujące elementy:

* Subskrypcja usługi Azure AD. Jeśli nie masz środowiska usługi Azure AD, możesz skorzystać z miesięcznej wersji próbnej [tutaj](https://azure.microsoft.com/pricing/free-trial/)
* Subskrypcja aplikacji Palo Alto Networks - Admin UI z włączonym logowaniem jednokrotnym

## <a name="scenario-description"></a>Opis scenariusza

W tym samouczku skonfigurujesz i przetestujesz logowanie jednokrotne usługi Azure AD w środowisku testowym.

* Aplikacja Palo Alto Networks - Admin UI obsługuje logowanie jednokrotne inicjowane przez **dostawcę usług**
* Aplikacja Palo Alto Networks - Admin UI obsługuje aprowizację użytkownika **Just in time**

## <a name="adding-palo-alto-networks---admin-ui-from-the-gallery"></a>Dodawanie aplikacji Palo Alto Networks - Admin UI z galerii

Aby skonfigurować integrację aplikacji Palo Alto Networks - Admin UI w usłudze Azure AD, należy dodać pozycję Palo Alto Networks - Admin UI z galerii do listy zarządzanych aplikacji SaaS.

1. Zaloguj się do [Azure Portal](https://portal.azure.com) przy użyciu konta służbowego lub konto Microsoft prywatnego.
1. W okienku nawigacji po lewej stronie wybierz usługę **Azure Active Directory** .
1. Przejdź do **aplikacji przedsiębiorstwa** , a następnie wybierz pozycję **wszystkie aplikacje**.
1. Aby dodać nową aplikację, wybierz pozycję **Nowa aplikacja**.
1. W sekcji **Dodaj z galerii** wpisz **Palo Alto Networks-admin UI** w polu wyszukiwania.
1. Wybierz kolejno opcje **Palo Alto Networks — interfejs użytkownika administratora** z panelu wyniki, a następnie Dodaj aplikację. Poczekaj kilka sekund, gdy aplikacja zostanie dodana do dzierżawy.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurowanie i testowanie logowania jednokrotnego usługi Azure AD

W tej sekcji skonfigurujesz i testujesz Logowanie jednokrotne usługi Azure AD za pomocą interfejsu użytkownika Palo Alto Networks — administratora na podstawie użytkownika testowego o nazwie **B. Simon**.
Aby logowanie jednokrotne działało, należy ustanowić relację połączenia między użytkownikiem usługi Azure AD i powiązanym użytkownikiem aplikacji Palo Alto Networks - Admin UI.

W celu skonfigurowania i przetestowania logowania jednokrotnego usługi Azure AD z aplikacją Palo Alto Networks - Admin UI należy wykonać poniższe bloki konstrukcyjne:

1. **[Skonfiguruj Logowanie jednokrotne usługi Azure AD](#configure-azure-ad-sso)** , aby umożliwić użytkownikom korzystanie z tej funkcji.
    * **[Utwórz użytkownika testowego usługi Azure AD](#create-an-azure-ad-test-user)** — aby przetestować Logowanie jednokrotne w usłudze Azure AD za pomocą usługi B. Simon.
    * **[Przypisz użytkownika testowego usługi Azure AD](#assign-the-azure-ad-test-user)** — aby umożliwić usłudze B. Simon korzystanie z logowania jednokrotnego w usłudze Azure AD.
1. **[Skonfiguruj Palo Alto Networks — administrator logowania jednokrotnego interfejsu użytkownika](#configure-palo-alto-networks---admin-ui-sso)** — aby skonfigurować ustawienia logowania jednokrotnego na stronie aplikacji.
    * **[Utwórz Palo Alto Networks — administrator interfejsu użytkownika testowego](#create-palo-alto-networks---admin-ui-test-user)** — ma odpowiedni odpowiednik B. Simon w Palo Alto Networks — interfejs użytkownika administratora, który jest połączony z reprezentacją usługi Azure AD użytkownika.
1. **[Przetestuj Logowanie jednokrotne](#test-sso)** — aby sprawdzić, czy konfiguracja działa.

### <a name="configure-azure-ad-sso"></a>Konfigurowanie rejestracji jednokrotnej w usłudze Azure AD

W tej sekcji włączysz logowanie jednokrotne usługi Azure AD w witrynie Azure Portal.

Aby skonfigurować logowanie jednokrotne usługi Azure AD z aplikacją Palo Alto Networks - Admin UI, należy wykonać następujące kroki:

1. W witrynie [Azure Portal](https://portal.azure.com/) na stronie integracji aplikacji **Palo Alto Networks - Admin UI** wybierz opcję **Logowanie jednokrotne**.

    ![Link do konfigurowania logowania jednokrotnego](common/select-sso.png)

1. W oknie dialogowym **Wybieranie metody logowania jednokrotnego** wybierz tryb **SAML/WS-Fed**, aby włączyć logowanie jednokrotne.

    ![Wybieranie trybu logowania jednokrotnego](common/select-saml-option.png)

1. Na stronie **Konfigurowanie logowania jednokrotnego za pomocą protokołu SAML** kliknij ikonę **Edytuj**, aby otworzyć okno dialogowe **Podstawowa konfiguracja protokołu SAML**.

    ![Edycja podstawowej konfiguracji protokołu SAML](common/edit-urls.png)

1. W sekcji **Podstawowa konfiguracja protokołu SAML** wykonaj następujące czynności:

    a. W polu tekstowym **adres URL logowania** wpisz adres URL, używając następującego wzorca:`https://<Customer Firewall FQDN>/php/login.php`

    b. W polu **Identyfikator** wpisz adres URL, używając następującego wzorca:`https://<Customer Firewall FQDN>:443/SAML20/SP`

    c. W polu tekstowym **Adres URL odpowiedzi** wpisz adres URL usługi Assertion Consumer Service (ACS) w następującym formacie: `https://<Customer Firewall FQDN>:443/SAML20/SP/ACS`

    > [!NOTE]
    > Te wartości nie są prawdziwe. Zastąp je rzeczywistymi wartościami adresu URL logowania, identyfikatora i adresu URL odpowiedzi. Aby uzyskać te wartości, skontaktuj się z [zespołem pomocy technicznej aplikacji Palo Alto Networks - Admin UI](https://support.paloaltonetworks.com/support). Przydatne mogą się również okazać wzorce przedstawione w sekcji **Podstawowa konfiguracja protokołu SAML** w witrynie Azure Portal.
    >
    > Port 443 jest wymagany w **identyfikatorze** i **adresie URL odpowiedzi** , ponieważ te wartości są stałe w zaporze Palo Alto. Usunięcie numeru portu spowoduje wystąpienie błędu podczas logowania, jeśli został usunięty.

    > Port 443 jest wymagany w **identyfikatorze** i **adresie URL odpowiedzi** , ponieważ te wartości są stałe w zaporze Palo Alto. Usunięcie numeru portu spowoduje wystąpienie błędu podczas logowania, jeśli został usunięty.

1. Aplikacja interfejsu użytkownika Palo Alto Networks — administrator oczekuje potwierdzeń SAML w określonym formacie, co wymaga dodania niestandardowych mapowań atrybutów do konfiguracji atrybutów tokenu SAML. Poniższy zrzut ekranu przedstawia listę atrybutów domyślnych.

    ![image (obraz)](common/default-attributes.png)

   > [!NOTE]
   > Ponieważ wartości atrybutów są jedynie przykładowe, zamapuj odpowiednie wartości dla atrybutów *nazwa użytkownika* i *rola administratora*. Istnieje inny opcjonalny atrybut *domena dostępu*, który jest używany do ograniczania dostępu administratora do określonych systemów wirtualnego na zaporze.

1. Oprócz powyższych, aplikacja interfejsu użytkownika Palo Alto Networks — administrator oczekuje kilku atrybutów do przekazania z powrotem w odpowiedzi SAML, które przedstawiono poniżej. Te atrybuty są również wstępnie wypełnione, ale można je sprawdzić zgodnie z wymaganiami.

    | Nazwa |  Atrybut źródłowy|
    | --- | --- |
    | nazwa użytkownika | user.userprincipalname |
    | rola administratora | customadmin |
    | | |

    > [!NOTE]
    > Aby uzyskać więcej informacji o atrybutach, zobacz następujące artykuły:
    > * [Administrative role profile for Admin UI (adminrole)](https://www.paloaltonetworks.com/documentation/80/pan-os/pan-os/firewall-administration/manage-firewall-administrators/configure-an-admin-role-profile) (Profil roli administracyjnej dla aplikacji Admin UI (adminrole))
    > * [Device access domain for Admin UI (accessdomain)](https://docs.paloaltonetworks.com/pan-os/8-0/pan-os-web-interface-help/device/device-access-domain.html) (Domena dostępu do urządzeń dla aplikacji Admin UI (accessdomain))

1. Na stronie **Konfigurowanie logowania jednokrotnego za pomocą protokołu SAML** w sekcji **Certyfikat podpisywania SAML** kliknij link **Pobierz**, aby pobrać **kod XML metadanych federacji** na podstawie podanych opcji zgodnie z wymaganiami i zapisać go na komputerze.

    ![Link do pobierania certyfikatu](common/metadataxml.png)

1. W sekcji **Skonfiguruj aplikację Palo Alto Networks - Admin UI** skopiuj odpowiednie adresy URL zgodnie z wymaganiami.

    ![Kopiowanie adresów URL konfiguracji](common/copy-configuration-urls.png)

    a. Adres URL logowania

    b. Identyfikator usługi Azure AD

    c. Adres URL wylogowywania

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

W tej sekcji włączysz usługę B. Simon, aby korzystać z logowania jednokrotnego na platformie Azure przez przyznanie dostępu do interfejsu użytkownika Palo Alto Networks.

1. W Azure Portal wybierz pozycję **aplikacje dla przedsiębiorstw**, a następnie wybierz pozycję **wszystkie aplikacje**.
1. Na liście aplikacji wybierz pozycję **Palo Alto Networks - Admin UI**.
1. Na stronie Przegląd aplikacji Znajdź sekcję **Zarządzanie** i wybierz pozycję **Użytkownicy i grupy**.

   ![Link „Użytkownicy i grupy”](common/users-groups-blade.png)

1. Wybierz pozycję **Dodaj użytkownika**, a następnie w oknie dialogowym **Dodawanie przypisania** wybierz pozycję **Użytkownicy i grupy** .

    ![Link Dodaj użytkownika](common/add-assign-user.png)

1. W oknie dialogowym **Użytkownicy i grupy** wybierz pozycję **B. Simon** z listy Użytkownicy, a następnie kliknij przycisk **Wybierz** w dolnej części ekranu.
1. Jeśli oczekujesz dowolnej wartości roli w potwierdzeniu SAML, w oknie dialogowym **Wybierz rolę** wybierz odpowiednią rolę dla użytkownika z listy, a następnie kliknij przycisk **Wybierz** w dolnej części ekranu.
1. W oknie dialogowym **Dodawanie przypisania** kliknij przycisk **Przypisz** .

### <a name="configure-palo-alto-networks---admin-ui-sso"></a>Konfigurowanie Palo Alto Networks — administrator logowania jednokrotnego interfejsu użytkownika

1. W innym oknie przeglądarki otwórz interfejs użytkownika administratora zapory w aplikacji Palo Alto Networks z uprawnieniami administracyjnymi.

2. Wybierz kartę **Device** (Urządzenie).

    ![Karta Device (Urządzenie)](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_admin1.png)

3. W okienku po lewej stronie wybierz pozycję **SAML Identity Provider** (Dostawca tożsamości SAML), a następnie wybierz przycisk **Import** (Importuj), aby zaimportować plik metadanych.

    ![Przycisk importowania pliku metadanych](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_admin2.png)

4. W oknie **SAML Identity Provider Server Profile Import** (Importowanie profilu serwera dostawcy tożsamości SAML) wykonaj następujące czynności:

    ![Okno „SAML Identity Provider Server Profile Import” (Importowanie profilu serwera dostawcy tożsamości SAML)](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_idp.png)

    a. W polu **Profile Name** (Nazwa profilu) podaj nazwę (na przykład **AzureAD Admin UI**).

    b. W obszarze **Identity Provider Metadata** (Metadane dostawcy tożsamości) wybierz pozycję **Browse** (Przeglądaj), a następnie wybierz plik metadata.xml, który został wcześniej pobrany z witryny Azure Portal.

    c. Wyczyść pole wyboru **Validate Identity Provider Certificate** (Sprawdź poprawność certyfikatu dostawcy tożsamości).

    d. Wybierz przycisk **OK**.

    e. Aby zatwierdzić konfiguracje zapory, wybierz pozycję **Commit** (Zatwierdź).

5. W okienku po lewej stronie wybierz pozycję **SAML Identity Provider** (Dostawca tożsamości SAML), a następnie wybierz profil dostawcy tożsamości SAML (na przykład **AzureAD Admin UI**) utworzony w poprzednim kroku.

    ![Profil dostawcy tożsamości SAML](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_idp_select.png)

6. W oknie **SAML Identity Provider Server Profile** (Profil serwera dostawcy tożsamości SAML) wykonaj następujące czynności:

    ![Okno „SAML Identity Provider Server Profile” (Profil serwera dostawcy tożsamości SAML)](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_slo.png)
  
    a. W polu **Identity Provider SLO URL** (Adres URL wylogowania jednokrotnego dostawcy tożsamości) zastąp poprzednio zaimportowany adres URL wylogowania jednokrotnego następującym adresem URL: `https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0`
  
    b. Wybierz przycisk **OK**.

7. W interfejsie użytkownika administratora zapory Palo Alto Networks wybierz pozycję **Device** (Urządzenie), a następnie wybierz pozycję **Admin Roles** (Role administratora).

8. Wybierz przycisk **Add** (Dodaj).

9. W oknie **Admin Role Profile** (Profil roli administratora) w polu **Name** (Nazwa) podaj nazwę roli administratora (na przykład **fwadmin**). Nazwa roli administratora powinna być zgodna z nazwą atrybutu roli administratora SAML, która została wysłana przez dostawcę tożsamości. Wartość i nazwa roli administratora zostały utworzone w sekcji **Atrybuty użytkownika** w witrynie Azure Portal.

    ![Konfigurowanie roli administratora oprogramowania Palo Alto Networks](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_adminrole.png)
  
10. W interfejsie użytkownika administratora zapory wybierz pozycję **Device** (Urządzenie), a następnie wybierz pozycję **Authentication Profile** (Profil uwierzytelniania).

11. Wybierz przycisk **Add** (Dodaj).

12. W oknie **Authentication Profile** (Profil uwierzytelniania) wykonaj następujące czynności: 

    ![Okno „Authentication Profile” (Profil uwierzytelniania)](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_authentication_profile.png)

    a. W polu **Name** (Nazwa) podaj nazwę (na przykład **AzureSAML_Admin_AuthProfile**).

    b. Z listy rozwijanej **Type** (Typ) wybierz pozycję **SAML**. 

    c. Na liście rozwijanej **IdP Server Profile** (Profil serwera dostawcy tożsamości) wybierz odpowiedni profil serwera dostawcy tożsamości SAML (na przykład **AzureAD Admin UI**).

    c. Zaznacz pole wyboru **Enable Single Logout** (Włącz wylogowanie jednokrotne).

    d. W polu **Admin Role Attribute** (Atrybut roli administratora) wprowadź nazwę atrybutu (na przykład **adminrole**).

    e. Wybierz kartę **Advanced** (Zaawansowane), a następnie w obszarze **Allow List** (Lista dozwolonych) wybierz pozycję **Add** (Dodaj).

    ![Przycisk Add (Dodaj) na karcie Advanced (Zaawansowane)](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_allowlist.png)

    f. Zaznacz pole wyboru **All** (Wszystko) lub wybierz użytkowników i grupy, które mogą uwierzytelniać się za pomocą tego profilu.  
    Podczas uwierzytelniania użytkownika zapora dopasowuje skojarzoną nazwę użytkownika lub grupę do wpisów na tej liście. Jeśli nie dodasz wpisów, żaden użytkownik nie będzie mógł się uwierzytelnić.

    g. Wybierz przycisk **OK**.

13. Aby umożliwić administratorom używanie logowania jednokrotnego SAML przy użyciu **Device**platformy Azure, wybierz pozycję  >  **Konfiguracja**urządzenia. W okienku **Setup** (Konfiguracja) wybierz kartę **Management** (Zarządzanie), a następnie w obszarze **Authentication Settings** (Ustawienia uwierzytelniania) wybierz przycisk **Settings** (Ustawienia), czyli koło zębate.

    ![Przycisk Settings (Ustawienia)](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_authsetup.png)

14. Wybierz profil uwierzytelnianie SAML, który został utworzony w oknie profilu uwierzytelniania (na przykład **AzureSAML_Admin_AuthProfile**).

    ![Pole Authentication Profile (Profil uwierzytelniania)](./media/paloaltoadmin-tutorial/tutorial_paloaltoadmin_authsettings.png)

15. Wybierz przycisk **OK**.

16. Aby zatwierdzić konfigurację, wybierz pozycję **Commit** (Zatwierdź).

### <a name="create-palo-alto-networks---admin-ui-test-user"></a>Tworzenie użytkownika testowego aplikacji Palo Alto Networks - Admin UI

Aplikacja Palo Alto Networks - Admin UI obsługuje aprowizację użytkownika „just-in-time”. Jeśli użytkownik jeszcze nie istnieje, zostanie automatycznie utworzony w systemie po pomyślnym uwierzytelnieniu. Nie musisz nic robić, aby utworzyć użytkownika.

### <a name="test-sso"></a>Testuj Logowanie jednokrotne

W tej sekcji przetestujesz konfigurację logowania jednokrotnego usługi Azure AD przy użyciu panelu dostępu.

Po kliknięciu kafelka Palo Alto Networks - Admin UI w panelu dostępu powinno nastąpić automatyczne zalogowanie do aplikacji Palo Alto Networks - Admin UI, dla której skonfigurowano logowanie jednokrotne. Aby uzyskać więcej informacji na temat panelu dostępu, zobacz [wprowadzenie do panelu dostępu](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Dodatkowe zasoby

- [Lista samouczków dotyczących sposobu integrowania aplikacji SaaS z usługą Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Co to jest dostęp do aplikacji i logowanie jednokrotne za pomocą Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

- [Co to jest dostęp warunkowy w usłudze Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Wypróbuj Palo Alto Networks — interfejs użytkownika administratora z usługą Azure AD](https://aad.portal.azure.com/)

- [Co to jest kontrola sesji w Microsoft Cloud App Security?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Jak chronić interfejs użytkownika Palo Alto Networks — administrator z zaawansowaną widocznością i kontrolkami](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)
