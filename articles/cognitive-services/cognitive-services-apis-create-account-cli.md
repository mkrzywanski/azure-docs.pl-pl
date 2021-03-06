---
title: Tworzenie zasobu Cognitive Services przy użyciu interfejsu wiersza polecenia platformy Azure
titleSuffix: Azure Cognitive Services
description: Rozpocznij pracę z usługą Azure Cognitive Services, tworząc i subskrybując zasób przy użyciu interfejsu wiersza polecenia platformy Azure.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 07/27/2020
ms.author: aahi
ms.openlocfilehash: b77d69248059a494d823afcd149382dd52b0e49d
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/28/2020
ms.locfileid: "87317131"
---
# <a name="create-a-cognitive-services-resource-using-the-azure-command-line-interfacecli"></a>Tworzenie zasobu Cognitive Services przy użyciu interfejsu wiersza polecenia platformy Azure (CLI)

Skorzystaj z tego przewodnika Szybki Start, aby rozpocząć pracę z usługą Azure Cognitive Services przy użyciu [interfejsu wiersza polecenia platformy Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). Cognitive Services są reprezentowane przez [zasoby](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal) platformy Azure utworzone w ramach subskrypcji platformy Azure. Po utworzeniu zasobu Użyj kluczy i punktu końcowego wygenerowanego do uwierzytelniania aplikacji. 


W tym przewodniku szybki start dowiesz się, jak zarejestrować się w usłudze Azure Cognitive Services i utworzyć konto z subskrypcją pojedynczego lub wielousługowego za pomocą [interfejsu wiersza polecenia (CLI) platformy Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). Te usługi są reprezentowane przez [zasoby](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal)platformy Azure, które umożliwiają nawiązywanie połączenia z co najmniej jedną interfejsy API usług Cognitive Services platformy Azure.

[!INCLUDE [cognitive-services-subscription-types](../../includes/cognitive-services-subscription-types.md)]

## <a name="prerequisites"></a>Wymagania wstępne

* Prawidłowa subskrypcja platformy Azure — [Utwórz ją](https://azure.microsoft.com/free/) bezpłatnie.
* [Interfejs wiersza polecenia platformy Azure (CLI)](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)

## <a name="install-the-azure-cli-and-sign-in"></a>Instalowanie interfejsu wiersza polecenia platformy Azure i logowanie 

Zainstaluj [interfejs wiersza polecenia platformy Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). Aby zalogować się do lokalnej instalacji interfejsu wiersza polecenia, uruchom polecenie [AZ login](https://docs.microsoft.com/cli/azure/reference-index#az-login) :

```azurecli-interactive
az login
```

Możesz również użyć zielonego przycisku **Wypróbuj** , aby uruchomić te polecenia w przeglądarce.
 
## <a name="create-a-new-azure-cognitive-services-resource-group"></a>Utwórz nową grupę zasobów usługi Azure Cognitive Services

Przed utworzeniem zasobu Cognitive Services należy mieć grupę zasobów platformy Azure, która będzie zawierać zasób. W przypadku tworzenia nowego zasobu można utworzyć nową grupę zasobów lub użyć istniejącej grupy. W tym artykule pokazano, jak utworzyć nową grupę zasobów.

### <a name="choose-your-resource-group-location"></a>Wybierz lokalizację grupy zasobów

Aby utworzyć zasób, potrzebna jest jedna z lokalizacji platformy Azure dostępnych dla Twojej subskrypcji. Listę dostępnych lokalizacji można pobrać za pomocą polecenia [AZ Account List-Locations](/cli/azure/account#az-account-list-locations) . Większość Cognitive Services można uzyskać z kilku lokalizacji. Wybierz najbliżej siebie lub Zobacz, które lokalizacje są dostępne dla usługi.

> [!IMPORTANT]
> * Zapamiętaj swoją lokalizację platformy Azure, ponieważ będzie ona potrzebna podczas wywoływania Cognitive Services platformy Azure.
> * Dostępność niektórych Cognitive Services może różnić się w zależności od regionu. Aby uzyskać więcej informacji, zobacz [produkty platformy Azure według regionów](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services).  

```azurecli-interactive
az account list-locations \
    --query "[].{Region:name}" \
    --out table
```

Po utworzeniu lokalizacji platformy Azure Utwórz nową grupę zasobów w interfejsie wiersza polecenia platformy Azure przy użyciu poleceń [AZ Group Create](/cli/azure/group#az-group-create) .

W poniższym przykładzie Zastąp lokalizację platformy Azure `westus2` jedną z lokalizacji platformy Azure dostępnych dla Twojej subskrypcji.

```azurecli-interactive
az group create \
    --name cognitive-services-resource-group \
    --location westus2
```

## <a name="create-a-cognitive-services-resource"></a>Tworzenie zasobu usług Cognitive Services

### <a name="choose-a-cognitive-service-and-pricing-tier"></a>Wybieranie usługi poznawczej i warstwy cenowej

Podczas tworzenia nowego zasobu należy znać "rodzaj" usługi, która ma być używana, wraz z żądaną [warstwą cenową](https://azure.microsoft.com/pricing/details/cognitive-services/) (lub jednostką SKU). Te i inne informacje są używane jako parametry podczas tworzenia zasobu.

### <a name="multi-service"></a>Wiele usług

| Usługa                    | Rodzaj                      |
|----------------------------|---------------------------|
| Wiele usług. Aby uzyskać więcej informacji, zobacz stronę z [cennikiem](https://azure.microsoft.com/pricing/details/cognitive-services/) .            | `CognitiveServices`     |


> [!NOTE]
> Wiele Cognitive Services poniżej ma bezpłatną warstwę, której można użyć do wypróbowania usługi. Aby skorzystać z warstwy Bezpłatna, użyj `F0` jako jednostki SKU dla zasobu.

### <a name="vision"></a>Widzenie

| Usługa                    | Rodzaj                      |
|----------------------------|---------------------------|
| Przetwarzanie obrazów            | `ComputerVision`          |
| Custom Vision — przewidywanie | `CustomVision.Prediction` |
| Custom Vision — szkolenie   | `CustomVision.Training`   |
| Rozpoznawanie twarzy                       | `Face`                    |
| Rozpoznawanie formularzy            | `FormRecognizer`          |
| Rozpoznawanie pisma odręcznego             | `InkRecognizer`           |

### <a name="search"></a>Wyszukiwanie

| Usługa            | Rodzaj                  |
|--------------------|-----------------------|
| Automatyczne sugerowanie Bing   | `Bing.Autosuggest.v7` |
| Wyszukiwanie niestandardowe Bing | `Bing.CustomSearch`   |
| Wyszukiwanie jednostek Bing | `Bing.EntitySearch`   |
| Wyszukiwanie Bing        | `Bing.Search.v7`      |
| Sprawdzanie pisowni Bing   | `Bing.SpellCheck.v7`  |

### <a name="speech"></a>Mowa

| Usługa            | Rodzaj                 |
|--------------------|----------------------|
| Usługi mowy    | `SpeechServices`     |
| Rozpoznawanie mowy | `SpeakerRecognition` |

### <a name="language"></a>Język

| Usługa            | Rodzaj                |
|--------------------|---------------------|
| Zrozumienie formularza | `FormUnderstanding` |
| LUIS               | `LUIS`              |
| QnA Maker          | `QnAMaker`          |
| Analiza tekstu     | `TextAnalytics`     |
| Tłumaczenie tekstu   | `TextTranslation`   |

### <a name="decision"></a>Decyzja

| Usługa           | Rodzaj               |
|-------------------|--------------------|
| Narzędzie do wykrywania anomalii  | `AnomalyDetector`  |
| Content Moderator | `ContentModerator` |
| Personalizacja      | `Personalizer`     |

Listę dostępnych rodzajów usługi poznawczej można znaleźć za pomocą polecenia [AZ cognitiveservices Account List-Kinds](https://docs.microsoft.com/cli/azure/cognitiveservices/account?view=azure-cli-latest#az-cognitiveservices-account-list-kinds) :

```azurecli-interactive
az cognitiveservices account list-kinds
```

### <a name="add-a-new-resource-to-your-resource-group"></a>Dodawanie nowego zasobu do grupy zasobów

Aby utworzyć i subskrybować nowy zasób Cognitive Services, użyj polecenia [AZ cognitiveservices Account Create](https://docs.microsoft.com/cli/azure/cognitiveservices/account?view=azure-cli-latest#az-cognitiveservices-account-create) . To polecenie dodaje nowy zasób rozliczany do utworzonej wcześniej grupy zasobów. Podczas tworzenia nowego zasobu należy znać "rodzaj" usługi, która ma być używana, wraz z jej warstwą cenową (lub jednostką SKU) i lokalizacją platformy Azure:

Można utworzyć zasób F0 (wolny) dla detektora anomalii o nazwie `anomaly-detector-resource` przy użyciu poniższego polecenia.

```azurecli-interactive
az cognitiveservices account create \
    --name anomaly-detector-resource \
    --resource-group cognitive-services-resource-group \
    --kind AnomalyDetector \
    --sku F0 \
    --location westus2 \
    --yes
```

[!INCLUDE [Register Azure resource for subscription](./includes/register-resource-subscription.md)]

## <a name="get-the-keys-for-your-resource"></a>Pobierz klucze dla zasobu

Aby zalogować się do lokalnej instalacji interfejsu wiersza polecenia (CLI), użyj polecenia [AZ login](https://docs.microsoft.com/cli/azure/reference-index?view=azure-cli-latest#az-login) .

```azurecli-interactive
az login
```

Użyj polecenia [AZ cognitiveservices Account Keys list](https://docs.microsoft.com/cli/azure/cognitiveservices/account/keys?view=azure-cli-latest#az-cognitiveservices-account-keys-list) , aby uzyskać klucze dla zasobu usługi poznawczej.

```azurecli-interactive
    az cognitiveservices account keys list \
    --name anomaly-detector-resource \
    --resource-group cognitive-services-resource-group
```

[!INCLUDE [cognitive-services-environment-variables](../../includes/cognitive-services-environment-variables.md)]

## <a name="pricing-tiers-and-billing"></a>Warstwy cenowe i rozliczenia

Warstwy cenowe (oraz opłata naliczana) są zależne od liczby wysyłanych transakcji przy użyciu informacji o uwierzytelnianiu. Każda warstwa cenowa określa:
* Maksymalna liczba dozwolonych transakcji na sekundę (TPS).
* funkcje usługi są włączone w ramach warstwy cenowej.
* Koszt wstępnie zdefiniowanej liczby transakcji. Przekroczenie tej kwoty spowoduje dodatkową opłatą określoną w [szczegółach cennika](https://azure.microsoft.com/pricing/details/cognitive-services/custom-vision-service/) usługi.

## <a name="get-current-quota-usage-for-your-resource"></a>Pobierz bieżące użycie przydziału dla zasobu

Użyj polecenia [AZ cognitiveservices Account List-Usage](https://docs.microsoft.com/cli/azure/cognitiveservices/account?view=azure-cli-latest#az-cognitiveservices-account-list-usage) , aby uzyskać użycie zasobu usługi poznawczej.

```azurecli-interactive
az cognitiveservices account list-usage \
    --name anomaly-detector-resource \
    --resource-group cognitive-services-resource-group \
    --subscription subscription-name
```

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Jeśli chcesz wyczyścić i usunąć zasób Cognitive Services, można go usunąć lub grupy zasobów. Usunięcie grupy zasobów spowoduje również usunięcie wszystkich innych zasobów znajdujących się w grupie.

Aby usunąć grupę zasobów i skojarzone z nią zasoby, użyj polecenia AZ Group Delete.

```azurecli-interactive
az group delete --name cognitive-services-resource-group
```

## <a name="see-also"></a>Zobacz także

* [Uwierzytelnianie żądań w usłudze Azure Cognitive Services](authentication.md)
* [Co to jest platforma Azure Cognitive Services?](Welcome.md)
* [Obsługa języka naturalnego](language-support.md)
* [Obsługa kontenerów platformy Docker](cognitive-services-container-support.md)
