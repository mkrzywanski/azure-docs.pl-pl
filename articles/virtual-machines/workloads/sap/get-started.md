---
title: Wprowadzenie do oprogramowania SAP na maszynach wirtualnych platformy Azure | Microsoft Docs
description: Dowiedz się więcej o rozwiązaniach SAP uruchamianych na maszynach wirtualnych w Microsoft Azure
services: virtual-machines-linux
documentationcenter: ''
author: msjuergent
manager: bburns
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: ad8e5c75-0cf6-4564-ae62-ea1246b4e5f2
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 07/16/2020
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7319adfc88eedb007677a78693ab0b2e514e646f
ms.sourcegitcommit: d7bd8f23ff51244636e31240dc7e689f138c31f0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/24/2020
ms.locfileid: "87171665"
---
# <a name="use-azure-to-host-and-run-sap-workload-scenarios"></a>Korzystanie z platformy Azure do hostowania i uruchamiania scenariuszy obciążeń SAP

W przypadku korzystania z Microsoft Azure można w niezawodny sposób uruchamiać obciążenia i scenariusze o kluczowym znaczeniu SAP w skalowalnej, zgodnej i korporacyjnej platformie. Uzyskasz skalowalność, elastyczność i oszczędność kosztów platformy Azure. Dzięki rozbudowanym partnerstwu między firmą Microsoft i SAP można uruchamiać aplikacje SAP w ramach scenariuszy deweloperskich i testowych oraz produkcyjnych na platformie Azure i być w pełni obsługiwane. W przypadku oprogramowania SAP NetWeaver do SAP S/4HANA, SAP BI w systemie Linux do systemu Windows i SAP HANA do bazy danych SQL.

Oprócz hostowania scenariuszy SAP NetWeaver z różnymi systemami DBMS na platformie Azure, można hostować inne scenariusze obciążeń SAP, takie jak SAP BI na platformie Azure. 

Unikatowość platformy Azure dla SAP HANA to oferta, która umożliwia rozróżnianie platformy Azure. Aby umożliwić hostowanie większej ilości pamięci i scenariuszy SAP wymagających zasobów procesora, które obejmują SAP HANA, platforma Azure umożliwia korzystanie z sprzętu bez systemu operacyjnego przeznaczonego dla klientów. Za pomocą tego rozwiązania można uruchamiać wdrożenia SAP HANA, które wymagają do 24 TB (120 TB skalowania w poziomie) pamięci dla usługi S/4HANA lub innych obciążeń SAP HANA. 

Scenariusze obsługi obciążeń SAP na platformie Azure mogą również stworzyć wymagania integracji tożsamości i logowania jednokrotnego. Taka sytuacja może wystąpić w przypadku korzystania z Azure Active Directory (Azure AD) w celu łączenia różnych składników SAP oraz ofert oprogramowania SAP jako usługi (SaaS) lub platformy jako usługi (PaaS). Lista takich integracji i scenariuszy logowania jednokrotnego za pomocą usługi Azure AD i jednostek SAP została opisana i udokumentowana w sekcji "Integracja tożsamości i logowanie jednokrotne w usłudze AAD SAP".

## <a name="changes-to-the-sap-workload-section"></a>Zmiany w sekcji obciążenia SAP
Zmiany w dokumentach w sekcji obciążenia SAP na platformie Azure są wymienione na końcu tego artykułu. Wpisy w dzienniku zmian są przechowywane przez około 180 dni.

## <a name="you-want-to-know"></a>Chcesz wiedzieć
Jeśli masz określone pytania, przejdźmy do określonych dokumentów lub przepływów w tej sekcji na stronie startowej. Chcesz poznać następujące informacje:

- Obsługiwane jednostki maszyn wirtualnych platformy Azure i usługi HANA, dla których wydano oprogramowanie SAP i wersje systemu operacyjnego. Zapoznaj się z dokumentem [jakie oprogramowanie SAP jest obsługiwane w przypadku wdrażania na platformie Azure](./sap-supported-product-on-azure.md) na potrzeby odpowiedzi i proces znajdowania informacji
- Scenariusze wdrażania SAP są obsługiwane przez maszyny wirtualne platformy Azure i duże wystąpienia HANA. Informacje o obsługiwanych scenariuszach można znaleźć w dokumentach:
    - [Obsługiwane scenariusze obciążenia SAP na maszynie wirtualnej na platformie Azure](./sap-planning-supported-configurations.md)
    - [Obsługiwane scenariusze dla dużego wystąpienia HANA](./hana-supported-scenario.md)
- Jakie usługi platformy Azure, typy maszyn wirtualnych platformy Azure i usługi Azure Storage są dostępne w różnych regionach świadczenia usługi Azure, sprawdź dostępne dla lokacji [produkty według regionów](https://azure.microsoft.com/global-infrastructure/services/) 
- Czy ramka HA innej firmy działa, oprócz systemu Windows i Pacemaker? Sprawdź dolną część [uwagi dotyczącej pomocy technicznej SAP #1928533](https://launchpad.support.sap.com/#/notes/1928533)

 
## <a name="sap-hana-on-azure-large-instances"></a>Oprogramowanie SAP HANA na platformie Azure (duże wystąpienia)

Seria dokumentów prowadzi użytkownika przez SAP HANA na platformie Azure (duże wystąpienia) lub w przypadku krótkich dużych wystąpień usługi HANA. Aby uzyskać informacje o dużych wystąpieniach platformy HANA, należy zapoznać się z [omówieniem dokumentu i architekturą SAP HANA na platformie Azure (duże wystąpienia)](./hana-overview-architecture.md) i zapoznać się z podaną dokumentacją w sekcji duże wystąpienie usługi Hana



## <a name="sap-hana-on-azure-virtual-machines"></a>SAP HANA w usłudze Azure Virtual Machines
W tej części dokumentacji omówiono różne aspekty SAP HANA. Jako warunek wstępny należy zapoznać się z głównymi usługami platformy Azure, które zapewniają podstawowe usługi platformy Azure IaaS. W związku z tym potrzebna jest znajomość zasobów obliczeniowych, magazynu i sieci platformy Azure. Wiele z tych tematów jest obsługiwanych w [przewodniku planowania platformy Azure](./planning-guide.md)związanego z programem SAP NetWeaver. 

 

## <a name="sap-netweaver-deployed-on-azure-virtual-machines"></a>Oprogramowanie SAP NetWeaver wdrożone na maszynach wirtualnych platformy Azure
Ta sekcja zawiera listę dokumentacji dotyczącej planowania i wdrażania dla oprogramowania SAP NetWeaver i Business One na platformie Azure. Dokumentacja dotyczy podstaw i używania baz danych innych niż HANA z obciążeniem SAP na platformie Azure. Dokumenty i artykuły dotyczące wysokiej dostępności są również podstawą dla wysokiej dostępności HANA na platformie Azure, na przykład:

- [Przewodnik planowania platformy Azure](./planning-guide.md). 
- [Oprogramowanie SAP Business One na maszynach wirtualnych platformy Azure](./business-one-azure.md)
- [Ochrona wielowarstwowego wdrożenia aplikacji SAP NetWeaver przy użyciu Site Recovery](../../../site-recovery/site-recovery-sap.md)
- [Łącznik SAP LaMa dla platformy Azure](./lama-installation.md)

Aby uzyskać informacje na temat baz danych innych niż HANA w obciążeniu SAP na platformie Azure, zobacz:

- [Zagadnienia dotyczące wdrażania systemu Azure Virtual Machines DBMS dla obciążeń SAP](./dbms_guide_general.md)
- [SQL Server wdrożenie systemu Azure Virtual Machines DBMS dla oprogramowania SAP NetWeaver](./dbms_guide_sqlserver.md)
- [Wdrażanie systemu DBMS usługi Azure Virtual Machines oprogramowania Oracle dla obciążenia SAP](./dbms_guide_oracle.md)
- [Wdrożenie programu IBM DB2 Azure Virtual Machines DBMS dla obciążenia SAP](./dbms_guide_ibm.md)
- [Wdrażanie systemu DBMS usługi Azure Virtual Machines produktu SAP ESE dla obciążenia SAP](./dbms_guide_sapase.md)
- [Wdrażanie oprogramowania SAP MaxDB, pamięci podręcznej na żywo i serwera zawartości na maszynach wirtualnych platformy Azure](./dbms_guide_maxdb.md)

Aby uzyskać informacje na temat SAP HANA baz danych na platformie Azure, zobacz sekcję "SAP HANA w usłudze Azure Virtual Machines".

Aby uzyskać informacje na temat wysokiej dostępności obciążeń SAP na platformie Azure, zobacz:

- [Azure Virtual Machines wysoka dostępność dla oprogramowania SAP NetWeaver](./sap-high-availability-guide-start.md)

Ten dokument wskazuje różne inne dokumenty o architekturze i scenariuszach. W późniejszych dokumentach scenariuszy linki do szczegółowych dokumentów technicznych, które wyjaśniają wdrożenie i konfigurację różnych metod wysokiej dostępności są dostępne. Różne dokumenty pokazujące sposób ustanawiania i konfigurowania wysokiej dostępności dla obciążeń SAP NetWeaver obejmują systemy operacyjne Linux i Windows.


Aby uzyskać informacje na temat integracji między usługami Azure Active Directory (Azure AD) i SAP oraz logowaniem jednokrotnym, zobacz:

- [Samouczek: integracja Azure Active Directory z chmurą SAP dla klienta](../../../active-directory/saas-apps/sap-customer-cloud-tutorial.md?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Samouczek: integracja Azure Active Directory z uwierzytelnianiem tożsamości platformy SAP Cloud Platform](../../../active-directory/saas-apps/sap-hana-cloud-platform-identity-authentication-tutorial.md?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Samouczek: integracja Azure Active Directory z platformą SAP w chmurze](../../../active-directory/saas-apps/sap-hana-cloud-platform-tutorial.md?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Samouczek: integracja usługi Azure Active Directory z oprogramowaniem SAP NetWeaver](../../../active-directory/saas-apps/sap-netweaver-tutorial.md?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Samouczek: integracja usługi Azure Active Directory z oprogramowaniem SAP Business ByDesign](../../../active-directory/saas-apps/sapbusinessbydesign-tutorial.md?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Samouczek: integracja Azure Active Directory z SAP HANA](../../../active-directory/saas-apps/saphana-tutorial.md?toc=%2fazure%2fvirtual-machines%2fworkloads%2fsap%2ftoc.json)
- [Środowisko S/4HANA: Fiori programu Launchpad — Logowanie jednokrotne w usłudze Azure AD](https://blogs.sap.com/2017/02/20/your-s4hana-environment-part-7-fiori-launchpad-saml-single-sing-on-with-azure-ad/)

Aby uzyskać informacje na temat integracji usług platformy Azure z składnikami SAP, zobacz:

- [Używanie oprogramowania SAP HANA w programie Power BI Desktop](/power-bi/desktop-sap-hana)
- [Zapytanie bezpośrednie i platforma SAP HANA](/power-bi/desktop-directquery-sap-hana)
- [Używanie łącznika SAP BW Connector w programie Power BI Desktop](/power-bi/desktop-sap-bw-connector) 
- [Usługa Azure Data Factory oferuje integrację danych oprogramowania SAP HANA i Business Warehouse](https://azure.microsoft.com/blog/azure-data-factory-offer-sap-hana-and-business-warehouse-data-integration)


## <a name="change-log"></a>Dziennik zmian

- 07/23/2020: dodano [SAP HANA — duże wystąpienia zapisywania w artykule dotyczącym rezerwacji platformy Azure](../../../cost-management-billing/reservations/prepay-hana-large-instances-reserved-capacity.md) wyjaśniającym, co musisz wiedzieć przed zakupieniem SAP HANA — duże wystąpienia rezerwacji i sposobami zakupu.
- 07/16/2020: opisz, jak używać Azure PowerShell, aby zainstalować nowe rozszerzenie maszyny wirtualnej dla oprogramowania SAP w [przewodniku wdrażania](deployment-guide.md)
- 7/04/2020: wydanie [usługi Azure monitor dla rozwiązań SAP (wersja zapoznawcza)](./azure-monitor-overview.md)
- 07/01/2020: sugerowanie tańszej konfiguracji magazynu w oparciu o funkcjonalność usługi Azure Premium Storage w dokumencie [SAP HANA konfiguracje magazynu maszyn wirtualnych platformy Azure](./hana-vm-operations-storage.md) 
- 06/24/2020: zmiana w [konfigurowaniu Pacemaker na platformie Azure w](./high-availability-guide-suse-pacemaker.md) celu wydania nowego udoskonalonego agenta usługi Azure ogrodzenia i bardziej odpornej konfiguracji STONITH dla urządzeń na podstawie agenta usługi Azure ogrodzenia 
- 06/24/2020: zmiana podczas [konfigurowania Pacemaker na RHEL na platformie Azure](./high-availability-guide-rhel-pacemaker.md) w celu zwolnienia bardziej odpornej konfiguracji STONITH
- 06/23/2020: zmiany dotyczące [planowania i implementacji usługi azure Virtual Machines dla przewodnika dla oprogramowania SAP NetWeaver](./planning-guide.md) i wprowadzenia [typów usługi Azure Storage dla PRZEWODNIKa obciążeń SAP](./planning-guide-storage.md)
- 06/22/2020: Dodaj kroki instalacji dla nowego rozszerzenia maszyny wirtualnej dla SAP do [przewodnika wdrażania](deployment-guide.md)
- 06/16/2020: zmiana [łączności publicznej punktu końcowego dla maszyn wirtualnych przy użyciu usługi Azure Standard ILB w scenariuszach dotyczących oprogramowania SAP ha](./high-availability-guide-standard-load-balancer-outbound-connections.md) aby dodać link do usługi SUSE Public Cloud infrastructure 101 Documentation 
- 06/10/2020: Dodawanie nowych jednostek SKU do [dostępnych jednostek SKU dla](./hana-available-skus.md) [architektury magazynu i SAP HANA (duże wystąpienia)](./hana-storage-architecture.md)
- 05/21/2020: zmiana podczas [konfigurowania Pacemaker na platformie Azure](./high-availability-guide-suse-pacemaker.md) i [KONFIGUROWANIA Pacemaker na RHEL na platformie Azure](./high-availability-guide-rhel-pacemaker.md) w celu dodania linku do [łączności z publicznym punktem końcowym dla maszyn wirtualnych przy użyciu usługi Azure Standard ILB w scenariuszach dotyczących oprogramowania SAP ha](./high-availability-guide-standard-load-balancer-outbound-connections.md)  
- 05/19/2020: Dodaj ważny komunikat, aby nie używać grupy woluminów głównych przy użyciu LVM dla woluminów związanych z [platformą HANA SAP HANA w konfiguracjach magazynu maszyn wirtualnych platformy Azure](./hana-vm-operations-storage.md)
- 05/19/2020: Dodaj nowe obsługiwane systemy operacyjne dla dużej instancji HANA typu II w [zgodnych systemach operacyjnych dla dużych wystąpień Hana](/- azure/virtual-machines/workloads/sap/os-compatibility-matrix-hana-large-instance)
- 05/12/2020: zmiana [łączności publicznej punktu końcowego dla maszyn wirtualnych przy użyciu usługi Azure Standard ILB w SCENARIUSZACH SAP ha](./high-availability-guide-standard-load-balancer-outbound-connections.md) do aktualizowania łączy i dodawania informacji dla konfiguracji zapory innej firmy
- 05/11/2020: Zmień [wysoką dostępność SAP HANA na maszynach wirtualnych platformy Azure w usłudze SLES](./sap-hana-high-availability.md) , aby ustawić zasób lepkość na wartość 0 dla zasobu netcat, co prowadzi do wydajniejszej pracy w trybie failover 
- 05/05/2020: zmiany [dotyczące planowania i implementacji usługi Azure Virtual Machines dla oprogramowania SAP NetWeaver](./planning-guide.md) w celu wyrażenia, że wdrożenia Gen2 są dostępne dla Mv1 rodziny maszyn wirtualnych
- 04/24/2020: zmiany w [SAP HANA skalowanie w poziomie z aktywnym węzłem na maszynach wirtualnych platformy Azure z usługą ANF na SLES](./sap-hana-scale-out-standby-netapp-files-suse.md), w [SAP HANA skalowanie w poziomie z węzłem gotowości](./sap-hana-scale-out-standby-netapp-files-rhel.md)na maszynach wirtualnych platformy Azure z ANF na RHEL, [wysokiej dostępności dla oprogramowania SAP NetWeaver](./high-availability-guide-suse-netapp-files.md) na maszynach wirtualnych platformy Azure na SLES z ANF i [wysoką dostępność dla SAP NetWeaver na maszynach](./high-availability-guide-rhel-netapp-files.md) wirtualnych platformy Azure w RHEL
- 04/22/2020: Zmień [wysoką dostępność SAP HANA na maszynach wirtualnych platformy Azure w usłudze SLES](./sap-hana-high-availability.md) , aby usunąć atrybut meta `is-managed` z instrukcji, ponieważ powoduje to konflikt z umieszczeniem klastra w trybie konserwacji lub z niego
- 04/21/2020: dodano usługę SQL Azure DB jako obsługiwaną w systemie DBMS dla oprogramowania SAP (Hybris) Commerce platform 1811 i nowszych w artykułach [jakie oprogramowanie SAP jest obsługiwane przez wdrożenia platformy Azure](./sap-supported-product-on-azure.md) oraz [Certyfikaty i konfiguracje SAP działające w systemie Microsoft Azure](./sap-certifications.md)
- 04/16/2020: dodano SAP HANA jako obsługiwaną platformę DBMS for SAP (Hybris) dla platformy commerce w artykułach [jakie oprogramowanie SAP jest obsługiwane przez wdrożenia platformy Azure](./sap-supported-product-on-azure.md) oraz [Certyfikaty i konfiguracje SAP działające w systemie Microsoft Azure](./sap-certifications.md)
- 04/13/2020: Popraw dokładne numery wydań oprogramowania SAP ASE w oprogramowaniu [SAP ASE Azure Virtual Machines DBMS wdrożenia dla obciążeń SAP](./dbms_guide_sapase.md)
- 04/07/2020: zmiana konfiguracji [Pacemaker na SLES na platformie Azure](./high-availability-guide-suse-pacemaker.md) w celu wyjaśnienia chmurowego konfigurowania — instrukcje dotyczące platformy Azure
- 04/06/2020: zmiany w [SAP HANA skalowanie w poziomie z węzłem gotowości na maszynach wirtualnych platformy Azure z Azure NetApp Files na SLES](./sap-hana-scale-out-standby-netapp-files-suse.md) i w [SAP HANA skalowanie w poziomie z węzłem gotowości na maszynach wirtualnych platformy Azure z Azure NetApp Files RHEL](./sap-hana-scale-out-standby-netapp-files-rhel.md) , aby usunąć odwołania do NetApp [TR-4435](https://www.netapp.com/us/media/tr-4746.pdf) (zastąpione przez [TR-4746](https://www.netapp.com/us/media/tr-4746.pdf))
- 03/31/2020: zmiana [wysokiej dostępności SAP HANA na maszynach wirtualnych platformy Azure na SLES](./sap-hana-high-availability.md) i [wysokiej dostępności SAP HANA na maszynach wirtualnych platformy Azure w usłudze RHEL](./sap-hana-high-availability-rhel.md) , aby dodać instrukcje dotyczące określania rozmiaru rozłożonego podczas tworzenia woluminów rozłożonych
- 03/27/2020: zmiana [wysokiej dostępności dla oprogramowania SAP NW na maszynach wirtualnych platformy Azure w systemie SLES z ANF dla aplikacji SAP](./high-availability-guide-suse-netapp-files.md) w celu wyrównania opcji instalacji systemu plików do NetApp TR-4746 (Usuń opcję instalacji synchronizacji)
- 03/26/2020: Zmień [wysoką dostępność dla oprogramowania SAP NetWeaver na maszynach wirtualnych platformy Azure w przewodniku dotyczącym usługi SLES o wiele identyfikatorów SID](./high-availability-guide-suse-multi-sid.md) , aby dodać odwołanie do NetApp TR-4746
- 03/26/2020: Zmień [wysoką dostępność dla oprogramowania SAP NetWeaver na maszynach wirtualnych platformy Azure na platformie SLES dla aplikacji SAP](./high-availability-guide-suse.md), [wysokiej dostępności dla oprogramowania SAP NetWeaver na maszynach wirtualnych platformy Azure na platformie SLES z Azure NETAPP Files dla aplikacji SAP](./high-availability-guide-suse-netapp-files.md). [wysoka dostępność dla systemu plików NFS na maszynach wirtualnych platformy Azure w systemie SLES](./high-availability-guide-suse-nfs.md), wysoka dostępność dla [oprogramowania SAP NetWeaver na maszynach wirtualnych platformy Azure w systemie SLES, wysoka](./high-availability-guide-suse-multi-sid.md)dostępność dla oprogramowania SAP NetWeaver na maszynach wirtualnych platformy Azure na RHEL dla [aplikacji SAP i wysokiej](./high-availability-guide-rhel.md) dostępności [dla SAP NetWeaver na maszynach wirtualnych](./high-availability-guide-rhel-netapp-files.md) Azure Azure Load Balancer Azure NetApp Files
- 03/19/2020: główna wersja dokumentu [szybkiego startu: Instalacja ręczna SAP HANA pojedynczego wystąpienia na platformie azure Virtual Machines](./hana-get-started.md) w celu [zainstalowania SAP HANA na platformie Azure Virtual Machines](./hana-get-started.md)
- 03/17/2020: zmiana podczas [konfigurowania Pacemaker SUSE Linux Enterprise Server na platformie Azure w](./high-availability-guide-suse-pacemaker.md) celu usunięcia ustawienia konfiguracji SBD, które nie jest już konieczne
- 03/16/2020: wyjaśnienie scenariusza certyfikacji kolumny w SAP HANA IaaS certyfikowaną platformę, w [której oprogramowanie SAP jest obsługiwane dla wdrożeń platformy Azure](./sap-supported-product-on-azure.md)
- 03/11/2020: zmiana [obciążenia SAP w scenariuszach obsługiwanych przez maszynę wirtualną platformy Azure](./sap-planning-supported-configurations.md) w celu wyjaśnienia wielu baz danych na obsługę wystąpień w systemie DBMS
- 03/11/2020: zmiana na [platformie Azure Virtual Machines planowanie i wdrażanie w przypadku](./planning-guide.md) maszyn wirtualnych o generacji 1 i 2. generacji
- 03/10/2020: Zmień [konfigurację magazynu maszyn wirtualnych platformy Azure na SAP HANA](./hana-vm-operations-storage.md) , aby wyjaśnić rzeczywiste istniejące limity PRZEPŁYWności ANF
- 03/09/2020: Zmień [wysoką dostępność dla oprogramowania SAP NetWeaver na maszynach wirtualnych platformy Azure na SUSE Linux Enterprise Server dla aplikacji SAP](./high-availability-guide-suse.md), [wysokiej dostępności dla oprogramowania SAP NetWeaver na maszynach wirtualnych platformy azure na platformie SUSE Linux Enterprise Server z Azure NETAPP Files dla aplikacji SAP](./high-availability-guide-suse-netapp-files.md). [wysoka dostępność dla systemu plików NFS na maszynach wirtualnych platformy Azure na SUSE Linux Enterprise Server](./high-availability-guide-suse-nfs.md), [Konfigurowanie Pacemaker na SUSE Linux Enterprise Server na platformie Azure](./high-availability-guide-suse-pacemaker.md), [wysokiej dostępności programu IBM Db2 LUW na maszynach wirtualnych platformy Azure na SUSE Linux Enterprise Server z Pacemaker](./dbms-guide-ha-ibm.md), [wysoką dostępność SAP HANA na maszynach wirtualnych platformy Azure na SUSE Linux Enterprise Server](./sap-hana-high-availability.md) i [wysokiej dostępności dla oprogramowania SAP NetWeaver na maszynach wirtualnych platformy Azure w systemie SLES](./high-availability-guide-suse-multi-sid.md) , aby zaktualizować zasoby klastra przy użyciu agenta zasobów Azure-lb 
- 03/05/2020: zmiany struktury i zmiany zawartości dla regionów świadczenia usługi Azure i usługi Azure Virtual Machines na [platformie azure Virtual Machines planowanie i wdrażanie oprogramowania SAP NetWeaver](./planning-guide.md)
- 03/03/2020: zmiana [wysokiej dostępności dla oprogramowania SAP NW na maszynach wirtualnych platformy Azure w systemie SLES z ANF dla aplikacji SAP](./high-availability-guide-suse-netapp-files.md) , aby zmienić bardziej wydajny układ woluminu ANF
- 03/01/2020: Przewodnik dotyczący [tworzenia kopii zapasowych SAP HANA na platformie Azure Virtual Machines](./sap-hana-backup-guide.md) do uwzględnienia usługi Azure Backup. Zmniejszona i wąska zawartość w [SAP HANA Azure Backup na poziomie pliku](./sap-hana-backup-file-level.md) i usunięta trzeci dokument zawierający kopię zapasową za pomocą migawek dysków. Zawartość jest obsługiwana w przewodniku tworzenia kopii zapasowych SAP HANA na platformie Azure Virtual Machines 
- 27 lutego 2020: zmiana [wysokiej dostępności dla oprogramowania SAP NW na maszynach wirtualnych platformy Azure w systemie SLES for SAP](./high-availability-guide-suse.md), [wysoka dostępność dla oprogramowania SAP NW na maszynach wirtualnych platformy Azure na platformie SLES z ANF dla aplikacji SAP](./high-availability-guide-suse-netapp-files.md) i [wysokiej dostępności dla SAP NetWeaver na maszynach wirtualnych platformy Azure w systemie SLES](./high-availability-guide-suse-multi-sid.md) w celu dostosowania parametru klastra "on Fail"
- 26 lutego 2020: Zmień [konfigurację magazynu maszyn wirtualnych platformy Azure na SAP HANA](./hana-vm-operations-storage.md) , aby wyjaśnić wybór systemu plików dla platformy Hana na platformie Azure
- 26 lutego 2020: zmiana [architektury i scenariuszy wysokiej dostępności dla SAP](./sap-high-availability-architecture-scenarios.md) w celu uwzględnienia linku do ha dla SAP NetWeaver na maszynach wirtualnych platformy Azure w przewodniku dotyczącym usługi RHEL z obsługą wiele identyfikatorów SID
- 26 lutego 2020: zmiana [wysokiej dostępności dla oprogramowania SAP NW na maszynach wirtualnych platformy Azure w systemie SLES for SAP](./high-availability-guide-suse.md), [wysoka dostępność dla oprogramowania SAP NW na maszynach wirtualnych platformy Azure na platformie SLES z ANF dla aplikacji SAP](./high-availability-guide-suse-netapp-files.md), [maszyny wirtualne platformy Azure](./high-availability-guide-rhel.md) o wysokiej dostępności dla oprogramowania SAP NetWeaver na RHEL i [maszyn wirtualnych platformy Azure o wysokiej dostępności dla oprogramowania SAP NetWeaver w Azure NetApp Files RHEL](./high-availability-guide-rhel-netapp-files.md)
- 26 lutego 2020: wydanie [wysokiej dostępności dla oprogramowania SAP NetWeaver na maszynach wirtualnych platformy Azure w przewodniku dotyczącym usługi RHEL z obsługą wiele identyfikatorów SID](./high-availability-guide-rhel-multi-sid.md) w celu dodania linku do przewodnika po klastrze z obsługą wiele identyfikatorów SID
- 02/25/2020: zmiana [architektury i scenariuszy wysokiej dostępności dla SAP](./sap-high-availability-architecture-scenarios.md) w celu dodania linków do NOWSZYCH artykułów ha
- 25 lutego 2020: zmiana [wysokiej dostępności programu IBM DB2 LUW na maszynach wirtualnych platformy Azure na SUSE Linux Enterprise Server z Pacemaker](./dbms-guide-ha-ibm.md) , aby wskazać dokument, który opisuje dostęp do publicznego punktu końcowego za pomocą standardowego modułu równoważenia obciążenia platformy Azure
- 21 lutego 2020: Pełna wersja artykułu [SAP ASE platformy Azure Virtual Machines DBMS wdrażanie dla obciążeń SAP](./dbms_guide_sapase.md)
- 21 lutego 2020: Zmień [konfigurację magazynu maszyn wirtualnych platformy Azure na SAP HANA](./hana-vm-operations-storage.md) , aby reprezentować nowe zalecenie w rozmiarze rozłożonym dla/Hana/Data i dodać ustawienie harmonogramu we/wy
- 21 lutego 2020: zmiany w dokumentach w dużych wystąpieniach platformy HANA do reprezentowania nowo certyfikowanych jednostek SKU S224 i S224m
- 21 lutego 2020: zmiana [wysokiej dostępności maszyn wirtualnych platformy Azure dla oprogramowania SAP NetWeaver na RHEL](./high-availability-guide-rhel.md) i [maszyn wirtualnych platformy Azure o wysokiej dostępności dla oprogramowania SAP NetWeaver na RHEL z Azure NetApp Files](./high-availability-guide-rhel-netapp-files.md) aby dostosować ograniczenia klastra dla architektury replikacji serwera (ENSA2)
- 20 lutego 2020: zmiana [wysokiej dostępności dla oprogramowania SAP NetWeaver na maszynach wirtualnych platformy Azure w przewodniku dotyczącym usługi SLES z obsługą wiele identyfikatorów SID](./high-availability-guide-suse-multi-sid.md) w celu dodania linku do przewodnika po klastrze z obsługą wiele identyfikatorów SID
- 13 lutego 2020: zmiany w [usłudze Azure Virtual Machines planowanie i wdrażanie oprogramowania SAP NetWeaver](./planning-guide.md) w celu zaimplementowania linków do nowych dokumentów
- 13 lutego 2020: dodano nowy dokument [obciążenie SAP w scenariuszu obsługiwanym przez maszynę wirtualną platformy Azure](./sap-planning-supported-configurations.md)
- 13 lutego 2020: dodano nowy dokument, [jakie oprogramowanie SAP jest obsługiwane w przypadku wdrażania platformy Azure](./sap-supported-product-on-azure.md)
- 13 lutego 2020: zmiana [wysokiej dostępności programu IBM DB2 LUW na maszynach wirtualnych platformy Azure na serwerze Red Hat Enterprise Linux](./high-availability-guide-rhel-ibm-db2-luw.md) , aby wskazać dokument, który opisuje dostęp do publicznego punktu końcowego za pomocą standardowego modułu równoważenia obciążenia platformy Azure
- 13 lutego 2020: Dodaj nowe typy maszyn wirtualnych do [certyfikatów SAP i konfiguracji uruchomionych w Microsoft Azure](./sap-certifications.md)
- 13 lutego 2020: Dodawanie nowych informacji pomocy technicznej SAP [obciążenia SAP na platformie Azure: Lista kontrolna planowania i wdrażania](./sap-deployment-checklist.md)
- 13 lutego 2020: zmiana na [maszynach wirtualnych platformy Azure o wysokiej dostępności dla oprogramowania SAP NetWeaver na RHEL](./high-availability-guide-rhel.md) i [maszynach wirtualnych platformy Azure o wysokiej dostępności dla oprogramowania SAP NetWeaver na RHEL z Azure NetApp Files](./high-availability-guide-rhel-netapp-files.md) w celu wyrównania limitów czasu zasobów klastra do zaleceń dotyczących limitu czasu Red Hat
- 11 lutego 2020: wydanie [SAP HANA w przypadku migracji dużych wystąpień na platformę Azure do platformy azure Virtual Machines](./hana-large-instance-virtual-machine-migration.md)
- 07 lutego 2020: zmiana [łączności publicznej punktu końcowego dla maszyn wirtualnych przy użyciu usługi Azure Standard ILB w scenariuszach dotyczących oprogramowania SAP ha](./high-availability-guide-standard-load-balancer-outbound-connections.md) do aktualizowania przykładowego ZRZUTu sieciowej grupy zabezpieczeń
- Luty 03 2020: zmiana [wysokiej dostępności dla oprogramowania SAP NW na maszynach wirtualnych platformy Azure w systemie SLES for SAP](./high-availability-guide-suse.md) i [wysokiej dostępności dla oprogramowania SAP NW na maszynach wirtualnych platformy Azure na platformie SLES z ANF for SAP](./high-availability-guide-suse-netapp-files.md) , aby usunąć ostrzeżenie o używaniu łącznika w nazwach hostów węzłów klastra na SLES
- 28 stycznia 2020: zmiana [o wysokiej dostępności SAP HANA na maszynach wirtualnych platformy Azure w systemie RHEL](./sap-hana-high-availability-rhel.md) , aby wyrównać limity czasu SAP HANA zasobów klastra do zaleceń dotyczących limitu czasu Red Hat
- 17 stycznia 2020: zmiana w [grupach umieszczania bliskości platformy Azure w celu uzyskania optymalnego opóźnienia sieci przy użyciu aplikacji SAP](./sap-proximity-placement-scenarios.md) aby zmienić sekcję przenoszenie istniejących maszyn wirtualnych do grupy umieszczania zbliżeniowego
- 17 stycznia 2020: zmiana [konfiguracji obciążenia SAP strefy dostępności platformy Azure](./sap-ha-availability-zones.md) , aby wskazywała na procedurę, która automatyzuje pomiary opóźnienia między strefy dostępności
- 16 stycznia 2020: zmiana w [sposobie instalowania i konfigurowania SAP HANA (duże wystąpienia) na platformie Azure](./hana-installation.md) w celu adaptacji WYDAŃ systemu operacyjnego do katalogu sprzętowego Hana IaaS
- 16 stycznia 2020: zmiany w [wysokiej dostępności dla oprogramowania SAP NetWeaver na maszynach wirtualnych platformy Azure w przewodniku dotyczącym usługi SLES z obsługą wiele identyfikatorów SID](./high-availability-guide-suse-multi-sid.md) , aby dodać instrukcje dla systemów SAP, przy użyciu kolejki Server 2 (ENSA2)
- 10 stycznia 2020: zmiany w [SAP HANA skalowanie w poziomie z węzłem gotowości na maszynach wirtualnych platformy Azure z Azure NetApp Files na SLES](./sap-hana-scale-out-standby-netapp-files-suse.md) i w [SAP HANA skalowanie w poziomie z węzłem gotowości na maszynach wirtualnych platformy Azure z Azure NetApp Files na RHEL](./sap-hana-scale-out-standby-netapp-files-rhel.md) , aby dodać instrukcje dotyczące wprowadzania zmian w sposób `nfs4_disable_idmapping` trwały.
- 10 stycznia 2020: zmiany w [wysokiej dostępności dla oprogramowania SAP NetWeaver na maszynach wirtualnych platformy Azure w usłudze SLES z Azure NetApp Files dla aplikacji SAP](./high-availability-guide-suse-netapp-files.md) oraz na [platformie Azure Virtual Machines wysokiej dostępności dla oprogramowania SAP NetWeaver na RHEL przy użyciu Azure NetApp files for SAP Applications](./high-availability-guide-rhel-netapp-files.md) , aby dodać instrukcje dotyczące instalowania Azure NetApp Files woluminów NFSv4.
- 23 grudnia 2019: wydanie [wysokiej dostępności dla oprogramowania SAP NetWeaver na maszynach wirtualnych platformy Azure w przewodniku dotyczącym usługi SLES o wiele identyfikatorów SID](./high-availability-guide-suse-multi-sid.md)
- 18 grudnia 2019: wersja [SAP HANA skalowanie w poziomie z węzłem gotowości na maszynach wirtualnych platformy Azure z Azure NetApp Files na RHEL](./sap-hana-scale-out-standby-netapp-files-rhel.md)
- 21 listopada 2019: zmiany w [SAP HANA skalowanie w poziomie za pomocą węzła wstrzymywania na maszynach wirtualnych platformy Azure z Azure NetApp Files na SUSE Linux Enterprise Server](./sap-hana-scale-out-standby-netapp-files-suse.md) , aby uprościć konfigurację mapowania identyfikatorów systemu plików NFS i zmienić zalecany podstawowy interfejs sieciowy, aby uprościć Routing.
- 15 listopada 2019: drobne zmiany [dotyczące wysokiej dostępności dla oprogramowania SAP NetWeaver na SUSE Linux Enterprise Server z Azure NetApp Files dla aplikacji SAP](high-availability-guide-suse-netapp-files.md) i [wysokiej dostępności dla oprogramowania sap NetWeaver na Red Hat Enterprise Linux z Azure NetApp files for SAP Applications](high-availability-guide-rhel-netapp-files.md) w celu wyjaśnienia ograniczeń rozmiaru puli pojemności i usunięcia instrukcji, które obsługują tylko wersję NFSv3.
- 12 listopada 2019: wydanie [wysokiej dostępności dla SAP NetWeaver w systemie Windows z Azure NetApp Files (SMB)](high-availability-guide-windows-netapp-files-smb.md)
- 8 listopada 2019: zmiany w [wysokiej dostępności SAP HANA na maszynach wirtualnych platformy Azure na SUSE Linux Enterprise Server](sap-hana-high-availability.md), [konfigurowanie replikacji systemu SAP HANA na maszynach wirtualnych platformy Azure.](sap-hana-high-availability-rhel.md) [platforma Azure Virtual Machines wysoka dostępność dla oprogramowania SAP NETWEAVER na SUSE Linux Enterprise Server dla aplikacji SAP](high-availability-guide-suse.md), [Azure Virtual Machines wysoka dostępność dla oprogramowania SAP NetWeaver na SUSE Linux Enterprise Server z Azure NetApp Files](high-availability-guide-suse-netapp-files.md), [platforma azure Virtual Machines wysoką dostępność dla oprogramowania SAP NetWeaver na Red Hat Enterprise Linux](high-availability-guide-rhel.md), [platforma Azure Virtual Machines wysoką dostępność dla oprogramowania SAP NetWeaver na](high-availability-guide-rhel-netapp-files.md)Red Hat Enterprise Linux Azure NetApp Files, [wysoka dostępność dla systemu plików NFS na maszynach wirtualnych](high-availability-guide-suse-nfs.md)platformy Azure na SUSE Linux Enterprise Server, [GlusterFS na maszynach wirtualnych](high-availability-guide-rhel-glusterfs.md) platformy Azure na Red Hat Enterprise Linux  
- 8 listopada 2019: zmiany dotyczące [planowania obciążeń SAP i listy kontrolnej wdrożenia](sap-deployment-checklist.md) w celu wyjaśnienia zalecenia dotyczące szyfrowania  
- 4 listopada 2019: zmiany [SUSE Linux Enterprise Server dotyczące konfigurowania Pacemaker na platformie Azure](high-availability-guide-suse-pacemaker.md) w celu utworzenia klastra bezpośrednio z konfiguracją emisji pojedynczej  
