---
title: Automatyczne skalowanie zadań Stream Analytics
description: W tym artykule opisano sposób automatycznego skalowania Stream Analytics zadania na podstawie wstępnie zdefiniowanego harmonogramu lub wartości metryk zadania
author: sidramadoss
ms.author: sidram
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: how-to
ms.date: 06/03/2020
ms.openlocfilehash: 07cbb28b98fcbac1932424c1c72f388813ec2400
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/07/2020
ms.locfileid: "86037566"
---
# <a name="autoscale-stream-analytics-jobs-using-azure-automation"></a>Automatyczne skalowanie zadań Stream Analytics przy użyciu Azure Automation

Koszt zadań Stream Analytics można zoptymalizować, konfigurując automatyczne skalowanie. Skalowanie automatyczne zwiększa lub zmniejsza liczbę jednostek przesyłania strumieniowego zadania (SUs) w celu dopasowania do zmiany obciążenia wejściowego. Zamiast nadmiernej aprowizacji zadania można skalować w górę lub w dół odpowiednio do potrzeb. Istnieją dwa sposoby konfigurowania zadań do automatycznego skalowania:
1. **Wstępnie Zdefiniuj harmonogram** , gdy masz przewidywalne obciążenie wejściowe. Na przykład oczekujesz wyższego poziomu zdarzeń wejściowych w ciągu dnia i chcesz, aby zadanie było uruchamiane z większą liczbą usług.
2. **Wyzwalaj operacje skalowania w górę i w dół w oparciu o metryki zadań** , gdy nie masz przewidywalnego obciążenia wejściowego. Można dynamicznie zmienić liczbę usług SUs na podstawie metryk zadania, takich jak liczba zdarzeń wejściowych lub zaległe zdarzenia wejściowe.

## <a name="prerequisites"></a>Wymagania wstępne
Przed rozpoczęciem konfigurowania skalowania automatycznego dla danego zadania wykonaj następujące czynności.
1. Zadanie jest zoptymalizowane pod kątem [topologii równoległej](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-parallelization). Jeśli można zmienić skalę zadania, gdy jest ono uruchomione, zadanie ma topologię równoległą i można ją skonfigurować do automatycznego skalowania.
2. [Utwórz konto Azure Automation](https://docs.microsoft.com/azure/automation/automation-create-standalone-account) z włączoną opcją "Uruchom jako". To konto musi mieć uprawnienia do zarządzania zadaniami Stream Analytics.

## <a name="set-up-azure-automation"></a>Skonfiguruj Azure Automation
### <a name="configure-variables"></a>Konfiguruj zmienne
Dodaj następujące zmienne w ramach konta Azure Automation. Te zmienne zostaną użyte w elementach Runbook, które są opisane w następnych krokach.

| Nazwa | Typ | Wartość |
| --- | --- | --- |
| **jobName** | String | Nazwa zadania Stream Analytics, które ma być skalowane automatycznie. |
| **resourceGroupName** | String | Nazwa grupy zasobów, w której znajduje się to zadanie. |
| **subId** | String | Identyfikator subskrypcji, w której znajduje się to zadanie. |
| **increasedSU** | Integer | Wyższa wartość SU, w której ma być skalowane zadanie w harmonogramie. Ta wartość musi być jedną z prawidłowych opcji SU, które są widoczne w ustawieniach **skalowania** zadania podczas jego działania. |
| **decreasedSU** | Integer | Dolna wartość SU, na którą zadanie ma być skalowane w harmonogramie. Ta wartość musi być jedną z prawidłowych opcji SU, które są widoczne w ustawieniach **skalowania** zadania podczas jego działania. |
| **maxSU** | Integer | Maksymalna wartość SU, w której ma być skalowane zadanie, w przypadku automatycznego skalowania przez załadowanie. Ta wartość musi być jedną z prawidłowych opcji SU, które są widoczne w ustawieniach **skalowania** zadania podczas jego działania. |
| **minSU** | Integer | Minimalna wartość SU, w której ma być skalowane zadanie, w przypadku automatycznego skalowania przez załadowanie. Ta wartość musi być jedną z prawidłowych opcji SU, które są widoczne w ustawieniach **skalowania** zadania podczas jego działania. |

![Dodawanie zmiennych w Azure Automation](./media/autoscale/variables.png)

### <a name="create-runbooks"></a>Tworzenie elementów Runbook
Następnym krokiem jest utworzenie dwóch elementów Runbook programu PowerShell. Jeden na potrzeby skalowania w górę i w dół dla operacji skalowania w dół.
1. Na koncie Azure Automation przejdź do pozycji **elementy Runbook** w obszarze **Automatyzacja procesów** , a następnie wybierz pozycję **Utwórz element Runbook**.
2. Nazwij pierwszy element Runbook *ScaleUpRunbook* z typem ustawionym na PowerShell. Użyj [skryptu ScaleUpRunbook PowerShell](https://github.com/Azure/azure-stream-analytics/blob/master/Autoscale/ScaleUpRunbook.ps1) dostępnego w serwisie GitHub. Zapisz i Opublikuj.
3. Utwórz inny element Runbook o nazwie *ScaleDownRunbook* z typem PowerShell. Użyj [skryptu ScaleDownRunbook PowerShell](https://github.com/Azure/azure-stream-analytics/blob/master/Autoscale/ScaleDownRunbook.ps1) dostępnego w serwisie GitHub. Zapisz i Opublikuj.

![Automatyczne skalowanie elementów Runbook w Azure Automation](./media/autoscale/runbooks.png)

Masz teraz elementy Runbook, które mogą automatycznie wyzwalać operacje skalowania w górę i w dół w ramach zadania Stream Analytics. Te elementy Runbook można wyzwalać przy użyciu wstępnie zdefiniowanego harmonogramu lub mogą być ustawiane dynamicznie w oparciu o metryki zadań.

## <a name="autoscale-based-on-a-schedule"></a>Automatyczne skalowanie na podstawie harmonogramu
Azure Automation umożliwia skonfigurowanie harmonogramu wyzwalania elementów Runbook.
1. Na koncie Azure Automation wybierz pozycję **harmonogramy** w obszarze **zasoby udostępnione**. Następnie wybierz pozycję **Dodaj harmonogram**.
2. Można na przykład utworzyć dwa harmonogramy. Reprezentuje to, kiedy zadanie ma być skalowane w górę, a drugie, które reprezentuje, gdy zadanie ma być skalowane w dół. Można zdefiniować cykl dla tych harmonogramów.

   ![Harmonogramy w Azure Automation](./media/autoscale/schedules.png)

3. Otwórz **ScaleUpRunbook** a następnie wybierz pozycję **harmonogramy** w obszarze **zasoby**. Następnie można połączyć element Runbook z harmonogramem utworzonym w poprzednich krokach. Można mieć wiele harmonogramów połączonych z tym samym elementem Runbook, co może być przydatne, jeśli chcesz uruchomić tę samą operację skalowania w różnych porach dnia.

![Planowanie elementów Runbook w Azure Automation](./media/autoscale/schedulerunbook.png)

1. Powtórz poprzedni krok dla **ScaleDownRunbook**.

## <a name="autoscale-based-on-load"></a>Automatyczne skalowanie na podstawie obciążenia
Mogą wystąpić sytuacje, w których nie można przewidzieć obciążenia wejściowego. W takich przypadkach bardziej optymalne jest skalowanie w górę i w dół w ramach kroków w ramach minimalnej i maksymalnej wartości. Reguły alertów można skonfigurować w zadaniach Stream Analytics, aby wyzwalać elementy Runbook, gdy metryki zadań przechodzą powyżej lub poniżej wartości progowej.
1. W ramach konta Azure Automation Utwórz dwie zmienne o wartościach całkowitych o nazwie **minSU** i **maxSU**. To ustawienie określa granice, w ramach których zadanie będzie skalowane w ramach kroków.
2. Utwórz dwa nowe elementy Runbook. Możesz użyć [skryptu programu StepScaleUp PowerShell](https://github.com/Azure/azure-stream-analytics/blob/master/Autoscale/StepScaleUp.ps1) , który zwiększa liczbę usług SUs zadania w przyrostach do wartości **maxSU** . Można również użyć [skryptu programu StepScaleDown PowerShell](https://github.com/Azure/azure-stream-analytics/blob/master/Autoscale/StepScaleDown.ps1) , który zmniejsza liczbę czynności programu SUs dla zadania, dopóki nie zostanie osiągnięta wartość **minSU** . Alternatywnie można użyć elementów Runbook z poprzedniej sekcji, jeśli masz określone wartości SU, do których chcesz skalować.
3. W zadaniu Stream Analytics wybierz pozycję **reguły alertów** w obszarze **monitorowanie**. 
4. Utwórz dwie grupy akcji. Jeden do użycia podczas operacji skalowania w górę i innej dla operacji skalowania w dół. Wybierz pozycję **Zarządzaj akcjami** , a następnie kliknij pozycję **Dodaj grupę akcji**. 
5. Wypełnij pola wymagane. Wybierz **element Runbook usługi Automation** po wybraniu **typu akcji**. Wybierz element Runbook, który ma zostać wyzwolony po uruchomieniu alertu. Następnie utwórz grupę akcji.

   ![Tworzenie grupy akcji](./media/autoscale/create-actiongroup.png)
6. Utwórz [**nową regułę alertu**](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-set-up-alerts#set-up-alerts-in-the-azure-portal) w zadaniu. Określ warunek na podstawie wybranej metryki. [ *Zdarzenia wejściowe*, *użycie Su%* lub *zaległe zdarzenia wejściowe* ](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-monitoring#metrics-available-for-stream-analytics) są zalecanymi metrykami używanymi do definiowania logiki skalowania automatycznego. Zaleca się również korzystanie z 1-minutowej *szczegółowości agregacji* i *częstotliwości oceny* podczas wyzwalania operacji skalowania w górę. Pozwoli to zagwarantować, że zadanie będzie miało dużo zasobów do poradzenia z dużymi skokami w woluminie wejściowym.
7. Wybierz grupę akcji utworzoną w ostatnim kroku i Utwórz alert.
8. Powtórz kroki od 2 do 4 dla wszelkich dodatkowych operacji skalowania, które mają być wyzwalane na podstawie warunku metryk zadań.

Najlepszym rozwiązaniem jest uruchomienie testów skalowania przed uruchomieniem zadania w środowisku produkcyjnym. Przetestowanie zadania pod kątem różnych obciążeń wejściowych pozwala uzyskać informacje o liczbie wymaganych zadań programu SUs dla różnych przepływów pracy. Może to poinformować o warunkach zdefiniowanych w regułach alertów, które wyzwalają operacje skalowania w górę i w dół. 

## <a name="next-steps"></a>Następne kroki
* [Tworzenie zapytań działania równoległego w Azure Stream Analytics](stream-analytics-parallelization.md)
* [Skalowanie zadań Azure Stream Analytics w celu zwiększenia przepływności](stream-analytics-scale-jobs.md)
