---
title: Debuguj zadania cykliczne w Azure Data Lake Analytics
description: Dowiedz się, jak używać Azure Data Lake Tools for Visual Studio do debugowania nietypowego zadania cyklicznego.
services: data-lake-analytics
ms.reviewer: jasonh
ms.assetid: dc9b21d8-c5f4-4f77-bcbc-eff458f48de2
ms.service: data-lake-analytics
ms.topic: how-to
ms.date: 05/20/2018
ms.openlocfilehash: 86d5134e257d2dae642eceb933a78047773b25a9
ms.sourcegitcommit: 0e8a4671aa3f5a9a54231fea48bcfb432a1e528c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/24/2020
ms.locfileid: "87129954"
---
# <a name="troubleshoot-an-abnormal-recurring-job"></a>Rozwiązywanie problemów z nietypowym zadaniem cyklicznym

W tym artykule pokazano, jak używać [Azure Data Lake Tools for Visual Studio](https://aka.ms/adltoolsvs) do rozwiązywania problemów z cyklicznymi zadaniami. Dowiedz się więcej o zadaniach potokowych i cyklicznych w [blogu Azure Data Lake i Azure HDInsight](https://blogs.msdn.microsoft.com/azuredatalake/2017/09/19/managing-pipeline-recurring-jobs-in-azure-data-lake-analytics-made-easy/).

Zadania cykliczne zwykle współdzielą tę samą logikę zapytań i podobne dane wejściowe. Załóżmy na przykład, że zadanie cykliczne jest uruchamiane co poniedziałek rano o godzinie 8 Aby obliczyć tygodniowy aktywny użytkownik w ubiegłym tygodniu. Skrypty tych zadań współużytkują jeden szablon skryptu, który zawiera logikę zapytania. Dane wejściowe dla tych zadań są danymi użycia dla ostatniego tygodnia. Udostępnianie tej samej logiki zapytań i podobnych danych wejściowych zwykle oznacza, że wydajność tych zadań jest podobna i stabilna. Jeśli jedno z cyklicznych zadań nagle działa nieprawidłowo, kończy się niepowodzeniem lub spowalnia pracę, warto wykonać następujące czynności:

- Zobacz raporty statystyczne dotyczące poprzednich przebiegów zadania cyklicznego, aby zobaczyć, co się stało.
- Porównaj nietypowe zadanie z normalną, aby ustalić, co zostało zmienione.

**Widok zadań pokrewnych** w Azure Data Lake Tools for Visual Studio ułatwia przyspieszenie rozwiązywania problemów w obu przypadkach.

## <a name="step-1-find-recurring-jobs-and-open-related-job-view"></a>Krok 1: Znajdź zadania cykliczne i Otwórz widok powiązanego zadania

Aby użyć widoku zadania powiązanego do rozwiązywania problemów z cyklicznym zadaniem, należy najpierw znaleźć zadanie cykliczne w programie Visual Studio, a następnie otworzyć widok powiązane zadania.

### <a name="case-1-you-have-the-url-for-the-recurring-job"></a>Przypadek 1: adres URL zadania cyklicznego

Za pomocą **narzędzi**  >  **Data Lake**  >  **widoku zadania**można wkleić adres URL zadania, aby otworzyć widok zadań w programie Visual Studio. Wybierz pozycję **Wyświetl powiązane zadania** , aby otworzyć widok powiązane zadania.

![Łącze Wyświetl powiązane zadania w narzędziach Data Lake Analytics](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/view-related-job.png)
 
### <a name="case-2-you-have-the-pipeline-for-the-recurring-job-but-not-the-url"></a>Przypadek 2: masz potok dla zadania cyklicznego, ale nie adres URL

W programie Visual Studio możesz otworzyć przeglądarkę potoku za Eksplorator serwera > konta Azure Data Lake Analytics > **potoków**. (Jeśli nie możesz znaleźć tego węzła w Eksplorator serwera, [Pobierz najnowszą wtyczkę](https://aka.ms/adltoolsvs)). 

![Wybieranie węzła potoki](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/pipeline-browser.png)

W przeglądarce potoku wszystkie potoki dla konta Data Lake Analytics są wymienione na liście po lewej stronie. Można rozwinąć potoki, aby znaleźć wszystkie zadania cykliczne, a następnie wybrać te, które mają problemy. Widok powiązane zadania zostanie otwarty po prawej stronie.

![Wybieranie potoku i otwieranie widoku zadania powiązanego](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/recurring-job-view.png)

## <a name="step-2-analyze-a-statistics-report"></a>Krok 2. analizowanie raportu statystycznego

Podsumowanie i raport statystyk są wyświetlane w górnej części widoku zadań. W tym miejscu możesz znaleźć potencjalną główną przyczynę problemu. 

1.  W raporcie oś X pokazuje czas przesłania zadania. Użyj go, aby znaleźć nietypowe zadanie.
2.  Aby sprawdzić statystyki i uzyskać szczegółowe informacje o problemie i możliwych rozwiązaniach, należy użyć procesu na poniższym diagramie.

![Diagram procesów do sprawdzania statystyk](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/recurring-job-metrics-debugging-flow.png)

## <a name="step-3-compare-the-abnormal-job-to-a-normal-job"></a>Krok 3. Porównanie nietypowego zadania z normalnym zadaniem

Wszystkie przesłane zadania cykliczne można znaleźć za pomocą listy zadań w dolnej części widoku zadania pokrewne. Aby znaleźć więcej szczegółowych informacji i potencjalnych rozwiązań, kliknij prawym przyciskiem myszy nietypowe zadanie. Widok różnica zadań umożliwia porównanie nietypowego zadania z poprzednią normalną.

![Menu skrótów do porównywania zadań](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/compare-job.png)

Należy zwrócić uwagę na duże różnice między tymi dwoma zadaniami. Te różnice prawdopodobnie powodują problemy z wydajnością. Aby to sprawdzić, wykonaj czynności opisane na poniższym diagramie:

![Diagram procesu służący do sprawdzania różnic między zadaniami](./media/data-lake-analytics-data-lake-tools-debug-recurring-job/recurring-job-diff-debugging-flow.png)

## <a name="next-steps"></a>Następne kroki

* [Rozwiązywanie problemów z pochyleniem danych](data-lake-analytics-data-lake-tools-data-skew-solutions.md)
* [Debuguj kod języka C# zdefiniowany przez użytkownika dla niezakończonych zadań U-SQL](data-lake-analytics-debug-u-sql-jobs.md)
