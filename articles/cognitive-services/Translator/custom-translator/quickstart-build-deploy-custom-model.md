---
title: 'Szybki start: Tworzenie, wdrażanie i używanie modelu niestandardowego — rozszerzenie Custom Translator'
titleSuffix: Azure Cognitive Services
description: W tym przewodniku Szybki start szczegółowo omówiono tworzenie systemu tłumaczenia przy użyciu rozszerzenia Custom Translator.
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 05/26/2020
ms.author: swmachan
ms.topic: quickstart
ms.openlocfilehash: b0992c4d18fdb9cb5201ab3ef52fba8ee3feb7a2
ms.sourcegitcommit: 845a55e6c391c79d2c1585ac1625ea7dc953ea89
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2020
ms.locfileid: "85964383"
---
# <a name="quickstart-build-deploy-and-use-a-custom-model-for-translation"></a>Szybki start: Tworzenie, wdrażanie i używanie niestandardowego modelu tłumaczenia

Ten artykuł zawiera szczegółowe instrukcje tworzenia systemu tłumaczenia za pomocą rozszerzenia Custom Translator.

## <a name="prerequisites"></a>Wymagania wstępne

1. Aby używać portalu [Custom Translator](https://portal.customtranslator.azure.ai), musisz zalogować się na [konto Microsoft](https://signup.live.com) lub [konto usługi Azure AD](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-whatis) (konto organizacji hostowane na platformie Azure).

2. Subskrypcja usługi Translator za pośrednictwem Azure Portal. Do skojarzenia z obszarem roboczym w usłudze translator niestandardowy potrzebny będzie klucz subskrypcji usługi Translator. Zobacz [, jak zarejestrować się w usłudze translator](https://docs.microsoft.com/azure/cognitive-services/translator/translator-text-how-to-signup).

3. Jeśli masz oba powyższe funkcje, zaloguj się do portalu usługi [Custom translator](https://portal.customtranslator.azure.ai) , aby utworzyć obszary robocze, projekty, przekazać pliki i utworzyć/wdrożyć modele.

## <a name="create-a-workspace"></a>Tworzenie obszaru roboczego

Jeśli użytkownik jest użytkownikiem po raz pierwszy, zostanie poproszony o zgodę na warunki użytkowania usługi, utworzenie obszaru roboczego i skojarzenie obszaru roboczego z subskrypcją usługi Translator.

![Utwórz obszar roboczy Tworzenie obszaru roboczego Tworzenie obszaru roboczego Tworzenie obszaru roboczego Utwórz obszar roboczy ](media/quickstart/terms-of-service.png)
 ![ ](media/quickstart/create-workspace-1.png)
 ![ ](media/quickstart/create-workspace-2.png)
 ![ ](media/quickstart/create-workspace-3.png)
 ![ ](media/quickstart/create-workspace-4.png)
 ![ ](media/quickstart/create-workspace-5.png)
 ![](media/quickstart/create-workspace-6.png)

W kolejnych odwiedzinach w portalu usługi tłumaczenia niestandardowego przejdź do strony Ustawienia, na której możesz zarządzać obszarem roboczym, utworzyć więcej obszarów roboczych, skojarzyć klucz subskrypcji usługi Translator z obszarami roboczymi, dodać współwłaściciele i zmienić klucz subskrypcji.

## <a name="create-a-project"></a>Tworzenie projektu

Na stronie docelowej portalu Custom Translator kliknij pozycję New Project (Nowy projekt). W oknie dialogowym możesz wprowadzić odpowiednią nazwę projektu, parę języków i kategorię, a także inne pola, które mają zastosowanie. Następnie zapisz projekt. Aby uzyskać więcej informacji, zobacz sekcję [Tworzenie projektu](how-to-create-project.md).

![Tworzenie projektu](media/quickstart/ct-how-to-create-project.png)


## <a name="upload-documents"></a>Przekazywanie dokumentów

Następnie przekaż zestawy dokumentów dotyczących [szkolenia](training-and-model.md#training-document-type-for-custom-translator), [dostrajania](training-and-model.md#tuning-document-type-for-custom-translator) i [testowania](training-and-model.md#testing-dataset-for-custom-translator). Możesz przekazać zarówno dokumenty [równoległe](what-are-parallel-documents.md), jak i dokumenty kombinowane. Możesz również przekazać [słownik](what-is-dictionary.md).

Dokumenty można przekazywać na karcie dokumentów lub na stronie danego projektu.

![Przekazywanie dokumentów](media/quickstart/ct-how-to-upload.png)

Przekazując materiały, wybierz typ dokumentu (szkolenie, dostrajanie lub testowanie) oraz parę języków. W przypadku przekazywania dokumentów równoległych musisz dodatkowo podać nazwę dokumentu. Aby uzyskać więcej informacji, zobacz sekcję [Przekazywanie dokumentu](how-to-upload-document.md).

## <a name="create-a-model"></a>Tworzenie modelu

Po przekazaniu wszystkich wymaganych dokumentów należy utworzyć model.

Wybierz utworzony projekt. Pojawią się wszystkie przekazane dokumenty, których para języków jest zgodna z parą języków w projekcie. Wybierz dokumenty, które chcesz uwzględnić w modelu. Możesz wybrać dane [szkoleniowe](training-and-model.md#training-document-type-for-custom-translator), [dostrajania](training-and-model.md#tuning-document-type-for-custom-translator) i [testowe](training-and-model.md#testing-dataset-for-custom-translator) albo wskazać tylko dane szkoleniowe — rozszerzenie Custom Translator automatycznie utworzy zestaw dostrajania i zestaw testowy w modelu.

![Tworzenie modelu](media/quickstart/ct-how-to-train.png)

Po wybraniu odpowiednich dokumentów kliknij przycisk tworzenia modelu, aby utworzyć model i rozpocząć szkolenie. Na karcie modeli możesz sprawdzić stan szkolenia i szczegóły wszystkich wytrenowanych modeli.

Aby uzyskać więcej informacji, zobacz sekcję [Tworzenie modelu](how-to-train-model.md).

## <a name="analyze-your-model"></a>Analizowanie modelu

Po pomyślnym zakończeniu szkolenia sprawdź wyniki. Wynik BLEU jest metryką, która określa jakość tłumaczenia. Możesz również ręcznie porównać tłumaczenia uzyskane za pomocą modelu niestandardowego z tłumaczeniami zawartymi z zestawie testowym, przechodząc na kartę „Test”, a następnie klikając pozycję „System Results” (Wyniki systemu). Sprawdzenie kilku takich tłumaczeń pozwala wiarygodnie ustalić, jaka jest jakość tłumaczeń generowanych przez system. Aby uzyskać więcej informacji, zobacz sekcję [Wyniki testu systemu](how-to-view-system-test-results.md).

## <a name="deploy-a-trained-model"></a>Wdrażanie przeszkolonego modelu

Gdy wszystko jest gotowe do wdrożenia przeszkolonego modelu, kliknij przycisk „Deploy” (Wdróż). W danym projekcie możesz mieć jeden wdrożony model. Stan wdrożenia możesz sprawdzić w kolumnie Status (Stan). Aby uzyskać więcej informacji, zobacz sekcję [Wdrożenie modelu](how-to-view-system-test-results.md#deploy-a-model).

![Wdrażanie przeszkolonego modelu](media/quickstart/ct-how-to-deploy.png)

## <a name="use-a-deployed-model"></a>Korzystanie z wdrożonego modelu

Do wdrożonych modeli można uzyskać dostęp za pośrednictwem translatora, określając IDKategorii] ( https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-translate?tabs=curl) . Więcej informacji o usłudze translator można znaleźć na stronie sieci Web [odwołań API](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference) .

## <a name="next-steps"></a>Następne kroki

- Dowiedz się, jak nawigować po [obszarze roboczym rozszerzenia Custom Translator i jak zarządzać projektami](workspace-and-project.md).
