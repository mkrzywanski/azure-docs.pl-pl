---
title: Zarządzanie serwerem Azure Portal-Azure Database for MariaDB
description: Dowiedz się, jak zarządzać serwerem Azure Database for MariaDB przy użyciu Azure Portal.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: how-to
ms.date: 3/18/2020
ms.openlocfilehash: 196b1dda17de1c9430ab048ac90d1d10b6a595b4
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/08/2020
ms.locfileid: "86104694"
---
# <a name="manage-an-azure-database-for-mariadb-server-using-the-azure-portal"></a>Zarządzanie serwerem Azure Database for MariaDB przy użyciu Azure Portal
W tym artykule pokazano, jak zarządzać serwerami Azure Database for MariaDB. Zadania zarządzania obejmują skalowanie obliczeniowe i magazynowe, Resetowanie hasła administratora oraz wyświetlanie szczegółów serwera.

## <a name="sign-in"></a>Zaloguj
Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).

## <a name="create-a-server"></a>Tworzenie serwera
Przejdź do [przewodnika Szybki Start](quickstart-create-mariadb-server-database-using-azure-portal.md) , aby dowiedzieć się, jak utworzyć serwer Azure Database for MariaDB i rozpocząć pracę z nim.

## <a name="scale-compute-and-storage"></a>Skalowanie zasobów obliczeniowych i magazynu

Po utworzeniu serwera można skalować warstwy Ogólnego przeznaczenia i zoptymalizowane pod kątem pamięci stosownie do potrzeb. Możesz również skalować obliczenia i pamięć, zwiększając lub zmniejszając rdzeni wirtualnych. Magazyn można skalować w górę (nie można jednak skalować w dół).

### <a name="scale-between-general-purpose-and-memory-optimized-tiers"></a>Skalowanie między Ogólnego przeznaczenia i warstw zoptymalizowanych pod kątem pamięci

Możesz skalować od Ogólnego przeznaczenia do zoptymalizowanej pod kątem pamięci i na odwrót. Zmiana na i z warstwy Podstawowa po utworzeniu serwera nie jest obsługiwana. 

1. Wybierz serwer w Azure Portal. Wybierz pozycję **warstwa cenowa**znajdująca się w sekcji **Ustawienia** .

2. Wybierz **ogólnego przeznaczenia** lub **zoptymalizowane pod kątem pamięci**, w zależności od tego, na czym polegasz skalowaniu. 

    ![zmiana ceny — warstwa](./media/howto-create-manage-server-portal/change-pricing-tier.png)

    > [!NOTE]
    > Zmiana warstw powoduje ponowne uruchomienie serwera.

4. Wybierz **przycisk OK** , aby zapisać zmiany.


### <a name="scale-vcores-up-or-down"></a>Skalowanie rdzeni wirtualnych w górę lub w dół

1. Wybierz serwer w Azure Portal. Wybierz pozycję **warstwa cenowa**znajdująca się w sekcji **Ustawienia** .

2. Zmień ustawienie **rdzeń wirtualny** , przesuwając suwak do żądanej wartości.

    ![Skalowanie — obliczenia](./media/howto-create-manage-server-portal/scaling-compute.png)

    > [!NOTE]
    > Skalowanie rdzeni wirtualnych powoduje ponowne uruchomienie serwera.

3. Wybierz **przycisk OK** , aby zapisać zmiany.


### <a name="scale-storage-up"></a>Skalowanie w górę

1. Wybierz serwer w Azure Portal. Wybierz pozycję **warstwa cenowa**znajdująca się w sekcji **Ustawienia** .

2. Zmień ustawienie **magazynu** , przesuwając suwak w górę do żądanej wartości.

    ![Skalowanie magazynu](./media/howto-create-manage-server-portal/scaling-storage.png)

    > [!NOTE]
    > Nie można skalować magazynu.

3. Wybierz **przycisk OK** , aby zapisać zmiany.


## <a name="update-admin-password"></a>Aktualizowanie hasła administratora
Hasło roli administratora można zmienić przy użyciu Azure Portal.

1. Wybierz serwer w Azure Portal. W oknie **Przegląd** wybierz pozycję **Resetuj hasło**.

   ![overview](./media/howto-create-manage-server-portal/overview-reset-password.png)

2. Wprowadź nowe hasło i Potwierdź hasło. W polu tekstowym zostanie wyświetlony monit o podanie wymagań dotyczących złożoności haseł.

   ![Resetowanie hasła](./media/howto-create-manage-server-portal/reset-password.png)

3. Wybierz **przycisk OK** , aby zapisać nowe hasło.


## <a name="delete-a-server"></a>Usuwanie serwera

Serwer można usunąć, jeśli nie jest już potrzebny. 

1. Wybierz serwer w Azure Portal. W oknie **Przegląd** wybierz pozycję **Usuń**.

    ![delete](./media/howto-create-manage-server-portal/overview-delete.png)

2. Wpisz nazwę serwera w polu wejściowym, aby upewnić się, że jest to serwer, który chcesz usunąć.

    ![potwierdzenie usunięcia](./media/howto-create-manage-server-portal/confirm-delete.png)

    > [!NOTE]
    > Usuwanie serwera jest nieodwracalne.

3. Wybierz pozycję **Usuń**.


## <a name="next-steps"></a>Następne kroki
- Informacje o [kopiach zapasowych i przywracaniu serwera](howto-restore-server-portal.md)
- Informacje o [opcjach dostrajania i monitorowania w Azure Database for MariaDB](concepts-monitoring.md)
