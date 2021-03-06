---
title: Praca z zasadami zabezpieczeń | Microsoft Docs
description: W tym artykule opisano sposób pracy z zasadami zabezpieczeń w programie Azure Security Center.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 2d248817-ae97-4c10-8f5d-5c207a8019ea
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/04/2019
ms.author: memildin
ms.openlocfilehash: 52488eb43377978d7f936ba0aa452cc872f8d899
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/20/2020
ms.locfileid: "86519358"
---
# <a name="working-with-security-policies"></a>Praca z zasadami zabezpieczeń

W tym artykule opisano sposób konfigurowania zasad zabezpieczeń oraz sposób ich wyświetlania w Security Center. 

## <a name="introduction-to-security-policies"></a>Wprowadzenie do zasad zabezpieczeń

Zasada zabezpieczeń definiuje żądaną konfigurację obciążeń i pomaga zapewnić zgodność z wymaganiami firmy lub instytucji nadzoru.

Azure Security Center wykonuje zalecenia dotyczące zabezpieczeń w oparciu o wybrane zasady. Zasady Security Center są oparte na inicjatywach zasad utworzonych w programie Azure Policy. Za pomocą [Azure Policy](../governance/policy/overview.md) można zarządzać zasadami oraz konfigurować zasady w grupach zarządzania i wielu subskrypcjach.

Security Center oferuje następujące opcje pracy z zasadami zabezpieczeń:

* **Wyświetlanie i edytowanie wbudowanych zasad domyślnych** — po włączeniu Security Center, wbudowana inicjatywa o nazwie "wartość domyślna" w wersji ASC zostanie automatycznie przypisana do wszystkich Security Center zarejestrowanych subskrypcji (warstwy cenowe bezpłatne lub standardowe). Aby dostosować tę inicjatywę, możesz włączyć lub wyłączyć poszczególne zasady w ramach tego programu. Zapoznaj się z listą [wbudowanych zasad zabezpieczeń](security-center-policy-definitions.md) , aby poznać dostępne opcje.

* **Dodawanie własnych zasad niestandardowych** — Jeśli chcesz dostosować inicjatywy zabezpieczeń stosowane do subskrypcji, możesz to zrobić w ramach Security Center. Następnie otrzymasz zalecenia, jeśli maszyny nie będą zgodne z tworzonymi zasadami. Aby uzyskać instrukcje dotyczące tworzenia i przypisywania zasad niestandardowych, zobacz [Używanie niestandardowych zasad zabezpieczeń](custom-security-policies.md).

* **Dodaj zasady zgodności z przepisami** — pulpit nawigacyjny zgodności z przepisami Security Center przedstawia stan wszystkich ocen w danym środowisku w kontekście określonego standardu lub rozporządzenia (na przykład Azure CIS, NIST SP 800-53 R4, Swift CSP CSCF-V2020). Aby uzyskać więcej informacji, zobacz [poprawianie zgodności z przepisami](security-center-compliance-dashboard.md).


## <a name="managing-your-security-policies"></a>Zarządzanie zasadami zabezpieczeń

Aby wyświetlić zasady zabezpieczeń w usłudze Security Center:

1. Na pulpicie nawigacyjnym **Security Center** wybierz pozycję **zasady zabezpieczeń**.

    ![Okienko Zarządzanie zasadami](./media/security-center-policies/security-center-policy-mgt.png)

   Na ekranie **Zarządzanie zasadami** można zobaczyć liczbę grup zarządzania, subskrypcji i obszarów roboczych, a także strukturę grupy zarządzania.

1. Wybierz subskrypcję lub grupę zarządzania, której zasady chcesz wyświetlić.

1. Zostanie wyświetlona strona zasady zabezpieczeń dla tej subskrypcji lub grupy zarządzania. Są w nim wyświetlane zasady dostępne i przypisane.

   ![ekran zasad](./media/tutorial-security-policy/security-policy-page.png)

    > [!NOTE]
    > Jeśli istnieje etykieta "MG odziedziczona" wraz z zasadami domyślnymi, oznacza to, że zasady zostały przypisane do grupy zarządzania i są dziedziczone przez przeglądaną subskrypcję.


1. Wybierz spośród dostępnych opcji na tej stronie:

    1. Aby korzystać z zasad branżowych, wybierz pozycję **Dodaj więcej standardów**. Aby uzyskać więcej informacji, zobacz [Aktualizacja dynamicznych pakietów zgodności](update-regulatory-compliance-packages.md).

    1. Aby przypisywać niestandardowe inicjatywy i zarządzać nimi, wybierz pozycję **Dodaj niestandardowe inicjatywy**. Aby uzyskać więcej informacji, zobacz [Korzystanie z niestandardowych zasad zabezpieczeń](custom-security-policies.md).

    1. Aby wyświetlić i edytować zasady domyślne, wybierz pozycję **Wyświetl czynne zasady** i postępuj zgodnie z poniższym opisem. 

       ![ekran zasad](./media/security-center-policies/policy-screen.png)
       
       Ten ekran **zasad zabezpieczeń** odzwierciedla akcję podejmowaną przez zasady przypisane do wybranej subskrypcji lub grupy zarządzania.
       
       * Użyj linków w górnej części strony, aby otworzyć **przypisanie** zasad, które ma zastosowanie do subskrypcji lub grupy zarządzania. Te linki umożliwiają dostęp do przypisania i edytowanie lub wyłączanie zasad. Jeśli na przykład widzisz, że określone przypisanie zasad skutecznie odmawia ochrony punktu końcowego, Użyj linku do edytowania lub wyłączania zasad.
       
       * Na liście zasad można zobaczyć obowiązującą aplikację zasad w ramach subskrypcji lub grupy zarządzania. Są brane pod uwagę ustawienia poszczególnych zasad, które mają zastosowanie do zakresu, oraz łączny wynik akcji podejmowanych przez zasady. Na przykład, jeśli w jednym przypisaniu zasad jest wyłączone, ale w innym jest ustawiona na AuditIfNotExist, to efekt skumulowany stosuje AuditIfNotExist. Większym aktywnym efektem jest zawsze pierwszeństwo.
       
       * Efektem zasad może być: dołączanie, inspekcja, AuditIfNotExists, Odmów, DeployIfNotExists, wyłączone. Aby uzyskać więcej informacji na temat sposobu stosowania efektów, zobacz [opis efektów zasad](../governance/policy/concepts/effects.md).

       > [!NOTE]
       > Podczas wyświetlania przypisanych zasad można zobaczyć wiele przypisań i zobaczyć, jak poszczególne przypisania są skonfigurowane samodzielnie.


## <a name="who-can-edit-security-policies"></a>Kto może edytować zasady zabezpieczeń?

Zasady zabezpieczeń można edytować za pośrednictwem portalu Azure Policy przy użyciu interfejsu API REST lub środowiska Windows PowerShell.

Security Center korzysta z Access Control opartych na rolach (RBAC), które udostępnia wbudowane role, które można przypisać do użytkowników, grup i usług platformy Azure. Gdy użytkownicy otworzą Security Center, zobaczą tylko informacje związane z zasobami, do których mogą uzyskać dostęp. Oznacza to, że użytkownicy mają przypisaną rolę *właściciela*, *współautora*lub *czytelnika* do subskrypcji zasobu. Istnieją również dwie konkretne role Security Center:

- **Czytelnik zabezpieczeń**: ma uprawnienia do wyświetlania Security Center elementów, takich jak zalecenia, alerty, zasady i kondycja. Nie można wprowadzić zmian.
- **Administrator zabezpieczeń**: ma takie same uprawnienia do wyświetlania jak *czytelnik zabezpieczeń*. Może także aktualizować zasady zabezpieczeń i odrzucać alerty.


## <a name="disable-security-policies-and-disable-recommendations"></a>Wyłączanie zasad zabezpieczeń i wyłączanie zaleceń

Gdy inicjatywa zabezpieczeń wyzwala zalecenie, które nie jest istotne dla danego środowiska, można zapobiec ponownemu wyświetlaniu tego zalecenia. Aby wyłączyć zalecenie, wyłącz określone zasady, które generują zalecenie.

Zalecenie, które ma zostać wyłączone, nadal będzie wyświetlane, jeśli jest wymagane w przypadku normy stosowanej w przypadku zastosowania do narzędzi zgodności z przepisami Security Center. Nawet jeśli zasady zostały wyłączone w ramach inicjatywy wbudowanej, zasady obowiązujące w ramach inicjatywy w ramach normy wykonawczej nadal będą wyzwalać zalecenie, jeśli jest to niezbędne do zapewnienia zgodności. Nie można wyłączyć zasad z inicjatywy standardowych inicjatyw.

Aby uzyskać więcej informacji na temat zaleceń, zobacz [Zarządzanie zaleceniami](security-center-recommendations.md)dotyczącymi zabezpieczeń.

1. W Security Center z sekcji **zgodność zasad &** wybierz pozycję **zasady zabezpieczeń**.

   ![Zarządzanie zasadami](./media/tutorial-security-policy/policy-management.png)

2. Wybierz subskrypcję lub grupę zarządzania, dla której chcesz wyłączyć zalecenie.

   > [!NOTE]
   > Pamiętaj, że grupa zarządzania stosuje zasady do swoich subskrypcji. W związku z tym wyłączenie zasady subskrypcji, która należy do grupy zarządzania korzystającej nadal z tych samych zasad, sprawi, że będziesz nadal otrzymywać rekomendacje dotyczące zasad. Zasady będą nadal stosowane z poziomu zarządzania i rekomendacje będą nadal generowane.

1. Wybierz pozycję **Wyświetl czynne zasady**.

   ![Wyłącz zasady](./media/tutorial-security-policy/view-effective-policy.png)

1. Wybierz przypisane zasady.

   ![Wyłącz zasady](./media/tutorial-security-policy/security-policy.png)

1. W sekcji **Parametry** Wyszukaj zasady wywołujące zalecenie, które chcesz wyłączyć, a następnie z listy rozwijanej wybierz pozycję **wyłączone** .

   ![Wyłącz zasady](./media/tutorial-security-policy/disable-policy.png)

1. Wybierz pozycję **Zapisz**.

   > [!NOTE]
   > Wyłączenie zmian zasad może potrwać do 12 godzin.



## <a name="next-steps"></a>Następne kroki
W tym artykule wyjaśniono zasady zabezpieczeń. Aby uzyskać powiązane informacje, zobacz następujące artykuły:

* Aby uzyskać instrukcje dotyczące sposobu ustawiania zasad przy użyciu programu PowerShell, zobacz [Szybki Start: Tworzenie przypisania zasad w celu zidentyfikowania niezgodnych zasobów przy użyciu modułu Azure PowerShell](../governance/policy/assign-policy-powershell.md)

* Aby uzyskać instrukcje dotyczące edytowania zasad zabezpieczeń w programie Azure Policy, zobacz [Tworzenie zasad i zarządzanie nimi w celu wymuszenia zgodności](../governance/policy/tutorials/create-and-manage.md).

* Aby uzyskać instrukcje dotyczące sposobu ustawiania zasad w ramach subskrypcji lub grup zarządzania przy użyciu Azure Policy, zobacz [co to jest Azure Policy?](../governance/policy/overview.md)