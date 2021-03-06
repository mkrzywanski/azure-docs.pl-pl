---
title: 'Samouczek: Inicjowanie obsługi użytkowników dla ThousandEyes — Azure AD'
description: Dowiedz się, jak skonfigurować Azure Active Directory w celu automatycznego aprowizacji i cofania aprowizacji kont użytkowników w usłudze ThousandEyes.
services: active-directory
documentationcenter: ''
author: ArvindHarinder1
manager: CelesteDG
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/28/2019
ms.author: arvinh
ms.collection: M365-identity-device-management
ms.openlocfilehash: d87cffce636146eac3e557670ffc4fb2fc34ae38
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "77062884"
---
# <a name="tutorial-configure-thousandeyes-for-automatic-user-provisioning"></a>Samouczek: Konfigurowanie ThousandEyes na potrzeby automatycznego aprowizacji użytkowników

Celem tego samouczka jest przedstawienie czynności, które należy wykonać w ThousandEyes i usłudze Azure AD w celu automatycznego aprowizacji i cofania aprowizacji kont użytkowników z usługi Azure AD do ThousandEyes. 

## <a name="prerequisites"></a>Wymagania wstępne

Scenariusz opisany w tym samouczku założono, że masz już następujące elementy:

* Dzierżawa usługi Azure Active Directory
* Dzierżawa ThousandEyes z [planem standardowym](https://www.thousandeyes.com/pricing) lub lepsza 
* Konto użytkownika w ThousandEyes z uprawnieniami administratora 

> [!NOTE]
> Integracja z obsługą administracyjną usługi Azure AD opiera się na [interfejsie API Standard scim ThousandEyes](https://success.thousandeyes.com/PublicArticlePage?articleIdParam=kA044000000CnWrCAK), który jest dostępny dla zespołów ThousandEyes w planie standardowym.

## <a name="assigning-users-to-thousandeyes"></a>Przypisywanie użytkowników do ThousandEyes

Azure Active Directory używa koncepcji o nazwie "przydziały", aby określić, którzy użytkownicy powinni otrzymywać dostęp do wybranych aplikacji. W kontekście automatycznego inicjowania obsługi kont użytkowników są synchronizowane tylko użytkownicy i grupy, które zostały przypisane do aplikacji w usłudze Azure AD. 

Przed skonfigurowaniem i włączeniem usługi aprowizacji należy zdecydować, którzy użytkownicy i/lub grupy w usłudze Azure AD reprezentują użytkowników, którzy potrzebują dostępu do aplikacji ThousandEyes. Po ustaleniu tych użytkowników możesz przypisać je do aplikacji ThousandEyes, postępując zgodnie z poniższymi instrukcjami:

[Przypisywanie użytkownika lub grupy do aplikacji dla przedsiębiorstw](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-thousandeyes"></a>Ważne wskazówki dotyczące przypisywania użytkowników do ThousandEyes

* Zaleca się, aby jeden użytkownik usługi Azure AD został przypisany do ThousandEyes w celu przetestowania konfiguracji aprowizacji. Dodatkowych użytkowników i/lub grupy można przypisywać później.

* Podczas przypisywania użytkownika do ThousandEyes należy wybrać rolę **użytkownika** lub inną prawidłową rolę specyficzną dla aplikacji (jeśli jest dostępna) w oknie dialogowym przypisania. **Domyślna rola dostępu** nie działa w przypadku inicjowania obsługi administracyjnej, a użytkownicy są pomijani.

## <a name="configuring-user-provisioning-to-thousandeyes"></a>Konfigurowanie aprowizacji użytkowników do ThousandEyes 

Ta sekcja przeprowadzi Cię przez proces łączenia się z interfejsem API aprowizacji usługi Azure AD do konta użytkownika ThousandEyes, a następnie Konfigurując usługę aprowizacji do tworzenia, aktualizowania i wyłączania przypisanych kont użytkowników w usłudze ThousandEyes na podstawie przypisywania użytkowników i grup w usłudze Azure AD.

> [!TIP]
> Możesz również włączyć funkcję logowania jednokrotnego opartego na protokole SAML dla ThousandEyes, postępując zgodnie z instrukcjami podanymi w [Azure Portal](https://portal.azure.com). Logowanie jednokrotne można skonfigurować niezależnie od automatycznej aprowizacji, chociaż te dwie funkcje napadają nawzajem.

### <a name="configure-automatic-user-account-provisioning-to-thousandeyes-in-azure-ad"></a>Konfigurowanie automatycznego inicjowania obsługi konta użytkownika w usłudze Azure AD ThousandEyes

1. W [Azure Portal](https://portal.azure.com)przejdź do sekcji **Azure Active Directory > aplikacje dla przedsiębiorstw > wszystkie aplikacje** .

2. Jeśli już skonfigurowano ThousandEyes do logowania jednokrotnego, Wyszukaj wystąpienie elementu ThousandEyes przy użyciu pola wyszukiwania. W przeciwnym razie wybierz pozycję **Dodaj** i Wyszukaj **ThousandEyes** w galerii aplikacji. Wybierz pozycję ThousandEyes z wyników wyszukiwania, a następnie dodaj ją do listy aplikacji.

3. Wybierz wystąpienie elementu ThousandEyes, a następnie wybierz kartę **Inicjowanie obsługi** .

4. Ustaw **tryb aprowizacji** na **automatyczny**.

    ![Inicjowanie obsługi ThousandEyes](./media/thousandeyes-provisioning-tutorial/ThousandEyes1.png)

5. W sekcji **poświadczenia administratora** wprowadź **token okaziciela OAuth** wygenerowany przez konto ThousandEyes (możesz znaleźć i wygenerować token w sekcji **profilu** konta ThousandEyes).

    ![Inicjowanie obsługi ThousandEyes](./media/thousandeyes-provisioning-tutorial/ThousandEyes2.png)

6. W Azure Portal kliknij pozycję **Testuj połączenie** , aby upewnić się, że usługa Azure AD może nawiązać połączenie z aplikacją ThousandEyes. Jeśli połączenie nie powiedzie się, upewnij się, że konto usługi ThousandEyes ma uprawnienia administratora, a następnie spróbuj ponownie wykonać krok 5.

7. Wprowadź adres e-mail osoby lub grupy, które powinny otrzymywać powiadomienia o błędach dotyczących aprowizacji w polu **E-mail powiadomienia** , i zaznacz pole wyboru "Wyślij powiadomienie e-mail, gdy wystąpi błąd".

8. Kliknij pozycję **Zapisz**.

9. W sekcji mapowania wybierz pozycję **synchronizuj Azure Active Directory użytkowników do ThousandEyes**.

10. W sekcji **mapowania atrybutów** Przejrzyj atrybuty użytkownika, które są synchronizowane z usługi Azure AD do ThousandEyes. Atrybuty wybrane jako **pasujące** właściwości są używane do dopasowania kont użytkowników w programie ThousandEyes for Updates. Wybierz przycisk Zapisz, aby zatwierdzić zmiany.

11. Aby włączyć usługę Azure AD Provisioning dla usługi ThousandEyes, Zmień **stan aprowizacji** na **włączone** w sekcji **Ustawienia** .

12. Kliknij pozycję **Zapisz**.

Ta operacja uruchamia początkową synchronizację wszystkich użytkowników i/lub grup przypisanych do ThousandEyes w sekcji Użytkownicy i grupy. Synchronizacja początkowa trwa dłużej niż kolejne synchronizacje, które wystąpiły co około 40 minut, o ile usługa jest uruchomiona. Za pomocą sekcji **szczegóły synchronizacji** można monitorować postęp i wykonywać linki do dzienników aktywności aprowizacji, które opisują wszystkie akcje wykonywane przez usługę aprowizacji.

Aby uzyskać więcej informacji na temat sposobu odczytywania dzienników aprowizacji usługi Azure AD, zobacz [Raportowanie dotyczące automatycznego inicjowania obsługi konta użytkownika](../app-provisioning/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Zasoby dodatkowe

* [Zarządzanie obsługą kont użytkowników w aplikacjach dla przedsiębiorstw](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Co to jest dostęp do aplikacji i logowanie jednokrotne za pomocą Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Następne kroki

* [Dowiedz się, jak przeglądać dzienniki i uzyskiwać raporty dotyczące aktywności aprowizacji](../app-provisioning/check-status-user-account-provisioning.md)
