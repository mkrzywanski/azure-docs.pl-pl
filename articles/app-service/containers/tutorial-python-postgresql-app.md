---
title: 'Samouczek: wdrażanie aplikacji Django języka Python za pomocą Postgres'
description: Utwórz aplikację sieci Web w języku Python z bazą danych PostgreSQL i Wdróż ją na platformie Azure. Samouczek używa platformy Django Framework, a aplikacja jest hostowana w Azure App Service w systemie Linux.
ms.devlang: python
ms.topic: tutorial
ms.date: 07/22/2020
ms.custom:
- mvc
- seodec18
- seo-python-october2019
- cli-validate
- tracking-python
- devx-track-azurecli
ms.openlocfilehash: dfc7cf0a0d3b46020d94e86a3158b1e0cbf6ea5f
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/31/2020
ms.locfileid: "87485290"
---
# <a name="tutorial-deploy-a-django-web-app-with-postgresql-in-azure-app-service"></a>Samouczek: wdrażanie aplikacji sieci Web Django za pomocą PostgreSQL w Azure App Service

W tym samouczku przedstawiono sposób wdrażania aplikacji sieci Web w języku Python [Django](https://www.djangoproject.com/) opartej na danych w celu [Azure App Service](app-service-linux-intro.md) i łączenia jej z bazą danych usługi Azure Database for Postgres. App Service zapewnia wysoce skalowalną, samoobsługową usługę hostingu w sieci Web.

W tym samouczku użyjesz interfejsu wiersza polecenia platformy Azure, aby wykonać następujące zadania:

> [!div class="checklist"]
> * Konfigurowanie początkowego środowiska przy użyciu języka Python i interfejsu wiersza polecenia platformy Azure
> * Tworzenie bazy danych Azure Database for PostgreSQL
> * Wdróż kod do Azure App Service i Połącz się z usługą Postgres
> * Zaktualizuj kod i Wdróż ponownie
> * Wyświetlanie dzienników diagnostycznych
> * Zarządzanie aplikacją internetową w Azure Portal

## <a name="set-up-your-initial-environment"></a>Konfigurowanie środowiska początkowego

1. Posiadanie konta platformy Azure z aktywną subskrypcją. [Utwórz konto bezpłatnie](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).
1. Zainstaluj środowisko Python w wersji <a href="https://www.python.org/downloads/" target="_blank">3,6 lub nowszej</a>.
1. Zainstaluj <a href="/cli/azure/install-azure-cli" target="_blank">interfejs wiersza polecenia platformy Azure</a> 2.0.80 lub nowszy, za pomocą którego uruchamiasz polecenia w dowolnej powłoce, aby udostępnić i skonfigurować zasoby platformy Azure.

Otwórz okno terminalu i sprawdź, czy wersja języka Python to 3,6 lub nowszego:

# <a name="bash"></a>[Bash](#tab/bash)

```bash
python3 --version
```

# <a name="powershell"></a>[PowerShell](#tab/powershell)

```cmd
py -3 --version
```

# <a name="cmd"></a>[Cmd](#tab/cmd)

```cmd
py -3 --version
```

---

Sprawdź, czy wersja interfejsu wiersza polecenia platformy Azure to 2.0.80 lub nowszy:

```azurecli
az --version
```

Następnie zaloguj się do platformy Azure za pomocą interfejsu wiersza polecenia:

```azurecli
az login
```

To polecenie umożliwia otwarcie przeglądarki w celu zebrania poświadczeń. Po zakończeniu wykonywania polecenia zostaną wyświetlone dane wyjściowe JSON zawierające informacje o Twoich subskrypcjach.

Po zalogowaniu możesz uruchamiać polecenia platformy Azure za pomocą interfejsu wiersza polecenia platformy Azure, aby pracować z zasobami w ramach subskrypcji.


## <a name="clone-or-download-the-sample-app"></a>Klonowanie lub pobieranie przykładowej aplikacji

# <a name="git-clone"></a>[Klonowanie git](#tab/clone)

Klonuj repozytorium przykładowe:

```terminal
git clone https://github.com/Azure-Samples/djangoapp
```

Następnie przejdź do tego folderu:

```terminal
cd djangoapp
```

# <a name="download"></a>[Pobieranie](#tab/download)

Odwiedź stronę [https://github.com/Azure-Samples/djangoapp](https://github.com/Azure-Samples/djangoapp) , wybierz pozycję **Klonuj**, a następnie wybierz pozycję **Pobierz plik zip**. 

Rozpakuj plik ZIP do folderu o nazwie *djangoapp*. 

Następnie otwórz okno terminalu w tym folderze *djangoapp* .

---

Przykład djangoapp zawiera opartą na danych aplikację do sondowania Django, którą uzyskasz, wykonując [pisanie pierwszej aplikacji Django](https://docs.djangoproject.com/en/2.1/intro/tutorial01/) w dokumentacji Django. Ukończona aplikacja jest udostępniana w tym miejscu dla wygody użytkownika.

Przykład jest również modyfikowany do uruchamiania w środowisku produkcyjnym, takim jak App Service:

- Ustawienia produkcyjne znajdują się w pliku *azuresite/Production. PR* . Szczegóły dotyczące programowania znajdują się w *azuresite/Settings. PR*.
- Aplikacja używa ustawień produkcyjnych, gdy `DJANGO_ENV` zmienna środowiskowa jest ustawiona na "produkcja". Tę zmienną środowiskową utworzysz w dalszej części tego samouczka wraz z innymi osobami używanymi do konfiguracji bazy danych PostgreSQL.

Te zmiany są specyficzne dla konfigurowania Django do uruchamiania w dowolnym środowisku produkcyjnym i nie są szczególnie App Service. Aby uzyskać więcej informacji, zobacz [Lista kontrolna wdrożenia Django](https://docs.djangoproject.com/en/2.1/howto/deployment/checklist/).

## <a name="create-postgres-database-in-azure"></a>Tworzenie bazy danych Postgres na platformie Azure

<!-- > [!NOTE]
> Before you create an Azure Database for PostgreSQL server, check which [compute generation](/azure/postgresql/concepts-pricing-tiers#compute-generations-and-vcores) is available in your region. If your region doesn't support Gen4 hardware, change *--sku-name* in the following command line to a value that's supported in your region, such as B_Gen4_1.  -->

Zainstaluj `db-up` rozszerzenie dla interfejsu wiersza polecenia platformy Azure:

```azurecli
az extension add --name db-up
```

Jeśli `az` polecenie nie jest rozpoznawane, upewnij się, że masz zainstalowany interfejs wiersza polecenia platformy Azure zgodnie z opisem w artykule [Konfigurowanie początkowego środowiska](#set-up-your-initial-environment).

Następnie Utwórz bazę danych Postgres na platformie Azure przy użyciu [`az postgres up`](/cli/azure/ext/db-up/postgres#ext-db-up-az-postgres-up) polecenia:

```azurecli
az postgres up --resource-group DjangoPostgres-tutorial-rg --location westus2 --sku-name B_Gen4_1 --server-name <postgre-server-name> --database-name pollsdb --admin-user <admin-username> --admin-password <admin-password> --ssl-enforcement Enabled
```

- Zamień na *\<postgres-server-name>* nazwę, która jest unikatowa na wszystkich platformie Azure (punkt końcowy serwera to `https://\<postgres-server-name>.postgres.database.azure.com` ). Dobrym wzorcem jest użycie kombinacji nazwy firmy i innej unikatowej wartości.
- Dla programu *\<admin-username>* i *\<admin-password>* Określ poświadczenia, aby utworzyć użytkownika administratora dla tego serwera Postgres.
- Używana w tym miejscu [warstwa cenowa](../../postgresql/concepts-pricing-tiers.md) B_Gen4_1 (podstawowa, obliczenia, 1 rdzeń) jest tańsza. W przypadku produkcyjnych baz danych należy pominąć ten `--sku-name` argument, aby użyć warstwy GP_Gen5_2 (ogólnego przeznaczenia, generacji 5, 2 rdzenie).

To polecenie wykonuje następujące akcje, co może potrwać kilka minut:

- Utwórz [grupę zasobów](../../azure-resource-manager/management/overview.md#terminology) o nazwie `DjangoPostgres-tutorial-rg` , jeśli jeszcze nie istnieje.
- Utwórz serwer Postgres.
- Utwórz domyślne konto administratora z unikatową nazwą użytkownika i hasłem. (Aby określić własne poświadczenia, użyj `--admin-user` `--admin-password` argumentów i z `az postgres up` poleceniem).
- Utwórz `pollsdb` bazę danych.
- Włącz dostęp z lokalnego adresu IP.
- Włącz dostęp z poziomu usług platformy Azure.
- Utwórz użytkownika bazy danych z dostępem do `pollsdb` bazy danych.

Wszystkie kroki można wykonać oddzielnie z innymi `az postgres` `psql` poleceniami, ale `az postgres up` wszystkie kroki są wykonywane jednocześnie.

Po zakończeniu wykonywania polecenia generuje obiekt JSON, który zawiera różne parametry połączenia dla bazy danych wraz z adresem URL serwera, wygenerowaną nazwą użytkownika (taką jak " joyfulKoala@msdocs-djangodb-12345 ") i hasłem GUID. Skopiuj nazwę użytkownika i hasło do tymczasowego pliku tekstowego, które będą potrzebne w dalszej części tego samouczka.

<!-- not all locations support az postgres up -->
> [!TIP]
> `-l <location-name>`, można ustawić na jeden z [regionów świadczenia usługi Azure](https://azure.microsoft.com/global-infrastructure/regions/). Dostępne regiony można pobrać za pomocą [`az account list-locations`](/cli/azure/account#az-account-list-locations) polecenia. W przypadku aplikacji produkcyjnych Umieść swoją bazę danych i aplikację w tej samej lokalizacji.

## <a name="deploy-the-code-to-azure-app-service"></a>Wdróż kod w Azure App Service

W tej sekcji utworzysz hosta aplikacji w aplikacji App Service, Połącz tę aplikację z bazą danych Postgres, a następnie wdróż swój kod na tym hoście.

### <a name="create-the-app-service-app"></a>Tworzenie aplikacji App Service

Upewnij się, że jesteś w katalogu głównym repozytorium ( `djangoapp` ), który zawiera kod aplikacji.

Utwórz aplikację App Service (proces hosta) za pomocą [`az webapp up`](/cli/azure/webapp#az-webapp-up) polecenia:

```azurecli
az webapp up --resource-group DjangoPostgres-tutorial-rg --location westus2 --plan DjangoPostgres-tutorial-plan --sku B1 --name <app-name>
```
<!-- without --sku creates PremiumV2 plan -->

- Dla `--location` argumentu Użyj tej samej lokalizacji co w przypadku bazy danych w poprzedniej sekcji.
- Zamień na *\<app-name>* unikatową nazwę na całym systemie Azure (punkt końcowy serwera to `https://\<app-name>.azurewebsites.net` ). Dozwolone znaki dla *\<app-name>* są `A` - `Z` , `0` - `9` , i `-` . Dobrym wzorcem jest użycie kombinacji nazwy firmy i identyfikatora aplikacji.

To polecenie wykonuje następujące akcje, co może potrwać kilka minut:

<!-- - Create the resource group if it doesn't exist. `--resource-group` is optional. -->
<!-- No it doesn't. az webapp up doesn't respect --resource-group -->
- Utwórz [grupę zasobów](../../azure-resource-manager/management/overview.md#terminology) , jeśli jeszcze nie istnieje. (W tym poleceniu zostanie użyta ta sama Grupa zasobów, w której wcześniej została utworzona baza danych).
- Utwórz plan [App Service](../overview-hosting-plans.md) *DjangoPostgres — samouczek — plan* w warstwie cenowej Basic (B1), jeśli nie istnieje. `--plan`i `--sku` są opcjonalne.
- Utwórz aplikację App Service, jeśli nie istnieje.
- Włącz domyślne rejestrowanie dla aplikacji, jeśli nie została jeszcze włączona.
- Przekaż repozytorium przy użyciu wdrożenia ZIP z włączonym automatyzacją kompilacji.

Po pomyślnym wdrożeniu polecenie generuje dane wyjściowe JSON podobne do poniższego przykładu:

![Przykład AZ webapp up dane wyjściowe polecenia](./media/tutorial-python-postgresql-app/az-webapp-up-output.png)

> [!TIP]
> Wiele poleceń interfejsu wiersza polecenia platformy Azure buforuje typowe parametry, takie jak nazwa grupy zasobów i plan App Service, do pliku *. Azure/config*. W związku z tym nie trzeba określać wszystkich parametrów z nowszymi poleceniami. Na przykład aby ponownie wdrożyć aplikację po wprowadzeniu zmian, można po prostu uruchomić polecenie `az webapp up` bez żadnych parametrów. Polecenia, które pochodzą z rozszerzeń CLI, takie jak `az postgres up` , nie są już używane do korzystania z pamięci podręcznej, dlatego należy określić grupę zasobów i lokalizację w tym miejscu przy użyciu `az webapp up` .

> [!NOTE]
> Jeśli spróbujesz odwiedzić adres URL aplikacji w tym momencie, wystąpi błąd "DisallowedHost at/". Ten błąd występuje, ponieważ aplikacja nie została jeszcze skonfigurowana do używania ustawień produkcyjnych omówionych wcześniej w poniższej sekcji.

### <a name="configure-environment-variables-to-connect-the-database"></a>Konfigurowanie zmiennych środowiskowych w celu nawiązania połączenia z bazą danych

Po wdrożeniu kodu do App Service, następnym krokiem jest połączenie aplikacji z bazą danych Postgres na platformie Azure.

Kod aplikacji oczekuje na znalezienie informacji o bazie danych w wielu zmiennych środowiskowych. Aby ustawić zmienne środowiskowe w App Service, należy utworzyć "Ustawienia aplikacji" przy użyciu polecenia [AZ webapp config AppSettings Set](/cli/azure/webapp/config/appsettings#az-webapp-config-appsettings-set) .

```azurecli
az webapp config appsettings set --settings DJANGO_ENV="production" DBHOST="<postgres-server-name>.postgres.database.azure.com" DBNAME="pollsdb" DBUSER="<username>" DBPASS="<password>"
```

- Zamień na *\<postgres-server-name>* nazwę użytą wcześniej z `az postgres up` poleceniem.
- Zamień *\<username>* i *\<password>* z poświadczeniami, które zostały również wygenerowane przez polecenie.
- Nazwa grupy zasobów i aplikacji jest rysowana z wartości pamięci podręcznej w pliku *. Azure/config* .
- Polecenie tworzy ustawienia o nazwie `DJANGO_ENV` ,,, `DBHOST` `DBNAME` `DBUSER` i `DBPASS` zgodnie z oczekiwaniami w kodzie aplikacji.
- W kodzie w języku Python te ustawienia są dostępne jako zmienne środowiskowe z instrukcjami takimi jak `os.environ.get('DJANGO_ENV')` . Aby uzyskać więcej informacji, zobacz [dostęp do zmiennych środowiskowych](how-to-configure-python.md#access-environment-variables).

### <a name="run-django-database-migrations"></a>Uruchom migracje bazy danych Django

Migracja baz danych Django upewnij się, że schemat w PostgreSQL w bazie danych platformy Azure jest zgodny z tymi opisanymi w kodzie.

1. Otwórz sesję SSH w przeglądarce, przechodząc do *https:// \<app-name> . SCM.azurewebsites.NET/webssh/Host* i zaloguj się przy użyciu poświadczeń konta platformy Azure (nie poświadczeń serwera bazy danych).

1. W sesji SSH Uruchom następujące polecenia (można wkleić polecenia za pomocą **klawiszy CTRL** + **SHIFT** + **V**):

    ```bash
    cd site/wwwroot
    
    # Activate default virtual environment in App Service container
    source /antenv/bin/activate
    # Install packages
    pip install -r requirements.txt
    # Run database migrations
    python manage.py migrate
    # Create the super user (follow prompts)
    python manage.py createsuperuser
    ```
    
1. `createsuperuser`Polecenie prosi o poświadczenia administratora. Na potrzeby tego samouczka Użyj domyślnej nazwy użytkownika `root` , naciśnij klawisz **Enter** , aby adres e-mail pozostawić puste, a następnie wprowadź `Pollsdb1` hasło.
    
### <a name="create-a-poll-question-in-the-app"></a>Tworzenie pytania dotyczącego ankiety w aplikacji

1. W przeglądarce Otwórz adres URL *http: \/ / \<app-name> . azurewebsites.NET*. Aplikacja powinna wyświetlić komunikat "Brak dostępnych sondowań", ponieważ w bazie danych nie ma jeszcze określonych sond.

1. Przejdź do *protokołu http: \/ / \<app-name> . azurewebsites.NET/admin*. Zaloguj się przy użyciu poświadczeń administratora z poprzedniej sekcji ( `root` i `Pollsdb1` ). W obszarze **sondy**wybierz pozycję **Dodaj** obok pozycji **pytania** i Utwórz pytanie dotyczące ankiety z wybranymi opcjami.

1. Przejdź ponownie do *protokołu http: \/ / \<app-name> . azurewebsites.NET/* , aby potwierdzić, że pytania są teraz prezentowane użytkownikowi. Odpowiedz na pytania, na które chcesz wygenerować niektóre dane w bazie danych.

**Gratulacje!** Korzystasz z aplikacji sieci Web w języku Python Django w Azure App Service dla systemu Linux z aktywną bazą danych Postgres.

> [!NOTE]
> App Service wykrywa projekt Django, szukając pliku *WSGI.py* w każdym podfolderze, który `manage.py startproject` tworzy domyślnie. Gdy App Service odnajdzie ten plik, ładuje aplikację sieci Web Django. Aby uzyskać więcej informacji, zobacz [Konfigurowanie wbudowanego obrazu w języku Python](how-to-configure-python.md).


## <a name="make-code-changes-and-redeploy"></a>Wprowadzanie zmian w kodzie i ponowne wdrażanie

W tej sekcji wprowadzasz lokalne zmiany w aplikacji i ponownie Wdróż kod w App Service. W procesie należy skonfigurować środowisko wirtualne języka Python, które obsługuje bieżące prace.

### <a name="run-the-app-locally"></a>Lokalne uruchamianie aplikacji

W oknie terminalu uruchom następujące polecenia. Pamiętaj, aby podczas tworzenia administratora postępuj zgodnie z monitami:

# <a name="bash"></a>[bash](#tab/bash)

```bash
# Configure the Python virtual environment
python3 -m venv venv
source venv/bin/activate

# Install packages
pip install -r requirements.txt
# Run Django migrations
python manage.py migrate
# Create Django superuser (follow prompts)
python manage.py createsuperuser
# Run the dev server
python manage.py runserver
```

# <a name="powershell"></a>[PowerShell](#tab/powershell)

```powershell
# Configure the Python virtual environment
py -3 -m venv venv
Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned -Force
venv\scripts\activate

# Install packages
pip install -r requirements.txt
# Run Django migrations
python manage.py migrate
# Create Django superuser (follow prompts)
python manage.py createsuperuser
# Run the dev server
python manage.py runserver
```

# <a name="cmd"></a>[CMD](#tab/cmd)

```cmd
:: Configure the Python virtual environment
py -3 -m venv venv
venv\scripts\activate

:: Install packages
pip install -r requirements.txt
:: Run Django migrations
python manage.py migrate
:: Create Django superuser (follow prompts)
python manage.py createsuperuser
:: Run the dev server
python manage.py runserver
```
---

Po całkowitym załadowaniu aplikacji sieci Web serwer Django Development udostępnia adres URL aplikacji lokalnej w komunikacie "Uruchamianie serwera deweloperskiego pod adresem http://127.0.0.1:8000/ . Zamknij serwer przy użyciu kombinacji klawiszy CTRL-BREAK.

![Przykład Django serwera deweloperskiego](./media/tutorial-python-postgresql-app/django-dev-server-output.png)

Przetestuj aplikację lokalnie, wykonując następujące czynności:

1. Przejdź do *protokołu http: \/ /localhost: 8000* w przeglądarce, która powinna wyświetlać komunikat "Brak dostępnych sondowań". 

1. Przejdź do *protokołu http: \/ /localhost: 8000/admin* i zaloguj się przy użyciu utworzonego wcześniej konta administratora. W obszarze **sondy**ponownie wybierz pozycję **Dodaj** obok pozycji **pytania** i Utwórz pytanie dotyczące ankiety z wybranymi opcjami. 

1. Przejdź ponownie do *protokołu http: \/ /localhost: 8000* i Odpowiedz na pytanie, aby przetestować aplikację. 

1. Zatrzymaj serwer Django, naciskając klawisz **Ctrl** + **C**.

W przypadku uruchamiania lokalnego aplikacja korzysta z lokalnej bazy danych Sqlite3 i nie zakłóca pracy w produkcyjnej bazie danych. W razie potrzeby można również użyć lokalnej bazy danych PostgreSQL, aby lepiej symulować środowisko produkcyjne.

### <a name="update-the-app"></a>Aktualizowanie aplikacji

W programie `polls/models.py` Znajdź wiersz zaczynający się od `choice_text` i Zmień `max_length` parametr na 100:

```python
# Find this lie of code and set max_length to 100 instead of 200
choice_text = models.CharField(max_length=100)
```

Ponieważ został zmieniony model danych, Utwórz nową migrację Django i Przeprowadź migrację bazy danych:

```
python manage.py makemigrations
python manage.py migrate
```

Uruchom ponownie serwer programistyczny `python manage.py runserver` i przetestuj aplikację pod adresem *http: \/ /localhost: 8000/admin*:

Zatrzymaj ponownie serwer sieci Web Django za pomocą **klawisza Ctrl** + **C**.

### <a name="redeploy-the-code-to-azure"></a>Ponowne wdrażanie kodu na platformie Azure

Uruchom następujące polecenie w katalogu głównym repozytorium:

```azurecli
az webapp up
```

To polecenie używa parametrów w pamięci podręcznej w pliku *. Azure/config* . Ponieważ App Service wykryje, że aplikacja już istnieje, po prostu ponownie wdraża kod.

### <a name="rerun-migrations-in-azure"></a>Ponowne uruchamianie migracji na platformie Azure

Ze względu na to, że wprowadzono zmiany w modelu danych, należy ponownie uruchomić migracje bazy danych w App Service.

Otwórz ponownie sesję SSH w przeglądarce, przechodząc do *https:// \<app-name> . SCM.azurewebsites.NET/webssh/Host*. Następnie uruchom następujące polecenia:

```
cd site/wwwroot

# Activate default virtual environment in App Service container
source /antenv/bin/activate
# Run database migrations
python manage.py migrate
```

### <a name="review-app-in-production"></a>Przeglądanie aplikacji w środowisku produkcyjnym

Przejdź do *protokołu http: \/ / \<app-name> . azurewebsites.NET* i przetestuj aplikację ponownie w środowisku produkcyjnym. (Ponieważ zmieniono tylko długość pola bazy danych, zmiana jest zauważalna tylko wtedy, gdy użytkownik spróbuje wprowadzić dłuższy czas odpowiedzi podczas tworzenia pytania).

## <a name="stream-diagnostic-logs"></a>Przesyłanie strumieniowe dzienników diagnostycznych

Możesz uzyskać dostęp do dzienników konsoli wygenerowanych z wewnątrz kontenera, który hostuje aplikację na platformie Azure.

Uruchom następujące polecenie interfejsu wiersza polecenia platformy Azure, aby wyświetlić strumień dziennika. To polecenie używa parametrów w pamięci podręcznej w pliku *. Azure/konfiguracji* .

```azurecli
az webapp log tail
```

Jeśli nie widzisz dzienników konsoli, sprawdź ponownie w ciągu 30 sekund.

Aby zatrzymać przesyłanie strumieniowe dzienników w dowolnym momencie, wpisz **Ctrl** + **C**.

> [!NOTE]
> Możesz również sprawdzić pliki dziennika z przeglądarki pod adresem `https://<app-name>.scm.azurewebsites.net/api/logs/docker` .
>
> `az webapp up`włącza opcję rejestrowania domyślnego. Ze względu na wydajność to rejestrowanie wyłącza się po pewnym czasie, ale włącza się ponownie przy każdym uruchomieniu `az webapp up` . Aby włączyć tę funkcję ręcznie, uruchom następujące polecenie:
>
> ```azurecli
> az webapp log config --docker-container-logging filesystem
> ```

## <a name="manage-your-app-in-the-azure-portal"></a>Zarządzanie aplikacją w Azure Portal

W [Azure Portal](https://portal.azure.com)Wyszukaj nazwę aplikacji i wybierz aplikację w wynikach.

![Przejdź do aplikacji w języku Python Django w Azure Portal](./media/tutorial-python-postgresql-app/navigate-to-django-app-in-app-services-in-the-azure-portal.png)

Domyślnie w portalu jest wyświetlana strona **omówienia** aplikacji, która zawiera ogólny widok wydajności. W tym miejscu można również wykonywać podstawowe zadania zarządzania, takie jak przeglądanie, zatrzymywanie, ponowne uruchamianie i usuwanie. Na kartach po lewej stronie strony są pokazane poszczególne strony konfiguracji, które można otworzyć.

![Zarządzanie aplikacją Django języka Python na stronie Przegląd w Azure Portal](./media/tutorial-python-postgresql-app/manage-django-app-in-app-services-in-the-azure-portal.png)

## <a name="clean-up-resources"></a>Czyszczenie zasobów

Jeśli chcesz zachować aplikację lub przejść do następnego samouczka, przejdź do [sekcji Następne kroki](#next-steps). W przeciwnym razie, aby uniknąć ponoszenia opłat, możesz usunąć grupę zasobów utworzoną dla tego samouczka:

```azurecli
az group delete
```

Polecenie używa nazwy grupy zasobów zapisanej w pamięci podręcznej w pliku *. Azure/config* . Usunięcie grupy zasobów spowoduje również cofnięcie alokacji i usunięcie wszystkich znajdujących się w niej zasobów.

## <a name="next-steps"></a>Następne kroki

Dowiedz się, jak zmapować niestandardową nazwę DNS na aplikację:

> [!div class="nextstepaction"]
> [Samouczek: mapowanie niestandardowej nazwy DNS na aplikację](../app-service-web-tutorial-custom-domain.md)

Dowiedz się, jak App Service uruchamia aplikację w języku Python:

> [!div class="nextstepaction"]
> [Konfigurowanie aplikacji języka Python](how-to-configure-python.md)
