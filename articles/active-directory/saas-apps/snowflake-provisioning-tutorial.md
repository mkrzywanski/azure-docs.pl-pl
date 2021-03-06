---
title: 'Samouczek: Konfigurowanie płatnej śniegu w celu automatycznego aprowizacji użytkowników przy użyciu Azure Active Directory | Microsoft Docs'
description: Dowiedz się, jak skonfigurować Azure Active Directory, aby automatycznie inicjować udostępnianie kont użytkowników i cofać ich obsługę.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: f9ce85f4-0992-4bc6-8276-4b2efbce8dcb
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2019
ms.author: zhchia
ms.openlocfilehash: 46ebb122b0165d469b1c40871d5939e50a8595c9
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/23/2020
ms.locfileid: "87016324"
---
# <a name="tutorial-configure-snowflake-for-automatic-user-provisioning"></a>Samouczek: Konfigurowanie płatnej śniegu na potrzeby automatycznego aprowizacji użytkowników

Celem tego samouczka jest przedstawienie kroków, które należy wykonać w śniegu i Azure Active Directory (Azure AD) w celu skonfigurowania usługi Azure AD w celu automatycznego aprowizacji i cofania aprowizacji użytkowników i/lub grup na [płatki śniegu](https://www.Snowflake.com/pricing/). Aby uzyskać ważne informacje o tym, jak działa ta usługa, jak ona dotyczy, i często zadawanych pytań, zobacz [Automatyzowanie aprowizacji użytkowników i Anulowanie udostępniania aplikacji SaaS przy użyciu programu Azure Active Directory](../manage-apps/user-provisioning.md). 


> [!NOTE]
> Ten łącznik jest obecnie w publicznej wersji zapoznawczej. Aby uzyskać więcej informacji na temat ogólnych Microsoft Azure warunki użytkowania funkcji w wersji zapoznawczej, zobacz [dodatkowe warunki użytkowania dla Microsoft Azure podglądów](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="capabilities-supported"></a>Obsługiwane możliwości
> [!div class="checklist"]
> * Tworzenie użytkowników w śniegu
> * Usuń użytkowników w śniegu, gdy nie wymagają już dostępu
> * Utrzymywanie synchronizacji atrybutów użytkowników między usługą Azure AD i Płatą śniegu
> * Udostępnianie grup i członkostwa w grupach w śniegu
> * [Logowanie](https://docs.microsoft.com/azure/active-directory/saas-apps/snowflake-tutorial) jednokrotne do śniegu (zalecane)

## <a name="prerequisites"></a>Wymagania wstępne

Scenariusz opisany w tym samouczku założono, że masz już następujące wymagania wstępne:

* [Dzierżawa usługi Azure AD](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant).
* Konto użytkownika w usłudze Azure AD z [uprawnieniami](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) do konfigurowania aprowizacji (np. Administrator aplikacji, administrator aplikacji w chmurze, właściciel aplikacji lub Administrator globalny).
* [Dzierżawca płatka śniegu](https://www.Snowflake.com/pricing/).
* Konto użytkownika w śniegu z uprawnieniami administratora.

## <a name="step-1-plan-your-provisioning-deployment"></a>Krok 1. Planowanie wdrożenia aprowizacji
1. Dowiedz się [, jak działa usługa aprowizacji](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning).
2. Określ, kto będzie [objęty zakresem aprowizacji](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts).
3. Określ, które dane mają być [mapowane między usługą Azure AD i płatą śniegu](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes). 

## <a name="step-2-configure-snowflake-to-support-provisioning-with-azure-ad"></a>Krok 2. Konfigurowanie obsługi administracyjnej za pomocą usługi Azure AD

Przed skonfigurowaniem płatnej śniegu w celu automatycznego aprowizacji użytkowników przy użyciu usługi Azure AD należy włączyć funkcję aprowizacji Standard scim na śniegu.

1. Zaloguj się do konsoli administracyjnej płatnej śniegu. Wprowadź poniżej zapytanie w wyróżnionym arkuszu, a następnie kliknij przycisk **Uruchom**.

    ![Konsola administracyjna płatka śniegu](media/Snowflake-provisioning-tutorial/image00.png)

2.  Token dostępu Standard scim zostanie wygenerowany dla dzierżawy płatnej. Aby go pobrać, kliknij link wyróżniony poniżej.

    ![Dodaj Standard SCIMa śniegu](media/Snowflake-provisioning-tutorial/image01.png)

3. Skopiuj wygenerowaną wartość tokenu i kliknij przycisk **gotowe**. Ta wartość zostanie wprowadzona w polu **token tajny** na karcie aprowizacji aplikacji płatnej śniegu w Azure Portal.

    ![Dodaj Standard SCIMa śniegu](media/Snowflake-provisioning-tutorial/image02.png)

## <a name="step-3-add-snowflake-from-the-azure-ad-application-gallery"></a>Krok 3. Dodaj płatną śnieg z galerii aplikacji usługi Azure AD

Dodaj płatną śnieg z galerii aplikacji usługi Azure AD, aby rozpocząć zarządzanie obsługą na Płatę śniegu. Jeśli wcześniej skonfigurowano płatny śnieg na potrzeby logowania jednokrotnego, możesz użyć tej samej aplikacji. Jednak zaleca się utworzenie osobnej aplikacji podczas wstępnego testowania integracji. Dowiedz się więcej o dodawaniu aplikacji z galerii [tutaj](https://docs.microsoft.com/azure/active-directory/manage-apps/add-gallery-app). 

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>Krok 4. Zdefiniuj, kto będzie w zakresie aprowizacji 

Usługa Azure AD Provisioning umożliwia określenie zakresu użytkowników, którzy będą obsługiwani w oparciu o przypisanie do aplikacji i lub na podstawie atrybutów użytkownika/grupy. Jeśli wybierzesz zakres, który zostanie zainicjowany do aplikacji na podstawie przypisania, możesz wykonać następujące [kroki](../manage-apps/assign-user-or-group-access-portal.md) , aby przypisać użytkowników i grupy do aplikacji. Jeśli zdecydujesz się na określenie zakresu, który zostanie zainicjowany na podstawie atrybutów użytkownika lub grupy, możesz użyć filtru określania zakresu, zgodnie z opisem w [tym miejscu](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts). 

* Podczas przypisywania użytkowników i grup do śniegu należy wybrać rolę inną niż **domyślny dostęp**. Użytkownicy z domyślną rolą dostępu są wykluczeni z aprowizacji i zostaną oznaczeni jako nieskutecznie uprawnieni do dzienników aprowizacji. Jeśli jedyną rolą dostępną w aplikacji jest domyślna rola dostępu, można [zaktualizować manifest aplikacji](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps) , aby dodać dodatkowe role. 

* Zacznij od małych. Przetestuj przy użyciu małego zestawu użytkowników i grup przed przekazaniem ich do wszystkich osób. W przypadku wybrania dla zakresu aprowizacji przypisanych użytkowników i grup można kontrolować ten sposób, przypisując do aplikacji jednego lub dwóch użytkowników lub grupy. Gdy zakres jest ustawiony dla wszystkich użytkowników i grup, można określić [Filtr określania zakresu na podstawie atrybutu](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts). 


## <a name="step-5-configure-automatic-user-provisioning-to-snowflake"></a>Krok 5. Skonfiguruj automatyczne Inicjowanie obsługi administracyjnej użytkowników na Płatę śniegu 

Ta sekcja przeprowadzi Cię przez kroki konfigurowania usługi Azure AD Provisioning w celu tworzenia, aktualizowania i wyłączania użytkowników i/lub grup w śniegu na podstawie przypisań użytkowników i/lub grup w usłudze Azure AD.

### <a name="to-configure-automatic-user-provisioning-for-snowflake-in-azure-ad"></a>Aby skonfigurować automatyczne Inicjowanie obsługi użytkowników dla płatnych śniegów w usłudze Azure AD:

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com). Wybierz pozycję **aplikacje dla przedsiębiorstw**, a następnie wybierz pozycję **wszystkie aplikacje**.

    ![Blok Aplikacje dla przedsiębiorstw](common/enterprise-applications.png)

2. Na liście Aplikacje wybierz pozycję **płatka śnieg**.

    ![Link aplikacji Snowflake na liście aplikacji](common/all-applications.png)

3. Wybierz kartę **aprowizacji** .

    ![Karta aprowizacji](common/provisioning.png)

4. Ustaw **tryb aprowizacji** na **automatyczny**.

    ![Karta aprowizacji](common/provisioning-automatic.png)

5. W sekcji poświadczenia administratora wprowadź odpowiednie wartości **w polach** **adres** URL i **token uwierzytelniania Standard scim 2,0** . Kliknij pozycję **Testuj połączenie** , aby upewnić się, że usługa Azure AD może nawiązać połączenie z płatą Jeśli połączenie zakończy się niepowodzeniem, upewnij się, że Twoje konto płatne w sieci śnieg ma uprawnienia administratora i spróbuj ponownie.

    ![Adres URL dzierżawy + token](common/provisioning-testconnection-tenanturltoken.png)

7. W polu **adres E-mail powiadomienia** wprowadź adres e-mail osoby lub grupy, które powinny otrzymywać powiadomienia o błędach aprowizacji, i zaznacz pole wyboru — **Wyślij powiadomienie e-mail, gdy wystąpi awaria**.

    ![Wiadomość E-mail z powiadomieniem](common/provisioning-notification-email.png)

8. Kliknij pozycję **Zapisz**.

9. W sekcji **mapowania** wybierz opcję **Synchronizuj Azure Active Directory użytkownicy z płatą śniegu**.

10. Przejrzyj atrybuty użytkownika, które są synchronizowane z usługi Azure AD do śniegu w sekcji **Mapowanie atrybutów** . Atrybuty wybrane jako **pasujące** właściwości są używane w celu dopasowania do kont użytkowników w śniegu w przypadku operacji aktualizacji. Wybierz przycisk **Zapisz** , aby zatwierdzić zmiany.

   |Atrybut|Typ|
   |---|---|
   |aktywne|Boolean (wartość logiczna)|
   |displayName|String (ciąg)|
   |wiadomości e-mail [Type EQ "Work"]. Value|String (ciąg)|
   |userName|String (ciąg)|
   |Nazwa. imię|String (ciąg)|
   |Nazwa. rodzina|String (ciąg)|
   |urn: IETF: params: Standard scim: schematy: rozszerzenie: Enterprise: 2.0: User: defaultRole|String (ciąg)|
   |urn: IETF: params: Standard scim: schematy: rozszerzenie: Enterprise: 2.0: User: defaultWarehouse|String (ciąg)|

11. W sekcji **mapowania** wybierz pozycję **Synchronizuj grupy Azure Active Directory z płatą śniegu**.

12. Przejrzyj atrybuty grupy, które są synchronizowane z usługi Azure AD do śniegu w sekcji **Mapowanie atrybutów** . Atrybuty wybrane jako **pasujące** właściwości są używane do dopasowania grup w śniegu dla operacji aktualizacji. Wybierz przycisk **Zapisz** , aby zatwierdzić zmiany.

      |Atrybut|Typ|
      |---|---|
      |displayName|String (ciąg)|
      |elementy członkowskie|Dokumentacja|

13. Aby skonfigurować filtry określania zakresu, zapoznaj się z poniższymi instrukcjami w [samouczku dotyczącym filtru określania zakresu](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).

14. Aby włączyć usługę Azure AD Provisioning dla płatka śnieg, Zmień **stan aprowizacji** na **włączone** w sekcji **Ustawienia** .

    ![Stan aprowizacji jest przełączany](common/provisioning-toggle-on.png)

15. Zdefiniuj użytkowników i/lub grupy, które chcesz udostępnić śnieg, wybierając odpowiednie wartości w **zakresie** w sekcji **Ustawienia** . Jeśli ta opcja jest niedostępna, skonfiguruj wymagane pola w obszarze poświadczenia administratora, a następnie kliknij przycisk **Zapisz** i Odśwież stronę. 

    ![Zakres aprowizacji](common/provisioning-scope.png)

16. Gdy wszystko będzie gotowe do udostępnienia, kliknij przycisk **Zapisz**.

    ![Zapisywanie konfiguracji aprowizacji](common/provisioning-configuration-save.png)

    Ta operacja uruchamia początkową synchronizację wszystkich użytkowników i/lub grup zdefiniowanych w **zakresie** w sekcji **Ustawienia** . Synchronizacja początkowa trwa dłużej niż kolejne synchronizacje, które wystąpiły co około 40 minut, o ile usługa Azure AD Provisioning jest uruchomiona.

## <a name="step-6-monitor-your-deployment"></a>Krok 6. Monitorowanie wdrożenia
Po skonfigurowaniu aprowizacji Użyj następujących zasobów do monitorowania wdrożenia:

1. Użyj [dzienników aprowizacji](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs) , aby określić, którzy użytkownicy zostali zainicjowani pomyślnie lub niepomyślnie
2. Sprawdź [pasek postępu](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-when-will-provisioning-finish-specific-user) , aby zobaczyć stan cyklu aprowizacji oraz sposób jego zakończenia.
3. Jeśli konfiguracja aprowizacji wydaje się być w złej kondycji, aplikacja zostanie przestawiona na kwarantannę. Więcej informacji o Stanach kwarantanny znajduje się [tutaj](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-quarantine-status).  

## <a name="connector-limitations"></a>Ograniczenia łącznika

* Tokeny Standard scim wygenerowały płaty śniegu wygaśnie w ciągu 6 miesięcy. Należy pamiętać, że należy je odświeżyć przed wygaśnięciem, aby umożliwić kontynuowanie synchronizacji aprowizacji. 

## <a name="change-log"></a>Dziennik zmian

* 07/21/2020 — możliwość usuwania nietrwałego dla wszystkich użytkowników (za pośrednictwem aktywnego atrybutu).

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Zarządzanie obsługą kont użytkowników w aplikacjach dla przedsiębiorstw](../app-provisioning/configure-automatic-user-provisioning-portal.md).
* [Co to jest dostęp do aplikacji i logowanie jednokrotne za pomocą Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Następne kroki
* [Dowiedz się, jak przeglądać dzienniki i uzyskiwać raporty dotyczące działań aprowizacji](../app-provisioning/check-status-user-account-provisioning.md).
