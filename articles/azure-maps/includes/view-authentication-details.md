---
title: Wyświetl szczegóły uwierzytelniania Azure Maps
description: Użyj Azure Portal, aby wyświetlić szczegóły uwierzytelniania Azure Maps.
author: anastasia-ms
ms.author: v-stharr
ms.date: 06/17/2020
ms.topic: include
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: a6ffcbf5a8c36958dd3ea74de4d826fe25a1139c
ms.sourcegitcommit: 0e8a4671aa3f5a9a54231fea48bcfb432a1e528c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/24/2020
ms.locfileid: "87126463"
---
Możesz wyświetlić szczegóły uwierzytelniania konta Azure Maps w Azure Portal. Na Twoim koncie w menu **Ustawienia** wybierz pozycję **uwierzytelnianie**.

![Szczegóły uwierzytelniania](../media/how-to-manage-authentication/how-to-view-auth.png)

Po utworzeniu konta Azure Maps `x-ms-client-id` wartość Azure Maps jest obecna na stronie Szczegóły uwierzytelniania Azure Portal. Ta wartość reprezentuje konto, które będzie używane na potrzeby żądań interfejsu API REST. Ta wartość powinna być przechowywana w konfiguracji aplikacji i pobierana przed przystąpieniem do wykonywania żądań HTTP podczas korzystania z uwierzytelniania usługi Azure AD z Azure Maps.
