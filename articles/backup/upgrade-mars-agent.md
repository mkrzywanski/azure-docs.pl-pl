---
title: Uaktualnij agenta Microsoft Azure Recovery Services (MARS)
description: Dowiedz się, jak uaktualnić agenta Microsoft Azure Recovery Services (MARS).
ms.topic: conceptual
ms.date: 03/03/2020
ms.openlocfilehash: 49daf438b855d19961519d93b6c3ec535de4756f
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "78672897"
---
# <a name="upgrade-the-microsoft-azure-recovery-services-mars-agent"></a>Uaktualnij agenta Microsoft Azure Recovery Services (MARS)

W tym artykule dowiesz się, jak:

* Identyfikowanie serwerów ze starszymi wersjami agenta MARS
* Aktualizowanie instalacji usług MARS na tych serwerach

## <a name="identify-servers-with-earlier-versions-of-the-mars-agent"></a>Identyfikowanie serwerów ze starszymi wersjami agenta MARS

W przypadku instalacji agenta Azure Backup i usługi Azure Backup Server:

1. Przejdź do magazynu Recovery Services, w którym zarejestrowano serwery, których kopie zapasowe mogą być tworzone przez starsze wersje agenta. Można znaleźć reprezentatywną listę magazynów ze starszymi Azure Backup agentami w Azure Backup alertach aktualizacji z platformy Azure.
1. W sekcji **Ustawienia** po lewej stronie magazynu Recovery Services wybierz pozycję **infrastruktura kopii zapasowych** w sekcji **Zarządzanie** .
1. Aby odnaleźć agentów usługi Azure Backup zainstalowanych w ramach instalacji serwera Azure Backup, przejdź do lokalizacji **serwery zarządzania kopiami zapasowymi** w obszarze **serwery zarządzania**. Spowoduje to wyświetlenie listy serwerów z instalacjami Azure Backup Server wraz z numerem wersji skojarzonego agenta Azure Backup.

    ![Lista agentów MARS zainstalowanych w ramach instalacji serwera Azure Backup](./media/upgrade-mars-agent/backup-management-servers.png)

1. Aby sprawdzić wersje agentów dla instalacji agenta usługi Microsoft Azure Recovery Services (MARS) lub agenta Azure Backup, przejdź do obszaru **chronione serwery** w obszarze **serwery zarządzania**. Następnie wybierz pozycję **Azure Backup Agent** w obszarze Typ zarządzania kopiami zapasowymi. Spowoduje to wyświetlenie listy serwerów z zainstalowanymi agentami Azure Backup wraz z numerem wersji instalacji.

    ![Lista serwerów z zainstalowanym agentem MARS](./media/upgrade-mars-agent/protected-servers.png)

1. Posortuj Azure Backup kolumnie wersja agenta, klikając kolumnę **wersja agenta** dla instalacji agenta Mars lub kolumnę **wersji agenta Azure Backup** na potrzeby instalacji serwera usługi Azure Backup.

1. Poprzedni krok umożliwi wyświetlenie listy serwerów z Azure Backup agentami, które mają wersje starsze niż 2.0.9083.0 lub wersje agentów wymienione jako puste. Są to serwery, na których należy zaktualizować agentów Azure Backup.

## <a name="update-the-mars-agent-installation-on-the-server"></a>Aktualizowanie instalacji agenta MARS na serwerze

Po zidentyfikowaniu serwerów, które wymagają aktualizacji agenta Azure Backup, należy wykonać następujące kroki dla każdego określonego serwera (przy użyciu usługi Azure Backup Server lub agenta MARS). [Pobierz najnowszą wersję agenta usługi Azure Backup](https://aka.ms/azurebackup_agent) przed wykonaniem poniższych kroków.

1. Kliknij wiersz, który ma Azure Backup Agent jest mniejszy niż 2.0.9083.0 lub pusty. Spowoduje to otwarcie ekranu Szczegóły serwera.

    ![Chronione serwery z nieaktualnymi wersjami agentów](./media/upgrade-mars-agent/old-agent-version.png)

    ![Serwery usługi Azure Backup z nieaktualnymi wersjami agentów](./media/upgrade-mars-agent/backup-management-servers-old-versions.png)

1. Kliknij przycisk **Połącz** , aby odebrać plik połączenia pulpitu zdalnego, aby nawiązać połączenie z serwerem lub połączyć się bezpośrednio z serwerem za pośrednictwem połączenia pulpit zdalny na serwerze.

    ![Nawiązywanie połączenia z serwerem za pośrednictwem usługi Podłączanie pulpitu zdalnego](./media/upgrade-mars-agent/connect-to-server.png)

    >[!NOTE]
    > Jeśli wymieniony serwer nie istnieje lub został zlikwidowany, można zignorować pozostałe kroki poniżej i przejść do następnego serwera.

1. Wprowadź szczegóły logowania administracyjnego i zaloguj się.

1. Jeśli serwer proxy serwera lub serwera ma ograniczony dostęp do Internetu, upewnij się, że ustawienia zapory na serwerze/proxy są skonfigurowane tak, aby zezwalały na adres URL odpowiedni dla używanej chmury platformy Azure:

    Chmura platformy Azure | Adres URL
    -- | ---
    Azure Cloud (Publiczny) |   `https://login.windows.net`
    Azure Chiny — chmura 21Vianet   | `https://login.chinacloudapi.cn`
    Chmura platformy Azure dla instytucji rządowych USA |   `https://login.microsoftonline.us`
    Chmura w języku niemieckim platformy Azure  |  `https://login.microsoftonline.de`

1. Skopiuj do serwera Instalatora aktualizacji agenta usługi Azure Backup.

    ![Kopiuj Instalatora aktualizacji agenta usługi Azure Backup do serwera](./media/upgrade-mars-agent/copy-agent-installer.png)

1. Uruchom instalatora. Zostanie otwarty Kreator uaktualnienia agenta Microsoft Azure Recovery Services.

    ![Kreator uaktualnienia agenta Microsoft Azure Recovery Services](./media/upgrade-mars-agent/agent-upgrade-wizard.png)

1. Kliknij przycisk **Dalej**.

1. Kliknij przycisk **Uaktualnij**.

    ![Instalacja agenta Microsoft Azure Recovery Services](./media/upgrade-mars-agent/upgrade-installation.png)

1. Końcowy ekran potwierdzający wskazuje, że Agent Azure Backup został pomyślnie zaktualizowany.

## <a name="for-system-center-data-protection-manager-sc-dpm-customers"></a>Dla klientów programu System Center Data Protection Manager (SC DPM)

Jeśli zainstalowano agentów usługi Azure Backup na serwerach programu System Center Data Protection Manager (SC DPM), należy wykonać poniższe kroki, aby określić, czy serwery programu DPM muszą mieć aktualizację agenta usługi Azure Backup:

1. Zaloguj się do serwera SC DPM jako administrator.
2. Otwórz konsolę programu DPM.
3. Kliknij pozycję **Zarządzanie** w lewym dolnym rogu konsoli programu.
4. W informacjach wyświetlanych na lewym pasku nawigacyjnym Poszukaj informacji o wersji agenta Azure Backup.
5. Jeśli wersja jest starsza niż 2.0.9083.0, Pobierz najnowszą wersję Instalatora agenta usługi Azure Backup i uruchom Instalatora na serwerze programu DPM w celu zaktualizowania agenta Azure Backup.

Powtórz powyższe kroki dla wszystkich serwerów programu DPM w danym środowisku.

## <a name="next-steps"></a>Następne kroki

Informacje na temat [tworzenia kopii zapasowych maszyn z systemem Windows przy użyciu agenta Azure Backup Mars](backup-windows-with-mars-agent.md)
