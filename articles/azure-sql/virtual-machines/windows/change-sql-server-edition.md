---
title: Zmiana w miejscu wersji SQL Server
description: Dowiedz się, jak zmienić wersję SQL Server maszyny wirtualnej na platformie Azure.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
tags: azure-resource-manager
ms.service: virtual-machines-sql
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/14/2020
ms.author: mathoma
ms.reviewer: jroth
ms.custom: seo-lt-2019
ms.openlocfilehash: 228f76c9ecb262ffca7851f4339b73c5574f09dc
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "84669171"
---
# <a name="in-place-change-of-sql-server-edition-on-azure-vm"></a>Zmiana w miejscu SQL Server edycji na maszynie wirtualnej platformy Azure
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

W tym artykule opisano sposób zmiany wersji SQL Server na maszynie wirtualnej z systemem Windows na platformie Azure. 

Wersja SQL Server jest określana przez klucz produktu i jest określana podczas procesu instalacji przy użyciu nośnika instalacyjnego. Wersja określa, jakie [funkcje](/sql/sql-server/editions-and-components-of-sql-server-2017) są dostępne w produkcie SQL Server. Wersję SQL Server można zmienić przy użyciu nośnika instalacyjnego, a obniżenie poziomu obniża koszty lub uaktualnienie, aby umożliwić obsługę większej liczby funkcji.

Gdy wersja SQL Server została zmieniona wewnętrznie na maszynę wirtualną SQL Server, należy zaktualizować Właściwość Edition SQL Server w Azure Portal na potrzeby rozliczeń. 

## <a name="prerequisites"></a>Wymagania wstępne

Aby przeprowadzić zmianę w miejscu wersji SQL Server, potrzebne są następujące elementy: 

- [Subskrypcja platformy Azure](https://azure.microsoft.com/free/).
- [SQL Server maszynę wirtualną w systemie Windows](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision) zarejestrowana przy użyciu [dostawcy zasobów maszyny wirtualnej SQL](sql-vm-resource-provider-register.md).
- Skonfiguruj nośnik z **odpowiednią wersją** SQL Server. Klienci posiadający [program Software Assurance](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default) mogą uzyskać nośnik instalacyjny z [centrum licencjonowania zbiorowego](https://www.microsoft.com/Licensing/servicecenter/default.aspx). Klienci, którzy nie mają programu Software Assurance, mogą korzystać z nośnika instalacyjnego z witryny Azure Marketplace SQL Server obrazu maszyny wirtualnej, który ma odpowiednie wydanie (zazwyczaj znajduje się w temacie `C:\SQLServerFull` ). 


## <a name="upgrade-an-edition"></a>Uaktualnianie wersji

> [!WARNING]
> Uaktualnienie wersji SQL Server spowoduje ponowne uruchomienie usługi dla SQL Server wraz ze wszystkimi skojarzonymi usługami, takimi jak Analysis Services i R Services. 

Aby uaktualnić wersję SQL Server, uzyskaj SQL Server instalacyjny nośnik dla odpowiedniej wersji SQL Server, a następnie wykonaj następujące czynności:

1. Otwórz Setup.exe z nośnika instalacyjnego SQL Server. 
1. Przejdź do pozycji **konserwacja** i wybierz opcję **uaktualnienia wersji** . 

   ![Wybór na potrzeby uaktualnienia wersji SQL Server](./media/change-sql-server-edition/edition-upgrade.png)

1. Wybierz pozycję **dalej** do momentu uzyskania dostępu **do strony gotowy do uaktualnienia wersji** , a następnie wybierz pozycję **Uaktualnij**. Okno instalatora może przestać odpowiadać przez kilka minut, gdy zmiana zacznie obowiązywać. **Pełna** Strona potwierdzi, że uaktualnienie wersji zostało zakończone. 

Po uaktualnieniu wersji SQL Server należy zmodyfikować właściwość wersja SQL Server maszyny wirtualnej w Azure Portal. Spowoduje to zaktualizowanie metadanych i rozliczeń skojarzonych z tą maszyną wirtualną.

## <a name="downgrade-an-edition"></a>Starsza wersja

Aby można było obniżyć wersję SQL Server, należy całkowicie odinstalować SQL Server i ponownie ją zainstalować przy użyciu nośnika instalacyjnego wymagane wydanie. 

> [!WARNING]
> Odinstalowanie SQL Server może pociągnąć za sobą dodatkowy Przestój. 

Wersję SQL Server można obniżyć, wykonując następujące czynności:

1. Utwórz kopię zapasową wszystkich baz danych, w tym systemowych baz danych. 
1. Przenieś systemowe bazy danych (Master, model i msdb) do nowej lokalizacji. 
1. Całkowicie Odinstaluj SQL Server i wszystkie skojarzone usługi. 
1. Uruchom ponownie maszynę wirtualną. 
1. Zainstaluj SQL Server przy użyciu nośnika z odpowiednią wersją SQL Server.
1. Zainstaluj najnowsze dodatki Service Pack i aktualizacje zbiorcze.  
1. Zastąp nowe systemowe bazy danych, które zostały utworzone podczas instalacji z systemowymi bazami danych, które zostały wcześniej przeniesione do innej lokalizacji. 

Po obniżeniu wersji SQL Server należy zmodyfikować właściwość wersja SQL Server maszyny wirtualnej w Azure Portal. Spowoduje to zaktualizowanie metadanych i rozliczeń skojarzonych z tą maszyną wirtualną.

## <a name="change-edition-in-portal"></a>Zmień wersję w portalu 

Po zmianie wersji SQL Server przy użyciu nośnika instalacyjnego i zarejestrowaniu maszyny wirtualnej SQL Server z [dostawcą zasobów maszyny wirtualnej SQL](sql-vm-resource-provider-register.md)można użyć Azure Portal do zmodyfikowania właściwości wersji maszyny wirtualnej SQL Server na potrzeby rozliczania. Aby to zrobić, wykonaj następujące kroki: 

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com). 
1. Przejdź do zasobu maszyny wirtualnej SQL Server. 
1. W obszarze **Ustawienia**wybierz pozycję **Konfiguruj**. Następnie wybierz żądaną wersję SQL Server z listy rozwijanej w obszarze **Edycja**. 

   ![Metadane zmiany wersji](./media/change-sql-server-edition/edition-change-in-portal.png)

1. Przejrzyj ostrzeżenie z informacją, że musisz najpierw zmienić wersję SQL Server i że właściwość Edition musi być zgodna z wersją SQL Server. 
1. Wybierz pozycję **Zastosuj** , aby zastosować zmiany metadanych wersji. 


## <a name="remarks"></a>Uwagi

- Właściwość Edition maszyny wirtualnej SQL Server musi być zgodna z wersją wystąpienia SQL Server zainstalowaną dla wszystkich SQL Server maszyn wirtualnych, w tym z opcją płatność zgodnie z rzeczywistym użyciem i w ramach licencji na własne typy licencji.
- W przypadku porzucenia SQL Server zasobów maszyny wirtualnej powrócisz do ustawienia kodowanej wersji obrazu.
- Możliwość zmiany wersji to funkcja dostawcy zasobów maszyny wirtualnej SQL. Wdrożenie obrazu portalu Azure Marketplace za pomocą Azure Portal powoduje automatyczne zarejestrowanie SQL Serverj maszyny wirtualnej przy użyciu dostawcy zasobów. Jednak klienci, którzy samodzielnie instalują SQL Server, będą musieli ręcznie [zarejestrować swoją SQL Serverą maszynę wirtualną](sql-vm-resource-provider-register.md).
- Dodanie maszyny wirtualnej SQL Server do zestawu dostępności wymaga ponownego utworzenia maszyny wirtualnej. Wszystkie maszyny wirtualne dodane do zestawu dostępności pozostaną z powrotem do wersji domyślnej, a wydanie należy ponownie zmodyfikować.

## <a name="next-steps"></a>Następne kroki

Aby uzyskać więcej informacji, zobacz następujące artykuły: 

* [Omówienie SQL Server na maszynie wirtualnej z systemem Windows](sql-server-on-azure-vm-iaas-what-is-overview.md)
* [Często zadawane pytania dotyczące SQL Server na maszynie wirtualnej z systemem Windows](frequently-asked-questions-faq.md)
* [Wskazówki dotyczące cen dla SQL Server na maszynie wirtualnej z systemem Windows](pricing-guidance.md)
* [Informacje o wersji SQL Server na maszynie wirtualnej z systemem Windows](doc-changes-updates-release-notes.md)


