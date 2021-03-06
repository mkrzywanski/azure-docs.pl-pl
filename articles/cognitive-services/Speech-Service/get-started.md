---
title: Wypróbuj bezpłatnie usługę rozpoznawania mowy
titleSuffix: Azure Cognitive Services
description: Wprowadzenie do usługi mowy jest proste i niedrogie. Dostępne są bezpłatne opłaty za korzystanie z dwóch opcji, dzięki czemu możesz odkryć, co usługa może zrobić, i zdecydować, czy jest to odpowiednie dla Twoich potrzeb.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 04/03/2020
ms.author: erhopf
ms.custom: seodec18, seo-javascript-october2019
ms.openlocfilehash: 4604bfe7c815f79733f99a1a3727e4c68527e7ec
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/29/2020
ms.locfileid: "80656126"
---
# <a name="try-the-speech-service-for-free"></a>Wypróbuj bezpłatnie usługę mowy ""

W tym artykule można wybrać opcję łatwego testowania bezpłatnej usługi mowy, aby można było wykryć, co usługa może zrobić, i zdecydować, czy jest ona odpowiednia dla Twoich potrzeb. Wybierz jedną z dwóch następujących opcji, w zależności od sytuacji i przypadków użycia:

- [Opcja 1](#no-card): natychmiastowe pobieranie kluczy interfejsu API **bezpłatnej wersji próbnej** bez podawania żadnych informacji o karcie kredytowej (musisz mieć istniejące konto platformy Azure). **Bezpłatna wersja próbna** trwa 30 dni, a dane są usuwane na końcu. Ta opcja jest Najlepsza w przypadku szybkiego eksperymentu z usługą.
- [Opcja 2](#new-resource): Tworzenie nowego zasobu mowy na platformie Azure bez dodatkowych opłat przy użyciu **bezpłatnej subskrypcji** (wymagane są informacje o karcie kredytowej). **Bezpłatna subskrypcja** głównie ma bardziej rygorystyczne limity stawki niż płatna subskrypcja. Ta opcja jest przydatna, jeśli chcesz przetestować usługę, ale również planujesz uaktualnienie do płatnej subskrypcji w przyszłości i nie chcesz utracić danych.

## <a name="try-the-speech-service-without-credit-card-info"></a><a id="no-card"></a>Wypróbuj usługę mowy bez informacji o karcie kredytowej

Wykonaj następujące kroki, aby aktywować 30-dniową bezpłatną wersję próbną i pobrać klucze interfejsu API. Okres próbny rozpocznie się natychmiast po zakończeniu następujących kroków.

1. Przejdź do pozycji [Wypróbuj usługę poznawczej](https://azure.microsoft.com/try/cognitive-services/).
1. Wybierz kartę **interfejsy API rozpoznawania mowy** .
1. Wybierz pozycję **Pobierz klucz interfejsu API**.

Zostaną wyświetlone opcje rozliczeń. Wybierz opcję bezpłatna, a następnie przeczytaj i zatwierdź umowę użytkownika. Zostaną wyświetlone klucze, za pomocą których można wypróbować usługę mowy bezpłatnie przez 30 dni.

## <a name="try-the-speech-service-by-creating-an-azure-resource"></a><a id="new-resource"></a>Wypróbuj usługę mowy, tworząc zasób platformy Azure

W przypadku następujących kroków wymagane są zarówno konto Microsoft, jak i konto platformy Azure. Jeśli nie masz konto Microsoft, możesz skorzystać z jednej bezpłatnej rejestracji w [portalu konto Microsoft](https://account.microsoft.com/account). Wybierz pozycję **Zaloguj się przy użyciu konta Microsoft** a następnie, gdy zostanie wyświetlony monit o zalogowanie się, wybierz pozycję **Utwórz konto Microsoft**. Postępuj zgodnie z instrukcjami, aby utworzyć i zweryfikować nowe konto Microsoft.

Gdy masz konto Microsoft, przejdź do [strony tworzenia konta platformy Azure](https://azure.microsoft.com/free/ai/), wybierz pozycję **Rozpocznij bezpłatnie**i Utwórz nowe konto platformy Azure przy użyciu konto Microsoft.

> [!NOTE]
> Usługa mowy ma dwie warstwy usług: bezpłatna i subskrypcja, które mają różne ograniczenia i korzyści. Po zarejestrowaniu się w celu uzyskania bezpłatnego konta platformy Azure otrzymasz od $200 w wysokości, które można zastosować do płatnej subskrypcji usługi mowy, która jest ważna przez maksymalnie 30 dni.
>
> W przypadku korzystania z bezpłatnej warstwy usługi mowy o niskiej pojemności można zachować tę bezpłatną subskrypcję nawet po wygaśnięciu bezpłatnej wersji próbnej lub opłaty za usługę.
>
> Aby uzyskać więcej informacji, zobacz [Cognitive Services Cennik — Speech Service](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/).

### <a name="create-the-resource"></a>Tworzenie zasobu

Aby dodać zasób usługi mowy (warstwa Bezpłatna lub płatna) do konta platformy Azure:

1. Zaloguj się do [Azure Portal](https://portal.azure.com/) przy użyciu konto Microsoft.

1. Wybierz pozycję **Utwórz zasób** w lewym górnym rogu portalu. Jeśli nie widzisz opcji **Utwórz zasób**, zawsze możesz ją znaleźć, wybierając menu zwinięte w lewym górnym rogu:

   ![zwinięty przycisk nawigacji](media/index/collapsed-nav.png)

1. W **nowym** oknie wpisz "mowę" w polu wyszukiwania, a następnie naciśnij klawisz ENTER.

1. W wynikach wyszukiwania wybierz pozycję **Speech**.

   ![Wyniki wyszukiwania mowy](media/index/speech-search.png)

1. Wybierz pozycję **Utwórz**, a następnie:

   - Nadaj unikatowej nazwie nowemu zasobowi. Nazwa pomaga rozróżnić wiele subskrypcji powiązanych z tą samą usługą.
   - Wybierz subskrypcję platformy Azure, z którą jest skojarzony nowy zasób, aby określić, w jaki sposób opłaty są naliczane.
   - Wybierz [region](regions.md) , w którym będzie używany zasób.
   - Wybierz opcję bezpłatna (F0) lub płatna (S0). Aby uzyskać pełne informacje na temat cen i przydziałów użycia dla każdej warstwy, wybierz pozycję **Wyświetl pełne szczegóły cennika**.
   - Utwórz nową grupę zasobów dla tej subskrypcji mowy lub Przypisz subskrypcję do istniejącej grupy zasobów. Grupy zasobów ułatwiają zachowanie różnych subskrypcji platformy Azure.
   - Wybierz przycisk **Utwórz**. Spowoduje to przejście do omówienia wdrażania i wyświetlenie komunikatów o postępie wdrażania.

> [!NOTE]
> W jednym lub wielu regionach można utworzyć nieograniczoną liczbę subskrypcji w warstwie Standardowa. Można jednak utworzyć tylko jedną subskrypcję warstwy Bezpłatna. Wdrożenie modelu w warstwie Bezpłatna, które pozostanie nieużywane przez 7 dni, zostanie zlikwidowane automatycznie.

Wdrożenie nowego zasobu mowy trwa kilka minut. Po zakończeniu wdrażania wybierz pozycję **Przejdź do zasobu** , a następnie w okienku nawigacji po lewej stronie wybierz pozycję **klucze** , aby wyświetlić klucze subskrypcji usługi mowy. Każda subskrypcja ma dwa klucze: Możesz użyć dowolnego klawisza w aplikacji. Aby szybko skopiować/wkleić klucz do edytora kodu lub innej lokalizacji, wybierz przycisk Kopiuj obok każdego klucza, Przełącz system Windows, aby wkleić zawartość schowka do żądanej lokalizacji.

> [!IMPORTANT]
> Te klucze subskrypcji są używane do uzyskiwania dostępu do interfejsu API usługi poznawczej. Nie udostępniaj kluczy. Przechowuj je bezpiecznie — na przykład przy użyciu Azure Key Vault. Zalecamy także regularne ponowne generowanie tych kluczy. Tylko jeden klucz jest wymagany do wywołania interfejsu API. Po ponownym wygenerowaniu pierwszego klucza można użyć drugiego klawisza w celu uzyskania ciągłego dostępu do usługi.

## <a name="switch-to-a-new-subscription"></a>Przełącz do nowej subskrypcji

Aby przełączyć się z jednej subskrypcji na drugą, na przykład jeśli bezpłatna wersja próbna wygaśnie lub opublikowanie aplikacji zastąpi klucz regionu i subskrypcji w kodzie przy użyciu regionu i klucza subskrypcji nowego zasobu platformy Azure.

## <a name="about-regions"></a>Regiony — informacje

- Jeśli aplikacja korzysta z [zestawu Speech SDK](speech-sdk.md), podczas tworzenia konfiguracji mowy należy podać kod regionu `westus`, taki jak.
- Jeśli aplikacja używa jednego z [interfejsów API REST](rest-apis.md)usługi mowy, region jest częścią identyfikatora URI punktu końcowego, który jest używany podczas tworzenia żądań.
- Klucze utworzone dla regionu są prawidłowe tylko w tym regionie. Próba użycia ich z innymi regionami spowoduje błędy uwierzytelniania.

## <a name="next-steps"></a>Następne kroki

Wykonaj jedną z naszych 10-minutowych przewodników szybki start lub zapoznaj się z naszymi przykładami zestawu SDK:

> [!div class="nextstepaction"]
> [Szybki Start: Rozpoznawanie mowy w](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-csharp&tabs=dotnet)
> [przykładach zestawu Speech SDK](speech-sdk.md#sample-source-code) języka C#
