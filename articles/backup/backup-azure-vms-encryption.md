---
title: Tworzenie kopii zapasowych i Przywracanie zaszyfrowanych maszyn wirtualnych platformy Azure
description: Opisuje sposób tworzenia kopii zapasowych i przywracania zaszyfrowanych maszyn wirtualnych platformy Azure przy użyciu usługi Azure Backup.
ms.topic: conceptual
ms.date: 07/29/2020
ms.openlocfilehash: 25c5e66bde817e824a307df2a2b1b5f76c773c01
ms.sourcegitcommit: 42107c62f721da8550621a4651b3ef6c68704cd3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/29/2020
ms.locfileid: "87405767"
---
# <a name="back-up-and-restore-encrypted-azure-vm"></a>Tworzenie kopii zapasowej i przywracanie zaszyfrowanej maszyny wirtualnej platformy Azure

W tym artykule opisano sposób tworzenia kopii zapasowych i przywracania maszyn wirtualnych systemu Windows lub Linux z szyfrowanymi dyskami przy użyciu usługi [Azure Backup](backup-overview.md) .

Jeśli chcesz dowiedzieć się więcej o tym, jak Azure Backup współdziała z maszynami wirtualnymi platformy Azure przed rozpoczęciem, zapoznaj się z następującymi zasobami:

- [Przejrzyj](backup-architecture.md#architecture-built-in-azure-vm-backup) architekturę kopii zapasowych maszyny wirtualnej platformy Azure.
- [Dowiedz się więcej o](backup-azure-vms-introduction.md) Kopia zapasowa maszyny wirtualnej platformy Azure i rozszerzenie Azure Backup.

## <a name="encryption-support"></a>Obsługa szyfrowania

Azure Backup obsługuje tworzenie kopii zapasowych maszyn wirtualnych platformy Azure, w których dyski systemu operacyjnego/danych są szyfrowane za pomocą Azure Disk Encryption (ADE). ADE używa funkcji BitLocker do szyfrowania maszyn wirtualnych z systemem Windows oraz funkcji dm-crypt dla maszyn wirtualnych z systemem Linux. Program ADE integruje się z Azure Key Vault w celu zarządzania kluczami i wpisami tajnymi dysków. Klucze szyfrowania klucza Key Vault (KEKs) mogą służyć do dodawania dodatkowej warstwy zabezpieczeń, szyfrowania wpisów tajnych szyfrowania przed zapisaniem ich w Key Vault.

Azure Backup można tworzyć kopie zapasowe maszyn wirtualnych platformy Azure i przywracać je za pomocą aplikacji w usłudze Azure AD, co zostało podsumowane w poniższej tabeli.

**Typ dysku maszyny wirtualnej** | **ADE (klucz szyfrowania bloków/dm-crypt)** | **ADE i KEK**
--- | --- | ---
**Niezarządzany** | Tak | Tak
**Zarządzany**  | Tak | Tak

- Dowiedz się więcej na temat [ADE](../security/fundamentals/azure-disk-encryption-vms-vmss.md), [Key Vault](../key-vault/general/overview.md)i [KEKs](../virtual-machine-scale-sets/disk-encryption-key-vault.md#set-up-a-key-encryption-key-kek).
- Przeczytaj [często zadawane pytania](../security/fundamentals/azure-disk-encryption-vms-vmss.md) dotyczące szyfrowania dysków maszyn wirtualnych platformy Azure.

### <a name="limitations"></a>Ograniczenia

- Możesz tworzyć kopie zapasowe zaszyfrowanych maszyn wirtualnych i przywracać je w ramach tej samej subskrypcji i regionu.
- Azure Backup obsługuje maszyny wirtualne zaszyfrowane przy użyciu kluczy autonomicznych. Każdy klucz, który jest częścią certyfikatu używanego do szyfrowania maszyny wirtualnej, nie jest obecnie obsługiwany.
- Można tworzyć kopie zapasowe zaszyfrowanych maszyn wirtualnych i przywracać je w ramach tej samej subskrypcji i regionu co Recovery Services magazynem kopii zapasowych.
- Zaszyfrowane maszyny wirtualne nie mogą zostać odzyskane na poziomie plików/folderów. Musisz odzyskać całą maszynę wirtualną, aby przywrócić pliki i foldery.
- Podczas przywracania maszyny wirtualnej nie można użyć opcji [Zastąp istniejącą maszynę wirtualną](backup-azure-arm-restore-vms.md#restore-options) dla szyfrowanych maszyn wirtualnych. Ta opcja jest obsługiwana tylko w przypadku nieszyfrowanych dysków zarządzanych.

## <a name="before-you-start"></a>Przed rozpoczęciem

Przed rozpoczęciem wykonaj następujące czynności:

1. Upewnij się, że masz co najmniej jedną maszynę wirtualną z [systemem Windows](../virtual-machines/linux/disk-encryption-overview.md) lub [Linux](../virtual-machines/linux/disk-encryption-overview.md) z włączonym ADE.
2. [Przegląd macierzy obsługi](backup-support-matrix-iaas.md) kopii zapasowej maszyny wirtualnej platformy Azure
3. [Utwórz](backup-create-rs-vault.md) magazyn kopii zapasowych Recovery Services, jeśli go nie masz.
4. Jeśli włączysz szyfrowanie dla maszyn wirtualnych, które są już włączone dla kopii zapasowej, musisz po prostu podać kopię zapasową z uprawnieniami dostępu do Key Vault, aby kopie zapasowe mogły kontynuować działanie bez zakłóceń. [Dowiedz się więcej](#provide-permissions) o przypisywaniu tych uprawnień.

Ponadto istnieje kilka rzeczy, które mogą być konieczne w pewnych okolicznościach:

- **Zainstaluj agenta maszyny wirtualnej na maszynie wirtualnej**: Azure Backup tworzenie kopii zapasowych maszyn wirtualnych platformy Azure przez zainstalowanie rozszerzenia agenta maszyny wirtualnej platformy Azure uruchomionego na tym komputerze. Jeśli maszyna wirtualna została utworzona na podstawie obrazu portalu Azure Marketplace, Agent jest zainstalowany i uruchomiony. Jeśli tworzysz niestandardową maszynę wirtualną lub migrujesz maszynę lokalną, może być konieczne [ręczne zainstalowanie agenta](backup-azure-arm-vms-prepare.md#install-the-vm-agent).

## <a name="configure-a-backup-policy"></a>Konfigurowanie zasad kopii zapasowych

1. Jeśli jeszcze nie utworzono magazynu Recovery Services kopii zapasowych, wykonaj [te instrukcje](backup-create-rs-vault.md).
1. Otwórz magazyn w portalu, a następnie wybierz pozycję **+ kopia zapasowa** w sekcji **Przegląd** .

    ![Okienko kopia zapasowa](./media/backup-azure-vms-encryption/select-backup.png)

1. W **celu utworzenia kopii zapasowej**,  >  **gdzie jest uruchomione Twoje obciążenie?** wybierz pozycję **Azure**.
1. W **czym chcesz utworzyć kopię zapasową?** wybierz **maszynę wirtualną**. Następnie wybierz pozycję **kopia zapasowa**.

      ![Blok scenariusza](./media/backup-azure-vms-encryption/select-backup-goal-one.png)

1. W obszarze **zasady tworzenia kopii zapasowych**  >  **Wybierz pozycję Zasady tworzenia kopii zapasowych**, a następnie wybierz zasady, które chcesz skojarzyć z magazynem. Następnie wybierz pozycję **OK**.
    - Zasady tworzenia kopii zapasowych określają, kiedy są tworzone kopie zapasowe, oraz czas ich przechowywania.
    - Szczegóły domyślnych zasad znajdują się w menu rozwijanym.

    ![Otwarcie bloku scenariusza](./media/backup-azure-vms-encryption/select-backup-goal-two.png)

1. Jeśli nie chcesz używać zasad domyślnych, wybierz pozycję **Utwórz nową**i [Utwórz zasady niestandardowe](backup-azure-arm-vms-prepare.md#create-a-custom-policy).

1. W obszarze **Virtual Machines**wybierz pozycję **Dodaj**.

    ![Otwarcie bloku scenariusza](./media/backup-azure-vms-encryption/add-virtual-machines.png)

1. Wybierz zaszyfrowane maszyny wirtualne, których kopię zapasową chcesz utworzyć, przy użyciu wybranych zasad, a następnie wybierz **przycisk OK**.

      ![Wybieranie szyfrowanych maszyn wirtualnych](./media/backup-azure-vms-encryption/selected-encrypted-vms.png)

1. Jeśli używasz Azure Key Vault, na stronie magazyn zostanie wyświetlony komunikat, że Azure Backup potrzebują dostępu tylko do odczytu do kluczy i wpisów tajnych w Key Vault.

    - Jeśli zostanie wyświetlony ten komunikat, nie jest wymagana żadna akcja.

        ![Dostęp OK](./media/backup-azure-vms-encryption/access-ok.png)

    - Jeśli zostanie wyświetlony ten komunikat, musisz ustawić uprawnienia zgodnie z opisem w [poniższej procedurze](#provide-permissions).

        ![Ostrzeżenie dostępu](./media/backup-azure-vms-encryption/access-warning.png)

1. Wybierz pozycję **Włącz kopię zapasową** , aby wdrożyć zasady tworzenia kopii zapasowych w magazynie, a następnie Włącz tworzenie kopii zapasowych dla wybranych maszyn wirtualnych.

## <a name="trigger-a-backup-job"></a>Wyzwalanie zadania tworzenia kopii zapasowej

Początkowa kopia zapasowa będzie uruchamiana zgodnie z harmonogramem, ale można ją uruchomić od razu w następujący sposób:

1. W menu magazyn wybierz pozycję **elementy kopii zapasowej**.
2. W obszarze **elementy kopii zapasowej**wybierz pozycję **maszyna wirtualna platformy Azure**.
3. Na liście **elementy kopii zapasowej** wybierz wielokropek (...).
4. Wybierz pozycję **Utwórz kopię zapasową teraz**.
5. W obszarze **kopia zapasowa**Użyj formantu kalendarza, aby wybrać ostatni dzień przechowywania punktu odzyskiwania. Następnie wybierz pozycję **OK**.
6. Monitoruj powiadomienia portalu. Postęp zadania można monitorować na pulpicie nawigacyjnym magazynu > **zadania tworzenia kopii zapasowej**  >  **w toku**. W zależności od rozmiaru maszyny wirtualnej tworzenie początkowej kopii zapasowej może potrwać pewien czas.

## <a name="provide-permissions"></a>Podaj uprawnienia

Azure Backup wymaga dostępu tylko do odczytu, aby utworzyć kopię zapasową kluczy i wpisów tajnych wraz z skojarzonymi maszynami wirtualnymi.

- Twoja Key Vault jest skojarzona z dzierżawą usługi Azure AD subskrypcji platformy Azure. Jeśli jesteś użytkownikiem będącym **członkiem**, Azure Backup uzyskuje dostęp do Key Vault bez dalszych akcji.
- Jeśli jesteś użytkownikiem- **gościem**, musisz podać uprawnienia Azure Backup, aby uzyskać dostęp do magazynu kluczy.

Aby ustawić uprawnienia:

1. W Azure Portal wybierz pozycję **wszystkie usługi**i Wyszukaj **magazyny kluczy**.
1. Wybierz magazyn kluczy skojarzony z zaszyfrowaną maszyną wirtualną, której kopię zapasową chcesz utworzyć.
1. Wybierz pozycję **zasady dostępu**  >  **Dodaj zasady dostępu**.

    ![Dodawanie zasad dostępu](./media/backup-azure-vms-encryption/add-access-policy.png)

1. Wybierz pozycję **Wybierz podmiot zabezpieczeń**, a następnie wpisz **Zarządzanie kopią zapasową**.
1. Wybierz pozycję **Usługa zarządzania kopiami zapasowymi**  >  **Select**.

    ![Wybór usługi kopii zapasowej](./media/backup-azure-vms-encryption/select-backup-service.png)

1. W obszarze **Dodaj zasady dostępu**  >  **Skonfiguruj z szablonu (opcjonalnie)** wybierz pozycję **Azure Backup**.
    - Wymagane uprawnienia są wstępnie wypełnione dla **uprawnień klucza** i uprawnień do wpisów **tajnych**.
    - Jeśli maszyna wirtualna jest zaszyfrowana **tylko**przy użyciu klucz szyfrowania bloków, usuń zaznaczenie dla **uprawnień kluczowych** , ponieważ potrzebujesz tylko uprawnień do wpisów tajnych.

    ![Wybór usługi Azure Backup](./media/backup-azure-vms-encryption/select-backup-template.png)

1. Wybierz pozycję **Dodaj**. **Usługa zarządzania kopiami zapasowymi** jest dodawana do **zasad dostępu**.

    ![Zasady dostępu](./media/backup-azure-vms-encryption/backup-service-access-policy.png)

1. Wybierz pozycję **Zapisz** , aby zapewnić Azure Backup z uprawnieniami.

## <a name="restore-an-encrypted-vm"></a>Przywracanie zaszyfrowanej maszyny wirtualnej

Zaszyfrowane maszyny wirtualne można przywrócić tylko przez przywrócenie dysku maszyny wirtualnej, jak wyjaśniono poniżej. **Zastępowanie istniejącej** i **przywrócenie maszyny wirtualnej** nie jest obsługiwane.

Przywróć zaszyfrowane maszyny wirtualne w następujący sposób:

1. [Przywróć dysk maszyny wirtualnej](backup-azure-arm-restore-vms.md#restore-disks).
2. Utwórz ponownie wystąpienie maszyny wirtualnej, wykonując jedną z następujących czynności:
    1. Użyj szablonu wygenerowanego podczas operacji przywracania, aby dostosować ustawienia maszyny wirtualnej i wyzwolić wdrożenie maszyny wirtualnej. [Dowiedz się więcej](backup-azure-arm-restore-vms.md#use-templates-to-customize-a-restored-vm).
    2. Utwórz nową maszynę wirtualną na podstawie przywróconych dysków przy użyciu programu PowerShell. [Dowiedz się więcej](backup-azure-vms-automation.md#create-a-vm-from-restored-disks).
3. W przypadku maszyn wirtualnych z systemem Linux ponownie zainstaluj rozszerzenie ADE, aby dyski z danymi były otwarte i zainstalowane.

## <a name="next-steps"></a>Następne kroki

Jeśli napotkasz jakiekolwiek problemy, zapoznaj się z następującymi artykułami:

- [Typowe błędy](backup-azure-vms-troubleshoot.md) podczas tworzenia kopii zapasowych i przywracania zaszyfrowanych maszyn wirtualnych platformy Azure.
- Problemy z [agentem maszyny wirtualnej platformy Azure/rozszerzeniem kopii zapasowej](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md) .
