---
title: Elementy zawartości
titleSuffix: Azure Media Services
description: Dowiedz się więcej o zasobach i sposobach ich użycia przez Azure Media Services.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 03/09/2020
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 6c9f69a39f725b082771b66959a219581c281ed5
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/23/2020
ms.locfileid: "87043505"
---
# <a name="assets-in-azure-media-services-v3"></a>Zasoby w Azure Media Services v3

W Azure Media Services, element [zawartości](/rest/api/media/assets) jest podstawową koncepcją. Jest to miejsce, w którym można wprowadzać multimedia (na przykład za pośrednictwem przekazywania lub pozyskiwania na żywo), nośniki wyjściowe (z danych wyjściowych zadania) i publikować multimedia z (na potrzeby przesyłania strumieniowego). 

Zasób jest mapowany do kontenera obiektów BLOB na [koncie usługi Azure Storage](storage-account-concept.md) , a pliki w elemencie zawartości są przechowywane jako blokowe obiekty blob w tym kontenerze. Elementy zawartości zawierają informacje o plikach cyfrowych przechowywanych w usłudze Azure Storage (w tym wideo, audio, obrazy, kolekcje miniatur, ścieżki tekstowe i pliki napisów).

Media Services obsługuje warstwy obiektów blob, gdy konto używa magazynu ogólnego przeznaczenia w wersji 2 (GPv2). Za pomocą GPv2 można przenosić pliki do [magazynu chłodnego lub archiwum](../../storage/blobs/storage-blob-storage-tiers.md). Magazyn **archiwalny** jest odpowiedni do archiwizowania plików źródłowych, gdy nie są już potrzebne (na przykład po ich zakodowaniu).

Warstwa magazynu **archiwum** jest zalecana tylko dla bardzo dużych plików źródłowych, które zostały już zakodowane i dane wyjściowe zadania kodowania zostały umieszczone w wyjściowym kontenerze obiektów BLOB. Obiekty blob w kontenerze danych wyjściowych, które mają zostać skojarzone z elementem zawartości i używają do przesyłania strumieniowego lub analizowania zawartości, muszą znajdować się w warstwie magazynowania **gorąca** lub **chłodna** .

## <a name="naming"></a>Nazewnictwo 

### <a name="assets"></a>Elementy zawartości

Nazwy zasobów muszą być unikatowe. Nazwy zasobów Media Services v3 (na przykład zasoby, zadania, przekształcenia) podlegają ograniczeniom nazw Azure Resource Manager. Aby uzyskać więcej informacji, zobacz [konwencje nazewnictwa](media-services-apis-overview.md#naming-conventions).

### <a name="blobs"></a>Obiekty blob

Nazwy plików/obiektów BLOB w obrębie elementu zawartości muszą spełniać zarówno [wymagania dotyczące nazw obiektów BLOB](/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata) , jak i [wymagania dotyczące nazw NTFS](/windows/win32/fileio/naming-a-file). Przyczyną tych wymagań jest możliwość skopiowania plików z magazynu obiektów BLOB do lokalnego dysku NTFS w celu przetworzenia.

## <a name="next-steps"></a>Następne kroki

[Zarządzanie zasobami w Media Services](manage-asset-concept.md)

## <a name="see-also"></a>Zobacz też

[Różnice między Media Services V2 i V3](migrate-from-v2-to-v3.md)
