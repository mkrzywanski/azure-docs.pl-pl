---
title: Migawka wirtualnego dysku twardego w celu utworzenia wielu identycznych dysków zarządzanych (Windows) — PowerShell
description: Przykładowy skrypt programu Azure PowerShell — tworzenie migawki z wirtualnego dysku twardego w celu utworzenia wielu identycznych dysków zarządzanych w krótkim czasie
services: virtual-machines-windows
documentationcenter: storage
author: ramankumarlive
manager: kavithag
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-windows
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 06/05/2017
ms.author: ramankum
ms.openlocfilehash: 0625c968a3c60d38ca2bbe2f13318ccd85d61a61
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/23/2020
ms.locfileid: "87082379"
---
# <a name="create-a-snapshot-from-a-vhd-to-create-multiple-identical-managed-disks-in-small-amount-of-time-with-powershell-windows"></a>Tworzenie migawki z wirtualnego dysku twardego w celu utworzenia wielu identycznych dysków zarządzanych w niewielkim czasie za pomocą programu PowerShell (Windows)

Ten skrypt tworzy migawkę z pliku VHD na koncie magazynu w ramach tej samej lub innej subskrypcji. Ten skrypt umożliwia zaimportowanie wyspecjalizowanego (nieuogólnionego/nieprzetworzonego przez program Sysprep) wirtualnego dysku twardego do migawki, a następnie utworzenie wielu identycznych dysków zarządzanych w krótkim czasie za pomocą migawki. Ponadto za jego pomocą można zaimportować dane wirtualnego dysku twardego do migawki, a następnie przy użyciu migawki utworzyć wiele identycznych dysków zarządzanych w krótkim czasie. 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


 

## <a name="sample-script"></a>Przykładowy skrypt

[!code-powershell[main](../../../powershell_scripts/virtual-machine/create-snapshots-from-vhd-in-different-subscription/create-snapshots-from-vhd-in-different-subscription.ps1 "Create snapshot from VHD")]


## <a name="next-steps"></a>Następne kroki

[Tworzenie dysku zarządzanego na podstawie migawki](virtual-machines-windows-powershell-sample-create-managed-disk-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json)


[Tworzenie maszyny wirtualnej przez dołączenie dysku zarządzanego jako dysku systemu operacyjnego](./virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Aby uzyskać więcej informacji na temat modułu Azure PowerShell, zobacz [dokumentację programu Azure PowerShell](/powershell/azure/).

Więcej przykładowych skryptów programu PowerShell na potrzeby maszyny wirtualnej można znaleźć w [dokumentacji dotyczącej maszyny wirtualnej platformy Azure z systemem Windows](../windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
