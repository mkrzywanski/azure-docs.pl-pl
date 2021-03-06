---
title: Plik dyrektywy include
description: Plik dyrektywy include
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: include
ms.date: 07/08/2020
ms.author: raynew
ms.custom: include file
ms.openlocfilehash: 60012f79c3c04a4ff14c4a7f0609b6940d3402c4
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/20/2020
ms.locfileid: "86544448"
---
**Wymagania dotyczące konfiguracji i serwera przetwarzania**


## <a name="hardware-requirements"></a>Wymagania sprzętowe

**Składnik** | **Wymaganie** 
--- | ---
Rdzenie procesora CPU | 8 
Pamięć RAM | 16 GB
Liczba dysków | 3, w tym dysk systemu operacyjnego, dysk pamięci podręcznej serwera przetwarzania i dysk przechowywania na potrzeby powrotu po awarii 
Wolne miejsce na dysku (pamięć podręczna serwera przetwarzania) | 600 GB
Wolne miejsce na dysku (dysk przechowywania) | 600 GB
 | 

## <a name="software-requirements"></a>Wymagania dotyczące oprogramowania

**Składnik** | **Wymaganie** 
--- | ---
System operacyjny | Windows Server 2012 z dodatkiem R2 <br> Windows Server 2016
Ustawienia regionalne systemu operacyjnego | Angielski (EN-*)
Role systemu Windows Server | Nie należy włączać tych ról: <br> - Active Directory Domain Services <br>- Internet Information Services <br> - Hyper-V 
Zasady grupy | Nie włączaj tych zasad grupy: <br> -Zapobiegaj dostępowi do wiersza polecenia. <br> — Uniemożliwia dostęp do narzędzi do edytowania rejestru. <br> — Logika zaufania dla plików załączników. <br> — Włącz wykonywanie skryptu. <br> [Dowiedz się więcej](https://technet.microsoft.com/library/gg176671(v=ws.10).aspx)
IIS | -Brak istniejącej domyślnej witryny sieci Web <br> — Żadna istniejąca witryna sieci Web/aplikacja nasłuchu na porcie 443 <br>-Włącz [uwierzytelnianie anonimowe](https://technet.microsoft.com/library/cc731244(v=ws.10).aspx) <br> -Włącz ustawienie [FastCGI](https://technet.microsoft.com/library/cc753077(v=ws.10).aspx) 
FIPS (Federal Information Processing Standards) | Nie włączaj trybu FIPS
|

## <a name="network-requirements"></a>Wymagania dotyczące sieci

**Składnik** | **Wymaganie** 
--- | --- 
Typ adresu IP | Statyczny 
Porty | 443 (organizowanie kanału sterowania)<br>9443 (transport danych) 
Typ karty sieciowej | VMXNET3 (Jeśli serwer konfiguracji jest maszyną wirtualną VMware)
 |
**Dostęp do Internetu** (serwer musi mieć dostęp do następujących adresów URL, bezpośrednio lub za pośrednictwem serwera proxy):|
\*.backup.windowsazure.com | Używany do transferu i koordynacji replikowanych danych
\*.blob.core.windows.net | Służy do uzyskiwania dostępu do konta magazynu przechowującego zreplikowane dane. Możesz podać konkretny adres URL konta magazynu pamięci podręcznej.
\*.hypervrecoverymanager.windowsazure.com | Używany do operacji zarządzania replikacją i koordynacji
https:\//login.microsoftonline.com | Używany do operacji zarządzania replikacją i koordynacji 
time.nist.gov | Służy do sprawdzania synchronizacji czasu między systemem i czasem globalnym
time.windows.com | Służy do sprawdzania synchronizacji czasu między systemem i czasem globalnym
| <ul> <li> https:\//management.azure.com </li><li> https:\//secure.aadcdn.microsoftonline-p.com </li><li> https: \/ /login.Live.com </li><li> https: \/ /Graph.Windows.NET </li><li> https:\//login.windows.net </li><li> *. services.visualstudio.com (opcjonalnie) </li><li> https: \/ /www.Live.com </li><li> https: \/ /www.Microsoft.com </li></ul> | Instalator OVF potrzebuje dostępu do tych dodatkowych adresów URL. Są one używane do kontroli dostępu i zarządzania tożsamościami przez Azure Active Directory.
https: \/ /dev.mysql.com/get/Downloads/MySQLInstaller/mysql-installer-community-5.7.20.0.msi  | Aby ukończyć pobieranie bazy danych MySQL. </br> W kilku regionach pobieranie może zostać przekierowane na adres URL sieci CDN. Upewnij się, że adres URL usługi CDN jest również listy dozwolonych, w razie potrzeby.
|

> [!NOTE]
> Jeśli masz [połączenia prywatne](../articles/site-recovery/hybrid-how-to-enable-replication-private-endpoints.md) z magazynem Site Recovery, nie potrzebujesz dodatkowego dostępu do Internetu dla serwera konfiguracji. Wyjątkiem jest to, że podczas konfigurowania maszyny CS przy użyciu szablonu komórki jajowe będziesz potrzebować dostępu do następujących adresów URL za pośrednictwem i powyżej prywatnego linku dostępu — https://management.azure.com https://www.live.com i https://www.microsoft.com . Jeśli nie chcesz zezwalać na dostęp do tych adresów URL, skonfiguruj CS za pomocą ujednoliconego Instalatora.

## <a name="required-software"></a>Wymagane oprogramowanie

**Składnik** | **Wymaganie** 
--- | ---
VMware vSphere PowerCLI | Należy zainstalować [PowerCLI w wersji 6,0](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1) , jeśli serwer konfiguracji jest uruchomiony na maszynie wirtualnej VMware.
MYSQL | Należy zainstalować MySQL. Można zainstalować go ręcznie lub Site Recovery może go zainstalować. (Aby uzyskać więcej informacji, zobacz [Konfigurowanie ustawień](../articles/site-recovery/vmware-azure-deploy-configuration-server.md#configure-settings) ).
|

## <a name="sizing-and-capacity-requirements"></a>Wymagania dotyczące wielkości i pojemności

Poniższa tabela zawiera podsumowanie wymagań dotyczących pojemności dla serwera konfiguracji. W przypadku replikowania wielu maszyn wirtualnych VMware zapoznaj się z [zagadnieniami dotyczącymi planowania pojemności](../articles/site-recovery/site-recovery-plan-capacity-vmware.md) i uruchom [Narzędzie planista wdrażania usługi Azure Site Recovery](../articles/site-recovery/site-recovery-deployment-planner.md).


**Procesor CPU** | **Pamięć** | **Dysk pamięci podręcznej** | **Szybkość zmian danych** | **Zreplikowane maszyny**
--- | --- | --- | --- | ---
8 procesorów wirtualnych vCPU<br/><br/> 2 gniazda * 4 rdzenie \@ 2,5 GHz | 16 GB | 300 GB | 500 GB lub mniej | Maszyny < 100
12 procesorów wirtualnych vCPU<br/><br/> 2 SOCKS * 6 rdzeni \@ 2,5 GHz | 18 GB | 600 GB | 500 GB — 1 TB | 100 do 150 maszyn
16 procesorów wirtualnych vCPU<br/><br/> 2 SOCKS * 8 rdzeni \@ 2,5 GHz | 32 GB | 1 TB | 1-2 TB | 150 – 200 maszyn
|

