---
title: Plik dyrektywy include
description: Plik dyrektywy include
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.date: 05/05/2020
ms.topic: include
ms.author: diberry
ms.openlocfilehash: da9388a3bd5f4d46ec34ed226e3ee23a96b2f494
ms.sourcegitcommit: 46f8457ccb224eb000799ec81ed5b3ea93a6f06f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/28/2020
ms.locfileid: "87375130"
---
## <a name="sign-in-to-luis-portal"></a>Zaloguj się do portalu LUIS

Nowy użytkownik do LUIS musi wykonać następującą procedurę:

1. Zaloguj się do [portalu Luis](https://www.luis.ai), wybierz swój kraj/region i zaakceptuj warunki użytkowania. Jeśli zamiast tego zobaczysz **Moje aplikacje** , zasób Luis już istnieje i należy przejść do sekcji Tworzenie aplikacji. W przypadku obsługiwanych regionów odwiedź witrynę [tworzenia i publikowania regionów oraz skojarzone klucze](https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions).

1. Wybierz pozycję **Utwórz zasób platformy Azure** , a następnie wybierz pozycję **Utwórz zasób autorstwa, aby przeprowadzić migrację aplikacji do programu.**

    ![Wybierz typ zasobu Language Understanding tworzenia](../media/luis-how-to-azure-subscription/sign-in-create-resource.png)

1. Wprowadź szczegółowe informacje o zasobie.

    ![Utwórz zasób tworzenia](../media/migrate-authoring-key/choose-authoring-resource-form.png)

    Podczas **tworzenia nowego zasobu tworzenia**należy podać następujące informacje:

    * **Nazwa zasobu** — wybrana przez Ciebie Nazwa niestandardowa, używana jako część adresu URL dla zapytań dotyczących tworzenia i przewidywania punktów końcowych.
    * **Dzierżawca** — dzierżawa, z którą skojarzona jest subskrypcja platformy Azure.
    * **Nazwa subskrypcji** — subskrypcja, za którą zostanie naliczona stawka dla zasobu.
    * **Grupa zasobów** — wybrana lub utworzona nazwa niestandardowej grupy zasobów. Grupy zasobów umożliwiają grupowanie zasobów platformy Azure w celu uzyskania dostępu i zarządzania.
    * **Lokalizacja** — wybór lokalizacji zależy od wybranej **grupy zasobów** .
    * **Warstwa cenowa** — warstwa cenowa określa maksymalną liczbę transakcji na sekundę i miesiąc.

1. Zostanie wyświetlone podsumowanie zasobu, który ma zostać utworzony. Wybierz pozycję **Dalej**.

    ![Utwórz zasób tworzenia](../media/sign-in/sign-in-confirm-key-selection.png)

1. Potwierdź, wybierając pozycję **Kontynuuj**.

    ![Utwórz zasób tworzenia](../media/sign-in/sign-in-confirm-continue.png)
