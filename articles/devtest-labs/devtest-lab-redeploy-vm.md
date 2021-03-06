---
title: Ponowne wdrażanie maszyny wirtualnej w laboratorium w Azure DevTest Labs | Microsoft Docs
description: Dowiedz się, jak ponownie wdrożyć maszynę wirtualną (przechodź z jednego węzła platformy Azure do innego) w Azure DevTest Labs.
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: da0edf13adaa0d7ecd84ee2c190f376c19b398db
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "85480239"
---
# <a name="redeploy-a-vm-in-a-lab-in-azure-devtest-labs"></a>Ponowne wdrażanie maszyny wirtualnej w laboratorium w Azure DevTest Labs
Jeśli nie można nawiązać połączenia z maszyną wirtualną w środowisku laboratoryjnym za pośrednictwem połączenia pulpitu zdalnego, wdróż ponownie MASZYNę wirtualną i ponów próbę nawiązania połączenia z nią. Po ponownym wdrożeniu maszyny wirtualnej DevTest Labs przeniesie maszynę wirtualną z węzła, na którym jest uruchomiona, do nowego węzła w ramach infrastruktury platformy Azure. Następnie uruchamia maszynę wirtualną przy zachowaniu wszystkich opcji konfiguracji i skojarzonych zasobów. Ta funkcja umożliwia zaoszczędzenie czasu poświęcanego na rozwiązywanie problemów z połączeniem pulpitu zdalnego lub dostępem aplikacji do maszyn wirtualnych opartych na systemie Windows w laboratorium. 

## <a name="steps-to-redeploy-a-vm-in-a-lab"></a>Procedura ponownego wdrażania maszyny wirtualnej w laboratorium 
Aby ponownie wdrożyć maszynę wirtualną w laboratorium w Azure DevTest Labs, wykonaj następujące czynności: 

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).
2. Wybierz pozycję **wszystkie usługi**, a następnie z listy wybierz pozycję **DevTest Labs** .
3. Z listy laboratoriów wybierz laboratorium obejmujące maszynę wirtualną, którą chcesz ponownie wdrożyć.  
4. W lewym panelu wybierz pozycję **My Virtual Machines**. 
5. Z listy maszyn wirtualnych wybierz maszynę wirtualną.
6. Na stronie maszyna wirtualna dla maszyny wirtualnej wybierz pozycję **Wdróż** ponownie w obszarze **operacje** w menu po lewej stronie.

    ![Ponowne wdrożenie](media/devtest-lab-redeploy-vm/redeploy.png)
7. Zapoznaj się z informacjami na stronie i wybierz przycisk **Wdróż** ponownie. 9. Sprawdź stan operacji ponownego wdrażania w oknie **powiadomienia** .

    ![Stan ponownego wdrożenia](media/devtest-lab-redeploy-vm/redeploy-status.png)

## <a name="next-steps"></a>Następne kroki
Dowiedz się, jak zmienić rozmiar maszyny wirtualnej w Azure DevTest Labs, zobacz [zmiana rozmiaru maszyny wirtualnej](devtest-lab-resize-vm.md).


