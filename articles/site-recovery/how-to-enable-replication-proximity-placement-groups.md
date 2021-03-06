---
title: Replikowanie maszyn wirtualnych platformy Azure działających w grupach umieszczania w sąsiedztwie
description: Dowiedz się, jak replikować maszyny wirtualne platformy Azure działające w grupach umieszczania zbliżeniowe przy użyciu Azure Site Recovery.
author: Sharmistha-Rai
manager: gaggupta
ms.topic: how-to
ms.date: 05/25/2020
ms.openlocfilehash: bd7619f000d16a55e1cfb31cc3b9cfb3b6d33502
ms.sourcegitcommit: 0b8320ae0d3455344ec8855b5c2d0ab3faa974a3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2020
ms.locfileid: "87432611"
---
# <a name="replicate-azure-virtual-machines-running-in-proximity-placement-groups-to-another-region"></a>Replikowanie maszyn wirtualnych platformy Azure działających w grupach umieszczania w sąsiedztwie w innym regionie

W tym artykule opisano sposób replikowania maszyn wirtualnych, trybu failover i powrotu po awarii działających w grupie umieszczania sąsiedztwa do regionu pomocniczego.

[Grupy umieszczania w sąsiedztwie](../virtual-machines/windows/proximity-placement-groups-portal.md) to funkcja logicznego grupowania maszyn wirtualnych platformy Azure, której można użyć w celu zmniejszenia opóźnień sieci między maszynami wirtualnymi związanymi z aplikacjami. Gdy maszyny wirtualne są wdrażane w ramach tej samej grupy położenia bliskości, są fizycznie zlokalizowane jak najbliżej siebie. Grupy umieszczania zbliżeniowe są szczególnie przydatne do rozwiązywania wymagań dotyczących obciążeń zależnych od opóźnienia.

## <a name="disaster-recovery-with-proximity-placement-groups"></a>Odzyskiwanie po awarii przy użyciu grup umieszczania w sąsiedztwie

W typowym scenariuszu maszyny wirtualne mogą działać w grupie umieszczania bliskości, aby uniknąć opóźnień sieci między różnymi warstwami aplikacji. Chociaż może to zapewnić optymalne opóźnienia sieci aplikacji, należy chronić te aplikacje przy użyciu Site Recovery dla dowolnego błędu poziomu regionu. Site Recovery replikuje dane z jednego regionu do innego regionu platformy Azure i łączy maszyny w regionie odzyskiwania po awarii w przypadku przejścia w tryb failover.

## <a name="considerations"></a>Zagadnienia do rozważenia

- Najlepszym nakładem pracy jest przełączenie w tryb failover i powrót po awarii maszyn wirtualnych do grupy umieszczania sąsiedztwa. Jeśli jednak maszyna wirtualna nie może zostać przełączona w tryb failover lub powrotu po awarii, nastąpi przejście do trybu failover/powrotu po awarii, a maszyny wirtualne zostaną utworzone poza grupą położenia sąsiedztwa.
-  Jeśli zestaw dostępności jest przypięty do grupy umieszczania bliskości, a podczas pracy w trybie failover/powrotu po awarii w zestawie dostępności istnieją ograniczenia alokacji, maszyny wirtualne zostaną utworzone poza zestaw dostępności i grupę umieszczania sąsiedztwa.
-  Site Recovery dla grup umieszczania zbliżeniowe nie jest obsługiwana w przypadku dysków niezarządzanych.

> [!NOTE]
> Azure Site Recovery nie obsługuje powrotu po awarii z dysków zarządzanych dla scenariuszy funkcji Hyper-V do platformy Azure. Z tego powodu powrót po awarii z grupy umieszczania sąsiedztwa na platformie Azure do funkcji Hyper-V nie jest obsługiwany.

## <a name="prerequisites"></a>Wymagania wstępne

1. Upewnij się, że masz Azure PowerShell AZ module. Jeśli musisz zainstalować lub uaktualnić Azure PowerShell, postępuj zgodnie [z tym przewodnikiem, aby zainstalować i skonfigurować Azure PowerShell](/powershell/azure/install-az-ps).
2. Minimalna Azure PowerShell AZ Version powinna być 4.1.0. Aby sprawdzić bieżącą wersję, użyj poniższego polecenia.
    ```
    Get-InstalledModule -Name Az
    ```

## <a name="set-up-site-recovery-for-virtual-machines-in-proximity-placement-group"></a>Skonfiguruj Site Recovery Virtual Machines w grupie umieszczania sąsiedztwa

> [!NOTE]
> Upewnij się, że masz unikatowy identyfikator docelowej grupy umieszczania sąsiedztwa. Jeśli tworzysz nową grupę umieszczania bliskości, sprawdź to polecenie w [tym miejscu](https://docs.microsoft.com/azure/virtual-machines/windows/proximity-placement-groups#create-a-proximity-placement-group) i jeśli używasz istniejącej grupy umieszczania bliskości, a następnie użyj tego polecenia w [tym miejscu](https://docs.microsoft.com/azure/virtual-machines/windows/proximity-placement-groups#list-proximity-placement-groups).

### <a name="azure-to-azure"></a>Azure–Azure

1. [Zaloguj](./azure-to-azure-powershell.md#sign-in-to-your-microsoft-azure-subscription) się do swojego konta i ustaw swoją subskrypcję.
2. Pobierz szczegóły maszyny wirtualnej, którą chcesz replikować, jak opisano w [tym miejscu](./azure-to-azure-powershell.md#get-details-of-the-virtual-machine-to-be-replicated).
3. [Utwórz](./azure-to-azure-powershell.md#create-a-recovery-services-vault) magazyn usługi Recovery Services i [Ustaw](./azure-to-azure-powershell.md#set-the-vault-context) kontekst magazynu.
4. Przygotuj magazyn, aby rozpocząć replikację maszyny wirtualnej. Obejmuje to utworzenie [obiektu usługi Service Fabric](./azure-to-azure-powershell.md#create-a-site-recovery-fabric-object-to-represent-the-primary-source-region) dla regionów głównych i odzyskiwania.
5. [Utwórz](./azure-to-azure-powershell.md#create-a-site-recovery-protection-container-in-the-primary-fabric) kontener ochrony Site Recovery dla sieci szkieletowej podstawowej i odzyskiwania.
6. [Utwórz](./azure-to-azure-powershell.md#create-a-replication-policy) zasady replikacji.
7. Utwórz mapowanie kontenera ochrony między podstawowym i głównym kontenerem ochrony przy użyciu [tych](./azure-to-azure-powershell.md#create-a-protection-container-mapping-between-the-primary-and-recovery-protection-container) kroków oraz mapowania kontenera ochrony dla powrotu po awarii, jak [opisano tutaj](./azure-to-azure-powershell.md#create-a-protection-container-mapping-for-failback-reverse-replication-after-a-failover).
8. Utwórz konto magazynu pamięci podręcznej [, wykonując następujące kroki.](./azure-to-azure-powershell.md#create-cache-storage-account-and-target-storage-account)
9. Utwórz wymagane mapowania sieci zgodnie z opisem w [tym miejscu](./azure-to-azure-powershell.md#create-network-mappings).
10. Aby replikować maszynę wirtualną platformy Azure z dyskami zarządzanymi, użyj poniższego polecenia cmdlet programu PowerShell —

```azurepowershell
#Get the resource group that the virtual machine must be created in when failed over.
$RecoveryRG = Get-AzResourceGroup -Name "a2ademorecoveryrg" -Location "West US 2"

#Specify replication properties for each disk of the VM that is to be replicated (create disk replication configuration)

#OS Disk
$OSdisk = Get-AzDisk -DiskName $OSdiskName -ResourceGroupName $OSdiskResourceGroup
$OSdiskId = $OSdisk.Id
$RecoveryOSDiskAccountType = $OSdisk.Sku.Name
$RecoveryReplicaDiskAccountType = $OSdisk.Sku.Name

$OSDiskReplicationConfig = New-AzRecoveryServicesAsrAzureToAzureDiskReplicationConfig -ManagedDisk -LogStorageAccountId $EastUSCacheStorageAccount.Id -DiskId $OSdiskId -RecoveryResourceGroupId $RecoveryRG.ResourceId -RecoveryReplicaDiskAccountType $RecoveryReplicaDiskAccountType -RecoveryTargetDiskAccountType $RecoveryOSDiskAccountType

#Data disk
$datadisk = Get-AzDisk -DiskName $datadiskName -ResourceGroupName $datadiskResourceGroup
$datadiskId1 = $datadisk[0].Id
$RecoveryReplicaDiskAccountType = $datadisk[0].Sku.Name
$RecoveryTargetDiskAccountType = $datadisk[0].Sku.Name

$DataDisk1ReplicationConfig  = New-AzRecoveryServicesAsrAzureToAzureDiskReplicationConfig -ManagedDisk -LogStorageAccountId $EastUSCacheStorageAccount.Id -DiskId $datadiskId1 -RecoveryResourceGroupId $RecoveryRG.ResourceId -RecoveryReplicaDiskAccountType $RecoveryReplicaDiskAccountType -RecoveryTargetDiskAccountType $RecoveryTargetDiskAccountType

#Create a list of disk replication configuration objects for the disks of the virtual machine that are to be replicated.

$diskconfigs = @()
$diskconfigs += $OSDiskReplicationConfig, $DataDisk1ReplicationConfig

#Start replication by creating replication protected item. Using a GUID for the name of the replication protected item to ensure uniqueness of name.

$TempASRJob = New-AzRecoveryServicesAsrReplicationProtectedItem -AzureToAzure -AzureVmId $VM.Id -Name (New-Guid).Guid -ProtectionContainerMapping $EusToWusPCMapping -AzureToAzureDiskReplicationConfiguration $diskconfigs -RecoveryResourceGroupId $RecoveryRG.ResourceId -RecoveryProximityPlacementGroupId $targetPpg.Id
```
Po pomyślnym zakończeniu operacji uruchamiania replikacji dane maszyny wirtualnej są replikowane do regionu odzyskiwania.

Proces replikacji rozpoczyna się od wstępnego wypełniania kopii dysków replikowanych maszyny wirtualnej w regionie odzyskiwania. Ta faza jest nazywana fazą replikacji początkowej.

Po zakończeniu replikacji początkowej replikacja przechodzi do fazy synchronizacji różnicowej. W tym momencie maszyna wirtualna jest chroniona i można na niej wykonać operację testowania pracy w trybie failover. Stan replikacji replikowanego elementu reprezentujący maszynę wirtualną przechodzi do stanu chronionego po zakończeniu replikacji początkowej.

Monitoruj stan replikacji i kondycję replikacji maszyny wirtualnej, pobierając szczegóły dotyczące chronionego elementu replikacji.

```azurepowershell
Get-AzRecoveryServicesAsrReplicationProtectedItem -ProtectionContainer $PrimaryProtContainer | Select FriendlyName, ProtectionState, ReplicationHealth
```

11. Aby przeprowadzić test pracy w trybie failover, zweryfikuj i oczyść test pracy w trybie failover, wykonaj [następujące](./azure-to-azure-powershell.md#do-a-test-failover-validate-and-cleanup-test-failover) kroki.
12. Aby przełączyć się do trybu failover, wykonaj kroki opisane [tutaj](./azure-to-azure-powershell.md#fail-over-to-azure).
13. Aby ponownie włączyć ochronę i powrót po awarii do regionu źródłowego, użyj poniższego polecenia cmdlet programu PowerShell —

```azurepowershell
#Create Cache storage account for replication logs in the primary region
$WestUSCacheStorageAccount = New-AzStorageAccount -Name "a2acachestoragewestus" -ResourceGroupName "A2AdemoRG" -Location 'West US' -SkuName Standard_LRS -Kind Storage


#Use the recovery protection container, new cache storage account in West US and the source region VM resource group 
Update-AzRecoveryServicesAsrProtectionDirection -ReplicationProtectedItem $ReplicationProtectedItem -AzureToAzure -ProtectionContainerMapping $WusToEusPCMapping -LogStorageAccountId $WestUSCacheStorageAccount.Id -RecoveryResourceGroupID $sourceVMResourcegroup.ResourceId -RecoveryProximityPlacementGroupId $vm.ProximityPlacementGroup.Id
```

14. Aby wyłączyć replikację, wykonaj kroki opisane [tutaj](./azure-to-azure-powershell.md#disable-replication).

### <a name="vmware-to-azure"></a>Z programu VMware do platformy Azure

1. Upewnij się, że [przygotowano lokalne serwery VMware](./vmware-azure-tutorial-prepare-on-premises.md) do odzyskiwania po awarii na platformie Azure.
2. Zaloguj się do swojego konta i ustaw subskrypcję zgodnie z opisem w [tym miejscu](./vmware-azure-disaster-recovery-powershell.md#log-into-azure).
3. [Skonfiguruj](./vmware-azure-disaster-recovery-powershell.md#set-up-a-recovery-services-vault) magazyn Recovery Services i [Ustaw kontekst magazynu](./vmware-azure-disaster-recovery-powershell.md#set-the-vault-context).
4. [Sprawdź poprawność](./vmware-azure-disaster-recovery-powershell.md#validate-vault-registration) rejestracji magazynu.
5. [Utwórz](./vmware-azure-disaster-recovery-powershell.md#create-a-replication-policy) zasady replikacji.
6. [Dodaj](./vmware-azure-disaster-recovery-powershell.md#add-a-vcenter-server-and-discover-vms) serwer vCenter i odnajdź maszyny wirtualne i [Utwórz](./vmware-azure-disaster-recovery-powershell.md#create-storage-accounts-for-replication) konta magazynu na potrzeby replikacji.
7. Aby replikować oprogramowanie VMware Virtual Machines, zapoznaj się z informacjami w tym miejscu i wykonaj następujące polecenie cmdlet programu PowerShell —

```azurepowershell
#Get the target resource group to be used
$ResourceGroup = Get-AzResourceGroup -Name "VMwareToAzureDrPs"

#Get the target virtual network to be used
$RecoveryVnet = Get-AzVirtualNetwork -Name "ASR-vnet" -ResourceGroupName "asrrg"

#Get the protection container mapping for replication policy named ReplicationPolicy
$PolicyMap = Get-AzRecoveryServicesAsrProtectionContainerMapping -ProtectionContainer $ProtectionContainer | where PolicyFriendlyName -eq "ReplicationPolicy"

#Get the protectable item corresponding to the virtual machine CentOSVM1
$VM1 = Get-AzRecoveryServicesAsrProtectableItem -ProtectionContainer $ProtectionContainer -FriendlyName "CentOSVM1"

# Enable replication for virtual machine CentOSVM1 using the Az.RecoveryServices module 2.0.0 onwards to replicate to managed disks
# The name specified for the replicated item needs to be unique within the protection container. Using a random GUID to ensure uniqueness
$Job_EnableReplication1 = New-AzRecoveryServicesAsrReplicationProtectedItem -VMwareToAzure -ProtectableItem $VM1 -Name (New-Guid).Guid -ProtectionContainerMapping $PolicyMap -ProcessServer $ProcessServers[1] -Account $AccountHandles[2] -RecoveryResourceGroupId $ResourceGroup.ResourceId -logStorageAccountId $LogStorageAccount.Id -RecoveryAzureNetworkId $RecoveryVnet.Id -RecoveryAzureSubnetName "Subnet-1" -RecoveryProximityPlacementGroupId $targetPpg.Id
```

8. Można sprawdzić stan replikacji i kondycję replikacji maszyny wirtualnej za pomocą polecenia cmdlet Get-ASRReplicationProtectedItem.

```azurepowershell
Get-AzRecoveryServicesAsrReplicationProtectedItem -ProtectionContainer $ProtectionContainer | Select FriendlyName, ProtectionState, ReplicationHealth
```

9. Skonfiguruj ustawienia trybu failover, wykonując kroki opisane [tutaj](./vmware-azure-disaster-recovery-powershell.md#configure-failover-settings).
10. [Uruchom](./vmware-azure-disaster-recovery-powershell.md#run-a-test-failover) test pracy w trybie failover.
11. Przełączenie do trybu failover na platformie Azure za pomocą [tych](./vmware-azure-disaster-recovery-powershell.md#fail-over-to-azure) kroków.

### <a name="hyper-v-to-azure"></a>Z funkcji Hyper-V do platformy Azure

1. Upewnij się, że [przygotowano lokalne serwery funkcji Hyper-V](./hyper-v-prepare-on-premises-tutorial.md) na potrzeby odzyskiwania po awarii na platformie Azure.
2. [Zaloguj](./hyper-v-azure-powershell-resource-manager.md#step-1-sign-in-to-your-azure-account) się do platformy Azure.
3. [Skonfiguruj](./hyper-v-azure-powershell-resource-manager.md#step-2-set-up-the-vault) magazyn i [Ustaw](./hyper-v-azure-powershell-resource-manager.md#step-3-set-the-recovery-services-vault-context) Recovery Services kontekstu magazynu.
4. [Utwórz](./hyper-v-azure-powershell-resource-manager.md#step-4-create-a-hyper-v-site) lokację funkcji Hyper-V.
5. [Zainstaluj](./hyper-v-azure-powershell-resource-manager.md#step-5-install-the-provider-and-agent) dostawcę i agenta.
6. [Utwórz](./hyper-v-azure-powershell-resource-manager.md#step-6-create-a-replication-policy) zasady replikacji.
7. Włącz replikację, wykonując poniższe kroki: 
    
    a. Pobierz element objęty ochroną, który odnosi się do maszyny wirtualnej, która ma być chroniona, w następujący sposób:

    ```azurepowershell
    $VMFriendlyName = "Fabrikam-app"          #Name of the VM
    $ProtectableItem = Get-AzRecoveryServicesAsrProtectableItem -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName
    ```
    b. Ochrona maszyny wirtualnej. Jeśli do chronionej maszyny wirtualnej jest dołączony więcej niż jeden dysk, określ dysk systemu operacyjnego za pomocą parametru OSDiskName.
    
    ```azurepowershell
    $OSType = "Windows"          # "Windows" or "Linux"
    $DRjob = New-AzRecoveryServicesAsrReplicationProtectedItem -ProtectableItem $VM -Name $VM.Name -ProtectionContainerMapping $ProtectionContainerMapping -RecoveryAzureStorageAccountId   $StorageAccountID -OSDiskName $OSDiskNameList[$i] -OS $OSType -RecoveryResourceGroupId $ResourceGroupID -RecoveryProximityPlacementGroupId $targetPpg.Id
    ```
    c. Poczekaj, aż maszyny wirtualne osiągną stan chroniony po replikacji początkowej. Może to chwilę potrwać, w zależności od takich czynników, jak ilość danych, które mają być replikowane, oraz dostępną przepustowość w strumieniu do platformy Azure. W przypadku stanu chronionego stan zadania i StateDescription są aktualizowane w następujący sposób: 
    
    ```azurepowershell
    $DRjob = Get-AzRecoveryServicesAsrJob -Job $DRjob
    $DRjob | Select-Object -ExpandProperty State

    $DRjob | Select-Object -ExpandProperty StateDescription
    ```
    d. Zaktualizuj właściwości odzyskiwania (takie jak rozmiar roli maszyny wirtualnej) i sieć platformy Azure, do której ma zostać dołączona karta sieciowa maszyny wirtualnej po przejściu w tryb failover.

    ```azurepowershell
    $nw1 = Get-AzVirtualNetwork -Name "FailoverNw" -ResourceGroupName "MyRG"

    $VMFriendlyName = "Fabrikam-App"

    $rpi = Get-AzRecoveryServicesAsrReplicationProtectedItem -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName

    $UpdateJob = Set-AzRecoveryServicesAsrReplicationProtectedItem -InputObject $rpi -PrimaryNic $VM.NicDetailsList[0].NicId -RecoveryNetworkId $nw1.Id -RecoveryNicSubnetName $nw1.Subnets[0].Name

    $UpdateJob = Get-AzRecoveryServicesAsrJob -Job $UpdateJob

    $UpdateJob | Select-Object -ExpandProperty state

    Get-AzRecoveryServicesAsrJob -Job $job | Select-Object -ExpandProperty state
    ```
8. Uruchom test [pracy w trybie failover](./hyper-v-azure-powershell-resource-manager.md#step-8-run-a-test-failover).


## <a name="next-steps"></a>Następne kroki

Aby przeprowadzić ponowne włączenie ochrony i powrotu po awarii dla oprogramowania VMware na platformę Azure, wykonaj kroki opisane [tutaj](./vmware-azure-prepare-failback.md).

Aby przeprowadzić przejście w tryb failover dla funkcji Hyper-V do platformy Azure, wykonaj kroki opisane [tutaj](./site-recovery-failover.md) i aby przeprowadzić powrót po awarii, wykonaj kroki opisane [tutaj](./hyper-v-azure-failback.md).

Aby uzyskać więcej informacji, zobacz [tryb failover w Site Recovery](site-recovery-failover.md).
