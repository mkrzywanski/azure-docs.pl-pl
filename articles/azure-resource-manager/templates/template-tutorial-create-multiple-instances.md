---
title: Tworzenie wielu wystąpień zasobu
description: Dowiedz się, jak utworzyć szablon usługi Azure Resource Manager w celu utworzenia wielu wystąpień zasobów platformy Azure.
author: mumian
ms.date: 04/23/2020
ms.topic: tutorial
ms.author: jgao
ms.custom: devx-track-azurecli
ms.openlocfilehash: 5e662380c57d20a2e372669dce1bb4c44e75b067
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/31/2020
ms.locfileid: "87496325"
---
# <a name="tutorial-create-multiple-resource-instances-with-arm-templates"></a>Samouczek: Tworzenie wielu wystąpień zasobów przy użyciu szablonów ARM

Dowiedz się, jak wykonać iterację w szablonie Azure Resource Manager (ARM), aby utworzyć wiele wystąpień zasobu platformy Azure. W tym samouczku zmodyfikujesz szablon w celu utworzenia trzech wystąpień konta magazynu.

![Azure Resource Manager tworzy diagram wielu wystąpień](./media/template-tutorial-create-multiple-instances/resource-manager-template-create-multiple-instances-diagram.png)

Ten samouczek obejmuje następujące zadania:

> [!div class="checklist"]
> * Otwieranie szablonu szybkiego startu
> * Edytowanie szablonu
> * Wdrożenie szablonu

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem [utwórz bezpłatne konto](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Wymagania wstępne

Aby ukończyć pracę z tym artykułem, potrzebne są następujące zasoby:

* Program Visual Studio Code z rozszerzeniem Resource Manager Tools. Zobacz [Szybki Start: tworzenie Azure Resource Manager szablonów z Visual Studio Code](quickstart-create-templates-use-visual-studio-code.md).

## <a name="open-a-quickstart-template"></a>Otwieranie szablonu szybkiego startu

[Szablony szybkiego startu platformy Azure](https://azure.microsoft.com/resources/templates/) to repozytorium szablonów usługi ARM. Zamiast tworzyć szablon od podstaw, możesz znaleźć szablon przykładowy i zmodyfikować go. Szablon używany w tym przewodniku Szybki start ma nazwę [Create a standard storage account (Tworzenie standardowego konta magazynu)](https://azure.microsoft.com/resources/templates/101-storage-account-create/). Szablon definiuje zasób konta usługi Azure Storage.

1. W obszarze Visual Studio Code wybierz pozycję **plik** > **Otwórz plik**.
2. W polu **File name (Nazwa pliku)** wklej następujący adres URL:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json
    ```
3. Wybierz pozycję **Open (Otwórz)**, aby otworzyć plik.
4. W szablonie zdefiniowano zasób „Microsoft.Storage/storageAccounts”. Porównaj szablon z [dokumentacją dotyczącą szablonów](/azure/templates/Microsoft.Storage/storageAccounts). Warto uzyskać podstawową wiedzę na temat szablonu przed rozpoczęciem jego dostosowywania.
5. Wybierz pozycję **plik** > **Zapisz jako,** aby zapisać plik jako **azuredeploy.jsna** komputerze lokalnym.

## <a name="edit-the-template"></a>Edytowanie szablonu

Istniejący szablon tworzy jedno konto magazynu. Możesz dostosować szablon, aby utworzyć trzy konta magazynu.

Z poziomu programu Visual Studio Code wprowadź następujące cztery zmiany:

![Usługa Azure Resource Manager tworzy wiele wystąpień](./media/template-tutorial-create-multiple-instances/resource-manager-template-create-multiple-instances.png)

1. Dodaj element `copy` do definicji zasobu konta magazynu. W elemencie copy określ liczbę iteracji i zmienną dla tej pętli. Wartość licznika musi być dodatnią liczbą całkowitą i nie może przekraczać 800.
2. Funkcja `copyIndex()` zwraca bieżącą iterację w pętli. Jako prefiksu nazwy należy użyć indeksu. Funkcja `copyIndex()` rozpoczyna liczenie od zera. Aby przesunąć wartość indeksu, możesz przekazać wartość do funkcji copyIndex(). Na przykład *copyIndex(1)*.
3. Usuń element **variables**, ponieważ nie jest już używany.
4. Usuń element **outputs**. Nie jest już potrzebny.

Ukończony szablon wygląda następująco:

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
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2019-04-01",
      "name": "[concat(copyIndex(),'storage', uniqueString(resourceGroup().id))]",
      "location": "[parameters('location')]",
      "sku": {
        "name": "[parameters('storageAccountType')]"
      },
      "kind": "StorageV2",
      "copy": {
        "name": "storagecopy",
        "count": 3
      },
      "properties": {}
    }
  ]
}
```

Aby uzyskać więcej informacji na temat tworzenia wielu wystąpień, zobacz [wdrażanie wielu wystąpień zasobu lub właściwości w szablonach ARM](./copy-resources.md)

## <a name="deploy-the-template"></a>Wdrożenie szablonu

1. Zaloguj się do [Azure Cloud Shell](https://shell.azure.com)

1. Wybierz preferowane środowisko, wybierając opcję **PowerShell** lub **bash** (dla interfejsu wiersza polecenia) w lewym górnym rogu.  Po przełączeniu wymagane jest ponowne uruchomienie powłoki.

    ![Azure Portal Cloud Shell przekazywania pliku](./media/template-tutorial-use-template-reference/azure-portal-cloud-shell-upload-file.png)

1. Wybierz pozycję **Przekaż/pobierz pliki**, a następnie wybierz pozycję **Przekaż**. Zobacz poprzedni zrzut ekranu. Wybierz plik, który został zapisany w poprzedniej sekcji. Po przekazaniu pliku można użyć polecenia **ls** i **Cat** polecenia, aby sprawdzić, czy plik został pomyślnie przekazany.

1. W Cloud Shell Uruchom następujące polecenia. Wybierz kartę, aby wyświetlić kod programu PowerShell lub kod interfejsu wiersza polecenia.

    # <a name="cli"></a>[Interfejs wiersza polecenia](#tab/CLI)

    ```azurecli
    echo "Enter a project name that is used to generate resource group name:" &&
    read projectName &&
    echo "Enter the location (i.e. centralus):" &&
    read location &&
    resourceGroupName="${projectName}rg" &&
    az group create --name $resourceGroupName --location "$location" &&
    az deployment group create --resource-group $resourceGroupName --template-file "$HOME/azuredeploy.json"
    ```

    # <a name="powershell"></a>[Program PowerShell](#tab/PowerShell)

    ```azurepowershell
    $projectName = Read-Host -Prompt "Enter a project name that is used to generate resource group name"
    $location = Read-Host -Prompt "Enter the location (i.e. centralus)"
    $resourceGroupName = "${projectName}rg"

    New-AzResourceGroup -Name $resourceGroupName -Location "$location"
    New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile "$HOME/azuredeploy.json"
    ```

    ---

Aby wyświetlić wszystkie trzy konta magazynu, pomiń parametr --name:

# <a name="azure-cli"></a>[Interfejs wiersza polecenia platformy Azure](#tab/azure-cli)

```azurecli
echo "Enter a project name that is used to generate resource group name:" &&
read projectName &&
resourceGroupName="${projectName}rg" &&
az storage account list --resource-group $resourceGroupName &&
echo "Press [ENTER] to continue ..."
```

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

```azurepowershell
$projectName = Read-Host -Prompt "Enter a project name that is used to generate resource group name"
$resourceGroupName = "${projectName}rg"

Get-AzStorageAccount -ResourceGroupName $resourceGroupName
Write-Host "Press [ENTER] to continue ..."
```

---

Porównaj nazwy kont magazynu z definicją nazwy w szablonie.

## <a name="clean-up-resources"></a>Czyszczenie zasobów

Gdy zasoby platformy Azure nie będą już potrzebne, wyczyść wdrożone zasoby, usuwając grupę zasobów.

1. Z Azure Portal z menu po lewej stronie wybierz pozycję **Grupa zasobów** .
2. Wprowadź nazwę grupy zasobów w polu **Filtruj według nazwy**.
3. Wybierz nazwę grupy zasobów.  Zostanie wyświetlona łączna liczba trzech zasobów w grupie zasobów.
4. W górnym menu wybierz pozycję **Usuń grupę zasobów** .

## <a name="next-steps"></a>Następne kroki

W tym samouczku przedstawiono sposób tworzenia wielu wystąpień konta magazynu.  W następnym samouczku utworzysz szablon z wieloma zasobami i wieloma typami zasobów. Niektóre zasoby zawierają zasoby zależne.

> [!div class="nextstepaction"]
> [Tworzenie zasobów zależnych](./template-tutorial-create-templates-with-dependent-resources.md)
