---
title: Podstawa zabezpieczeń platformy Azure dla Network Watcher
description: Podstawa zabezpieczeń platformy Azure dla Network Watcher
author: msmbaldwin
ms.service: security
ms.topic: conceptual
ms.date: 06/22/2020
ms.author: mbaldwin
ms.custom: security-benchmark
ms.openlocfilehash: 2a4cba847610ab7d6119c04f9e3b1c32a192d6c9
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "85256000"
---
# <a name="azure-security-baseline-for-network-watcher"></a>Podstawa zabezpieczeń platformy Azure dla Network Watcher

Podstawą zabezpieczeń platformy Azure dla Network Watcher są zalecenia, które pomogą ulepszyć stan bezpieczeństwa wdrożenia.

Punkt odniesienia dla tej usługi jest rysowany w [wersji 1,0 usługi Azure Security test](https://docs.microsoft.com/azure/security/benchmarks/overview), która zawiera zalecenia dotyczące sposobu zabezpieczania rozwiązań w chmurze na platformie Azure z naszymi najlepszymi wskazówkami.

Aby uzyskać więcej informacji, zobacz [podstawy zabezpieczeń platformy Azure — omówienie](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview).

## <a name="network-security"></a>Bezpieczeństwo sieci

*Aby uzyskać więcej informacji, zobacz [Kontrola zabezpieczeń: zabezpieczenia sieci](https://docs.microsoft.com/azure/security/benchmarks/security-control-network-security).*

### <a name="11-protect-azure-resources-within-virtual-networks"></a>1,1: Ochrona zasobów platformy Azure w ramach sieci wirtualnych

**Wskazówki**: nie dotyczy; Network Watcher ma możliwość monitorowania połączenia między punktem końcowym i maszyną wirtualną, ale nie może być zabezpieczony przez sieć wirtualną, sieciową grupę zabezpieczeń lub zaporę platformy Azure.

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: nie dotyczy

### <a name="12-monitor-and-log-the-configuration-and-traffic-of-virtual-networks-subnets-and-nics"></a>1,2: Monitoruj i Rejestruj konfigurację oraz ruch sieci wirtualnych, podsieci i kart sieciowych

**Wskazówki**: nie dotyczy; Usługa Azure Network Watcher umożliwia analizowanie dzienników przepływów i dostarczanie rozbudowanych wizualizacji danych, jednak Network Watcher sama nie produkuje dzienników przepływów ani pakietów capturable.

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: nie dotyczy

### <a name="13-protect-critical-web-applications"></a>1,3: Ochrona krytycznych aplikacji sieci Web

**Wskazówki**: nie dotyczy; to zalecenie jest przeznaczone dla aplikacji sieci Web działających na Azure App Service lub zasobach obliczeniowych.

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: nie dotyczy

### <a name="14-deny-communications-with-known-malicious-ip-addresses"></a>1,4: odmowa komunikacji ze znanymi złośliwymi adresami IP

**Wskazówki**: nie dotyczy; Usługa Azure Network Watcher nie jest zasobem z potencjalnym narażeniem na sieci publiczne, które muszą być chronione przed złośliwym ruchem.

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: nie dotyczy

### <a name="15-record-network-packets"></a>1,5: rejestrowanie pakietów sieciowych

**Wskazówki**: nie dotyczy; Usługa Azure Network Watcher umożliwia analizowanie dzienników przepływów i dostarczanie rozbudowanych wizualizacji danych, jednak Network Watcher sama nie produkuje dzienników przepływów ani pakietów capturable.

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: nie dotyczy

### <a name="16-deploy-network-based-intrusion-detectionintrusion-prevention-systems-idsips"></a>1,6: wdrażanie opartych na sieci systemów zapobiegania wykrywaniem i dostępem intruzów (identyfikatorów/adresów IP)

**Wskazówki**: nie dotyczy; Usługa Azure Network Watcher nie jest zasobem z potencjalnym narażeniem na sieci publiczne, które muszą być chronione przed złośliwym ruchem.

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: nie dotyczy

### <a name="17-manage-traffic-to-web-applications"></a>1,7: zarządzanie ruchem do aplikacji sieci Web

**Wskazówki**: nie dotyczy; to zalecenie jest przeznaczone dla aplikacji sieci Web działających na Azure App Service lub zasobach obliczeniowych.

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: nie dotyczy

### <a name="18-minimize-complexity-and-administrative-overhead-of-network-security-rules"></a>1,8: Minimalizacja złożoności i kosztów administracyjnych reguł zabezpieczeń sieci

**Wskazówki**: nie dotyczy; Usługa Azure Network Watcher umożliwia analizowanie dzienników przepływów i dostarczanie rozbudowanych wizualizacji danych, jednak Network Watcher sama nie jest punktem końcowym, w którym można filtrować lub zezwalać na ruch z użyciem tagów usługi lub sieciowych grup zabezpieczeń.

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: nie dotyczy

### <a name="19-maintain-standard-security-configurations-for-network-devices"></a>1,9: Obsługa standardowych konfiguracji zabezpieczeń dla urządzeń sieciowych

**Wskazówki**: Definiowanie i implementowanie standardowych konfiguracji zabezpieczeń dla usługi Azure Network Watcher przy użyciu Azure Policy. Użyj aliasów Azure Policy w przestrzeni nazw "Microsoft. Network", aby utworzyć zasady niestandardowe do inspekcji lub wymuszania konfiguracji sieci wystąpień Network Watcher. Możesz również używać wbudowanych definicji zasad, takich jak:

Wdróż obserwatora sieciowego po utworzeniu sieci wirtualnych

Network Watcher powinna być włączona

* [Jak skonfigurować Azure Policy i zarządzać nimi](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Jak utworzyć zasadę niestandardową z aliasami zasad](https://docs.microsoft.com/azure/governance/policy/tutorials/create-custom-policy-definition)

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: nie dotyczy

### <a name="110-document-traffic-configuration-rules"></a>1,10: udokumentowanie reguł konfiguracji ruchu

**Wskazówki**: nie dotyczy; Chociaż usługa Azure Network Watcher obsługuje tagi, Network Watcher sama sama nie kontroluje przepływu ruchu sieciowego.

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: nie dotyczy

### <a name="111-use-automated-tools-to-monitor-network-resource-configurations-and-detect-changes"></a>1,11: Użyj zautomatyzowanych narzędzi do monitorowania konfiguracji zasobów sieciowych i wykrywania zmian

**Wskazówki**: Użyj dziennika aktywności platformy Azure do monitorowania zmian wprowadzonych w usłudze Azure Network Watcher. Możesz tworzyć alerty w ramach Azure Monitor, które będą wyzwalać, gdy zmiany zostaną wykonane.

* [Jak wyświetlać i pobierać zdarzenia dziennika aktywności platformy Azure](https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-view)

* [Jak utworzyć alerty w Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log)

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: nie dotyczy

## <a name="logging-and-monitoring"></a>Rejestrowanie i monitorowanie

*Aby uzyskać więcej informacji, zobacz [Kontrola zabezpieczeń: rejestrowanie i monitorowanie](https://docs.microsoft.com/azure/security/benchmarks/security-control-logging-monitoring).*

### <a name="21-use-approved-time-synchronization-sources"></a>2,1: Użyj źródeł synchronizacji zatwierdzonego czasu

**Wskazówki**: nie dotyczy; Firma Microsoft utrzymuje źródło czasu używane dla zasobów platformy Azure, takie jak Azure Network Watcher, dla sygnatur czasowych w dziennikach.

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: nie dotyczy

### <a name="22-configure-central-security-log-management"></a>2,2: Skonfiguruj centralne zarządzanie dziennikami zabezpieczeń

**Wskazówki**: Użyj dziennika aktywności platformy Azure do monitorowania konfiguracji i wykrywania zmian dla wystąpień Network Watcher platformy Azure. Poza płaszczyzną kontroli (np. Azure Portal) Network Watcher sama nie generuje dzienników związanych z ruchem sieciowym. Network Watcher udostępnia narzędzia do monitorowania, diagnozowania, wyświetlania metryk i włączania lub wyłączania dzienników dla zasobów w sieci wirtualnej platformy Azure.

* [Jak wyświetlać i pobierać zdarzenia dziennika aktywności platformy Azure](https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-view)

* [Opis Network Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview)

**Monitorowanie Azure Security Center**: tak

**Odpowiedzialność**: klient

### <a name="23-enable-audit-logging-for-azure-resources"></a>2,3: Włączanie rejestrowania inspekcji dla zasobów platformy Azure

**Wskazówki**: Użyj dziennika aktywności platformy Azure do monitorowania konfiguracji i wykrywania zmian dla wystąpień Network Watcher platformy Azure. Poza płaszczyzną kontroli (np. Azure Portal) Network Watcher sama nie generuje dzienników inspekcji. Network Watcher udostępnia narzędzia do monitorowania, diagnozowania, wyświetlania metryk i włączania lub wyłączania dzienników dla zasobów w sieci wirtualnej platformy Azure.

* [Jak wyświetlać i pobierać zdarzenia dziennika aktywności platformy Azure](https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-view)

* [Opis Network Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview)

**Monitorowanie Azure Security Center**: tak

**Odpowiedzialność**: klient

### <a name="24-collect-security-logs-from-operating-systems"></a>2,4: Zbierz dzienniki zabezpieczeń z systemów operacyjnych

**Wskazówki**: nie dotyczy; to zalecenie jest przeznaczone do zasobów obliczeniowych.

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: nie dotyczy

### <a name="25-configure-security-log-storage-retention"></a>2,5: Konfigurowanie przechowywania magazynu dzienników zabezpieczeń

**Wskazówki**: w Azure monitor ustawić okres przechowywania dziennika dla log Analytics obszarów roboczych skojarzonych z platformą Azure Network Watcher zgodnie z regulacjami zgodności w organizacji.

* [Jak ustawić parametry przechowywania dziennika](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period)

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: klient

### <a name="26-monitor-and-review-logs"></a>2,6: dzienniki monitorowania i przeglądania

**Wskazówki**: Użyj dziennika aktywności platformy Azure do monitorowania konfiguracji i wykrywania zmian dla wystąpień Network Watcher platformy Azure. Poza płaszczyzną kontroli (np. Azure Portal) Network Watcher sama nie generuje dzienników związanych z ruchem sieciowym. Network Watcher udostępnia narzędzia do monitorowania, diagnozowania, wyświetlania metryk i włączania lub wyłączania dzienników dla zasobów w sieci wirtualnej platformy Azure.

* [Jak wyświetlać i pobierać zdarzenia dziennika aktywności platformy Azure](https://docs.microsoft.com/azure/azure-monitor/platform/activity-log-view)

* [Opis Network Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview)

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: klient

### <a name="27-enable-alerts-for-anomalous-activities"></a>2,7: Włączanie alertów dla nietypowych działań

**Wskazówki**: można skonfigurować w taki sposób, aby otrzymywać alerty na podstawie dzienników aktywności związanych z usługą Azure Network Watcher. Azure Monitor umożliwia skonfigurowanie alertu w celu wysłania powiadomienia e-mail, wywołania elementu webhook lub wywoływać aplikację logiki platformy Azure.

* [Jak zarządzać alertami w Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts)

**Monitorowanie Azure Security Center**: tak

**Odpowiedzialność**: klient

### <a name="28-centralize-anti-malware-logging"></a>2,8: scentralizowanie rejestrowania chroniącego przed złośliwym oprogramowaniem

**Wskazówki**: nie dotyczy; Usługa Azure Network Watcher nie przetwarza ani nie tworzy dzienników związanych z oprogramowaniem chroniącym przed złośliwym kodem.

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: nie dotyczy

### <a name="29-enable-dns-query-logging"></a>2,9: Włączanie rejestrowania zapytań DNS

**Wskazówki**: nie dotyczy; Usługa Azure Network Watcher nie przetwarza ani nie tworzy dzienników związanych z usługą DNS.

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: nie dotyczy

### <a name="210-enable-command-line-audit-logging"></a>2,10: Włączanie rejestrowania inspekcji w wierszu polecenia

**Wskazówki**: nie dotyczy; to zalecenie jest przeznaczone do zasobów obliczeniowych.

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: nie dotyczy

## <a name="identity-and-access-control"></a>Tożsamość i kontrola dostępu

*Aby uzyskać więcej informacji, zobacz [Kontrola zabezpieczeń: tożsamość i kontrola dostępu](https://docs.microsoft.com/azure/security/benchmarks/security-control-identity-access-control).*

### <a name="31-maintain-an-inventory-of-administrative-accounts"></a>3,1: obsługa spisu kont administracyjnych

**Wskazówki**: przechowywanie spisu kont użytkowników, które mają dostęp administracyjny do płaszczyzny kontroli (np. Azure Portal) Network Watcher platformy Azure. Aby można było korzystać z funkcji Network Watcher, konto, które logujesz się do platformy Azure, musi być przypisane do wbudowanych ról współautor, współautor lub sieci lub przypisane do roli niestandardowej przypisanej do określonych funkcji Network Watcher.

Aby skonfigurować kontrolę dostępu opartą na rolach (RBAC), można użyć okienka tożsamości i kontroli dostępu (IAM) w Azure Portal dla subskrypcji. Role są stosowane do użytkowników, grup, podmiotów usługi i tożsamości zarządzanych w Active Directory.

* [Zrozumienie RBAC na platformie Azure](https://docs.microsoft.com/azure/role-based-access-control/overview)

* [Uprawnienia kontroli dostępu opartej na rolach wymagane do korzystania z funkcji Network Watcher](https://docs.microsoft.com/azure/network-watcher/required-rbac-permissions)

**Monitorowanie Azure Security Center**: tak

**Odpowiedzialność**: klient

### <a name="32-change-default-passwords-where-applicable"></a>3,2: Zmień domyślne hasła, jeśli ma to zastosowanie

**Wskazówki**: usługa Azure AD nie ma koncepcji domyślnych haseł. Inne zasoby platformy Azure wymagające hasła wymuszają utworzenie hasła przy użyciu wymagań dotyczących złożoności i minimalnej długości hasła, które różnią się w zależności od usługi. Użytkownik jest odpowiedzialny za aplikacje innych firm i usługi Marketplace, które mogą korzystać z domyślnych haseł.

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: klient

### <a name="33-use-dedicated-administrative-accounts"></a>3,3: Użyj dedykowanych kont administracyjnych

**Wskazówki**: Tworzenie standardowych procedur operacyjnych dotyczących korzystania z dedykowanych kont administracyjnych. Użyj Azure Security Center Zarządzanie tożsamościami i dostępem, aby monitorować liczbę kont administracyjnych.

Ponadto, aby ułatwić śledzenie dedykowanych kont administracyjnych, można użyć zaleceń z Azure Security Center lub wbudowanych zasad platformy Azure, takich jak:
- Do subskrypcji powinien być przypisany więcej niż jeden właściciel
- Przestarzałe konta z uprawnieniami właściciela powinny zostać usunięte z subskrypcji
- Konta zewnętrzne z uprawnieniami właściciela powinny zostać usunięte z subskrypcji

* [Jak używać Azure Security Center do monitorowania tożsamości i dostępu (wersja zapoznawcza)](https://docs.microsoft.com/azure/security-center/security-center-identity-access)

* [Jak używać Azure Policy](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

**Monitorowanie Azure Security Center**: tak

**Odpowiedzialność**: klient

### <a name="34-use-single-sign-on-sso-with-azure-active-directory"></a>3,4: Korzystaj z logowania jednokrotnego (SSO) z usługą Azure Active Directory

**Wskazówki**: nie dotyczy; Logowanie jednokrotne (SSO) zwiększa bezpieczeństwo i wygodę, gdy użytkownicy logują się do aplikacji niestandardowych w usłudze Azure Active Directory (AD). Dostęp do usługi Azure Network Watcher jest już zintegrowany z Azure Active Directory i jest dostępny za pomocą Azure Portal oraz Azure Resource Manager interfejsu API REST.

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: nie dotyczy

### <a name="35-use-multi-factor-authentication-for-all-azure-active-directory-based-access"></a>3,5: Użyj uwierzytelniania wieloskładnikowego, aby uzyskać dostęp oparty na Azure Active Directory

**Wskazówki**: włączanie Azure Active Directory Multi-Factor Authentication i postępuj zgodnie z zaleceniami związanymi z zarządzaniem tożsamościami i dostępem Azure Security Center.

* [Jak włączyć usługę MFA na platformie Azure](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted)

* [Jak monitorować tożsamość i dostęp w Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-identity-access)

**Monitorowanie Azure Security Center**: tak

**Odpowiedzialność**: klient

### <a name="36-use-dedicated-machines-privileged-access-workstations-for-all-administrative-tasks"></a>3,6: Używaj dedykowanych maszyn (uprzywilejowany dostęp do stacji roboczych) dla wszystkich zadań administracyjnych

**Wskazówki**: Użyj stacji roboczej dostępu uprzywilejowanego (dostępem uprzywilejowanym) z włączoną usługą Azure Multi-Factor Authentication (MFA), aby zalogować się i skonfigurować zasoby związane z platformą Azure.

* [Stacje robocze z dostępem uprzywilejowanym](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations)

* [Planowanie wdrożenia usługi Azure Multi-Factor Authentication opartej na chmurze](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted)

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: klient

### <a name="37-log-and-alert-on-suspicious-activities-from-administrative-accounts"></a>3,7: Rejestruj i Ostrzegaj o podejrzanych działaniach z kont administracyjnych

**Wskazówki**: Użyj Azure Active Directory (AD) PRIVILEGED Identity Management (PIM) do generowania dzienników i alertów w przypadku wystąpienia podejrzanych lub niebezpiecznych działań w środowisku.

Ponadto za pomocą funkcji wykrywania ryzyka usługi Azure AD można wyświetlać alerty i raporty na temat ryzykownego zachowania użytkowników.

* [Jak wdrożyć Privileged Identity Management (PIM)](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-deployment-plan)

* [Omówienie wykrywania ryzyka usługi Azure AD](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-risk-events)

**Monitorowanie Azure Security Center**: tak

**Odpowiedzialność**: klient

### <a name="38-manage-azure-resources-from-only-approved-locations"></a>3,8: zarządzanie zasobami platformy Azure tylko z zatwierdzonych lokalizacji

**Wskazówki**: Użyj dostępu warunkowego o nazwie Locations, aby zezwolić na dostęp do Azure Portal tylko z określonych logicznych grup zakresów lub krajów/regionów adresów IP.

* [Jak skonfigurować nazwane lokalizacje na platformie Azure](https://docs.microsoft.com/azure/active-directory/reports-monitoring/quickstart-configure-named-locations)

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: klient

### <a name="39-use-azure-active-directory"></a>3,9: Użyj Azure Active Directory

**Wskazówki**: Użyj Azure Active Directory (Azure AD) jako centralnego systemu uwierzytelniania i autoryzacji dla wystąpień wskaźnikowych platformy Azure. Usługa Azure AD chroni dane przy użyciu silnego szyfrowania danych przechowywanych i przesyłanych. Usługa Azure AD również Sole, skróty i bezpieczne przechowywanie poświadczeń użytkownika.

* [Jak utworzyć i skonfigurować wystąpienie usługi Azure AD](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-access-create-new-tenant)

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: klient

### <a name="310-regularly-review-and-reconcile-user-access"></a>3,10: regularnie Przeglądaj i Uzgodnij dostęp użytkowników

**Wskazówki**: Azure Active Directory (AD) zawiera dzienniki ułatwiające odnajdywanie starych kont. Ponadto za pomocą przeglądów dostępu do tożsamości platformy Azure można efektywnie zarządzać członkostwem w grupach, dostępem do aplikacji dla przedsiębiorstw i przypisaniami ról. Dostęp użytkowników może być regularnie przeglądany, aby upewnić się, że tylko Ci użytkownicy mają ciągły dostęp.

* [Informacje o raportowaniu usługi Azure AD](https://docs.microsoft.com/azure/active-directory/reports-monitoring/)

* [Jak korzystać z przeglądów dostępu do tożsamości platformy Azure](https://docs.microsoft.com/azure/active-directory/governance/access-reviews-overview)

**Monitorowanie Azure Security Center**: tak

**Odpowiedzialność**: klient

### <a name="311-monitor-attempts-to-access-deactivated-credentials"></a>3,11: Monitor próbuje uzyskać dostęp do zdezaktywowanych poświadczeń

**Wskazówki**: Użyj Azure Active Directory (AD) jako centralnego systemu uwierzytelniania i autoryzacji dla Network Watcher platformy Azure. Usługa Azure AD chroni dane przy użyciu silnego szyfrowania danych przechowywanych i przesyłanych. Usługa Azure AD również Sole, skróty i bezpieczne przechowywanie poświadczeń użytkownika.

Masz dostęp do źródeł działań związanych z logowaniem do usługi Azure AD, inspekcją i ryzykiem dzienników zdarzeń, które umożliwiają integrację z platformą Azure, lub SIEM innych firm.

Proces ten można usprawnić, tworząc ustawienia diagnostyczne dla kont użytkowników usługi Azure AD i wysyłając dzienniki inspekcji i dzienniki logowania do obszaru roboczego Log Analytics. Żądane alerty dzienników można skonfigurować w ramach Log Analytics.

* [Jak zintegrować dzienniki aktywności platformy Azure z usługą Azure Monitor](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics)

* [Jak przejść do tablicy wskaźnikowej platformy Azure](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: klient

### <a name="312-alert-on-account-login-behavior-deviation"></a>3,12: odchylenia zachowania podczas logowania do konta

**Wskazówki**: w celu odróżnienia działania logowania do konta na płaszczyźnie kontroli (np. Azure Portal) użyj funkcji Azure AD Identity Protection i wykrywania ryzyka, aby skonfigurować automatyczne odpowiedzi na wykryte podejrzane działania związane z tożsamościami użytkowników. Możesz również pozyskać dane do usługi Azure wskaźnikowej na potrzeby dalszej analizy.

* [Jak wyświetlić ryzykowne logowanie do usługi Azure AD](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-risky-sign-ins)

* [Jak skonfigurować i włączyć zasady dotyczące ryzyka związanego z ochroną tożsamości](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-identity-protection-configure-risk-policies)

* [Jak dołączyć wskaźnik na platformie Azure](https://docs.microsoft.com/azure/sentinel/quickstart-onboard)

**Monitorowanie Azure Security Center**: obecnie niedostępna

**Odpowiedzialność**: klient

### <a name="313-provide-microsoft-with-access-to-relevant-customer-data-during-support-scenarios"></a>3,13: Zapewnij firmie Microsoft dostęp do odpowiednich danych klienta w scenariuszach pomocy technicznej

**Wskazówki**: nie dotyczy; Skrytka klienta nie ma zastosowania do usługi Azure Network Watcher.

* [Lista usług obsługiwanych przez Skrytka klienta](https://docs.microsoft.com/azure/security/fundamentals/customer-lockbox-overview#supported-services-and-scenarios-in-general-availability)

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: nie dotyczy

## <a name="data-protection"></a>Ochrona danych

*Aby uzyskać więcej informacji, zobacz [Kontrola zabezpieczeń: Ochrona danych](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-protection).*

### <a name="41-maintain-an-inventory-of-sensitive-information"></a>4,1: przechowywanie spisu poufnych informacji

**Wskazówki**: Użyj tagów, aby pomóc w śledzeniu zasobów platformy Azure, które przechowują lub przetwarzają informacje poufne.

* [Tworzenie i używanie tagów](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: nie dotyczy

### <a name="42-isolate-systems-storing-or-processing-sensitive-information"></a>4,2: Izoluj systemy przechowujące lub przetwarzające informacje poufne

**Wskazówki**: implementowanie oddzielnych subskrypcji i/lub grup zarządzania na potrzeby tworzenia, testowania i produkcji.

* [Jak utworzyć dodatkowe subskrypcje platformy Azure](https://docs.microsoft.com/azure/billing/billing-create-subscription)

* [Jak utworzyć Grupy zarządzania](https://docs.microsoft.com/azure/governance/management-groups/create)

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: nie dotyczy

### <a name="43-monitor-and-block-unauthorized-transfer-of-sensitive-information"></a>4,3: Monitoruj i blokuj nieautoryzowany transfer informacji poufnych

**Wskazówki**: Firma Microsoft zarządza podstawową infrastrukturą dla Network Watcher platformy Azure i powiązanych zasobów oraz ma zaimplementowane ścisłe kontrole, aby zapobiec utracie lub narażeniu danych klientów.

* [Zrozumienie ochrony danych klientów na platformie Azure](https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data)

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: Microsoft

### <a name="44-encrypt-all-sensitive-information-in-transit"></a>4,4: Szyfruj wszystkie poufne informacje podczas przesyłania

**Wskazówki**: Jeśli korzystasz z usługi Azure VPN Gateway w celu utworzenia bezpiecznego połączenia między siecią lokalną i sieciami wirtualnymi platformy Azure, upewnij się, że lokalna Brama sieci lokalnej została skonfigurowana ze zgodnymi parametrami komunikacji i szyfrowania IPSec. Każda niepowodzna konfiguracja spowoduje utratę łączności między siecią lokalną a platformą Azure.

* [Obsługiwane parametry protokołu IPSec dla usługi Azure VPN Gateway](https://docs.microsoft.com/azure/network-watcher/network-watcher-diagnose-on-premises-connectivity)

* [Jak skonfigurować połączenie lokacja-lokacja w Azure Portal](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal)

**Monitorowanie Azure Security Center**: obecnie niedostępna

**Odpowiedzialność**: klient

### <a name="45-use-an-active-discovery-tool-to-identify-sensitive-data"></a>4,5: Użyj aktywnego narzędzia do odnajdywania, aby identyfikować poufne dane

**Wskazówki**: nie dotyczy; Na platformie Azure Network Watcher nie są przechowywane żadne dane klientów.

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: nie dotyczy

### <a name="46-use-role-based-access-control-to-control-access-to-resources"></a>4,6: stosowanie kontroli dostępu opartej na rolach w celu kontrolowania dostępu do zasobów

**Wskazówki**: Aby skonfigurować kontrolę dostępu opartą na ROLACH (RBAC), możesz użyć okienka kontrola tożsamości i dostępu (IAM) w Azure Portal dla subskrypcji. Role są stosowane do użytkowników, grup, podmiotów usługi i tożsamości zarządzanych w Active Directory. Można używać wbudowanych ról lub ról niestandardowych dla użytkowników indywidualnych i grup.

Aby można było korzystać z funkcji Network Watcher, konto, które logujesz się do platformy Azure, musi być przypisane do wbudowanych ról współautor, współautor lub sieci lub przypisane do roli niestandardowej przypisanej do określonych funkcji Network Watcher.

* [Jak skonfigurować RBAC na platformie Azure](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal)

* [Opis uprawnień RBAC w Network Watcher](https://docs.microsoft.com/azure/network-watcher/required-rbac-permissions)

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: klient

### <a name="47-use-host-based-data-loss-prevention-to-enforce-access-control"></a>4,7: Wymuś kontrolę dostępu przy użyciu ochrony przed utratą danych opartą na hoście

**Wskazówki**: nie dotyczy; to zalecenie jest przeznaczone do zasobów obliczeniowych. Firma Microsoft zarządza podstawową infrastrukturą dla systemu Azure Network Watcher i ma zaimplementowane ścisłe kontrole, aby zapobiec utracie lub narażeniu danych klientów.

* [Ochrona danych klienta platformy Azure](https://docs.microsoft.com/azure/security/fundamentals/protection-customer-data)

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: nie dotyczy

### <a name="48-encrypt-sensitive-information-at-rest"></a>4,8: Szyfruj poufne informacje w spoczynku

**Wskazówki**: nie dotyczy; Na platformie Azure Network Watcher nie są przechowywane żadne dane klientów. Network Watcher przechowuje dzienniki i inne informacje w usłudze Azure Storage, w przypadku których dane są szyfrowane w stanie spoczynku.

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: Microsoft

### <a name="49-log-and-alert-on-changes-to-critical-azure-resources"></a>4,9: Rejestruj i Ostrzegaj o zmianach krytycznych zasobów platformy Azure

**Wskazówki**: Użyj Azure monitor z dziennikiem aktywności platformy Azure, aby utworzyć alerty, kiedy zmiany są wprowadzane do usługi Azure Network Watcher i innych krytycznych lub powiązanych zasobów.

* [Jak utworzyć alerty dla zdarzeń dziennika aktywności platformy Azure](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log)

**Monitorowanie Azure Security Center**: tak

**Odpowiedzialność**: klient

## <a name="vulnerability-management"></a>Zarządzanie lukami w zabezpieczeniach

*Aby uzyskać więcej informacji, zobacz [Kontrola zabezpieczeń: Zarządzanie lukami w zabezpieczeniach](https://docs.microsoft.com/azure/security/benchmarks/security-control-vulnerability-management).*

### <a name="51-run-automated-vulnerability-scanning-tools"></a>5,1: uruchamianie narzędzi do skanowania automatycznych luk w zabezpieczeniach

**Wskazówki**: nie dotyczy; Firma Microsoft przeprowadza zarządzanie lukami w systemach podstawowych, które obsługują usługę Azure Network Watcher.

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: nie dotyczy

### <a name="52-deploy-automated-operating-system-patch-management-solution"></a>5,2: Wdróż automatyczne rozwiązanie do zarządzania poprawkami systemu operacyjnego

**Wskazówki**: nie dotyczy; to zalecenie jest przeznaczone do zasobów obliczeniowych.

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: nie dotyczy

### <a name="53-deploy-automated-patch-management-solution-for-third-party-software-titles"></a>5,3: Wdróż rozwiązanie zautomatyzowanego zarządzania poprawkami dla tytułów oprogramowania innych firm

**Wskazówki**: nie dotyczy; to zalecenie jest przeznaczone do zasobów obliczeniowych.

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: nie dotyczy

### <a name="54-compare-back-to-back-vulnerability-scans"></a>5,4: porównanie luk w zabezpieczeniach z tyłu do tyłu

**Wskazówki**: nie dotyczy; Firma Microsoft przeprowadza zarządzanie lukami w systemach podstawowych, które obsługują usługę Azure Network Watcher.

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: nie dotyczy

### <a name="55-use-a-risk-rating-process-to-prioritize-the-remediation-of-discovered-vulnerabilities"></a>5,5: Użyj procesu oceny ryzyka, aby określić priorytety korygowania odkrytych luk w zabezpieczeniach

**Wskazówki**: nie dotyczy; Firma Microsoft przeprowadza zarządzanie lukami w systemach podstawowych, które obsługują usługę Azure Network Watcher.

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: nie dotyczy

## <a name="inventory-and-asset-management"></a>Zarządzanie magazynem i zasobami

*Aby uzyskać więcej informacji, zobacz [Kontrola zabezpieczeń: Spis i zarządzanie zasobami](https://docs.microsoft.com/azure/security/benchmarks/security-control-inventory-asset-management).*

### <a name="61-use-automated-asset-discovery-solution"></a>6,1: Użyj rozwiązania automatycznego odnajdywania zasobów

**Wskazówki**: Użyj grafu zasobów platformy Azure do wykonywania zapytań/odnajdywania wszystkich zasobów (takich jak obliczenia, magazyn, Sieć, porty i protokoły itp.) w ramach subskrypcji. Upewnij się, że masz odpowiednie uprawnienia (odczyt) w dzierżawie i Wylicz wszystkie subskrypcje platformy Azure oraz zasoby w ramach subskrypcji.

Mimo że klasyczne zasoby platformy Azure mogą zostać odnalezione za pośrednictwem grafu zasobów, zdecydowanie zaleca się tworzenie i używanie Azure Resource Manager zasobów do przodu.

* [Jak tworzyć zapytania przy użyciu grafu zasobów platformy Azure](https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal)

* [Jak wyświetlić subskrypcje platformy Azure](https://docs.microsoft.com/powershell/module/az.accounts/get-azsubscription?view=azps-3.0.0)

* [Opis kontroli RBAC platformy Azure](https://docs.microsoft.com/azure/role-based-access-control/overview)

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: klient

### <a name="62-maintain-asset-metadata"></a>6,2: Konserwowanie metadanych zasobów

**Wskazówki**: stosowanie tagów do zasobów platformy Azure, dzięki czemu metadane są logicznie zorganizowane w taksonomię.

* [Tworzenie i używanie tagów](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: nie dotyczy

### <a name="63-delete-unauthorized-azure-resources"></a>6,3: Usuń nieautoryzowane zasoby platformy Azure

**Wskazówki**: używanie tagowania, grup zarządzania i oddzielnych subskrypcji, gdzie to konieczne, do organizowania i śledzenia zasobów platformy Azure. Regularnie Uzgadniaj spis i zapewnij, że nieautoryzowane zasoby są usuwane z subskrypcji w odpowiednim czasie.

Ponadto należy używać Azure Policy do umieszczania ograniczeń dotyczących typu zasobów, które można utworzyć w subskrypcjach klientów, przy użyciu następujących wbudowanych definicji zasad:
- Niedozwolone typy zasobów
- Dozwolone typy zasobów

* [Jak utworzyć dodatkowe subskrypcje platformy Azure](https://docs.microsoft.com/azure/billing/billing-create-subscription)

* [Tworzenie grup zarządzania](https://docs.microsoft.com/azure/governance/management-groups/create)

* [Tworzenie i używanie tagów](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags)

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: klient

### <a name="64-define-and-maintain-an-inventory-of-approved-azure-resources"></a>6,4: Definiowanie i obsługa spisu zatwierdzonych zasobów platformy Azure

**Wskazówki**: nie dotyczy; to zalecenie jest przeznaczone do zasobów obliczeniowych.

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: nie dotyczy

### <a name="65-monitor-for-unapproved-azure-resources"></a>6,5: Monitoruj niezatwierdzone zasoby platformy Azure

**Wskazówki**: Użyj Azure Policy, aby wprowadzić ograniczenia dotyczące typu zasobów, które mogą być tworzone w ramach subskrypcji.

Użyj grafu zasobów platformy Azure do wykonywania zapytań dotyczących zasobów i odnajdywania ich w ramach subskrypcji. Upewnij się, że wszystkie zasoby platformy Azure obecne w środowisku są zatwierdzone.

* [Jak skonfigurować Azure Policy i zarządzać nimi](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Jak tworzyć zapytania przy użyciu grafu zasobów platformy Azure](https://docs.microsoft.com/azure/governance/resource-graph/first-query-portal)

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: klient

### <a name="66-monitor-for-unapproved-software-applications-within-compute-resources"></a>6,6: Monitoruj niezatwierdzone aplikacje oprogramowania w ramach zasobów obliczeniowych

**Wskazówki**: nie dotyczy; to zalecenie jest przeznaczone do zasobów obliczeniowych.

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: nie dotyczy

### <a name="67-remove-unapproved-azure-resources-and-software-applications"></a>6,7: Usuń niezatwierdzone zasoby platformy Azure i aplikacje oprogramowania

**Wskazówki**: nie dotyczy; to zalecenie jest przeznaczone do zasobów obliczeniowych.

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: nie dotyczy

### <a name="68-use-only-approved-applications"></a>6,8: Używaj tylko zatwierdzonych aplikacji

**Wskazówki**: nie dotyczy; to zalecenie jest przeznaczone do zasobów obliczeniowych.

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: nie dotyczy

### <a name="69-use-only-approved-azure-services"></a>6,9: Używaj tylko zatwierdzonych usług platformy Azure

**Wskazówki**: Użyj Azure Policy, aby wprowadzić ograniczenia dotyczące typu zasobów, które można utworzyć w subskrypcjach klientów, przy użyciu następujących wbudowanych definicji zasad:
- Niedozwolone typy zasobów
- Dozwolone typy zasobów

* [Jak skonfigurować Azure Policy i zarządzać nimi](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Jak odmówić określonego typu zasobu za pomocą Azure Policy](https://docs.microsoft.com/azure/governance/policy/samples/not-allowed-resource-types)

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: klient

### <a name="610-maintain-an-inventory-of-approved-software-titles"></a>6,10: przechowywanie spisu zatwierdzonych tytułów oprogramowania

**Wskazówki**: nie dotyczy; to zalecenie jest przeznaczone do zasobów obliczeniowych.

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: nie dotyczy

### <a name="611-limit-users-ability-to-interact-with-azure-resource-manager"></a>6,11: Ogranicz możliwość korzystania przez użytkowników z Azure Resource Manager

**Wskazówki**: Skonfiguruj dostęp warunkowy platformy Azure, aby ograniczyć możliwość korzystania przez użytkowników z Azure Resource Manager przez skonfigurowanie "blokowania dostępu" dla aplikacji "Microsoft Azure Management".

* [Jak skonfigurować dostęp warunkowy w celu blokowania dostępu do Azure Resource Manager](https://docs.microsoft.com/azure/role-based-access-control/conditional-access-azure-management)

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: klient

### <a name="612-limit-users-ability-to-execute-scripts-within-compute-resources"></a>6,12: Ogranicz możliwość wykonywania skryptów w zasobach obliczeniowych przez użytkowników

**Wskazówki**: nie dotyczy; to zalecenie jest przeznaczone do zasobów obliczeniowych.

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: nie dotyczy

### <a name="613-physically-or-logically-segregate-high-risk-applications"></a>6,13: fizyczne lub logiczne rozdzielenie aplikacji wysokiego ryzyka

**Wskazówki**: nie dotyczy; to zalecenie jest przeznaczone dla aplikacji sieci Web działających na Azure App Service lub zasobach obliczeniowych.

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: nie dotyczy

## <a name="secure-configuration"></a>Bezpieczna konfiguracja

*Aby uzyskać więcej informacji, zobacz [Kontrola zabezpieczeń: bezpieczna konfiguracja](https://docs.microsoft.com/azure/security/benchmarks/security-control-secure-configuration).*

### <a name="71-establish-secure-configurations-for-all-azure-resources"></a>7,1: Ustanów bezpieczne konfiguracje dla wszystkich zasobów platformy Azure

**Wskazówki**: Definiowanie i implementowanie standardowych konfiguracji zabezpieczeń dla usługi Azure Network Watcher przy użyciu Azure Policy. Użyj aliasów Azure Policy w przestrzeni nazw "Microsoft. Network", aby utworzyć zasady niestandardowe do inspekcji lub wymuszania konfiguracji sieci wystąpień Network Watcher. Możesz również używać wbudowanych definicji zasad, takich jak:

* [Wdróż obserwatora sieciowego po utworzeniu sieci wirtualnych](https://github.com/Azure/azure-policy/blob/master/samples/built-in-policy/deploy-network-watcher-in-vnet-regions/README.md)

* [Zobacz również: jak konfigurować Azure Policy i zarządzać nimi](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Jak utworzyć zasadę niestandardową z aliasami zasad](https://docs.microsoft.com/azure/governance/policy/tutorials/create-custom-policy-definition)

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: nie dotyczy

### <a name="72-establish-secure-operating-system-configurations"></a>7,2: Ustanów bezpieczne konfiguracje systemów operacyjnych

**Wskazówki**: nie dotyczy; te wytyczne są przeznaczone dla zasobów obliczeniowych.

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: nie dotyczy

### <a name="73-maintain-secure-azure-resource-configurations"></a>7,3: obsługa bezpiecznych konfiguracji zasobów platformy Azure

**Wskazówki**: Użyj Azure Policy [Odmów] i [Wdróż, jeśli nie istnieje], aby wymusić bezpieczne ustawienia dla zasobów platformy Azure.

* [Jak skonfigurować Azure Policy i zarządzać nimi](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Zrozumienie efektów Azure Policy](https://docs.microsoft.com/azure/governance/policy/concepts/effects)

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: nie dotyczy

### <a name="74-maintain-secure-operating-system-configurations"></a>7,4: Zachowaj konfiguracje bezpiecznego systemu operacyjnego

**Wskazówki**: nie dotyczy; te wytyczne są przeznaczone dla zasobów obliczeniowych.

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: nie dotyczy

### <a name="75-securely-store-configuration-of-azure-resources"></a>7,5: bezpiecznie przechowuj konfigurację zasobów platformy Azure

**Wskazówki**: Jeśli używasz niestandardowych definicji Azure Policy, Użyj usługi Azure DevOps lub Azure Repos, aby bezpiecznie przechowywać kod i zarządzać nim.

* [Jak przechowywać kod w usłudze Azure DevOps](https://docs.microsoft.com/azure/devops/repos/git/gitworkflow?view=azure-devops)

* [Dokumentacja Azure Repos](https://docs.microsoft.com/azure/devops/repos/index?view=azure-devops)

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: nie dotyczy

### <a name="76-securely-store-custom-operating-system-images"></a>7,6: bezpieczne przechowywanie niestandardowych obrazów systemu operacyjnego

**Wskazówki**: nie dotyczy; te wytyczne są przeznaczone dla zasobów obliczeniowych.

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: nie dotyczy

### <a name="77-deploy-configuration-management-tools-for-azure-resources"></a>7,7: Wdrażanie narzędzi do zarządzania konfiguracją dla zasobów platformy Azure

**Wskazówki**: Definiowanie i implementowanie standardowych konfiguracji zabezpieczeń dla usługi Azure Network Watcher przy użyciu Azure Policy. Użyj aliasów Azure Policy w przestrzeni nazw "Microsoft. Network", aby utworzyć zasady niestandardowe do inspekcji lub wymuszania konfiguracji sieci wystąpień Network Watcher. Możesz również używać wbudowanych definicji zasad, takich jak:

* [Wdróż obserwatora sieciowego po utworzeniu sieci wirtualnych](https://github.com/Azure/azure-policy/blob/master/samples/built-in-policy/deploy-network-watcher-in-vnet-regions/README.md)

Zobacz również:

* [Jak skonfigurować Azure Policy i zarządzać nimi](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

* [Jak utworzyć zasadę niestandardową z aliasami zasad](https://docs.microsoft.com/azure/governance/policy/tutorials/create-custom-policy-definition)

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: nie dotyczy

### <a name="78-deploy-configuration-management-tools-for-operating-systems"></a>7,8: Wdrażanie narzędzi do zarządzania konfiguracją dla systemów operacyjnych

**Wskazówki**: nie dotyczy; te wytyczne są przeznaczone dla zasobów obliczeniowych.

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: nie dotyczy

### <a name="79-implement-automated-configuration-monitoring-for-azure-resources"></a>7,9: Zaimplementuj automatyczne monitorowanie konfiguracji dla zasobów platformy Azure

**Wskazówki**: Użyj wbudowanych definicji Azure Policy, a także aliasów Azure Policy w przestrzeni nazw "Microsoft. Network", aby utworzyć niestandardowe definicje Azure Policy na potrzeby alertów, inspekcji i wymuszania konfiguracji systemu. Użyj Azure Policy [Audit], [Odmów] i [Wdróż, jeśli nie istnieje], aby automatycznie wymuszać konfiguracje dla zasobów platformy Azure.

* [Jak skonfigurować Azure Policy i zarządzać nimi](https://docs.microsoft.com/azure/governance/policy/tutorials/create-and-manage)

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: nie dotyczy

### <a name="710-implement-automated-configuration-monitoring-for-operating-systems"></a>7,10: Zaimplementuj automatyczne monitorowanie konfiguracji dla systemów operacyjnych

**Wskazówki**: nie dotyczy; te wytyczne są przeznaczone dla zasobów obliczeniowych.

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: nie dotyczy

### <a name="711-manage-azure-secrets-securely"></a>7,11: bezpieczne zarządzanie wpisami tajnymi platformy Azure

**Wskazówki**: nie dotyczy; nie istnieją hasła, wpisy tajne ani klucze skojarzone z usługą Azure Network Watcher.

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: nie dotyczy

### <a name="712-manage-identities-securely-and-automatically"></a>7,12: bezpieczne i automatyczne zarządzanie tożsamościami

**Wskazówki**: nie dotyczy; Usługa Azure Network Watcher nie korzysta z zarządzanych tożsamości.

* [Usługi platformy Azure, które obsługują tożsamości zarządzane](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/services-support-managed-identities)

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: nie dotyczy

### <a name="713-eliminate-unintended-credential-exposure"></a>7,13: eliminowanie nieprzewidzianego narażenia na poświadczenia

**Wskazówki**: Implementuj skaner poświadczeń, aby identyfikować poświadczenia w kodzie. Skaner poświadczeń zachęca również do przemieszczania odnalezionych poświadczeń do bardziej bezpiecznych lokalizacji, takich jak Azure Key Vault.

* [Jak skonfigurować skaner poświadczeń](https://secdevtools.azurewebsites.net/helpcredscan.html)

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: klient

## <a name="malware-defense"></a>Ochrona przed złośliwym oprogramowaniem

*Aby uzyskać więcej informacji, zobacz [Kontrola zabezpieczeń: Obrona złośliwego oprogramowania](https://docs.microsoft.com/azure/security/benchmarks/security-control-malware-defense).*

### <a name="81-use-centrally-managed-anti-malware-software"></a>8,1: Użyj centralnie zarządzanego oprogramowania chroniącego przed złośliwym oprogramowaniem

**Wskazówki**: nie dotyczy; te wytyczne są przeznaczone dla zasobów obliczeniowych. Oprogramowanie chroniące przed złośliwym oprogramowaniem firmy Microsoft jest włączone na podstawowym hoście, który obsługuje usługi platformy Azure (na przykład Azure App Service), ale nie jest uruchamiane w treści klienta.

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: nie dotyczy

### <a name="82-pre-scan-files-to-be-uploaded-to-non-compute-azure-resources"></a>8,2: przeskanuj pliki przed przekazaniem do zasobów platformy Azure, które nie są obliczeniowe

**Wskazówki**: nie dotyczy. Network Watcher nie działa na danych przekazanych przez użytkownika.

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: klient

### <a name="83-ensure-anti-malware-software-and-signatures-are-updated"></a>8,3: Upewnij się, że oprogramowanie chroniące przed złośliwym oprogramowaniem i podpisy zostały zaktualizowane

**Wskazówki**: nie dotyczy; to zalecenie jest przeznaczone do zasobów obliczeniowych. Oprogramowanie chroniące przed złośliwym oprogramowaniem firmy Microsoft jest włączone na podstawowym hoście obsługującym usługi platformy Azure, ale nie jest uruchamiane w treści klienta.

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: nie dotyczy

## <a name="data-recovery"></a>Odzyskiwanie danych

*Aby uzyskać więcej informacji, zobacz [Kontrola zabezpieczeń: odzyskiwanie danych](https://docs.microsoft.com/azure/security/benchmarks/security-control-data-recovery).*

### <a name="91-ensure-regular-automated-back-ups"></a>9,1: Zapewnij regularne zautomatyzowane przywracanie awaryjne

**Wskazówki**: nie dotyczy; Na platformie Azure Network Watcher nie są przechowywane żadne dane klientów.

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: nie dotyczy

### <a name="92-perform-complete-system-backups-and-backup-any-customer-managed-keys"></a>9,2: wykonaj kompletne kopie zapasowe systemu i Utwórz kopię zapasową wszystkich kluczy zarządzanych przez klienta

**Wskazówki**: nie dotyczy; Na platformie Azure Network Watcher nie są przechowywane żadne dane klientów.

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: nie dotyczy

### <a name="93-validate-all-backups-including-customer-managed-keys"></a>9,3: Weryfikuj wszystkie kopie zapasowe, w tym klucze zarządzane przez klienta

**Wskazówki**: nie dotyczy; Na platformie Azure Network Watcher nie są przechowywane żadne dane klientów.

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: nie dotyczy

### <a name="94-ensure-protection-of-backups-and-customer-managed-keys"></a>9,4: zapewnianie ochrony kopii zapasowych i kluczy zarządzanych przez klienta

**Wskazówki**: nie dotyczy; Na platformie Azure Network Watcher nie są przechowywane żadne dane klientów.

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: nie dotyczy

## <a name="incident-response"></a>Reagowanie na zdarzenia

*Aby uzyskać więcej informacji, zobacz [Kontrola zabezpieczeń: odpowiedź na zdarzenia](https://docs.microsoft.com/azure/security/benchmarks/security-control-incident-response).*

### <a name="101-create-an-incident-response-guide"></a>10,1: Tworzenie przewodnika odpowiedzi na zdarzenia

**Wskazówki**: Tworzenie przewodnika odpowiedzi na zdarzenia dla organizacji. Upewnij się, że istnieją zarejestrowane plany reakcji na zdarzenia, które definiują wszystkie role pracowników, a także etapy obsługi zdarzeń/zarządzania z wykrywania do oceny po zdarzeniu.

* [Jak skonfigurować automatyzację przepływu pracy w Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-planning-and-operations-guide)

* [Wskazówki dotyczące tworzenia własnego procesu reagowania na zdarzenia zabezpieczeń](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

* [Anatomia incydentu centrum Microsoft Security Response](https://msrc-blog.microsoft.com/2019/07/01/inside-the-msrc-building-your-own-security-incident-response-process/)

* [Klient może również korzystać z przewodnika obsługi zdarzeń związanych z bezpieczeństwem programu NIST, aby pomóc w tworzeniu własnego planu reagowania na zdarzenia](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: klient

### <a name="102-create-an-incident-scoring-and-prioritization-procedure"></a>10,2: Tworzenie oceny incydentu i procedury priorytetyzacji

**Wskazówki**: Security Center przypisuje ważność do każdego alertu, aby pomóc w ustaleniu, które alerty należy najpierw zbadać. Ważność jest oparta na tym, jak dobrze Security Center znajduje się w wyszukiwaniu lub analitycznym używanym do wystawiania alertu, a także poziom pewności, że istniało złośliwy wpływ na działanie, które prowadziło do alertu.

Dodatkowo jasno Oznacz subskrypcje (na przykład produkcyjny, nieprodukcyjny) i Utwórz system nazewnictwa, aby jasno identyfikować i klasyfikować zasoby platformy Azure.

**Monitorowanie Azure Security Center**: tak

**Odpowiedzialność**: klient

### <a name="103-test-security-response-procedures"></a>10,3: procedury odpowiedzi na zabezpieczenia testowe

**Wskazówki**: przeprowadzanie ćwiczeń w celu przetestowania możliwości reagowania na zdarzenia systemu w regularnych erze. Zidentyfikuj słabe punkty i przerwy i popraw plan zgodnie z wymaganiami.

* [Zapoznaj się z publikacją NIST: Przewodnik dotyczący testowania, uczenia i ćwiczeń programów dla planów i możliwości IT](https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-84.pdf)

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: klient

### <a name="104-provide-security-incident-contact-details-and-configure-alert-notifications-for-security-incidents"></a>10,4: Podaj szczegóły kontaktu dotyczącego zabezpieczeń i Skonfiguruj powiadomienia dotyczące alertów dotyczących zdarzeń związanych z zabezpieczeniami

**Wskazówki**: informacje kontaktowe dotyczące zdarzenia zabezpieczeń będą używane przez firmę Microsoft do skontaktowania się z Tobą, jeśli firma Microsoft Security Response Center (MSRC) wykryje, że dostęp do danych klienta został uzyskany przez nielegalną lub nieautoryzowaną osobę. Przejrzyj zdarzenia po fakcie, aby upewnić się, że problemy zostały rozwiązane.

* [Jak ustawić kontakt z zabezpieczeniami Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-provide-security-contact-details)

**Monitorowanie Azure Security Center**: tak

**Odpowiedzialność**: klient

### <a name="105-incorporate-security-alerts-into-your-incident-response-system"></a>10,5: Uwzględnij alerty zabezpieczeń w systemie odpowiedzi na zdarzenia

**Wskazówki**: eksportowanie alertów i zaleceń dotyczących Azure Security Center przy użyciu funkcji eksportu ciągłego. Eksport ciągły umożliwia wyeksportowanie alertów i zaleceń ręcznie lub w stały sposób ciągły. Możesz użyć łącznika danych Azure Security Center, aby przesłać strumieniowo wskaźnik do alertów.

* [Jak skonfigurować eksport ciągły](https://docs.microsoft.com/azure/security-center/continuous-export)

* [Jak przesłać strumieniowo alerty do usługi Azure wskaźnikowego](https://docs.microsoft.com/azure/sentinel/connect-azure-security-center)

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: klient

### <a name="106-automate-the-response-to-security-alerts"></a>10,6: Automatyzowanie odpowiedzi na alerty zabezpieczeń

**Wskazówki**: Użyj funkcji automatyzacji przepływu pracy w programie Azure Security Center, aby automatycznie wyzwalać odpowiedzi za pośrednictwem "Logic Apps" na temat alertów zabezpieczeń i zaleceń.

* [Jak skonfigurować automatyzację przepływu pracy i Logic Apps](https://docs.microsoft.com/azure/security-center/workflow-automation)

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: klient

## <a name="penetration-tests-and-red-team-exercises"></a>Testy penetracyjne i ćwiczenia typu „red team”

*Aby uzyskać więcej informacji, zobacz [Kontrola zabezpieczeń: testy penetracji i czerwone ćwiczenia zespołu](https://docs.microsoft.com/azure/security/benchmarks/security-control-penetration-tests-red-team-exercises).*

### <a name="111-conduct-regular-penetration-testing-of-your-azure-resources-and-ensure-remediation-of-all-critical-security-findings"></a>11,1: Przeprowadź regularne testowanie penetracji zasobów platformy Azure i zadbaj o skorygowanie wszystkich krytycznych ustaleń dotyczących zabezpieczeń

**Wskazówki**: * [postępuj zgodnie z zasadami firmy Microsoft dotyczącymi zaangażowania, aby upewnić się, że testy penetracji nie naruszają zasad firmy Microsoft](https://www.microsoft.com/msrc/pentest-rules-of-engagement?rtc=1)

* [W tym miejscu znajdziesz więcej informacji na temat strategii i wykonywania trójwymiarowych operacji tworzenia zespołu i testowania aplikacji na żywo w witrynie Microsoft.](https://gallery.technet.microsoft.com/Cloud-Red-Teaming-b837392e)

**Monitorowanie Azure Security Center**: nie dotyczy

**Odpowiedzialność**: udostępnione

## <a name="next-steps"></a>Następne kroki

- Zobacz [test porównawczy zabezpieczeń platformy Azure](https://docs.microsoft.com/azure/security/benchmarks/overview)
- Dowiedz się więcej o [punktach odniesienia zabezpieczeń platformy Azure](https://docs.microsoft.com/azure/security/benchmarks/security-baselines-overview)
