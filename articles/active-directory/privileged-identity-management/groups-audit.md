---
title: Wyświetl raport inspekcji dla przypisań uprzywilejowanych grup dostępu w Privileged Identity Management (PIM) — Azure AD | Microsoft Docs
description: Wyświetl historię działań i inspekcji dla przypisań uprzywilejowanych grup dostępu w Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
editor: ''
ms.service: active-directory
ms.topic: article
ms.workload: identity
ms.subservice: pim
ms.date: 07/27/2020
ms.author: curtand
ms.reviewer: shaunliu
ms.collection: M365-identity-device-management
ms.openlocfilehash: d9bbc90776ca007b84d5f67c50f8550ee9c881c7
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/31/2020
ms.locfileid: "87506045"
---
# <a name="audit-activity-history-for-privileged-access-group-assignments-preview-in-privileged-identity-management"></a>Inspekcja historii aktywności dla przypisań uprzywilejowanych grup dostępu (wersja zapoznawcza) w Privileged Identity Management

Za pomocą Privileged Identity Management (PIM) można wyświetlać działania, aktywacje i historię inspekcji dla członków i właścicieli grup usługi Azure Privileged Access w organizacji Azure Active Directory (Azure AD).

> [!NOTE]
> Jeśli organizacja ma funkcje zarządzania, które są używane przez usługę zarządzania [zasobami delegowanymi przez platformę Azure](../../lighthouse/concepts/azure-delegated-resource-management.md), w tym miejscu nie będą wyświetlane przypisania ról autoryzowane przez tego dostawcę usług.

Wykonaj następujące kroki, aby wyświetlić historię inspekcji dla uprzywilejowanych grup dostępu.

## <a name="view-resource-audit-history"></a>Wyświetl historię inspekcji zasobów

**Inspekcja zasobów** umożliwia wyświetlenie wszystkich działań skojarzonych z uprzywilejowanymi grupami dostępu.

1. Otwórz **Azure AD Privileged Identity Management**.

1. Wybierz pozycję **dostęp uprzywilejowany (wersja zapoznawcza)**.

1. W obszarze **działanie**wybierz pozycję **Inspekcja zasobów**.

1. Przefiltruj historię przy użyciu wstępnie zdefiniowanej daty lub niestandardowego zakresu.

    ![Lista inspekcji zasobów z filtrami](media/groups-audit/groups-resource-audit.png)

## <a name="view-my-audit"></a>Wyświetl moją inspekcję

Moja Inspekcja umożliwia wyświetlenie własnej aktywności roli użytkownika.

1. Otwórz **Azure AD Privileged Identity Management**.

1. Wybierz pozycję **dostęp uprzywilejowany (wersja zapoznawcza)**.

1. Wybierz członka lub grupę, dla których chcesz wyświetlić historię inspekcji.

1. Wybierz pozycję **Moje inspekcje**.

1. Przefiltruj historię przy użyciu wstępnie zdefiniowanej daty lub niestandardowego zakresu.

    ![Lista inspekcji dla bieżącego użytkownika](media/azure-pim-resource-rbac/my-audit-time.png)

## <a name="next-steps"></a>Następne kroki

- [Przypisywanie członkostwa w grupie i własności w Privileged Identity Management](groups-assign-member-owner.md)
- [Zatwierdzanie lub odrzucanie żądań dla uprzywilejowanych grup dostępu w usłudze PIM](groups-approval-workflow.md)
- [Wyświetlanie historii inspekcji dla ról usługi Azure AD w usłudze PIM](groups-audit.md)
