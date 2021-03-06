---
title: Tagi obrazu kontenera usług Cognitive Services
titleSuffix: Azure Cognitive Services
description: Kompleksowa lista wszystkich tagów obrazu kontenera usługi poznawczej.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: reference
ms.date: 04/01/2020
ms.author: aahi
ms.openlocfilehash: cabc3d2a0f8eb3a75938d1768bb0085aab528391
ms.sourcegitcommit: bb0afd0df5563cc53f76a642fd8fc709e366568b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/19/2020
ms.locfileid: "83584607"
---
# <a name="azure-cognitive-services-container-image-tags"></a>Tagi obrazu kontenera Cognitive Services platformy Azure

Usługa Azure Cognitive Services oferuje wiele obrazów kontenerów. Rejestry kontenerów i odpowiednie repozytoria różnią się między obrazami kontenerów. Każda nazwa obrazu kontenera oferuje wiele tagów. Tag obrazu kontenera jest mechanizmem przechowywania wersji obrazu kontenera. Ten artykuł jest przeznaczony do użycia jako wyczerpujące odwołanie do wyświetlania wszystkich Cognitive Services obrazów kontenerów i ich dostępnych tagów.

> [!TIP]
> W przypadku korzystania z programu [`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/) należy zwrócić uwagę na wielkość liter w rejestrze kontenerów, repozytorium, nazwę obrazu kontenera i odpowiedni tag — w miarę **uwzględniania wielkości**liter.

## <a name="anomaly-detector"></a>Narzędzie do wykrywania anomalii

Obraz kontenera [wykrywania anomalii][ad-containers] można znaleźć w obszarze `mcr.microsoft.com` syndykaty rejestru kontenerów. Znajduje się w `azure-cognitive-services` repozytorium i ma nazwę `anomaly-detector` . W pełni kwalifikowana nazwa obrazu kontenera to, `mcr.microsoft.com/azure-cognitive-services/anomaly-detector` .

Ten obraz kontenera ma dostępne następujące znaczniki:

| Tagi obrazu                    | Uwagi |
|-------------------------------|:------|
| `latest`                      |       |

## <a name="computer-vision"></a>Przetwarzanie obrazów

Obraz kontenera [Przetwarzanie obrazów][cv-containers] można znaleźć w `containerpreview.azurecr.io` rejestrze kontenerów. Znajduje się w `microsoft` repozytorium i ma nazwę `cognitive-services-read` . W pełni kwalifikowana nazwa obrazu kontenera to, `containerpreview.azurecr.io/microsoft/cognitive-services-read` .

Ten obraz kontenera ma dostępne następujące znaczniki:

| Tagi obrazu                    | Uwagi |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.011580001-amd64-preview` |       |
| `1.1.009920003-amd64-preview` |       |
| `1.1.009910003-amd64-preview` |       |

## <a name="face"></a>Rozpoznawanie twarzy

Obraz kontenera [kroju][fa-containers] można znaleźć w `containerpreview.azurecr.io` rejestrze kontenerów. Znajduje się w `microsoft` repozytorium i ma nazwę `cognitive-services-face` . W pełni kwalifikowana nazwa obrazu kontenera to, `containerpreview.azurecr.io/microsoft/cognitive-services-face` .

Ten obraz kontenera ma dostępne następujące znaczniki:

| Tagi obrazu                    | Uwagi |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.009301-amd64-preview`    |       |
| `1.1.008710001-amd64-preview` |       |
| `1.1.007750002-amd64-preview` |       |
| `1.1.007360001-amd64-preview` |       |
| `1.1.006770001-amd64-preview` |       |
| `1.1.006490002-amd64-preview` |       |
| `1.0.005940002-amd64-preview` |       |
| `1.0.005550001-amd64-preview` |       |

## <a name="form-recognizer"></a>Rozpoznawanie formularzy

Obraz kontenera [aparatu rozpoznawania formularzy][fr-containers] można znaleźć w `containerpreview.azurecr.io` rejestrze kontenerów. Znajduje się w `microsoft` repozytorium i ma nazwę `cognitive-services-form-recognizer` . W pełni kwalifikowana nazwa obrazu kontenera to, `containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer` .

Ten obraz kontenera ma dostępne następujące znaczniki:

| Tagi obrazu                    | Uwagi |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.009301-amd64-preview`    |       |
| `1.1.008640001-amd64-preview` |       |
| `1.1.008510001-amd64-preview` |       |

## <a name="language-understanding-luis"></a>Language Understanding (LUIS)

Obraz kontenera [Luis][lu-containers] można znaleźć w obszarze `mcr.microsoft.com` syndykaty rejestru kontenerów. Znajduje się w `azure-cognitive-services` repozytorium i ma nazwę `luis` . W pełni kwalifikowana nazwa obrazu kontenera to, `mcr.microsoft.com/azure-cognitive-services/luis` .

Ten obraz kontenera ma dostępne następujące znaczniki:

| Tagi obrazu                    | Uwagi |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.010330004-amd64-preview` |       |
| `1.1.009301-amd64-preview`    |       |
| `1.1.008710001-amd64-preview` |       |
| `1.1.008510001-amd64-preview` |       |
| `1.1.008010002-amd64-preview` |       |
| `1.1.007750002-amd64-preview` |       |
| `1.1.007360001-amd64-preview` |       |
| `1.1.007020001-amd64-preview` |       |

## <a name="custom-speech-to-text"></a>Custom Speech do tekstu

Obraz kontenera [Custom Speech-to-Text][sp-cstt] można znaleźć w `containerpreview.azurecr.io` rejestrze kontenerów. Znajduje się w `microsoft` repozytorium i ma nazwę `cognitive-services-custom-speech-to-text` . W pełni kwalifikowana nazwa obrazu kontenera to, `containerpreview.azurecr.io/microsoft/cognitive-services-custom-speech-to-text` .

Ten obraz kontenera ma dostępne następujące znaczniki:

| Tagi obrazu            | Uwagi |
|-----------------------|:------|
| `latest`              |       |
| `2.2.0-amd64-preview` |       |
| `2.1.1-amd64-preview` |       |
| `2.1.0-amd64-preview` |       |
| `2.0.2-amd64-preview` |       |
| `2.0.0-amd64-preview` |       |

## <a name="custom-text-to-speech"></a>Niestandardowa Zamiana tekstu na mowę

Niestandardowy obraz kontenera [zamiany tekstu na mowę][sp-ctts] można znaleźć w `containerpreview.azurecr.io` rejestrze kontenerów. Znajduje się w `microsoft` repozytorium i ma nazwę `cognitive-services-custom-text-to-speech` . W pełni kwalifikowana nazwa obrazu kontenera to, `containerpreview.azurecr.io/microsoft/cognitive-services-custom-text-to-speech` .

Ten obraz kontenera ma dostępne następujące znaczniki:

| Tagi obrazu            | Uwagi |
|-----------------------|:------|
| `latest`              |       |
| `1.3.0-amd64-preview` |       |

## <a name="speech-to-text"></a>Zamiana mowy na tekst

Obraz kontenera [zamiany mowy na tekst][sp-stt] można znaleźć w `containerpreview.azurecr.io` rejestrze kontenerów. Znajduje się w `microsoft` repozytorium i ma nazwę `cognitive-services-speech-to-text` . W pełni kwalifikowana nazwa obrazu kontenera to, `containerpreview.azurecr.io/microsoft/cognitive-services-speech-to-text` .

Ten obraz kontenera ma dostępne następujące znaczniki:

| Tagi obrazu                  | Uwagi                                    |
|-----------------------------|:-----------------------------------------|
| `latest`                    | Obraz kontenera z `en-US` ustawieniami regionalnymi. |
| `2.2.0-amd64-ar-ae-preview` | Obraz kontenera z `ar-AE` ustawieniami regionalnymi. |
| `2.2.0-amd64-ar-eg-preview` | Obraz kontenera z `ar-EG` ustawieniami regionalnymi. |
| `2.2.0-amd64-ar-kw-preview` | Obraz kontenera z `ar-KW` ustawieniami regionalnymi. |
| `2.2.0-amd64-ar-qa-preview` | Obraz kontenera z `ar-QA` ustawieniami regionalnymi. |
| `2.2.0-amd64-ar-sa-preview` | Obraz kontenera z `ar-SA` ustawieniami regionalnymi. |
| `2.2.0-amd64-ca-es-preview` | Obraz kontenera z `ca-ES` ustawieniami regionalnymi. |
| `2.2.0-amd64-da-dk-preview` | Obraz kontenera z `da-DK` ustawieniami regionalnymi. |
| `2.2.0-amd64-de-de-preview` | Obraz kontenera z `de-DE` ustawieniami regionalnymi. |
| `2.2.0-amd64-en-au-preview` | Obraz kontenera z `en-AU` ustawieniami regionalnymi. |
| `2.2.0-amd64-en-ca-preview` | Obraz kontenera z `en-CA` ustawieniami regionalnymi. |
| `2.2.0-amd64-en-gb-preview` | Obraz kontenera z `en-GB` ustawieniami regionalnymi. |
| `2.2.0-amd64-en-in-preview` | Obraz kontenera z `en-IN` ustawieniami regionalnymi. |
| `2.2.0-amd64-en-nz-preview` | Obraz kontenera z `en-NZ` ustawieniami regionalnymi. |
| `2.2.0-amd64-en-us-preview` | Obraz kontenera z `en-US` ustawieniami regionalnymi. |
| `2.2.0-amd64-es-es-preview` | Obraz kontenera z `es-ES` ustawieniami regionalnymi. |
| `2.2.0-amd64-es-mx-preview` | Obraz kontenera z `es-MX` ustawieniami regionalnymi. |
| `2.2.0-amd64-fi-fi-preview` | Obraz kontenera z `fi-FI` ustawieniami regionalnymi. |
| `2.2.0-amd64-fr-ca-preview` | Obraz kontenera z `fr-CA` ustawieniami regionalnymi. |
| `2.2.0-amd64-fr-fr-preview` | Obraz kontenera z `fr-FR` ustawieniami regionalnymi. |
| `2.2.0-amd64-gu-in-preview` | Obraz kontenera z `gu-IN` ustawieniami regionalnymi. |
| `2.2.0-amd64-hi-in-preview` | Obraz kontenera z `hi-IN` ustawieniami regionalnymi. |
| `2.2.0-amd64-it-it-preview` | Obraz kontenera z `it-IT` ustawieniami regionalnymi. |
| `2.2.0-amd64-ja-jp-preview` | Obraz kontenera z `ja-JP` ustawieniami regionalnymi. |
| `2.2.0-amd64-ko-kr-preview` | Obraz kontenera z `ko-KR` ustawieniami regionalnymi. |
| `2.2.0-amd64-mr-in-preview` | Obraz kontenera z `mr-IN` ustawieniami regionalnymi. |
| `2.2.0-amd64-nb-no-preview` | Obraz kontenera z `nb-NO` ustawieniami regionalnymi. |
| `2.2.0-amd64-nl-nl-preview` | Obraz kontenera z `nl-NL` ustawieniami regionalnymi. |
| `2.2.0-amd64-pl-pl-preview` | Obraz kontenera z `pl-PL` ustawieniami regionalnymi. |
| `2.2.0-amd64-pt-br-preview` | Obraz kontenera z `pt-BR` ustawieniami regionalnymi. |
| `2.2.0-amd64-pt-pt-preview` | Obraz kontenera z `pt-PT` ustawieniami regionalnymi. |
| `2.2.0-amd64-ru-ru-preview` | Obraz kontenera z `ru-RU` ustawieniami regionalnymi. |
| `2.2.0-amd64-sv-se-preview` | Obraz kontenera z `sv-SE` ustawieniami regionalnymi. |
| `2.2.0-amd64-ta-in-preview` | Obraz kontenera z `ta-IN` ustawieniami regionalnymi. |
| `2.2.0-amd64-te-in-preview` | Obraz kontenera z `te-IN` ustawieniami regionalnymi. |
| `2.2.0-amd64-th-th-preview` | Obraz kontenera z `th-TH` ustawieniami regionalnymi. |
| `2.2.0-amd64-tr-tr-preview` | Obraz kontenera z `tr-TR` ustawieniami regionalnymi. |
| `2.2.0-amd64-zh-cn-preview` | Obraz kontenera z `zh-CN` ustawieniami regionalnymi. |
| `2.2.0-amd64-zh-hk-preview` | Obraz kontenera z `zh-HK` ustawieniami regionalnymi. |
| `2.2.0-amd64-zh-tw-preview` | Obraz kontenera z `zh-TW` ustawieniami regionalnymi. |
| `2.1.1-amd64-en-us-preview` | Obraz kontenera z `en-US` ustawieniami regionalnymi. |
| `2.1.1-amd64-ar-ae-preview` | Obraz kontenera z `ar-AE` ustawieniami regionalnymi. |
| `2.1.1-amd64-ar-eg-preview` | Obraz kontenera z `ar-EG` ustawieniami regionalnymi. |
| `2.1.1-amd64-ar-kw-preview` | Obraz kontenera z `ar-KW` ustawieniami regionalnymi. |
| `2.1.1-amd64-ar-qa-preview` | Obraz kontenera z `ar-QA` ustawieniami regionalnymi. |
| `2.1.1-amd64-ar-sa-preview` | Obraz kontenera z `ar-SA` ustawieniami regionalnymi. |
| `2.1.1-amd64-ca-es-preview` | Obraz kontenera z `ca-ES` ustawieniami regionalnymi. |
| `2.1.1-amd64-da-dk-preview` | Obraz kontenera z `da-DK` ustawieniami regionalnymi. |
| `2.1.1-amd64-de-de-preview` | Obraz kontenera z `de-DE` ustawieniami regionalnymi. |
| `2.1.1-amd64-en-au-preview` | Obraz kontenera z `en-AU` ustawieniami regionalnymi. |
| `2.1.1-amd64-en-ca-preview` | Obraz kontenera z `en-CA` ustawieniami regionalnymi. |
| `2.1.1-amd64-en-gb-preview` | Obraz kontenera z `en-GB` ustawieniami regionalnymi. |
| `2.1.1-amd64-en-in-preview` | Obraz kontenera z `en-IN` ustawieniami regionalnymi. |
| `2.1.1-amd64-en-nz-preview` | Obraz kontenera z `en-NZ` ustawieniami regionalnymi. |
| `2.1.1-amd64-en-us-preview` | Obraz kontenera z `en-US` ustawieniami regionalnymi. |
| `2.1.1-amd64-es-es-preview` | Obraz kontenera z `es-ES` ustawieniami regionalnymi. |
| `2.1.1-amd64-es-mx-preview` | Obraz kontenera z `es-MX` ustawieniami regionalnymi. |
| `2.1.1-amd64-fi-fi-preview` | Obraz kontenera z `fi-FI` ustawieniami regionalnymi. |
| `2.1.1-amd64-fr-ca-preview` | Obraz kontenera z `fr-CA` ustawieniami regionalnymi. |
| `2.1.1-amd64-fr-fr-preview` | Obraz kontenera z `fr-FR` ustawieniami regionalnymi. |
| `2.1.1-amd64-gu-in-preview` | Obraz kontenera z `gu-IN` ustawieniami regionalnymi. |
| `2.1.1-amd64-hi-in-preview` | Obraz kontenera z `hi-IN` ustawieniami regionalnymi. |
| `2.1.1-amd64-it-it-preview` | Obraz kontenera z `it-IT` ustawieniami regionalnymi. |
| `2.1.1-amd64-ja-jp-preview` | Obraz kontenera z `ja-JP` ustawieniami regionalnymi. |
| `2.1.1-amd64-ko-kr-preview` | Obraz kontenera z `ko-KR` ustawieniami regionalnymi. |
| `2.1.1-amd64-mr-in-preview` | Obraz kontenera z `mr-IN` ustawieniami regionalnymi. |
| `2.1.1-amd64-nb-no-preview` | Obraz kontenera z `nb-NO` ustawieniami regionalnymi. |
| `2.1.1-amd64-nl-nl-preview` | Obraz kontenera z `nl-NL` ustawieniami regionalnymi. |
| `2.1.1-amd64-pl-pl-preview` | Obraz kontenera z `pl-PL` ustawieniami regionalnymi. |
| `2.1.1-amd64-pt-br-preview` | Obraz kontenera z `pt-BR` ustawieniami regionalnymi. |
| `2.1.1-amd64-pt-pt-preview` | Obraz kontenera z `pt-PT` ustawieniami regionalnymi. |
| `2.1.1-amd64-ru-ru-preview` | Obraz kontenera z `ru-RU` ustawieniami regionalnymi. |
| `2.1.1-amd64-sv-se-preview` | Obraz kontenera z `sv-SE` ustawieniami regionalnymi. |
| `2.1.1-amd64-ta-in-preview` | Obraz kontenera z `ta-IN` ustawieniami regionalnymi. |
| `2.1.1-amd64-te-in-preview` | Obraz kontenera z `te-IN` ustawieniami regionalnymi. |
| `2.1.1-amd64-th-th-preview` | Obraz kontenera z `th-TH` ustawieniami regionalnymi. |
| `2.1.1-amd64-tr-tr-preview` | Obraz kontenera z `tr-TR` ustawieniami regionalnymi. |
| `2.1.1-amd64-zh-cn-preview` | Obraz kontenera z `zh-CN` ustawieniami regionalnymi. |
| `2.1.1-amd64-zh-hk-preview` | Obraz kontenera z `zh-HK` ustawieniami regionalnymi. |
| `2.1.1-amd64-zh-tw-preview` | Obraz kontenera z `zh-TW` ustawieniami regionalnymi. |
| `2.1.0-amd64-ar-ae-preview` | Obraz kontenera z `ar-AE` ustawieniami regionalnymi. |
| `2.1.0-amd64-ar-eg-preview` | Obraz kontenera z `ar-EG` ustawieniami regionalnymi. |
| `2.1.0-amd64-ar-kw-preview` | Obraz kontenera z `ar-KW` ustawieniami regionalnymi. |
| `2.1.0-amd64-ar-qa-preview` | Obraz kontenera z `ar-QA` ustawieniami regionalnymi. |
| `2.1.0-amd64-ar-sa-preview` | Obraz kontenera z `ar-SA` ustawieniami regionalnymi. |
| `2.1.0-amd64-ca-es-preview` | Obraz kontenera z `ca-ES` ustawieniami regionalnymi. |
| `2.1.0-amd64-da-dk-preview` | Obraz kontenera z `da-DK` ustawieniami regionalnymi. |
| `2.1.0-amd64-de-de-preview` | Obraz kontenera z `de-DE` ustawieniami regionalnymi. |
| `2.1.0-amd64-en-au-preview` | Obraz kontenera z `en-AU` ustawieniami regionalnymi. |
| `2.1.0-amd64-en-ca-preview` | Obraz kontenera z `en-CA` ustawieniami regionalnymi. |
| `2.1.0-amd64-en-gb-preview` | Obraz kontenera z `en-GB` ustawieniami regionalnymi. |
| `2.1.0-amd64-en-in-preview` | Obraz kontenera z `en-IN` ustawieniami regionalnymi. |
| `2.1.0-amd64-en-nz-preview` | Obraz kontenera z `en-NZ` ustawieniami regionalnymi. |
| `2.1.0-amd64-en-us-preview` | Obraz kontenera z `en-US` ustawieniami regionalnymi. |
| `2.1.0-amd64-es-es-preview` | Obraz kontenera z `es-ES` ustawieniami regionalnymi. |
| `2.1.0-amd64-es-mx-preview` | Obraz kontenera z `es-MX` ustawieniami regionalnymi. |
| `2.1.0-amd64-fi-fi-preview` | Obraz kontenera z `fi-FI` ustawieniami regionalnymi. |
| `2.1.0-amd64-fr-ca-preview` | Obraz kontenera z `fr-CA` ustawieniami regionalnymi. |
| `2.1.0-amd64-fr-fr-preview` | Obraz kontenera z `fr-FR` ustawieniami regionalnymi. |
| `2.1.0-amd64-gu-in-preview` | Obraz kontenera z `gu-IN` ustawieniami regionalnymi. |
| `2.1.0-amd64-hi-in-preview` | Obraz kontenera z `hi-IN` ustawieniami regionalnymi. |
| `2.1.0-amd64-it-it-preview` | Obraz kontenera z `it-IT` ustawieniami regionalnymi. |
| `2.1.0-amd64-ja-jp-preview` | Obraz kontenera z `ja-JP` ustawieniami regionalnymi. |
| `2.1.0-amd64-ko-kr-preview` | Obraz kontenera z `ko-KR` ustawieniami regionalnymi. |
| `2.1.0-amd64-mr-in-preview` | Obraz kontenera z `mr-IN` ustawieniami regionalnymi. |
| `2.1.0-amd64-nb-no-preview` | Obraz kontenera z `nb-NO` ustawieniami regionalnymi. |
| `2.1.0-amd64-nl-nl-preview` | Obraz kontenera z `nl-NL` ustawieniami regionalnymi. |
| `2.1.0-amd64-pl-pl-preview` | Obraz kontenera z `pl-PL` ustawieniami regionalnymi. |
| `2.1.0-amd64-pt-br-preview` | Obraz kontenera z `pt-BR` ustawieniami regionalnymi. |
| `2.1.0-amd64-pt-pt-preview` | Obraz kontenera z `pt-PT` ustawieniami regionalnymi. |
| `2.1.0-amd64-ru-ru-preview` | Obraz kontenera z `ru-RU` ustawieniami regionalnymi. |
| `2.1.0-amd64-sv-se-preview` | Obraz kontenera z `sv-SE` ustawieniami regionalnymi. |
| `2.1.0-amd64-ta-in-preview` | Obraz kontenera z `ta-IN` ustawieniami regionalnymi. |
| `2.1.0-amd64-te-in-preview` | Obraz kontenera z `te-IN` ustawieniami regionalnymi. |
| `2.1.0-amd64-th-th-preview` | Obraz kontenera z `th-TH` ustawieniami regionalnymi. |
| `2.1.0-amd64-tr-tr-preview` | Obraz kontenera z `tr-TR` ustawieniami regionalnymi. |
| `2.1.0-amd64-zh-cn-preview` | Obraz kontenera z `zh-CN` ustawieniami regionalnymi. |
| `2.1.0-amd64-zh-hk-preview` | Obraz kontenera z `zh-HK` ustawieniami regionalnymi. |
| `2.1.0-amd64-zh-tw-preview` | Obraz kontenera z `zh-TW` ustawieniami regionalnymi. |
| `2.0.3-amd64-ja-jp-preview` | Obraz kontenera z `ja-JP` ustawieniami regionalnymi. |
| `2.0.2-amd64-ar-ae-preview` | Obraz kontenera z `ar-AE` ustawieniami regionalnymi. |
| `2.0.2-amd64-ar-eg-preview` | Obraz kontenera z `ar-EG` ustawieniami regionalnymi. |
| `2.0.2-amd64-ar-kw-preview` | Obraz kontenera z `ar-KW` ustawieniami regionalnymi. |
| `2.0.2-amd64-ar-qa-preview` | Obraz kontenera z `ar-QA` ustawieniami regionalnymi. |
| `2.0.2-amd64-ar-sa-preview` | Obraz kontenera z `ar-SA` ustawieniami regionalnymi. |
| `2.0.2-amd64-ca-es-preview` | Obraz kontenera z `ca-ES` ustawieniami regionalnymi. |
| `2.0.2-amd64-da-dk-preview` | Obraz kontenera z `da-DK` ustawieniami regionalnymi. |
| `2.0.2-amd64-de-de-preview` | Obraz kontenera z `de-DE` ustawieniami regionalnymi. |
| `2.0.2-amd64-en-au-preview` | Obraz kontenera z `en-AU` ustawieniami regionalnymi. |
| `2.0.2-amd64-en-ca-preview` | Obraz kontenera z `en-CA` ustawieniami regionalnymi. |
| `2.0.2-amd64-en-gb-preview` | Obraz kontenera z `en-GB` ustawieniami regionalnymi. |
| `2.0.2-amd64-en-in-preview` | Obraz kontenera z `en-IN` ustawieniami regionalnymi. |
| `2.0.2-amd64-en-nz-preview` | Obraz kontenera z `en-NZ` ustawieniami regionalnymi. |
| `2.0.2-amd64-en-us-preview` | Obraz kontenera z `en-US` ustawieniami regionalnymi. |
| `2.0.2-amd64-es-es-preview` | Obraz kontenera z `es-ES` ustawieniami regionalnymi. |
| `2.0.2-amd64-es-mx-preview` | Obraz kontenera z `es-MX` ustawieniami regionalnymi. |
| `2.0.2-amd64-fi-fi-preview` | Obraz kontenera z `fi-FI` ustawieniami regionalnymi. |
| `2.0.2-amd64-fr-ca-preview` | Obraz kontenera z `fr-CA` ustawieniami regionalnymi. |
| `2.0.2-amd64-fr-fr-preview` | Obraz kontenera z `fr-FR` ustawieniami regionalnymi. |
| `2.0.2-amd64-gu-in-preview` | Obraz kontenera z `gu-IN` ustawieniami regionalnymi. |
| `2.0.2-amd64-hi-in-preview` | Obraz kontenera z `hi-IN` ustawieniami regionalnymi. |
| `2.0.2-amd64-it-it-preview` | Obraz kontenera z `it-IT` ustawieniami regionalnymi. |
| `2.0.2-amd64-ja-jp-preview` | Obraz kontenera z `ja-JP` ustawieniami regionalnymi. |
| `2.0.2-amd64-ko-kr-preview` | Obraz kontenera z `ko-KR` ustawieniami regionalnymi. |
| `2.0.2-amd64-mr-in-preview` | Obraz kontenera z `mr-IN` ustawieniami regionalnymi. |
| `2.0.2-amd64-nb-no-preview` | Obraz kontenera z `nb-NO` ustawieniami regionalnymi. |
| `2.0.2-amd64-nl-nl-preview` | Obraz kontenera z `nl-NL` ustawieniami regionalnymi. |
| `2.0.2-amd64-pl-pl-preview` | Obraz kontenera z `pl-PL` ustawieniami regionalnymi. |
| `2.0.2-amd64-pt-br-preview` | Obraz kontenera z `pt-BR` ustawieniami regionalnymi. |
| `2.0.2-amd64-pt-pt-preview` | Obraz kontenera z `pt-PT` ustawieniami regionalnymi. |
| `2.0.2-amd64-ru-ru-preview` | Obraz kontenera z `ru-RU` ustawieniami regionalnymi. |
| `2.0.2-amd64-sv-se-preview` | Obraz kontenera z `sv-SE` ustawieniami regionalnymi. |
| `2.0.2-amd64-ta-in-preview` | Obraz kontenera z `ta-IN` ustawieniami regionalnymi. |
| `2.0.2-amd64-te-in-preview` | Obraz kontenera z `te-IN` ustawieniami regionalnymi. |
| `2.0.2-amd64-th-th-preview` | Obraz kontenera z `th-TH` ustawieniami regionalnymi. |
| `2.0.2-amd64-tr-tr-preview` | Obraz kontenera z `tr-TR` ustawieniami regionalnymi. |
| `2.0.2-amd64-zh-cn-preview` | Obraz kontenera z `zh-CN` ustawieniami regionalnymi. |
| `2.0.2-amd64-zh-hk-preview` | Obraz kontenera z `zh-HK` ustawieniami regionalnymi. |
| `2.0.2-amd64-zh-tw-preview` | Obraz kontenera z `zh-TW` ustawieniami regionalnymi. |
| `2.0.1-amd64-ar-ae-preview` | Obraz kontenera z `ar-AE` ustawieniami regionalnymi. |
| `2.0.1-amd64-ar-eg-preview` | Obraz kontenera z `ar-EG` ustawieniami regionalnymi. |
| `2.0.1-amd64-ar-kw-preview` | Obraz kontenera z `ar-KW` ustawieniami regionalnymi. |
| `2.0.1-amd64-ar-qa-preview` | Obraz kontenera z `ar-QA` ustawieniami regionalnymi. |
| `2.0.1-amd64-ar-sa-preview` | Obraz kontenera z `ar-SA` ustawieniami regionalnymi. |
| `2.0.1-amd64-ca-es-preview` | Obraz kontenera z `ca-ES` ustawieniami regionalnymi. |
| `2.0.1-amd64-da-dk-preview` | Obraz kontenera z `da-DK` ustawieniami regionalnymi. |
| `2.0.1-amd64-de-de-preview` | Obraz kontenera z `de-DE` ustawieniami regionalnymi. |
| `2.0.1-amd64-en-au-preview` | Obraz kontenera z `en-AU` ustawieniami regionalnymi. |
| `2.0.1-amd64-en-ca-preview` | Obraz kontenera z `en-CA` ustawieniami regionalnymi. |
| `2.0.1-amd64-en-gb-preview` | Obraz kontenera z `en-GB` ustawieniami regionalnymi. |
| `2.0.1-amd64-en-in-preview` | Obraz kontenera z `en-IN` ustawieniami regionalnymi. |
| `2.0.1-amd64-en-nz-preview` | Obraz kontenera z `en-NZ` ustawieniami regionalnymi. |
| `2.0.1-amd64-en-us-preview` | Obraz kontenera z `en-US` ustawieniami regionalnymi. |
| `2.0.1-amd64-es-es-preview` | Obraz kontenera z `es-ES` ustawieniami regionalnymi. |
| `2.0.1-amd64-es-mx-preview` | Obraz kontenera z `es-MX` ustawieniami regionalnymi. |
| `2.0.1-amd64-fi-fi-preview` | Obraz kontenera z `fi-FI` ustawieniami regionalnymi. |
| `2.0.1-amd64-fr-ca-preview` | Obraz kontenera z `fr-CA` ustawieniami regionalnymi. |
| `2.0.1-amd64-fr-fr-preview` | Obraz kontenera z `fr-FR` ustawieniami regionalnymi. |
| `2.0.1-amd64-gu-in-preview` | Obraz kontenera z `gu-IN` ustawieniami regionalnymi. |
| `2.0.1-amd64-hi-in-preview` | Obraz kontenera z `hi-IN` ustawieniami regionalnymi. |
| `2.0.1-amd64-it-it-preview` | Obraz kontenera z `it-IT` ustawieniami regionalnymi. |
| `2.0.1-amd64-ja-jp-preview` | Obraz kontenera z `ja-JP` ustawieniami regionalnymi. |
| `2.0.1-amd64-ko-kr-preview` | Obraz kontenera z `ko-KR` ustawieniami regionalnymi. |
| `2.0.1-amd64-mr-in-preview` | Obraz kontenera z `mr-IN` ustawieniami regionalnymi. |
| `2.0.1-amd64-nb-no-preview` | Obraz kontenera z `nb-NO` ustawieniami regionalnymi. |
| `2.0.1-amd64-nl-nl-preview` | Obraz kontenera z `nl-NL` ustawieniami regionalnymi. |
| `2.0.1-amd64-pl-pl-preview` | Obraz kontenera z `pl-PL` ustawieniami regionalnymi. |
| `2.0.1-amd64-pt-br-preview` | Obraz kontenera z `pt-BR` ustawieniami regionalnymi. |
| `2.0.1-amd64-pt-pt-preview` | Obraz kontenera z `pt-PT` ustawieniami regionalnymi. |
| `2.0.1-amd64-ru-ru-preview` | Obraz kontenera z `ru-RU` ustawieniami regionalnymi. |
| `2.0.1-amd64-sv-se-preview` | Obraz kontenera z `sv-SE` ustawieniami regionalnymi. |
| `2.0.1-amd64-ta-in-preview` | Obraz kontenera z `ta-IN` ustawieniami regionalnymi. |
| `2.0.1-amd64-te-in-preview` | Obraz kontenera z `te-IN` ustawieniami regionalnymi. |
| `2.0.1-amd64-th-th-preview` | Obraz kontenera z `th-TH` ustawieniami regionalnymi. |
| `2.0.1-amd64-tr-tr-preview` | Obraz kontenera z `tr-TR` ustawieniami regionalnymi. |
| `2.0.1-amd64-zh-cn-preview` | Obraz kontenera z `zh-CN` ustawieniami regionalnymi. |
| `2.0.1-amd64-zh-hk-preview` | Obraz kontenera z `zh-HK` ustawieniami regionalnymi. |
| `2.0.1-amd64-zh-tw-preview` | Obraz kontenera z `zh-TW` ustawieniami regionalnymi. |
| `2.0.0-amd64-ar-eg-preview` | Obraz kontenera z `ar-EG` ustawieniami regionalnymi. |
| `2.0.0-amd64-ca-es-preview` | Obraz kontenera z `ca-ES` ustawieniami regionalnymi. |
| `2.0.0-amd64-da-dk-preview` | Obraz kontenera z `da-DK` ustawieniami regionalnymi. |
| `2.0.0-amd64-de-de-preview` | Obraz kontenera z `de-DE` ustawieniami regionalnymi. |
| `2.0.0-amd64-en-au-preview` | Obraz kontenera z `en-AU` ustawieniami regionalnymi. |
| `2.0.0-amd64-en-ca-preview` | Obraz kontenera z `en-CA` ustawieniami regionalnymi. |
| `2.0.0-amd64-en-gb-preview` | Obraz kontenera z `en-GB` ustawieniami regionalnymi. |
| `2.0.0-amd64-en-in-preview` | Obraz kontenera z `en-IN` ustawieniami regionalnymi. |
| `2.0.0-amd64-en-nz-preview` | Obraz kontenera z `en-NZ` ustawieniami regionalnymi. |
| `2.0.0-amd64-en-us-preview` | Obraz kontenera z `en-US` ustawieniami regionalnymi. |
| `2.0.0-amd64-es-es-preview` | Obraz kontenera z `es-ES` ustawieniami regionalnymi. |
| `2.0.0-amd64-es-mx-preview` | Obraz kontenera z `es-MX` ustawieniami regionalnymi. |
| `2.0.0-amd64-fi-fi-preview` | Obraz kontenera z `fi-FI` ustawieniami regionalnymi. |
| `2.0.0-amd64-fr-ca-preview` | Obraz kontenera z `fr-CA` ustawieniami regionalnymi. |
| `2.0.0-amd64-fr-fr-preview` | Obraz kontenera z `fr-FR` ustawieniami regionalnymi. |
| `2.0.0-amd64-hi-in-preview` | Obraz kontenera z `hi-IN` ustawieniami regionalnymi. |
| `2.0.0-amd64-it-it-preview` | Obraz kontenera z `it-IT` ustawieniami regionalnymi. |
| `2.0.0-amd64-ja-jp-preview` | Obraz kontenera z `ja-JP` ustawieniami regionalnymi. |
| `2.0.0-amd64-ko-kr-preview` | Obraz kontenera z `ko-KR` ustawieniami regionalnymi. |
| `2.0.0-amd64-nb-no-preview` | Obraz kontenera z `nb-NO` ustawieniami regionalnymi. |
| `2.0.0-amd64-nl-nl-preview` | Obraz kontenera z `nl-NL` ustawieniami regionalnymi. |
| `2.0.0-amd64-pl-pl-preview` | Obraz kontenera z `pl-PL` ustawieniami regionalnymi. |
| `2.0.0-amd64-pt-br-preview` | Obraz kontenera z `pt-BR` ustawieniami regionalnymi. |
| `2.0.0-amd64-pt-pt-preview` | Obraz kontenera z `pt-PT` ustawieniami regionalnymi. |
| `2.0.0-amd64-ru-ru-preview` | Obraz kontenera z `ru-RU` ustawieniami regionalnymi. |
| `2.0.0-amd64-sv-se-preview` | Obraz kontenera z `sv-SE` ustawieniami regionalnymi. |
| `2.0.0-amd64-th-th-preview` | Obraz kontenera z `th-TH` ustawieniami regionalnymi. |
| `2.0.0-amd64-tr-tr-preview` | Obraz kontenera z `tr-TR` ustawieniami regionalnymi. |
| `2.0.0-amd64-zh-cn-preview` | Obraz kontenera z `zh-CN` ustawieniami regionalnymi. |
| `2.0.0-amd64-zh-hk-preview` | Obraz kontenera z `zh-HK` ustawieniami regionalnymi. |
| `2.0.0-amd64-zh-tw-preview` | Obraz kontenera z `zh-TW` ustawieniami regionalnymi. |
| `1.2.0-amd64-de-de-preview` | Obraz kontenera z `de-DE` ustawieniami regionalnymi. |
| `1.2.0-amd64-en-au-preview` | Obraz kontenera z `en-AU` ustawieniami regionalnymi. |
| `1.2.0-amd64-en-ca-preview` | Obraz kontenera z `en-CA` ustawieniami regionalnymi. |
| `1.2.0-amd64-en-gb-preview` | Obraz kontenera z `en-GB` ustawieniami regionalnymi. |
| `1.2.0-amd64-en-in-preview` | Obraz kontenera z `en-IN` ustawieniami regionalnymi. |
| `1.2.0-amd64-en-us-preview` | Obraz kontenera z `en-US` ustawieniami regionalnymi. |
| `1.2.0-amd64-es-es-preview` | Obraz kontenera z `es-ES` ustawieniami regionalnymi. |
| `1.2.0-amd64-es-mx-preview` | Obraz kontenera z `es-MX` ustawieniami regionalnymi. |
| `1.2.0-amd64-fr-ca-preview` | Obraz kontenera z `fr-CA` ustawieniami regionalnymi. |
| `1.2.0-amd64-fr-fr-preview` | Obraz kontenera z `fr-FR` ustawieniami regionalnymi. |
| `1.2.0-amd64-it-it-preview` | Obraz kontenera z `it-IT` ustawieniami regionalnymi. |
| `1.2.0-amd64-ja-jp-preview` | Obraz kontenera z `ja-JP` ustawieniami regionalnymi. |
| `1.2.0-amd64-pt-br-preview` | Obraz kontenera z `pt-BR` ustawieniami regionalnymi. |
| `1.2.0-amd64-zh-cn-preview` | Obraz kontenera z `zh-CN` ustawieniami regionalnymi. |
| `1.1.3-amd64-de-de-preview` | Obraz kontenera z `de-DE` ustawieniami regionalnymi. |
| `1.1.3-amd64-en-au-preview` | Obraz kontenera z `en-AU` ustawieniami regionalnymi. |
| `1.1.3-amd64-en-ca-preview` | Obraz kontenera z `en-CA` ustawieniami regionalnymi. |
| `1.1.3-amd64-en-gb-preview` | Obraz kontenera z `en-GB` ustawieniami regionalnymi. |
| `1.1.3-amd64-en-in-preview` | Obraz kontenera z `en-IN` ustawieniami regionalnymi. |
| `1.1.3-amd64-en-us-preview` | Obraz kontenera z `en-US` ustawieniami regionalnymi. |
| `1.1.3-amd64-es-es-preview` | Obraz kontenera z `es-ES` ustawieniami regionalnymi. |
| `1.1.3-amd64-es-mx-preview` | Obraz kontenera z `es-MX` ustawieniami regionalnymi. |
| `1.1.3-amd64-fr-ca-preview` | Obraz kontenera z `fr-CA` ustawieniami regionalnymi. |
| `1.1.3-amd64-fr-fr-preview` | Obraz kontenera z `fr-FR` ustawieniami regionalnymi. |
| `1.1.3-amd64-it-it-preview` | Obraz kontenera z `it-IT` ustawieniami regionalnymi. |
| `1.1.3-amd64-ja-jp-preview` | Obraz kontenera z `ja-JP` ustawieniami regionalnymi. |
| `1.1.3-amd64-pt-br-preview` | Obraz kontenera z `pt-BR` ustawieniami regionalnymi. |
| `1.1.3-amd64-zh-cn-preview` | Obraz kontenera z `zh-CN` ustawieniami regionalnymi. |
| `1.1.2-amd64-de-de-preview` | Obraz kontenera z `de-DE` ustawieniami regionalnymi. |
| `1.1.2-amd64-en-au-preview` | Obraz kontenera z `en-AU` ustawieniami regionalnymi. |
| `1.1.2-amd64-en-ca-preview` | Obraz kontenera z `en-CA` ustawieniami regionalnymi. |
| `1.1.2-amd64-en-gb-preview` | Obraz kontenera z `en-GB` ustawieniami regionalnymi. |
| `1.1.2-amd64-en-in-preview` | Obraz kontenera z `en-IN` ustawieniami regionalnymi. |
| `1.1.2-amd64-en-us-preview` | Obraz kontenera z `en-US` ustawieniami regionalnymi. |
| `1.1.2-amd64-es-es-preview` | Obraz kontenera z `es-ES` ustawieniami regionalnymi. |
| `1.1.2-amd64-es-mx-preview` | Obraz kontenera z `es-MX` ustawieniami regionalnymi. |
| `1.1.2-amd64-fr-ca-preview` | Obraz kontenera z `fr-CA` ustawieniami regionalnymi. |
| `1.1.2-amd64-fr-fr-preview` | Obraz kontenera z `fr-FR` ustawieniami regionalnymi. |
| `1.1.2-amd64-it-it-preview` | Obraz kontenera z `it-IT` ustawieniami regionalnymi. |
| `1.1.2-amd64-ja-jp-preview` | Obraz kontenera z `ja-JP` ustawieniami regionalnymi. |
| `1.1.2-amd64-pt-br-preview` | Obraz kontenera z `pt-BR` ustawieniami regionalnymi. |
| `1.1.2-amd64-zh-cn-preview` | Obraz kontenera z `zh-CN` ustawieniami regionalnymi. |
| `1.1.1-amd64-de-de-preview` | Obraz kontenera z `de-DE` ustawieniami regionalnymi. |
| `1.1.1-amd64-en-au-preview` | Obraz kontenera z `en-AU` ustawieniami regionalnymi. |
| `1.1.1-amd64-en-ca-preview` | Obraz kontenera z `en-CA` ustawieniami regionalnymi. |
| `1.1.1-amd64-en-gb-preview` | Obraz kontenera z `en-GB` ustawieniami regionalnymi. |
| `1.1.1-amd64-en-in-preview` | Obraz kontenera z `en-IN` ustawieniami regionalnymi. |
| `1.1.1-amd64-en-us-preview` | Obraz kontenera z `en-US` ustawieniami regionalnymi. |
| `1.1.1-amd64-es-es-preview` | Obraz kontenera z `es-ES` ustawieniami regionalnymi. |
| `1.1.1-amd64-es-mx-preview` | Obraz kontenera z `es-MX` ustawieniami regionalnymi. |
| `1.1.1-amd64-fr-ca-preview` | Obraz kontenera z `fr-CA` ustawieniami regionalnymi. |
| `1.1.1-amd64-fr-fr-preview` | Obraz kontenera z `fr-FR` ustawieniami regionalnymi. |
| `1.1.1-amd64-it-it-preview` | Obraz kontenera z `it-IT` ustawieniami regionalnymi. |
| `1.1.1-amd64-ja-jp-preview` | Obraz kontenera z `ja-JP` ustawieniami regionalnymi. |
| `1.1.1-amd64-pt-br-preview` | Obraz kontenera z `pt-BR` ustawieniami regionalnymi. |
| `1.1.1-amd64-zh-cn-preview` | Obraz kontenera z `zh-CN` ustawieniami regionalnymi. |
| `1.1.0-amd64-de-de-preview` | Obraz kontenera z `de-DE` ustawieniami regionalnymi. |
| `1.1.0-amd64-en-au-preview` | Obraz kontenera z `en-AU` ustawieniami regionalnymi. |
| `1.1.0-amd64-en-ca-preview` | Obraz kontenera z `en-CA` ustawieniami regionalnymi. |
| `1.1.0-amd64-en-gb-preview` | Obraz kontenera z `en-GB` ustawieniami regionalnymi. |
| `1.1.0-amd64-en-in-preview` | Obraz kontenera z `en-IN` ustawieniami regionalnymi. |
| `1.1.0-amd64-en-us-preview` | Obraz kontenera z `en-US` ustawieniami regionalnymi. |
| `1.1.0-amd64-es-es-preview` | Obraz kontenera z `es-ES` ustawieniami regionalnymi. |
| `1.1.0-amd64-es-mx-preview` | Obraz kontenera z `es-MX` ustawieniami regionalnymi. |
| `1.1.0-amd64-fr-ca-preview` | Obraz kontenera z `fr-CA` ustawieniami regionalnymi. |
| `1.1.0-amd64-fr-fr-preview` | Obraz kontenera z `fr-FR` ustawieniami regionalnymi. |
| `1.1.0-amd64-it-it-preview` | Obraz kontenera z `it-IT` ustawieniami regionalnymi. |
| `1.1.0-amd64-ja-jp-preview` | Obraz kontenera z `ja-JP` ustawieniami regionalnymi. |
| `1.1.0-amd64-pt-br-preview` | Obraz kontenera z `pt-BR` ustawieniami regionalnymi. |
| `1.1.0-amd64-zh-cn-preview` | Obraz kontenera z `zh-CN` ustawieniami regionalnymi. |
| `1.0.0-amd64-de-de-preview` | Obraz kontenera z `de-DE` ustawieniami regionalnymi. |
| `1.0.0-amd64-en-au-preview` | Obraz kontenera z `en-AU` ustawieniami regionalnymi. |
| `1.0.0-amd64-en-ca-preview` | Obraz kontenera z `en-CA` ustawieniami regionalnymi. |
| `1.0.0-amd64-en-gb-preview` | Obraz kontenera z `en-GB` ustawieniami regionalnymi. |
| `1.0.0-amd64-en-in-preview` | Obraz kontenera z `en-IN` ustawieniami regionalnymi. |
| `1.0.0-amd64-en-us-preview` | Obraz kontenera z `en-US` ustawieniami regionalnymi. |
| `1.0.0-amd64-es-es-preview` | Obraz kontenera z `es-ES` ustawieniami regionalnymi. |
| `1.0.0-amd64-es-mx-preview` | Obraz kontenera z `es-MX` ustawieniami regionalnymi. |
| `1.0.0-amd64-fr-ca-preview` | Obraz kontenera z `fr-CA` ustawieniami regionalnymi. |
| `1.0.0-amd64-fr-fr-preview` | Obraz kontenera z `fr-FR` ustawieniami regionalnymi. |
| `1.0.0-amd64-it-it-preview` | Obraz kontenera z `it-IT` ustawieniami regionalnymi. |
| `1.0.0-amd64-ja-jp-preview` | Obraz kontenera z `ja-JP` ustawieniami regionalnymi. |
| `1.0.0-amd64-pt-br-preview` | Obraz kontenera z `pt-BR` ustawieniami regionalnymi. |
| `1.0.0-amd64-zh-cn-preview` | Obraz kontenera z `zh-CN` ustawieniami regionalnymi. |

## <a name="text-to-speech"></a>Zamiana tekstu na mowę

Obraz kontenera [zamiany tekstu na mowę][sp-tts] można znaleźć w `containerpreview.azurecr.io` rejestrze kontenerów. Znajduje się w `microsoft` repozytorium i ma nazwę `cognitive-services-text-to-speech` . W pełni kwalifikowana nazwa obrazu kontenera to, `containerpreview.azurecr.io/microsoft/cognitive-services-text-to-speech` .

Ten obraz kontenera ma dostępne następujące znaczniki:

| Tagi obrazu                                  | Uwagi                                                                      |
|---------------------------------------------|:---------------------------------------------------------------------------|
| `latest`                                    | Obraz kontenera z `en-US` ustawieniami regionalnymi i `en-US-JessaRUS` głosem.        |
| `1.3.0-amd64-ar-eg-hoda-preview`            | Obraz kontenera z `ar-EG` ustawieniami regionalnymi i `ar-EG-Hoda` głosem.            |
| `1.3.0-amd64-ar-sa-naayf-preview`           | Obraz kontenera z `ar-SA` ustawieniami regionalnymi i `ar-SA-Naayf` głosem.           |
| `1.3.0-amd64-bg-bg-ivan-preview`            | Obraz kontenera z `bg-BG` ustawieniami regionalnymi i `bg-BG-Ivan` głosem.            |
| `1.3.0-amd64-ca-es-herenarus-preview`       | Obraz kontenera z `ca-ES` ustawieniami regionalnymi i `ca-ES-HerenaRUS` głosem.       |
| `1.3.0-amd64-cs-cz-jakub-preview`           | Obraz kontenera z `cs-CZ` ustawieniami regionalnymi i `cs-CZ-Jakub` głosem.           |
| `1.3.0-amd64-da-dk-hellerus-preview`        | Obraz kontenera z `da-DK` ustawieniami regionalnymi i `da-DK-HelleRUS` głosem.        |
| `1.3.0-amd64-de-at-michael-preview`         | Obraz kontenera z `de-AT` ustawieniami regionalnymi i `de-AT-Michael` głosem.         |
| `1.3.0-amd64-de-ch-karsten-preview`         | Obraz kontenera z `de-CH` ustawieniami regionalnymi i `de-CH-Karsten` głosem.         |
| `1.3.0-amd64-de-de-hedda-preview`           | Obraz kontenera z `de-DE` ustawieniami regionalnymi i `de-DE-Hedda` głosem.           |
| `1.3.0-amd64-de-de-heddarus-preview`        | Obraz kontenera z `de-DE` ustawieniami regionalnymi i `de-DE-Hedda` głosem.           |
| `1.3.0-amd64-de-de-heddarus-preview`        | Obraz kontenera z `de-DE` ustawieniami regionalnymi i `de-DE-HeddaRUS` głosem.        |
| `1.3.0-amd64-de-de-stefan-apollo-preview`   | Obraz kontenera z `de-DE` ustawieniami regionalnymi i `de-DE-Stefan-Apollo` głosem.   |
| `1.3.0-amd64-el-gr-stefanos-preview`        | Obraz kontenera z `el-GR` ustawieniami regionalnymi i `el-GR-Stefanos` głosem.        |
| `1.3.0-amd64-en-au-catherine-preview`       | Obraz kontenera z `en-AU` ustawieniami regionalnymi i `en-AU-Catherine` głosem.       |
| `1.3.0-amd64-en-au-hayleyrus-preview`       | Obraz kontenera z `en-AU` ustawieniami regionalnymi i `en-AU-HayleyRUS` głosem.       |
| `1.3.0-amd64-en-ca-heatherrus-preview`      | Obraz kontenera z `en-CA` ustawieniami regionalnymi i `en-CA-HeatherRUS` głosem.      |
| `1.3.0-amd64-en-ca-linda-preview`           | Obraz kontenera z `en-CA` ustawieniami regionalnymi i `en-CA-Linda` głosem.           |
| `1.3.0-amd64-en-gb-george-apollo-preview`   | Obraz kontenera z `en-GB` ustawieniami regionalnymi i `en-GB-George-Apollo` głosem.   |
| `1.3.0-amd64-en-gb-hazelrus-preview`        | Obraz kontenera z `en-GB` ustawieniami regionalnymi i `en-GB-HazelRUS` głosem.        |
| `1.3.0-amd64-en-gb-susan-apollo-preview`    | Obraz kontenera z `en-GB` ustawieniami regionalnymi i `en-GB-Susan-Apollo` głosem.    |
| `1.3.0-amd64-en-ie-sean-preview`            | Obraz kontenera z `en-IE` ustawieniami regionalnymi i `en-IE-Sean` głosem.            |
| `1.3.0-amd64-en-in-heera-apollo-preview`    | Obraz kontenera z `en-IN` ustawieniami regionalnymi i `en-IN-Heera-Apollo` głosem.    |
| `1.3.0-amd64-en-in-priyarus-preview`        | Obraz kontenera z `en-IN` ustawieniami regionalnymi i `en-IN-PriyaRUS` głosem.        |
| `1.3.0-amd64-en-in-ravi-apollo-preview`     | Obraz kontenera z `en-IN` ustawieniami regionalnymi i `en-IN-Ravi-Apollo` głosem.     |
| `1.3.0-amd64-en-us-benjaminrus-preview`     | Obraz kontenera z `en-US` ustawieniami regionalnymi i `en-US-BenjaminRUS` głosem.     |
| `1.3.0-amd64-en-us-guy24krus-preview`       | Obraz kontenera z `en-US` ustawieniami regionalnymi i `en-US-Guy24kRUS` głosem.       |
| `1.3.0-amd64-en-us-jessa24krus-preview`     | Obraz kontenera z `en-US` ustawieniami regionalnymi i `en-US-Jessa24kRUS` głosem.     |
| `1.3.0-amd64-en-us-jessarus-preview`        | Obraz kontenera z `en-US` ustawieniami regionalnymi i `en-US-JessaRUS` głosem.        |
| `1.3.0-amd64-en-us-zirarus-preview`         | Obraz kontenera z `en-US` ustawieniami regionalnymi i `en-US-ZiraRUS` głosem.         |
| `1.3.0-amd64-es-es-helenarus-preview`       | Obraz kontenera z `es-ES` ustawieniami regionalnymi i `es-ES-HelenaRUS` głosem.       |
| `1.3.0-amd64-es-es-laura-apollo-preview`    | Obraz kontenera z `es-ES` ustawieniami regionalnymi i `es-ES-Laura-Apollo` głosem.    |
| `1.3.0-amd64-es-es-pablo-apollo-preview`    | Obraz kontenera z `es-ES` ustawieniami regionalnymi i `es-ES-Pablo-Apollo` głosem.    |
| `1.3.0-amd64-es-mx-hildarus-preview`        | Obraz kontenera z `es-MX` ustawieniami regionalnymi i `es-MX-HildaRUS` głosem.        |
| `1.3.0-amd64-es-mx-raul-apollo-preview`     | Obraz kontenera z `es-MX` ustawieniami regionalnymi i `es-MX-Raul-Apollo` głosem.     |
| `1.3.0-amd64-fi-fi-heidirus-preview`        | Obraz kontenera z `fi-FI` ustawieniami regionalnymi i `fi-FI-HeidiRUS` głosem.        |
| `1.3.0-amd64-fr-ca-caroline-preview`        | Obraz kontenera z `fr-CA` ustawieniami regionalnymi i `fr-CA-Caroline` głosem.        |
| `1.3.0-amd64-fr-ca-harmonierus-preview`     | Obraz kontenera z `fr-CA` ustawieniami regionalnymi i `fr-CA-HarmonieRUS` głosem.     |
| `1.3.0-amd64-fr-ch-guillaume-preview`       | Obraz kontenera z `fr-CH` ustawieniami regionalnymi i `fr-CH-Guillaume` głosem.       |
| `1.3.0-amd64-fr-fr-hortenserus-preview`     | Obraz kontenera z `fr-FR` ustawieniami regionalnymi i `fr-FR-HortenseRUS` głosem.     |
| `1.3.0-amd64-fr-fr-julie-apollo-preview`    | Obraz kontenera z `fr-FR` ustawieniami regionalnymi i `fr-FR-Julie-Apollo` głosem.    |
| `1.3.0-amd64-fr-fr-paul-apollo-preview`     | Obraz kontenera z `fr-FR` ustawieniami regionalnymi i `fr-FR-Paul-Apollo` głosem.     |
| `1.3.0-amd64-he-il-asaf-preview`            | Obraz kontenera z `he-IL` ustawieniami regionalnymi i `he-IL-Asaf` głosem.            |
| `1.3.0-amd64-hi-in-hemant-preview`          | Obraz kontenera z `hi-IN` ustawieniami regionalnymi i `hi-IN-Hemant` głosem.          |
| `1.3.0-amd64-hi-in-kalpana-apollo-preview`  | Obraz kontenera z `hi-IN` ustawieniami regionalnymi i `hi-IN-Kalpana-Apollo` głosem.  |
| `1.3.0-amd64-hi-in-kalpana-apollo-preview`  | Obraz kontenera z `hi-IN` ustawieniami regionalnymi i `hi-IN-Kalpana` głosem.         |
| `1.3.0-amd64-hi-in-kalpana-preview`         | Obraz kontenera z `hi-IN` ustawieniami regionalnymi i `hi-IN-Kalpana` głosem.         |
| `1.3.0-amd64-hr-hr-matej-preview`           | Obraz kontenera z `hr-HR` ustawieniami regionalnymi i `hr-HR-Matej` głosem.           |
| `1.3.0-amd64-hu-hu-szabolcs-preview`        | Obraz kontenera z `hu-HU` ustawieniami regionalnymi i `hu-HU-Szabolcs` głosem.        |
| `1.3.0-amd64-id-id-andika-preview`          | Obraz kontenera z `id-ID` ustawieniami regionalnymi i `id-ID-Andika` głosem.          |
| `1.3.0-amd64-it-it-cosimo-apollo-preview`   | Obraz kontenera z `it-IT` ustawieniami regionalnymi i `it-IT-Cosimo-Apollo` głosem.   |
| `1.3.0-amd64-it-it-luciarus-preview`        | Obraz kontenera z `it-IT` ustawieniami regionalnymi i `it-IT-LuciaRUS` głosem.        |
| `1.3.0-amd64-ja-jp-ayumi-apollo-preview`    | Obraz kontenera z `ja-JP` ustawieniami regionalnymi i `ja-JP-Ayumi-Apollo` głosem.    |
| `1.3.0-amd64-ja-jp-harukarus-preview`       | Obraz kontenera z `ja-JP` ustawieniami regionalnymi i `ja-JP-HarukaRUS` głosem.       |
| `1.3.0-amd64-ja-jp-ichiro-apollo-preview`   | Obraz kontenera z `ja-JP` ustawieniami regionalnymi i `ja-JP-Ichiro-Apollo` głosem.   |
| `1.3.0-amd64-ko-kr-heamirus-preview`        | Obraz kontenera z `ko-KR` ustawieniami regionalnymi i `ko-KR-HeamiRUS` głosem.        |
| `1.3.0-amd64-ms-my-rizwan-preview`          | Obraz kontenera z `ms-MY` ustawieniami regionalnymi i `ms-MY-Rizwan` głosem.          |
| `1.3.0-amd64-nb-no-huldarus-preview`        | Obraz kontenera z `nb-NO` ustawieniami regionalnymi i `nb-NO-HuldaRUS` głosem.        |
| `1.3.0-amd64-nl-nl-hannarus-preview`        | Obraz kontenera z `nl-NL` ustawieniami regionalnymi i `nl-NL-HannaRUS` głosem.        |
| `1.3.0-amd64-pl-pl-paulinarus-preview`      | Obraz kontenera z `pl-PL` ustawieniami regionalnymi i `pl-PL-PaulinaRUS` głosem.      |
| `1.3.0-amd64-pt-br-daniel-apollo-preview`   | Obraz kontenera z `pt-BR` ustawieniami regionalnymi i `pt-BR-Daniel-Apollo` głosem.   |
| `1.3.0-amd64-pt-br-heloisarus-preview`      | Obraz kontenera z `pt-BR` ustawieniami regionalnymi i `pt-BR-HeloisaRUS` głosem.      |
| `1.3.0-amd64-pt-pt-heliarus-preview`        | Obraz kontenera z `pt-PT` ustawieniami regionalnymi i `pt-PT-HeliaRUS` głosem.        |
| `1.3.0-amd64-ro-ro-andrei-preview`          | Obraz kontenera z `ro-RO` ustawieniami regionalnymi i `ro-RO-Andrei` głosem.          |
| `1.3.0-amd64-ru-ru-ekaterinarus-preview`    | Obraz kontenera z `ru-RU` ustawieniami regionalnymi i `ru-RU-EkaterinaRUS` głosem.    |
| `1.3.0-amd64-ru-ru-irina-apollo-preview`    | Obraz kontenera z `ru-RU` ustawieniami regionalnymi i `ru-RU-Irina-Apollo` głosem.    |
| `1.3.0-amd64-ru-ru-pavel-apollo-preview`    | Obraz kontenera z `ru-RU` ustawieniami regionalnymi i `ru-RU-Pavel-Apollo` głosem.    |
| `1.3.0-amd64-sk-sk-filip-preview`           | Obraz kontenera z `sk-SK` ustawieniami regionalnymi i `sk-SK-Filip` głosem.           |
| `1.3.0-amd64-sl-si-lado-preview`            | Obraz kontenera z `sl-SI` ustawieniami regionalnymi i `sl-SI-Lado` głosem.            |
| `1.3.0-amd64-sv-se-hedvigrus-preview`       | Obraz kontenera z `sv-SE` ustawieniami regionalnymi i `sv-SE-HedvigRUS` głosem.       |
| `1.3.0-amd64-ta-in-valluvar-preview`        | Obraz kontenera z `ta-IN` ustawieniami regionalnymi i `ta-IN-Valluvar` głosem.        |
| `1.3.0-amd64-te-in-chitra-preview`          | Obraz kontenera z `te-IN` ustawieniami regionalnymi i `te-IN-Chitra` głosem.          |
| `1.3.0-amd64-th-th-pattara-preview`         | Obraz kontenera z `th-TH` ustawieniami regionalnymi i `th-TH-Pattara` głosem.         |
| `1.3.0-amd64-tr-tr-sedarus-preview`         | Obraz kontenera z `tr-TR` ustawieniami regionalnymi i `tr-TR-SedaRUS` głosem.         |
| `1.3.0-amd64-vi-vn-an-preview`              | Obraz kontenera z `vi-VN` ustawieniami regionalnymi i `vi-VN-An` głosem.              |
| `1.3.0-amd64-zh-cn-huihuirus-preview`       | Obraz kontenera z `zh-CN` ustawieniami regionalnymi i `zh-CN-HuihuiRUS` głosem.       |
| `1.3.0-amd64-zh-cn-kangkang-apollo-preview` | Obraz kontenera z `zh-CN` ustawieniami regionalnymi i `zh-CN-Kangkang-Apollo` głosem. |
| `1.3.0-amd64-zh-cn-yaoyao-apollo-preview`   | Obraz kontenera z `zh-CN` ustawieniami regionalnymi i `zh-CN-Yaoyao-Apollo` głosem.   |
| `1.3.0-amd64-zh-hk-danny-apollo-preview`    | Obraz kontenera z `zh-HK` ustawieniami regionalnymi i `zh-HK-Danny-Apollo` głosem.    |
| `1.3.0-amd64-zh-hk-tracy-apollo-preview`    | Obraz kontenera z `zh-HK` ustawieniami regionalnymi i `zh-HK-Tracy-Apollo` głosem.    |
| `1.3.0-amd64-zh-hk-tracyrus-preview`        | Obraz kontenera z `zh-HK` ustawieniami regionalnymi i `zh-HK-TracyRUS` głosem.        |
| `1.3.0-amd64-zh-tw-hanhanrus-preview`       | Obraz kontenera z `zh-TW` ustawieniami regionalnymi i `zh-TW-HanHanRUS` głosem.       |
| `1.3.0-amd64-zh-tw-yating-apollo-preview`   | Obraz kontenera z `zh-TW` ustawieniami regionalnymi i `zh-TW-Yating-Apollo` głosem.   |
| `1.3.0-amd64-zh-tw-zhiwei-apollo-preview`   | Obraz kontenera z `zh-TW` ustawieniami regionalnymi i `zh-TW-Zhiwei-Apollo` głosem.   |
| `1.2.0-amd64-de-de-heddarus-preview`        | Obraz kontenera z `de-DE` ustawieniami regionalnymi i `de-DE-Hedda` głosem.           |
| `1.2.0-amd64-de-de-heddarus-preview`        | Obraz kontenera z `de-DE` ustawieniami regionalnymi i `de-DE-HeddaRUS` głosem.        |
| `1.2.0-amd64-de-de-stefan-apollo-preview`   | Obraz kontenera z `de-DE` ustawieniami regionalnymi i `de-DE-Stefan-Apollo` głosem.   |
| `1.2.0-amd64-en-au-catherine-preview`       | Obraz kontenera z `en-AU` ustawieniami regionalnymi i `en-AU-Catherine` głosem.       |
| `1.2.0-amd64-en-au-hayleyrus-preview`       | Obraz kontenera z `en-AU` ustawieniami regionalnymi i `en-AU-HayleyRUS` głosem.       |
| `1.2.0-amd64-en-gb-george-apollo-preview`   | Obraz kontenera z `en-GB` ustawieniami regionalnymi i `en-GB-George-Apollo` głosem.   |
| `1.2.0-amd64-en-gb-hazelrus-preview`        | Obraz kontenera z `en-GB` ustawieniami regionalnymi i `en-GB-HazelRUS` głosem.        |
| `1.2.0-amd64-en-gb-susan-apollo-preview`    | Obraz kontenera z `en-GB` ustawieniami regionalnymi i `en-GB-Susan-Apollo` głosem.    |
| `1.2.0-amd64-en-in-heera-apollo-preview`    | Obraz kontenera z `en-IN` ustawieniami regionalnymi i `en-IN-Heera-Apollo` głosem.    |
| `1.2.0-amd64-en-in-priyarus-preview`        | Obraz kontenera z `en-IN` ustawieniami regionalnymi i `en-IN-PriyaRUS` głosem.        |
| `1.2.0-amd64-en-in-ravi-apollo-preview`     | Obraz kontenera z `en-IN` ustawieniami regionalnymi i `en-IN-Ravi-Apollo` głosem.     |
| `1.2.0-amd64-en-us-benjaminrus-preview`     | Obraz kontenera z `en-US` ustawieniami regionalnymi i `en-US-BenjaminRUS` głosem.     |
| `1.2.0-amd64-en-us-guy24krus-preview`       | Obraz kontenera z `en-US` ustawieniami regionalnymi i `en-US-Guy24kRUS` głosem.       |
| `1.2.0-amd64-en-us-jessa24krus-preview`     | Obraz kontenera z `en-US` ustawieniami regionalnymi i `en-US-Jessa24kRUS` głosem.     |
| `1.2.0-amd64-en-us-jessarus-preview`        | Obraz kontenera z `en-US` ustawieniami regionalnymi i `en-US-JessaRUS` głosem.        |
| `1.2.0-amd64-en-us-zirarus-preview`         | Obraz kontenera z `en-US` ustawieniami regionalnymi i `en-US-ZiraRUS` głosem.         |
| `1.2.0-amd64-es-es-helenarus-preview`       | Obraz kontenera z `es-ES` ustawieniami regionalnymi i `es-ES-HelenaRUS` głosem.       |
| `1.2.0-amd64-es-es-laura-apollo-preview`    | Obraz kontenera z `es-ES` ustawieniami regionalnymi i `es-ES-Laura-Apollo` głosem.    |
| `1.2.0-amd64-es-es-pablo-apollo-preview`    | Obraz kontenera z `es-ES` ustawieniami regionalnymi i `es-ES-Pablo-Apollo` głosem.    |
| `1.2.0-amd64-es-mx-hildarus-preview`        | Obraz kontenera z `es-MX` ustawieniami regionalnymi i `es-MX-HildaRUS` głosem.        |
| `1.2.0-amd64-es-mx-raul-apollo-preview`     | Obraz kontenera z `es-MX` ustawieniami regionalnymi i `es-MX-Raul-Apollo` głosem.     |
| `1.2.0-amd64-fr-ca-caroline-preview`        | Obraz kontenera z `fr-CA` ustawieniami regionalnymi i `fr-CA-Caroline` głosem.        |
| `1.2.0-amd64-fr-ca-harmonierus-preview`     | Obraz kontenera z `fr-CA` ustawieniami regionalnymi i `fr-CA-HarmonieRUS` głosem.     |
| `1.2.0-amd64-fr-fr-hortenserus-preview`     | Obraz kontenera z `fr-FR` ustawieniami regionalnymi i `fr-FR-HortenseRUS` głosem.     |
| `1.2.0-amd64-fr-fr-julie-apollo-preview`    | Obraz kontenera z `fr-FR` ustawieniami regionalnymi i `fr-FR-Julie-Apollo` głosem.    |
| `1.2.0-amd64-fr-fr-paul-apollo-preview`     | Obraz kontenera z `fr-FR` ustawieniami regionalnymi i `fr-FR-Paul-Apollo` głosem.     |
| `1.2.0-amd64-it-it-cosimo-apollo-preview`   | Obraz kontenera z `it-IT` ustawieniami regionalnymi i `it-IT-Cosimo-Apollo` głosem.   |
| `1.2.0-amd64-it-it-luciarus-preview`        | Obraz kontenera z `it-IT` ustawieniami regionalnymi i `it-IT-LuciaRUS` głosem.        |
| `1.2.0-amd64-ja-jp-ayumi-apollo-preview`    | Obraz kontenera z `ja-JP` ustawieniami regionalnymi i `ja-JP-Ayumi-Apollo` głosem.    |
| `1.2.0-amd64-ja-jp-harukarus-preview`       | Obraz kontenera z `ja-JP` ustawieniami regionalnymi i `ja-JP-HarukaRUS` głosem.       |
| `1.2.0-amd64-ja-jp-ichiro-apollo-preview`   | Obraz kontenera z `ja-JP` ustawieniami regionalnymi i `ja-JP-Ichiro-Apollo` głosem.   |
| `1.2.0-amd64-ko-kr-heamirus-preview`        | Obraz kontenera z `ko-KR` ustawieniami regionalnymi i `ko-KR-HeamiRUS` głosem.        |
| `1.2.0-amd64-pt-br-daniel-apollo-preview`   | Obraz kontenera z `pt-BR` ustawieniami regionalnymi i `pt-BR-Daniel-Apollo` głosem.   |
| `1.2.0-amd64-pt-br-heloisarus-preview`      | Obraz kontenera z `pt-BR` ustawieniami regionalnymi i `pt-BR-HeloisaRUS` głosem.      |
| `1.2.0-amd64-zh-cn-huihuirus-preview`       | Obraz kontenera z `zh-CN` ustawieniami regionalnymi i `zh-CN-HuihuiRUS` głosem.       |
| `1.2.0-amd64-zh-cn-kangkang-apollo-preview` | Obraz kontenera z `zh-CN` ustawieniami regionalnymi i `zh-CN-Kangkang-Apollo` głosem. |
| `1.2.0-amd64-zh-cn-yaoyao-apollo-preview`   | Obraz kontenera z `zh-CN` ustawieniami regionalnymi i `zh-CN-Yaoyao-Apollo` głosem.   |
| `1.1.0-amd64-de-de-hedda-preview`           | Obraz kontenera z `de-DE` ustawieniami regionalnymi i `de-DE-Hedda` głosem.           |
| `1.1.0-amd64-de-de-heddarus-preview`        | Obraz kontenera z `de-DE` ustawieniami regionalnymi i `de-DE-Hedda` głosem.           |
| `1.1.0-amd64-de-de-heddarus-preview`        | Obraz kontenera z `de-DE` ustawieniami regionalnymi i `de-DE-HeddaRUS` głosem.        |
| `1.1.0-amd64-de-de-stefan-apollo-preview`   | Obraz kontenera z `de-DE` ustawieniami regionalnymi i `de-DE-Stefan-Apollo` głosem.   |
| `1.1.0-amd64-en-au-catherine-preview`       | Obraz kontenera z `en-AU` ustawieniami regionalnymi i `en-AU-Catherine` głosem.       |
| `1.1.0-amd64-en-au-hayleyrus-preview`       | Obraz kontenera z `en-AU` ustawieniami regionalnymi i `en-AU-HayleyRUS` głosem.       |
| `1.1.0-amd64-en-gb-george-apollo-preview`   | Obraz kontenera z `en-GB` ustawieniami regionalnymi i `en-GB-George-Apollo` głosem.   |
| `1.1.0-amd64-en-gb-hazelrus-preview`        | Obraz kontenera z `en-GB` ustawieniami regionalnymi i `en-GB-HazelRUS` głosem.        |
| `1.1.0-amd64-en-gb-susan-apollo-preview`    | Obraz kontenera z `en-GB` ustawieniami regionalnymi i `en-GB-Susan-Apollo` głosem.    |
| `1.1.0-amd64-en-in-heera-apollo-preview`    | Obraz kontenera z `en-IN` ustawieniami regionalnymi i `en-IN-Heera-Apollo` głosem.    |
| `1.1.0-amd64-en-in-priyarus-preview`        | Obraz kontenera z `en-IN` ustawieniami regionalnymi i `en-IN-PriyaRUS` głosem.        |
| `1.1.0-amd64-en-in-ravi-apollo-preview`     | Obraz kontenera z `en-IN` ustawieniami regionalnymi i `en-IN-Ravi-Apollo` głosem.     |
| `1.1.0-amd64-en-us-benjaminrus-preview`     | Obraz kontenera z `en-US` ustawieniami regionalnymi i `en-US-BenjaminRUS` głosem.     |
| `1.1.0-amd64-en-us-guy24krus-preview`       | Obraz kontenera z `en-US` ustawieniami regionalnymi i `en-US-Guy24kRUS` głosem.       |
| `1.1.0-amd64-en-us-jessa24krus-preview`     | Obraz kontenera z `en-US` ustawieniami regionalnymi i `en-US-Jessa24kRUS` głosem.     |
| `1.1.0-amd64-en-us-jessarus-preview`        | Obraz kontenera z `en-US` ustawieniami regionalnymi i `en-US-JessaRUS` głosem.        |
| `1.1.0-amd64-en-us-zirarus-preview`         | Obraz kontenera z `en-US` ustawieniami regionalnymi i `en-US-ZiraRUS` głosem.         |
| `1.1.0-amd64-es-es-helenarus-preview`       | Obraz kontenera z `es-ES` ustawieniami regionalnymi i `es-ES-HelenaRUS` głosem.       |
| `1.1.0-amd64-es-es-laura-apollo-preview`    | Obraz kontenera z `es-ES` ustawieniami regionalnymi i `es-ES-Laura-Apollo` głosem.    |
| `1.1.0-amd64-es-es-pablo-apollo-preview`    | Obraz kontenera z `es-ES` ustawieniami regionalnymi i `es-ES-Pablo-Apollo` głosem.    |
| `1.1.0-amd64-es-mx-hildarus-preview`        | Obraz kontenera z `es-MX` ustawieniami regionalnymi i `es-MX-HildaRUS` głosem.        |
| `1.1.0-amd64-es-mx-raul-apollo-preview`     | Obraz kontenera z `es-MX` ustawieniami regionalnymi i `es-MX-Raul-Apollo` głosem.     |
| `1.1.0-amd64-fr-ca-caroline-preview`        | Obraz kontenera z `fr-CA` ustawieniami regionalnymi i `fr-CA-Caroline` głosem.        |
| `1.1.0-amd64-fr-ca-harmonierus-preview`     | Obraz kontenera z `fr-CA` ustawieniami regionalnymi i `fr-CA-HarmonieRUS` głosem.     |
| `1.1.0-amd64-fr-fr-hortenserus-preview`     | Obraz kontenera z `fr-FR` ustawieniami regionalnymi i `fr-FR-HortenseRUS` głosem.     |
| `1.1.0-amd64-fr-fr-julie-apollo-preview`    | Obraz kontenera z `fr-FR` ustawieniami regionalnymi i `fr-FR-Julie-Apollo` głosem.    |
| `1.1.0-amd64-fr-fr-paul-apollo-preview`     | Obraz kontenera z `fr-FR` ustawieniami regionalnymi i `fr-FR-Paul-Apollo` głosem.     |
| `1.1.0-amd64-it-it-cosimo-apollo-preview`   | Obraz kontenera z `it-IT` ustawieniami regionalnymi i `it-IT-Cosimo-Apollo` głosem.   |
| `1.1.0-amd64-it-it-luciarus-preview`        | Obraz kontenera z `it-IT` ustawieniami regionalnymi i `it-IT-LuciaRUS` głosem.        |
| `1.1.0-amd64-ja-jp-ayumi-apollo-preview`    | Obraz kontenera z `ja-JP` ustawieniami regionalnymi i `ja-JP-Ayumi-Apollo` głosem.    |
| `1.1.0-amd64-ja-jp-harukarus-preview`       | Obraz kontenera z `ja-JP` ustawieniami regionalnymi i `ja-JP-HarukaRUS` głosem.       |
| `1.1.0-amd64-ja-jp-ichiro-apollo-preview`   | Obraz kontenera z `ja-JP` ustawieniami regionalnymi i `ja-JP-Ichiro-Apollo` głosem.   |
| `1.1.0-amd64-ko-kr-heamirus-preview`        | Obraz kontenera z `ko-KR` ustawieniami regionalnymi i `ko-KR-HeamiRUS` głosem.        |
| `1.1.0-amd64-pt-br-daniel-apollo-preview`   | Obraz kontenera z `pt-BR` ustawieniami regionalnymi i `pt-BR-Daniel-Apollo` głosem.   |
| `1.1.0-amd64-pt-br-heloisarus-preview`      | Obraz kontenera z `pt-BR` ustawieniami regionalnymi i `pt-BR-HeloisaRUS` głosem.      |
| `1.1.0-amd64-zh-cn-huihuirus-preview`       | Obraz kontenera z `zh-CN` ustawieniami regionalnymi i `zh-CN-HuihuiRUS` głosem.       |
| `1.1.0-amd64-zh-cn-kangkang-apollo-preview` | Obraz kontenera z `zh-CN` ustawieniami regionalnymi i `zh-CN-Kangkang-Apollo` głosem. |
| `1.1.0-amd64-zh-cn-yaoyao-apollo-preview`   | Obraz kontenera z `zh-CN` ustawieniami regionalnymi i `zh-CN-Yaoyao-Apollo` głosem.   |
| `1.0.0-amd64-en-us-benjaminrus-preview`     | Obraz kontenera z `en-US` ustawieniami regionalnymi i `en-US-BenjaminRUS` głosem.     |
| `1.0.0-amd64-en-us-guy24krus-preview`       | Obraz kontenera z `en-US` ustawieniami regionalnymi i `en-US-Guy24kRUS` głosem.       |
| `1.0.0-amd64-en-us-jessa24krus-preview`     | Obraz kontenera z `en-US` ustawieniami regionalnymi i `en-US-Jessa24kRUS` głosem.     |
| `1.0.0-amd64-en-us-jessarus-preview`        | Obraz kontenera z `en-US` ustawieniami regionalnymi i `en-US-JessaRUS` głosem.        |
| `1.0.0-amd64-en-us-zirarus-preview`         | Obraz kontenera z `en-US` ustawieniami regionalnymi i `en-US-ZiraRUS` głosem.         |
| `1.0.0-amd64-zh-cn-huihuirus-preview`       | Obraz kontenera z `zh-CN` ustawieniami regionalnymi i `zh-CN-HuihuiRUS` głosem.       |
| `1.0.0-amd64-zh-cn-kangkang-apollo-preview` | Obraz kontenera z `zh-CN` ustawieniami regionalnymi i `zh-CN-Kangkang-Apollo` głosem. |
| `1.0.0-amd64-zh-cn-yaoyao-apollo-preview`   | Obraz kontenera z `zh-CN` ustawieniami regionalnymi i `zh-CN-Yaoyao-Apollo` głosem.   |

## <a name="key-phrase-extraction"></a>Wyodrębnianie kluczowych fraz

Obraz kontenera [wyodrębnianie kluczowych fraz][ta-kp] można znaleźć w obszarze `mcr.microsoft.com` syndykaty rejestru kontenerów. Znajduje się w `azure-cognitive-services` repozytorium i ma nazwę `keyphrase` . W pełni kwalifikowana nazwa obrazu kontenera to, `mcr.microsoft.com/azure-cognitive-services/keyphrase` .

Ten obraz kontenera ma dostępne następujące znaczniki:

| Tagi obrazu                    | Uwagi |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.009301-amd64-preview`    |       |
| `1.1.008510001-amd64-preview` |       |
| `1.1.007750002-amd64-preview` |       |
| `1.1.007360001-amd64-preview` |       |
| `1.1.006770001-amd64-preview` |       |

## <a name="language-detection"></a>Wykrywanie języka

Obraz kontenera [wykrywanie języka][ta-la] można znaleźć w obszarze `mcr.microsoft.com` syndykaty rejestru kontenerów. Znajduje się w `azure-cognitive-services` repozytorium i ma nazwę `language` . W pełni kwalifikowana nazwa obrazu kontenera to, `mcr.microsoft.com/azure-cognitive-services/language` .

Ten obraz kontenera ma dostępne następujące znaczniki:

| Tagi obrazu                    | Uwagi |
|-------------------------------|:------|
| `latest`                      |       |
| `1.1.009301-amd64-preview`    |       |
| `1.1.008510001-amd64-preview` |       |
| `1.1.007750002-amd64-preview` |       |
| `1.1.007360001-amd64-preview` |       |
| `1.1.006770001-amd64-preview` |       |

## <a name="sentiment-analysis"></a>Analiza tonacji

Obraz kontenera [Analiza tonacji][ta-se] można znaleźć w obszarze `mcr.microsoft.com` syndykaty rejestru kontenerów. Znajduje się w `azure-cognitive-services` repozytorium i ma nazwę `sentiment` . W pełni kwalifikowana nazwa obrazu kontenera to, `mcr.microsoft.com/azure-cognitive-services/sentiment` .

Ten obraz kontenera ma dostępne następujące znaczniki:

| Tagi obrazu | Uwagi                                         |
|------------|:----------------------------------------------|
| `latest`   |                                               |
| `3.0-en`   | Analiza tonacji v3 (angielski)               |
| `3.0-es`   | Analiza tonacji v3 (hiszpański)               |
| `3.0-fr`   | Analiza tonacji v3 (francuski)                |
| `3.0-it`   | Analiza tonacji v3 (włoski)               |
| `3.0-de`   | Analiza tonacji v3 (niemiecki)                |
| `3.0-zh`   | Analiza tonacji v3 (chiński uproszczony)  |
| `3.0-zht`  | Analiza tonacji v3 (chiński tradycyjny) |
| `3.0-ja`   | Analiza tonacji v3 (japoński)              |
| `3.0-pt`   | Analiza tonacji v3 (portugalski)            |
| `3.0-nl`   | Analiza tonacji v3 (holenderski)                 |
| `1.1.009301-amd64-preview`    | analiza tonacji v2      |
| `1.1.008510001-amd64-preview` |       |
| `1.1.007750002-amd64-preview` |       |
| `1.1.007360001-amd64-preview` |       |
| `1.1.006770001-amd64-preview` |       |

[ad-containers]: ../anomaly-Detector/anomaly-detector-container-howto.md
[cv-containers]: ../computer-vision/computer-vision-how-to-install-containers.md
[fa-containers]: ../face/face-how-to-install-containers.md
[fr-containers]: ../form-recognizer/form-recognizer-container-howto.md
[lu-containers]: ../luis/luis-container-howto.md
[sp-stt]: ../speech-service/speech-container-howto.md?tabs=stt
[sp-cstt]: ../speech-service/speech-container-howto.md?tabs=cstt
[sp-tts]: ../speech-service/speech-container-howto.md?tabs=tts
[sp-ctts]: ../speech-service/speech-container-howto.md?tabs=ctts
[ta-kp]: ../text-analytics/how-tos/text-analytics-how-to-install-containers.md?tabs=keyphrase
[ta-la]: ../text-analytics/how-tos/text-analytics-how-to-install-containers.md?tabs=language
[ta-se]: ../text-analytics/how-tos/text-analytics-how-to-install-containers.md?tabs=sentiment
