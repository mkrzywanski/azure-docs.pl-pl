---
title: Tworzenie przeglądu dostępu do ról zasobów platformy Azure w usłudze PIM — Azure AD | Microsoft Docs
description: Dowiedz się, jak utworzyć przegląd dostępu ról zasobów platformy Azure w Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.subservice: pim
ms.date: 11/08/2019
ms.author: curtand
ms.custom: pim
ms.collection: M365-identity-device-management
ms.openlocfilehash: fd9e3aaf0747e9b28526a43d9f734084f8a75662
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "84743698"
---
# <a name="create-an-access-review-of-azure-resource-roles-in-privileged-identity-management"></a>Tworzenie przeglądu dostępu do ról zasobów platformy Azure w Privileged Identity Management

Dostęp do ról uprzywilejowanych zasobów platformy Azure dla pracowników zmienia się w czasie. Aby zmniejszyć ryzyko związane ze starymi przypisaniami ról, należy regularnie przeglądać dostęp. Za pomocą usługi Azure Active Directory (Azure AD) Privileged Identity Management (PIM) można tworzyć przeglądy dostępu dla uprzywilejowanych ról zasobów platformy Azure. Istnieje również możliwość skonfigurowania cyklicznych przeglądów dostępu, które są wykonywane automatycznie.

W tym artykule opisano sposób tworzenia co najmniej jednej oceny dostępu dla uprzywilejowanych ról zasobów platformy Azure.

## <a name="prerequisites"></a>Wymagania wstępne

[Administrator ról uprzywilejowanych](../users-groups-roles/directory-assign-admin-roles.md#privileged-role-administrator)

## <a name="open-access-reviews"></a>Otwarte przeglądy dostępu

1. Zaloguj się do [Azure Portal](https://portal.azure.com/) przy użyciu użytkownika, który jest członkiem roli administrator ról uprzywilejowanych.

1. Otwórz **Azure AD Privileged Identity Management**.

1. W menu po lewej stronie wybierz pozycję **zasoby platformy Azure**.

1. Wybierz zasób, którym chcesz zarządzać, na przykład subskrypcję lub grupę zarządzania.

1. W obszarze Zarządzaj wybierz pozycję **przeglądy dostępu**.

    ![Zasoby platformy Azure — lista przeglądów dostępu pokazująca stan wszystkich przeglądów](./media/pim-resource-roles-start-access-review/access-reviews.png)

[!INCLUDE [Privileged Identity Management access reviews](../../../includes/active-directory-privileged-identity-management-access-reviews.md)]

## <a name="start-the-access-review"></a>Rozpocznij przegląd dostępu

Po określeniu ustawień przeglądu dostępu kliknij przycisk **Uruchom**. Przegląd dostępu zostanie wyświetlony na liście ze wskaźnikiem jego stanu.

![Lista przeglądów dostępu pokazująca stan rozpoczętego przeglądu](./media/pim-resource-roles-start-access-review/access-reviews-list.png)

Domyślnie usługa Azure AD wysyła wiadomość e-mail do recenzentów wkrótce po rozpoczęciu przeglądu. Jeśli nie chcesz, aby usługa Azure AD wysłała wiadomość e-mail, pamiętaj, aby poinformować recenzentów, że przegląd dostępu oczekuje na jego zakończenie. Można wyświetlić instrukcje dotyczące sposobu [przeglądania dostępu do ról zasobów platformy Azure](pim-resource-roles-perform-access-review.md).

## <a name="manage-the-access-review"></a>Zarządzanie przeglądem dostępu

Postęp można śledzić, ponieważ recenzenci ukończyją przeglądy na stronie **Przegląd** przeglądu dostępu. Żadne prawa dostępu nie są zmieniane w katalogu, dopóki nie [zostanie ukończona Recenzja](pim-resource-roles-complete-access-review.md).

![Strona przeglądu dostępu przegląd przedstawia szczegóły przeglądu](./media/pim-resource-roles-start-access-review/access-review-overview.png)

Jeśli jest to jednorazowe przegląd, po przekroczeniu okresu kontroli dostępu lub przejściu przez administratora do przeglądu dostępu wykonaj kroki opisane w temacie [Kończenie przeglądu dostępu ról zasobów platformy Azure](pim-resource-roles-complete-access-review.md) , aby zobaczyć i zastosować wyniki.  

Aby zarządzać serią przeglądów dostępu, przejdź do przeglądu dostępu i znajdziesz nadchodzące wystąpienia w zaplanowanych recenzjach, a następnie edytuj datę końcową lub Dodaj/Usuń recenzentów.

Po dokonaniu wyboru w **ustawieniach ukończenia**funkcja automatycznego zastosowania zostanie wykonana po zakończeniu przeglądu lub po ręcznym zatrzymaniu przeglądu. Stan przeglądu zmieni się z **zakończone** przez Stany pośrednie, takie jak **stosowanie** i ostatecznie do stanu **zastosowania**. Należy się spodziewać, że w ciągu kilku minut będzie widocznych odrzuconych użytkowników (jeśli istnieją).

## <a name="next-steps"></a>Następne kroki

- [Przeglądanie dostępu do ról zasobów platformy Azure](pim-resource-roles-perform-access-review.md)
- [Ukończ przegląd dostępu do ról zasobów platformy Azure](pim-resource-roles-complete-access-review.md)
- [Tworzenie przeglądu dostępu do ról usługi Azure AD](pim-how-to-start-security-review.md)
