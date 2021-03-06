---
title: Wystąpił problem podczas dodawania aplikacji w galerii usługi Azure AD | Microsoft Docs
description: Poznaj często spotykane problemy podczas dodawania aplikacji w galerii usługi Azure AD i możliwości ich rozwiązywania
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
ms.date: 09/11/2018
ms.author: kenwith
ms.collection: M365-identity-device-management
ms.openlocfilehash: e4387a3ca02297311cd9ab025c315ca3324cbbbe
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "84760409"
---
# <a name="problem-adding-an-azure-ad-gallery-application"></a>Wystąpił problem podczas dodawania aplikacji w galerii usługi Azure AD

Ten artykuł ułatwia zapoznanie się z typowymi problemami występującymi podczas dodawania aplikacji w galerii usługi Azure AD oraz możliwości ich rozwiązania.

## <a name="i-clicked-the-add-button-and-my-application-took-a-long-time-to-appear"></a>Po kliknięciu przycisku "Dodaj", gdy aplikacja trwała długo

W pewnych okolicznościach może upłynąć nawet 1-2 minut (i czasami dłużej), aby aplikacja była wyświetlana po dodaniu jej do katalogu. Chociaż nie jest to normalne oczekiwana wydajność, można zobaczyć, że dodawanie aplikacji jest w toku, klikając ikonę **powiadomienia** (dzwonka) w prawym górnym rogu [Azure Portal](https://portal.azure.com/) i szukając **w toku** lub **ukończonego** powiadomienia z etykietą **Dodawanie aplikacji.**

Jeśli aplikacja nie jest nigdy dodawana lub wystąpi błąd podczas klikania przycisku **Dodaj** , zobaczysz **powiadomienie** w stanie **błędu** . Jeśli potrzebujesz więcej szczegółowych informacji o błędzie, aby dowiedzieć się więcej lub udostępnić inżynierowi pomocy technicznej, możesz zobaczyć więcej informacji o błędzie, wykonując czynności opisane w sekcji " [jak wyświetlić szczegóły powiadomienia portalu](#how-to-see-the-details-of-a-portal-notification) ".

## <a name="i-clicked-the-add-button-and-my-application-didnt-appear"></a>Kliknięto przycisk "Dodaj", a moja aplikacja nie została wyświetlona

Czasami, ze względu na przejściowe problemy, problemy z siecią lub usterkę, dodawanie aplikacji kończy się niepowodzeniem. Możesz to powiedzieć, gdy klikniesz ikonę **powiadomienia** (dzwonka) w prawym górnym rogu Azure Portal i zobaczysz ikonę czerwoną (!) obok powiadomienia o **dodawaniu aplikacji** . Oznacza to, że wystąpił błąd podczas tworzenia aplikacji.

Jeśli wystąpi błąd podczas klikania przycisku **Dodaj** , zobaczysz **powiadomienie** w stanie **błędu** . Jeśli potrzebujesz więcej szczegółowych informacji o błędzie, aby dowiedzieć się więcej lub udostępnić inżynierowi pomocy technicznej, możesz zobaczyć więcej informacji o błędzie, wykonując czynności opisane w sekcji " [jak wyświetlić szczegóły powiadomienia portalu](#how-to-see-the-details-of-a-portal-notification) ".

## <a name="i-dont-know-how-to-set-up-my-application-once-ive-added-it"></a>Nie wiem, jak skonfigurować aplikację po dodaniu

Jeśli potrzebujesz pomocy dotyczącej uczenia się dotyczącej aplikacji, warto zacząć od [listy samouczków dotyczących integrowania aplikacji SaaS z](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) artykułem Azure Active Directory.

Oprócz tego [Biblioteka dokumentów aplikacji usługi Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-apps-index) pomaga dowiedzieć się więcej na temat logowania jednokrotnego w usłudze Azure AD oraz o sposobie jego działania.

## <a name="how-to-see-the-details-of-a-portal-notification"></a>Jak wyświetlić szczegóły powiadomienia portalu

Szczegóły dowolnego powiadomienia portalu można wyświetlić, wykonując poniższe kroki:

1.  Wybierz ikonę **powiadomienia** (dzwonka) w prawym górnym rogu Azure Portal

2.  Wybierz dowolne powiadomienie w stanie **błędu** (te z czerwonym (!) obok nich).

    >[!NOTE]
    >Nie można klikać powiadomień w stanie " **powodzenie** " lub " **w toku** ".
    >
    >

4.  Skorzystaj z informacji w obszarze **szczegóły powiadomienia** , aby poznać więcej szczegółów o problemie.

5.  Jeśli nadal potrzebujesz pomocy, możesz również udostępnić te informacje inżynierowi pomocy technicznej lub grupie produktów, aby uzyskać pomoc dotyczącą problemu.

6.  Kliknij ikonę **kopiowania** **icon** znajdującą się po prawej stronie pola tekstowego **błąd kopiowania** , aby skopiować wszystkie szczegóły powiadomienia, które mają być współużytkowane przez inżyniera pomocy technicznej lub grupy produktów.

## <a name="how-to-get-help-by-sending-notification-details-to-a-support-engineer"></a>Jak uzyskać pomoc, wysyłając szczegóły powiadomień do inżyniera pomocy technicznej

Bardzo ważne jest, aby udostępnić **wszystkie szczegóły wymienione poniżej** z inżynierem pomocy technicznej, jeśli potrzebujesz pomocy, dzięki czemu mogą one pomóc szybko. Można to zrobić łatwo, pobierając **zrzut ekranu** lub klikając **ikonę kopiowania błędu**, która znajduje się po prawej stronie pola tekstowego **błąd kopiowania** .

## <a name="notification-details-explained"></a>Wyjaśniono szczegóły powiadomień

Więcej informacji o powiadomieniach można znaleźć w następujących opisach.

### <a name="essential-notification-items"></a>Podstawowe elementy powiadomień

- **Title** — opisowy tytuł powiadomienia

  * Przykład — **Ustawienia serwera proxy aplikacji**

- **Opis** — opis tego, co wystąpił w wyniku operacji

  -   Przykład — **wprowadzony wewnętrzny adres URL jest już używany przez inną aplikację**

- **Identyfikator powiadomienia** — unikatowy identyfikator powiadomienia

  -   Przykład — **clientNotification-2adbfc06-2073-4678-A69F-7eb78d96b068**

- **Identyfikator żądania klienta** — konkretny identyfikator żądania wprowadzony przez przeglądarkę

  -   Przykład — **302fd775-3329-4670-a9f3-bea37004f0bc**

- **Sygnatura czasowa UTC** — znacznik czasu, w którym wystąpiło powiadomienie, w formacie UTC

  -   Przykład – **2017-03-23T19:50:43.7583681 z**

- **Wewnętrzny identyfikator transakcji** — identyfikator wewnętrzny, którego można użyć do wyszukania błędu w naszych systemach

  -   Przykład — **71a2f329-ca29-402f-aa72-bc00a7aca603**

- **UPN** — użytkownik, który wykonał operację

  -   Przykład — **tperkins \@ f128.info**

- **Identyfikator dzierżawy** — unikatowy identyfikator dzierżawy, której użytkownik wykonał operację, był członkiem

  -   Przykład — **7918d4b5-0442-4a97-be2d-36f9f9962ece**

- **Identyfikator obiektu użytkownika** — unikatowy identyfikator użytkownika, który wykonał operację

  -   Przykład — **17f84be4-51f8-483a-b533-383791227a99**

### <a name="detailed-notification-items"></a>Szczegółowe elementy powiadomień

-   **Nazwa wyświetlana** — **(może być pusta)** bardziej szczegółową nazwę wyświetlaną dla błędu

    -   Przykład — **Ustawienia serwera proxy aplikacji**

-   **Status** — określony stan powiadomienia

    -   Przykład — **Niepowodzenie**

-   **Identyfikator obiektu** — **(może być pusty)** identyfikator obiektu, dla którego wykonano operację

    -   Przykład — **8e08161d-f2fd-40ad-a34a-a9632d6bb599**

-   **Szczegóły** — szczegółowy opis tego, co się stało w wyniku operacji

    -   Przykład — **wewnętrzny adres URL `https://bing.com/` jest nieprawidłowy, ponieważ jest już używany**

-   **Błąd kopiowania** — kliknij **ikonę kopiowania** z prawej strony pola tekstowego **błąd kopiowania** , aby skopiować wszystkie szczegóły powiadomienia, które mają być współużytkowane z obsługą lub grupą produktów. 
-   inżynier ds

    -   Przyklad```{"errorCode":"InternalUrl\_Duplicate","localizedErrorDetails":{"errorDetail":"Internal url 'https://google.com/' is invalid since it is already in use"},"operationResults":\[{"objectId":null,"displayName":null,"status":0,"details":"Internal url 'https://bing.com/' is invalid since it is already in use"}\],"timeStampUtc":"2017-03-23T19:50:26.465743Z","clientRequestId":"302fd775-3329-4670-a9f3-bea37004f0bb","internalTransactionId":"ea5b5475-03b9-4f08-8e95-bbb11289ab65","upn":"tperkins@f128.info","tenantId":"7918d4b5-0442-4a97-be2d-36f9f9962ece","userObjectId":"17f84be4-51f8-483a-b533-383791227a99"}```

