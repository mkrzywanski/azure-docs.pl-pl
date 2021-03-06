---
title: Tworzenie centrum zdarzeń za pomocą interfejsu wiersza polecenia platformy Azure — Azure Event Hubs | Microsoft Docs
description: W tym przewodniku Szybki start opisano tworzenie centrum zdarzeń za pomocą interfejsu wiersza polecenia platformy Azure oraz wysyłanie i odbieranie zdarzeń za pomocą języka Java.
ms.topic: quickstart
ms.date: 06/23/2020
ms.author: spelluru
ms.custom: devx-track-azurecli
ms.openlocfilehash: acdeb1e176be1676bd531eca33faf1e0e18f3ecd
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/31/2020
ms.locfileid: "87502345"
---
# <a name="quickstart-create-an-event-hub-using-azure-cli"></a>Szybki start: tworzenie centrum zdarzeń za pomocą interfejsu wiersza polecenia platformy Azure

Azure Event Hubs to platforma do pozyskiwania i strumieniowego przesyłania danych, która umożliwia odbieranie i przetwarzanie milionów zdarzeń na sekundę. Usługa Event Hubs pozwala przetwarzać i przechowywać zdarzenia, dane lub dane telemetryczne generowane przez rozproszone oprogramowanie i urządzenia. Dane wysłane do centrum zdarzeń mogą zostać przekształcone i zmagazynowane przy użyciu dowolnego dostawcy analityki czasu rzeczywistego lub adapterów przetwarzania wsadowego/magazynowania. Aby zapoznać się ze szczegółowym omówieniem usługi Event Hubs, zobacz [Omówienie usługi Event Hubs](event-hubs-about.md) i [Funkcje usługi Event Hubs](event-hubs-features.md).

W tym przewodniku Szybki start utworzysz centrum zdarzeń za pomocą interfejsu wiersza polecenia platformy Azure.

## <a name="prerequisites"></a>Wymagania wstępne
Do wykonania kroków tego przewodnika Szybki start jest potrzebna subskrypcja platformy Azure. Jeśli go nie masz, przed rozpoczęciem [Utwórz bezpłatne konto][] .

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Jeśli zdecydujesz się zainstalować interfejs wiersza polecenia platformy Azure i korzystać z niego lokalnie, ten samouczek będzie wymagał interfejsu wiersza polecenia platformy Azure w wersji 2.0.4 lub nowszej. Uruchom polecenie `az --version`, aby sprawdzić wersję. Jeśli konieczna będzie instalacja lub uaktualnienie interfejsu, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure]( /cli/azure/install-azure-cli).

## <a name="sign-in-to-azure"></a>Logowanie się na platformie Azure

Poniższe kroki nie są wymagane, jeśli uruchamiasz polecenia w usłudze Cloud Shell. Jeśli korzystasz z interfejsu wiersza polecenia lokalnie, wykonaj następujące kroki, aby zalogować się do platformy Azure i ustawić bieżącą subskrypcję:

Uruchom następujące polecenia, aby zalogować się na platformie Azure:

```azurecli-interactive
az login
```

Ustawianie kontekstu bieżącej subskrypcji. Zastąp wartość `MyAzureSub` nazwą subskrypcji platformy Azure, której chcesz użyć:

```azurecli-interactive
az account set --subscription MyAzureSub
``` 

## <a name="create-a-resource-group"></a>Tworzenie grupy zasobów
Grupa zasobów to logiczna kolekcja zasobów platformy Azure. Wszystkie zasoby są wdrażane i zarządzane w ramach grupy zasobów. Uruchom poniższe polecenie, aby utworzyć grupę zasobów:

```azurecli-interactive
# Create a resource group. Specify a name for the resource group.
az group create --name <resource group name> --location eastus
```

## <a name="create-an-event-hubs-namespace"></a>Tworzenie przestrzeni nazw usługi Event Hubs
Przestrzeń nazw usługi Event Hubs udostępnia unikatowy kontener zakresu przywoływany przy użyciu jego w pełni kwalifikowanej nazwy domeny, w którym można utworzyć jedno lub wiele centrów zdarzeń. Aby utworzyć przestrzeń nazw w grupie zasobów, uruchom następujące polecenie:

```azurecli-interactive
# Create an Event Hubs namespace. Specify a name for the Event Hubs namespace.
az eventhubs namespace create --name <Event Hubs namespace> --resource-group <resource group name> -l <region, for example: East US>
```

## <a name="create-an-event-hub"></a>Tworzenie centrum zdarzeń
Uruchom następujące polecenie, aby utworzyć centrum zdarzeń:

```azurecli-interactive
# Create an event hub. Specify a name for the event hub. 
az eventhubs eventhub create --name <event hub name> --resource-group <resource group name> --namespace-name <Event Hubs namespace>
```

Gratulacje! Za pomocą interfejsu wiersza polecenia platformy Azure utworzono przestrzeń nazw usługi Event Hubs i centrum zdarzeń w ramach tej przestrzeni nazw. 

## <a name="next-steps"></a>Następne kroki

W tym artykule utworzono grupę zasobów, przestrzeń nazw usługi Event Hubs i centrum zdarzeń. Aby uzyskać instrukcje krok po kroku dotyczące wysyłania zdarzeń do (lub) odbierania zdarzeń z centrum zdarzeń, zobacz samouczki **wysyłania i odbierania zdarzeń** : 

- [.NET Core](get-started-dotnet-standard-send-v2.md)
- [Java](get-started-java-send-v2.md)
- [Python](get-started-python-send-v2.md)
- [JavaScript](get-started-node-send-v2.md)
- [Przejdź](event-hubs-go-get-started-send.md)
- [C (tylko wysyłanie)](event-hubs-c-getstarted-send.md)
- [Apache Storm (tylko odbieranie)](event-hubs-storm-getstarted-receive.md)

[Utwórz bezpłatne konto]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio
[Install the Azure CLI]: /cli/azure/install-azure-cli
[az group create]: /cli/azure/group#az_group_create
[fully qualified domain name]: https://wikipedia.org/wiki/Fully_qualified_domain_name
