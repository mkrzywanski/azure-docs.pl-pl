---
title: Lokalizacja zasobu szablonu
description: Opisuje sposób ustawiania lokalizacji zasobów w szablonie Azure Resource Manager.
ms.topic: conceptual
ms.date: 09/04/2019
ms.custom: devx-track-azurecli
ms.openlocfilehash: e60fa9727ef899c3192c751614736cd1dda5b382
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/31/2020
ms.locfileid: "87504198"
---
# <a name="set-resource-location-in-arm-template"></a>Ustawianie lokalizacji zasobu w szablonie ARM

Podczas wdrażania szablonu Azure Resource Manager (ARM) należy podać lokalizację każdego zasobu. Lokalizacja nie musi być taka sama jak lokalizacja grupy zasobów.

## <a name="get-available-locations"></a>Pobierz dostępne lokalizacje

Różne typy zasobów są obsługiwane w różnych lokalizacjach. Aby uzyskać obsługiwane lokalizacje dla typu zasobu, użyj Azure PowerShell lub interfejsu wiersza polecenia platformy Azure.

# <a name="powershell"></a>[Program PowerShell](#tab/azure-powershell)

```azurepowershell-interactive
((Get-AzResourceProvider -ProviderNamespace Microsoft.Batch).ResourceTypes `
  | Where-Object ResourceTypeName -eq batchAccounts).Locations
```

# <a name="azure-cli"></a>[Interfejs wiersza polecenia platformy Azure](#tab/azure-cli)

```azurecli-interactive
az provider show \
  --namespace Microsoft.Batch \
  --query "resourceTypes[?resourceType=='batchAccounts'].locations | [0]" \
  --out table
```

---

## <a name="use-location-parameter"></a>Użyj parametru Location

Aby zapewnić elastyczność podczas wdrażania szablonu, należy użyć parametru w celu określenia lokalizacji zasobów. Ustaw wartość domyślną parametru na `resourceGroup().location` .

Poniższy przykład przedstawia konto magazynu wdrożone w lokalizacji określonej jako parametr:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageAccountType": {
      "type": "string",
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS",
        "Standard_ZRS",
        "Premium_LRS"
      ],
      "metadata": {
        "description": "Storage Account type"
      }
    },
    "location": {
      "type": "string",
      "defaultValue": "[resourceGroup().location]",
      "metadata": {
        "description": "Location for all resources."
      }
    }
  },
  "variables": {
    "storageAccountName": "[concat('storage', uniquestring(resourceGroup().id))]"
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2018-07-01",
      "name": "[variables('storageAccountName')]",
      "location": "[parameters('location')]",
      "sku": {
        "name": "[parameters('storageAccountType')]"
      },
      "kind": "StorageV2",
      "properties": {}
    }
  ],
  "outputs": {
    "storageAccountName": {
      "type": "string",
      "value": "[variables('storageAccountName')]"
    }
  }
}
```

## <a name="next-steps"></a>Następne kroki

* Aby zapoznać się z pełną listą funkcji szablonu, zobacz [Azure Resource Manager Template Functions](template-functions.md).
* Aby uzyskać więcej informacji na temat plików szablonów, zobacz [Omówienie struktury i składni szablonów usługi ARM](template-syntax.md).
