---
title: Tworzenie zakresów szyfrowania i zarządzanie nimi (wersja zapoznawcza)
description: Dowiedz się, jak utworzyć zakres szyfrowania w celu wyodrębnienia danych obiektów BLOB na poziomie kontenera lub obiektu BLOB.
services: storage
author: tamram
ms.service: storage
ms.date: 08/04/2020
ms.topic: conceptual
ms.author: tamram
ms.reviewer: ozgun
ms.subservice: common
ms.openlocfilehash: d4dd3f3ced8aac6852fe8516a4a5cadca2ebdc49
ms.sourcegitcommit: 97a0d868b9d36072ec5e872b3c77fa33b9ce7194
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/04/2020
ms.locfileid: "87564150"
---
# <a name="create-and-manage-encryption-scopes-preview"></a>Tworzenie zakresów szyfrowania i zarządzanie nimi (wersja zapoznawcza)

Zakresy szyfrowania (wersja zapoznawcza) umożliwiają zarządzanie szyfrowaniem na poziomie pojedynczego obiektu BLOB lub kontenera. Zakres szyfrowania izoluje dane obiektów BLOB w bezpiecznej enklawy na koncie magazynu. Zakresy szyfrowania można używać do tworzenia bezpiecznych granic między danymi znajdującymi się na tym samym koncie magazynu, ale należą do różnych klientów. Aby uzyskać więcej informacji na temat zakresów szyfrowania, zobacz [zakresy szyfrowania dla usługi BLOB Storage (wersja zapoznawcza)](../common/storage-service-encryption.md#encryption-scopes-for-blob-storage-preview).

W tym artykule pokazano, jak utworzyć zakres szyfrowania. Przedstawiono w nim również sposób określania zakresu szyfrowania podczas tworzenia obiektu BLOB lub kontenera.

## <a name="create-an-encryption-scope"></a>Tworzenie zakresu szyfrowania

Zakresy szyfrowania można tworzyć za pomocą klucza zarządzanego przez firmę Microsoft lub z kluczem zarządzanym przez klienta, który jest przechowywany w Azure Key Vault. Aby utworzyć zakres szyfrowania z kluczem zarządzanym przez klienta, należy najpierw utworzyć magazyn kluczy platformy Azure i dodać klucz, który ma być używany dla zakresu. Magazyn kluczy musi mieć włączone zarówno właściwości ochrony **trwałego usuwania** , jak i **przeczyszczania** i muszą znajdować się w tym samym regionie co konto magazynu. Aby uzyskać więcej informacji, zobacz [Używanie kluczy zarządzanych przez klienta w usłudze Azure Key Vault do zarządzania szyfrowaniem usługi Azure Storage](../common/encryption-customer-managed-keys.md).

Zakres szyfrowania jest automatycznie włączany podczas jego tworzenia. Po utworzeniu zakresu szyfrowania można go określić podczas tworzenia obiektu BLOB. Podczas tworzenia kontenera można także określić domyślny zakres szyfrowania, który jest automatycznie stosowany do wszystkich obiektów BLOB w kontenerze.

# <a name="portal"></a>[Portal](#tab/portal)

Aby utworzyć zakres szyfrowania w Azure Portal, wykonaj następujące kroki:

1. W witrynie Azure Portal przejdź do swojego konta magazynu.
1. Wybierz ustawienie **szyfrowanie** .
1. Wybierz kartę **zakresy szyfrowania** .
1. Kliknij przycisk **Dodaj** , aby dodać nowy zakres szyfrowania.
1. W okienku tworzenie **zakresu szyfrowania** wprowadź nazwę nowego zakresu.
1. Wybierz typ szyfrowania, **klucze zarządzane przez firmę Microsoft** lub **klucze zarządzane przez klienta**.
    - W przypadku wybrania **kluczy zarządzanych przez firmę Microsoft**kliknij przycisk **Utwórz** , aby utworzyć zakres szyfrowania.
    - W przypadku wybrania **kluczy zarządzanych przez klienta**Określ Magazyn kluczy, klucz i wersję klucza do użycia dla tego zakresu szyfrowania, jak pokazano na poniższej ilustracji.

    :::image type="content" source="media/encryption-scope-manage/create-encryption-scope-customer-managed-key-portal.png" alt-text="Zrzut ekranu przedstawiający sposób tworzenia zakresu szyfrowania w Azure Portal":::

Aby uzyskać więcej informacji na temat konfigurowania kluczy zarządzanych przez klienta przy użyciu Azure Key Vault na potrzeby szyfrowania usługi Azure Storage, zobacz [Konfigurowanie kluczy zarządzanych przez klienta w Azure Key Vault przy użyciu Azure Portal](../common/storage-encryption-keys-portal.md).

# <a name="powershell"></a>[PowerShell](#tab/powershell)

Aby utworzyć zakres szyfrowania przy użyciu programu PowerShell, najpierw Zainstaluj wersję modułu AZ. Storage Preview. Zalecane jest korzystanie z najnowszej wersji zapoznawczej, ale zakresy szyfrowania są obsługiwane w wersji 1.13.4 — wersja zapoznawcza i nowsza. Usuń wszystkie inne wersje modułu AZ. Storage.

Następujące polecenie instaluje AZ. Storage [2.1.1-Preview](https://www.powershellgallery.com/packages/Az.Storage/2.1.1-preview) module:

```powershell
Install-Module -Name Az.Storage -RequiredVersion 2.1.1-preview -AllowPrerelease
```

### <a name="create-an-encryption-scope-protected-by-microsoft-managed-keys"></a>Utwórz zakres szyfrowania chroniony przez klucze zarządzane przez firmę Microsoft

Aby utworzyć nowy zakres szyfrowania, który jest chroniony przez klucze zarządzane przez firmę Microsoft, wywołaj polecenie **New-AzStorageEncryptionScope** z `-StorageEncryption` parametrem.

Pamiętaj, aby zastąpić wartości symboli zastępczych w przykładzie własnymi wartościami:

```powershell
$rgName = "<resource-group>"
$accountName = "<storage-account>"
$scopeName1 = "customer1scope"

New-AzStorageEncryptionScope -ResourceGroupName $rgName `
    -AccountName $accountName `
    -EncryptionScopeName $scopeName1 `
    -StorageEncryption
```

### <a name="create-an-encryption-scope-protected-by-customer-managed-keys"></a>Tworzenie zakresu szyfrowania chronionego przez klucze zarządzane przez klienta

Aby utworzyć nowy zakres szyfrowania, który jest chroniony przez klucze zarządzane przez klienta z Azure Key Vault, najpierw Skonfiguruj klucze zarządzane przez klienta dla konta magazynu. Do konta magazynu należy przypisać tożsamość zarządzaną, a następnie użyć tożsamości zarządzanej w celu skonfigurowania zasad dostępu dla magazynu kluczy, tak aby konto magazynu miało uprawnienia dostępu do niego. Aby uzyskać więcej informacji, zobacz [Konfigurowanie kluczy zarządzanych przez klienta za pomocą Azure Key Vault przy użyciu programu PowerShell](../common/storage-encryption-keys-powershell.md).

Aby skonfigurować klucze zarządzane przez klienta do użycia z zakresem szyfrowania, należy włączyć zarówno właściwości ochrony **trwałego usuwania** , jak i **przeczyszczania** w magazynie kluczy. Magazyn kluczy musi znajdować się w tym samym regionie co konto magazynu. Aby uzyskać więcej informacji, zobacz [Używanie kluczy zarządzanych przez klienta w usłudze Azure Key Vault do zarządzania szyfrowaniem usługi Azure Storage](../common/encryption-customer-managed-keys.md).

Pamiętaj, aby zastąpić wartości symboli zastępczych w przykładzie własnymi wartościami:

```powershell
$rgName = "<resource-group>"
$accountName = "<storage-account>"
$keyVaultName = "<key-vault>"
$keyUri = "<key-uri-with-version>"
$scopeName2 = "customer2scope"


# Assign a system managed identity to the storage account.
$storageAccount = Set-AzStorageAccount -ResourceGroupName $rgName `
    -Name $accountName `
    -AssignIdentity

# Configure the access policy for the key vault.
Set-AzKeyVaultAccessPolicy `
    -VaultName $keyVaultName `
    -ObjectId $storageAccount.Identity.PrincipalId `
    -PermissionsToKeys wrapkey,unwrapkey,get
```

Następnie Wywołaj polecenie **New-AzStorageEncryptionScope** z `-KeyvaultEncryption` parametrem i określ identyfikator URI klucza. Upewnij się, że w identyfikatorze URI klucza została uwzględniona wersja klucza. Pamiętaj, aby zastąpić wartości symboli zastępczych w przykładzie własnymi wartościami:

```powershell
New-AzStorageEncryptionScope -ResourceGroupName $rgName `
    -AccountName $accountName `
    -EncryptionScopeName $scopeName2 `
    -KeyUri $keyUri `
    -KeyvaultEncryption
```

# <a name="azure-cli"></a>[Interfejs wiersza polecenia platformy Azure](#tab/cli)

Aby utworzyć zakres szyfrowania przy użyciu interfejsu wiersza polecenia platformy Azure, najpierw zainstaluj interfejs wiersza polecenia platformy Azure w wersji 2.4.0 lub nowszej.

### <a name="create-an-encryption-scope-protected-by-microsoft-managed-keys"></a>Utwórz zakres szyfrowania chroniony przez klucze zarządzane przez firmę Microsoft

Aby utworzyć nowy zakres szyfrowania, który jest chroniony przez klucze zarządzane przez firmę Microsoft, wywołaj polecenie [AZ Storage account Encryption-Scope Create](/cli/azure/storage/account/encryption-scope#az-storage-account-encryption-scope-create) , określając `--key-source` parametr jako `Microsoft.Storage` . Pamiętaj, aby zastąpić wartości zastępcze własnymi wartościami:

```azurecli-interactive
az storage account encryption-scope create \
    --resource-group <resource-group> \
    --account-name <storage-account> \
    --name <scope> \
    --key-source Microsoft.Storage
```

### <a name="create-an-encryption-scope-protected-by-customer-managed-keys"></a>Tworzenie zakresu szyfrowania chronionego przez klucze zarządzane przez klienta

Aby utworzyć nowy zakres szyfrowania, który jest chroniony przez klucze zarządzane przez firmę Microsoft, wywołaj polecenie [AZ Storage account Encryption-Scope Create](/cli/azure/storage/account/encryption-scope#az-storage-account-encryption-scope-create) , określając `--key-source` parametr jako `Microsoft.Storage` . Pamiętaj, aby zastąpić wartości zastępcze własnymi wartościami:

Aby utworzyć nowy zakres szyfrowania, który jest chroniony przez klucze zarządzane przez klienta z Azure Key Vault, najpierw Skonfiguruj klucze zarządzane przez klienta dla konta magazynu. Do konta magazynu należy przypisać tożsamość zarządzaną, a następnie użyć tożsamości zarządzanej w celu skonfigurowania zasad dostępu dla magazynu kluczy, tak aby konto magazynu miało uprawnienia dostępu do niego. Aby uzyskać więcej informacji, zobacz [Konfigurowanie kluczy zarządzanych przez klienta za pomocą Azure Key Vault przy użyciu interfejsu wiersza polecenia platformy Azure](../common/storage-encryption-keys-cli.md).

Aby skonfigurować klucze zarządzane przez klienta do użycia z zakresem szyfrowania, należy włączyć zarówno właściwości ochrony **trwałego usuwania** , jak i **przeczyszczania** w magazynie kluczy. Magazyn kluczy musi znajdować się w tym samym regionie co konto magazynu. Aby uzyskać więcej informacji, zobacz [Używanie kluczy zarządzanych przez klienta w usłudze Azure Key Vault do zarządzania szyfrowaniem usługi Azure Storage](../common/encryption-customer-managed-keys.md).

Pamiętaj, aby zastąpić wartości symboli zastępczych w przykładzie własnymi wartościami:

```azurecli-interactive
az login
az account set --subscription <subscription-id>

az storage account update \
    --name <storage-account> \
    --resource-group <resource_group> \
    --assign-identity

storage_account_principal=$(az storage account show \
    --name <storage-account> \
    --resource-group <resource-group> \
    --query identity.principalId \
    --output tsv)

az keyvault set-policy \
    --name <key-vault> \
    --resource-group <resource_group> \
    --object-id $storage_account_principal \
    --key-permissions get unwrapKey wrapKey
```

Następnie Wywołaj polecenie **AZ Storage account Encryption-Scope Create** z `--key-uri` parametrem i określ identyfikator URI klucza. Upewnij się, że w identyfikatorze URI klucza została uwzględniona wersja klucza. Pamiętaj, aby zastąpić wartości symboli zastępczych w przykładzie własnymi wartościami:

```azurecli-interactive
az storage account encryption-scope create \
    --resource-group <resource-group> \
    --account-name <storage-account> \
    --name <scope> \
    --key-source Microsoft.KeyVault \
    --key-uri <key-uri>
```

---

## <a name="list-encryption-scopes-for-storage-account"></a>Wyświetl listę zakresów szyfrowania dla konta magazynu

# <a name="portal"></a>[Portal](#tab/portal)

Aby wyświetlić zakresy szyfrowania dla konta magazynu w Azure Portal, przejdź do ustawienia **zakresy szyfrowania** dla konta magazynu. W tym okienku można włączyć lub wyłączyć zakres szyfrowania albo zmienić klucz dla zakresu szyfrowania.

:::image type="content" source="media/encryption-scope-manage/list-encryption-scopes-portal.png" alt-text="Zrzut ekranu przedstawiający listę zakresów szyfrowania w Azure Portal":::

# <a name="powershell"></a>[PowerShell](#tab/powershell)

Aby wyświetlić listę zakresów szyfrowania dostępnych dla konta magazynu za pomocą programu PowerShell, wywołaj polecenie Get-AzStorageEncryptionScope. Pamiętaj, aby zastąpić wartości symboli zastępczych w przykładzie własnymi wartościami:

```powershell
Get-AzStorageEncryptionScope -ResourceGroupName $rgName `
    -StorageAccountName $accountName
```

Aby wyświetlić listę wszystkich zakresów szyfrowania w grupie zasobów według konta magazynu, użyj składni potoku w następujący sposób:

```powershell
Get-AzStorageAccount -ResourceGroupName $rgName | Get-AzStorageEncryptionScope
```

# <a name="azure-cli"></a>[Interfejs wiersza polecenia platformy Azure](#tab/cli)

Aby wyświetlić listę zakresów szyfrowania dostępnych dla konta magazynu za pomocą interfejsu wiersza polecenia platformy Azure, wywołaj polecenie [AZ Storage account Encryption-Scope list](/cli/azure/storage/account/encryption-scope#az-storage-account-encryption-scope-list) . Pamiętaj, aby zastąpić wartości symboli zastępczych w przykładzie własnymi wartościami:

```azurecli-interactive
az storage account encryption-scope list \
    --account-name <storage-account> \
    --resource-group <resource-group>
```

---

## <a name="create-a-container-with-a-default-encryption-scope"></a>Tworzenie kontenera z domyślnym zakresem szyfrowania

# <a name="portal"></a>[Portal](#tab/portal)

Aby utworzyć kontener z domyślnym zakresem szyfrowania w Azure Portal, należy najpierw utworzyć zakres szyfrowania zgodnie z opisem w temacie [Tworzenie zakresu szyfrowania](#create-an-encryption-scope). Następnie wykonaj następujące kroki, aby utworzyć kontener:

1. Przejdź do listy kontenerów na koncie magazynu, a następnie wybierz przycisk **Dodaj** , aby utworzyć nowy kontener.
1. Rozwiń ustawienia **Zaawansowane** w okienku **nowy kontener** .
1. Z listy rozwijanej **zakres szyfrowania** wybierz domyślny zakres szyfrowania dla kontenera.
1. Aby wymagać, aby wszystkie obiekty blob w kontenerze używały domyślnego zakresu szyfrowania, zaznacz pole wyboru, aby **użyć tego zakresu szyfrowania dla wszystkich obiektów BLOB w kontenerze**. Jeśli to pole wyboru jest zaznaczone, każdy obiekt BLOB w kontenerze nie może zastąpić domyślnego zakresu szyfrowania.

    :::image type="content" source="media/encryption-scope-manage/create-container-default-encryption-scope.png" alt-text="Zrzut ekranu przedstawiający kontener z domyślnym zakresem szyfrowania":::

# <a name="powershell"></a>[PowerShell](#tab/powershell)

Aby utworzyć kontener z domyślnym zakresem szyfrowania przy użyciu programu PowerShell, wywołaj polecenie [New-AzRmStorageContainer](/powershell/module/az.storage/new-azrmstoragecontainer) , określając zakres dla `-DefaultEncryptionScope` parametru. Polecenie **New-AzRmStorageContainer** tworzy kontener przy użyciu dostawcy zasobów usługi Azure Storage, który umożliwia Konfigurowanie zakresów szyfrowania i innych operacji zarządzania zasobami.

Pojedynczy obiekt BLOB można utworzyć przy użyciu własnego zakresu szyfrowania, chyba że kontener jest skonfigurowany tak, aby wymagał, aby wszystkie obiekty blob używały jego domyślnego zakresu. Aby wymusić użycie domyślnego zakresu kontenera przez wszystkie obiekty blob w kontenerze, ustaw `-PreventEncryptionScopeOverride` parametr na `true` .

```powershell
$containerName1 = "container1"
$containerName2 = "container2"

# Create a container with a default encryption scope that cannot be overridden.
New-AzRmStorageContainer -ResourceGroupName $rgName `
    -StorageAccountName $accountName `
    -Name $containerName1 `
    -DefaultEncryptionScope $scopeName1 `
    -PreventEncryptionScopeOverride $true
```

# <a name="azure-cli"></a>[Interfejs wiersza polecenia platformy Azure](#tab/cli)

Aby utworzyć kontener z domyślnym zakresem szyfrowania za pomocą interfejsu wiersza polecenia platformy Azure, wywołaj polecenie [AZ Storage Container Create](/cli/azure/storage/container#az-storage-container-create) , określając zakres dla `--default-encryption-scope` parametru. Pojedynczy obiekt BLOB można utworzyć przy użyciu własnego zakresu szyfrowania, chyba że kontener jest skonfigurowany tak, aby wymagał, aby wszystkie obiekty blob używały jego domyślnego zakresu. Aby wymusić użycie domyślnego zakresu kontenera przez wszystkie obiekty blob w kontenerze, ustaw `--prevent-encryption-scope-override` parametr na `true` .

W poniższym przykładzie używane jest konto usługi Azure AD do autoryzacji operacji tworzenia kontenera. Możesz również użyć klucza dostępu do konta. Aby uzyskać więcej informacji, zobacz [Autoryzuj dostęp do danych obiektu BLOB lub kolejki za pomocą interfejsu wiersza polecenia platformy Azure](../common/authorize-data-operations-cli.md).

```azurecli-interactive
az storage container create \
    --account-name <storage-account> \
    --resource-group <resource-group> \
    --name <container> \
    --default-encryption-scope <scope> \
    --prevent-encryption-scope-override true \
    --auth-mode login
```

---

Jeśli klient próbuje określić zakres podczas przekazywania obiektu BLOB do kontenera z domyślnym zakresem szyfrowania, a kontener jest skonfigurowany tak, aby uniemożliwić obiektom BLOB zastępowanie zakresu domyślnego, operacja kończy się niepowodzeniem z komunikatem wskazującym, że żądanie jest zabronione przez zasady szyfrowania kontenerów.

## <a name="upload-a-blob-with-an-encryption-scope"></a>Przekazywanie obiektu BLOB z zakresem szyfrowania

Podczas przekazywania obiektu BLOB można określić zakres szyfrowania dla tego obiektu BLOB lub użyć domyślnego zakresu szyfrowania dla kontenera, jeśli został on określony. 

# <a name="portal"></a>[Portal](#tab/portal)

Aby przekazać obiekt BLOB z zakresem szyfrowania określonym w Azure Portal, należy najpierw utworzyć zakres szyfrowania zgodnie z opisem w temacie [Tworzenie zakresu szyfrowania](#create-an-encryption-scope). Następnie wykonaj następujące kroki, aby utworzyć obiekt BLOB:

1. Przejdź do kontenera, do którego chcesz przekazać obiekt BLOB.
1. Wybierz przycisk **Przekaż** , a następnie Znajdź obiekt BLOB do przekazania.
1. Rozwiń ustawienia **Zaawansowane** w okienku **przekazywanie obiektu BLOB** .
1. Zlokalizuj sekcję listy rozwijanej **zakres szyfrowania** . Domyślnie obiekt BLOB jest tworzony z domyślnym zakresem szyfrowania dla kontenera, jeśli został on określony. Jeśli kontener wymaga, aby obiekty blob używały domyślnego zakresu szyfrowania, ta sekcja jest wyłączona.
1. Aby określić inny zakres dla przekazywanego obiektu BLOB, wybierz opcję **Wybierz istniejący zakres**, a następnie wybierz żądany zakres z listy rozwijanej.

    :::image type="content" source="media/encryption-scope-manage/upload-blob-encryption-scope.png" alt-text="Zrzut ekranu przedstawiający sposób przekazywania obiektu BLOB z zakresem szyfrowania":::

# <a name="powershell"></a>[PowerShell](#tab/powershell)

Aby przekazać obiekt BLOB z zakresem szyfrowania określonym przy użyciu programu PowerShell, wywołaj polecenie [Set-AzStorageBlobContent](/powershell/module/az.storage/set-azstorageblobcontent) i podaj zakres szyfrowania dla obiektu BLOB.

```powershell
$containerName2 = "container2"
$localSrcFile = "C:\temp\helloworld.txt"
$ctx = (Get-AzStorageAccount -ResourceGroupName $rgName -StorageAccountName $accountName).Context

# Create a new container with no default scope defined.
New-AzStorageContainer -Name $containerName2 -Context $ctx
# Upload a block upload with an encryption scope specified.
Set-AzStorageBlobContent -Context $ctx -Container $containerName2 -File $localSrcFile -Blob "helloworld.txt" -BlobType Block -EncryptionScope $scopeName2
```

# <a name="azure-cli"></a>[Interfejs wiersza polecenia platformy Azure](#tab/cli)

Aby przekazać obiekt BLOB z zakresem szyfrowania określonym przy użyciu interfejsu wiersza polecenia platformy Azure, wywołaj polecenie [AZ Storage BLOB upload](/cli/azure/storage/blob#az-storage-blob-upload) i podaj zakres szyfrowania dla obiektu BLOB.

Jeśli używasz Azure Cloud Shell, postępuj zgodnie z instrukcjami opisanymi w sekcji [przekazywanie obiektu BLOB](storage-quickstart-blobs-cli.md#upload-a-blob) , aby utworzyć plik w katalogu głównym. Następnie można przekazać ten plik do obiektu BLOB przy użyciu poniższego przykładu.

```azurecli-interactive
az storage blob upload \
    --account-name <storage-account> \
    --container-name <container> \
    --file <file> \
    --name <file> \
    --encryption-scope <scope>
```

---

## <a name="change-the-encryption-key-for-a-scope"></a>Zmień klucz szyfrowania dla zakresu

# <a name="portal"></a>[Portal](#tab/portal)

Aby zmienić klucz chroniący zakres w Azure Portal, wykonaj następujące kroki:

1. Przejdź do karty **zakresy szyfrowania** , aby wyświetlić listę zakresów szyfrowania dla konta magazynu.
1. Wybierz przycisk **więcej** obok zakresu, który chcesz zmodyfikować.
1. W okienku **Edytowanie zakresu szyfrowania** można zmienić typ szyfrowania z klucza zarządzanego przez firmę Microsoft na klucz zarządzany przez klienta lub odwrotnie.
1. Aby wybrać nowy klucz zarządzany przez klienta, wybierz opcję **Użyj nowego klucza** i określ Magazyn kluczy, klucz i wersję klucza.

# <a name="powershell"></a>[PowerShell](#tab/powershell)

Aby zmienić klucz chroniący zakres szyfrowania z klucza zarządzanego przez klienta do klucza zarządzanego przez firmę Microsoft przy użyciu programu PowerShell, wywołaj polecenie **Update-AzStorageEncryptionScope** i przekaż `-StorageEncryption` parametr:

```powershell
Update-AzStorageEncryptionScope -ResourceGroupName $rgName `
    -StorageAccountName $accountName `
    -EncryptionScopeName $scopeName2 `
    -StorageEncryption
```

Aby zmienić klucz chroniący zakres szyfrowania z klucza zarządzanego przez firmę Microsoft do klucza zarządzanego przez klienta, najpierw upewnij się, że zostały włączone klucze zarządzane przez klienta z Azure Key Vault dla konta magazynu. Aby uzyskać więcej informacji, zobacz [Konfigurowanie kluczy zarządzanych przez klienta za pomocą Azure Key Vault przy użyciu programu PowerShell](../common/storage-encryption-keys-powershell.md). Następnie Wywołaj polecenie **Update-AzStorageEncryptionScope** i przekaż `-KeyUri` `-KeyvaultEncryption` Parametry i:

```powershell
Update-AzStorageEncryptionScope -ResourceGroupName $rgName `
    -StorageAccountName $accountName `
    -EncryptionScopeName $scopeName1 `
    -KeyUri $keyUri `
    -KeyvaultEncryption
```

# <a name="azure-cli"></a>[Interfejs wiersza polecenia platformy Azure](#tab/cli)

Aby zmienić klucz chroniący zakres szyfrowania z klucza zarządzanego przez klienta do klucza zarządzanego przez firmę Microsoft za pomocą interfejsu wiersza polecenia platformy Azure, wywołaj polecenie [AZ Storage account Encryption-Scope update](/cli/azure/storage/account/encryption-scope#az-storage-account-encryption-scope-update) i przekaż `--key-source` parametrowi wartość `Microsoft.Storage` :

```azurecli-interactive
az storage account encryption-scope update \
    --account-name <storage-account> \
    --resource-group <resource-group>
    --name <scope> \
    --key-source Microsoft.Storage
```

Aby zmienić klucz chroniący zakres szyfrowania z klucza zarządzanego przez firmę Microsoft do klucza zarządzanego przez klienta, najpierw upewnij się, że zostały włączone klucze zarządzane przez klienta z Azure Key Vault dla konta magazynu. Aby uzyskać więcej informacji, zobacz [Konfigurowanie kluczy zarządzanych przez klienta za pomocą Azure Key Vault przy użyciu interfejsu wiersza polecenia platformy Azure](../common/storage-encryption-keys-cli.md). Następnie Wywołaj polecenie **AZ Storage account Encryption-Scope update** , Przekaż `--key-uri` parametr i przekaż `--key-source` parametr z wartością `Microsoft.KeyVault` :

```powershell
az storage account encryption-scope update \
    --resource-group <resource-group> \
    --account-name <storage-account> \
    --name <scope> \
    --key-source Microsoft.KeyVault \
    --key-uri <key-uri>
```

---

## <a name="disable-an-encryption-scope"></a>Wyłącz zakres szyfrowania

# <a name="portal"></a>[Portal](#tab/portal)

Aby wyłączyć zakres szyfrowania w Azure Portal, przejdź do ustawienia **zakresy szyfrowania** dla konta magazynu, wybierz żądany zakres szyfrowania i wybierz pozycję **Wyłącz**.

# <a name="powershell"></a>[PowerShell](#tab/powershell)

Aby wyłączyć zakres szyfrowania przy użyciu programu PowerShell, wywołaj polecenie Update-AzStorageEncryptionScope i Uwzględnij `-State` parametr z wartością `disabled` , jak pokazano w poniższym przykładzie. Aby ponownie włączyć zakres szyfrowania, należy wywołać to samo polecenie z `-State` parametrem ustawionym na `enabled` . Pamiętaj, aby zastąpić wartości symboli zastępczych w przykładzie własnymi wartościami:

```powershell
Update-AzStorageEncryptionScope -ResourceGroupName $rgName `
    -StorageAccountName $accountName `
    -EncryptionScopeName $scopeName1 `
    -State disabled
```

# <a name="azure-cli"></a>[Interfejs wiersza polecenia platformy Azure](#tab/cli)

Aby wyłączyć zakres szyfrowania za pomocą interfejsu wiersza polecenia platformy Azure, wywołaj polecenie [AZ Storage account Encryption-Scope update](/cli/azure/storage/account/encryption-scope#az-storage-account-encryption-scope-update) i Uwzględnij `--state` parametr z wartością `Disabled` , jak pokazano w poniższym przykładzie. Aby ponownie włączyć zakres szyfrowania, należy wywołać to samo polecenie z `--state` parametrem ustawionym na `Enabled` . Pamiętaj, aby zastąpić wartości symboli zastępczych w przykładzie własnymi wartościami:

```azurecli-interactive
az storage account encryption-scope update \
    --account-name <storage-account> \
    --resource-group <resource-group> \
    --name <scope> \
    --state Disabled
```

---

## <a name="next-steps"></a>Następne kroki

- [Szyfrowanie w usłudze Azure Storage dla danych magazynowanych](../common/storage-service-encryption.md)
- [Używanie kluczy zarządzanych przez klienta w usłudze Azure Key Vault do zarządzania szyfrowaniem usługi Azure Storage](../common/encryption-customer-managed-keys.md)
