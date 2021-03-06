---
title: Nawiązywanie połączenia z serwerem SFTP przy użyciu protokołu SSH
description: Automatyzowanie zadań, które monitorują, tworzą, zarządzają, wysyłają i odbierają pliki dla serwera SFTP przy użyciu protokołu SSH i Azure Logic Apps
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.reviewer: estfan, logicappspm
ms.topic: article
ms.date: 07/20/2020
tags: connectors
ms.openlocfilehash: f3de582ff69dbd57aa4692fd5c3901602569cf9e
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/28/2020
ms.locfileid: "87286618"
---
# <a name="monitor-create-and-manage-sftp-files-by-using-ssh-and-azure-logic-apps"></a>Monitorowanie i tworzenie plików SFTP oraz zarządzanie nimi za pomocą protokołu SSH i usługi Azure Logic Apps

Aby zautomatyzować zadania, które monitorują, tworzą, wysyłają i odbierają pliki na serwerze [zabezpieczonym protokół transferu plików (SFTP)](https://www.ssh.com/ssh/sftp/) przy użyciu protokołu [Secure Shell (SSH)](https://www.ssh.com/ssh/protocol/) , można tworzyć i automatyzować przepływy pracy integracji przy użyciu Azure Logic Apps oraz łącznika SFTP-SSH. SFTP to protokół sieciowy, który zapewnia dostęp do plików, transfer plików i zarządzanie plikami za pośrednictwem dowolnego niezawodnego strumienia danych.

> [!NOTE]
> Łącznik SFTP-SSH aktualnie nie obsługuje tych serwerów SFTP:
> 
> * IBM datapowershell
> * MessageWay
> * OpenText Secure MFT
> * OpenText GXS

Poniżej przedstawiono kilka przykładowych zadań, które można zautomatyzować:

* Monitoruj, gdy pliki są dodawane lub zmieniane.
* Pobieranie, tworzenie, kopiowanie, zmienianie nazwy, aktualizowanie, wyświetlanie i usuwanie plików.
* Tworzenie folderów.
* Pobieranie zawartości i metadanych pliku.
* Wyodrębnij archiwa do folderów.

Można użyć wyzwalaczy, które monitorują zdarzenia na serwerze SFTP i udostępniają dane wyjściowe dla innych akcji. Możesz użyć akcji, które wykonują różne zadania na serwerze SFTP. Możesz również mieć inne akcje w aplikacji logiki, używając danych wyjściowych z akcji SFTP. Na przykład w przypadku regularnego pobierania plików z serwera SFTP można wysyłać alerty e-mail dotyczące tych plików i ich zawartości przy użyciu łącznika programu Office 365 Outlook lub łącznika Outlook.com. Jeśli jesteś nowym sposobem logiki aplikacji, zapoznaj [się z tematem Azure Logic Apps?](../logic-apps/logic-apps-overview.md)

Aby uzyskać różnice między łącznikiem protokołu SFTP-SSH a łącznikiem SFTP, zapoznaj się z sekcją [porównanie protokołu SFTP-SSH i SFTP](#comparison) w dalszej części tego tematu.

## <a name="limits"></a>Limity

* W przypadku akcji protokołu SFTP-SSH, które obsługują dzielenie może obsłużyć pliki o rozmiarze do 1 GB, natomiast akcje SFTP-SSH, które nie obsługują fragmentów [, mogą obsługiwać](../logic-apps/logic-apps-handle-large-messages.md) pliki do 50 MB. Mimo że domyślny rozmiar fragmentu to 15 MB, ten rozmiar można dynamicznie zmieniać, rozpoczynając od 5 MB i stopniowo zwiększając do 50 MB, na podstawie takich czynników, jak opóźnienie sieci, czas odpowiedzi serwera i tak dalej.

  > [!NOTE]
  > W przypadku aplikacji logiki w [środowisku usługi integracji (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md), wersja tego łącznika z oznaczeniem ISE wymaga, aby w zamian używały [limitów komunikatów ISE](../logic-apps/logic-apps-limits-and-config.md#message-size-limits) .

  Można zastąpić to zachowanie adaptacyjne w przypadku [określenia stałego rozmiaru fragmentu](#change-chunk-size) do użycia. Ten rozmiar może nawiązać od 5 MB do 50 MB. Załóżmy na przykład, że masz plik 45 MB i sieć, która może obsługiwać ten rozmiar plików bez opóźnień. Adaptacyjne rozdzielenie skutkuje wieloma wywołaniami, a tym samym wywołaniem. Aby zmniejszyć liczbę wywołań, można spróbować ustawić rozmiar fragmentu 50 MB. W innym scenariuszu, jeśli aplikacja logiki ma limit czasu, na przykład w przypadku używania fragmentów 15 MB można spróbować zmniejszyć rozmiar do 5 MB.

  Rozmiar fragmentu jest skojarzony z połączeniem, co oznacza, że można użyć tego samego połączenia dla akcji, które obsługują rozdzielenie, a następnie dla akcji, które nie obsługują rozdzielania. W tym przypadku rozmiar fragmentu dla akcji, które nie obsługują zakresów podziału z 5 MB do 50 MB. W tej tabeli przedstawiono, które działania SFTP obsługują fragmenty:

  | Akcja | Obsługa fragmentów | Przesłoń obsługę rozmiaru fragmentu |
  |--------|------------------|-----------------------------|
  | **Kopiuj plik** | Nie | Nie dotyczy |
  | **Utwórz plik** | Tak | Tak |
  | **Utwórz folder** | Nie dotyczy | Nie dotyczy |
  | **Usuń plik** | Nie dotyczy | Nie dotyczy |
  | **Wyodrębnij archiwum do folderu** | Nie dotyczy | Nie dotyczy |
  | **Pobierz zawartość pliku** | Tak | Tak |
  | **Pobierz zawartość pliku przy użyciu ścieżki** | Tak | Tak |
  | **Pobierz metadane pliku** | Nie dotyczy | Nie dotyczy |
  | **Pobierz metadane pliku przy użyciu ścieżki** | Nie dotyczy | Nie dotyczy |
  | **Wyświetl listę plików w folderze** | Nie dotyczy | Nie dotyczy |
  | **Zmień nazwę pliku** | Nie dotyczy | Nie dotyczy |
  | **Plik aktualizacji** | Nie | Nie dotyczy |
  ||||

* Protokół SFTP-SSH wyzwalacze nie obsługują fragmentacji komunikatów. Podczas żądania zawartości pliku wyzwalane są tylko pliki o rozmiarze 15 MB lub mniejszej. Aby uzyskać pliki o rozmiarze większym niż 15 MB, użyj tego wzorca:

  1. Użyj wyzwalacza SFTP-SSH zwracającego tylko właściwości pliku, na przykład **gdy plik jest dodawany lub modyfikowany (tylko właściwości)**.

  1. Śledź wyzwalacz z akcją **Pobierz zawartość pliku** SFTP-SSH, która odczytuje kompletny plik i niejawnie używa fragmenty komunikatów.

<a name="comparison"></a>

## <a name="compare-sftp-ssh-versus-sftp"></a>Porównanie protokołów SFTP-SSH i SFTP

Poniżej przedstawiono inne kluczowe różnice między łącznikiem SFTP-SSH a łącznikiem SFTP, gdzie łącznik SFTP-SSH ma następujące możliwości:

* Używa [biblioteki SSH.NET](https://github.com/sshnet/SSH.NET), która jest biblioteką Secure Shell (SSH), która obsługuje platformę .NET.

* Udostępnia akcję **Utwórz folder** , która tworzy folder w określonej ścieżce na serwerze SFTP.

* Udostępnia akcję **zmiany nazwy pliku** , która zmienia nazwę pliku na serwerze SFTP.

* Buforuje połączenie z serwerem SFTP *przez maksymalnie 1 godzinę*, co zwiększa wydajność i zmniejsza liczbę prób nawiązania połączenia z serwerem. Aby ustawić czas trwania tego zachowania buforowania, Edytuj Właściwość [**ClientAliveInterval**](https://man.openbsd.org/sshd_config#ClientAliveInterval) w konfiguracji SSH na serwerze SFTP.

## <a name="prerequisites"></a>Wymagania wstępne

* Subskrypcja platformy Azure. Jeśli nie masz subskrypcji platformy Azure, [zarejestruj się w celu założenia bezpłatnego konta platformy Azure](https://azure.microsoft.com/free/).

* Adres serwera SFTP i poświadczenia konta, które umożliwiają aplikacji logiki dostęp do Twojego konta SFTP. Wymagany jest również dostęp do prywatnego klucza SSH oraz hasła prywatnego klucza SSH. Aby można było używać fragmentów podczas przekazywania dużych plików, wymagane są uprawnienia do odczytu i zapisu dla folderu głównego na serwerze SFTP. W przeciwnym razie zostanie wyświetlony komunikat o błędzie "401 bez autoryzacji".

  > [!IMPORTANT]
  >
  > Łącznik SFTP-SSH obsługuje *tylko* te formaty kluczy prywatnych, algorytmy i odciski palców:
  >
  > * **Formaty kluczy prywatnych**: klucze RSA (Rivest Shamir Adleman) i DSA (algorytm podpisywania cyfrowego) w formatach OpenSSH i SSH.com. Jeśli klucz prywatny jest w formacie. PPK), najpierw [przekonwertuj klucz na format pliku OpenSSH (PEM)](#convert-to-openssh).
  >
  > * **Algorytmy szyfrowania**: des-EDE3-CBC, des-EDE3-CFB, des-CBC, aes-128-CBC, AES-192-CBC i AES-256-CBC
  >
  > * **Odcisk palca**: MD5
  >
  > Po dodaniu wyzwalacza SFTP-SSH lub akcji do aplikacji logiki należy podać informacje o połączeniu dla serwera SFTP. Po podaniu klucza prywatnego SSH dla tego połączenia ***nie wprowadzaj ręcznie ani nie edytuj klucza***, co może spowodować niepowodzenie połączenia. Zamiast tego należy ***skopiować klucz*** z pliku prywatnego klucza SSH i ***wkleić*** go do szczegółów połączenia. 
  > Aby uzyskać więcej informacji, zobacz sekcję [łączenie się](#connect) z PROTOKOŁem SSH w dalszej części tego artykułu.

* Podstawowa wiedza [na temat tworzenia aplikacji logiki](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* Aplikacja logiki, w której chcesz uzyskać dostęp do konta SFTP. Aby rozpocząć pracę z wyzwalaczem SFTP-SSH, [Utwórz pustą aplikację logiki](../logic-apps/quickstart-create-first-logic-app-workflow.md). Aby użyć akcji SFTP-SSH, uruchom aplikację logiki z innym wyzwalaczem, na przykład wyzwalaczem **cyklu** .

## <a name="how-sftp-ssh-triggers-work"></a>Jak działają wyzwalacze SFTP-SSH

Protokół SFTP-SSH wyzwalacze działają przez sondowanie systemu plików SFTP i wyszukiwanie plików, które zostały zmienione od czasu ostatniego sondowania. Niektóre narzędzia pozwalają zachować sygnaturę czasową, gdy pliki zmienią się. W takich przypadkach należy wyłączyć tę funkcję, aby wyzwalacz mógł funkcjonować. Poniżej przedstawiono niektóre typowe ustawienia:

| Klient SFTP | Akcja |
|-------------|--------|
| WinSCP | Przejdź do pozycji **Opcje**  >  **Preferencje**  >  **transfer**  >  **Edycja**  >  **Zachowaj sygnaturę czasową**  >  **Wyłącz** |
| FileZilla | Przejdź do **transferu**  >  **Zachowaj sygnatury czasowe transferowanych plików**  >  **wyłączone** |
|||

Gdy wyzwalacz odnajdzie nowy plik, wyzwalacz sprawdza, czy nowy plik jest zakończony i nie jest częściowo zapisany. Na przykład plik może mieć zmiany w toku, gdy wyzwalacz sprawdza serwer plików. Aby uniknąć powrotu częściowo zapisywanego pliku, wyzwalacz odnotowuje sygnaturę czasową dla pliku, który ma ostatnio wprowadzone zmiany, ale nie zwraca natychmiast tego pliku. Wyzwalacz zwraca plik tylko wtedy, gdy ponownie sonduje serwer. Czasami takie zachowanie może spowodować opóźnienie, który jest maksymalnie dwa razy interwał sondowania wyzwalacza.

<a name="convert-to-openssh"></a>

## <a name="convert-putty-based-key-to-openssh"></a>Konwertuj wyskakujące klucze na OpenSSH

Jeśli klucz prywatny jest w formacie pobierania, który używa rozszerzenia nazwy pliku. ppk (podano klucz prywatny), najpierw przekonwertuj klucz do formatu OpenSSH, który używa rozszerzenia nazwy pliku PEM (Privacy Enhanced Mail).

### <a name="unix-based-os"></a>System operacyjny oparty na systemie UNIX

1. Jeśli w systemie nie zainstalowano narzędzi do prezentacji, zrób to teraz, na przykład:

   `sudo apt-get install -y putty`

1. Uruchom to polecenie, które tworzy plik, którego można użyć z łącznikiem SFTP-SSH:

   `puttygen <path-to-private-key-file-in-PuTTY-format> -O private-openssh -o <path-to-private-key-file-in-OpenSSH-format>`

   Na przykład:

   `puttygen /tmp/sftp/my-private-key-putty.ppk -O private-openssh -o /tmp/sftp/my-private-key-openssh.pem`

### <a name="windows-os"></a>System operacyjny Windows

1. Jeśli jeszcze tego nie zrobiono, [Pobierz najnowsze narzędzie do generowania generatora (puttygen.exe)](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html), a następnie uruchom narzędzie.

1. Na tym ekranie wybierz pozycję **Załaduj**.

   ![Wybierz pozycję "Załaduj"](./media/connectors-sftp-ssh/puttygen-load.png)

1. Przejdź do pliku klucza prywatnego w formacie podano i wybierz pozycję **Otwórz**.

1. Z menu **konwersje** wybierz pozycję **Eksportuj klucz OpenSSH**.

   ![Wybierz pozycję "Eksportuj klucz OpenSSH"](./media/connectors-sftp-ssh/export-openssh-key.png)

1. Zapisz plik klucza prywatnego z `.pem` rozszerzeniem nazwy pliku.

## <a name="considerations"></a>Zagadnienia do rozważenia

W tej sekcji opisano zagadnienia dotyczące wyzwalaczy i akcji tego łącznika.

<a name="create-file"></a>

### <a name="create-file"></a>Utwórz plik

Aby utworzyć plik na serwerze SFTP, możesz skorzystać z akcji **Utwórz plik** SFTP-SSH. Gdy ta akcja spowoduje utworzenie pliku, usługa Logic Apps automatycznie wywoła serwer SFTP w celu pobrania metadanych pliku. Jeśli jednak przeniesiesz nowo utworzony plik, zanim usługa Logic Apps będzie mogła nawiązać wywołanie w celu uzyskania metadanych, zostanie wyświetlony `404` komunikat o błędzie `'A reference was made to a file or folder which does not exist'` . Aby pominąć odczytywanie metadanych pliku po utworzeniu pliku, wykonaj kroki, aby [dodać i ustawić właściwość **Pobierz wszystkie metadane pliku** na wartość **nie**](#file-does-not-exist).

<a name="connect"></a>

## <a name="connect-to-sftp-with-ssh"></a>Nawiązywanie połączenia z protokołem SFTP przy użyciu protokołu SSH

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

1. Zaloguj się do [Azure Portal](https://portal.azure.com)i Otwórz aplikację logiki w Projektancie aplikacji logiki, jeśli nie jest jeszcze otwarta.

1. W przypadku pustych aplikacji logiki w polu wyszukiwania wprowadź `sftp ssh` jako filtr. Na liście Wyzwalacze wybierz wyzwalacz, który chcesz.

   -lub-

   W przypadku istniejących aplikacji logiki w ostatnim kroku, w którym chcesz dodać akcję, wybierz pozycję **nowy krok**. W polu wyszukiwania wprowadź `sftp ssh` jako filtr. Na liście Akcje wybierz żądaną akcję.

   Aby dodać akcję między krokami, przesuń wskaźnik myszy nad strzałkę między krokami. Wybierz wyświetlony znak plus ( **+** ), a następnie wybierz pozycję **Dodaj akcję**.

1. Podaj niezbędne szczegóły dotyczące połączenia.

   > [!IMPORTANT]
   >
   > Po wprowadzeniu klucza prywatnego SSH we właściwości **prywatnego klucza SSH** wykonaj te dodatkowe kroki, aby upewnić się, że podajesz pełną i poprawną wartość dla tej właściwości. Nieprawidłowy klucz powoduje niepowodzenie połączenia.

   Mimo że można użyć dowolnego edytora tekstu, poniżej przedstawiono przykładowe kroki, które pokazują, jak poprawnie skopiować i wkleić klucz przy użyciu Notepad.exe na przykład.

   1. Otwórz plik klucza prywatnego SSH w edytorze tekstu. W tych krokach użyto Notatnika jako przykładu.

   1. W menu **Edycja** Notatnik wybierz pozycję **Zaznacz wszystko**.

   1. Wybierz pozycję **Edytuj**  >  **kopię**.

   1. W wyzwalaczu SFTP-SSH lub akcji, który został dodany, wklej *pełny* klucz skopiowany do właściwości **prywatnego klucza SSH** , która obsługuje wiele wierszy.  ***Upewnij się, że wkleisz*** klucz. ***Nie wprowadzaj ręcznie ani nie edytuj klucza***.

1. Po zakończeniu wprowadzania szczegółów połączenia wybierz pozycję **Utwórz**.

1. Podaj teraz niezbędne szczegóły wybranego wyzwalacza lub akcji i Kontynuuj tworzenie przepływu pracy aplikacji logiki.

<a name="change-chunk-size"></a>

## <a name="override-chunk-size"></a>Przesłoń rozmiar fragmentu

Aby zastąpić domyślne zachowanie adaptacyjne używane do rozdzielania, można określić stały rozmiar fragmentu od 5 MB do 50 MB.

1. W prawym górnym rogu akcji wybierz przycisk wielokropka (**...**), a następnie wybierz pozycję **Ustawienia**.

   ![Otwórz ustawienia protokołu SFTP-SSH](./media/connectors-sftp-ssh/sftp-ssh-connector-setttings.png)

1. W obszarze **transfer zawartości**w właściwości **rozmiar fragmentu** wprowadź wartość całkowitą z `5` do `50` , na przykład: 

   ![Określ rozmiar fragmentu do użycia zamiast niego](./media/connectors-sftp-ssh/specify-chunk-size-override-default.png)

1. Po zakończeniu wybierz pozycję **Gotowe**.

## <a name="examples"></a>Przykłady

<a name="file-added-modified"></a>

### <a name="sftp---ssh-trigger-when-a-file-is-added-or-modified"></a>Wyzwalacz SFTP-SSH: po dodaniu lub zmodyfikowaniu pliku

Ten wyzwalacz uruchamia przepływ pracy aplikacji logiki, gdy na serwerze SFTP zostanie dodany lub zmieniony plik. Na przykład można dodać warunek, który sprawdza zawartość pliku i pobiera zawartość w zależności od tego, czy zawartość spełnia określony warunek. Następnie można dodać akcję, która pobiera zawartość pliku i umieszcza tę zawartość w folderze na serwerze SFTP.

**Przykład przedsiębiorstwa**: ten wyzwalacz służy do monitorowania folderu SFTP dla nowych plików reprezentujących zamówienia klienta. Następnie można użyć akcji SFTP, takiej jak **pobieranie zawartości pliku** , aby uzyskać zawartość zamówienia do dalszej obróbki i przechowywać ją w bazie danych zamówień.

<a name="get-content"></a>

### <a name="sftp---ssh-action-get-file-content-using-path"></a>Działanie SFTP-SSH: pobieranie zawartości pliku przy użyciu ścieżki

Ta akcja pobiera zawartość z pliku na serwerze SFTP przez określenie ścieżki pliku. Na przykład można dodać wyzwalacz z poprzedniego przykładu i warunek, który musi spełniać zawartość pliku. Jeśli warunek ma wartość true, Akcja, która pobiera zawartość, może zostać uruchomiona.

<a name="troubleshooting-errors"></a>

## <a name="troubleshoot-errors"></a>Rozwiązywanie problemów

W tej sekcji opisano możliwe rozwiązania typowych błędów lub problemów.

<a name="file-does-not-exist"></a>

### <a name="404-error-a-reference-was-made-to-a-file-or-folder-which-does-not-exist"></a>404 błąd: "odwołanie zostało wykonane do pliku lub folderu, który nie istnieje"

Ten błąd może wystąpić, gdy aplikacja logiki utworzy nowy plik na serwerze SFTP za pośrednictwem akcji **tworzenia pliku** SFTP-SSH, ale nowo utworzony plik zostanie natychmiast przeniesiony przed uzyskaniem metadanych pliku przez usługę Logic Apps. Gdy aplikacja logiki uruchamia akcję **Utwórz plik** , usługa Logic Apps automatycznie WYWOŁA serwer SFTP w celu pobrania metadanych pliku. Jeśli jednak plik zostanie przeniesiony, usługa Logic Apps nie będzie już znajdować pliku, więc zostanie wyświetlony `404` komunikat o błędzie.

Jeśli nie możesz uniknąć ani opóźnić przeniesienia pliku, możesz pominąć odczytywanie metadanych pliku po utworzeniu pliku, wykonując następujące czynności:

1. W akcji **Utwórz plik** Otwórz listę **Dodaj nowy parametr** , wybierz właściwość **Pobierz wszystkie metadane pliku** i ustaw wartość na **nie**.

1. Jeśli te metadane pliku są potrzebne później, można użyć akcji **Pobierz metadane pliku** .

## <a name="connector-reference"></a>Dokumentacja łączników

Aby uzyskać więcej szczegółowych informacji technicznych dotyczących tego łącznika, takich jak wyzwalacze, akcje i limity, zgodnie z opisem w pliku Swagger łącznika, zobacz [stronę odwołania łącznika](/connectors/sftpwithssh/).

> [!NOTE]
> W przypadku aplikacji logiki w [środowisku usługi integracji (ISE)](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md), wersja tego łącznika z oznaczeniem ISE, wymaga, aby fragmenty używały [limitów komunikatów ISE](../logic-apps/logic-apps-limits-and-config.md#message-size-limits) .

## <a name="next-steps"></a>Następne kroki

* Dowiedz się więcej na temat innych [łączników Logic Apps](../connectors/apis-list.md)

