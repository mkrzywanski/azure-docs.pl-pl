---
title: Wersja zapoznawcza — Tworzenie wersji obrazu zaszyfrowanej przy użyciu własnych kluczy
description: Utwórz wersję obrazu w galerii obrazów udostępnionych przy użyciu kluczy szyfrowania zarządzanych przez klienta.
author: cynthn
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: how-to
ms.date: 05/06/2020
ms.author: cynthn
ms.openlocfilehash: 469e225a1cc40dc2ecc45339d9355484e87c4af2
ms.sourcegitcommit: f844603f2f7900a64291c2253f79b6d65fcbbb0c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/10/2020
ms.locfileid: "86223588"
---
# <a name="preview-use-customer-managed-keys-for-encrypting-images"></a>Wersja zapoznawcza: Używanie kluczy zarządzanych przez klienta do szyfrowania obrazów

Obrazy z galerii są przechowywane jako dyski zarządzane, dzięki czemu są one automatycznie szyfrowane za pomocą szyfrowania po stronie serwera. Szyfrowanie po stronie serwera używa 256-bitowego [szyfrowania AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard), jednego z najsilniejszych szyfrów blokowych i jest zgodne ze standardem FIPS 140-2. Aby uzyskać więcej informacji na temat modułów kryptograficznych związanych z dyskami zarządzanymi platformy Azure, zobacz [interfejs API kryptografii: Kolejna generacja](/windows/desktop/seccng/cng-portal)

Możesz polegać na kluczach zarządzanych przez platformę do szyfrowania obrazów lub można zarządzać szyfrowaniem przy użyciu własnych kluczy. Jeśli zdecydujesz się na zarządzanie szyfrowaniem przy użyciu własnych kluczy, możesz określić *klucz zarządzany przez klienta* , który będzie używany do szyfrowania i odszyfrowywania wszystkich dysków w obrazie. 

Szyfrowanie po stronie serwera przy użyciu kluczy zarządzanych przez klienta używa Azure Key Vault. Możesz zaimportować [klucze RSA](../key-vault/keys/hsm-protected-keys.md) do Key Vault lub wygenerować nowe klucze rsa w Azure Key Vault.

Aby używać kluczy zarządzanych przez klienta dla obrazów, najpierw musisz mieć Azure Key Vault. Następnie utworzysz zestaw szyfrowania dysku. Zestaw szyfrowania dysków jest używany podczas tworzenia wersji obrazu.

Aby uzyskać więcej informacji na temat tworzenia i używania zestawów szyfrowania dysków, zobacz [klucze zarządzane przez klienta](./windows/disk-encryption.md#customer-managed-keys).

## <a name="limitations"></a>Ograniczenia

Istnieje kilka ograniczeń dotyczących używania kluczy zarządzanych przez klienta do szyfrowania obrazów udostępnionej galerii obrazów:  

- Zestawy kluczy szyfrowania muszą znajdować się w tej samej subskrypcji i regionie co obraz.

- Nie można udostępniać obrazów, które używają kluczy zarządzanych przez klienta. 

- Nie można replikować obrazów, które używają kluczy zarządzanych przez klienta, do innych regionów.

- Po zastosowaniu własnych kluczy do zaszyfrowania dysku lub obrazu nie można wrócić do korzystania z kluczy zarządzanych przez platformę do szyfrowania tych dysków lub obrazów.


> [!IMPORTANT]
> Szyfrowanie przy użyciu kluczy zarządzanych przez klienta jest obecnie dostępne w publicznej wersji zapoznawczej.
> Ta wersja zapoznawcza nie jest objęta umową dotyczącą poziomu usług i nie zalecamy korzystania z niej w przypadku obciążeń produkcyjnych. Niektóre funkcje mogą być nieobsługiwane lub ograniczone. Aby uzyskać więcej informacji, zobacz [Uzupełniające warunki korzystania z wersji zapoznawczych platformy Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).


## <a name="powershell"></a>PowerShell

Aby uzyskać publiczną wersję zapoznawczą, należy najpierw zarejestrować tę funkcję.

```azurepowershell-interactive
Register-AzProviderFeature -FeatureName SIGEncryption -ProviderNamespace Microsoft.Compute
```

Ukończenie rejestracji trwa kilka minut. Użyj Get-AzProviderFeature, aby sprawdzić stan rejestracji funkcji.

```azurepowershell-interactive
Get-AzProviderFeature -FeatureName SIGEncryption -ProviderNamespace Microsoft.Compute
```

Gdy RegistrationState zwraca zarejestrowany, możesz przejść do następnego kroku.

Sprawdź rejestrację dostawcy. Upewnij się, że zwraca `Registered` .

```azurepowershell-interactive
Get-AzResourceProvider -ProviderNamespace Microsoft.Compute | Format-table -Property ResourceTypes,RegistrationState
```

Jeśli nie zwróci tego `Registered` , użyj następującej czynności, aby zarejestrować dostawców:

```azurepowershell-interactive
Register-AzResourceProvider -ProviderNamespace Microsoft.Compute
```

Aby określić szyfrowanie dysku ustawione dla wersji obrazu, użyj polecenie [New-AzGalleryImageDefinition](/powershell/module/az.compute/new-azgalleryimageversion) z `-TargetRegion` parametrem. 

```azurepowershell-interactive

$sourceId = <ID of the image version source>

$osDiskImageEncryption = @{DiskEncryptionSetId='subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myRG/providers/Microsoft.Compute/diskEncryptionSets/myDESet'}

$dataDiskImageEncryption1 = @{DiskEncryptionSetId='subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myRG/providers/Microsoft.Compute/diskEncryptionSets/myDESet1';Lun=1}

$dataDiskImageEncryption2 = @{DiskEncryptionSetId='subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myRG/providers/Microsoft.Compute/diskEncryptionSets/myDESet2';Lun=2}

$dataDiskImageEncryptions = @($dataDiskImageEncryption1,$dataDiskImageEncryption2)

$encryption1 = @{OSDiskImage=$osDiskImageEncryption;DataDiskImages=$dataDiskImageEncryptions}

$region1 = @{Name='West US';ReplicaCount=1;StorageAccountType=Standard_LRS;Encryption=$encryption1}

$targetRegion = @{$region1}


# Create the image
New-AzGalleryImageVersion `
   -ResourceGroupName $rgname `
   -GalleryName $galleryName `
   -GalleryImageDefinitionName $imageDefinitionName `
   -Name $versionName -Location $location `
   -SourceImageId $sourceId `
   -ReplicaCount 2 `
   -StorageAccountType Standard_LRS `
   -PublishingProfileEndOfLifeDate '2020-12-01' `
   -TargetRegion $targetRegion
```

### <a name="create-a-vm"></a>Tworzenie maszyny wirtualnej

Możesz utworzyć maszynę wirtualną na podstawie udostępnionej galerii obrazów i użyć kluczy zarządzanych przez klienta do szyfrowania dysków. Składnia jest taka sama jak tworzenie [uogólnionej](vm-generalized-image-version-powershell.md) lub [wyspecjalizowanej](vm-specialized-image-version-powershell.md) maszyny wirtualnej na podstawie obrazu, należy użyć rozszerzonego zestawu parametrów i dodać `Set-AzVMOSDisk -Name $($vmName +"_OSDisk") -DiskEncryptionSetId $diskEncryptionSet.Id -CreateOption FromImage` do konfiguracji maszyny wirtualnej.

W przypadku dysków z danymi należy dodać parametr w `-DiskEncryptionSetId $setID` przypadku użycia polecenie [Add-AzVMDataDisk](/powershell/module/az.compute/add-azvmdatadisk).


## <a name="cli"></a>Interfejs wiersza polecenia 

Aby uzyskać publiczną wersję zapoznawczą, należy najpierw zarejestrować tę funkcję.

```azurecli-interactive
az feature register --namespace Microsoft.Compute --name SIGEncryption
```

Sprawdź stan rejestracji funkcji.

```azurecli-interactive
az feature show --namespace Microsoft.Compute --name SIGEncryption | grep state
```

Po powrocie tego typu `"state": "Registered"` można przejść do następnego kroku.

Sprawdź swoją rejestrację.

```azurecli-interactive
az provider show -n Microsoft.Compute | grep registrationState
```

Jeśli nie powiedzie się, uruchom następujące polecenie:

```azurecli-interactive
az provider register -n Microsoft.Compute
```


Aby określić szyfrowanie dysku ustawione dla wersji obrazu, użyj [AZ Image Gallery Create-Image-Version](/cli/azure/sig/image-version#az-sig-image-version-create) z `--target-region-encryption` parametrem. Format programu `--target-region-encryption` to rozdzielana spacjami Lista kluczy służąca do szyfrowania dysków systemu operacyjnego i danych. Powinien wyglądać następująco: `<encryption set for the OS disk>,<Lun number of the data disk>, <encryption set for the data disk>, <Lun number for the second data disk>, <encryption set for the second data disk>` . 

Jeśli źródło dysku systemu operacyjnego jest dyskiem zarządzanym lub maszyną wirtualną, użyj, `--managed-image` Aby określić źródło dla wersji obrazu. W tym przykładzie źródłem jest obraz zarządzany z dyskiem systemu operacyjnego, a także dysk danych o numerze LUN 0. Dysk systemu operacyjnego zostanie zaszyfrowany za pomocą DiskEncryptionSet1, a dysk danych zostanie zaszyfrowany przy użyciu DiskEncryptionSet2.

```azurecli-interactive
az sig image-version create \
   -g MyResourceGroup \
   --gallery-image-version 1.0.0 \
   --target-regions westus=2=standard_lrs \
   --target-region-encryption DiskEncryptionSet1,0,DiskEncryptionSet2 \
   --gallery-name MyGallery \
   --gallery-image-definition MyImage \
   --managed-image "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/images/myImage"
```

Jeśli źródło dysku systemu operacyjnego jest migawką, użyj, `--os-snapshot` Aby określić dysk systemu operacyjnego. Jeśli istnieją migawki dysków danych, które powinny być również częścią wersji obrazu, Dodaj je przy użyciu, `--data-snapshot-luns` Aby określić jednostkę LUN i `--data-snapshots` określić migawki.

W tym przykładzie źródła są migawkami dysków. Istnieje dysk systemu operacyjnego, a także dysk danych o numerze LUN 0. Dysk systemu operacyjnego zostanie zaszyfrowany za pomocą DiskEncryptionSet1, a dysk danych zostanie zaszyfrowany przy użyciu DiskEncryptionSet2.

```azurecli-interactive
az sig image-version create \
   -g MyResourceGroup \
   --gallery-image-version 1.0.0 \
   --target-regions westus=2=standard_lrs \
   --target-region-encryption DiskEncryptionSet1,0,DiskEncryptionSet2 \
   --os-snapshot "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/snapshots/myOSSnapshot"
   --data-snapshot-luns 0
   --data-snapshots "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/snapshots/myDDSnapshot"
   --gallery-name MyGallery \
   --gallery-image-definition MyImage 
   
```

### <a name="create-the-vm"></a>Tworzenie maszyny wirtualnej

Możesz utworzyć maszynę wirtualną na podstawie udostępnionej galerii obrazów i użyć kluczy zarządzanych przez klienta do szyfrowania dysków. Składnia jest taka sama jak tworzenie [uogólnionej](vm-generalized-image-version-cli.md) lub [wyspecjalizowanej](vm-specialized-image-version-cli.md) maszyny wirtualnej na podstawie obrazu, wystarczy dodać `--os-disk-encryption-set` parametr z identyfikatorem zestawu szyfrowania. W przypadku dysków z danymi Dodaj `--data-disk-encryption-sets` listę rozdzielaną spacjami zestawów szyfrowania dysków dla dysków danych.


## <a name="portal"></a>Portal

Podczas tworzenia wersji obrazu w portalu możesz użyć karty **szyfrowanie** , aby wprowadzić informacje o zestawach szyfrowania magazynu.

1. Na stronie **Tworzenie wersji obrazu** wybierz kartę **szyfrowanie** .
2. W obszarze **typ szyfrowania**wybierz pozycję **szyfrowanie w usłudze REST przy użyciu klucza zarządzanego przez klienta**. 
3. Dla każdego dysku w obrazie wybierz z listy rozwijanej **ustawienia szyfrowanie dysków** , które mają być używane. 

### <a name="create-the-vm"></a>Tworzenie maszyny wirtualnej

Możesz utworzyć maszynę wirtualną na podstawie udostępnionej galerii obrazów i użyć kluczy zarządzanych przez klienta do szyfrowania dysków. Podczas tworzenia maszyny wirtualnej w portalu na karcie **dyski** wybierz opcję **szyfrowanie w trybie spoczynku z kluczami zarządzanymi przez klienta** dla **typu szyfrowania**. Następnie można wybrać zestaw szyfrowania z listy rozwijanej.

## <a name="next-steps"></a>Następne kroki

Dowiedz się więcej o [szyfrowaniu dysków po stronie serwera](./windows/disk-encryption.md).

Aby uzyskać informacje o sposobach dostarczania informacji o planie zakupu, zobacz temat [dostarczanie informacji o planie zakupu portalu Azure Marketplace podczas tworzenia obrazów](marketplace-images.md).
