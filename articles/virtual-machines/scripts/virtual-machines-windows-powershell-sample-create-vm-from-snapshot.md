---
title: Tworzenie maszyny wirtualnej na podstawie migawki (Windows) — przykład programu PowerShell
description: Przykładowy skrypt programu Azure PowerShell — tworzenie maszyny wirtualnej na podstawie migawki
services: virtual-machines-windows
documentationcenter: virtual-machines
author: ramankumarlive
manager: kavithag
editor: ramankum
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/10/2017
ms.author: ramankum
ms.custom: mvc
ms.openlocfilehash: 8ec7b8655a499057e91f138411529a5f271ea511
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/23/2020
ms.locfileid: "87082362"
---
# <a name="create-a-virtual-machine-from-a-snapshot-with-powershell-windows"></a>Tworzenie maszyny wirtualnej na podstawie migawki przy użyciu programu PowerShell (Windows)

Ten skrypt umożliwia utworzenie maszyny wirtualnej na podstawie migawki dysku systemu operacyjnego. 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

 

## <a name="sample-script"></a>Przykładowy skrypt

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-vm-from-snapshot/create-vm-from-snapshot.ps1 "Create VM from managed os disk")]

## <a name="clean-up-deployment"></a>Czyszczenie wdrożenia 

Uruchom następujące polecenie, aby usunąć grupę zasobów, maszynę wirtualną i wszystkie powiązane zasoby.

```powershell
Remove-AzResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Objaśnienia dla skryptu

Ten skrypt używa następujących poleceń w celu uzyskania właściwości migawki, utworzenia dysku zarządzanego na podstawie migawki i utworzenia maszyny wirtualnej. Każda pozycja w tabeli stanowi link do dokumentacji polecenia.

| Polecenie | Uwagi |
|---|---|
| [Get-AzSnapshot](/powershell/module/az.compute/get-azsnapshot) | Pobiera migawkę przy użyciu nazwy migawki. |
| [New-AzDiskConfig](/powershell/module/az.compute/new-azdiskconfig) | Tworzy konfigurację dysku. Ta konfiguracja jest używana podczas procesu tworzenia dysku. |
| [New-AzDisk](/powershell/module/az.compute/new-azdisk) | Tworzy dysk zarządzany. |
| [New-AzVMConfig](/powershell/module/az.compute/new-azvmconfig) | Tworzy konfigurację maszyny wirtualnej. Ta konfiguracja zawiera informacje, takie jak nazwa maszyny wirtualnej, system operacyjny i poświadczenia administracyjne. Jest ona używana podczas tworzenia maszyny wirtualnej. |
| [Set-AzVMOSDisk](/powershell/module/az.compute/set-azvmosdisk) | Dołącza dysk zarządzany jako dysk systemu operacyjnego do maszyny wirtualnej |
| [New-AzPublicIpAddress](/powershell/module/az.network/new-azpublicipaddress) | Tworzy publiczny adres IP. |
| [New-AzNetworkInterface](/powershell/module/az.network/new-aznetworkinterface) | Tworzy interfejs sieciowy. |
| [New-AzVM](/powershell/module/az.compute/new-azvm) | Tworzy maszynę wirtualną. |
|[Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Usuwa grupę zasobów i wszystkie zasoby w niej zawarte. |

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji na temat modułu Azure PowerShell, zobacz [dokumentację programu Azure PowerShell](/powershell/azure/).

Więcej przykładowych skryptów programu PowerShell na potrzeby maszyny wirtualnej można znaleźć w [dokumentacji dotyczącej maszyny wirtualnej platformy Azure z systemem Windows](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
