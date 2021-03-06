---
title: 'Szybki Start: Tworzenie klastra usług ML przy użyciu szablonu — Azure HDInsight'
description: W tym przewodniku szybki start pokazano, jak za pomocą szablonu Menedżer zasobów utworzyć klaster usług ML w usłudze Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: quickstart
ms.custom: subject-armqs
ms.date: 03/13/2020
ms.openlocfilehash: fd3e541624c2375ed71fb9768bb0c42ec101f47b
ms.sourcegitcommit: 124f7f699b6a43314e63af0101cd788db995d1cb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/08/2020
ms.locfileid: "86087711"
---
# <a name="quickstart-create-ml-services-cluster-in-azure-hdinsight-using-arm-template"></a>Szybki Start: Tworzenie klastra usług ML w usłudze Azure HDInsight przy użyciu szablonu ARM

W tym przewodniku szybki start użyjesz szablonu Azure Resource Manager (szablon ARM), aby utworzyć klaster [usług ml](./r-server-overview.md) w usłudze Azure HDInsight. Microsoft Machine Learning Server jest dostępna jako opcja wdrażania podczas tworzenia klastrów usługi HDInsight na platformie Azure. Typ klastra, który zapewnia tę opcję, nosi nazwę usługi ML. Ta funkcja zapewnia analitykom danych, statystyków i programistom języka R dostęp na żądanie do skalowalnych, dystrybuowanych metod analizy w usłudze HDInsight.

[!INCLUDE [About Azure Resource Manager](../../../includes/resource-manager-quickstart-introduction.md)]

Jeśli Twoje środowisko spełnia wymagania wstępne i masz doświadczenie w korzystaniu z szablonów usługi ARM, wybierz przycisk **Wdróż na platformie Azure** . Szablon zostanie otwarty w Azure Portal.

[![Wdrażanie na platformie Azure](../../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-rserver%2Fazuredeploy.json)

## <a name="prerequisites"></a>Wymagania wstępne

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="review-the-template"></a>Przegląd szablonu

Szablon używany w tym przewodniku szybki start pochodzi z [szablonów szybkiego startu platformy Azure](https://azure.microsoft.com/resources/templates/101-hdinsight-rserver/).

:::code language="json" source="~/quickstart-templates/101-hdinsight-rserver/azuredeploy.json" range="1-171" highlight="49-85":::

Dwa zasoby platformy Azure są zdefiniowane w szablonie:

* [Microsoft. Storage/storageAccounts](/azure/templates/microsoft.storage/storageaccounts): Tworzenie konta usługi Azure Storage.
* [Microsoft. HDInsight/klaster](/azure/templates/microsoft.hdinsight/clusters): Tworzenie klastra usługi HDInsight.

## <a name="deploy-the-template"></a>Wdrożenie szablonu

1. Wybierz przycisk **Wdróż na platformie Azure** poniżej, aby zalogować się do platformy Azure i otworzyć szablon ARM.

    [![Wdrażanie na platformie Azure](../../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-rserver%2Fazuredeploy.json)

1. Wprowadź lub wybierz poniższe wartości:

    |Właściwość |Opis |
    |---|---|
    |Subskrypcja|Z listy rozwijanej wybierz subskrypcję platformy Azure używaną na potrzeby klastra.|
    |Grupa zasobów|Z listy rozwijanej wybierz istniejącą grupę zasobów lub wybierz pozycję **Utwórz nową**.|
    |Lokalizacja|Wartość zostanie automatycznie wypełniona przy użyciu lokalizacji używanej przez grupę zasobów.|
    |Nazwa klastra|Podaj globalnie unikatową nazwę. Dla tego szablonu należy używać tylko małych liter i cyfr.|
    |Nazwa użytkownika logowania klastra|Podaj nazwę użytkownika — wartość domyślna to **admin**.|
    |Hasło logowania klastra|Podaj hasło. Hasło musi składać się z co najmniej 10 znaków i musi zawierać co najmniej jedną cyfrę, jedną wielką i jedną małą literę oraz jeden znak inny niż alfanumeryczny (z wyjątkiem znaków "" "). |
    |Nazwa użytkownika SSH|Podaj nazwę użytkownika, wartość domyślna to sshuser|
    |Hasło ssh|Podaj hasło.|

    ![HBase szablonu Menedżer zasobów](./media/quickstart-resource-manager-template/resource-manager-template-rserver.png)

1. Przejrzyj **warunki i postanowienia**. Następnie wybierz opcję **Zgadzam się na powyższe warunki i**postanowienia, a następnie **Kup**. Otrzymasz powiadomienie, że wdrożenie jest w toku. Utworzenie klastra trwa około 20 minut.

## <a name="review-deployed-resources"></a>Przejrzyj wdrożone zasoby

Po utworzeniu klastra otrzymasz powiadomienie o **pomyślnym wdrożeniu** za pomocą linku **Przejdź do zasobu** . Strona Grupa zasobów będzie zawierać listę nowego klastra usługi HDInsight oraz domyślny magazyn skojarzony z klastrem. Każdy klaster ma konto [usługi Azure Storage](../hdinsight-hadoop-use-blob-storage.md) lub [Azure Data Lake Storage zależności konta](../hdinsight-hadoop-use-data-lake-store.md) . Jest ono określane jako domyślne konto magazynu. Klaster usługi HDInsight i jego domyślne konto magazynu muszą znajdować się w tym samym regionie platformy Azure. Usunięcie klastrów nie powoduje usunięcia konta magazynu.

## <a name="clean-up-resources"></a>Czyszczenie zasobów

Po zakończeniu przewodnika Szybki Start możesz chcieć usunąć klaster. Dzięki usłudze HDInsight dane są przechowywane w usłudze Azure Storage, dzięki czemu można bezpiecznie usunąć klaster, gdy nie jest używany. Opłata jest naliczana również za klaster usługi HDInsight, nawet wtedy, gdy nie jest używana. Ze względu na to, że opłaty za klaster są dużo razy większe niż opłaty za magazyn, sprawia to, że należy usunąć klastry, gdy nie są używane.

W Azure Portal przejdź do klastra, a następnie wybierz pozycję **Usuń**.

![HBase szablonu Menedżer zasobów](./media/quickstart-resource-manager-template/azure-portal-delete-rserver.png)

Dodatkowo możesz wybrać nazwę grupy zasobów, aby otworzyć stronę grupy zasobów, a następnie wybrać pozycję **Usuń grupę zasobów**. Usuwając grupę zasobów, należy usunąć zarówno klaster usługi HDInsight, jak i domyślne konto magazynu.

## <a name="next-steps"></a>Następne kroki

W tym przewodniku szybki start przedstawiono sposób tworzenia klastra usług ML w usłudze HDInsight przy użyciu szablonu ARM. W następnym artykule dowiesz się, jak uruchomić skrypt języka R z serwerem RStudio, który demonstruje użycie platformy Spark dla rozproszonych obliczeń języka R..

> [!div class="nextstepaction"]
> [Wykonywanie skryptu języka R w klastrze usług ML w usłudze Azure HDInsight przy użyciu serwera RStudio](./machine-learning-services-quickstart-job-rstudio.md)
