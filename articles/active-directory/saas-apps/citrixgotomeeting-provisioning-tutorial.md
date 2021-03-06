---
title: 'Samouczek: Konfigurowanie spotkanie GoToMeeting dla automatycznej aprowizacji użytkowników przy użyciu Azure Active Directory | Microsoft Docs'
description: Dowiedz się, jak skonfigurować logowanie jednokrotne między usługą Azure Active Directory a usługą GoToMeeting.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 0f59fedb-2cf8-48d2-a5fb-222ed943ff78
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: f0ac06fc3018b4230cbf32712067c48400599082
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "77058267"
---
# <a name="tutorial-configure-gotomeeting-for-automatic-user-provisioning"></a>Samouczek: Konfigurowanie spotkanie GoToMeeting na potrzeby automatycznego aprowizacji użytkowników

Celem tego samouczka jest przedstawienie czynności, które należy wykonać w spotkanie GoToMeeting i usłudze Azure AD w celu automatycznego aprowizacji i cofania aprowizacji kont użytkowników z usługi Azure AD do spotkanie GoToMeeting.

## <a name="prerequisites"></a>Wymagania wstępne

Scenariusz opisany w tym samouczku założono, że masz już następujące elementy:

*   Dzierżawa usługi Azure Active Directory.
*   Subskrypcja z włączonym logowaniem jednokrotnym w usłudze spotkanie GoToMeeting.
*   Konto użytkownika w spotkanie GoToMeeting z uprawnieniami administratora zespołu.

## <a name="assigning-users-to-gotomeeting"></a>Przypisywanie użytkowników do spotkanie GoToMeeting

Azure Active Directory używa koncepcji o nazwie "przydziały", aby określić, którzy użytkownicy powinni otrzymywać dostęp do wybranych aplikacji. W kontekście automatycznego inicjowania obsługi kont użytkowników są synchronizowane tylko użytkownicy i grupy, które zostały przypisane do aplikacji w usłudze Azure AD.

Przed skonfigurowaniem i włączeniem usługi aprowizacji należy zdecydować, którzy użytkownicy i/lub grupy w usłudze Azure AD reprezentują użytkowników, którzy potrzebują dostępu do aplikacji spotkanie GoToMeeting. Po ustaleniu tych użytkowników możesz przypisać je do aplikacji spotkanie GoToMeeting, postępując zgodnie z poniższymi instrukcjami:

[Przypisywanie użytkownika lub grupy do aplikacji dla przedsiębiorstw](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-gotomeeting"></a>Ważne wskazówki dotyczące przypisywania użytkowników do spotkanie GoToMeeting

*   Zaleca się, aby jeden użytkownik usługi Azure AD został przypisany do spotkanie GoToMeeting w celu przetestowania konfiguracji aprowizacji. Dodatkowych użytkowników i/lub grupy można przypisywać później.

*   Podczas przypisywania użytkownika do spotkanie GoToMeeting należy wybrać prawidłową rolę użytkownika. Rola "dostęp domyślny" nie działa w przypadku aprowizacji.

## <a name="enable-automated-user-provisioning"></a>Włącz automatyczne Inicjowanie obsługi użytkowników

Ta sekcja przeprowadzi Cię przez proces łączenia się z interfejsem API aprowizacji usługi Azure AD do konta użytkownika spotkanie GoToMeeting, a następnie Konfigurując usługę aprowizacji do tworzenia, aktualizowania i wyłączania przypisanych kont użytkowników w usłudze spotkanie GoToMeeting na podstawie przypisywania użytkowników i grup w usłudze Azure AD.

> [!TIP]
> Możesz również włączyć funkcję logowania jednokrotnego opartego na protokole SAML dla spotkanie GoToMeeting, postępując zgodnie z instrukcjami podanymi w [Azure Portal](https://portal.azure.com). Logowanie jednokrotne można skonfigurować niezależnie od automatycznej aprowizacji, chociaż te dwie funkcje napadają nawzajem.

### <a name="to-configure-automatic-user-account-provisioning"></a>Aby skonfigurować automatyczne Inicjowanie obsługi konta użytkownika:

1. W [Azure Portal](https://portal.azure.com)przejdź do sekcji **Azure Active Directory > aplikacje dla przedsiębiorstw > wszystkie aplikacje** .

1. Jeśli już skonfigurowano spotkanie GoToMeeting do logowania jednokrotnego, Wyszukaj wystąpienie elementu spotkanie GoToMeeting przy użyciu pola wyszukiwania. W przeciwnym razie wybierz pozycję **Dodaj** i Wyszukaj **spotkanie GoToMeeting** w galerii aplikacji. Wybierz pozycję spotkanie GoToMeeting z wyników wyszukiwania, a następnie dodaj ją do listy aplikacji.

1. Wybierz wystąpienie elementu spotkanie GoToMeeting, a następnie wybierz kartę **Inicjowanie obsługi** .

1. Ustaw tryb **aprowizacji** na **automatyczny**. 

    ![aprowizacji](./media/citrixgotomeeting-provisioning-tutorial/provisioning.png)

1. W sekcji poświadczenia administratora wykonaj następujące czynności:
   
    a. W polu tekstowym **Nazwa użytkownika administratora spotkanie GoToMeeting** wpisz nazwę użytkownika administratora.

    b. W polu tekstowym **hasło administratora spotkanie GoToMeeting** hasło administratora.

1. W Azure Portal kliknij pozycję **Testuj połączenie** , aby upewnić się, że usługa Azure AD może nawiązać połączenie z aplikacją spotkanie GoToMeeting. Jeśli połączenie nie powiedzie się, upewnij się, że konto spotkanie GoToMeeting ma uprawnienia administratora zespołu, a następnie spróbuj ponownie wykonać krok **"poświadczenia administratora"** .

1. Wprowadź adres e-mail osoby lub grupy, które powinny otrzymywać powiadomienia o błędach aprowizacji w polu **E-mail powiadomienia** , i zaznacz pole wyboru.

1. Kliknij przycisk **Zapisz.**

1. W sekcji mapowania wybierz pozycję **synchronizuj Azure Active Directory użytkowników do spotkanie GoToMeeting.**

1. W sekcji **mapowania atrybutów** Przejrzyj atrybuty użytkownika, które są synchronizowane z usługi Azure AD do spotkanie GoToMeeting. Atrybuty wybrane jako **pasujące** właściwości są używane do dopasowania kont użytkowników w programie spotkanie GoToMeeting for Updates. Wybierz przycisk Zapisz, aby zatwierdzić zmiany.

1. Aby włączyć usługę Azure AD Provisioning dla usługi spotkanie GoToMeeting, Zmień **stan aprowizacji** na **włączone** w sekcji Ustawienia.

1. Kliknij przycisk **Zapisz.**

Rozpocznie się początkową synchronizację wszystkich użytkowników i/lub grup przypisanych do spotkanie GoToMeeting w sekcji Użytkownicy i grupy. Synchronizacja początkowa trwa dłużej niż kolejne synchronizacje, które wystąpiły co około 40 minut, o ile usługa jest uruchomiona. Za pomocą sekcji **szczegóły synchronizacji** można monitorować postęp i wykonywać linki do dzienników aktywności aprowizacji, które opisują wszystkie akcje wykonywane przez usługę aprowizacji w aplikacji spotkanie GoToMeeting.

Aby uzyskać więcej informacji na temat sposobu odczytywania dzienników aprowizacji usługi Azure AD, zobacz [Raportowanie dotyczące automatycznego inicjowania obsługi konta użytkownika](../app-provisioning/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Zasoby dodatkowe

* [Zarządzanie obsługą kont użytkowników w aplikacjach dla przedsiębiorstw](tutorial-list.md)
* [Co to jest dostęp do aplikacji i logowanie jednokrotne za pomocą Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)
* [Konfigurowanie logowania jednokrotnego](https://docs.microsoft.com/azure/active-directory/active-directory-saas-citrix-gotomeeting-tutorial)


