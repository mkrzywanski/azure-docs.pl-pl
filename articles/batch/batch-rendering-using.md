---
title: Korzystanie z funkcji renderowania
description: Jak korzystać z funkcji renderowania Azure Batch. Spróbuj użyć aplikacji Batch Explorer bezpośrednio lub wywołać z wtyczki aplikacji klienckiej.
author: mscurrell
ms.author: markscu
ms.date: 03/05/2020
ms.topic: how-to
ms.openlocfilehash: e4f33f0974462197382ea6ac5d2b68d61909f73b
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/23/2020
ms.locfileid: "87079248"
---
# <a name="using-azure-batch-rendering"></a>Używanie renderowania Azure Batch

Istnieje kilka sposobów używania renderowania Azure Batch:

* Interfejsy API:
  * Napisz kod przy użyciu dowolnego interfejsu API usługi Batch.  Deweloperzy mogą integrować możliwości Azure Batch z istniejącymi aplikacjami lub przepływami pracy, zarówno w chmurze, jak i lokalnie.
* Narzędzia wiersza polecenia:
  * W celu użycia wsadowego skryptu można użyć [wiersza polecenia platformy Azure](/cli/azure/) lub [programu PowerShell](/powershell/azure/) .
  * W szczególności [Obsługa szablonu interfejsu wiersza polecenia](./batch-cli-templates.md) w usłudze Batch ułatwia tworzenie pul i przesyłanie zadań.
* Batch Explorer interfejsie użytkownika:
  * [Batch Explorer](https://github.com/Azure/BatchLabs) to międzyplatformowe narzędzie klienta, które umożliwia również zarządzanie i monitorowanie kont wsadowych.
  * Dla każdej aplikacji renderowania są udostępniane różne szablony puli i zadań, których można użyć do łatwego tworzenia pul i przesyłania zadań.  Zestaw szablonów znajduje się w interfejsie użytkownika aplikacji z plikami szablonów dostępnymi z usługi GitHub.
  * Szablony niestandardowe można tworzyć od podstaw lub można kopiować i modyfikować dostarczone szablony z usługi GitHub.
* Wtyczki aplikacji klienckich:
  * Dostępne są wtyczki zezwalające na używanie renderowania wsadowego bezpośrednio w aplikacjach do projektowania i modelowania klientów.  Wtyczki głównie wywołują aplikację Batch Explorer z informacjami kontekstowymi dotyczącymi bieżącego modelu 3W i zawiera funkcje ułatwiające zarządzanie zasobami.

Najlepszym sposobem na wypróbowanie Azure Batch renderowania i najprostszych sposobów dla użytkowników końcowych, którzy nie są deweloperami i nie są ekspertami platformy Azure, jest użycie Batch Explorer aplikacji bezpośrednio lub wywołania z wtyczki aplikacji klienckiej.

## <a name="using-batch-explorer"></a>Używanie Batch Explorer

Aby zapoznać się z samouczkiem krok po kroku dotyczącym używania Batch Explorer do wykonania renderowania, zobacz [samouczek programu Blend](./tutorial-rendering-batchexplorer-blender.md).

### <a name="download-and-install"></a>Pobierz i zainstaluj

[Pliki do pobrania Batch Explorer są dostępne](https://azure.github.io/BatchExplorer/) dla systemów Windows, OSX i Linux.

### <a name="using-templates-to-create-pools-and-run-jobs"></a>Tworzenie pul i uruchamianie zadań przy użyciu szablonów

Obszerny zestaw szablonów jest dostępny do użytku z Batch Explorer, które ułatwiają tworzenie pul i przesyłanie zadań dla różnych aplikacji do renderowania bez konieczności określania wszystkich właściwości wymaganych do tworzenia pul, zadań i zadań bezpośrednio z usługą Batch.  Szablony dostępne w Batch Explorer są przechowywane i widoczne w [repozytorium GitHub](https://github.com/Azure/BatchExplorer-data/tree/master/ncj).

![Galeria Batch Explorer](./media/batch-rendering-using/batch-explorer-gallery.png)

Szablony są udostępniane dla wszystkich aplikacji dostępnych w portalu Marketplace.  Dla każdej aplikacji istnieje wiele szablonów, łącznie z szablonami puli do uwzględnienia w przypadku pul procesora CPU i procesora GPU, pul systemów Windows i Linux; Szablony zadań zawierają pełną ramkę lub renderowanie rozproszonego renderowania w programie Blend i śledzenie. Zestaw dostarczonych szablonów zostanie rozszerzony z upływem czasu, aby pomieścić inne możliwości usługi Batch, takie jak automatyczne skalowanie puli.

Istnieje również możliwość, że szablony niestandardowe mają być tworzone od podstaw lub przez modyfikację dostarczonych szablonów. Szablonów niestandardowych można użyć, wybierając element "Local templates" w sekcji "Gallery" Batch Explorer.

### <a name="file-system-and-data-movement"></a>System plików i przenoszenie danych

Sekcja "dane" w Batch Explorer umożliwia kopiowanie plików między lokalnym systemem plików i kontami usługi Azure Storage.

## <a name="client-application-plug-ins"></a>Wtyczki aplikacji klienta

Wtyczki są dostępne dla niektórych aplikacji klienckich.  Wtyczki umożliwiają tworzenie pul i zadań bezpośrednio z poziomu aplikacji lub wywołaj Batch Explorer.

* [Blender 2,79](https://github.com/Azure/azure-batch-rendering/tree/master/plugins/blender)
* [Blender 2.8 +](https://github.com/Azure/azure-batch-rendering/tree/master/plugins/blender28)
* [Autodesk 3ds Max](https://github.com/Azure/azure-batch-rendering/tree/master/plugins/3ds-max)
* [Autodesk Maya](https://github.com/Azure/azure-batch-maya)

## <a name="next-steps"></a>Następne kroki

Przykłady renderowania wsadowego Wypróbuj dwa samouczki:

* [Renderowanie przy użyciu interfejsu wiersza polecenia platformy Azure](./tutorial-rendering-cli.md)
* [Renderowanie przy użyciu programu Batch Explorer](./tutorial-rendering-batchexplorer-blender.md)
