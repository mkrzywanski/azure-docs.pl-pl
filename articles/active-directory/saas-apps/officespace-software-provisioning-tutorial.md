---
title: 'Samouczek: Konfigurowanie oprogramowania OfficeSpace na potrzeby automatycznej aprowizacji użytkowników przy użyciu Azure Active Directory | Microsoft Docs'
description: Dowiedz się, jak skonfigurować Azure Active Directory w celu automatycznego aprowizacji i cofania aprowizacji kont użytkowników w celu OfficeSpace oprogramowania.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: f832a0a6-ad0a-453f-a747-9cd717e11181
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/02/2019
ms.author: Zhchia
ms.openlocfilehash: 3d472b300400cf230773ba01f3f4362988c34e81
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "77063428"
---
# <a name="tutorial-configure-officespace-software-for-automatic-user-provisioning"></a>Samouczek: Konfigurowanie oprogramowania OfficeSpace na potrzeby automatycznego aprowizacji użytkowników

Celem tego samouczka jest przedstawienie czynności, które należy wykonać w OfficeSpace oprogramowania i Azure Active Directory (Azure AD) w celu skonfigurowania usługi Azure AD w celu automatycznego aprowizacji i cofania aprowizacji użytkowników i/lub grup do OfficeSpace oprogramowania.

> [!NOTE]
> Ten samouczek zawiera opis łącznika utworzonego na podstawie usługi Azure AD User Provisioning. Aby uzyskać ważne informacje o tym, jak działa ta usługa, jak ona dotyczy, i często zadawanych pytań, zobacz [Automatyzowanie aprowizacji użytkowników i Anulowanie udostępniania aplikacji SaaS przy użyciu programu Azure Active Directory](../app-provisioning/user-provisioning.md).
>
> Ten łącznik jest obecnie w publicznej wersji zapoznawczej. Aby uzyskać więcej informacji na temat ogólnych Microsoft Azure warunki użytkowania funkcji w wersji zapoznawczej, zobacz [dodatkowe warunki użytkowania dla Microsoft Azure podglądów](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Wymagania wstępne

Scenariusz opisany w tym samouczku założono, że masz już następujące wymagania wstępne:

* Dzierżawa usługi Azure AD
* [Dzierżawa oprogramowania OfficeSpace](https://www.officespacesoftware.com/)
* Konto użytkownika w OfficeSpace oprogramowania z uprawnieniami administratora.

## <a name="assigning-users-to-officespace-software"></a>Przypisywanie użytkowników do OfficeSpace oprogramowania

Azure Active Directory używa koncepcji zwanej *zadaniami* w celu określenia, którzy użytkownicy powinni otrzymywać dostęp do wybranych aplikacji. W kontekście automatycznej aprowizacji użytkowników są synchronizowane tylko użytkownicy i/lub grupy, które zostały przypisane do aplikacji w usłudze Azure AD.

Przed skonfigurowaniem i włączeniem automatycznej aprowizacji użytkowników należy zdecydować, którzy użytkownicy i/lub grupy w usłudze Azure AD potrzebują dostępu do oprogramowania OfficeSpace. Po podaniu tych użytkowników i/lub grup można przypisywać OfficeSpace oprogramowanie, postępując zgodnie z poniższymi instrukcjami:
* [Przypisywanie użytkownika lub grupy do aplikacji dla przedsiębiorstw](../manage-apps/assign-user-or-group-access-portal.md)

## <a name="important-tips-for-assigning-users-to-officespace-software"></a>Ważne porady dotyczące przypisywania użytkowników do oprogramowania OfficeSpace

* Zaleca się, aby jeden użytkownik usługi Azure AD został przypisany do OfficeSpace oprogramowania w celu przetestowania automatycznej konfiguracji inicjowania obsługi użytkowników. Dodatkowych użytkowników i/lub grupy można przypisywać później.

* Podczas przypisywania użytkownika do OfficeSpace oprogramowania należy wybrać dowolną prawidłową rolę specyficzną dla aplikacji (jeśli jest dostępna) w oknie dialogowym przypisania. Użytkownicy z **domyślną rolą dostępu** są wykluczeni z aprowizacji.

## <a name="set-up-officespace-software-for-provisioning"></a>Konfigurowanie oprogramowania OfficeSpace na potrzeby aprowizacji

1. Zaloguj się do [konsoli administratora oprogramowania OfficeSpace](https://support.officespacesoftware.com/hc). Przejdź do **ustawień > łączników**.

    ![Konsola administracyjna oprogramowania OfficeSpace](media/officespace-software-provisioning-tutorial/settings.png)

2.  Przejdź do **katalogu synchronizacja > Standard scim**.

    ![OfficeSpace oprogramowania Dodaj Standard scim](media/officespace-software-provisioning-tutorial/scim.png)

3.  Skopiuj **token uwierzytelniania Standard scim**. Ta wartość zostanie wprowadzona w polu token tajny na karcie inicjowanie aplikacji OfficeSpace oprogramowania w Azure Portal.

    ![Token tworzenia oprogramowania OfficeSpace](media/officespace-software-provisioning-tutorial/token.png)

## <a name="add-officespace-software-from-the-gallery"></a>Dodawanie oprogramowania OfficeSpace z galerii

Przed skonfigurowaniem oprogramowania OfficeSpace na potrzeby automatycznego aprowizacji użytkowników w usłudze Azure AD należy dodać oprogramowanie OfficeSpace z galerii aplikacji usługi Azure AD do listy zarządzanych aplikacji SaaS.

**Aby dodać oprogramowanie OfficeSpace z galerii aplikacji usługi Azure AD, wykonaj następujące czynności:**

1. W **[Azure Portal](https://portal.azure.com)** w lewym panelu nawigacyjnym wybierz pozycję **Azure Active Directory**.

    ![Przycisk Azure Active Directory](common/select-azuread.png)

2. Przejdź do pozycji **aplikacje dla przedsiębiorstw**, a następnie wybierz pozycję **wszystkie aplikacje**.

    ![Blok Aplikacje dla przedsiębiorstw](common/enterprise-applications.png)

3. Aby dodać nową aplikację, wybierz przycisk **Nowa aplikacja** w górnej części okienka.

    ![Przycisk Nowa aplikacja](common/add-new-app.png)

4. W polu wyszukiwania wprowadź **OfficeSpace oprogramowanie**, wybierz pozycję **OfficeSpace Software** w panelu wyniki, a następnie kliknij przycisk **Dodaj** , aby dodać aplikację.

    ![OfficeSpace oprogramowanie na liście wyników](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-officespace-software"></a>Konfigurowanie automatycznego aprowizacji użytkowników w oprogramowaniu OfficeSpace 

Ta sekcja przeprowadzi Cię przez kroki konfigurowania usługi Azure AD Provisioning w celu tworzenia, aktualizowania i wyłączania użytkowników i/lub grup w oprogramowaniu OfficeSpace na podstawie przypisań użytkowników i/lub grup w usłudze Azure AD.

> [!TIP]
> Możesz również włączyć funkcję logowania jednokrotnego opartego na protokole SAML dla oprogramowania OfficeSpace, postępując zgodnie z instrukcjami podanymi w [samouczku logowanie](https://docs.microsoft.com/azure/active-directory/saas-apps/officespace-tutorial)jednokrotne w programie OfficeSpace. Logowanie jednokrotne można skonfigurować niezależnie od automatycznej aprowizacji użytkowników, chociaż te dwie funkcje uzupełniają się wzajemnie.

### <a name="to-configure-automatic-user-provisioning-for-officespace-software-in-azure-ad"></a>Aby skonfigurować automatyczne Inicjowanie obsługi administracyjnej użytkowników dla oprogramowania OfficeSpace w usłudze Azure AD:

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com). Wybierz pozycję **aplikacje dla przedsiębiorstw**, a następnie wybierz pozycję **wszystkie aplikacje**.

    ![Blok Aplikacje dla przedsiębiorstw](common/enterprise-applications.png)

2. Na liście Aplikacje wybierz pozycję **OfficeSpace oprogramowanie**.

    ![Link OfficeSpace Software na liście aplikacji](common/all-applications.png)

3. Wybierz kartę **aprowizacji** .

    ![Karta aprowizacji](common/provisioning.png)

4. Ustaw **tryb aprowizacji** na **automatyczny**.

    ![Karta aprowizacji](common/provisioning-automatic.png)

5. W sekcji **poświadczenia administratora** wpisz wejściowy `https://<subdomain>.officespacesoftware.com/api/scim/v2/` adres URL w **adresie URL dzierżawy**. Na przykład: `https://contoso.officespacesoftware.com/api/scim/v2/`. Wprowadź wartość **tokenu uwierzytelniania Standard scim** pobraną wcześniej w **tokenie tajnym**. Kliknij pozycję **Testuj połączenie** , aby upewnić się, że usługa Azure AD może nawiązać połączenie z oprogramowaniem OfficeSpace. Jeśli połączenie nie powiedzie się, upewnij się, że konto oprogramowania OfficeSpace ma uprawnienia administratora, a następnie spróbuj ponownie.

    ![Adres URL dzierżawy + token](common/provisioning-testconnection-tenanturltoken.png)

6. W polu **adres E-mail powiadomienia** wprowadź adres e-mail osoby lub grupy, które powinny otrzymywać powiadomienia o błędach aprowizacji, a następnie zaznacz pole wyboru **Wyślij powiadomienie e-mail po wystąpieniu błędu** .

    ![Wiadomość E-mail z powiadomieniem](common/provisioning-notification-email.png)

7. Kliknij pozycję **Zapisz**.

8. W sekcji **mapowania** wybierz pozycję **Synchronizuj Azure Active Directory użytkowników, aby OfficeSpace oprogramowanie**.

    ![Mapowania użytkowników oprogramowania OfficeSpace](media/officespace-software-provisioning-tutorial/usermappings.png)

9. Przejrzyj atrybuty użytkownika, które są synchronizowane z usługi Azure AD do OfficeSpace oprogramowania w sekcji **Mapowanie atrybutów** . Atrybuty wybrane jako **pasujące** właściwości są używane do dopasowywania kont użytkowników w oprogramowaniu OfficeSpace na potrzeby operacji aktualizacji. Wybierz przycisk **Zapisz** , aby zatwierdzić zmiany.

    ![Atrybuty użytkownika oprogramowania OfficeSpace](media/officespace-software-provisioning-tutorial/userattributes.png)

11. Aby skonfigurować filtry określania zakresu, zapoznaj się z poniższymi instrukcjami w [samouczku dotyczącym filtru określania zakresu](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).

12. Aby włączyć usługę Azure AD Provisioning dla oprogramowania OfficeSpace, Zmień **stan aprowizacji** na **włączone** w sekcji **Ustawienia** .

    ![Stan aprowizacji jest przełączany](common/provisioning-toggle-on.png)

13. Zdefiniuj użytkowników i/lub grupy, które chcesz udostępnić OfficeSpace oprogramowanie, wybierając odpowiednie wartości w **zakresie** w sekcji **Ustawienia** .

    ![Zakres aprowizacji](common/provisioning-scope.png)

14. Gdy wszystko będzie gotowe do udostępnienia, kliknij przycisk **Zapisz**.

    ![Zapisywanie konfiguracji aprowizacji](common/provisioning-configuration-save.png)

Ta operacja uruchamia początkową synchronizację wszystkich użytkowników i/lub grup zdefiniowanych w **zakresie** w sekcji **Ustawienia** . Synchronizacja początkowa trwa dłużej niż kolejne synchronizacje, które wystąpiły co około 40 minut, o ile usługa Azure AD Provisioning jest uruchomiona. Możesz użyć sekcji **szczegóły synchronizacji** do monitorowania postępu i postępuj zgodnie z raportem aktywności aprowizacji, który opisuje wszystkie akcje wykonywane przez usługę Azure AD Provisioning na OfficeSpace oprogramowania.

Aby uzyskać więcej informacji na temat sposobu odczytywania dzienników aprowizacji usługi Azure AD, zobacz [Raportowanie dotyczące automatycznego inicjowania obsługi konta użytkownika](../app-provisioning/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Zasoby dodatkowe

* [Zarządzanie obsługą kont użytkowników w aplikacjach dla przedsiębiorstw](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Co to jest dostęp do aplikacji i logowanie jednokrotne za pomocą Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Następne kroki

* [Dowiedz się, jak przeglądać dzienniki i uzyskiwać raporty dotyczące aktywności aprowizacji](../app-provisioning/check-status-user-account-provisioning.md)

