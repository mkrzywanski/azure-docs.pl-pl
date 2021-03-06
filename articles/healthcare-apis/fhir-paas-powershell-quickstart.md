---
title: 'Szybki Start: wdrażanie interfejsu API platformy Azure dla usługi FHIR przy użyciu programu PowerShell'
description: W tym przewodniku szybki start dowiesz się, jak wdrożyć usługę Azure API for FHIR przy użyciu programu PowerShell.
services: healthcare-apis
author: hansenms
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: quickstart
ms.date: 10/15/2019
ms.author: mihansen
ms.openlocfilehash: d7156543a66cdf50d7cfddec27e685429324f9e6
ms.sourcegitcommit: ea006cd8e62888271b2601d5ed4ec78fb40e8427
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/14/2020
ms.locfileid: "84820240"
---
# <a name="quickstart-deploy-azure-api-for-fhir-using-powershell"></a>Szybki Start: wdrażanie interfejsu API platformy Azure dla usługi FHIR przy użyciu programu PowerShell

W tym przewodniku szybki start dowiesz się, jak wdrożyć usługę Azure API for FHIR przy użyciu programu PowerShell.

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="register-the-azure-api-for-fhir-resource-provider"></a>Rejestrowanie interfejsu API platformy Azure dla dostawcy zasobów FHIR

Jeśli `Microsoft.HealthcareApis` dostawca zasobów nie został jeszcze zarejestrowany dla Twojej subskrypcji, możesz zarejestrować go za pomocą:

```azurepowershell-interactive
Register-AzResourceProvider -ProviderNamespace Microsoft.HealthcareApis
```

## <a name="create-azure-resource-group"></a>Utwórz grupę zasobów platformy Azure

```azurepowershell-interactive
New-AzResourceGroup -Name "myResourceGroupName" -Location westus2
```

## <a name="deploy-azure-api-for-fhir"></a>Wdrażanie usługi Azure API for FHIR

```azurepowershell-interactive
New-AzHealthcareApisService -Name nameoffhirservice -ResourceGroupName myResourceGroupName -Location westus2 -Kind fhir-R4
```

> [!NOTE]
> W zależności od `Az` zainstalowanej wersji modułu programu PowerShell, zainicjowany serwer FHIR może być skonfigurowany do korzystania z [lokalnej kontroli RBAC](configure-local-rbac.md) i mieć aktualnie zalogowany użytkownik programu PowerShell na liście dozwolonych identyfikatorów obiektów tożsamości dla wdrożonej usługi FHIR. W przyszłości zalecamy [Używanie funkcji RBAC platformy Azure](configure-azure-rbac.md) do przypisywania ról płaszczyzny danych i może być konieczne usunięcie tego identyfikatora obiektu użytkownika po wdrożeniu w celu włączenia trybu kontroli RBAC platformy Azure.


## <a name="fetch-capability-statement"></a>Fetch — instrukcja możliwości

Będzie można sprawdzić, czy jest uruchomione konto interfejsu API platformy Azure dla usługi FHIR, pobierając instrukcję możliwości FHIR:

```azurepowershell-interactive
$metadata = Invoke-WebRequest -Uri "https://nameoffhirservice.azurehealthcareapis.com/metadata"
$metadata.RawContent
```

## <a name="clean-up-resources"></a>Czyszczenie zasobów

Jeśli nie chcesz nadal korzystać z tej aplikacji, Usuń grupę zasobów, wykonując następujące czynności:

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroupName
```

## <a name="next-steps"></a>Następne kroki

W tym przewodniku szybki start wdrożono interfejs API platformy Azure dla usługi FHIR w ramach subskrypcji. Aby ustawić dodatkowe ustawienia w interfejsie API platformy Azure dla usługi FHIR, należy przejoć do dodatkowych ustawień przewodnika.

>[!div class="nextstepaction"]
>[Dodatkowe ustawienia w interfejsie API platformy Azure dla usługi FHIR](azure-api-for-fhir-additional-settings.md)
