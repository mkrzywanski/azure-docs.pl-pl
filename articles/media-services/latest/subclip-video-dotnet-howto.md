---
title: Podcinanie wideo przy kodowaniu przy użyciu Azure Media Services
description: W tym temacie opisano sposób podcinania wideo przy kodowaniu przy użyciu Azure Media Services za pomocą zestawu SDK platformy .NET
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 06/09/2019
ms.author: juliako
ms.openlocfilehash: 31fa7fe6b16b7ba97b8ce5e6697f1aaaf88aa294
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/23/2020
ms.locfileid: "87091899"
---
# <a name="subclip-a-video-when-encoding-with-media-services---net"></a>Podcinanie wideo przy kodowaniu przy użyciu Media Services-.NET

Można przyciąć lub podciąć klip wideo przy kodowaniu przy użyciu [zadania](/rest/api/media/jobs). Ta funkcja działa z dowolnym [przekształceniem](/rest/api/media/transforms) utworzonym przy użyciu ustawień wstępnych [BuiltInStandardEncoderPreset](/rest/api/media/transforms/createorupdate#builtinstandardencoderpreset) lub predefiniowanych ustawień [StandardEncoderPreset](/rest/api/media/transforms/createorupdate#standardencoderpreset) .

Poniższy przykład w języku C# tworzy zadanie, które przycina wideo w elemencie zawartości, gdy przesyła zadanie kodowania. 

## <a name="prerequisites"></a>Wymagania wstępne

Aby wykonać kroki opisane w tym temacie, należy:

- [Tworzenie konta usługi Azure Media Services](./create-account-howto.md)
- Utwórz transformację oraz zasoby wejściowe i wyjściowe. Możesz zobaczyć, jak utworzyć zasoby przekształcenia i danych wejściowych i wyjściowych w [klipach wideo przekazywania, kodowania i przesyłania strumieniowego przy użyciu samouczka platformy .NET](stream-files-tutorial-with-api.md) .
- Zapoznaj się z tematem [pojęć dotyczących kodowania](encoding-concept.md) .

## <a name="example"></a>Przykład

```csharp
/// <summary>
/// Submits a request to Media Services to apply the specified Transform to a given input video.
/// </summary>
/// <param name="client">The Media Services client.</param>
/// <param name="resourceGroupName">The name of the resource group within the Azure subscription.</param>
/// <param name="accountName"> The Media Services account name.</param>
/// <param name="transformName">The name of the transform.</param>
/// <param name="jobName">The (unique) name of the job.</param>
/// <param name="inputAssetName">The name of the input asset.</param>
/// <param name="outputAssetName">The (unique) name of the  output asset that will store the result of the encoding job. </param>
// <SubmitJob>
private static async Task<Job> JobWithBuiltInStandardEncoderWithSingleClipAsync(
    IAzureMediaServicesClient client,
    string resourceGroupName,
    string accountName,
    string transformName,
    string jobName,
    string inputAssetName,
    string outputAssetName)
{
    var jobOutputs = new List<JobOutputAsset>
    {
        new JobOutputAsset(state: JobState.Queued, progress: 0, assetName: outputAssetName)
    };

    var clipStart = new AbsoluteClipTime()
    {
        Time = new TimeSpan(0, 0, 20)
    };

    var clipEnd = new AbsoluteClipTime()
    {
        Time = new TimeSpan(0, 0, 30)
    };

    var jobInput = new JobInputAsset(assetName: inputAssetName, start: clipStart, end: clipEnd);

    Job job = await client.Jobs.CreateAsync(
        resourceGroupName,
        accountName,
        transformName,
        jobName,
        new Job(input: jobInput, outputs: jobOutputs.ToArray(), name: jobName)
        {
            Description = $"A Job with transform {transformName} and single clip.",
            Priority = Priority.Normal,
        });

    return job;
}
```

## <a name="next-steps"></a>Następne kroki

[Jak kodować przy użyciu przekształcenia niestandardowego](customize-encoder-presets-how-to.md) 
