---
title: Wyświetl metryki użycia interfejsu API Azure Maps | Mapy Microsoft Azure
description: W tym artykule dowiesz się, jak wyświetlać metryki dla wywołań interfejsu API Maps Microsoft Azure w Azure Portal.
author: anastasia-ms
ms.author: v-stharr
ms.date: 08/06/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 57fb699a7225a53af1f093520855e8e2ef132f42
ms.sourcegitcommit: 0e8a4671aa3f5a9a54231fea48bcfb432a1e528c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/24/2020
ms.locfileid: "87130736"
---
# <a name="view-azure-maps-api-usage-metrics"></a>Wyświetlanie metryk użycia interfejsu API usługi Azure Maps

W tym artykule pokazano, jak wyświetlić metryki użycia interfejsu API dla konta Azure Maps w [Azure Portal](https://portal.azure.com). Metryki są wyświetlane w wygodnym formacie grafu, co umożliwia dostosowanie czasu trwania.

## <a name="view-metric-snapshot"></a>Wyświetl migawkę metryki

Niektóre typowe metryki można zobaczyć na stronie **Przegląd** konta Maps. Obecnie pokazuje *łączne żądania*, *łączne błędy*i *dostępność* w czasie trwania możliwego do wyboru czasu.

![Przegląd metryk użycia Azure Maps](media/how-to-view-api-usage/portal-overview.png)

Przejdź do następnej sekcji, jeśli chcesz dostosować te wykresy do konkretnej analizy.

## <a name="view-detailed-metrics"></a>Wyświetl szczegółowe metryki

1. Zaloguj się do subskrypcji platformy Azure w [portalu](https://portal.azure.com).

2. Kliknij element menu **wszystkie zasoby** po lewej stronie i przejdź do *konta Azure Maps*.

3. Po otwarciu konta Maps kliknij menu **metryki** po lewej stronie.

4. W okienku **metryki** wybierz jedną z następujących opcji:

   1. **Dostępność** — który pokazuje *ŚREDNIĄ* dostępność interfejsu API w danym okresie czasu.
   2. **Użycie** — pokazuje, jak *licznik* użycia dla Twojego konta.

      ![Okienko metryk użycia Azure Maps](media/how-to-view-api-usage/portal-metrics.png)

5. Następnie możesz wybrać *zakres czasu* , klikając **ostatnie 24 godziny (automatyczne)**. Domyślnie zakres czasu jest ustawiany na 24 godziny. Po kliknięciu zobaczysz wszystkie wybrane zakresy czasu. Możesz wybrać *stopień szczegółowości czasu* i wybrać opcję wyświetlania czasu jako *lokalnego* lub *GMT* w tej samej liście rozwijanej. Kliknij pozycję **Zastosuj**.

    ![Zakres czasu metryk Azure Maps](media/how-to-view-api-usage/time-range.png)

6. Po dodaniu metryki możesz **dodać filtr** z właściwości odpowiednich dla tej metryki. Następnie wybierz wartość właściwości, która ma być widoczna na grafie.

    ![Filtr metryk użycia Azure Maps](media/how-to-view-api-usage/filter.png)

7. Możesz również **zastosować dzielenie** dla metryki na podstawie wybranej właściwości metryki. Umożliwia to dzielenie wykresu na wiele wykresów, dla każdej wartości tej właściwości. Na poniższej ilustracji kolor każdego wykresu odpowiada wartości właściwości widocznej w dolnej części wykresu.

    ![Dzielenie metryk użycia Azure Maps](media/how-to-view-api-usage/splitting.png)

8. Możesz również obserwować wiele metryk na tym samym wykresie. Wystarczy kliknąć przycisk **Dodaj metrykę** na górze.

## <a name="next-steps"></a>Następne kroki

Dowiedz się więcej na temat Azure Maps interfejsów API, dla których chcesz śledzić użycie:
> [!div class="nextstepaction"] 
> [Azure Maps zestawu SDK sieci Web](how-to-use-map-control.md)

> [!div class="nextstepaction"] 
> [Azure Maps Android SDK](how-to-use-android-map-control-library.md)

> [!div class="nextstepaction"]
> [Dokumentacja interfejsu API REST Azure Maps](https://docs.microsoft.com/rest/api/maps)
