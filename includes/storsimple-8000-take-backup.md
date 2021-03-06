---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: 48a3326dbe0e9eed4a5490e720248555586d189c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "67183283"
---
### <a name="to-take-a-backup"></a>Aby utworzyć kopię zapasową

1. Przejdź do usługi Menedżer urządzeń StorSimple. Z tabelarycznej listy urządzeń wybierz i kliknij swoje urządzenie, a następnie kliknij pozycję **Wszystkie urządzenia**. W bloku **Ustawienia** przejdź do pozycji **Ustawienia > Zarządzaj > Zasady kopii zapasowych**.

    ![Dodawanie zasad tworzenia kopii zapasowej](./media/storsimple-8000-take-backup/step8takebu1.png)

2. W bloku **Zasady kopii zapasowych** kliknij pozycję **+ Dodaj zasady**.

    ![Dodawanie zasad tworzenia kopii zapasowej](./media/storsimple-8000-take-backup/step8takebu2.png)

3. W bloku **Określanie zasad tworzenia kopii zapasowych** podaj nazwę zasad tworzenia kopii zapasowej składającą się z 3–150 znaków.

4. Wybierz woluminy do wykonania kopii zapasowej. W przypadku wybrania więcej niż jednego woluminu woluminy te są grupowane w celu utworzenia kopii zapasowej spójnej na poziomie awarii.

    ![Dodawanie zasad tworzenia kopii zapasowej](./media/storsimple-8000-take-backup/step8takebu4.png)

5. W bloku **Dodaj pierwszy harmonogram**:

    1. Wybierz typ kopii zapasowej. Aby przyspieszyć przywracanie, wybierz pozycję migawka **lokalna** . W celu odporności danych wybierz pozycję migawka w **chmurze** .
    2. Określ częstotliwość wykonywania kopii zapasowych w minutach, godzinach, dniach lub tygodniach.
    3. Wybierz czas przechowywania. Opcje przechowywania zależą od częstotliwości tworzenia kopii zapasowej. Na przykład w przypadku zasad dziennych można określić zasady przechowywania w tygodniach, natomiast czas przechowywania w przypadku zasad miesięcznych jest wyrażany w miesiącach.
    4. Wybierz datę i godzinę rozpoczęcia dla zasad tworzenia kopii zapasowej.
    5. Kliknij pozycję **OK**, aby utworzyć zasady kopii zapasowych.

        ![Dodawanie zasad tworzenia kopii zapasowej](./media/storsimple-8000-take-backup/step8takebu5.png) 

6. Kliknij przycisk **Utwórz**, aby zacząć tworzenie zasad kopii zapasowych. Otrzymasz powiadomienie o pomyślnym utworzeniu zasad kopii zapasowych. Lista zasad kopii zapasowych także zostanie zaktualizowana.
      
      ![Dodawanie zasad tworzenia kopii zapasowej](./media/storsimple-8000-take-backup/step8takebu9.png)
      
      Masz teraz zasady kopii zapasowych, które będą powodować tworzenie zaplanowanych kopii zapasowych danych woluminu.




