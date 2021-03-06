---
title: 'Samouczek: Konfigurowanie OneWorld pakietu dla automatycznej aprowizacji użytkowników przy użyciu Azure Active Directory | Microsoft Docs'
description: Dowiedz się, jak skonfigurować Logowanie jednokrotne między Azure Active Directory a pakietem OneWorld.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 8a6d3994-ee33-4a6f-b0a2-9d0389467f16
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4c9a823e6515c2bfe09e1ab7bcef471eb8169e75
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "77063299"
---
# <a name="tutorial-configuring-netsuite-for-automatic-user-provisioning"></a>Samouczek: Konfigurowanie pakietu dla automatycznej aprowizacji użytkowników

Celem tego samouczka jest przedstawienie czynności, które należy wykonać w pakietach OneWorld i Azure AD w celu automatycznego aprowizacji i cofania aprowizacji kont użytkowników z usługi Azure AD do pakietu.

## <a name="prerequisites"></a>Wymagania wstępne

Scenariusz opisany w tym samouczku założono, że masz już następujące elementy:

*   Dzierżawa usługi Azure Active Directory.
*   Subskrypcja OneWorld pakietu. Należy pamiętać, że automatyczne Inicjowanie obsługi użytkowników jest obecnie obsługiwane tylko z pakietem OneWorld.
*   Konto użytkownika w pakiecie z uprawnieniami administratora.

## <a name="assigning-users-to-netsuite-oneworld"></a>Przypisywanie użytkowników do pakietu OneWorld

Azure Active Directory używa koncepcji o nazwie "przydziały", aby określić, którzy użytkownicy powinni otrzymywać dostęp do wybranych aplikacji. W kontekście automatycznego inicjowania obsługi kont użytkowników są synchronizowane tylko użytkownicy i grupy, które zostały przypisane do aplikacji w usłudze Azure AD.

Przed skonfigurowaniem i włączeniem usługi aprowizacji należy zdecydować, którzy użytkownicy i/lub grupy w usłudze Azure AD reprezentują użytkowników, którzy potrzebują dostępu do aplikacji pakietu. Po ustaleniu tych użytkowników można przypisać je do swojej aplikacji pakietu, postępując zgodnie z poniższymi instrukcjami:

[Przypisywanie użytkownika lub grupy do aplikacji dla przedsiębiorstw](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

### <a name="important-tips-for-assigning-users-to-netsuite-oneworld"></a>Ważne porady dotyczące przypisywania użytkowników do pakietu OneWorld

*   Zaleca się, aby jeden użytkownik usługi Azure AD został przypisany do pakietu servicesuite w celu przetestowania konfiguracji aprowizacji. Dodatkowych użytkowników i/lub grupy można przypisywać później.

*   Podczas przypisywania użytkownika do pakietu, należy wybrać prawidłową rolę użytkownika. Rola "dostęp domyślny" nie działa w przypadku aprowizacji.

## <a name="enable-user-provisioning"></a>Włącz Inicjowanie obsługi użytkowników

Ta sekcja przeprowadzi Cię przez proces łączenia się z interfejsem API inicjowania obsługi konta użytkownika usługi Azure AD i konfigurowania usługi aprowizacji w celu tworzenia, aktualizowania i wyłączania przypisanych kont użytkowników w ramach pakietu z zestawu na podstawie przypisania użytkowników i grup w usłudze Azure AD.

> [!TIP] 
> Możesz również włączyć funkcję logowania jednokrotnego opartego na protokole SAML dla pakietu, postępując zgodnie z instrukcjami podanymi w [Azure Portal](https://portal.azure.com). Logowanie jednokrotne można skonfigurować niezależnie od automatycznej aprowizacji, chociaż te dwie funkcje napadają nawzajem.

### <a name="to-configure-user-account-provisioning"></a>Aby skonfigurować Inicjowanie obsługi konta użytkownika:

Celem tej sekcji jest zaprojektowanie sposobu włączania obsługi administracyjnej użytkowników Active Directory kont użytkowników do pakietu.

1. W [Azure Portal](https://portal.azure.com)przejdź do sekcji **Azure Active Directory > aplikacje dla przedsiębiorstw > wszystkie aplikacje** .

1. Jeśli już skonfigurowano pakiet dla logowania jednokrotnego, Wyszukaj swoje wystąpienie pakietu w polu wyszukiwania. W przeciwnym razie wybierz pozycję **Dodaj** i Wyszukaj **zestaw** w galerii aplikacji. Wybierz pozycję zestaw sieci z wyników wyszukiwania, a następnie dodaj go do listy aplikacji.

1. Wybierz swoje wystąpienie z pakietu, a następnie wybierz kartę **aprowizacji** .

1. Ustaw **tryb aprowizacji** na **automatyczny**. 

    ![aprowizacji](./media/netsuite-provisioning-tutorial/provisioning.png)

1. W sekcji **poświadczenia administratora** podaj następujące ustawienia konfiguracji:
   
    a. W polu tekstowym **Nazwa użytkownika administratora** wpisz nazwę konta pakietu, które ma profil **administratora systemu** w NetSuite.com przypisane.
   
    b. W polu tekstowym **hasło administratora** wpisz hasło dla tego konta.
      
1. W Azure Portal kliknij pozycję **Testuj połączenie** , aby upewnić się, że usługa Azure AD może nawiązać połączenie z aplikacją pakietu.

1. W polu **adres E-mail powiadomienia** wprowadź adres e-mail osoby lub grupy, które powinny otrzymywać powiadomienia o błędach aprowizacji, i zaznacz pole wyboru.

1. Kliknij przycisk **Zapisz.**

1. W sekcji mapowania wybierz pozycję **synchronizuj Azure Active Directory użytkownicy z pakietem.**

1. W sekcji **mapowania atrybutów** Przejrzyj atrybuty użytkownika, które są synchronizowane z usługi Azure AD z pakietem. Należy zauważyć, że atrybuty wybrane jako **pasujące** właściwości są używane w celu dopasowania do kont użytkowników w pakiecie dla operacji aktualizacji. Wybierz przycisk Zapisz, aby zatwierdzić zmiany.

1. Aby włączyć usługę Azure AD Provisioning dla pakietu, Zmień **stan aprowizacji** na **włączone** w sekcji Ustawienia.

1. Kliknij przycisk **Zapisz.**

Rozpocznie się początkową synchronizację wszystkich użytkowników i/lub grup przypisanych do pakietu, w sekcji Użytkownicy i grupy. Należy pamiętać, że synchronizacja początkowa trwa dłużej niż kolejne synchronizacje, które wystąpiły około 40 minut, o ile usługa jest uruchomiona. Za pomocą sekcji **szczegóły synchronizacji** można monitorować postęp i wykonywać linki do dzienników aktywności aprowizacji, które opisują wszystkie akcje wykonywane przez usługę aprowizacji w aplikacji pakietu.

Aby uzyskać więcej informacji na temat sposobu odczytywania dzienników aprowizacji usługi Azure AD, zobacz [Raportowanie dotyczące automatycznego inicjowania obsługi konta użytkownika](../app-provisioning/check-status-user-account-provisioning.md).

## <a name="additional-resources"></a>Zasoby dodatkowe

* [Zarządzanie obsługą kont użytkowników w aplikacjach dla przedsiębiorstw](tutorial-list.md)
* [Co to jest dostęp do aplikacji i logowanie jednokrotne za pomocą Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)
* [Konfigurowanie logowania jednokrotnego](netsuite-tutorial.md)
