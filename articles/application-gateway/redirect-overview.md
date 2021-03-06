---
title: Omówienie przekierowania dla usługi Azure Application Gateway
description: Dowiedz się więcej o możliwości przekierowania w usłudze Azure Application Gateway, aby przekierować ruch odbierany przez jeden odbiornik do innego odbiornika lub do zewnętrznej lokacji.
services: application-gateway
author: amsriva
ms.service: application-gateway
ms.topic: conceptual
ms.date: 11/16/2019
ms.author: amsriva
ms.openlocfilehash: 8c2bac1bee5a2301a29589f15d72d0f69b4a05b4
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "85253855"
---
# <a name="application-gateway-redirect-overview"></a>Omówienie przekierowania Application Gateway

W celu przekierowania ruchu można użyć bramy aplikacji.  Ma ona mechanizm ogólnego przekierowywania, który umożliwia przekierowywanie ruchu odbieranego przez jeden odbiornik do innego odbiornika lub do witryny zewnętrznej. Upraszcza to konfigurację aplikacji, optymalizuje użycie zasobów i obsługuje nowe scenariusze przekierowania, w tym globalne i oparte na ścieżkach przekierowywanie.

Typowy scenariusz przekierowania dla wielu aplikacji sieci Web to obsługa automatycznego przekierowania HTTP do HTTPS w celu zapewnienia, że cała komunikacja między aplikacją i jej użytkownikami odbywa się na zaszyfrowanej ścieżce. W przeszłości klienci korzystali z technik takich jak tworzenie dedykowanej puli zaplecza, której jedynym celem jest przekierowanie żądań odbieranych przez protokół HTTP do protokołu HTTPS. Dzięki obsłudze przekierowania w Application Gateway można to zrobić po prostu dodając nową konfigurację przekierowania do reguły routingu i określając inny odbiornik z protokołem HTTPS jako odbiornik docelowy.

Obsługiwane są następujące typy przekierowań:

- 301 trwałe przekierowanie
- 302 znaleziono
- 303 Zobacz inne
- 307 tymczasowe przekierowanie

Obsługa przekierowania dla usługi Application Gateway oferuje następujące możliwości:

-  **Przekierowywanie globalne**

   Przekierowuje z jednego odbiornika do innego odbiornika w bramie. Umożliwia to przekierowanie protokołu HTTP do HTTPS w witrynie.
- **Przekierowanie oparte na ścieżce**

   Ten typ przekierowania umożliwia przekierowywanie protokołu HTTP do protokołu HTTPS tylko w określonym obszarze witryny, na przykład w przypadku obszaru koszyka notowanego przez/Cart/*.
- **Przekieruj do zewnętrznej lokacji**

![przekierowania](./media/redirect-overview/redirect.png)

W przypadku tej zmiany klienci muszą utworzyć nowy obiekt konfiguracji przekierowania, który określa docelowy odbiornik lub zewnętrzną lokację, do której jest wymagane przekierowanie. Element konfiguracji obsługuje również opcje umożliwiające dołączenie ścieżki URI i ciągu zapytania do przekierowanego adresu URL. Możesz również wybrać typ przekierowania. Po utworzeniu ta konfiguracja przekierowania jest dołączona do odbiornika źródłowego za pośrednictwem nowej reguły. W przypadku korzystania z reguły podstawowej konfiguracja przekierowania jest skojarzona z odbiornikiem źródła i jest globalnym przekierowaniem. Gdy używana jest reguła oparta na ścieżce, konfiguracja przekierowania jest definiowana na mapie ścieżki URL. W związku z tym dotyczy tylko określonego obszaru ścieżki w lokacji.

### <a name="next-steps"></a>Następne kroki

[Konfigurowanie przekierowywania adresów URL na bramie aplikacji](tutorial-url-redirect-powershell.md)
