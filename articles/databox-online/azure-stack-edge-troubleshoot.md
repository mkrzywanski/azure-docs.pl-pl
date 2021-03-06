---
title: Użyj Azure Portal, aby rozwiązywać problemy Azure Stack Edge | Microsoft Docs
description: Opisuje sposób rozwiązywania problemów z Azure Stack Edge.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: troubleshooting
ms.date: 03/15/2019
ms.author: alkohli
ms.openlocfilehash: 00aae3386b3f648eafdef31dccb640d8120f7ad8
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "85563996"
---
# <a name="troubleshoot-your-azure-stack-edge-issues"></a>Rozwiązywanie problemów z Azure Stack Edge

W tym artykule opisano sposób rozwiązywania problemów na Azure Stackej krawędzi. 

W tym artykule omówiono sposób wykonywania następujących zadań:

> [!div class="checklist"]
> * Uruchamianie diagnostyki
> * Zbieranie pakietu dla pomocy technicznej
> * Rozwiązywanie problemów przy użyciu dzienników


## <a name="run-diagnostics"></a>Uruchamianie diagnostyki

Diagnozowanie i rozwiązywanie problemów związanych z błędami urządzenia jest możliwe dzięki testom diagnostycznym. Wykonaj następujące czynności w lokalnym internetowym interfejsie użytkownika, aby uruchomić testy diagnostyczne.

1. W lokalnym internetowym interfejsie użytkownika wybierz kolejno pozycje **Rozwiązywanie problemów > Testy diagnostyczne**. Wybierz test, który chcesz uruchomić, a następnie kliknij przycisk **Uruchom test**. To spowoduje uruchomienie testów pozwalających zdiagnozować wszelkie możliwe problemy z siecią, urządzeniem, internetowym serwerem proxy, czasem lub ustawieniami chmury. Otrzymasz powiadomienie, że urządzenie wykonuje testy.

    ![Wybierz testy](media/azure-stack-edge-troubleshoot/run-diag-1.png)
 
2. Po zakończeniu wykonywania testów zostaną wyświetlone wyniki. 

    ![Przejrzyj wyniki testu](media/azure-stack-edge-troubleshoot/run-diag-2.png)

    Jeśli test zakończy się niepowodzeniem, zostanie wyświetlony adres URL zalecanej akcji. Możesz kliknąć adres URL, aby wyświetlić zalecaną akcję.
 
    ![Przejrzyj ostrzeżenia dotyczące testów zakończonych niepowodzeniem](media/azure-stack-edge-troubleshoot/run-diag-3.png)


## <a name="collect-support-package"></a>Zbieranie pakietu dla pomocy technicznej

Pakiet dziennika składa się ze wszystkich odpowiednich dzienników, które mogą okazać się przydatne dla pomocy technicznej firmy Microsoft podczas rozwiązywania różnego rodzaju problemów z urządzeniami. Pakiet dziennika można wygenerować za pomocą lokalnego internetowego interfejsu użytkownika.

Wykonaj następujące czynności, aby zebrać pakiet dla pomocy technicznej. 

1. W lokalnym internetowym interfejsie użytkownika wybierz kolejno pozycje **Rozwiązywanie problemów > Pomoc techniczna**. Kliknij przycisk **Utwórz pakiet dla pomocy technicznej**. System rozpoczyna zbieranie pakietu dla pomocy technicznej. Zbieranie pakietu może potrwać kilka minut.

    ![Klikanie przycisku Dodaj użytkownika](media/azure-stack-edge-troubleshoot/collect-logs-1.png)
 
2. Po utworzeniu pakietu dla pomocy technicznej kliknij przycisk **Pobierz pakiet dla pomocy technicznej**. Skompresowany pakiet zostanie pobrany w wybranej ścieżce. Można rozpakować pakiet i wyświetlić systemowe pliki dziennika.

    ![Klikanie przycisku Dodaj użytkownika](media/azure-stack-edge-troubleshoot/collect-logs-2.png)

## <a name="use-logs-to-troubleshoot"></a>Rozwiązywanie problemów przy użyciu dzienników

Wszelkie błędy występujące podczas procesów przekazywania i odświeżania są uwzględniane w odpowiednich plikach błędów.

1. Aby wyświetlić pliki błędów, przejdź do udziału i kliknij go w celu wyświetlenia jego zawartości. 

      ![Łączenie i wyświetlanie zawartości udziału](media/azure-stack-edge-troubleshoot/troubleshoot-logs-1.png)

2. Kliknij _folder Microsoft Azure Stack Edge_. Ten folder zawiera dwa podfoldery:

    - Folder przekazywania zawierający pliki dziennika w przypadku błędów przekazywania.
    - Folder odświeżania dla błędów podczas odświeżania.

    Oto przykładowy plik dziennika, który ma zostać odświeżony.

    ```
    <root container="test1" machine="VM15BS020663" timestamp="03/18/2019 00:11:10" />
    <file item="test.txt" local="False" remote="True" error="16001" />
    <summary runtime="00:00:00.0945320" errors="1" creates="2" deletes="0" insync="3" replaces="0" pending="9" />
    ``` 

3. W razie pojawienia się w tym pliku błędu (zaznaczonego w próbce), zanotuj jego kod — w tym przypadku jest to 16001. Sprawdź opis dla tego kodu błędu w odniesieniu do następujących informacji o błędzie.

    [!INCLUDE [data-box-edge-edge-upload-error-reference](../../includes/data-box-edge-gateway-upload-error-reference.md)]


## <a name="next-steps"></a>Następne kroki

- Dowiedz się więcej o [znanych problemach w tej wersji](data-box-gateway-release-notes.md).
