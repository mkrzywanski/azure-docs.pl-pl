---
title: Jak przypisać użytkowników do aplikacji | Microsoft Docs
description: Informacje o sposobie przypisywania użytkowników do aplikacji w dzierżawie
services: active-directory
documentationcenter: ''
author: kenwith
manager: celestedg
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 07/11/2017
ms.author: kenwith
ms.collection: M365-identity-device-management
ms.openlocfilehash: 45124862fffa3a1ef6f601733407fbbea4eb5e74
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "84762976"
---
# <a name="how-to-assign-users-to-applications"></a>Jak przypisać użytkowników do aplikacji

Ten artykuł ułatwia zrozumienie sposobu, w jaki użytkownicy są przypisani do aplikacji w dzierżawie.

## <a name="how-do-users-get-assigned-to-an-application-in-azure-ad"></a>Jak użytkownicy są przypisani do aplikacji w usłudze Azure AD?

Aby użytkownik mógł uzyskać dostęp do aplikacji, musi zostać najpierw przypisany do niej w jakiś sposób. Przypisanie może być wykonywane przez administratora, delegata biznesowego lub czasami samego użytkownika. Poniżej opisano sposób, w jaki użytkownicy mogą uzyskać przypisane do aplikacji:

1.  Administrator [przypisuje użytkownikowi](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) bezpośrednio aplikację

2.  Administrator [przypisuje grupę](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal) , do której należy użytkownik, w tym:

    * Grupa, która została zsynchronizowana z poziomu lokalnego

    * Statyczna Grupa zabezpieczeń utworzona w chmurze

    * [Dynamiczna grupa zabezpieczeń](https://docs.microsoft.com/azure/active-directory/active-directory-groups-dynamic-membership-azure-portal) utworzona w chmurze

    * Grupa pakietu Office 365 utworzona w chmurze

    * Grupa [Wszyscy użytkownicy](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-dedicated-groups)

3.  Administrator umożliwia [samoobsługowy dostęp do aplikacji](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) , aby umożliwić użytkownikowi dodawanie aplikacji za pomocą [panelu dostępu do aplikacji](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) **Dodaj funkcję aplikacji** **bez zgody z firmy**

4.  Administrator umożliwia [samoobsługowy dostęp do aplikacji](https://docs.microsoft.com/azure/active-directory/active-directory-self-service-application-access) , aby umożliwić użytkownikowi dodawanie aplikacji za pomocą [panelu dostępu do aplikacji](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) **Dodaj** funkcję aplikacji, ale tylko**z poprzednimi zatwierdzeniami z wybranego zestawu osób zatwierdzających firmy**

5.  Administrator umożliwia samoobsługowe [Zarządzanie grupami](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management) , aby umożliwić użytkownikowi dołączenie do grupy, do której aplikacja jest przypisana **bez zgody firmy**

6.  Administrator umożliwia samoobsługowe [Zarządzanie grupami](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management) , aby umożliwić użytkownikowi dołączenie do grupy, do której przypisano aplikację, ale tylko **z poprzednią zgodą z wybranego zestawu osób zatwierdzających firmy** .

7.  Administrator przypisuje licencję do użytkownika bezpośrednio dla aplikacji pierwszej firmy, na przykład [Microsoft Office 365](https://products.office.com/)

8.  Administrator przypisuje licencję do grupy, do której należy użytkownik, do aplikacji pierwszej firmy, takiej jak [Microsoft Office 365](https://products.office.com/)

9.  [Administrator wyraża zgodę na użycie aplikacji](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview) przez wszystkich użytkowników, a następnie loguje się do aplikacji.

10. Użytkownik [wyraża zgodę na samą aplikację](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview) , logując się do aplikacji

## <a name="next-steps"></a>Następne kroki
[Zarządzanie aplikacjami za pomocą Azure Active Directory](what-is-application-management.md)
