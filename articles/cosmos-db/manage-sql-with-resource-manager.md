---
title: Tworzenie Azure Cosmos DB z szablonami Menedżer zasobów i zarządzanie nimi
description: Używanie szablonów Azure Resource Manager do tworzenia i konfigurowania interfejsu API Azure Cosmos DB dla podstawowego (SQL)
author: markjbrown
ms.service: cosmos-db
ms.topic: how-to
ms.date: 06/19/2020
ms.author: mjbrown
ms.openlocfilehash: 2b4a572abec8007fe6f1c7e963be19d28c7b48d6
ms.sourcegitcommit: 0100d26b1cac3e55016724c30d59408ee052a9ab
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/07/2020
ms.locfileid: "86028153"
---
# <a name="manage-azure-cosmos-db-core-sql-api-resources-with-azure-resource-manager-templates"></a>Zarządzanie zasobami interfejsu API Azure Cosmos DB Core (SQL) za pomocą szablonów Azure Resource Manager

W tym artykule dowiesz się, jak używać szablonów Azure Resource Manager, aby ułatwić wdrażanie kont Azure Cosmos DB, baz danych i kontenerów oraz zarządzanie nimi.

W tym artykule przedstawiono tylko przykłady Azure Resource Manager szablonów dla podstawowych kont interfejsu API (SQL). Możesz również znaleźć przykłady szablonów dla interfejsów API [Cassandra](manage-cassandra-with-resource-manager.md), [Gremlin](manage-gremlin-with-resource-manager.md), [MongoDB](manage-mongodb-with-resource-manager.md)i [Table](manage-table-with-resource-manager.md) .

> [!IMPORTANT]
>
> * Nazwy kont są ograniczone do 44 znaków, wszystkie małe litery.
> * Aby zmienić wartości przepływności, wdróż ponownie szablon z zaktualizowanymi jednostkami RU/s.
> * Po dodaniu lub usunięciu lokalizacji na koncie usługi Azure Cosmos nie można jednocześnie modyfikować innych właściwości. Te operacje należy wykonać oddzielnie.

Aby utworzyć dowolny z poniższych zasobów Azure Cosmos DB, Skopiuj poniższy przykładowy szablon do nowego pliku JSON. Opcjonalnie można utworzyć plik JSON parametrów do użycia podczas wdrażania wielu wystąpień tego samego zasobu z różnymi nazwami i wartościami. Istnieje wiele sposobów wdrażania szablonów Azure Resource Manager, w tym [Azure Portal](../azure-resource-manager/templates/deploy-portal.md), [interfejsu wiersza polecenia platformy Azure](../azure-resource-manager/templates/deploy-cli.md), [Azure PowerShell](../azure-resource-manager/templates/deploy-powershell.md) i [GitHub](../azure-resource-manager/templates/deploy-to-azure-button.md).

<a id="create-autoscale"></a>

## <a name="azure-cosmos-account-with-autoscale-throughput"></a>Konto usługi Azure Cosmos z przepływności automatycznego skalowania

Ten szablon służy do tworzenia konta usługi Azure Cosmos w dwóch regionach z opcjami spójności i przełączania w tryb failover, w których baza danych i kontener skonfigurowany pod kątem przepływności automatycznego skalowania z włączonymi większością opcji zasad. Ten szablon jest również dostępny dla jednego kliknięcia przycisku Wdróż z galerii szablonów szybkiego startu platformy Azure.

[:::image type="content" source="../media/template-deployments/deploy-to-azure.svg" alt-text="Wdróż na platformie Azure":::](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-cosmosdb-sql-autoscale%2Fazuredeploy.json)

:::code language="json" source="~/quickstart-templates/101-cosmosdb-sql-autoscale/azuredeploy.json":::

<a id="create-analytical-store"></a>

## <a name="azure-cosmos-account-with-analytical-store"></a>Konto usługi Azure Cosmos z magazynem analitycznym

Ten szablon tworzy konto usługi Azure Cosmos w jednym regionie z kontenerem z włączonym analitycznym czasem TTL i opcjami dla przepływności ręcznej lub skalowania automatycznego. Ten szablon jest również dostępny dla jednego kliknięcia przycisku Wdróż z galerii szablonów szybkiego startu platformy Azure.

[:::image type="content" source="../media/template-deployments/deploy-to-azure.svg" alt-text="Wdróż na platformie Azure":::](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-cosmosdb-sql-analytical-store%2Fazuredeploy.json)

:::code language="json" source="~/quickstart-templates/101-cosmosdb-sql-analytical-store/azuredeploy.json":::

<a id="create-manual"></a>

## <a name="azure-cosmos-account-with-standard-provisioned-throughput"></a>Konto usługi Azure Cosmos z zainicjowaną standardową przepływność

Ten szablon służy do tworzenia konta usługi Azure Cosmos w dwóch regionach z opcjami spójności i pracy awaryjnej, z bazą danych i kontenerem skonfigurowanym dla standardowej przepływności z włączonymi większością opcji zasad. Ten szablon jest również dostępny dla jednego kliknięcia przycisku Wdróż z galerii szablonów szybkiego startu platformy Azure.

[:::image type="content" source="../media/template-deployments/deploy-to-azure.svg" alt-text="Wdróż na platformie Azure":::](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-cosmosdb-sql%2Fazuredeploy.json)

:::code language="json" source="~/quickstart-templates/101-cosmosdb-sql/azuredeploy.json":::

<a id="create-sproc"></a>

## <a name="azure-cosmos-db-container-with-server-side-functionality"></a>Kontener Azure Cosmos DB z funkcjami po stronie serwera

Ten szablon służy do tworzenia konta usługi Azure Cosmos, bazy danych i kontenera z użyciem procedury składowanej, wyzwalacza i funkcji zdefiniowanej przez użytkownika. Ten szablon jest również dostępny dla jednego kliknięcia przycisku Wdróż z galerii szablonów szybkiego startu platformy Azure.

[:::image type="content" source="../media/template-deployments/deploy-to-azure.svg" alt-text="Wdróż na platformie Azure":::](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-cosmosdb-sql-container-sprocs%2Fazuredeploy.json)

:::code language="json" source="~/quickstart-templates/101-cosmosdb-sql-container-sprocs/azuredeploy.json":::

<a id="free-tier"></a>

## <a name="free-tier-azure-cosmos-db-account"></a>Konto Azure Cosmos DB warstwy bezpłatnej

Ten szablon służy do tworzenia bezpłatnego konta usługi Azure Cosmos i bazy danych z udostępnioną przepływem wydajności, które można udostępnić do 25 kontenerów. Ten szablon jest również dostępny dla jednego kliknięcia przycisku Wdróż z galerii szablonów szybkiego startu platformy Azure.

[:::image type="content" source="../media/template-deployments/deploy-to-azure.svg" alt-text="Wdróż na platformie Azure":::](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-cosmosdb-free%2Fazuredeploy.json)

:::code language="json" source="~/quickstart-templates/101-cosmosdb-free/azuredeploy.json":::

## <a name="next-steps"></a>Następne kroki

Oto kilka dodatkowych zasobów:

* [Dokumentacja Azure Resource Manager](/azure/azure-resource-manager/)
* [Schemat dostawcy zasobów Azure Cosmos DB](/azure/templates/microsoft.documentdb/allversions)
* [Azure Cosmos DB Szablony szybkiego startu](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Documentdb&pageNumber=1&sort=Popular)
* [Rozwiązywanie typowych błędów wdrażania Azure Resource Manager](../azure-resource-manager/templates/common-deployment-errors.md)
