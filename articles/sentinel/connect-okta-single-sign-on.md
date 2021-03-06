---
title: Łączenie danych logowania jednokrotnego usługi okta z usługą Azure wskaźnikowego | Microsoft Docs
description: Dowiedz się, jak połączyć dane logowania jednokrotnego w usłudze usługi okta z platformą Azure.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/17/2020
ms.author: yelevin
ms.openlocfilehash: 05a9b8009d896a2ee87df3e1c4493d249a887566
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/23/2020
ms.locfileid: "87083926"
---
# <a name="connect-your-okta-single-sign-on-to-azure-sentinel-with-azure-function"></a>Połącz Logowanie jednokrotne w usłudze usługi okta z platformą Azure — wskaźnikiem przy użyciu funkcji platformy Azure

> [!IMPORTANT]
> Łącznik danych logowania jednokrotnego usługi okta na platformie Azure jest obecnie w publicznej wersji zapoznawczej.
> Ta funkcja jest dostępna bez umowy dotyczącej poziomu usług i nie jest zalecana w przypadku obciążeń produkcyjnych. Niektóre funkcje mogą być nieobsługiwane lub ograniczone. Aby uzyskać więcej informacji, zobacz [Uzupełniające warunki korzystania z wersji zapoznawczych platformy Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Łącznik logowania jednokrotnego (SSO, usługi okta Single-on) umożliwia łatwe łączenie dzienników rozwiązań zabezpieczeń logowania jednokrotnego [(SSO)](https://www.okta.com/products/single-sign-on/) w usłudze usługi okta z systemem Azure — do wyświetlania pulpitów nawigacyjnych, tworzenia niestandardowych alertów i ulepszania badania. Integracja między logowaniem jednokrotnym i usługi okta na platformie Azure to użycie Azure Functions do ściągania danych dzienników przy użyciu interfejsu API REST.

> [!NOTE]
> Dane będą przechowywane w lokalizacji geograficznej obszaru roboczego, w którym jest uruchamiany wskaźnik platformy Azure.

## <a name="configure-and-connect-okta-single-sign-on"></a>Konfigurowanie logowania jednokrotnego usługi okta i łączenie się z nim

Azure Functions umożliwia integrację i ściąganie zdarzeń i dzienników bezpośrednio z rejestracji jednokrotnej usługi okta i przekazywanie ich do usługi Azure wskaźnikowej.

1. W portalu wskaźnikowym platformy Azure kliknij pozycję **Łączniki danych** i wybierz pozycję **usługi okta Single Sign-on** Connector.

1. Wybierz pozycję **Otwórz stronę łącznika**.

1. Postępuj zgodnie z instrukcjami na stronie logowania jednokrotnego **usługi okta** .

## <a name="find-your-data"></a>Znajdowanie danych

Po pomyślnym nawiązaniu połączenia dane są wyświetlane w Log Analytics w tabeli **Okta_CL** .

## <a name="validate-connectivity"></a>Sprawdź poprawność łączności

Rozpoczęcie wyświetlania dzienników w Log Analytics może zająć więcej niż 20 minut.

## <a name="next-steps"></a>Następne kroki

W tym dokumencie pokazano, jak połączyć Logowanie jednokrotne w usłudze usługi okta z platformą Azure do nauki przy użyciu aplikacji funkcji platformy Azure. Aby dowiedzieć się więcej na temat platformy Azure, zobacz następujące artykuły:

- Dowiedz się [, jak uzyskać wgląd w dane oraz potencjalne zagrożenia](quickstart-get-visibility.md).
- Rozpocznij [wykrywanie zagrożeń za pomocą platformy Azure — wskaźnik](tutorial-detect-threats-built-in.md).
- [Używaj skoroszytów](tutorial-monitor-your-data.md) do monitorowania danych.

