---
title: Zasady przesyłania strumieniowego w Azure Media Services | Microsoft Docs
description: Ten artykuł zawiera wyjaśnienie zasad przesyłania strumieniowego i sposobu ich użycia przez Azure Media Services.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 05/28/2019
ms.author: juliako
ms.openlocfilehash: 30164e1af1948f86c68e102f4d348674ea931e7b
ms.sourcegitcommit: 8def3249f2c216d7b9d96b154eb096640221b6b9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/03/2020
ms.locfileid: "87544017"
---
# <a name="streaming-policies"></a>Zasady przesyłania strumieniowego

W Azure Media Services v3 [zasady przesyłania strumieniowego](/rest/api/media/streamingpolicies) umożliwiają definiowanie protokołów przesyłania strumieniowego i opcji szyfrowania dla [lokalizatorów przesyłania strumieniowego](streaming-locators-concept.md). Media Services v3 udostępnia wstępnie zdefiniowane zasady przesyłania strumieniowego, dzięki czemu można używać ich bezpośrednio do wersji próbnej lub produkcyjnej. 

Obecnie dostępne wstępnie zdefiniowane zasady przesyłania strumieniowego:<br/>
* "Predefined_DownloadOnly"
* "Predefined_ClearStreamingOnly"
* "Predefined_DownloadAndClearStreaming"
* "Predefined_ClearKey"
* "Predefined_MultiDrmCencStreaming" 
* "Predefined_MultiDrmStreaming"

Poniższe "drzewo decyzyjne" ułatwia wybranie wstępnie zdefiniowanych zasad przesyłania strumieniowego dla Twojego scenariusza.

> [!IMPORTANT]
> * Właściwości **zasad przesyłania strumieniowego** , które są typu DateTime, są zawsze w formacie UTC.
> * Należy zapoznać się z ograniczonym zestawem zasad dla konta usługi multimediów i użyć ich ponownie dla lokalizatorów przesyłania strumieniowego, gdy są potrzebne te same opcje. Aby uzyskać więcej informacji, zobacz limity [przydziału i limity](limits-quotas-constraints.md).

## <a name="decision-tree"></a>Drzewo decyzyjne

Kliknij obraz, aby go wyświetlić w pełnym rozmiarze.  

[![Diagram przedstawiający drzewo decyzyjne mające na celu ułatwienie wyboru wstępnie zdefiniowanych zasad przesyłania strumieniowego dla Twojego scenariusza.](./media/streaming-policy/large.png)](./media/streaming-policy/large.png#lightbox)

W przypadku szyfrowania zawartości należy utworzyć [zasady dotyczące kluczy zawartości](content-key-policy-concept.md). **Zasady kluczy zawartości** nie są wymagane do wyczyszczenia przesyłania strumieniowego ani pobierania. 

Jeśli masz specjalne wymagania (na przykład jeśli chcesz określić różne protokoły, musisz użyć niestandardowej usługi dostarczania kluczy lub użyć jasnej ścieżki audio), możesz [utworzyć](/rest/api/media/streamingpolicies/create) niestandardowe zasady przesyłania strumieniowego. 

## <a name="get-a-streaming-policy-definition"></a>Pobierz definicję zasad przesyłania strumieniowego  

Jeśli chcesz zobaczyć definicję zasad przesyłania strumieniowego, użyj [Get](/rest/api/media/streamingpolicies/get) i określ nazwę zasad. Na przykład:

### <a name="rest"></a>REST

Żądanie:

```
GET https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contoso/providers/Microsoft.Media/mediaServices/contosomedia/streamingPolicies/clearStreamingPolicy?api-version=2018-07-01
```

Odpowiedź:

```
{
  "name": "clearStreamingPolicy",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contoso/providers/Microsoft.Media/mediaservices/contosomedia/streamingPolicies/clearStreamingPolicy",
  "type": "Microsoft.Media/mediaservices/streamingPolicies",
  "properties": {
    "created": "2018-08-08T18:29:30.8501486Z",
    "noEncryption": {
      "enabledProtocols": {
        "download": true,
        "dash": true,
        "hls": true,
        "smoothStreaming": true
      }
    }
  }
}
```

## <a name="filtering-ordering-paging"></a>Filtrowanie, porządkowanie, stronicowanie

Zobacz [filtrowanie, porządkowanie, stronicowanie jednostek Media Services](entities-overview.md).

## <a name="next-steps"></a>Następne kroki

* [Strumieniowe przesyłanie pliku](stream-files-dotnet-quickstart.md)
* [Używanie dynamicznego szyfrowania AES-128 i usługi dostarczania kluczy](protect-with-aes128.md)
* [Używanie usługi dostarczania licencji i szyfrowania dynamicznego w technologii DRM](protect-with-drm.md)
