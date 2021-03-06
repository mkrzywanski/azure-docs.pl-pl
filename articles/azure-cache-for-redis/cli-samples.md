---
title: Zarządzanie usługą Azure cache for Redis za pomocą interfejsu wiersza polecenia platformy Azure
description: 'Przykłady interfejsu wiersza polecenia platformy Azure służące do zarządzania pamięcią podręczną platformy Azure dla Redis: tworzenie pamięci podręcznej, usuwanie pamięci podręcznej, informacje o nazwie hosta, portach i kluczach, łączenie aplikacji sieci Web.'
author: yegu-ms
ms.author: yegu
ms.service: cache
ms.devlang: azurecli
ms.topic: conceptual
ms.date: 04/14/2017
ms.custom: devx-track-azurecli
ms.openlocfilehash: 9bfdd2d03b3ab6edd04a641787475930435a9ffc
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/31/2020
ms.locfileid: "87499606"
---
# <a name="manage-azure-cache-for-redis-with-azure-cli"></a>Zarządzanie usługą Azure cache for Redis za pomocą interfejsu wiersza polecenia platformy Azure

Poniższa tabela zawiera linki do skryptów bash utworzonych za pomocą interfejsu wiersza polecenia platformy Azure.

| Tworzenie pamięci podręcznej | Opis |
| ------------ | ----------- |
| [Tworzenie pamięci podręcznej](./scripts/create-cache.md) | Tworzy grupę zasobów i pamięć podręczną Azure w warstwie Podstawowa dla Redis. |
| [Tworzenie pamięci podręcznej Premium przy użyciu klastrowania](./scripts/create-premium-cache-cluster.md) | Tworzy grupę zasobów i pamięć podręczną warstwy Premium z włączoną obsługą klastrowania.|
| [Pobierz szczegóły pamięci podręcznej](./scripts/show-cache.md) | Pobiera szczegóły wystąpienia usługi Azure cache for Redis, w tym stanu aprowizacji. |
| [Pobieranie nazwy hosta, portów i kluczy](./scripts/cache-keys-ports.md) | Pobiera nazwę hosta, porty i klucze dla wystąpienia usługi Azure cache for Redis. |
|**Aplikacja sieci Web plus pamięć podręczna**| **Opis**|
| [Łączenie aplikacji internetowej z pamięcią podręczną Azure Cache for Redis](./../app-service/scripts/cli-connect-to-redis.md) | Tworzy aplikację internetową platformy Azure i pamięć podręczną platformy Azure dla usługi Redis, a następnie dodaje szczegóły połączenia Redis do ustawień aplikacji. |
|**Usuń pamięć podręczną**| **Opis** |
| [Usuwanie pamięci podręcznej](./scripts/delete-cache.md) | Usuwa wystąpienie usługi Azure cache for Redis  |

Aby uzyskać więcej informacji na temat interfejsu wiersza polecenia platformy Azure, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure](https://docs.microsoft.com/cli/azure/install-azure-cli) i [wprowadzenie do interfejsu wiersza polecenia platformy Azure](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli).
