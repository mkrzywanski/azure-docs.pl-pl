---
title: Inicjowanie obsługi platformy Azure-SSIS Integration Runtime
description: Dowiedz się, jak aprowizować środowisko Azure SSIS Integration Runtime w usłudze Azure Data Factory w celu wdrażania i uruchamiania pakietów SSIS na platformie Azure.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: tutorial
ms.custom: seo-lt-2019
ms.date: 07/06/2020
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: mflasko
ms.openlocfilehash: 76c936cb0c1a95ca1bf5919cbf2753fb6f050687
ms.sourcegitcommit: f684589322633f1a0fafb627a03498b148b0d521
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/06/2020
ms.locfileid: "85971015"
---
# <a name="provision-the-azure-ssis-integration-runtime-in-azure-data-factory"></a>Inicjowanie obsługi platformy Azure-SSIS Integration Runtime w Azure Data Factory

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Ten samouczek zawiera instrukcje dotyczące korzystania z Azure Portal w celu aprowizacji środowiska Integration Runtime (IR) na platformie SQL Server Integration Services Azure w Azure Data Factory (ADF). Azure-SSIS IR obsługuje:

- Uruchomione pakiety wdrożone do wykazu usług SSIS (SSISDB) hostowane przez Azure SQL Database Server/wystąpienie zarządzane (model wdrażania projektu)
- Uruchomione pakiety wdrożone w systemie plików, Azure Files lub SQL Server Database (MSDB) hostowane przez wystąpienie zarządzane Azure SQL (model wdrażania pakietów)

Po zainicjowaniu Azure-SSIS IR można używać dobrze znanych narzędzi do wdrażania i uruchamiania pakietów na platformie Azure. Te narzędzia już obsługują platformę Azure i obejmują SQL Server narzędzia do obsługi danych (SSDT), SQL Server Management Studio (SSMS) i narzędzia wiersza polecenia, takie jak [dtutil](https://docs.microsoft.com/sql/integration-services/dtutil-utility?view=sql-server-2017) i [AzureDTExec](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-azure-enabled-dtexec).

Aby uzyskać informacje koncepcyjne dotyczące środowisk Azure SSIS IR, zobacz [Omówienie środowiska Azure-SSIS Integration Runtime](concepts-integration-runtime.md#azure-ssis-integration-runtime).

W tym samouczku wykonasz następujące czynności:

> [!div class="checklist"]
> * Tworzenie fabryki danych.
> * Aprowizowanie środowiska Azure-SSIS Integration Runtime.

## <a name="prerequisites"></a>Wymagania wstępne

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

- **Subskrypcja platformy Azure**. Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/).

- **Serwer Azure SQL Database (opcjonalnie)**. Jeśli nie masz jeszcze serwera bazy danych, utwórz go w witrynie Azure Portal, zanim zaczniesz. Data Factory spowoduje utworzenie wystąpienia SSISDB na tym serwerze bazy danych. 

  Zaleca się utworzenie serwera bazy danych w tym samym regionie platformy Azure, co środowisko Integration Runtime. Ta konfiguracja umożliwia wykonywanie dzienników wykonywania zapisu środowiska Integration Runtime w usłudze SSISDB bez przekroczenia regionów świadczenia usługi Azure.

  Należy pamiętać o następujących kwestiach:

  - Na podstawie wybranego serwera bazy danych wystąpienie SSISDB można utworzyć w Twoim imieniu jako pojedynczą bazę danych w ramach puli elastycznej lub w wystąpieniu zarządzanym. Może być dostępna w sieci publicznej lub przez dołączenie do sieci wirtualnej. Aby uzyskać wskazówki dotyczące wybierania typu serwera bazy danych do hostowania SSISDB, zobacz [porównanie SQL Database i wystąpienia zarządzanego SQL](../data-factory/create-azure-ssis-integration-runtime.md#comparison-of-sql-database-and-sql-managed-instance). 
  
    Jeśli używasz serwera Azure SQL Database z regułami zapory IP/punktami końcowymi usługi sieci wirtualnej lub wystąpieniem zarządzanym z prywatnym punktem końcowym do hostowania SSISDB lub potrzebujesz dostępu do danych lokalnych bez konfigurowania samoobsługowego środowiska IR, musisz dołączyć Azure-SSIS IR do sieci wirtualnej. Aby uzyskać więcej informacji, zobacz [tworzenie Azure-SSIS IR w sieci wirtualnej](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime).

  - Upewnij się, że ustawienie **Zezwalaj na dostęp do usług platformy Azure** jest włączone dla serwera bazy danych. To ustawienie nie ma zastosowania w przypadku używania serwera Azure SQL Database z regułami zapory IP/punktami końcowymi usługi sieci wirtualnej lub wystąpieniem zarządzanym z prywatnym punktem końcowym do hostowania SSISDB. Aby uzyskać więcej informacji, zobacz [bezpieczny Azure SQL Database](../sql-database/sql-database-security-tutorial.md#create-firewall-rules). Aby włączyć to ustawienie przy użyciu programu PowerShell, zobacz polecenie [New-AzSqlServerFirewallRule](/powershell/module/az.sql/new-azsqlserverfirewallrule).

  - Dodaj adres IP komputera klienckiego lub zakres adresów IP, który zawiera adres IP komputera klienckiego, do listy adres IP klienta w ustawieniach zapory dla serwera bazy danych. Aby uzyskać więcej informacji, zobacz [Reguły zapory na poziomie serwera i na poziomie bazy danych usługi Azure SQL Database](../sql-database/sql-database-firewall-configure.md).

  - Można nawiązać połączenie z serwerem bazy danych przy użyciu uwierzytelniania SQL z poświadczeniami administratora serwera lub przy użyciu uwierzytelniania usługi Azure AD z zarządzaną tożsamością dla fabryki danych. Dla tych ostatnich należy dodać tożsamość zarządzaną dla fabryki danych do grupy usługi Azure AD z uprawnieniami dostępu do serwera bazy danych. Aby uzyskać więcej informacji, zobacz [tworzenie Azure-SSIS IR przy użyciu uwierzytelniania usługi Azure AD](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime).

  - Upewnij się, że serwer bazy danych nie ma już wystąpienia SSISDB. Inicjowanie obsługi Azure-SSIS IR nie obsługuje korzystania z istniejącego wystąpienia SSISDB.

> [!NOTE]
> Listę regionów świadczenia usługi Azure, w których Data Factory i Azure-SSIS IR są obecnie dostępne, można znaleźć w temacie [Data Factory i SSIS IR dostępność według regionów](https://azure.microsoft.com/global-infrastructure/services/?products=data-factory&regions=all). 

## <a name="create-a-data-factory"></a>Tworzenie fabryki danych

Aby utworzyć fabrykę danych za pomocą Azure Portal, wykonaj instrukcje krok po kroku w temacie [Tworzenie fabryki danych za pomocą interfejsu użytkownika](https://docs.microsoft.com/azure/data-factory/quickstart-create-data-factory-portal#create-a-data-factory). Wybierz pozycję **Przypnij do pulpitu nawigacyjnego** , aby umożliwić szybki dostęp po utworzeniu. 

Po utworzeniu fabryki danych Otwórz jej stronę przeglądu w Azure Portal. Wybierz kafelek **tworzenie & monitor** **, aby otworzyć stronę wprowadzenie** na osobnej karcie. W tym miejscu możesz kontynuować tworzenie Azure-SSIS IR.

## <a name="create-an-azure-ssis-integration-runtime"></a>Tworzenie środowiska Azure SSIS Integration Runtime

### <a name="from-the-data-factory-overview"></a>Z poziomu omówienia usługi Data Factory

1. Na stronie **Wprowadzenie** wybierz kafelek **Konfigurowanie środowiska SSIS Integration Runtime**. 

   ![Kafelek „Konfigurowanie środowiska SSIS Integration Runtime”](./media/tutorial-create-azure-ssis-runtime-portal/configure-ssis-integration-runtime-tile.png)

1. Aby poznać pozostałe kroki konfigurowania środowiska Azure-SSIS IR, zobacz sekcję [Aprowizowanie środowiska Azure SSIS Integration Runtime](#provision-an-azure-ssis-integration-runtime). 

### <a name="from-the-authoring-ui"></a>Z interfejsu użytkownika tworzenia

1. W interfejsie użytkownika Azure Data Factory przejdź do karty **Edycja** i wybierz pozycję **połączenia**. Następnie przejdź na kartę **Integration Runtime** , aby wyświetlić istniejące środowiska Integration Runtime w fabryce danych. 

   ![Opcje wyboru dotyczące wyświetlania istniejących środowisk IR](./media/tutorial-create-azure-ssis-runtime-portal/view-azure-ssis-integration-runtimes.png)

1. Wybierz pozycję **Nowy** , aby utworzyć Azure-SSIS IR i Otwórz okienko **konfiguracji środowiska Integration Runtime** . 

   ![Środowisko Integration Runtime za pomocą menu](./media/tutorial-create-azure-ssis-runtime-portal/edit-connections-new-integration-runtime-button.png)

1. W okienku **Konfiguracja środowiska Integration Runtime** wybierz **istniejące pakiety usług SSIS i przenieś je do wykonania na kafelku platformy Azure** , a następnie wybierz przycisk **dalej**.

   ![Określanie typu środowiska Integration Runtime](./media/tutorial-create-azure-ssis-runtime-portal/integration-runtime-setup-options.png)

1. Aby poznać pozostałe kroki konfigurowania środowiska Azure-SSIS IR, zobacz sekcję [Aprowizowanie środowiska Azure SSIS Integration Runtime](#provision-an-azure-ssis-integration-runtime). 

## <a name="provision-an-azure-ssis-integration-runtime"></a>Aprowizowanie środowiska Azure-SSIS Integration Runtime

Okienko **Konfiguracja środowiska Integration Runtime** zawiera trzy strony, na których można kolejno skonfigurować ustawienia ogólne, wdrożenia i zaawansowane.

### <a name="general-settings-page"></a>Strona ustawień ogólnych

Na stronie **Ustawienia ogólne** okienka **konfiguracji środowiska Integration Runtime** wykonaj następujące czynności. 

   ![Ustawienia ogólne](./media/tutorial-create-azure-ssis-runtime-portal/general-settings.png)

   1. W polu **Nazwa** wprowadź nazwę środowiska Integration Runtime. 

   1. W polu **Opis** wprowadź opis środowiska Integration Runtime. 

   1. W polu **Lokalizacja** wybierz lokalizację środowiska Integration Runtime. Wyświetlane są tylko obsługiwane lokalizacje. Zalecamy wybranie tej samej lokalizacji do hostowania bazy danych SSISDB co w przypadku serwera bazy danych. 

   1. W polu **Rozmiar węzła** wybierz rozmiar węzła w klastrze środowiska Integration Runtime. Wyświetlane są tylko obsługiwane rozmiary węzłów. Wybierz duży rozmiar węzła (skalowanie w górę), jeśli chcesz uruchomić wiele pakietów intensywnie korzystających z mocy obliczeniowej lub intensywnie korzystających z pamięci. 

   1. W polu **Liczba węzłów** wybierz liczbę węzłów w klastrze środowiska Integration Runtime. Wyświetlane są tylko obsługiwane liczby węzłów. Wybierz duży klaster z wieloma węzłami (skalowanie w poziomie), jeśli chcesz uruchomić wiele pakietów równolegle. 

   1. W polu **wydanie/licencja**wybierz wersję SQL Server dla środowiska Integration Runtime: Standard lub Enterprise. Wybierz pozycję Enterprise (przedsiębiorstwo), jeśli chcesz korzystać z funkcji zaawansowanych w środowisku Integration Runtime. 

   1. W obszarze **oszczędność pieniędzy**wybierz opcję korzyść użycia hybrydowego platformy Azure dla środowiska Integration Runtime: **tak** lub **nie**. Wybierz pozycję **tak** , jeśli chcesz uzyskać własną licencję SQL Server z programem Software Assurance, aby korzystać z oszczędności związanych z użyciem hybrydowej. 

   1. Wybierz pozycję **Dalej**. 

### <a name="deployment-settings-page"></a>Strona Ustawienia wdrożenia

Na stronie **Ustawienia wdrożenia** w okienku **Konfiguracja środowiska Integration Runtime** wykonaj następujące czynności.

   1. Zaznacz pole wyboru **Utwórz wykaz usług SSIS (SSISDB) hostowane przez serwer Azure SQL Database/wystąpienie zarządzane do przechowywania projektów/pakietów/środowisk/dzienników wykonywania** , aby określić, czy pakiety mają być wdrażane w usłudze SSISDB (model wdrażania projektu). Alternatywnie nie trzeba tworzyć SSISDB, jeśli chcesz wdrożyć pakiety w systemie plików, Azure Files lub SQL Server Database (MSDB) hostowanym przez wystąpienie zarządzane Azure SQL (model wdrażania pakietów).
   
      Niezależnie od modelu wdrażania zaznacz to pole wyboru, aby wybrać, czy chcesz użyć agenta SQL Server hostowanego przez wystąpienie zarządzane usługi Azure SQL do organizowania/planowania wykonań pakietów, ponieważ jest on włączony przez SSISDB. Aby uzyskać więcej informacji, zobacz [Planowanie wykonywania pakietów usług SSIS za pośrednictwem agenta wystąpienia zarządzanego usługi Azure SQL](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-managed-instance-agent).
   
      Jeśli zaznaczysz to pole wyboru, musisz przenieść własny serwer baz danych do SSISDB, który zostanie utworzony w Twoim imieniu i będzie zarządzany.

      ![Ustawienia wdrażania dla SSISDB](./media/tutorial-create-azure-ssis-runtime-portal/deployment-settings.png)
   
      1. W polu **Subskrypcja** określ subskrypcję platformy Azure, z której korzysta Twój serwer bazy danych hostujący bazę danych SSISDB. 

      1. W polu **Lokalizacja** określ lokalizację serwera bazy danych hostującego bazę danych SSISDB. Zalecamy wybranie tej samej lokalizacji co w przypadku środowiska Integration Runtime.

      1. W polu **Punkt końcowy serwera bazy danych wykazu** wybierz punkt końcowy serwera bazy danych hostującego bazę danych SSISDB. 
   
         Na podstawie wybranego serwera bazy danych wystąpienie SSISDB można utworzyć w Twoim imieniu jako pojedynczą bazę danych w ramach puli elastycznej lub w wystąpieniu zarządzanym. Może być dostępna w sieci publicznej lub przez dołączenie do sieci wirtualnej. Aby uzyskać wskazówki dotyczące wybierania typu serwera bazy danych do hostowania SSISDB, zobacz [porównanie SQL Database i wystąpienia zarządzanego SQL](../data-factory/create-azure-ssis-integration-runtime.md#comparison-of-sql-database-and-sql-managed-instance).   

         W przypadku wybrania serwera Azure SQL Database z regułami zapory IP/punktami końcowymi usługi sieci wirtualnej lub wystąpieniem zarządzanym z prywatnym punktem końcowym do hostowania SSISDB lub gdy wymagany jest dostęp do danych lokalnych bez konfigurowania samoobsługowego środowiska IR, należy dołączyć Azure-SSIS IR do sieci wirtualnej. Aby uzyskać więcej informacji, zobacz [tworzenie Azure-SSIS IR w sieci wirtualnej](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime).

      1. Zaznacz pole wyboru **Użyj uwierzytelniania usługi AAD z zarządzaną tożsamością dla usługi ADF,** aby wybrać metodę uwierzytelniania dla serwera bazy danych na potrzeby hostowania SSISDB. Wybierz opcję uwierzytelnianie SQL lub uwierzytelnianie usługi Azure AD z zarządzaną tożsamością dla fabryki danych.

         W przypadku zaznaczenia tego pola wyboru należy dodać tożsamość zarządzaną dla fabryki danych do grupy usługi Azure AD z uprawnieniami dostępu do serwera bazy danych. Aby uzyskać więcej informacji, zobacz [tworzenie Azure-SSIS IR przy użyciu uwierzytelniania usługi Azure AD](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime).
   
      1. W polu **Nazwa użytkownika administratora**wprowadź nazwę użytkownika uwierzytelniania SQL dla serwera bazy danych, który będzie hostować SSISDB. 

      1. W polu **hasło administratora**wprowadź hasło uwierzytelniania SQL dla serwera bazy danych, aby hostować SSISDB. 

      1. W polu **warstwa usługi bazy danych wykazu**wybierz warstwę usług dla serwera bazy danych, która będzie hostować SSISDB. Wybierz warstwę podstawowa, standardowa lub Premium lub wybierz nazwę puli elastycznej.

   1. Wybierz pozycję **Utwórz magazyny pakietów, aby zarządzać pakietami, które są wdrożone w systemie plików/Azure Files/SQL Server Database (msdb) hostowanym przez wystąpienie zarządzane usługi Azure SQL** , aby określić, czy chcesz zarządzać pakietami, które są wdrożone w bazie danych msdb, systemie plików czy Azure Files (model wdrażania pakietów) przy użyciu Azure-SSIS IR magazynów pakietów.
   
      Magazyn pakietów Azure-SSIS IR umożliwia importowanie/eksportowanie/usuwanie/uruchamianie pakietów oraz monitorowanie/zatrzymywanie uruchomionych pakietów za pośrednictwem programu SSMS podobnie jak w przypadku [starszego magazynu pakietów SSIS](https://docs.microsoft.com/sql/integration-services/service/package-management-ssis-service?view=sql-server-2017). Aby uzyskać więcej informacji, zobacz [Zarządzanie pakietami SSIS za pomocą magazynów pakietów Azure-SSIS IR](https://docs.microsoft.com/azure/data-factory/azure-ssis-integration-runtime-package-store).
   
      Jeśli zaznaczysz to pole wyboru, możesz dodać wiele magazynów pakietów do Azure-SSIS IR, wybierając pozycję **Nowy**. Z kolei jeden magazyn pakietów może być współużytkowany przez wiele urzędów danych Azure-SSIS.

      ![Ustawienia wdrażania dla systemu MSDB/system plików/Azure Files](./media/tutorial-create-azure-ssis-runtime-portal/deployment-settings2.png)

      W okienku **Dodaj magazyn pakietów** wykonaj następujące czynności.
   
      1. W polu **Nazwa magazynu pakietów**wprowadź nazwę swojego magazynu pakietów. 

      1. W przypadku **połączonej usługi magazynu pakietów**wybierz istniejącą połączoną usługę, która przechowuje informacje o dostępie do systemu plików/Azure Files/wystąpienia zarządzanego Azure SQL, w którym są wdrażane pakiety, lub Utwórz nowe, wybierając pozycję **nowe**. W okienku **Nowa połączona usługa** wykonaj następujące czynności. 

         ![Ustawienia wdrażania dla połączonych usług](./media/tutorial-create-azure-ssis-runtime-portal/deployment-settings-linked-service.png)

         1. W polu **Nazwa**wprowadź nazwę połączonej usługi. 
         
         1. W polu **Opis**wprowadź opis połączonej usługi. 
         
         1. W obszarze **Typ**wybierz pozycję **Azure File Storage**, **wystąpienie zarządzane Azure SQL**lub **system plików**.

         1. Możesz zignorować **połączenie za pośrednictwem środowiska Integration Runtime**, ponieważ zawsze korzystamy z Azure-SSIS IR, aby pobrać informacje o dostępie do magazynów pakietów.

         1. W przypadku wybrania **usługi Azure File Storage**wykonaj następujące czynności. 

            1. W **obszarze Metoda wyboru konta**wybierz pozycję **z subskrypcji platformy Azure** lub **wprowadź ręcznie**.
         
            1. Jeśli wybierzesz opcję **z subskrypcji platformy Azure**, wybierz odpowiednią **subskrypcję platformy Azure**, **nazwę konta magazynu**i **udział plików**.
            
            1. W przypadku wybrania opcji **wprowadź ręcznie**wprowadź wartość `\\<storage account name>.file.core.windows.net\<file share name>` w polach **host**, `Azure\<storage account name>` **nazwa użytkownika**i `<storage account key>` **hasło** lub wybierz swoje **Azure Key Vault** w miejscu, w którym są przechowywane jako wpis tajny.

         1. W przypadku wybrania **wystąpienia zarządzanego Azure SQL**należy wykonać następujące czynności. 

            1. Wybierz opcję **Parametry połączenia** , aby wprowadzić ją ręcznie lub **Azure Key Vault** , gdzie jest przechowywana jako wpis tajny.
         
            1. W przypadku wybrania opcji **Parametry połączenia**wykonaj następujące czynności. 

               1. W przypadku w **pełni kwalifikowanej nazwy domeny**wprowadź `<server name>.<dns prefix>.database.windows.net` `<server name>.public.<dns prefix>.database.windows.net,3342` odpowiednio lub jako prywatny lub publiczny punkt końcowy wystąpienia zarządzanego Azure SQL. W przypadku wprowadzenia prywatnego punktu końcowego **Test connection** nie ma zastosowania, ponieważ interfejs użytkownika funkcji ADF nie może uzyskać do niego dostępu.

               1. W obszarze **Nazwa bazy danych**wprowadź `msdb` .
               
               1. W obszarze **Typ uwierzytelniania**wybierz pozycję **uwierzytelnianie SQL**, **tożsamość zarządzana**lub nazwa **główna usługi**.

               1. W przypadku wybrania opcji **uwierzytelnianie SQL**wprowadź odpowiednią **nazwę użytkownika** i **hasło** lub wybierz **Azure Key Vault** , w których jest przechowywany jako wpis tajny.

               1. W przypadku wybrania opcji **tożsamość zarządzana**Przydziel usłudze ADF zarządzaną tożsamość do wystąpienia zarządzanego usługi Azure SQL.

               1. W przypadku wybrania **nazwy głównej usługi**wprowadź odpowiedni **Identyfikator** jednostki usługi i **klucz jednostki usługi** albo wybierz **Azure Key Vault** , w którym jest przechowywany jako wpis tajny.

         1. W przypadku wybrania opcji **system plików**wprowadź ścieżkę UNC do folderu, w którym pakiety są wdrożone dla **hosta**, a także odpowiednią **nazwę użytkownika** i **hasło** lub wybierz **Azure Key Vault** , w którym są przechowywane jako wpis tajny.

         1. Wybierz **Test connection** , jeśli ma to zastosowanie, a jeśli to się powiedzie, wybierz pozycję **Utwórz**.

      Dodane magazyny pakietów zostaną wyświetlone na stronie **Ustawienia wdrożenia** . Aby je usunąć, zaznacz ich pola wyboru, a następnie wybierz pozycję **Usuń**.

   1. Wybierz **Test connection** , jeśli ma to zastosowanie, a jeśli to się powiedzie, wybierz pozycję **dalej**.

### <a name="advanced-settings-page"></a>Strona Ustawienia zaawansowane

Na stronie **Ustawienia zaawansowane** okienka **Konfiguracja środowiska Integration Runtime** wykonaj następujące czynności. 

   ![Ustawienia zaawansowane](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings.png)

   1. Dla opcji **Maksymalna liczba równoległych wykonań na węzeł**wybierz maksymalną liczbę pakietów do uruchomienia współbieżnie na węzeł w klastrze środowiska Integration Runtime. Wyświetlane są tylko obsługiwane liczby pakietów. Wybierz niską liczbę, jeśli chcesz użyć więcej niż jednego rdzenia do uruchamiania pojedynczego dużego pakietu, który jest obliczeniowy lub intensywnie korzystający z pamięci. Wybierz dużą liczbę, jeśli chcesz uruchomić co najmniej jeden niewielki pakiet w jednym rdzeniu. 

   1. Zaznacz pole wyboru **dostosuj Azure-SSIS Integration Runtime z dodatkowymi konfiguracjami systemu/instalacjami składników** , aby określić, czy chcesz dodać do Azure-SSIS IR niestandardowe ustawienia standardowe/ekspresowe. Aby uzyskać więcej informacji, zobacz [Konfiguracja niestandardowa dla Azure-SSIS IR](https://docs.microsoft.com/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup).
   
   1. Wybierz **sieć wirtualną do przyłączenia do Azure-SSIS Integration Runtime, Zezwól na automatyczny dostęp do zasobów sieciowych, a opcjonalnie Przenieś własne statyczne publiczne adresy IP,** aby wybrać, czy chcesz dołączyć do Azure-SSIS IR do sieci wirtualnej.

      Wybierz go, jeśli używasz serwera Azure SQL Database z regułami zapory IP/punktami końcowymi usługi sieci wirtualnej lub wystąpieniem zarządzanym z prywatnym punktem końcowym do hostowania SSISDB, lub jeśli potrzebujesz dostępu do danych lokalnych bez konfigurowania samoobsługowego środowiska IR. Aby uzyskać więcej informacji, zobacz [tworzenie Azure-SSIS IR w sieci wirtualnej](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime). 
   
   1. Zaznacz pole wyboru **Skonfiguruj Samoobsługowe Integration Runtime jako serwer proxy dla Azure-SSIS Integration Runtime** , aby określić, czy chcesz skonfigurować własne środowisko IR jako serwer proxy dla Azure-SSIS IR. Aby uzyskać więcej informacji, zobacz [Konfigurowanie własnego środowiska IR jako serwera proxy](https://docs.microsoft.com/azure/data-factory/self-hosted-integration-runtime-proxy-ssis).   

   1. Wybierz pozycję **Continue** (Kontynuuj). 

Na stronie **Podsumowanie** okienka **konfiguracji środowiska Integration Runtime** Przejrzyj wszystkie ustawienia aprowizacji, zakładka zalecanych linków do dokumentacji, a następnie wybierz pozycję **Zakończ** , aby rozpocząć tworzenie środowiska Integration Runtime. 

   > [!NOTE]
   > Bez czasu instalacji niestandardowej ten proces powinien zakończyć się w ciągu 5 minut.
   >
   > Jeśli używasz SSISDB, Usługa Data Factory będzie łączyć się z serwerem bazy danych w celu przygotowania SSISDB. 
   > 
   > Podczas aprowizacji Azure-SSIS IR są również instalowane narzędzia do redystrybucyjny i pakiet Azure Feature Pack dla usług SSIS. Te składniki zapewniają łączność z plikami programu Excel, plikami dostępu i różnymi źródłami danych platformy Azure, a także ze źródłami danych, które już obsługują. Aby uzyskać więcej informacji na temat wbudowanych/preinstalowanych składników, zobacz [składniki wbudowane/preinstalowane na Azure-SSIS IR](https://docs.microsoft.com/azure/data-factory/built-in-preinstalled-components-ssis-integration-runtime). Aby uzyskać więcej informacji o dodatkowych składnikach, które można zainstalować, zobacz [Konfiguracja niestandardowa dla Azure-SSIS IR](https://docs.microsoft.com/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup).

### <a name="connections-pane"></a>Okienko połączenia

W okienku **połączenia** okienka **Zarządzanie** centrum przejdź do strony **Integration Runtimes** i wybierz pozycję **Odśwież**. 

   ![Okienko połączenia](./media/tutorial-create-azure-ssis-runtime-portal/connections-pane.png)

   Możesz edytować/zmienić konfigurację Azure-SSIS IR, wybierając jego nazwę. Możesz również wybrać odpowiednie przyciski do monitorowania/uruchamiania/zatrzymywania/usuwania Azure-SSIS IR, automatycznie wygenerować potok ADF z działaniem pakietu SSIS do uruchomienia na Azure-SSIS IR, a następnie wyświetlić kod JSON/ładunek Azure-SSIS IR.  Edytowanie/usuwanie Azure-SSIS IR można wykonać tylko wtedy, gdy jest ono zatrzymane.

## <a name="deploy-ssis-packages"></a>Wdrażanie pakietów usług SSIS

Jeśli używasz SSISDB, możesz wdrożyć w nim pakiety i uruchamiać je na Azure-SSIS IR przy użyciu narzędzi SSDT lub SSMS z obsługą platformy Azure. Narzędzia te łączą się z serwerem bazy danych za pośrednictwem swojego punktu końcowego serwera: 

- W przypadku serwera Azure SQL Database format punktu końcowego serwera to `<server name>.database.windows.net` .
- W przypadku wystąpienia zarządzanego z prywatnym punktem końcowym format punktu końcowego serwera to `<server name>.<dns prefix>.database.windows.net` .
- W przypadku wystąpienia zarządzanego z publicznym punktem końcowym format punktu końcowego serwera to `<server name>.public.<dns prefix>.database.windows.net,3342` . 

Jeśli nie korzystasz z SSISDB, możesz wdrożyć pakiety w systemie plików, Azure Files lub MSDB, hostowanym przez wystąpienie zarządzane usługi Azure SQL i uruchamiać je na Azure-SSIS IR przy użyciu narzędzi wiersza polecenia [dtutil](https://docs.microsoft.com/sql/integration-services/dtutil-utility?view=sql-server-2017) i [AzureDTExec](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-azure-enabled-dtexec) . 

Aby uzyskać więcej informacji, zobacz [wdrażanie projektów SSIS/pakietów](https://docs.microsoft.com/sql/integration-services/packages/deploy-integration-services-ssis-projects-and-packages?view=sql-server-ver15).

W obu przypadkach wdrożone pakiety można również uruchomić na Azure-SSIS IR przy użyciu działania wykonywania pakietu SSIS w potokach Data Factory. Aby uzyskać więcej informacji, zobacz [wywołania wykonywania pakietu SSIS jako działania pierwszej klasy Data Factory](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity).

Zobacz również następującą dokumentację usług SSIS: 

- [Wdrażanie, uruchamianie i monitorowanie pakietów SSIS na platformie Azure](/sql/integration-services/lift-shift/ssis-azure-deploy-run-monitor-tutorial) 
- [Łączenie się z usługą SSISDB na platformie Azure](/sql/integration-services/lift-shift/ssis-azure-connect-to-catalog-database) 
- [Zaplanuj wykonywanie pakietów na platformie Azure](/sql/integration-services/lift-shift/ssis-azure-schedule-packages) 
- [Connect to on-premises data sources with Windows authentication (Łączenie z lokalnymi źródłami danych przy użyciu uwierzytelniania systemu Windows)](/sql/integration-services/lift-shift/ssis-azure-connect-with-windows-auth) 

## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się więcej o dostosowywaniu środowiska Azure-SSIS Integration Runtime, przejdź do następującego artykułu: 

> [!div class="nextstepaction"]
> [Dostosowywanie Azure-SSIS IR](https://docs.microsoft.com/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup)