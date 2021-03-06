---
title: Instalowanie usługi Azure Blob Storage w systemie Linux przy użyciu protokołu NFS 3,0 (wersja zapoznawcza) | Microsoft Docs
description: Informacje o sposobie instalowania kontenera w usłudze BLOB Storage z maszyny wirtualnej platformy Azure opartej na systemie Linux lub systemu Linux działającego lokalnie przy użyciu protokołu NFS 3,0.
author: normesta
ms.subservice: blobs
ms.service: storage
ms.topic: conceptual
ms.date: 07/21/2020
ms.author: normesta
ms.reviewer: yzheng
ms.custom: references_regions
ms.openlocfilehash: d3907967572b22e7a70316080b08a4368a9805ce
ms.sourcegitcommit: f353fe5acd9698aa31631f38dd32790d889b4dbb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/29/2020
ms.locfileid: "87372913"
---
# <a name="mount-blob-storage-on-linux-using-the-network-file-system-nfs-30-protocol-preview"></a>Instalowanie usługi BLOB Storage w systemie Linux przy użyciu protokołu sieciowego systemu plików (NFS) 3,0 (wersja zapoznawcza)

Kontener w usłudze BLOB Storage można zainstalować z maszyny wirtualnej platformy Azure opartej na systemie Linux lub systemu Linux działającego lokalnie przy użyciu protokołu NFS 3,0. Ten artykuł zawiera wskazówki krok po kroku. Aby dowiedzieć się więcej o obsłudze protokołu NFS 3,0 w usłudze BLOB Storage, zobacz temat [Obsługa protokołu sieciowego systemu plików (NFS) 3,0 w usłudze Azure Blob Storage (wersja zapoznawcza)](network-file-system-protocol-support.md).

> [!NOTE]
> Obsługa protokołu NFS 3,0 w usłudze Azure Blob Storage jest w publicznej wersji zapoznawczej i jest dostępna w następujących regionach: Wschodnie stany USA, Stany Zjednoczone i Kanada środkowa.

## <a name="step-1-register-the-nfs-30-protocol-feature-with-your-subscription"></a>Krok 1. rejestrowanie funkcji protokołu NFS 3,0 w ramach subskrypcji

1. Otwórz okno polecenia programu PowerShell. 

2. Zaloguj się do subskrypcji platformy Azure za pomocą polecenia `Connect-AzAccount` i postępuj zgodnie z instrukcjami wyświetlanymi na ekranie.

   ```powershell
   Connect-AzAccount
   ```

3. Jeśli Twoja tożsamość jest skojarzona z więcej niż jedną subskrypcją, ustaw aktywną subskrypcję.

   ```powershell
   $context = Get-AzSubscription -SubscriptionId <subscription-id>
   Set-AzContext $context
   ```
   
   Zastąp `<subscription-id>` wartość symbolu zastępczego identyfikatorem subskrypcji.

4. Zarejestruj `AllowNFSV3` funkcję przy użyciu następującego polecenia.

   ```powershell
   Register-AzProviderFeature -FeatureName AllowNFSV3 -ProviderNamespace Microsoft.Storage 
   ```

5. Zarejestruj `PremiumHns` funkcję przy użyciu poniższego polecenia.

   ```powershell
   Register-AzProviderFeature -FeatureName PremiumHns -ProviderNamespace Microsoft.Storage  
   ```

6. Zarejestruj dostawcę zasobów przy użyciu następującego polecenia.
    
   ```powershell
   Register-AzResourceProvider -ProviderNamespace Microsoft.Storage   
   ```

## <a name="step-2-verify-that-the-feature-is-registered"></a>Krok 2: sprawdzenie, czy funkcja jest zarejestrowana 

Zatwierdzenie rejestracji może potrwać do godziny. Aby sprawdzić, czy rejestracja została zakończona, użyj następujących poleceń.

```powershell
Get-AzProviderFeature -ProviderNamespace Microsoft.Storage -FeatureName AllowNFSV3
Get-AzProviderFeature -ProviderNamespace Microsoft.Storage -FeatureName PremiumHns  
```

## <a name="step-3-create-an-azure-virtual-network-vnet"></a>Krok 3. Tworzenie Virtual Network platformy Azure (Sieć wirtualna)

Konto magazynu musi być zawarte w sieci wirtualnej. Sieć wirtualna umożliwia klientom bezpieczne łączenie się z kontem magazynu. Aby dowiedzieć się więcej o sieci wirtualnej i jak ją utworzyć, zobacz [dokumentację Virtual Network](https://docs.microsoft.com/azure/virtual-network/).

> [!NOTE]
> Klienci w tej samej sieci wirtualnej mogą instalować kontenery na Twoim koncie. Możesz również zainstalować kontener z poziomu klienta działającego w sieci lokalnej, ale musisz najpierw połączyć sieć lokalną z siecią wirtualną. Zobacz [obsługiwane połączenia sieciowe](network-file-system-protocol-support.md#supported-network-connections).

## <a name="step-4-configure-network-security"></a>Krok 4. Konfigurowanie zabezpieczeń sieci

Jedynym sposobem zabezpieczenia danych na koncie jest użycie sieci wirtualnej i innych ustawień zabezpieczeń sieci. Wszystkie inne narzędzia służące do zabezpieczania danych, w tym autoryzacja klucza konta, zabezpieczenia Azure Active Directory (AD) i listy kontroli dostępu (ACL) nie są jeszcze obsługiwane na kontach, na których włączono obsługę protokołu NFS 3,0. 

Aby zabezpieczyć dane na koncie, zobacz następujące zalecenia: [zalecenia dotyczące zabezpieczeń sieci dla usługi BLOB Storage](security-recommendations.md#networking).

## <a name="step-5-create-and-configure-a-storage-account"></a>Krok 5. Tworzenie i Konfigurowanie konta magazynu

Aby zainstalować kontener przy użyciu systemu plików NFS 3,0, należy utworzyć konto magazynu **po** zarejestrowaniu funkcji w ramach subskrypcji. Nie można włączyć kont, które istniały przed zarejestrowaniem funkcji. 

W wersji zapoznawczej tej funkcji protokół systemu plików NFS 3,0 jest obsługiwany tylko na kontach [BlockBlobStorage](../blobs/storage-blob-create-account-block-blob.md) .

Podczas konfigurowania konta wybierz następujące wartości:

|Ustawienie | Wartość|
|----|---|
|Location|Jeden z następujących regionów: Wschodnie stany USA, Stany USA i Kanada środkowa |
|Wydajność|Premium|
|Rodzaj konta|BlockBlobStorage|
|Replikacja|Magazyn lokalnie nadmiarowy (LRS)|
|Metoda łączności|Publiczny punkt końcowy (wybrane sieci) lub prywatny punkt końcowy|
|Wymagany bezpieczny transfer|Disabled|
|Hierarchiczna przestrzeń nazw|Enabled (Włączony)|
|SYSTEM PLIKÓW NFS V3|Enabled (Włączony)|

Możesz zaakceptować wartości domyślne dla wszystkich innych ustawień. 

## <a name="step-6-create-a-container"></a>Krok 6. Tworzenie kontenera

Utwórz kontener na koncie magazynu przy użyciu dowolnego z tych narzędzi lub zestawów SDK:

|narzędzia|Zestawy SDK|
|---|---|
|[Eksplorator usługi Azure Storage](data-lake-storage-explorer.md#create-a-container)|[.NET](data-lake-storage-directory-file-acl-dotnet.md#create-a-container)|
|[AzCopy](../common/storage-use-azcopy-blobs.md#create-a-container)|[Java](data-lake-storage-directory-file-acl-java.md#create-a-container)|
|[Program PowerShell](data-lake-storage-directory-file-acl-powershell.md#create-a-container)|[Python](data-lake-storage-directory-file-acl-python.md#create-a-container)|
|[Interfejs wiersza polecenia platformy Azure](data-lake-storage-directory-file-acl-cli.md#create-a-container)|[JavaScript](data-lake-storage-directory-file-acl-javascript.md)|
|[Witryna Azure Portal](https://portal.azure.com)|[REST](https://docs.microsoft.com/rest/api/storageservices/create-container)|

## <a name="step-7-mount-the-container"></a>Krok 7. Instalowanie kontenera

1. W systemie Linux Utwórz katalog.

   ```
   mkdir -p /mnt/test
   ```

2. Zainstaluj kontener za pomocą następującego polecenia.

   ```
   mount -o sec=sys,vers=3,nolock,proto=tcp <storage-account-name>.blob.core.windows.net:/<storage-account-name>/<container-name>  /mnt/test
   ```

   - Zastąp `<storage-account-name>` symbol zastępczy, który pojawia się w tym poleceniu nazwą konta magazynu.  

   - Zastąp `<container-name>` symbol zastępczy nazwą kontenera.

## <a name="resolve-common-issues"></a>Rozwiązywanie typowych problemów

|Problem/błąd | Rozwiązanie|
|---|---|
|`Access denied by server while mounting`|Upewnij się, że klient działa w ramach obsługiwanej podsieci. Zobacz [obsługiwane lokalizacje sieciowe](network-file-system-protocol-support.md#supported-network-connections).|
|`No such file or directory`| Upewnij się, że instalowany kontener został utworzony po sprawdzeniu, że funkcja została zarejestrowana. Zobacz [krok 2. Sprawdzanie, czy funkcja jest zarejestrowana](#step-2-verify-that-the-feature-is-registered). Upewnij się również, że wpisz polecenie instalacji i jest ono parametrami bezpośrednio w terminalu. Jeśli skopiujesz i wkleisz każdą część tego polecenia do terminalu z innej aplikacji, ukryte znaki w wklejonej informacji mogą spowodować pojawienie się tego błędu.|

## <a name="see-also"></a>Zobacz także

[Obsługa protokołu sieciowego systemu plików (NFS) 3,0 w usłudze Azure Blob Storage (wersja zapoznawcza)](network-file-system-protocol-support.md)







