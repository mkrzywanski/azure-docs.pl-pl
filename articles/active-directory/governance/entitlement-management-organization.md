---
title: Dodawanie połączonej organizacji w usłudze Azure AD uprawnień zarządzanie — Azure Active Directory
description: Dowiedz się, jak zezwolić osobom spoza organizacji na żądanie pakietów dostępu, dzięki czemu można współpracować nad projektami.
services: active-directory
documentationCenter: ''
author: barclayn
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.subservice: compliance
ms.date: 06/18/2020
ms.author: barclayn
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: 272dd95b97c65ecc52dd73909f1ed87d5e5ae3ca
ms.sourcegitcommit: 1e6c13dc1917f85983772812a3c62c265150d1e7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/09/2020
ms.locfileid: "86170500"
---
# <a name="add-a-connected-organization-in-azure-ad-entitlement-management"></a>Dodawanie połączonej organizacji w zarządzaniu prawami usługi Azure AD

Dzięki usłudze Azure Active Directory (Azure AD) Zarządzanie uprawnieniami można współpracować z osobami spoza organizacji. Jeśli często pracujesz z użytkownikami w zewnętrznym katalogu lub domenie usługi Azure AD, możesz je dodać jako podłączoną organizację. W tym artykule opisano sposób dodawania połączonej organizacji w taki sposób, aby umożliwić użytkownikom spoza organizacji żądanie zasobów w katalogu.

## <a name="what-is-a-connected-organization"></a>Co to jest połączona organizacja?

Połączona organizacja to zewnętrzny katalog usługi Azure AD lub domena, z którą istnieje relacja.

Załóżmy na przykład, że Pracujesz w banku Woodgrove i chcesz współpracować z dwiema organizacjami zewnętrznymi. Te dwie organizacje mają różne konfiguracje:

- Graficzny Instytut projektowania używa usługi Azure AD, a ich użytkownicy mają główną nazwę użytkownika kończącą się na *graphicdesigninstitute.com*.
- Firma Contoso nie korzysta jeszcze z usługi Azure AD. Użytkownicy firmy Contoso mają główną nazwę użytkownika kończącą się na *contoso.com*.

W takim przypadku można skonfigurować dwie połączone organizacje. Utworzysz jedną podłączoną organizację dla Instytutu projektowania grafiki i jeden dla contoso. Jeśli następnie dodasz dwie połączone organizacje do zasad, użytkownicy z każdej organizacji z główną nazwą użytkownika zgodną z zasadami mogą żądać pakietów dostępu. Użytkownicy z główną nazwą użytkownika, która ma domenę *graphicdesigninstitute.com* , będą odpowiadały organizacji połączonej z Instytutem projektu graficznego i mogą przesyłać żądania. Użytkownicy z główną nazwą użytkownika, która ma domenę *contoso.com* , będą odpowiadały organizacji połączonej z contoso, a także mogą żądać pakietów. Ponadto, ponieważ Instytut projektowania grafiki używa usługi Azure AD, wszyscy użytkownicy z główną nazwą, która pasuje do [zweryfikowanej domeny](../fundamentals/add-custom-domain.md#verify-your-custom-domain-name) dodanej do swojej dzierżawy, takiej jak *graphicdesigninstitute. example*, mogą również żądać pakietów dostępu przy użyciu tych samych zasad.

![Przykład połączonej organizacji](./media/entitlement-management-organization/connected-organization-example.png)

Sposób uwierzytelniania użytkowników z katalogu usługi Azure AD lub domeny zależy od typu uwierzytelniania. Typy uwierzytelniania dla połączonych organizacji są następujące:

- Azure AD
- [Federacja bezpośrednia](../b2b/direct-federation.md)
- [Jednorazowy kod dostępu](../b2b/one-time-passcode.md) (domena)

Aby zapoznać się z pokazem, jak dodać podłączoną organizację, Obejrzyj następujące wideo:

>[!VIDEO https://www.microsoft.com/videoplayer/embed/RE4dskS]

## <a name="add-a-connected-organization"></a>Dodawanie połączonej organizacji

Aby dodać zewnętrzny katalog lub domenę usługi Azure AD jako podłączoną organizację, postępuj zgodnie z instrukcjami w tej sekcji.

**Rola wymagana wstępnie**: *administrator globalny* lub *administrator użytkownika*

1. W Azure Portal wybierz pozycję **Azure Active Directory**, a następnie wybierz pozycję **Zarządzanie tożsamościami**.

1. W lewym okienku wybierz pozycję **połączone organizacje**, a następnie wybierz pozycję **Dodaj połączoną organizację**.

    ![Przycisk "Dodaj podłączoną organizację"](./media/entitlement-management-organization/connected-organization.png)

1. Wybierz kartę **podstawowe** , a następnie wprowadź nazwę wyświetlaną i opis organizacji.

    ![Okienko podstawowe "Dodawanie połączonej organizacji"](./media/entitlement-management-organization/organization-basics.png)

1. Wybierz kartę **katalog + domena** , a następnie wybierz pozycję **Dodaj katalog + domena**.

    Zostanie otwarte okienko **Wybieranie katalogów i domen** .

1. W polu wyszukiwania wprowadź nazwę domeny, aby wyszukać katalog lub domenę usługi Azure AD. Pamiętaj, aby wprowadzić całą nazwę domeny.

1. Sprawdź, czy nazwa organizacji i typ uwierzytelniania są poprawne. Sposób logowania użytkowników zależy od typu uwierzytelniania.

    ![Okienko "wybieranie katalogów i domen"](./media/entitlement-management-organization/organization-select-directories-domains.png)

1. Wybierz pozycję **Dodaj** , aby dodać katalog lub domenę usługi Azure AD. Obecnie można dodać tylko jeden katalog lub domenę usługi Azure AD na podłączoną organizację.

    > [!NOTE]
    > Wszyscy użytkownicy z katalogu lub domeny usługi Azure AD będą mogli zażądać tego pakietu dostępu. Obejmuje to użytkowników w usłudze Azure AD ze wszystkich poddomen skojarzonych z katalogiem, chyba że te domeny są blokowane przez listę dozwolonych lub zablokowanych usług Azure AD Business to Business (B2B). Aby uzyskać więcej informacji, zobacz [Zezwalanie lub blokowanie zaproszeń użytkownikom B2B z określonych organizacji](../b2b/allow-deny-list.md).

1. Po dodaniu katalogu lub domeny usługi Azure AD wybierz pozycję **Wybierz**.

    Organizacja zostanie wyświetlona na liście.

    ![Okienko "katalog + domena"](./media/entitlement-management-organization/organization-directory-domain.png)

1. Wybierz kartę **sponsorzy** , a następnie Dodaj opcjonalne sponsorzy dla tej połączonej organizacji.

    Sponsorzy są wewnętrznymi lub zewnętrznymi użytkownikami już w katalogu, które są punktem kontaktu dla relacji z tą połączoną organizacją. Wewnętrzni Sponsorzy są użytkownikami należącymi do katalogu. Zewnętrzne sponsorzy to użytkownicy-Goście z połączonej organizacji, którzy zostali wcześniej zaproszeni i znajdują się już w katalogu. Sponsorzy mogą być używani jako osoby zatwierdzające, gdy użytkownicy w tej połączonej organizacji żądają dostępu do tego pakietu dostępu. Aby uzyskać informacje na temat zapraszania użytkownika-gościa do katalogu, zobacz [dodawanie Azure Active Directory użytkowników współpracy B2B w Azure Portal](../b2b/add-users-administrator.md).

    Po wybraniu opcji **Dodaj/Usuń**zostanie otwarte okienko, w którym można wybrać wewnętrznych lub zewnętrznych sponsorów. W okienku zostanie wyświetlona niefiltrowana lista użytkowników i grup w Twoim katalogu.

    ![Okienko sponsorzy](./media/entitlement-management-organization/organization-sponsors.png)

1. Wybierz kartę **Recenzja + tworzenie** , przejrzyj ustawienia organizacji, a następnie wybierz pozycję **Utwórz**.

    ![Okienko "przegląd + tworzenie"](./media/entitlement-management-organization/organization-review-create.png)

## <a name="update-a-connected-organization"></a>Aktualizowanie połączonej organizacji 

Jeśli połączona organizacja zmieni się w inną domenę, nazwa organizacji ulegnie zmianie lub chcesz zmienić sponsorów, możesz zaktualizować połączoną organizację, postępując zgodnie z instrukcjami w tej sekcji.

**Rola wymagana wstępnie**: *administrator globalny*, *administrator użytkownika*lub *zapraszający Gości*

1. W Azure Portal wybierz pozycję **Azure Active Directory**, a następnie wybierz pozycję **Zarządzanie tożsamościami**.

1. W lewym okienku wybierz pozycję **połączone organizacje**, a następnie wybierz połączoną organizację, aby ją otworzyć.

1. W okienku Przegląd połączonej organizacji wybierz pozycję **Edytuj** , aby zmienić nazwę lub opis organizacji.  

1. W okienku **katalog + domena** wybierz pozycję **Aktualizuj katalog i domenę** , aby zmienić na inny katalog lub domenę.

1. W okienku **sponsorzy** wybierz pozycję **Dodaj wewnętrznych sponsorów** lub **Dodaj zewnętrznych sponsorów** , aby dodać użytkownika jako sponsora. Aby usunąć sponsora, wybierz sponsora i w okienku po prawej stronie wybierz pozycję **Usuń**.


## <a name="delete-a-connected-organization"></a>Usuwanie połączonej organizacji

Jeśli nie masz już relacji z zewnętrznym katalogiem lub domeną usługi Azure AD, możesz usunąć połączoną organizację.

**Rola wymagana wstępnie**: *administrator globalny*, *administrator użytkownika*lub *zapraszający Gości*

1. W Azure Portal wybierz pozycję **Azure Active Directory**, a następnie wybierz pozycję **Zarządzanie tożsamościami**.

1. W lewym okienku wybierz pozycję **połączone organizacje**, a następnie wybierz połączoną organizację, aby ją otworzyć.

1. W okienku Przegląd połączonej organizacji wybierz pozycję **Usuń** , aby ją usunąć.

    Obecnie można usunąć połączoną organizację tylko wtedy, gdy nie ma żadnych podłączonych użytkowników.

    ![Przycisk Usuń połączonej organizacji](./media/entitlement-management-organization/organization-delete.png)

## <a name="next-steps"></a>Następne kroki

- [Zarządzanie dostępem dla użytkowników zewnętrznych](https://docs.microsoft.com/azure/active-directory/governance/entitlement-management-external-users)
- [Zarządzanie dostępem użytkowników nieznajdujących się w katalogu](entitlement-management-access-package-request-policy.md#for-users-not-in-your-directory)
