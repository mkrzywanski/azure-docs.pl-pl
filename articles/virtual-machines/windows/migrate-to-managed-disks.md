---
title: Migrowanie maszyn wirtualnych platformy Azure do Managed Disks
description: Przeprowadź migrację maszyn wirtualnych platformy Azure utworzonych przy użyciu dysków niezarządzanych na kontach magazynu, aby użyć Managed Disks.
author: roygara
ms.service: virtual-machines-windows
ms.topic: how-to
ms.date: 05/30/2019
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: d64ebb053ef01e375edd52ad0bf2c1f424f1b837
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "84660822"
---
# <a name="migrate-azure-vms-to-managed-disks-in-azure"></a>Migrowanie maszyn wirtualnych platformy Azure do Managed Disks na platformie Azure

Usługa Azure Managed Disks upraszcza zarządzanie magazynem, eliminując konieczność osobnego zarządzania kontami magazynu.  Możesz również migrować istniejące maszyny wirtualne platformy Azure, aby Managed Disks korzyści z lepszej niezawodności maszyn wirtualnych w zestawie dostępności. Gwarantuje to, że dyski różnych maszyn wirtualnych w zestawie dostępności są wystarczająco odizolowane od siebie, aby uniknąć pojedynczego punktu awarii. Automatycznie umieszcza dyski różnych maszyn wirtualnych w zestawie dostępności w różnych jednostkach skalowania magazynu (sygnatury), które ograniczają wpływ awarii jednostek skalowania pojedynczego magazynu spowodowanych awariami sprzętu i oprogramowania.
Na podstawie Twoich potrzeb można wybierać spośród czterech typów opcji magazynu. Aby dowiedzieć się więcej o dostępnych typach dysków, zapoznaj się z artykułem [Wybieranie typu dysku](disks-types.md)

## <a name="migration-scenarios"></a>Scenariusze migracji

Można migrować do Managed Disks w następujących scenariuszach:

|Scenariusz  |Artykuł  |
|---------|---------|
|Konwertowanie autonomicznych maszyn wirtualnych i maszyn wirtualnych w zestawie dostępności na dyski zarządzane     |[Konwertowanie maszyn wirtualnych do korzystania z dysków zarządzanych](convert-unmanaged-to-managed-disks.md)         |
|Konwertowanie pojedynczej maszyny wirtualnej z klasycznej na Menedżer zasobów na dyskach zarządzanych     |[Tworzenie maszyny wirtualnej na podstawie klasycznego wirtualnego dysku twardego](create-vm-specialized-portal.md)         |
|Konwertuj wszystkie maszyny wirtualne w sieci wirtualnej z klasycznej do Menedżer zasobów na dyskach zarządzanych     |[Migruj zasoby IaaS z klasycznej do Menedżer zasobów](migration-classic-resource-manager-ps.md) a następnie [przekonwertuj maszynę wirtualną z dysków niezarządzanych na dyski zarządzane](convert-unmanaged-to-managed-disks.md)         |
|Uaktualnij maszyny wirtualne ze standardowymi dyskami niezarządzanymi do maszyn wirtualnych z zarządzanymi dyskami Premium     | Najpierw [przekonwertuj maszynę wirtualną z systemem Windows z dysków niezarządzanych na dyski zarządzane](convert-unmanaged-to-managed-disks.md). Następnie [zaktualizuj typ magazynu dla dysku zarządzanego](convert-disk-storage.md).         |

[!INCLUDE [classic-vm-deprecation](../../../includes/classic-vm-deprecation.md)]

## <a name="next-steps"></a>Następne kroki

- Dowiedz się więcej o [Managed disks](managed-disks-overview.md)
- Zapoznaj się z [cennikiem Managed disks](https://azure.microsoft.com/pricing/details/managed-disks/).
