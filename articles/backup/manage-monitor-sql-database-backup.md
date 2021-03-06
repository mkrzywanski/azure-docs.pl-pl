---
title: Monitorowanie SQL Server baz danych na maszynie wirtualnej platformy Azure i zarządzanie nimi
description: W tym artykule opisano sposób zarządzania i monitorowania SQL Server baz danych, które są uruchomione na maszynie wirtualnej platformy Azure.
ms.topic: conceptual
ms.date: 09/11/2019
ms.openlocfilehash: 14e3a4797fe60a3d1857f1e6d947fa0c669bdcfe
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "81537308"
---
# <a name="manage-and-monitor-backed-up-sql-server-databases"></a>Zarządzanie kopiami zapasowymi baz danych programu SQL Server i ich monitorowanie

W tym artykule opisano typowe zadania związane z zarządzaniem i monitorowaniem SQL Server baz danych, które są uruchomione na maszynie wirtualnej platformy Azure (VM) i których kopia zapasowa jest wykonywana w magazynie Recovery Services Azure Backup przez usługę [Azure Backup](backup-overview.md) . Dowiesz się, jak monitorować zadania i alerty, zatrzymywać i wznawiać ochronę bazy danych, uruchamiać zadania tworzenia kopii zapasowych i wyrejestrować maszynę wirtualną z kopii zapasowych.

Jeśli kopie zapasowe baz danych SQL Server nie zostały jeszcze skonfigurowane, zobacz [Tworzenie kopii zapasowych SQL Server baz danych na maszynach wirtualnych platformy Azure](backup-azure-sql-database.md)

## <a name="monitor-manual-backup-jobs-in-the-portal"></a>Monitorowanie zadań ręcznego tworzenia kopii zapasowej w portalu

Azure Backup przedstawia wszystkie zadania wyzwalane ręcznie w portalu **zadania tworzenia kopii zapasowej** . Zadania widoczne w tym portalu obejmują odnajdywanie i rejestrowanie bazy danych oraz operacje tworzenia kopii zapasowych i przywracania.

![Portal zadań tworzenia kopii zapasowej](./media/backup-azure-sql-database/jobs-list.png)

> [!NOTE]
> W portalu **zadań tworzenia kopii zapasowej** nie są wyświetlane zaplanowane zadania tworzenia kopii zapasowej. Do monitorowania zaplanowanych zadań kopii zapasowych należy użyć programu SQL Server Management Studio zgodnie z opisem w następnej sekcji.
>

Aby uzyskać szczegółowe informacje na temat scenariuszy monitorowania, przejdź do [obszaru monitorowanie w Azure Portal](backup-azure-monitoring-built-in-monitor.md) i [monitorowanie przy użyciu Azure monitor](backup-azure-monitoring-use-azuremonitor.md).  

## <a name="view-backup-alerts"></a>Wyświetlanie alertów dotyczących kopii zapasowych

Ponieważ kopie zapasowe dzienników są wykonywane co 15 minut, monitorowanie zadań tworzenia kopii zapasowej może być żmudnym. Azure Backup łatwość monitorowania przez wysyłanie alertów e-mail. Alerty e-mail są następujące:

- Wyzwolone dla wszystkich błędów tworzenia kopii zapasowej.
- Skonsolidowane na poziomie bazy danych według kodu błędu.
- Wysyłane tylko do pierwszego błędu kopii zapasowej bazy danych.

Aby monitorować alerty kopii zapasowej bazy danych:

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).

2. Na pulpicie nawigacyjnym magazynu wybierz pozycję **alerty i zdarzenia**.

   ![Wybieranie pozycji Alerty i zdarzenia](./media/backup-azure-sql-database/vault-menu-alerts-events.png)

3. W obszarze **alerty i zdarzenia**wybierz pozycję **alerty kopii zapasowych**.

   ![Wybieranie pozycji Alerty kopii zapasowej](./media/backup-azure-sql-database/backup-alerts-dashboard.png)

## <a name="stop-protection-for-a-sql-server-database"></a>Zatrzymywanie ochrony bazy danych programu SQL Server

Można zatrzymać tworzenie kopii zapasowej bazy danych SQL Server na kilka sposobów:

- Zatrzymaj wszystkie przyszłe zadania tworzenia kopii zapasowej i Usuń wszystkie punkty odzyskiwania.
- Zatrzymaj wszystkie przyszłe zadania tworzenia kopii zapasowej i pozostaw punkty odzyskiwania bez zmian.

Jeśli zdecydujesz się na pozostawienie punktów odzyskiwania, pamiętaj o następujących szczegółach:

- Wszystkie punkty odzyskiwania pozostaną nienaruszone, a całe oczyszczanie zatrzyma się przy zatrzymaniu ochrony z zachowaniem danych.
- Zostanie naliczona opłata za chronione wystąpienie i zużyty magazyn. Aby uzyskać więcej informacji, zobacz [Cennik usługi Azure Backup](https://azure.microsoft.com/pricing/details/backup/).
- Jeśli usuniesz źródło danych bez zatrzymywania kopii zapasowych, nowe kopie zapasowe zakończą się niepowodzeniem. Stare punkty odzyskiwania wygasną zgodnie z zasadami, ale jeden ostatni punkt odzyskiwania będzie zawsze utrzymywany do momentu zatrzymania wykonywania kopii zapasowych i usunięcia danych.

Aby zatrzymać ochronę bazy danych:

1. Na pulpicie nawigacyjnym magazynu wybierz pozycję **elementy kopii zapasowej**.

2. W obszarze **Typ zarządzania kopiami zapasowymi**wybierz pozycję **SQL na maszynie wirtualnej platformy Azure**.

    ![Wybieranie pozycji SQL na maszynie wirtualnej platformy Azure](./media/backup-azure-sql-database/sql-restore-backup-items.png)

3. Wybierz bazę danych, dla której chcesz zatrzymać ochronę.

    ![Wybieranie bazy danych w celu zatrzymania ochrony](./media/backup-azure-sql-database/sql-restore-sql-in-vm.png)

4. W menu Baza danych wybierz pozycję **Zatrzymaj tworzenie kopii zapasowej**.

    ![Wybieranie pozycji Zatrzymaj tworzenie kopii zapasowej](./media/backup-azure-sql-database/stop-db-button.png)

5. W menu **Zatrzymaj kopię zapasową** wybierz, czy chcesz zachować, czy usunąć dane. Jeśli chcesz, podaj przyczynę i komentarz.

    ![Zachowaj lub Usuń dane z menu Zatrzymaj kopię zapasową](./media/backup-azure-sql-database/stop-backup-button.png)

6. Wybierz pozycję **Zatrzymaj tworzenie kopii zapasowej**.

> [!NOTE]
>
>Aby uzyskać więcej informacji na temat opcji Usuń dane, zobacz często zadawane pytania poniżej:
>
>- [Czy po usunięciu bazy danych z wystąpienia z ochroną chronioną będą wykonywane kopie zapasowe?](faq-backup-sql-server.md#if-i-delete-a-database-from-an-autoprotected-instance-what-will-happen-to-the-backups)
>- [Czy w przypadku zatrzymania operacji tworzenia kopii zapasowej bazy danych z ochroną chronioną jej zachowaniem?](faq-backup-sql-server.md#if-i-change-the-name-of-the-database-after-it-has-been-protected-what-will-be-the-behavior)
>
>

## <a name="resume-protection-for-a-sql-database"></a>Wznawianie ochrony bazy danych SQL

Po zatrzymaniu ochrony bazy danych SQL w przypadku wybrania opcji **Zachowaj dane kopii zapasowej** można później wznowić ochronę. Jeśli nie zachowasz danych kopii zapasowej, nie możesz wznowić ochrony.

Aby wznowić ochronę bazy danych SQL:

1. Otwórz element kopii zapasowej i wybierz pozycję **Wznów wykonywanie kopii zapasowej**.

    ![Wybieranie pozycji Wznów tworzenie kopii zapasowej w celu wznowienia ochrony bazy danych](./media/backup-azure-sql-database/resume-backup-button.png)

2. W menu **Zasady kopii zapasowych** wybierz zasady, a następnie wybierz pozycję **Zapisz**.

## <a name="run-an-on-demand-backup"></a>Uruchamianie kopii zapasowej na żądanie

Można uruchamiać różne typy kopii zapasowych na żądanie:

- Pełna kopia zapasowa
- Pełna kopia zapasowa tylko do kopiowania
- Różnicowa kopia zapasowa
- Kopia zapasowa dziennika

Podczas gdy trzeba określić czas przechowywania dla pełnej kopii zapasowej, zakres przechowywania dla pełnej kopii zapasowej na żądanie zostanie automatycznie ustawiony na 45 dni od bieżącego czasu.

Aby uzyskać więcej informacji, zobacz [SQL Server typów kopii zapasowych](backup-architecture.md#sql-server-backup-types).

## <a name="unregister-a-sql-server-instance"></a>Wyrejestrowywanie wystąpienia programu SQL Server

Wyrejestruj wystąpienie SQL Server po wyłączeniu ochrony, ale przed usunięciem magazynu:

1. Na pulpicie nawigacyjnym magazynu w obszarze **Zarządzaj**wybierz pozycję **infrastruktura kopii zapasowych**.  

   ![Wybieranie pozycji Infrastruktura zapasowa](./media/backup-azure-sql-database/backup-infrastructure-button.png)

2. W obszarze **Serwery zarządzania** wybierz pozycję **Serwery chronione**.

   ![Wybieranie pozycji Serwery chronione](./media/backup-azure-sql-database/protected-servers.png)

3. W obszarze **serwery chronione**wybierz serwer do wyrejestrowania. Aby usunąć magazyn, musisz wyrejestrować wszystkie serwery.

4. Kliknij prawym przyciskiem myszy serwer chroniony, a następnie wybierz polecenie **Wyrejestruj**.

   ![Wybieranie pozycji Usuń](./media/backup-azure-sql-database/delete-protected-server.jpg)

## <a name="modify-policy"></a>Modyfikuj zasady

Zmodyfikuj zasady, aby zmienić częstotliwość tworzenia kopii zapasowych lub zakres przechowywania.

> [!NOTE]
> Każda zmiana w okresie przechowywania będzie stosowana z mocą wsteczną do wszystkich starszych punktów odzyskiwania poza nowymi.

Na pulpicie nawigacyjnym magazynu przejdź do pozycji **Zarządzaj**  >  **zasadami tworzenia kopii zapasowych** i wybierz zasady, które chcesz edytować.

  ![Zarządzanie zasadami tworzenia kopii zapasowych](./media/backup-azure-sql-database/modify-backup-policy.png)

  ![Modyfikowanie zasad tworzenia kopii zapasowych](./media/backup-azure-sql-database/modify-backup-policy-impact.png)

Modyfikacje zasad będą mieć wpływ na wszystkie powiązane elementy kopii zapasowej i wyzwalają odpowiednie zadania **konfigurowania ochrony** .

### <a name="inconsistent-policy"></a>Niespójne zasady

Czasami operacja modyfikowania zasad może prowadzić do **niespójnej** wersji zasad dla niektórych elementów kopii zapasowej. Dzieje się tak, gdy odpowiednie zadanie **konfigurowania ochrony** nie powiedzie się dla elementu kopii zapasowej po wyzwoleniu operacji modyfikowania zasad. Jest on wyświetlany w następujący sposób w widoku elementu kopii zapasowej:

  ![Niespójne zasady](./media/backup-azure-sql-database/inconsistent-policy.png)

Możesz naprawić wersję zasad dla wszystkich elementów, których dotyczy problem, w jednym kliknięciem:

  ![Napraw niespójne zasady](./media/backup-azure-sql-database/fix-inconsistent-policy.png)

## <a name="re-register-extension-on-the-sql-server-vm"></a>Ponowne rejestrowanie rozszerzenia na maszynie wirtualnej SQL Server

Czasami może to mieć wpływ na rozszerzenie obciążenia maszyny wirtualnej z jednego powodu. W takich przypadkach wszystkie operacje wyzwalane na maszynie wirtualnej rozpoczną się niepowodzeniem. Może być konieczne ponowne zarejestrowanie rozszerzenia na maszynie wirtualnej. Operacja **ponownego rejestrowania ponownie** zainstaluje rozszerzenie kopii zapasowej obciążenia na maszynie wirtualnej w celu kontynuowania operacji.

Użyj tej opcji z zachowaniem ostrożności; Ta operacja jest wyzwalana na maszynie wirtualnej z już prawidłowym rozszerzeniem. spowoduje to ponowne uruchomienie rozszerzenia. Może to spowodować niepowodzenie wszystkich zadań w toku. Przed wyzwoleniem operacji ponownego rejestrowania należy również sprawdzić obecność jednego lub kilku [objawów](backup-sql-server-azure-troubleshoot.md#re-registration-failures) .

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji, zobacz [Rozwiązywanie problemów z kopiami zapasowymi w bazie danych SQL Server](backup-sql-server-azure-troubleshoot.md).
