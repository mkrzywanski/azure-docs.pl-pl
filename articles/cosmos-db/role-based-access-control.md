---
title: Kontrola dostępu oparta na rolach w usłudze Azure Cosmos DB
description: Dowiedz się, jak Azure Cosmos DB zapewnia ochronę bazy danych za pomocą integracji z usługą Active Directory (RBAC).
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 06/03/2020
ms.author: mjbrown
ms.openlocfilehash: 6edf5de852ea836de8be02636dd8a971ccebb86d
ms.sourcegitcommit: 3d56d25d9cf9d3d42600db3e9364a5730e80fa4a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/03/2020
ms.locfileid: "87530575"
---
# <a name="role-based-access-control-in-azure-cosmos-db"></a>Kontrola dostępu oparta na rolach w usłudze Azure Cosmos DB

Azure Cosmos DB zapewnia wbudowaną funkcję kontroli dostępu opartej na rolach (RBAC) dla typowych scenariuszy zarządzania w programie Azure Cosmos DB. Osoba mająca profil w Azure Active Directory może przypisywać te role platformy Azure do użytkowników, grup, nazw podmiotów usług lub tożsamości zarządzanych w celu udzielania lub odmawiania dostępu do zasobów i operacji na zasobach Azure Cosmos DB. Przypisania ról są objęte zakresem tylko dostępu do płaszczyzny kontroli, który obejmuje dostęp do kont usługi Azure Cosmos, baz danych, kontenerów i ofert (przepływności).

## <a name="built-in-roles"></a>Wbudowane role

Poniżej przedstawiono wbudowane role obsługiwane przez Azure Cosmos DB:

|**Wbudowana rola**  |**Opis**  |
|---------|---------|
|[Współautor konta DocumentDB](../role-based-access-control/built-in-roles.md#documentdb-account-contributor)|Może zarządzać kontami Azure Cosmos DB.|
|[Cosmos DB czytelnika konta](../role-based-access-control/built-in-roles.md#cosmos-db-account-reader-role)|Może odczytywać Azure Cosmos DB dane konta.|
|[Operator kopii zapasowej Cosmos](../role-based-access-control/built-in-roles.md#cosmosbackupoperator)|Może przesłać żądanie przywracania dla bazy danych lub kontenera usługi Azure Cosmos.|
|[Operator Cosmos DB](../role-based-access-control/built-in-roles.md#cosmos-db-operator)|Program umożliwia inicjowanie obsługi kont, baz danych i kontenerów usługi Azure Cosmos, ale nie ma dostępu do kluczy wymaganych do uzyskania dostępu do danych.|

> [!IMPORTANT]
> Obsługa RBAC w Azure Cosmos DB dotyczy tylko operacji na płaszczyźnie kontroli. Operacje płaszczyzny danych są zabezpieczane przy użyciu kluczy głównych lub tokenów zasobów. Aby dowiedzieć się więcej, zobacz [bezpieczny dostęp do danych w Azure Cosmos DB](secure-access-to-data.md)

## <a name="identity-and-access-management-iam"></a>Zarządzanie tożsamościami i dostępem (IAM)

Okienko **kontroli dostępu (IAM)** w Azure Portal służy do konfigurowania kontroli dostępu opartej na rolach dla zasobów usługi Azure Cosmos. Role są stosowane do użytkowników, grup, podmiotów usługi i tożsamości zarządzanych w Active Directory. Można używać wbudowanych ról lub ról niestandardowych dla użytkowników indywidualnych i grup. Poniższy zrzut ekranu przedstawia Active Directory integrację (RBAC) przy użyciu funkcji kontroli dostępu (IAM) w Azure Portal:

:::image type="content" source="./media/role-based-access-control/database-security-identity-access-management-rbac.png" alt-text="Kontrola dostępu (IAM) w zabezpieczeniach bazy danych Azure Portal.":::

## <a name="custom-roles"></a>Role niestandardowe

Oprócz wbudowanych ról użytkownicy mogą również tworzyć [role niestandardowe](../role-based-access-control/custom-roles.md) na platformie Azure i stosować te role do jednostek usługi we wszystkich subskrypcjach w ramach dzierżawy Active Directory. Role niestandardowe zapewniają użytkownikom sposób tworzenia definicji ról platformy Azure z niestandardowym zestawem operacji dostawcy zasobów. Aby dowiedzieć się, które operacje są dostępne do tworzenia ról niestandardowych dla Azure Cosmos DB Zobacz, [Azure Cosmos DB operacje dostawcy zasobów](../role-based-access-control/resource-provider-operations.md#microsoftdocumentdb)

## <a name="preventing-changes-from-the-azure-cosmos-db-sdks"></a><a id="prevent-sdk-changes"></a>Uniemożliwianie zmian z zestawów SDK Azure Cosmos DB

Dostawcę zasobów Azure Cosmos DB można zablokować, aby zapobiec wprowadzeniu zmian w zasobach z klienta nawiązującego połączenie przy użyciu kluczy konta (czyli aplikacji łączących się za pośrednictwem zestawu Azure Cosmos SDK). Obejmuje to również zmiany wprowadzone w Azure Portal. Ta funkcja może być pożądana dla użytkowników, którzy chcą mieć wyższe stopnie kontroli i zarządzania w środowiskach produkcyjnych. Uniemożliwianie zmian z zestawu SDK włącza również funkcje, takie jak blokady zasobów i dzienniki diagnostyczne dla operacji na płaszczyźnie kontroli. Klienci łączący się z Azure Cosmos DB SDK nie będą mogli zmieniać żadnej właściwości dla kont, baz danych, kontenerów i przepływności usługi Azure Cosmos. Nie ma to wpływu na operacje dotyczące odczytywania i zapisywania danych w kontenerach Cosmos.

Gdy ta funkcja jest włączona, zmiany dowolnego zasobu mogą zostać wykonane tylko przez użytkownika z odpowiednią rolą platformy Azure i poświadczeniami Azure Active Directory, w tym tożsamości usługi zarządzanej.

> [!WARNING]
> Włączenie tej funkcji może mieć wpływ na aplikację. Upewnij się, że rozumiesz wpływ przed jego włączeniem.

### <a name="check-list-before-enabling"></a>Sprawdź listę przed włączeniem

To ustawienie uniemożliwi wszelkie zmiany dowolnego zasobu Cosmos z dowolnego klienta nawiązującego połączenie przy użyciu kluczy konta, w tym zestawu SDK Cosmos DB, wszelkich narzędzi, które łączą się za pośrednictwem kluczy konta lub z Azure Portal. Aby zapobiec występowaniu problemów lub błędom aplikacji po włączeniu tej funkcji, należy sprawdzić, czy aplikacje lub Azure Portal Użytkownicy wykonują dowolne z następujących akcji przed włączeniem tej funkcji, w tym:

- Wszelkie zmiany konta Cosmos, w tym wszelkie właściwości, lub Dodawanie lub usuwanie regionów.

- Tworzenie, usuwanie zasobów podrzędnych, takich jak bazy danych i kontenery. Obejmuje to zasoby dla innych interfejsów API, takich jak Cassandra, MongoDB, Gremlin i zasoby tabeli.

- Aktualizowanie przepływności dla zasobów bazy danych lub na poziomie kontenera.

- Modyfikowanie właściwości kontenera, w tym zasad indeksu, czasu wygaśnięcia i unikatowych kluczy.

- Modyfikowanie procedur składowanych, wyzwalaczy lub funkcji zdefiniowanych przez użytkownika.

Jeśli aplikacje (lub Użytkownicy za pośrednictwem Azure Portal) wykonują dowolne z tych akcji, muszą zostać zmigrowane, aby można je było uruchamiać za pomocą [szablonów ARM](manage-sql-with-resource-manager.md), [programu PowerShell](manage-with-powershell.md), [interfejsu wiersza polecenia platformy Azure](manage-with-cli.md), REST lub [biblioteki zarządzania platformy Azure](https://github.com/Azure-Samples/cosmos-management-net). Należy pamiętać, że zarządzanie platformą Azure jest dostępne w [wielu językach](https://docs.microsoft.com/azure/?product=featured#languages-and-tools).

### <a name="set-via-arm-template"></a>Ustawianie za pomocą szablonu ARM

Aby ustawić tę właściwość przy użyciu szablonu ARM, zaktualizuj istniejący szablon lub wyeksportuj nowy szablon dla bieżącego wdrożenia, a następnie Dołącz `"disableKeyBasedMetadataWriteAccess": true` do właściwości `databaseAccounts` zasobów. Poniżej znajduje się podstawowy przykład szablonu Azure Resource Manager z tym ustawieniem właściwości.

```json
{
    {
      "type": "Microsoft.DocumentDB/databaseAccounts",
      "name": "[variables('accountName')]",
      "apiVersion": "2020-04-01",
      "location": "[parameters('location')]",
      "kind": "GlobalDocumentDB",
      "properties": {
        "consistencyPolicy": "[variables('consistencyPolicy')[parameters('defaultConsistencyLevel')]]",
        "locations": "[variables('locations')]",
        "databaseAccountOfferType": "Standard",
        "disableKeyBasedMetadataWriteAccess": true
        }
    }
}
```

> [!IMPORTANT]
> Upewnij się, że podczas redploying z tą właściwością są uwzględniane inne właściwości konta i zasobów podrzędnych. Nie Wdrażaj tego szablonu, ponieważ spowoduje to zresetowanie wszystkich właściwości konta.

### <a name="set-via-azure-cli"></a>Ustawianie za pośrednictwem interfejsu wiersza polecenia platformy Azure

Aby włączyć korzystanie z interfejsu wiersza polecenia platformy Azure, użyj poniższego przykładu:

```azurecli-interactive
az cosmosdb update  --name [CosmosDBAccountName] --resource-group [ResourceGroupName]  --disable-key-based-metadata-write-access true

```

### <a name="set-via-powershell"></a>Ustaw za pośrednictwem programu PowerShell

Aby włączyć używanie Azure PowerShell, użyj poniższego polecenia:

```azurepowershell-interactive
Update-AzCosmosDBAccount -ResourceGroupName [ResourceGroupName] -Name [CosmosDBAccountName] -DisableKeyBasedMetadataWriteAccess true
```

## <a name="next-steps"></a>Następne kroki

- [Co to jest kontrola dostępu oparta na rolach (Azure RBAC)](../role-based-access-control/overview.md)
- [Role niestandardowe platformy Azure](../role-based-access-control/custom-roles.md)
- [Operacje dostawcy zasobów Azure Cosmos DB](../role-based-access-control/resource-provider-operations.md#microsoftdocumentdb)
