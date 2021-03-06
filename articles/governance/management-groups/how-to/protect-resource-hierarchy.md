---
title: Jak chronić hierarchię zasobów — zarządzanie platformą Azure
description: Dowiedz się, jak chronić hierarchię zasobów przy użyciu ustawień hierarchii, które obejmują ustawienie domyślnej grupy zarządzania.
ms.date: 05/21/2020
ms.topic: conceptual
ms.openlocfilehash: cdaad59d136e89c595a6a42a9760c73523a977c0
ms.sourcegitcommit: e71da24cc108efc2c194007f976f74dd596ab013
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/29/2020
ms.locfileid: "87422831"
---
# <a name="how-to-protect-your-resource-hierarchy"></a>Jak chronić hierarchię zasobów

Zasoby, grupy zasobów, subskrypcje, grupy zarządzania i dzierżawcę zbiorowo tworzą hierarchię zasobów. Ustawienia w głównej grupie zarządzania, takie jak role niestandardowe platformy Azure lub przypisania zasad Azure Policy, mogą mieć wpływ na każdy zasób w hierarchii zasobów. Ważne jest, aby chronić hierarchię zasobów przed zmianami, które mogą mieć negatywny wpływ na wszystkie zasoby.

Grupy zarządzania mają teraz ustawienia hierarchii, które umożliwiają administratorowi dzierżawy sterowanie tymi zachowaniami. W tym artykule opisano wszystkie dostępne ustawienia hierarchii oraz sposób ich konfigurowania.

## <a name="rbac-permissions-for-hierarchy-settings"></a>Uprawnienia RBAC dla ustawień hierarchii

Skonfigurowanie dowolnych ustawień hierarchii wymaga dwóch operacji RBAC w głównej grupie zarządzania:

- `Microsoft.Management/managementgroups/settings/write`
- `Microsoft.Management/managementgroups/settings/read`

Te operacje umożliwiają użytkownikowi odczytywanie i aktualizowanie ustawień hierarchii. Operacje nie zapewniają żadnych innych dostępów do hierarchii lub zasobów grupy zarządzania. Obie te operacje są dostępne w **Administratorze ustawień hierarchii**ról wbudowanej platformy Azure.

## <a name="setting---default-management-group"></a>Ustawienie — domyślna grupa zarządzania

Domyślnie Nowa subskrypcja dodana w ramach dzierżawy jest dodawana jako członek głównej grupy zarządzania. Jeśli przydziały zasad, kontroli dostępu opartej na rolach (RBAC) i inne konstrukcje ładu są przypisane do głównej grupy zarządzania, natychmiast wpływają na te nowe subskrypcje. Z tego powodu wiele organizacji nie stosuje tych konstrukcji w głównej grupie zarządzania, nawet jeśli jest to wymagane miejsce do przypisywania. W innych przypadkach bardziej restrykcyjny zestaw kontrolek jest wymagany w przypadku nowych subskrypcji, ale nie powinien być przypisany do wszystkich subskrypcji. To ustawienie obsługuje oba przypadki użycia.

Zezwalając na zdefiniowanie domyślnej grupy zarządzania dla nowych subskrypcji, można zastosować konstrukcje ładu dla całej organizacji w głównej grupie zarządzania, a także oddzielną grupę zarządzania z przypisaniami zasad lub przypisaniami ról platformy Azure bardziej przystosowanymi do nowej subskrypcji.

Aby skonfigurować to ustawienie, zostanie wywołany punkt końcowy interfejsu API REST [ustawień hierarchii](/rest/api/resources/hierarchysettings) . W tym celu należy użyć poniższego identyfikatora URI interfejsu API REST i formatu treści. Zastąp ciąg `{rootMgID}` identyfikatorem głównej grupy zarządzania i `{defaultGroupID}` identyfikatorem grupy zarządzania, aby stał się domyślną grupą zarządzania:

- Identyfikator URI interfejsu API REST

  ```http
  PUT https://management.azure.com/providers/Microsoft.Management/managementGroups/{rootMgID}/settings/default?api-version=2020-02-01
  ```

- Treść żądania

  ```json
  {
      "properties": {
          "defaultManagementGroup": "/providers/Microsoft.Management/managementGroups/{defaultGroupID}"
      }
  }
  ```

Aby ustawić domyślną grupę zarządzania z powrotem do głównej grupy zarządzania, należy użyć tego samego punktu końcowego i ustawić **defaultManagementGroup** na wartość `/providers/Microsoft.Management/managementGroups/{rootMgID}` .

## <a name="setting---require-authorization"></a>Ustawienie — Wymagaj autoryzacji

Wszyscy użytkownicy domyślnie mogą tworzyć nowe grupy zarządzania w ramach dzierżawy. Administratorzy dzierżawy mogą chcieć udzielić tych uprawnień tylko określonym użytkownikom, aby zachować spójność i zgodność w hierarchii grupy zarządzania. Jeśli ta funkcja jest włączona, użytkownik musi wykonać `Microsoft.Management/managementGroups/write` operację w głównej grupie zarządzania, aby utworzyć nowe podrzędne grupy zarządzania.

Aby skonfigurować to ustawienie, zostanie wywołany punkt końcowy interfejsu API REST [ustawień hierarchii](/rest/api/resources/hierarchysettings) . W tym celu należy użyć poniższego identyfikatora URI interfejsu API REST i formatu treści. Ta wartość jest wartością _logiczną_, dlatego dla wartości należy podać wartość **true** lub **false** . Wartość **true** włącza tę metodę ochrony hierarchii grup zarządzania:

- Identyfikator URI interfejsu API REST

  ```http
  PUT https://management.azure.com/providers/Microsoft.Management/managementGroups/{rootMgID}/settings/default?api-version=2020-02-01
  ```

- Treść żądania

  ```json
  {
      "properties": {
          "requireAuthorizationForGroupCreation": true
      }
  }
  ```

Aby wyłączyć ustawienie, użyj tego samego punktu końcowego i ustaw **requireAuthorizationForGroupCreation** na wartość **false**.

## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się więcej na temat grup zarządzania, zobacz:

- [Tworzenie grup zarządzania w celu organizowania zasobów platformy Azure](../create.md)
- [Jak zmienić lub usunąć grupy zarządzania oraz zarządzać nimi](../manage.md)
