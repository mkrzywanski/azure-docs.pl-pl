---
title: Centrum zarządzania
description: Zarządzanie połączeniami, konfiguracją kontroli źródła i właściwościami tworzenia globalnego w centrum zarządzania Azure Data Factory
services: data-factory
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
author: djpmsft
ms.author: daperlov
manager: anandsub
ms.date: 06/02/2020
ms.openlocfilehash: 308d19fde78edacebb168b8d4e459169338acc41
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "84346045"
---
# <a name="management-hub-in-azure-data-factory"></a>Centrum zarządzania w Azure Data Factory

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Centrum zarządzania, do którego uzyskuje się dostęp na karcie *Zarządzanie* w Azure Data Factory środowisku użytkownika, jest portalem, który hostuje globalne akcje zarządzania dla fabryki danych. W tym miejscu można zarządzać połączeniami z magazynami danych i obliczeniami zewnętrznymi, konfiguracją kontroli źródła i ustawieniami wyzwalacza.

## <a name="manage-connections"></a>Zarządzanie połączeniami

### <a name="linked-services"></a>Połączone usługi

Połączone usługi definiują informacje o połączeniu dla Azure Data Factory do nawiązywania połączeń z zewnętrznymi magazynami danych i środowiskami obliczeniowymi. Aby uzyskać więcej informacji, zobacz [pojęcia związane z usługami](concepts-linked-services.md). Tworzenie, edytowanie i usuwanie połączonej usługi odbywa się w centrum zarządzania.

![Zarządzanie połączonymi usługami](media/author-management-hub/management-hub-linked-services.png)

### <a name="integration-runtimes"></a>Środowisko Integration Runtime

Środowisko Integration Runtime to infrastruktura obliczeniowa używana przez Azure Data Factory do zapewniania możliwości integracji danych w różnych środowiskach sieciowych. Aby uzyskać więcej informacji, zapoznaj się z [pojęciami dotyczącymi środowiska Integration Runtime](concepts-integration-runtime.md). W centrum zarządzania można tworzyć, usuwać i monitorować środowiska Integration Runtime.

![Zarządzanie środowiskami Integration Runtime](media/author-management-hub/management-hub-integration-runtime.png)

## <a name="manage-source-control"></a>Zarządzanie kontrolą źródła

### <a name="git-configuration"></a>Konfiguracja usługi git

Wyświetlanie i edytowanie skonfigurowanych ustawień repozytorium Git w centrum zarządzania. Aby uzyskać więcej informacji, zapoznaj się z informacjami o [kontroli źródła w Azure Data Factory](source-control.md).

![Zarządzanie repozytorium git](media/author-management-hub/management-hub-git.png)

### <a name="parameterization-template"></a>Szablon parametryzacja

Aby zastąpić wygenerowane parametry szablonu Menedżer zasobów podczas publikowania z gałęzi współpracy, można wygenerować lub edytować plik parametrów niestandardowych. Aby uzyskać więcej informacji, Dowiedz się, jak [używać niestandardowych parametrów w szablonie Menedżer zasobów](continuous-integration-deployment.md#use-custom-parameters-with-the-resource-manager-template). Szablon parametryzacja jest dostępny tylko podczas pracy w repozytorium git. Jeśli *arm-template-parameters-definition.jsw* pliku nie istnieje w gałęzi roboczej, edytowanie szablonu domyślnego spowoduje jego wygenerowanie.

![Zarządzaj niestandardowymi paramsmi](media/author-management-hub/management-hub-custom-parameters.png)

## <a name="manage-authoring"></a>Zarządzanie tworzeniem

### <a name="triggers"></a>Wyzwalacze

Wyzwalacze określają, kiedy przebieg potoku powinien zostać wyłączony. Obecnie wyzwalacze mogą być w harmonogramie zegara ściany, działać w regularnych odstępach czasu lub zależeć od zdarzenia. Aby uzyskać więcej informacji, zapoznaj się z tematem [wykonywanie wyzwalacza](concepts-pipeline-execution-triggers.md#trigger-execution). W centrum zarządzania można utworzyć, edytować, usunąć lub wyświetlić bieżący stan wyzwalacza.

![Zarządzaj niestandardowymi paramsmi](media/author-management-hub/management-hub-triggers.png)

## <a name="next-steps"></a>Następne kroki

Dowiedz się, jak [skonfigurować repozytorium git](source-control.md) w usłudze ADF


