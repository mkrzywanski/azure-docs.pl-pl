---
title: 'Szybki Start: Tworzenie aplikacji w języku Python dla systemu Linux'
description: Zacznij korzystać z aplikacji systemu Linux na Azure App Service, wdrażając pierwszą aplikację w języku Python w kontenerze systemu Linux w App Service.
ms.topic: quickstart
ms.date: 06/30/2020
ms.custom: seo-python-october2019, cli-validate, tracking-python
ms.openlocfilehash: 1411c6ccc5228aa9248d5185bf44ecbfd496ed1f
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/23/2020
ms.locfileid: "87042356"
---
# <a name="quickstart-create-a-python-app-in-azure-app-service-on-linux"></a>Szybki Start: Tworzenie aplikacji w języku Python w Azure App Service w systemie Linux

W tym przewodniku szybki start wdrożono aplikację sieci Web w języku Python w celu [App Service w systemie Linux](app-service-linux-intro.md)— wysoce skalowalna, samoobsługowa usługa hostingu sieci Web na platformie Azure. Używasz lokalnego [interfejsu wiersza polecenia platformy Azure (CLI)](/cli/azure/install-azure-cli) na komputerze Mac, Linux lub Windows. Skonfigurowana aplikacja internetowa korzysta z bezpłatnej warstwy App Service, więc nie ponosisz żadnych kosztów w ramach tego artykułu.

Jeśli wolisz wdrażać aplikacje za pośrednictwem środowiska IDE, zobacz [wdrażanie aplikacji w języku Python do App Service z Visual Studio Code](/azure/developer/python/tutorial-deploy-app-service-on-linux-01).

## <a name="set-up-your-initial-environment"></a>Konfigurowanie środowiska początkowego

Przed rozpoczęciem należy wykonać następujące czynności:

1. Posiadanie konta platformy Azure z aktywną subskrypcją. [Utwórz konto bezpłatnie](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).
1. Zainstaluj środowisko Python w wersji <a href="https://www.python.org/downloads/" target="_blank">3,6 lub nowszej</a>.
1. Zainstaluj <a href="/cli/azure/install-azure-cli" target="_blank">interfejs wiersza polecenia platformy Azure</a> 2.0.80 lub nowszy, za pomocą którego uruchamiasz polecenia w dowolnej powłoce, aby udostępnić i skonfigurować zasoby platformy Azure.

Otwórz okno terminalu i sprawdź, czy wersja języka Python to 3,6 lub nowszego:

# <a name="bash"></a>[Bash](#tab/bash)

```bash
python3 --version
```

# <a name="powershell"></a>[Program PowerShell](#tab/powershell)

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

## <a name="clone-the-sample"></a>Klonowanie przykładu

Sklonuj przykładowe repozytorium za pomocą poniższego polecenia. ([Zainstaluj program git](https://git-scm.com/downloads) , jeśli nie masz już usługi Git).

```terminal
git clone https://github.com/Azure-Samples/python-docs-hello-world
```

Następnie przejdź do tego folderu:

```terminal
cd python-docs-hello-world
```

Przykładowy kod zawiera plik *Application.py* , który informuje, App Service, że kod zawiera aplikację z kolbą. Aby uzyskać więcej informacji, zobacz [Proces uruchamiania kontenera i dostosowania](how-to-configure-python.md).

## <a name="run-the-sample"></a>Uruchamianie aplikacji przykładowej

# <a name="bash"></a>[Bash](#tab/bash)

Najpierw Utwórz środowisko wirtualne i Zainstaluj zależności:

```bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

Następnie ustaw `FLASK_APP` zmienną środowiskową na moduł wprowadzania aplikacji i uruchom serwer programistyczny z kolbą:

```
export FLASK_APP=application.py
flask run
```

# <a name="powershell"></a>[Program PowerShell](#tab/powershell)

Najpierw Utwórz środowisko wirtualne i Zainstaluj zależności:

```powershell
py -3 -m venv env
env\scripts\activate
pip install -r requirements.txt
```

Następnie ustaw `FLASK_APP` zmienną środowiskową na moduł wprowadzania aplikacji i uruchom serwer programistyczny z kolbą:

```powershell
Set-Item Env:FLASK_APP ".\application.py"
flask run
```

# <a name="cmd"></a>[Cmd](#tab/cmd)

Najpierw Utwórz środowisko wirtualne i Zainstaluj zależności:

```cmd
py -3 -m venv env
env\scripts\activate
pip install -r requirements.txt
```

Następnie ustaw `FLASK_APP` zmienną środowiskową na moduł wprowadzania aplikacji i uruchom serwer programistyczny z kolbą:

```cmd
SET FLASK_APP=application.py
flask run
```

---

Otwórz przeglądarkę internetową i przejdź do przykładowej aplikacji pod adresem `http://localhost:5000/` . Aplikacja wyświetli komunikat **Hello World!**.

![Uruchamianie przykładowej aplikacji w języku Python lokalnie](./media/quickstart-python/run-hello-world-sample-python-app-in-browser-localhost.png)

W oknie terminalu naciśnij klawisz **Ctrl**, + **C** aby wyjść z serwera deweloperskiego.

## <a name="deploy-the-sample"></a>Wdróż przykład

Wdróż kod w folderze lokalnym (*Python-docs-Hello-World*) przy użyciu `az webapp up` polecenia:

```azurecli
az webapp up --sku F1 -n <app-name>
```

- Jeśli `az` polecenie nie jest rozpoznawane, upewnij się, że masz zainstalowany interfejs wiersza polecenia platformy Azure zgodnie z opisem w artykule [Konfigurowanie początkowego środowiska](#set-up-your-initial-environment).
- Zamień na `<app_name>` nazwę, która jest unikatowa na całym systemie Azure (*prawidłowe znaki to `a-z` , `0-9` i `-` *). Dobrym wzorcem jest użycie kombinacji nazwy firmy i identyfikatora aplikacji.
- `--sku F1`Argument tworzy aplikację sieci Web w warstwie cenowej bezpłatna. Pomiń ten argument, aby użyć szybszej warstwy Premium, która wiąże się z godziną.
- Opcjonalnie możesz dołączyć argument `-l <location-name>` `<location_name>` , gdzie jest regionem świadczenia usługi Azure, takim jak **środkowe**, **eastasia**, **westeurope**, **koreasouth**, **brazilsouth**, **centralindia**i tak dalej. Możesz pobrać listę dozwolonych regionów dla Twojego konta platformy Azure, uruchamiając [`az account list-locations`](/cli/azure/appservice?view=azure-cli-latest.md#az-appservice-list-locations) polecenie.

Wykonanie polecenia może potrwać kilka minut. W trakcie korzystania z programu są dostępne komunikaty dotyczące tworzenia grupy zasobów, planu App Service i aplikacji hostingu, konfigurowania rejestrowania, a następnie wykonywania wdrożenia ZIP. Następnie zostanie wyświetlony komunikat "można uruchomić aplikację pod adresem http:// &lt; App-Name &gt; . azurewebsites.NET", który jest adresem URL aplikacji na platformie Azure.

![Przykładowe dane wyjściowe polecenia AZ webapp up](./media/quickstart-python/az-webapp-up-output.png)

[!INCLUDE [AZ Webapp Up Note](../../../includes/app-service-web-az-webapp-up-note.md)]

## <a name="browse-to-the-app"></a>Przechodzenie do aplikacji

Przejdź do wdrożonej aplikacji w przeglądarce sieci Web pod adresem URL `http://<app-name>.azurewebsites.net` .

Przykładowy kod w języku Python używa kontenera systemu Linux w App Service przy użyciu wbudowanego obrazu.

![Uruchamianie przykładowej aplikacji w języku Python na platformie Azure](./media/quickstart-python/run-hello-world-sample-python-app-in-browser.png)

**Gratulacje!** Aplikacja w języku Python została wdrożona w celu App Service w systemie Linux.

## <a name="redeploy-updates"></a>Wdróż ponownie aktualizacje

W ulubionym edytorze kodu Otwórz *Application.py* i zaktualizuj `hello` funkcję w następujący sposób. Ta zmiana powoduje dodanie `print` instrukcji w celu wygenerowania danych wyjściowych rejestrowania, które są używane w następnej sekcji. 

```python
def hello():
    print("Handling request to home page.")
    return "Hello Azure!"
```

Zapisz zmiany i zamknij edytor. 

Ponownie Wdróż aplikację przy użyciu `az webapp up` polecenia:

```azurecli
az webapp up
```

To polecenie używa wartości, które są buforowane lokalnie w pliku *. Azure/config* , łącznie z nazwą aplikacji, grupą zasobów i planem App Service.

Po zakończeniu wdrażania Przełącz się z powrotem do okna przeglądarki otwartego na `http://<app-name>.azurewebsites.net` . Odśwież stronę, która powinna wyświetlać zmodyfikowany komunikat:

![Uruchamianie zaktualizowanej przykładowej aplikacji w języku Python na platformie Azure](./media/quickstart-python/run-updated-hello-world-sample-python-app-in-browser.png)

> [!TIP]
> Visual Studio Code zapewnia zaawansowane rozszerzenia dla języka Python i Azure App Service, co upraszcza proces wdrażania aplikacji sieci Web w języku Python w App Service. Aby uzyskać więcej informacji, zobacz [wdrażanie aplikacji w języku Python do App Service z Visual Studio Code](/azure/python/tutorial-deploy-app-service-on-linux-01).

## <a name="stream-logs"></a>Strumieniowe przesyłanie dzienników

Można uzyskać dostęp do dzienników konsoli wygenerowanych z wewnątrz aplikacji i kontenera, w którym działa. Dzienniki zawierają wszystkie dane wyjściowe wygenerowane przy użyciu `print` instrukcji.

Aby przesłać strumieniowo dzienniki, uruchom następujące polecenie:

```azurecli
az webapp log tail
```

Odśwież aplikację w przeglądarce, aby generować dzienniki konsoli, w tym komunikaty opisujące żądania HTTP do aplikacji. Jeśli dane wyjściowe nie pojawiają się natychmiast, spróbuj ponownie za 30 sekund.

Możesz również sprawdzić pliki dziennika z przeglądarki pod adresem `https://<app-name>.scm.azurewebsites.net/api/logs/docker` .

Aby zatrzymać przesyłanie strumieniowe dzienników w dowolnym momencie, wpisz **Ctrl** + **C**.

## <a name="manage-the-azure-app"></a>Zarządzanie aplikacją platformy Azure

Przejdź do witryny <a href="https://portal.azure.com" target="_blank">Azure Portal</a>, aby zarządzać utworzoną aplikacją. Wyszukaj i wybierz **App Services**.

![Przejdź do App Services w Azure Portal](./media/quickstart-python/navigate-to-app-services-in-the-azure-portal.png)

Wybierz nazwę aplikacji platformy Azure.

![Przejdź do aplikacji w języku Python w App Services w Azure Portal](./media/quickstart-python/navigate-to-app-in-app-services-in-the-azure-portal.png)

Wybranie aplikacji spowoduje otwarcie jej strony **Przegląd** , na której można wykonywać podstawowe zadania zarządzania, takie jak przeglądanie, zatrzymywanie, uruchamianie, ponowne uruchamianie i usuwanie.

![Zarządzaj swoją aplikacją w języku Python na stronie Przegląd w Azure Portal](./media/quickstart-python/manage-an-app-in-app-services-in-the-azure-portal.png)

Menu App Service zawiera różne strony służące do konfigurowania aplikacji.

## <a name="clean-up-resources"></a>Czyszczenie zasobów

W poprzednich krokach utworzono zasoby platformy Azure w grupie zasobów. Grupa zasobów ma nazwę "appsvc_rg_Linux_CentralUS" w zależności od lokalizacji. Jeśli używasz jednostki SKU App Service innej niż bezpłatna warstwa F1, te zasoby ponoszą bieżące koszty (zobacz [App Service Cennik](https://azure.microsoft.com/pricing/details/app-service/linux/)).

Jeśli nie chcesz potrzebować tych zasobów w przyszłości, Usuń grupę zasobów, uruchamiając następujące polecenie:

```azurecli
az group delete
```

Polecenie używa nazwy grupy zasobów zapisanej w pamięci podręcznej w pliku *. Azure/config* .

Wykonanie polecenia może potrwać minutę.

## <a name="next-steps"></a>Następne kroki

> [!div class="nextstepaction"]
> [Samouczek: aplikacja sieci Web języka Python (Django) z PostgreSQL](tutorial-python-postgresql-app.md)

> [!div class="nextstepaction"]
> [Dodawanie logowania użytkownika do aplikacji sieci Web w języku Python](../../active-directory/develop/quickstart-v2-python-webapp.md)

> [!div class="nextstepaction"]
> [Konfigurowanie aplikacji języka Python](how-to-configure-python.md)

> [!div class="nextstepaction"]
> [Samouczek: uruchamianie aplikacji języka Python w kontenerze niestandardowym](tutorial-custom-docker-image.md)
