---
title: Wskazówki dotyczące odzyskiwania po awarii dla Azure Data Lake Storage Gen1 | Microsoft Docs
description: Wskazówki dotyczące wysokiej dostępności i odzyskiwania po awarii dla Azure Data Lake Storage Gen1
author: twooley
ms.service: data-lake-store
ms.topic: conceptual
ms.date: 02/21/2018
ms.author: twooley
ms.openlocfilehash: ebcdeed608a5b9dc6202071869c4df1dcfd327a8
ms.sourcegitcommit: 29400316f0c221a43aff3962d591629f0757e780
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/02/2020
ms.locfileid: "87512758"
---
# <a name="high-availability-and-disaster-recovery-guidance-for-data-lake-storage-gen1"></a>Wskazówki dotyczące wysokiej dostępności i odzyskiwania po awarii dla Data Lake Storage Gen1

Data Lake Storage Gen1 zapewnia Magazyn lokalnie nadmiarowy (LRS). W związku z tym dane na koncie Data Lake Storage Gen1 są odporne na przejściowe awarie sprzętowe w centrum danych za pomocą automatycznych replik. Zapewnia to trwałość i wysoką dostępność, spełniając Data Lake Storage Gen1 umowy SLA. Ten artykuł zawiera wskazówki dotyczące dalszej ochrony danych przed rzadkimi przypadkami wypadków obejmującymi cały region lub przypadkowe usunięcie.

## <a name="disaster-recovery-guidance"></a>Wskazówki dotyczące odzyskiwania po awarii

Należy przygotować plan odzyskiwania po awarii. Zapoznaj się z informacjami w tym artykule i tymi dodatkowymi zasobami, które pomogą Ci utworzyć własny plan.

* [Odzyskiwanie aplikacji platformy Azure po awarii i ich wysoka dostępność](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md)
* [Odporność platformy Azure — wskazówki techniczne](../resiliency/resiliency-technical-guidance.md)

### <a name="best-practice-recommendations"></a>Zalecenia dotyczące najlepszych rozwiązań

Zalecamy skopiowanie danych o kluczowym znaczeniu do innego konta Data Lake Storage Gen1 w innym regionie o częstotliwości dopasowanej do potrzeb planu odzyskiwania po awarii. Istnieją różne metody kopiowania danych, w tym [ADLCopy](data-lake-store-copy-data-azure-storage-blob.md), [Azure PowerShell](data-lake-store-get-started-powershell.md)lub [Azure Data Factory](../data-factory/connector-azure-data-lake-store.md). Azure Data Factory to usługa przydatna w przypadku cyklicznego tworzenia i wdrażania potoków przepływu danych.

Jeśli wystąpi awaria regionalna, można uzyskać dostęp do danych w regionie, w którym dane zostały skopiowane. [Pulpit nawigacyjny Azure Service Health](https://azure.microsoft.com/status/) można monitorować, aby określić stan usługi platformy Azure na całym świecie.

## <a name="data-corruption-or-accidental-deletion-recovery-guidance"></a>Uszkodzenie lub przypadkowe usunięcie danych — wskazówki

Chociaż Data Lake Storage Gen1 zapewnia odporność danych za pośrednictwem zautomatyzowanych replik, nie uniemożliwia to aplikacji (lub deweloperów/użytkowników) uszkadzania danych ani ich przypadkowego usunięcia.

Aby zapobiec przypadkowemu usunięciu, zalecamy najpierw ustawić odpowiednie zasady dostępu dla konta usługi Data Lake Storage Gen1. Obejmuje to zastosowanie [blokad zasobów platformy Azure](../azure-resource-manager/management/lock-resources.md) w celu zablokowania ważnych zasobów i zastosowania kontroli dostępu na poziomie konta i pliku przy użyciu dostępnych [funkcji zabezpieczeń Data Lake Storage Gen1](data-lake-store-security-overview.md). Zalecamy również, aby regularnie tworzyć kopie krytycznych danych przy użyciu [ADLCopy](data-lake-store-copy-data-azure-storage-blob.md), [Azure PowerShell](data-lake-store-get-started-powershell.md) lub [Azure Data Factory](../data-factory/connector-azure-data-lake-store.md) w innym Data Lake Storage Gen1 koncie, folderze lub subskrypcji platformy Azure. Umożliwi to odzyskanie danych po ich uszkodzeniu lub usunięciu. Azure Data Factory to usługa przydatna w przypadku cyklicznego tworzenia i wdrażania potoków przepływu danych.

Możesz również włączyć [rejestrowanie diagnostyczne](data-lake-store-diagnostic-logs.md) dla konta Data Lake Storage Gen1, aby zebrać ślady inspekcji dostępu do danych. Dziennik inspekcji zawiera informacje o tym, kto może usunąć lub zaktualizować plik.

Można spróbować przywrócić usunięty element przy użyciu polecenia [AZ. kontach datalakestore](https://docs.microsoft.com/powershell/module/az.datalakestore/) Azure PowerShell module dla Data Lake Storage Gen 1. W celu zapoznaj się z tematem polecenie [Restore-AzDataLakeStoreDeletedItem](https://docs.microsoft.com/powershell/module/az.datalakestore/restore-azdatalakestoredeleteditem) . Przed podjęciem próby użycia tego polecenia Pamiętaj, aby przejrzeć sekcję [Opis](https://docs.microsoft.com/powershell/module/az.datalakestore/restore-azdatalakestoredeleteditem#description) .

## <a name="next-steps"></a>Następne kroki

* [Wprowadzenie do Data Lake Storage Gen1](data-lake-store-get-started-portal.md)
* [Zabezpieczanie danych w usłudze Data Lake Storage 1. generacji](data-lake-store-secure-data.md)
