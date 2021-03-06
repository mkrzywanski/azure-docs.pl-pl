---
title: Integracja rozwiązań zabezpieczeń w usłudze Azure Security Center | Microsoft Docs
description: Poznaj sposób integracji usługi Azure Security Center z partnerami w celu poprawy ogólnego stanu zabezpieczeń zasobów platformy Azure.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.assetid: 6af354da-f27a-467a-8b7e-6cbcf70fdbcb
ms.service: security-center
ms.topic: conceptual
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/19/2020
ms.author: memildin
ms.openlocfilehash: dd694fd013069c33e4f3af2c81447e014d41b691
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/20/2020
ms.locfileid: "86519264"
---
# <a name="integrate-security-solutions-in-azure-security-center"></a>Integracja rozwiązań zabezpieczeń w usłudze Azure Security Center
Ten dokument ułatwia zarządzanie rozwiązaniami zabezpieczeń już połączonymi z usługą Azure Security Center i dodawanie nowych.

## <a name="integrated-azure-security-solutions"></a>Zintegrowane rozwiązania zabezpieczeń platformy Azure
Usługa Security Center ułatwia włączanie zintegrowanych rozwiązań zabezpieczeń na platformie Azure. Korzyści to:

- **Uproszczone wdrażanie**: usługa Security Center oferuje udoskonaloną aprowizację zintegrowanych rozwiązań partnerskich. W przypadku rozwiązań takich jak oprogramowanie chroniące przed złośliwym kodem i Ocena luk w zabezpieczeniach Security Center można zainicjować obsługę agenta na maszynach wirtualnych. W przypadku urządzeń zapory Security Center może wymagać większości wymaganych konfiguracji sieci.
- **Zintegrowane wykrywania**: zdarzenia zabezpieczeń z rozwiązań partnerskich są automatycznie zbierane, agregowane i wyświetlane w ramach Security Center alertów i zdarzeń. Te zdarzenia są także połączone z funkcjami wykrywania z innych źródeł, aby zapewnić zaawansowane możliwości w zakresie wykrywania zagrożeń.
- **Ujednolicone zarządzanie monitorowaniem kondycji**: klienci mogą używać zintegrowanych zdarzeń kondycji do błyskawicznego monitorowania wszystkich rozwiązań partnerskich. Podstawowe funkcje zarządzania zapewniają łatwy dostęp do konfiguracji zaawansowanej przy użyciu rozwiązania partnerskiego.

Obecnie zintegrowane rozwiązania zabezpieczeń obejmują ocenę luk w zabezpieczeniach przez [Qualys](https://www.qualys.com/public-cloud/#azure) i [Rapid7](https://www.rapid7.com/products/insightvm/) oraz zaporę aplikacji sieci Web firmy Microsoft Application Gateway.

> [!NOTE]
> Security Center nie instaluje agenta Log Analytics na urządzeniach wirtualnych partnera, ponieważ większość dostawców zabezpieczeń zabroni zewnętrznych agentów działających na ich urządzeniach.

Aby dowiedzieć się więcej o integracji narzędzi do skanowania luk w zabezpieczeniach z programu Qualys, w tym wbudowanego skanera dostępnego dla klientów korzystających z warstwy Standardowa, zobacz: 

- [Zintegrowany skaner luk w zabezpieczeniach dla maszyn wirtualnych](built-in-vulnerability-assessment.md).
- [Wdrażanie rozwiązania do skanowania luk w zabezpieczeniach](partner-vulnerability-assessment.md).

Security Center oferuje również analizę luk w zabezpieczeniach dla:

* Bazy danych SQL — zobacz [Eksplorowanie raportów oceny luk w zabezpieczeniach na pulpicie nawigacyjnym oceny luk w zabezpieczeniach](security-center-iaas-advanced-data.md#explore-vulnerability-assessment-reports)
* Obrazy Azure Container Registry — zobacz [Azure Container Registry Integration with Security Center (wersja zapoznawcza)](azure-container-registry-integration.md)

## <a name="how-security-solutions-are-integrated"></a>Jak są integrowane rozwiązania zabezpieczeń
Rozwiązania zabezpieczeń platformy Azure, które zostały wdrożone z usługi Security Center, są automatycznie połączone. Można także połączyć inne źródła danych zabezpieczeń, w tym komputery działające lokalnie lub w innych chmurach.

[![Integracja rozwiązań partnerskich](./media/security-center-partner-integration/security-solutions-page.png)](./media/security-center-partner-integration/security-solutions-page.png#lightbox)

## <a name="manage-integrated-azure-security-solutions-and-other-data-sources"></a>Zarządzanie zintegrowanymi rozwiązaniami zabezpieczeń platformy Azure i innymi źródłami danych

1. W [Azure Portal](https://azure.microsoft.com/features/azure-portal/)Otwórz **Security Center**.

1. Z menu Security Center wybierz pozycję **rozwiązania zabezpieczeń**.

Na stronie **rozwiązania zabezpieczeń** można sprawdzić kondycję zintegrowanych rozwiązań zabezpieczeń platformy Azure i uruchomić podstawowe zadania zarządzania.

### <a name="connected-solutions"></a>Rozwiązania połączone

Sekcja **połączone rozwiązania** zawiera rozwiązania zabezpieczeń, które są obecnie połączone z Security Center. Pokazuje również stan kondycji każdego rozwiązania.  

![Rozwiązania połączone](./media/security-center-partner-integration/connected-solutions.png)

Stanem rozwiązania partnerskiego może być:

* **Zdrowy** (zielony) — brak problemów z kondycją.
* **Zła kondycja** (czerwony) — występuje problem z kondycją, który wymaga natychmiastowej uwagi.
* **Zatrzymane raportowanie** (pomarańczowy) — rozwiązanie przestało zgłaszać swoją kondycję.
* **Nie zgłoszono** (szare) — rozwiązanie nie zgłosiło jeszcze niczego i żadne dane kondycji nie są dostępne. Stan rozwiązania może być nieraportowany, jeśli był on ostatnio połączony i nadal jest wdrażany.

> [!NOTE]
> Jeśli dane stanu kondycji są niedostępne, Security Center pokazuje datę i godzinę ostatniego odebranego zdarzenia, aby wskazać, czy rozwiązanie zgłasza, czy nie. Jeśli dane dotyczące kondycji nie są dostępne i nie odebrano żadnych alertów w ciągu ostatnich 14 dni, Security Center wskazuje, że rozwiązanie jest w złej kondycji lub nie jest raportowane.
>
>

Wybierz opcję **Widok** , aby uzyskać dodatkowe informacje i opcje, takie jak:

   - **Konsola rozwiązania** — otwiera środowisko zarządzania dla tego rozwiązania.
   - **Link do maszyny wirtualnej** — otwiera stronę łączenie aplikacji. W tym miejscu możesz połączyć zasoby z rozwiązaniem partnerskim.
   - **Usuwanie rozwiązania**
   - **Ustaw opcję**

   ![Szczegóły rozwiązania partnerskiego](./media/security-center-partner-integration/partner-solutions-detail.png)


### <a name="discovered-solutions"></a>Rozwiązania odnalezione

Security Center automatycznie wykrywa rozwiązania zabezpieczeń działające na platformie Azure, ale nie połączyły się z Security Center i wyświetla rozwiązania w sekcji **odnalezione rozwiązania** . Rozwiązania te obejmują rozwiązania platformy Azure, takie jak [Azure AD Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection)i rozwiązania partnerskie.

> [!NOTE]
> Na poziomie subskrypcji dla odnalezionych rozwiązań jest wymagana warstwa standardowa Security Center. Zobacz [Cennik](security-center-pricing.md) , aby dowiedzieć się więcej o warstwach cenowych.
>

Wybierz pozycję **Połącz** w ramach rozwiązania, aby przeprowadzić integrację z usługą Security Center i otrzymywać powiadomienia o alertach zabezpieczeń.

### <a name="add-data-sources"></a>Dodawanie źródeł danych

Sekcja **Dodawanie źródeł danych** obejmuje inne dostępne źródła danych, które mogą zostać połączone. Aby uzyskać instrukcje dotyczące dodawania danych z dowolnego z tych źródeł, kliknij przycisk **DODAJ**.

![Źródła danych](./media/security-center-partner-integration/add-data-sources.png)



## <a name="next-steps"></a>Następne kroki

W tym artykule przedstawiono sposób zintegrowania rozwiązania partnerskiego w usłudze Security Center. Aby uzyskać powiązane informacje, zobacz następujące artykuły:

* [Eksportowanie alertów zabezpieczeń i zaleceń](continuous-export.md). Dowiedz się, jak skonfigurować integrację z platformą Azure, lub dowolnym innym SIEM.
* [Monitorowanie kondycji zabezpieczeń w usłudze Security Center](security-center-monitoring.md). Informacje na temat sposobu monitorowania kondycji zasobów platformy Azure.