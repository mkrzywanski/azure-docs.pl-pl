---
title: 'Szybki Start: Tworzenie pierwszej statycznej aplikacji sieci Web przy użyciu statycznej Web Apps platformy Azure'
description: Dowiedz się, jak utworzyć wystąpienie usługi Azure static Web Apps przy użyciu preferowanej platformy frontonu.
services: static-web-apps
author: craigshoemaker
ms.service: static-web-apps
ms.topic: quickstart
ms.date: 05/08/2020
ms.author: cshoe
ms.openlocfilehash: 6738f598275e91ce8a811c3ef6bcc6d5dc84e0bd
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/23/2020
ms.locfileid: "87089502"
---
# <a name="quickstart-building-your-first-static-web-app"></a>Szybki Start: Tworzenie pierwszej statycznej aplikacji sieci Web

Usługa Azure Static Web Apps publikuje witryny internetowe w środowisku produkcyjnym, kompilując w tym celu aplikacje z repozytorium GitHub. W tym przewodniku szybki start utworzysz aplikację sieci Web przy użyciu preferowanej platformy frontonu w repozytorium GitHub.

Jeśli nie masz subskrypcji platformy Azure, [Utwórz konto bezpłatnej wersji próbnej](https://azure.microsoft.com/free).

## <a name="prerequisites"></a>Wymagania wstępne

- Konto usługi [GitHub](https://github.com)
- Konto [platformy Azure](https://portal.azure.com)

## <a name="create-a-repository"></a>Tworzenie repozytorium

W tym artykule są wykorzystywane repozytoria szablonów usługi GitHub w celu ułatwienia tworzenia nowego repozytorium. Funkcja szablonów Starter Aplikacje skompilowane z różnymi platformami frontonu.

# <a name="angular"></a>[Angular](#tab/angular)

- Upewnij się, że zalogowano się do usługi GitHub, a następnie przejdź do następującej lokalizacji, aby utworzyć nowe repozytorium
  - https://github.com/staticwebdev/angular-basic/generate
- Nadaj nazwę repozytorium **My-static-Web-App**

# <a name="react"></a>[React](#tab/react)

- Upewnij się, że zalogowano się do usługi GitHub, a następnie przejdź do następującej lokalizacji, aby utworzyć nowe repozytorium
  - https://github.com/staticwebdev/react-basic/generate
- Nadaj nazwę repozytorium **My-static-Web-App**

# <a name="vue"></a>[VUE](#tab/vue)

- Upewnij się, że zalogowano się do usługi GitHub, a następnie przejdź do następującej lokalizacji, aby utworzyć nowe repozytorium
  - https://github.com/staticwebdev/vue-basic/generate
- Nadaj nazwę repozytorium **My-static-Web-App**

# <a name="no-framework"></a>[Brak struktury](#tab/vanilla-javascript)

- Upewnij się, że zalogowano się do usługi GitHub, a następnie przejdź do następującej lokalizacji, aby utworzyć nowe repozytorium
  - https://github.com/staticwebdev/vanilla-basic/generate
- Nadaj nazwę repozytorium **My-static-Web-App**

> [!NOTE]
> Statyczne Web Apps platformy Azure wymaga co najmniej jednego pliku HTML do utworzenia aplikacji sieci Web. Repozytorium utworzone w tym kroku zawiera jeden plik _index.html_ .

---

Kliknij przycisk **Utwórz repozytorium na podstawie szablonu** .

:::image type="content" source="media/getting-started/create-template.png" alt-text="Utwórz repozytorium na podstawie szablonu":::

## <a name="create-a-static-web-app"></a>Tworzenie statycznej aplikacji sieci Web

Po utworzeniu repozytorium można utworzyć statyczną aplikację sieci Web na podstawie Azure Portal.

- Przejdź do [Azure Portal](https://portal.azure.com)
- Kliknij pozycję **Utwórz zasób**
- Wyszukaj usługę **Static Web Apps**
- Kliknij pozycję **Static Web Apps (wersja zapoznawcza)**
- Kliknij pozycję **Utwórz**

### <a name="basics"></a>Podstawy

Zacznij od skonfigurowania nowej aplikacji i powiązania jej z repozytorium GitHub.

:::image type="content" source="media/getting-started/basics-tab.png" alt-text="Karta Podstawowe":::

- Wybierz swoją _subskrypcję platformy Azure_
- Wybierz lub Utwórz nową _grupę zasobów_
- Nadaj aplikacji nazwę **My-static-Web-App**.
  - Prawidłowe znaki to `a-z` (bez uwzględniania wielkości liter), `0-9`i `-`.
- Wybierz _region_ znajdujący się najbliżej siebie
- Wybierz **bezpłatną** _jednostkę SKU_
- Kliknij przycisk **Zaloguj się przy użyciu usługi GitHub** i uwierzytelnij się przy użyciu usługi GitHub

Po zalogowaniu się za pomocą usługi GitHub wprowadź informacje o repozytorium.

:::image type="content" source="media/getting-started/repository-details.png" alt-text="Szczegóły repozytorium":::

- Wybierz preferowaną _organizację_
- Wybierz pozycję **moja-First-Web-static-App** z listy rozwijanej _repozytorium_
- Wybierz pozycję **główna** z listy rozwijanej _rozgałęzienie_
- Kliknij przycisk **Dalej: Skompiluj >**, aby edytować konfigurację kompilacji

:::image type="content" source="media/getting-started/next-build-button.png" alt-text="Przycisk Następna kompilacja":::

> [!NOTE]
>  Jeśli nie widzisz żadnych repozytoriów, może być konieczne autoryzowanie Web Apps statycznej platformy Azure w usłudze GitHub. Przejdź do repozytorium GitHub i przejdź do pozycji **ustawienia > aplikacje > autoryzowane aplikacje OAuth**, wybierz pozycję **statyczne Web Apps platformy Azure**, a następnie wybierz pozycję **Udziel**. W przypadku repozytoriów organizacji musisz być właścicielem organizacji, aby przyznać uprawnienia.

### <a name="build"></a>Kompilacja

Następnie dodaj szczegóły konfiguracji specyficzne dla preferowanej struktury frontonu.

# <a name="angular"></a>[Angular](#tab/angular)

- Wprowadź **/** w polu _Lokalizacja aplikacji_
- Wyczyść wartość domyślną w polu _Lokalizacja interfejsu API_
- Wprowadź wartość **ROZKŁ/kątowy — podstawowa** w polu _Lokalizacja artefaktu aplikacji_

# <a name="react"></a>[React](#tab/react)

- Wprowadź **/** w polu _Lokalizacja aplikacji_
- Wyczyść wartość domyślną w polu _Lokalizacja interfejsu API_
- Wprowadź **kompilację** w polu _Lokalizacja artefaktu aplikacji_

# <a name="vue"></a>[VUE](#tab/vue)

- Wprowadź **/** w polu _Lokalizacja aplikacji_
- Wyczyść wartość domyślną w polu _Lokalizacja interfejsu API_
- Wprowadź wartość **ROZKŁ** w polu _Lokalizacja artefaktu aplikacji_

# <a name="no-framework"></a>[Brak struktury](#tab/vanilla-javascript)

- Wprowadź **/** w polu _Lokalizacja aplikacji_
- Wyczyść wartość domyślną w polu _Lokalizacja interfejsu API_
- Wyczyść wartość domyślną w polu _Lokalizacja artefaktu aplikacji_

---

Kliknij przycisk **Przejrzyj i utwórz**.

:::image type="content" source="media/getting-started/review-create.png" alt-text="Przycisk tworzenia przeglądu":::

Aby zmienić te wartości po utworzeniu aplikacji, można edytować [plik przepływu pracy](github-actions-workflow.md).

### <a name="review--create"></a>Przeglądanie i tworzenie

Po zweryfikowaniu żądania można nadal utworzyć aplikację.

Kliknij przycisk **Utwórz**

:::image type="content" source="media/getting-started/create-button.png" alt-text="Przycisk Utwórz":::

Po utworzeniu zasobu kliknij przycisk **Przejdź do zasobu**

:::image type="content" source="media/getting-started/resource-button.png" alt-text="Przycisk Przejdź do zasobu":::

## <a name="view-the-website"></a>Wyświetlanie witryny sieci Web

Istnieją dwa aspekty wdrażania aplikacji statycznej. Pierwszy ustanawia podstawowe zasoby platformy Azure tworzące aplikację. Drugi to przepływ pracy funkcji GitHub Actions, który kompiluje i publikuje aplikację.

Aby można było przejść do nowej lokacji statycznej, kompilacja wdrożenia musi być najpierw zakończona.

W oknie przeglądu Web Apps statycznego zostanie wyświetlona seria linków, które pomagają w współpracy z aplikacją sieci Web.

:::image type="content" source="media/getting-started/overview-window.png" alt-text="Okno przegląd":::

1. Kliknięcie transparentu "kliknij tutaj", aby sprawdzić stan uruchomionych akcji usługi GitHub, spowoduje wykonanie akcji GitHub względem repozytorium. Po sprawdzeniu, czy zadanie wdrożenia zostało zakończone, możesz przejść do witryny sieci Web za pośrednictwem wygenerowanego adresu URL.

2. Po zakończeniu przepływu pracy akcji usługi GitHub można kliknąć link _adresu URL_ , aby otworzyć witrynę sieci Web na nowej karcie.

## <a name="clean-up-resources"></a>Czyszczenie zasobów

Jeśli nie chcesz nadal korzystać z tej aplikacji, możesz usunąć wystąpienie usługi Azure static Web Apps, wykonując następujące czynności:

1. Otwórz witrynę [Azure Portal](https://portal.azure.com).
1. Wyszukaj **aplikację My-First-Web-static-** from na górnym pasku wyszukiwania
1. Kliknij nazwę aplikacji
1. Kliknij przycisk **Usuń**
1. Kliknij przycisk **tak** , aby potwierdzić akcję usuwania

## <a name="next-steps"></a>Następne kroki

> [!div class="nextstepaction"]
> [Dodawanie interfejsu API](add-api.md)
