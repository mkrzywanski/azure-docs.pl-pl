---
title: 'Samouczek: Konfigurowanie Velpic w celu automatycznego aprowizacji użytkowników przy użyciu Azure Active Directory | Microsoft Docs'
description: Dowiedz się, jak skonfigurować Azure Active Directory w celu automatycznego aprowizacji i cofania aprowizacji kont użytkowników w usłudze Velpic.
services: active-directory
documentationcenter: ''
author: zhchia
writer: zhchia
manager: beatrizd-msft
ms.assetid: d4ca2365-6729-48f7-bb7f-c0f5ffe740a3
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2019
ms.author: zhchia
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9b7a6c2c9b7ecb0b160f7481d95f7682f3f7a109
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "77064125"
---
# <a name="tutorial-configuring-velpic-for-automatic-user-provisioning"></a>Samouczek: Konfigurowanie Velpic na potrzeby automatycznego aprowizacji użytkowników

Celem tego samouczka jest przedstawienie czynności, które należy wykonać w Velpic i usłudze Azure AD w celu automatycznego aprowizacji i cofania aprowizacji kont użytkowników z usługi Azure AD do Velpic.

> [!NOTE]
> Ten samouczek zawiera opis łącznika utworzonego na podstawie usługi Azure AD User Provisioning. Aby uzyskać ważne informacje o tym, jak działa ta usługa, jak ona dotyczy, i często zadawanych pytań, zobacz [Automatyzowanie aprowizacji użytkowników i Anulowanie udostępniania aplikacji SaaS przy użyciu programu Azure Active Directory](../app-provisioning/user-provisioning.md).

## <a name="prerequisites"></a>Wymagania wstępne

Scenariusz opisany w tym samouczku założono, że masz już następujące elementy:

* Dzierżawa usługi Azure Active Directory
* Dzierżawa Velpic z [planem Enterprise](https://www.velpic.com/pricing.html) lub lepsza
* Konto użytkownika w Velpic z uprawnieniami administratora

## <a name="assigning-users-to-velpic"></a>Przypisywanie użytkowników do Velpic

Azure Active Directory używa koncepcji o nazwie "przydziały", aby określić, którzy użytkownicy powinni otrzymywać dostęp do wybranych aplikacji. W kontekście automatycznego inicjowania obsługi konta użytkownika zostaną zsynchronizowane tylko użytkownicy i grupy, które zostały przypisane do aplikacji w usłudze Azure AD. 

Przed skonfigurowaniem i włączeniem usługi aprowizacji należy zdecydować, którzy użytkownicy i/lub grupy w usłudze Azure AD reprezentują użytkowników, którzy potrzebują dostępu do aplikacji Velpic. Po ustaleniu tych użytkowników możesz przypisać je do aplikacji Velpic, postępując zgodnie z poniższymi instrukcjami:

[Przypisywanie użytkownika lub grupy do aplikacji dla przedsiębiorstw](../manage-apps/assign-user-or-group-access-portal.md)

### <a name="important-tips-for-assigning-users-to-velpic"></a>Ważne wskazówki dotyczące przypisywania użytkowników do Velpic

* Zaleca się, aby jeden użytkownik usługi Azure AD został przypisany do Velpic w celu przetestowania konfiguracji aprowizacji. Dodatkowych użytkowników i/lub grupy można przypisywać później.

* Podczas przypisywania użytkownika do Velpic należy wybrać rolę **użytkownika** lub inną prawidłową rolę specyficzną dla aplikacji (jeśli jest dostępna) w oknie dialogowym przypisania. Należy pamiętać, że **Domyślna rola dostępu** nie działa w przypadku aprowizacji, a użytkownicy zostaną pominięci.

## <a name="configuring-user-provisioning-to-velpic"></a>Konfigurowanie aprowizacji użytkowników do Velpic

Ta sekcja przeprowadzi Cię przez proces łączenia się z interfejsem API aprowizacji usługi Azure AD do konta użytkownika Velpic, a następnie Konfigurując usługę aprowizacji do tworzenia, aktualizowania i wyłączania przypisanych kont użytkowników w usłudze Velpic na podstawie przypisywania użytkowników i grup w usłudze Azure AD.

> [!TIP]
> Możesz również włączyć funkcję logowania jednokrotnego opartego na protokole SAML dla Velpic, postępując zgodnie z instrukcjami podanymi w [Azure Portal](https://portal.azure.com). Logowanie jednokrotne można skonfigurować niezależnie od automatycznej aprowizacji, chociaż te dwie funkcje napadają nawzajem.

### <a name="to-configure-automatic-user-account-provisioning-to-velpic-in-azure-ad"></a>Aby skonfigurować automatyczne Inicjowanie obsługi konta użytkownika w usłudze Azure AD Velpic:

1. W [Azure Portal](https://portal.azure.com)przejdź do sekcji **Azure Active Directory > aplikacje dla przedsiębiorstw > wszystkie aplikacje** .

2. Jeśli już skonfigurowano Velpic do logowania jednokrotnego, Wyszukaj wystąpienie elementu Velpic przy użyciu pola wyszukiwania. W przeciwnym razie wybierz pozycję **Dodaj** i Wyszukaj **Velpic** w galerii aplikacji. Wybierz pozycję Velpic z wyników wyszukiwania, a następnie dodaj ją do listy aplikacji.

3. Wybierz wystąpienie elementu Velpic, a następnie wybierz kartę **Inicjowanie obsługi** .

4. Ustaw **tryb aprowizacji** na **automatyczny**.

    ![Inicjowanie obsługi Velpic](./media/velpic-provisioning-tutorial/Velpic1.png)

5. W sekcji **poświadczenia administratora** wprowadź **adres URL dzierżawy&tajny token** Velpic. (Te wartości można znaleźć na koncie Velpic: **Zarządzaj**  >  **Integracja**  >  **Wtyczka**  >  **Standard scim**)

    ![Wartości autoryzacji](./media/velpic-provisioning-tutorial/Velpic2.png)

6. W Azure Portal kliknij pozycję **Testuj połączenie** , aby upewnić się, że usługa Azure AD może nawiązać połączenie z aplikacją Velpic. Jeśli połączenie nie powiedzie się, upewnij się, że konto usługi Velpic ma uprawnienia administratora, a następnie spróbuj ponownie wykonać krok 5.

7. Wprowadź adres e-mail osoby lub grupy, które powinny otrzymywać powiadomienia o błędach aprowizacji w polu **E-mail powiadomienia** , a następnie zaznacz pole wyboru poniżej.

8. Kliknij pozycję **Zapisz**.

9. W sekcji mapowania wybierz pozycję **synchronizuj Azure Active Directory użytkowników do Velpic**.

10. W sekcji **mapowania atrybutów** Przejrzyj atrybuty użytkownika, które zostaną zsynchronizowane z usługi Azure AD do Velpic. Należy zauważyć, że atrybuty wybrane jako **pasujące** właściwości będą używane w celu dopasowania do kont użytkowników w Velpic dla operacji aktualizacji. Wybierz przycisk Zapisz, aby zatwierdzić zmiany.

11. Aby włączyć usługę Azure AD Provisioning dla usługi Velpic, Zmień **stan aprowizacji** na **włączone** w sekcji **Ustawienia** .

12. Kliknij pozycję **Zapisz**.

Spowoduje to rozpoczęcie synchronizacji początkowej dla wszystkich użytkowników i/lub grup przypisanych do Velpic w sekcji Użytkownicy i grupy. Należy pamiętać, że synchronizacja początkowa zajmie więcej czasu niż kolejne synchronizacje, co będzie odbywać się około co 40 minut, o ile usługa jest uruchomiona. Sekcja **szczegóły synchronizacji** służy do monitorowania postępu i wykonywania linków do raportów dotyczących działań związanych z obsługą administracyjną, które opisują wszystkie akcje wykonywane przez usługę aprowizacji.

Aby uzyskać więcej informacji na temat sposobu odczytywania dzienników aprowizacji usługi Azure AD, zobacz [Raportowanie dotyczące automatycznego inicjowania obsługi konta użytkownika](../app-provisioning/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Zasoby dodatkowe

* [Zarządzanie obsługą kont użytkowników w aplikacjach dla przedsiębiorstw](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [Co to jest dostęp do aplikacji i logowanie jednokrotne za pomocą Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Następne kroki

* [Dowiedz się, jak przeglądać dzienniki i uzyskiwać raporty dotyczące aktywności aprowizacji](../app-provisioning/check-status-user-account-provisioning.md)