---
title: Włączanie Update Management Azure Automation z poziomu maszyny wirtualnej platformy Azure
description: W tym artykule opisano sposób włączania Update Management z maszyny wirtualnej platformy Azure.
services: automation
ms.date: 07/28/2020
ms.topic: conceptual
ms.custom: mvc
ms.openlocfilehash: 27832190125840e367edbfb2db8e4134f98b192d
ms.sourcegitcommit: cee72954f4467096b01ba287d30074751bcb7ff4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2020
ms.locfileid: "87450561"
---
# <a name="enable-update-management-from-an-azure-vm"></a>Włączanie rozwiązania Update Management z poziomu maszyny wirtualnej platformy Azure

W tym artykule opisano, jak można użyć maszyny wirtualnej platformy Azure, aby włączyć funkcję [Update Management](update-mgmt-overview.md) na innych maszynach. Aby włączyć maszyny wirtualne platformy Azure na dużą skalę, należy włączyć istniejącą maszynę wirtualną przy użyciu Update Management.

> [!NOTE]
> Podczas włączania Update Management tylko niektóre regiony są obsługiwane na potrzeby łączenia obszaru roboczego Log Analytics i konta usługi Automation. Aby uzyskać listę obsługiwanych par mapowania, zobacz [Mapowanie regionów dla konta usługi Automation i obszaru roboczego log Analytics](../how-to/region-mappings.md).

## <a name="prerequisites"></a>Wymagania wstępne

* Subskrypcja platformy Azure. Jeśli nie masz subskrypcji, możesz [aktywować korzyści dla subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) lub utworzyć [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
* [Konto usługi Automation](../index.yml) do zarządzania maszynami.
* [Maszyna wirtualna](../../virtual-machines/windows/quick-create-portal.md).

## <a name="sign-in-to-azure"></a>Logowanie do platformy Azure

Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).

## <a name="enable-the-feature-for-deployment"></a>Włącz funkcję wdrożenia

1. W [Azure Portal](https://portal.azure.com)wybierz pozycję **maszyny wirtualne** lub Wyszukaj i wybierz pozycję **maszyny wirtualne** ze strony głównej.

2. Wybierz maszynę wirtualną, dla której chcesz włączyć Update Management. Maszyny wirtualne mogą znajdować się w dowolnym regionie, niezależnie od lokalizacji konta usługi Automation. Ty

3. Na stronie maszyna wirtualna w obszarze **operacje**wybierz pozycję **Update Management**.

4. Musisz mieć `Microsoft.OperationalInsights/workspaces/read` uprawnienie do określenia, czy maszyna wirtualna jest włączona dla obszaru roboczego. Aby dowiedzieć się więcej o dodatkowych uprawnieniach, które są wymagane, zobacz [uprawnienia wymagane do włączania maszyn](../automation-role-based-access-control.md#feature-setup-permissions). Aby dowiedzieć się, jak jednocześnie włączyć wiele komputerów, zobacz [włączanie Update Management z konta usługi Automation](update-mgmt-enable-automation-account.md).

5. Wybierz obszar roboczy Log Analytics i konto usługi Automation, a następnie kliknij pozycję **Włącz** , aby włączyć Update Management. Po włączeniu Update Management może upłynąć około 15 minut, zanim będzie możliwe wyświetlenie oceny aktualizacji z maszyny wirtualnej.

    ![Włączanie rozwiązania Update Management](media/update-mgmt-enable-vm/manageupdates-update-enable.png)

## <a name="next-steps"></a>Następne kroki

* Aby używać Update Management dla maszyn wirtualnych, zobacz [Zarządzanie aktualizacjami i poprawkami dla maszyn wirtualnych platformy Azure](update-mgmt-manage-updates-for-vm.md).

* Aby rozwiązać ogólne błędy Update Management, zobacz [Rozwiązywanie problemów z Update Management](../troubleshoot/update-management.md).
