---
title: 'Samouczek: Konfigurowanie uwierzytelniania tożsamości platformy SAP w chmurze na potrzeby automatycznego inicjowania obsługi użytkowników przy użyciu Azure Active Directory | Microsoft Docs'
description: Dowiedz się, jak skonfigurować Azure Active Directory, aby automatycznie udostępniać i cofać obsługę administracyjną kont użytkowników na potrzeby uwierzytelniania tożsamości platformy w chmurze SAP.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: f49b5738-c769-403b-8f29-84ddeea7fbf1
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/19/2019
ms.author: Zhchia
ms.openlocfilehash: 534d75a5b9009f0febee2746179ab7357a4985e0
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "84732875"
---
# <a name="tutorial-configure-sap-cloud-platform-identity-authentication-for-automatic-user-provisioning"></a>Samouczek: Konfigurowanie uwierzytelniania tożsamości platformy SAP w chmurze na potrzeby automatycznej aprowizacji użytkowników

Celem tego samouczka jest przedstawienie czynności, które należy wykonać w ramach uwierzytelniania tożsamości platformy SAP w chmurze i Azure Active Directory (Azure AD) w celu skonfigurowania usługi Azure AD w celu automatycznego aprowizacji i cofania aprowizacji użytkowników i/lub grup do uwierzytelniania tożsamości platformy w chmurze SAP.

> [!NOTE]
> Ten samouczek zawiera opis łącznika utworzonego na podstawie usługi Azure AD User Provisioning. Aby uzyskać ważne informacje o tym, jak działa ta usługa, jak ona dotyczy, i często zadawanych pytań, zobacz [Automatyzowanie aprowizacji użytkowników i Anulowanie udostępniania aplikacji SaaS przy użyciu programu Azure Active Directory](../app-provisioning/user-provisioning.md).
>
> Ten łącznik jest obecnie w publicznej wersji zapoznawczej. Aby uzyskać więcej informacji na temat ogólnych Microsoft Azure warunki użytkowania funkcji w wersji zapoznawczej, zobacz [dodatkowe warunki użytkowania dla Microsoft Azure podglądów](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Wymagania wstępne

Scenariusz opisany w tym samouczku założono, że masz już następujące wymagania wstępne:

* Dzierżawa usługi Azure AD
* [Dzierżawa uwierzytelniania tożsamości platformy SAP Cloud Platform](https://cloudplatform.sap.com/pricing.html)
* Konto użytkownika w usłudze SAP Cloud Platform uwierzytelniania tożsamości z uprawnieniami administratora.

## <a name="assigning-users-to-sap-cloud-platform-identity-authentication"></a>Przypisywanie użytkowników do uwierzytelniania tożsamości platformy SAP Cloud Platform

Azure Active Directory używa koncepcji zwanej *zadaniami* w celu określenia, którzy użytkownicy powinni otrzymywać dostęp do wybranych aplikacji. W kontekście automatycznej aprowizacji użytkowników są synchronizowane tylko użytkownicy i/lub grupy, które zostały przypisane do aplikacji w usłudze Azure AD.

Przed skonfigurowaniem i włączeniem automatycznej aprowizacji użytkowników należy zdecydować, którzy użytkownicy i/lub grupy w usłudze Azure AD potrzebują dostępu do uwierzytelniania tożsamości platformy SAP Cloud Platform. Po ustaleniu tych użytkowników i/lub grup do uwierzytelniania tożsamości platformy w chmurze SAP należy wykonać następujące instrukcje:
* [Przypisywanie użytkownika lub grupy do aplikacji dla przedsiębiorstw](../manage-apps/assign-user-or-group-access-portal.md)

## <a name="important-tips-for-assigning-users-to-sap-cloud-platform-identity-authentication"></a>Ważne porady dotyczące przypisywania użytkowników do uwierzytelniania tożsamości platformy SAP Cloud Platform

* Zaleca się, aby jeden użytkownik usługi Azure AD został przypisany do uwierzytelniania tożsamości platformy SAP w chmurze w celu przetestowania automatycznej konfiguracji inicjowania obsługi użytkowników. Dodatkowych użytkowników i/lub grupy można przypisywać później.

* Podczas przypisywania użytkownika do uwierzytelniania tożsamości platformy w chmurze SAP należy wybrać dowolną prawidłową rolę specyficzną dla aplikacji (jeśli jest dostępna) w oknie dialogowym przypisania. Użytkownicy z **domyślną rolą dostępu** są wykluczeni z aprowizacji.

## <a name="setup-sap-cloud-platform-identity-authentication-for-provisioning"></a>Skonfiguruj uwierzytelnianie tożsamości platformy SAP Cloud Platform na potrzeby aprowizacji

1. Zaloguj się do [konsoli administracyjnej uwierzytelniania platformy SAP Cloud Identity Authentication](https://sapmsftintegration.accounts.ondemand.com/admin). Przejdź do **użytkowników & autoryzacje > Administratorzy**.

    ![Konsola administracyjna uwierzytelniania tożsamości platformy SAP Cloud Platform](media/sap-cloud-platform-identity-authentication-provisioning-tutorial/adminconsole.png)

2.  Naciśnij przycisk **+ Dodaj** w panelu po lewej stronie, aby dodać nowego administratora do listy. Wybierz pozycję **Dodaj system** i wprowadź nazwę systemu.   

> [!NOTE]
> Administratora użytkownika w oprogramowaniu SAP Cloud Platform Authentication musi być typu **system**. Tworzenie normalnego użytkownika administratora może prowadzić do *nieautoryzowanych* błędów podczas aprowizacji.   

3.  W obszarze Konfigurowanie autoryzacji przejdź na przycisk przełączania, aby **zarządzać użytkownikami** i **zarządzać grupami**.

    ![Uwierzytelnianie tożsamości platformy SAP Cloud Platform Dodawanie Standard scim](media/sap-cloud-platform-identity-authentication-provisioning-tutorial/configurationauth.png)

4. Otrzymasz wiadomość e-mail umożliwiającą aktywowanie konta i Ustawianie hasła dla **usługi SAP Cloud Identity Authentication**.

4.  Skopiuj **Identyfikator użytkownika** i **hasło**. Te wartości zostaną wprowadzone w polach Nazwa użytkownika i hasło administratora odpowiednio na karcie aprowizacji aplikacji do uwierzytelniania tożsamości platformy SAP Cloud Platform w Azure Portal.

## <a name="add-sap-cloud-platform-identity-authentication-from-the-gallery"></a>Dodawanie uwierzytelniania tożsamości platformy SAP w chmurze z galerii

Przed skonfigurowaniem uwierzytelniania tożsamości platformy SAP w chmurze w celu automatycznego aprowizacji użytkowników w usłudze Azure AD należy dodać uwierzytelnianie tożsamości platformy SAP Cloud Platform z galerii aplikacji usługi Azure AD do listy zarządzanych aplikacji SaaS.

**Aby dodać uwierzytelnianie tożsamości platformy SAP w chmurze z galerii aplikacji usługi Azure AD, wykonaj następujące czynności:**

1. W **[Azure Portal](https://portal.azure.com)** w lewym panelu nawigacyjnym wybierz pozycję **Azure Active Directory**.

    ![Przycisk Azure Active Directory](common/select-azuread.png)

2. Przejdź do pozycji **aplikacje dla przedsiębiorstw**, a następnie wybierz pozycję **wszystkie aplikacje**.

    ![Blok Aplikacje dla przedsiębiorstw](common/enterprise-applications.png)

3. Aby dodać nową aplikację, wybierz przycisk **Nowa aplikacja** w górnej części okienka.

    ![Przycisk Nowa aplikacja](common/add-new-app.png)

4. W polu wyszukiwania wprowadź **uwierzytelnianie tożsamości platformy w chmurze SAP**, wybierz pozycję **SAP Cloud Platform Identity Authentication** w panelu wyniki, a następnie kliknij przycisk **Dodaj** , aby dodać aplikację.

    ![Aplikacja SAP Cloud Platform Identity Authentication na liście wyników](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-sap-cloud-platform-identity-authentication"></a>Konfigurowanie automatycznej aprowizacji użytkowników do uwierzytelniania tożsamości platformy SAP Cloud Platform 

Ta sekcja przeprowadzi Cię przez kroki konfigurowania usługi Azure AD Provisioning w celu tworzenia, aktualizowania i wyłączania użytkowników i/lub grup w usłudze SAP Cloud Platform Authentication na podstawie przypisań użytkowników i/lub grup w usłudze Azure AD.

> [!TIP]
> Możesz również włączyć funkcję logowania jednokrotnego opartego na protokole SAML na potrzeby uwierzytelniania tożsamości platformy SAP w chmurze, postępując zgodnie z instrukcjami podanymi w [samouczku Logowanie jednokrotne w usłudze SAP Cloud Platform Authentication](https://docs.microsoft.com/azure/active-directory/saas-apps/sap-hana-cloud-platform-identity-authentication-tutorial). Logowanie jednokrotne można skonfigurować niezależnie od automatycznej aprowizacji użytkowników, chociaż te dwie funkcje napadają nawzajem

### <a name="to-configure-automatic-user-provisioning-for-sap-cloud-platform-identity-authentication-in-azure-ad"></a>Aby skonfigurować automatyczną obsługę administracyjną uwierzytelniania tożsamości platformy SAP w chmurze w usłudze Azure AD:

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com). Wybierz pozycję **aplikacje dla przedsiębiorstw**, a następnie wybierz pozycję **wszystkie aplikacje**.

    ![Blok Aplikacje dla przedsiębiorstw](common/enterprise-applications.png)

2. Na liście aplikacji wybierz pozycję **SAP Cloud Platform Identity Authentication**.

    ![Link do usługi SAP Cloud Platform Identity Authentication na liście Aplikacje](common/all-applications.png)

3. Wybierz kartę **aprowizacji** .

    ![Karta aprowizacji](common/provisioning.png)

4. Ustaw **tryb aprowizacji** na **automatyczny**.

    ![Karta aprowizacji](common/provisioning-automatic.png)

5. W sekcji **poświadczenia administratora** wprowadź `https://<tenantID>.accounts.ondemand.com/service/scim ` **adres URL dzierżawy**. Wprowadź odpowiednio wartości **identyfikatora użytkownika** i **hasła** pobrane wcześniej w polu **Nazwa użytkownika administratora** i **hasło administratora** . Kliknij pozycję **Testuj połączenie** , aby upewnić się, że usługa Azure AD może połączyć się z uwierzytelnianiem tożsamości platformy SAP Cloud Platform. Jeśli połączenie nie powiedzie się, upewnij się, że konto uwierzytelniania tożsamości platformy SAP Cloud Platform ma uprawnienia administratora, a następnie spróbuj ponownie.

    ![Adres URL dzierżawy + token](media/sap-cloud-platform-identity-authentication-provisioning-tutorial/testconnection.png)

6. W polu **adres E-mail powiadomienia** wprowadź adres e-mail osoby lub grupy, które powinny otrzymywać powiadomienia o błędach aprowizacji, i zaznacz pole wyboru — **Wyślij powiadomienie e-mail, gdy wystąpi awaria**.

    ![Wiadomość E-mail z powiadomieniem](common/provisioning-notification-email.png)

7. Kliknij pozycję **Zapisz**.

8. W sekcji **mapowania** wybierz pozycję **Synchronizuj Azure Active Directory użytkownicy do uwierzytelniania tożsamości platformy w chmurze SAP**.

    ![Mapowania użytkownika uwierzytelniania tożsamości platformy SAP Cloud Platform](media/sap-cloud-platform-identity-authentication-provisioning-tutorial/mapping.png)

9. Przejrzyj atrybuty użytkownika, które są synchronizowane z usługi Azure AD do uwierzytelniania tożsamości platformy w chmurze SAP w sekcji **Mapowanie atrybutów** . Atrybuty wybrane jako **pasujące** właściwości są używane do dopasowywania kont użytkowników w ramach uwierzytelniania tożsamości platformy SAP w chmurze dla operacji aktualizacji. Wybierz przycisk **Zapisz** , aby zatwierdzić zmiany.

    ![Atrybuty użytkownika uwierzytelniania tożsamości platformy SAP Cloud Platform](media/sap-cloud-platform-identity-authentication-provisioning-tutorial/userattributes.png)

10. Aby skonfigurować filtry określania zakresu, zapoznaj się z poniższymi instrukcjami w [samouczku dotyczącym filtru określania zakresu](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).

11. Aby włączyć usługę aprowizacji usługi Azure AD na potrzeby uwierzytelniania tożsamości platformy SAP w chmurze, Zmień **stan aprowizacji** na **włączone** w sekcji **Ustawienia** .

    ![Stan aprowizacji jest przełączany](common/provisioning-toggle-on.png)

12. Zdefiniuj użytkowników i/lub grupy, które chcesz udostępnić do uwierzytelniania tożsamości platformy w chmurze SAP, wybierając odpowiednie wartości w **zakresie** w sekcji **Ustawienia** .

    ![Zakres aprowizacji](common/provisioning-scope.png)

13. Gdy wszystko będzie gotowe do udostępnienia, kliknij przycisk **Zapisz**.

    ![Zapisywanie konfiguracji aprowizacji](common/provisioning-configuration-save.png)

Ta operacja uruchamia początkową synchronizację wszystkich użytkowników i/lub grup zdefiniowanych w **zakresie** w sekcji **Ustawienia** . Synchronizacja początkowa trwa dłużej niż kolejne synchronizacje, które wystąpiły co około 40 minut, o ile usługa Azure AD Provisioning jest uruchomiona. Możesz użyć sekcji **szczegóły synchronizacji** do monitorowania postępu i postępuj zgodnie z raportem aktywności aprowizacji, który opisuje wszystkie akcje wykonywane przez usługę Azure AD Provisioning na potrzeby uwierzytelniania tożsamości platformy SAP Cloud Platform.

Aby uzyskać więcej informacji na temat sposobu odczytywania dzienników aprowizacji usługi Azure AD, zobacz [Raportowanie dotyczące automatycznego inicjowania obsługi konta użytkownika](../app-provisioning/check-status-user-account-provisioning.md).

## <a name="connector-limitations"></a>Ograniczenia łącznika

* Punkt końcowy Standard scim uwierzytelniania tożsamości platformy w chmurze SAP wymaga, aby pewne atrybuty miały określony format. Więcej informacji o tych atrybutach i ich konkretnym formacie można znaleźć [tutaj](https://help.sap.com/viewer/6d6d63354d1242d185ab4830fc04feb1/Cloud/en-US/b10fc6a9a37c488a82ce7489b1fab64c.html#).

## <a name="additional-resources"></a>Zasoby dodatkowe

* [Zarządzanie obsługą kont użytkowników w aplikacjach dla przedsiębiorstw](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Co to jest dostęp do aplikacji i logowanie jednokrotne za pomocą Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Następne kroki

* [Dowiedz się, jak przeglądać dzienniki i uzyskiwać raporty dotyczące aktywności aprowizacji](../app-provisioning/check-status-user-account-provisioning.md)

