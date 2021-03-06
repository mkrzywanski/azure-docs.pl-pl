---
title: Microsoft Azure Stack Edge — Omówienie | Microsoft Docs
description: Opisuje Azure Stack Edge, rozwiązanie magazynu, które korzysta z urządzenia fizycznego na potrzeby transferu sieciowego na platformę Azure.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: overview
ms.date: 11/04/2019
ms.author: alkohli
ms.openlocfilehash: c44d15e36135b8cf8c97484718d4600f8ba03521
ms.sourcegitcommit: 856db17a4209927812bcbf30a66b14ee7c1ac777
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/29/2020
ms.locfileid: "82571112"
---
# <a name="what-is-azure-stack-edge"></a>Co to jest rozwiązanie Azure Stack Edge?

[!INCLUDE [data-box-edge-gateway-rename-note](../../includes/data-box-edge-gateway-rename-note.md)]

Azure Stack Edge to urządzenie obliczeniowe z systemem AI z możliwością transferu danych w sieci. Ten artykuł zawiera omówienie rozwiązania Azure Stack Edge, korzyści, kluczowych możliwości i scenariuszy, w których można wdrożyć to urządzenie.

Azure Stack Edge to rozwiązanie typu "sprzęt jako usługa". Firma Microsoft dostarcza urządzenie zarządzane przez chmurę z wbudowaną, programowalną bramą Gateway Array (FPGA), która umożliwia szybsze działanie systemu AI-inferencing i ma wszystkie możliwości bramy magazynu sieciowego. 

## <a name="use-cases"></a>Przypadki zastosowań

Poniżej przedstawiono różne scenariusze, w których Azure Stack Edge może być używana do szybkiego Machine Learning (ML) inferencing na granicy i przetwarzania wstępnego danych przed wysłaniem ich do platformy Azure.

- **Wnioskowanie z Azure Machine Learning** — z Azure Stack Edge, można uruchamiać modele ml, aby uzyskać szybkie wyniki, które mogą być przetwarzane przed wysłaniem danych do chmury. Pełny zestaw danych można opcjonalnie przesłać, aby kontynuować ponowne uczenie i ulepszanie modeli ML. Aby uzyskać więcej informacji na temat korzystania z wydajnych modeli sprzętu Azure ML na urządzeniu Azure Stack Edge, zobacz [wdrażanie przyspieszanych modeli sprzętu Azure ml na Azure Stack Edge](https://docs.microsoft.com/azure/machine-learning/how-to-deploy-fpga-web-service#deploy-to-a-local-edge-server).

- Wstępnie **Przetwarzaj** dane przekształceń danych przed wysłaniem ich do platformy Azure w celu utworzenia bardziej funkcjonalnego zestawu danych. Wstępne przetwarzanie umożliwia: 

    - Agregowanie danych.
    - Zmodyfikuj dane, na przykład aby usunąć dane osobowe.
    - Podzbiór danych w celu zoptymalizowania magazynu i przepustowości lub do dalszej analizy.
    - Analizowanie zdarzeń IoT i reagowanie na nie. 

- **Transferowanie danych za pośrednictwem sieci na platformę Azure** — umożliwia łatwe i szybkie przenoszenie danych do platformy Azure, dzięki czemu można w łatwy sposób tworzyć i przetwarzać dane oraz Azure Stack korzystać z nich na potrzeby archiwizowania. 

## <a name="key-capabilities"></a>Najważniejsze możliwości

Azure Stack Edge ma następujące możliwości:

|Możliwość |Opis  |
|---------|---------|
|Przyspieszone inferencing AI| Włączone przez wbudowaną FPGA.|
|Obliczanie       |Umożliwia analizowanie, przetwarzanie i filtrowanie danych.|
|Wysoka wydajność | Obliczenia o wysokiej wydajności i transfery danych.|
|Dostęp do danych     | Bezpośredni dostęp do danych z usług Azure Storage Blob i Azure Files przy użyciu interfejsów API w chmurze w celu dodatkowego przetwarzania danych w chmurze. Lokalna pamięć podręczna na urządzeniu służy do szybkiego dostępu do ostatnio używanych plików.|
|Zarządzane przez chmurę     |Urządzenia i usługi są zarządzane za pośrednictwem Azure Portal.  |
|Przekazywanie w trybie offline     | Tryb bez połączenia obsługuje scenariusze przekazywania w trybie offline.|
|Obsługiwane protokoły     | Obsługa standardowych protokołów SMB i NFS do pozyskiwania danych. <br> Aby uzyskać więcej informacji na temat obsługiwanych wersji, zobacz [Azure Stack Edge wymagania systemowe](azure-stack-edge-system-requirements.md).|
|Odświeżanie danych     | Możliwość odświeżania plików lokalnych za pomocą najnowszych wersji z chmury.|
|Szyfrowanie    | Obsługa funkcji BitLocker w celu lokalnego szyfrowania danych i zabezpieczania transferu danych do chmury za pośrednictwem protokołu *https*.|
|Ograniczanie przepustowości| Ograniczanie użycia przepustowości w godzinach szczytu.|
|ExpressRoute | Dodano zabezpieczenia przez ExpressRoute. Użyj konfiguracji komunikacji równorzędnej, w której ruch z urządzeń lokalnych do punktów końcowych magazynu w chmurze jest przekazywany przez ExpressRoute. Aby uzyskać więcej informacji, zobacz [Omówienie usługi ExpressRoute](../expressroute/expressroute-introduction.md).

## <a name="components"></a>Składniki

Rozwiązanie Azure Stack Edge składa się z Azure Stack zasobów brzegowych, Azure Stack Edge urządzenia fizycznego oraz lokalnego interfejsu użytkownika sieci Web.

* **Urządzenie fizyczne Azure Stack Edge** — serwer montowany w stojaku o rozmiarze 1U dostarczony przez firmę Microsoft, który można skonfigurować do wysyłania danych do platformy Azure.
    
* **Azure Stack Edge** — zasób w Azure Portal, który umożliwia zarządzanie Azure Stack urządzeniem brzegowym z poziomu interfejsu sieci Web, do którego można uzyskać dostęp z różnych lokalizacji geograficznych. Użyj zasobu brzegowego Azure Stack, aby tworzyć i zarządzać zasobami, wyświetlać i zarządzać urządzeniami oraz alertami oraz zarządzać udziałami.  

    <!--![The Azure Stack Edge service in Azure portal](media/data-box-overview/data-box-Edge-service1.png)-->

    Aby uzyskać więcej informacji, przejdź do [pozycji Utwórz zamówienie dla urządzenia brzegowego Azure Stack](azure-stack-edge-deploy-prep.md#create-a-new-resource).

* **Azure Stack krawędź lokalnego interfejsu użytkownika sieci Web** — Użyj lokalnego interfejsu użytkownika sieci Web, aby uruchomić diagnostykę, zamknąć i ponownie uruchomić Azure Stack Urządzenie brzegowe, wyświetlić dzienniki kopiowania i skontaktować się z pomoc techniczna firmy Microsoft w celu zarejestrowania żądania obsługi.

    <!--![The Azure Stack Edge local web UI](media/data-box-Edge-overview/data-box-Edge-local-web-ui.png)-->

    Aby uzyskać informacje na temat korzystania z interfejsu użytkownika opartego na sieci Web, przejdź do [witryny sieci Web za pomocą interfejsu użytkownika w celu administrowania Azure Stack krawędzią](azure-stack-edge-manage-access-power-connectivity-mode.md).

## <a name="region-availability"></a>Dostępność w danym regionie

Azure Stack Edge urządzenie fizyczne, zasób platformy Azure i docelowe konto magazynu, do którego są przesyłane dane, nie muszą znajdować się w tym samym regionie.

- **Dostępność zasobów** — Aby uzyskać listę wszystkich regionów, w których jest dostępny zasób Azure Stack Edge, zobacz Dostępność [produktów platformy Azure według regionów](https://azure.microsoft.com/global-infrastructure/services/?products=databox&regions=all). Azure Stack Edge można także wdrożyć w chmurze Azure Government. Aby uzyskać więcej informacji, zobacz [co to jest Azure Government?](https://docs.microsoft.com/azure/azure-government/documentation-government-welcome).
    
- **Docelowe konta magazynu** — konta magazynu, w których są przechowywane dane, są dostępne we wszystkich regionach platformy Azure. Regiony, w których przechowywane są konta magazynu Azure Stack dane brzegowe, powinny znajdować się w pobliżu lokalizacji urządzenia w celu uzyskania optymalnej wydajności. Duża odległość konta magazynu od urządzenia powoduje długie opóźnienia i mniejszą wydajność.

## <a name="next-steps"></a>Następne kroki

- Przejrzyj [wymagania systemowe Azure Stack Edge](azure-stack-edge-system-requirements.md).
- Zapoznaj się z [limitami Azure Stack krawędzi](azure-stack-edge-limits.md).
- Wdróż [Azure Stack Edge](azure-stack-edge-deploy-prep.md) w Azure Portal.
