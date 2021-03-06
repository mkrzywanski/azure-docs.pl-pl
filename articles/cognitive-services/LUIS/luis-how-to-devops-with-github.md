---
title: Jak zastosować DevOps za pomocą usługi GitHub-LUIS
titleSuffix: Azure Cognitive Services
description: Zastosuj DevOps z Language Understanding (LUIS) i GitHub.
services: cognitive-services
author: andycw
manager: cmayo
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 06/5/2020
ms.author: anwigley
ms.openlocfilehash: 6df65040277ac61cca5fb4bf7fce5b5a7b2f3afe
ms.sourcegitcommit: 52d2f06ecec82977a1463d54a9000a68ff26b572
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/15/2020
ms.locfileid: "84783762"
---
# <a name="apply-devops-to-luis-app-development-using-github-actions"></a>Zastosuj DevOps do tworzenia aplikacji LUIS przy użyciu akcji usługi GitHub

Przejdź do [repozytorium szablonów Luis DevOps](https://github.com/Azure-Samples/LUIS-DevOps-Template) , aby zapoznać się z kompletnym rozwiązaniem, które implementuje implementacje DevOps oraz najlepsze rozwiązania dotyczące inżynierii oprogramowania dla Luis. Za pomocą tego repozytorium szablonów można utworzyć własne repozytorium z wbudowaną obsługą przepływów pracy ciągłej integracji/ciągłego wdrażania, które umożliwiają [kontrolę źródła](luis-concept-devops-sourcecontrol.md), [Automatyczne kompilacje](luis-concept-devops-automation.md), [testowanie](luis-concept-devops-testing.md)i [Zarządzanie](luis-concept-devops-automation.md#release-management) wydaniami przy użyciu Luis dla własnego projektu.

## <a name="the-luis-devops-template-repo"></a>Repozytorium szablonów LUIS DevOps

[Repozytorium szablonów Luis DevOps](https://github.com/Azure-Samples/LUIS-DevOps-Template) zawiera instrukcje:

* **Klonowanie repozytorium szablonów** — Skopiuj szablon do własnego repozytorium GitHub.
* **Konfigurowanie zasobów Luis** — tworzenie [Luis zasobów tworzenia i przewidywania na platformie Azure](https://docs.microsoft.com/azure/cognitive-services/luis/luis-how-to-azure-subscription#create-resources-in-azure-cli) , które będą używane przez przepływy pracy ciągłej integracji.
* **Skonfiguruj przepływy pracy** ciągłej integracji/ciągłego konfigurowania parametrów dla przepływów pracy Ci/CD i Zapisz je w wpisach [tajnych usługi GitHub](https://help.github.com/actions/configuring-and-managing-workflows/creating-and-storing-encrypted-secrets).
* **Przegląda ["pętlę wewnętrzną dev"](https://mitchdenny.com/the-inner-loop/) ** — deweloper wprowadza aktualizacje przykładowej aplikacji Luis podczas pracy w gałęzi deweloperskiej, testuje aktualizacje, a następnie zgłasza żądanie ściągnięcia w celu zaproponowania zmian i sprawdzenia zatwierdzenia.
* **Wykonaj przepływy pracy** [ciągłej integracji/ciągłego wdrażania, aby utworzyć i przetestować aplikację Luis](luis-concept-devops-automation.md) przy użyciu akcji usługi GitHub.
* **Przeprowadzaj automatyczne testowanie** — wykonaj [zautomatyzowane testowanie wsadowe dla aplikacji Luis](luis-concept-devops-testing.md) , aby oszacować jakość aplikacji.
* **Wdróż aplikację Luis** — wykonaj [zadanie ciągłego dostarczania (CD)](luis-concept-devops-automation.md#continuous-delivery-cd) w celu opublikowania aplikacji Luis.
* **Używanie repozytorium z własnym projektem** — wyjaśnia, jak używać repozytorium z własną aplikacją Luis.

## <a name="next-steps"></a>Następne kroki

* Użyj [repozytorium szablonów DevOps Luis](https://github.com/Azure-Samples/LUIS-DevOps-Template) , aby zastosować DevOps z własnym projektem.
* [Strategie kontroli źródła i gałęzi dla LUIS](luis-concept-devops-sourcecontrol.md)
* [Testowanie pod kątem LUIS DevOps](luis-concept-devops-testing.md)
* [Przepływy pracy automatyzacji dla LUIS DevOps](luis-concept-devops-automation.md)
