---
title: Trenowanie modelu Pytorch
titleSuffix: Azure Machine Learning
description: Dowiedz się, jak szkolić model pytorch od podstaw lub Finetune go.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 05/26/2020
ms.openlocfilehash: af14d4770d032c23216b805045eb27fadded5954
ms.sourcegitcommit: 1e6c13dc1917f85983772812a3c62c265150d1e7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/09/2020
ms.locfileid: "86170262"
---
# <a name="train-pytorch-model"></a>Trenowanie modelu Pytorch

W tym artykule opisano, jak używać modułu **uczenie Pytorch model** w programie Azure Machine Learning Designer (wersja zapoznawcza) w celu uczenia modeli Pytorch, takich jak DenseNet. Szkolenia odbywają się po zdefiniowaniu modelu i ustawieniu jego parametrów i wymaganiu etykiet danych. 

## <a name="how-to-use-train-pytorch-model"></a>Jak używać modelu uczenia Pytorch 

1. Dodaj moduł [DenseNet](densenet.md) lub [ResNet](resnet.md) do wersji roboczej potoku w projektancie.

2. Dodaj moduł **uczenie modelu Pytorch** do potoku. Ten moduł można znaleźć w kategorii **szkolenia modeli** . Rozwiń węzeł **uczenie**, a następnie przeciągnij moduł **uczenie modelu Pytorch** do potoku.

   > [!NOTE]
   > Moduł **uczenie Pytorch model** jest lepszym rozwiązaniem w przypadku obliczeń typu **GPU** dla dużego zestawu danych. w przeciwnym razie potok zakończy się niepowodzeniem. Można wybrać opcję obliczenia dla określonego modułu w prawym okienku modułu przez ustawienie opcji **Użyj innego elementu docelowego obliczeń**.

3.  Z lewej strony, Dołącz niepociąg model. Dołącz zestaw danych szkoleniowych i zestaw danych walidacji do środkowego i prawego wejścia do **modelu uczenie Pytorch**.

    W przypadku nauczenia model musi być modelem pytorch, takim jak DenseNet; w przeciwnym razie zostanie zgłoszona wartość "InvalidModelDirectoryError".

    W przypadku zestawu danych zestaw danych szkoleniowych musi być katalogiem obrazu z etykietą. Zapoznaj się z artykułem **konwertowanie do katalogu obrazu** , aby uzyskać informacje o katalogu obrazu z etykietą. Jeśli nie ma etykiety, zostanie zgłoszony element "NotLabeledDatasetError".

    Zestaw danych szkoleniowych i zestaw danych sprawdzania poprawności mają te same kategorie etykiet, w przeciwnym razie zostanie zgłoszone InvalidDatasetError.

4.  W przypadku **epok**, określ, ile epok chcesz szkolić. Cały zestaw danych będzie powtarzany w każdej epoki, domyślnie 5.

5.  Dla **rozmiaru partii**Określ liczbę wystąpień do uczenia w partii, domyślnie 16.

6.  W polu **stawka szkoleniowa**Określ wartość dla *stawki szkoleniowej*. Wartość współczynnika uczenia steruje rozmiarem kroku, który jest używany w Optymalizatorze, takim jak SGD, za każdym razem, gdy model jest testowany i skorygowany.

    Zmniejszając szybkość, można testować model częściej, z ryzykiem, który może zostać zablokowany w lokalnej Plateau. Dzięki powiększeniu tego kroku można szybciej łączyć się z ryzykiem w przypadku przekroczenia rzeczywistych wartości. domyślnie 0,001.

7.  W przypadku **losowego inicjatora**opcjonalnie wpisz wartość całkowitą, która ma być używana jako inicjator. Użycie inicjatora jest zalecane, jeśli chcesz zapewnić odtwarzalność eksperymentu w ramach przebiegów.

8.  W przypadku wybrania tej metody należy określić liczbę epoki do wczesnego zatrzymania **szkolenia, jeśli**utrata walidacji nie zmniejszy się po kolei. Domyślnie 3.

9.  Prześlij potok. Jeśli zestaw danych ma większy rozmiar, zajmie trochę czasu.

## <a name="results"></a>Wyniki

Po zakończeniu przebiegu potoku, aby użyć modelu do oceniania, Połącz [model uczenia Pytorch](train-pytorch-model.md) z [modelem obrazu oceny](score-image-model.md), aby przewidzieć wartości dla nowych przykładów wejściowych.

## <a name="technical-notes"></a>Uwagi techniczne
###  <a name="expected-inputs"></a>Oczekiwane dane wejściowe  

| Nazwa               | Typ                    | Opis                              |
| ------------------ | ----------------------- | ---------------------------------------- |
| Nieuczenie modelu    | UntrainedModelDirectory | Nieuczenie modelu, wymaganie pytorch         |
| Zestaw danych szkoleniowych   | ImageDirectory          | Zestaw danych szkoleniowych                         |
| Zestaw danych walidacji | ImageDirectory          | Zestaw danych walidacji do oceny przy każdej epoki |

###  <a name="module-parameters"></a>Parametry modułu  

| Nazwa          | Zakres            | Typ    | Domyślne | Opis                              |
| ------------- | ---------------- | ------- | ------- | ---------------------------------------- |
| Epoki        | >0               | Integer | 5       | Wybierz kolumnę, która zawiera etykietę lub kolumnę wyniku |
| Rozmiar partii    | >0               | Integer | 16      | Liczba wystąpień do uczenia w partii   |
| Tempo nauki | >= Double. Epsilon | Float   | 0,001   | Początkowa stawka szkoleniowa dla Stochastycznegoego gradientu. |
| Losowy inicjator   | Dowolny              | Integer | 1       | Inicjator dla generatora liczb losowych używanego przez model. |
| Oczekując      | >0               | Integer | 3       | Ile epok z wczesnym zatrzymywaniem szkoleń   |

###  <a name="outputs"></a>Dane wyjściowe  

| Nazwa          | Typ           | Opis   |
| ------------- | -------------- | ------------- |
| Model szkolony | ModelDirectory | Model szkolony |

## <a name="next-steps"></a>Następne kroki

Zapoznaj się z [zestawem modułów dostępnych](module-reference.md) do Azure Machine Learning. 



