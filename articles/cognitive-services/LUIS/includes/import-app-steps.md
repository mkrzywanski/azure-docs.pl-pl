---
title: Importuj kroki aplikacji
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 06/22/2020
ms.author: diberry
ms.openlocfilehash: 37f1b85b4ce8510d5e288df985a55dba659f0c9b
ms.sourcegitcommit: 0100d26b1cac3e55016724c30d59408ee052a9ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/07/2020
ms.locfileid: "86035676"
---
1. W [portalu Luis](https://www.luis.ai)na stronie **Moje aplikacje** wybierz pozycję **+ Nowa aplikacja do konwersacji**, a następnie **zaimportuj jako plik JSON**. Znajdź zapisany plik JSON z poprzedniego kroku. Nie musisz zmieniać nazwy aplikacji. Wybierz pozycję **gotowe**

1. W sekcji **Zarządzanie** na karcie **wersje** wybierz `0.1` wersję, a następnie wybierz pozycję **Klonuj** , aby sklonować wersję, i nadaj jej nową nazwę `ml-entity` , a następnie wybierz pozycję **gotowe** , aby zakończyć proces klonowania. Ponieważ nazwa wersji jest używana jako część trasy adresu URL, nie może ona zawierać żadnych znaków, które są nieprawidłowe w adresie URL.

    > [!TIP]
    > Klonowanie do nowej wersji jest najlepszym rozwiązaniem Przed zmodyfikowaniem aplikacji. Po zakończeniu zmiany wersji wyeksportuj wersję (plik JSON lub Lu) i sprawdź plik w systemie kontroli źródła.

1. Wybierz **Build** opcję Kompiluj **, a następnie zapoznaj się z** intencjami, głównymi blokami konstrukcyjnymi aplikacji Luis.

    ![Przejdź do strony wersje na stronie intencje.](../media/tutorial-machine-learned-entity/new-version-imported-app.png)
