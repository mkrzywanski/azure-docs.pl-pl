---
title: Importowanie specyfikacji interfejsu OpenAPI przy użyciu witryny Azure Portal | Microsoft Docs
description: Dowiedz się, jak zaimportować specyfikację interfejsu OpenAPI za pomocą usługi API Management.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 04/20/2020
ms.author: apimpm
ms.openlocfilehash: 2e4dee74eb0c50e8e12d3f9ff0dccdd83271ea65
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/29/2020
ms.locfileid: "82202924"
---
# <a name="import-an-openapi-specification"></a>Importowanie specyfikacji interfejsu OpenAPI

W tym artykule pokazano, jak zaimportować interfejs API zaplecza „Specyfikacja interfejsu OpenAPI” znajdujący się pod adresem https://conferenceapi.azurewebsites.net?format=json. Ten interfejs API zaplecza jest zapewniany przez firmę Microsoft i hostowany na platformie Azure. Artykuł przedstawia również sposób testowania interfejsu API usługi APIM.

W tym artykule omówiono sposób wykonywania następujących zadań:

> [!div class="checklist"]
> * Importowanie interfejsu API zaplecza „Specyfikacja interfejsu OpenAPI”
> * Testowanie interfejsu API w witrynie Azure Portal
> * Testowanie interfejsu API w portalu dla deweloperów

## <a name="prerequisites"></a>Wymagania wstępne

Wykonaj procedury przedstawione w następującym przewodniku Szybki start: [Tworzenie wystąpienia usługi Azure API Management](get-started-create-service-instance.md)

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="import-and-publish-a-back-end-api"></a><a name="create-api"> </a>Importowanie i publikowanie interfejsu API zaplecza

1. Przejdź do usługi API Management w Azure Portal i wybierz opcję **interfejsy API** z menu.
2. Wybierz pozycję **Specyfikacja interfejsu OpenAPI** z listy **Dodawanie nowego interfejsu API**.

    ![Specyfikacja OpenAPI](./media/import-api-from-oas/oas-api.png)
3. Wprowadź ustawienia interfejsu API. Możesz ustawić wartości podczas tworzenia lub skonfigurować je później, przechodząc do karty **Ustawienia** . Ustawienia są wyjaśnione w samouczku [Importowanie i publikowanie pierwszego interfejsu API](import-and-publish.md#-import-and-publish-a-backend-api) .
4. Wybierz przycisk **Utwórz**.

> [!NOTE]
> Ograniczenia importu interfejsu API są udokumentowane w [innym artykule](api-management-api-import-restrictions.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-append-apis.md)]

[!INCLUDE [api-management-define-api-topics.md](../../includes/api-management-define-api-topics.md)]

## <a name="next-steps"></a>Następne kroki

> [!div class="nextstepaction"]
> [Przekształcanie i ochrona opublikowanego interfejsu API](transform-api.md)
