---
title: dołączanie pliku
description: dołączanie pliku
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 02/01/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 1aca39a7ff162aa3c42fdb3ca5999c71091ec02e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "67182964"
---
 Jeśli używasz Azure Cloud Shell, zaloguj się do konta platformy Azure automatycznie po kliknięciu przycisku "Wypróbuj". Aby zalogować się lokalnie, Otwórz konsolę programu PowerShell z podwyższonym poziomem uprawnień i uruchom polecenie cmdlet w celu nawiązania połączenia.

```azurepowershell
Connect-AzAccount
```

Jeśli masz więcej niż jedną subskrypcję, uzyskaj listę subskrypcji platformy Azure.

```azurepowershell-interactive
Get-AzSubscription
```

Wskaż subskrypcję, której chcesz użyć.

```azurepowershell-interactive
Select-AzSubscription -SubscriptionName "Name of subscription"
```