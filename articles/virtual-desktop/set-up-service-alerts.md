---
title: Konfigurowanie alertów usługi dla pulpitu wirtualnego systemu Windows — Azure
description: Jak skonfigurować Azure Service Health do odbierania powiadomień usługi dla pulpitu wirtualnego systemu Windows.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: tutorial
ms.date: 06/11/2019
ms.author: helohr
manager: lizross
ms.openlocfilehash: effe3125a2d41f0410a1a0b3a87c41da39feb6d9
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/28/2020
ms.locfileid: "87291355"
---
# <a name="tutorial-set-up-service-alerts"></a>Samouczek: Konfigurowanie alertów usługi

>[!IMPORTANT]
>Ta zawartość dotyczy pulpitu wirtualnego systemu Windows z Azure Resource Manager obiektów pulpitu wirtualnego systemu Windows. Jeśli używasz pulpitu wirtualnego systemu Windows (klasycznego) bez Azure Resource Manager obiektów, zobacz [ten artykuł](./virtual-desktop-fall-2019/set-up-service-alerts-2019.md).

Azure Service Health służy do monitorowania problemów z usługami i klasyfikatorów kondycji dla pulpitu wirtualnego systemu Windows. Azure Service Health może powiadomić użytkownika o różnych typach alertów (np. wiadomości e-mail lub wiadomości SMS), pomóc Ci zrozumieć efekt problemu i aktualizować je w miarę rozwiązywania problemu. Azure Service Health może również pomóc w ograniczeniu przestoju i przygotowaniu się do planowanej konserwacji i zmian, które mogą mieć wpływ na dostępność zasobów.

Z tego samouczka dowiesz się, jak wykonywać następujące czynności:

> [!div class="checklist"]
> * Tworzenie i Konfigurowanie alertów usługi.

Aby dowiedzieć się więcej na temat Azure Service Health, zobacz dokumentację dotyczącą [kondycji platformy Azure](https://docs.microsoft.com/azure/service-health/).

## <a name="create-service-alerts"></a>Tworzenie alertów usługi

W tej sekcji przedstawiono sposób konfigurowania Azure Service Health i konfigurowania powiadomień, do których można uzyskać dostęp na Azure Portal. Można skonfigurować różne typy alertów i zaplanować ich powiadomienie w odpowiednim czasie.

### <a name="recommended-service-alerts"></a>Zalecane alerty usługi

Zalecamy tworzenie alertów usługi dla następujących typów zdarzeń kondycji:

- **Problem z usługą:** Otrzymuj powiadomienia o najważniejszych problemach, które mają wpływ na łączność użytkowników z usługą lub z możliwością zarządzania dzierżawcą pulpitów wirtualnych systemu Windows.
- **Poradnik dotyczący kondycji:** Otrzymuj powiadomienia, które wymagają Twojej uwagi. Poniżej przedstawiono kilka przykładów tego typu powiadomienia:
    - Virtual Machines (maszyny wirtualne) nie są bezpiecznie skonfigurowane jako otwarty port 3389
    - Wycofanie funkcjonalności

### <a name="configure-service-alerts"></a>Konfigurowanie alertów usługi

Aby skonfigurować alerty usługi:

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com/).
2. Wybierz **Service Health.**
3. Postępuj zgodnie z instrukcjami w temacie [tworzenie alertów dziennika aktywności w powiadomieniach dotyczących usług](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log-service-notifications?toc=%2Fazure%2Fservice-health%2Ftoc.json#alert-and-new-action-group-using-azure-portal) , aby skonfigurować alerty i powiadomienia.

## <a name="next-steps"></a>Następne kroki

W tym samouczku przedstawiono sposób konfigurowania i używania Azure Service Health do monitorowania problemów z usługami i klasyfikatorów kondycji dla pulpitu wirtualnego systemu Windows. Aby dowiedzieć się, jak zalogować się do pulpitu wirtualnego systemu Windows, przejdź do programu Windows Virtual Desktop how-OTS.

> [!div class="nextstepaction"]
> [Nawiązywanie połączenia z klientem Pulpit zdalny w systemach Windows 7 i Windows 10](./connect-windows-7-10.md)
