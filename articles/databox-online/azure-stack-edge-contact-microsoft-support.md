---
title: Rejestracja biletu pomocy technicznej dla Azure Stack Edge Azure Data Box Gateway | Microsoft Docs
description: Dowiedz się, jak rejestrować żądania pomocy technicznej dotyczące problemów związanych z Azure Stack krawędzią lub zamówieniami Data Box Gateway.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 07/11/2019
ms.author: alkohli
ms.openlocfilehash: 4d513471e288c1aadbf70b24ef367965a0b69a80
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "84339913"
---
# <a name="open-a-support-ticket-for-azure-stack-edge-and-azure-data-box-gateway"></a>Otwórz bilet pomocy technicznej dla Azure Stack Edge i Azure Data Box Gateway

Ten artykuł ma zastosowanie do Azure Stack Edge i Azure Data Box Gateway obu tych, które są zarządzane przez usługę Azure Stack Edge/Azure Data Box Gateway. Jeśli wystąpią problemy z usługą, możesz utworzyć żądanie obsługi dla pomocy technicznej. W tym artykule przedstawiono następujące instrukcje:

* Jak utworzyć żądanie obsługi.
* Jak zarządzać cyklem życia żądania pomocy technicznej z poziomu portalu.

## <a name="create-a-support-request"></a>Tworzenie żądania obsługi

Wykonaj następujące kroki, aby utworzyć żądanie obsługi:

1. Przejdź do Azure Stack krawędź lub Data Box Gateway. Przejdź do sekcji **Obsługa i rozwiązywanie problemów** , a następnie wybierz pozycję **nowe żądanie obsługi**.

2. W obszarze **nowe żądanie obsługi**na karcie **podstawowe** wykonaj następujące czynności:

    1. Z listy rozwijanej **typ problemu** wybierz pozycję **techniczne**.
    2. Wybierz **subskrypcję**.
    3. W obszarze **Usługa**Sprawdź pozycję **Moje usługi**. Z listy rozwijanej wybierz pozycję **Azure Stack krawędź i Data Box Gateway**.
    4. Wybierz **zasób**. Odnosi się to do nazwy zamówienia.
    5. Podaj krótkie **Podsumowanie** problemu, który wystąpił. 
    6. Wybierz **typ problemu**.
    7. Na podstawie wybranego typu problemu wybierz odpowiedni **podtyp problemu**.
    8. Wybierz pozycję **Dalej: rozwiązania >>**.

        ![Informacje podstawowe](./media/azure-stack-edge-contact-microsoft-support/data-box-edge-support-request-1.png)

3. Na karcie **szczegóły** wykonaj następujące czynności:

    1. Podaj datę i godzinę rozpoczęcia problemu.
    2. Podaj **Opis** problemu.
    3. W polu **przekazywanie pliku**wybierz ikonę folderu, aby przeglądać wszystkie inne pliki, które chcesz przekazać.
    4. Sprawdź **udostępnianie informacji diagnostycznych**.
    5. W oparciu o twoją subskrypcję jest automatycznie wypełniany **Plan pomocy technicznej** .
    6. Z listy rozwijanej wybierz **ważność**.
    7. Określ **preferowaną metodę kontaktu**.
    8. **Godziny odpowiedzi** są automatycznie wybierane na podstawie Twojego planu subskrypcji.
    9. Określ język, który ma być obsługiwany.
    10. W **informacjach kontaktowych**Podaj swoją nazwę, adres e-mail, numer telefonu, kontakt opcjonalny, kraj/region. Pomoc techniczna firmy Microsoft korzysta z tych informacji w celu uzyskania dalszych informacji, diagnozowania i rozwiązywania problemów. 
    11. Wybierz kolejno pozycje **Dalej: recenzja + utwórz >>**.

        ![Problem](./media/azure-stack-edge-contact-microsoft-support/data-box-edge-support-request-2.png)

4. Na karcie **Przegląd i tworzenie** zapoznaj się z informacjami dotyczącymi biletu pomocy technicznej. Wybierz pozycję **Utwórz**. 

    ![Problem](./media/azure-stack-edge-contact-microsoft-support/data-box-edge-support-request-3.png)

    Po utworzeniu biletu pomocy technicznej skontaktuje się z Tobą, gdy tylko będzie to możliwe, aby kontynuować żądanie.

## <a name="get-hardware-support"></a>Uzyskaj pomoc techniczną

Te informacje dotyczą tylko Azure Stack urządzenia. Proces zgłaszania problemów ze sprzętem jest następujący:

1. Aby uzyskać problem ze sprzętem, Otwórz bilet pomocy technicznej z Azure Portal. W obszarze **typ problemu**wybierz pozycję **Azure Stack sprzęt**. Wybierz **podtyp problemu** jako **awaria sprzętu**.

    ![Problem ze sprzętem](./media/azure-stack-edge-contact-microsoft-support/data-box-edge-hardware-issue-1.png)

    Po utworzeniu biletu pomocy technicznej skontaktuje się z Tobą, gdy tylko będzie to możliwe, aby kontynuować żądanie.

2. Jeśli pomoc techniczna firmy Microsoft określa, że jest to problem sprzętowy, wykonywana jest jedna z następujących akcji:

    * Wysłano jednostkę wymiany pola (FRU) dla uszkodzonej części sprzętowej. Obecnie jedynymi obsługiwanymi FRUs są jednostki zasilacza i dyski twarde.
    * Tylko FRUs są zastępowane w następnym dniu roboczym. wszystko inne wymaga, aby wszystkie inne elementy wymagały pełnego zastąpienia systemu (FRS).

3. Jeśli bilet pomocy technicznej zostanie zgłoszony przed upływem 4:30 czasu lokalnego (od poniedziałku do piątku), technika Onsite zostanie wysłany do lokalizacji następnego dnia roboczego w celu przeprowadzenia jednostki FRU lub pełnego zastępowania urządzeń.

## <a name="manage-a-support-request"></a>Zarządzanie żądaniem obsługi

Po utworzeniu biletu pomocy technicznej możesz zarządzać jego cyklem życia z poziomu portalu.

### <a name="to-manage-your-support-requests"></a>Aby zarządzać żądaniami pomocy technicznej

1. Aby przejść do strony pomoc i obsługa techniczna, przejdź do okna **przeglądaj > pomoc i obsługa techniczna**.

    ![Zarządzanie żądaniami obsługi](./media/azure-stack-edge-contact-microsoft-support/data-box-edge-manage-support-request-1.png)

2. W **oknie Pomoc i obsługa techniczna**jest wyświetlana tabelaryczna lista **najnowszych żądań obsługi** .

    <!--[Manage support requests](./media/azure-stack-edge-contact-microsoft-support/data-box-edge-support-request-1.png)--> 

3. Wybierz i kliknij żądanie pomocy technicznej. Możesz wyświetlić stan i szczegóły tego żądania. Kliknij pozycję **+ Nowy komunikat** , jeśli chcesz kontynuować na tym żądaniu.

## <a name="next-steps"></a>Następne kroki

Dowiedz się, jak [rozwiązywać problemy związane z Azure Stack Edge](azure-stack-edge-troubleshoot.md).
Dowiedz się, jak [rozwiązywać problemy związane z Data Box Gateway](data-box-gateway-troubleshoot.md).
