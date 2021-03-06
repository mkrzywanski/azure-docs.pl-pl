---
title: Konfigurowanie aplikacji Node.js
description: Dowiedz się, jak skonfigurować wstępnie utworzony kontener Node.js dla aplikacji. W tym artykule przedstawiono najczęstsze zadania konfiguracyjne.
ms.devlang: nodejs
ms.topic: article
ms.date: 03/28/2019
ms.openlocfilehash: 699c77e937fc13cadf742d193ab1b0b8f00a2726
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "84905718"
---
# <a name="configure-a-linux-nodejs-app-for-azure-app-service"></a>Konfigurowanie aplikacji Node.js systemu Linux dla Azure App Service

Aplikacje Node.js muszą być wdrożone ze wszystkimi wymaganymi zależnościami NPM. Aparat wdrażania App Service (kudu) jest automatycznie uruchamiany `npm install --production` podczas wdrażania [repozytorium git](../deploy-local-git.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)lub [pakiet ZIP](../deploy-zip.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json) z procesami kompilacji, które zostały włączone. W przypadku wdrażania plików przy użyciu [protokołu FTP/S](../deploy-ftp.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)należy jednak ręcznie przekazać wymagane pakiety.

Ten przewodnik zawiera najważniejsze pojęcia i instrukcje dla Node.js deweloperów korzystających z wbudowanego kontenera systemu Linux w programie App Service. Jeśli nie korzystasz z Azure App Service, postępuj zgodnieNode.js z przewodnikiem [Szybki Start](quickstart-nodejs.md) i [Node.js z samouczkiem MongoDB](tutorial-nodejs-mongodb-app.md) .

## <a name="show-nodejs-version"></a>Pokaż wersję Node.js

Aby wyświetlić bieżącą wersję Node.js, uruchom następujące polecenie w [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp config show --resource-group <resource-group-name> --name <app-name> --query linuxFxVersion
```

Aby wyświetlić wszystkie obsługiwane wersje Node.js, uruchom następujące polecenie w [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp list-runtimes --linux | grep NODE
```

## <a name="set-nodejs-version"></a>Ustaw wersję Node.js

Aby ustawić [obsługiwaną wersję Node.js](#show-nodejs-version), uruchom następujące polecenie w [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp config set --resource-group <resource-group-name> --name <app-name> --linux-fx-version "NODE|10.14"
```

To ustawienie określa wersję Node.js, która ma być używana zarówno w czasie wykonywania, jak i podczas zautomatyzowanego przywracania pakietów w programie kudu.

> [!NOTE]
> Należy ustawić wersję Node.js w projekcie `package.json` . Aparat wdrażania działa w oddzielnym kontenerze zawierającym wszystkie obsługiwane wersje Node.js.

## <a name="customize-build-automation"></a>Dostosuj automatyzację kompilacji

Jeśli aplikacja zostanie wdrożona za pomocą usługi Git lub zip z włączonym automatyzacją kompilacji, App Service kroki automatyzacji kompilacji w następującej kolejności:

1. Uruchom skrypt niestandardowy, jeśli został określony przez `PRE_BUILD_SCRIPT_PATH` .
1. Uruchom `npm install` bez żadnych flag, które obejmują npm `preinstall` i `postinstall` skrypty, a także instaluje `devDependencies` .
1. Uruchom `npm run build` , jeśli skrypt kompilacji jest określony w *package.jsna*.
1. Uruchom `npm run build:azure` , jeśli kompilacja: skrypt platformy Azure został określony w *package.jsna*.
1. Uruchom skrypt niestandardowy, jeśli został określony przez `POST_BUILD_SCRIPT_PATH` .

> [!NOTE]
> Zgodnie z opisem w dokumentacji [npm](https://docs.npmjs.com/misc/scripts), skrypty o nazwie `prebuild` i `postbuild` uruchomione przed i po nim `build` odpowiednio, jeśli zostały określone. `preinstall`i `postinstall` działają odpowiednio przed i po `install` .

`PRE_BUILD_COMMAND`i `POST_BUILD_COMMAND` są zmiennymi środowiskowymi, które są domyślnie puste. Aby uruchomić polecenia przed kompilacją, zdefiniuj `PRE_BUILD_COMMAND` . Aby uruchomić polecenia po kompilacji, zdefiniuj `POST_BUILD_COMMAND` .

W poniższym przykładzie określono dwie zmienne do szeregu poleceń, oddzielone przecinkami.

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings PRE_BUILD_COMMAND="echo foo, scripts/prebuild.sh"
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings POST_BUILD_COMMAND="echo foo, scripts/postbuild.sh"
```

Aby uzyskać dodatkowe zmienne środowiskowe umożliwiające dostosowanie automatyzacji kompilacji, zobacz [Konfiguracja Oryx](https://github.com/microsoft/Oryx/blob/master/doc/configuration.md).

Aby uzyskać więcej informacji na temat App Service uruchamiania i kompilowania Node.js aplikacji w systemie Linux, zobacz [dokumentację Oryx: jak wykrywane są i kompilowane aplikacje Node.js](https://github.com/microsoft/Oryx/blob/master/doc/runtimes/nodejs.md).

## <a name="configure-nodejs-server"></a>Skonfiguruj serwer Node.js

Kontenery Node.js są dostarczane z [PM2](https://pm2.keymetrics.io/), menedżerem procesów produkcyjnych. Aplikację można skonfigurować tak, aby była uruchamiana z PM2 lub z NPM lub przy użyciu polecenia niestandardowego.

- [Uruchom polecenie niestandardowe](#run-custom-command)
- [Uruchom npm](#run-npm-start)
- [Uruchom z PM2](#run-with-pm2)

### <a name="run-custom-command"></a>Uruchom polecenie niestandardowe

App Service można uruchomić aplikację przy użyciu polecenia niestandardowego, takiego jak plik wykonywalny, taki jak *Run.sh*. Na przykład aby uruchomić `npm run start:prod` polecenie, uruchom następujące polecenie w [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp config set --resource-group <resource-group-name> --name <app-name> --startup-file "npm run start:prod"
```

### <a name="run-npm-start"></a>Uruchom npm

Aby rozpocząć korzystanie z aplikacji `npm start` , upewnij się, że `start` skrypt znajduje się w *package.js* pliku. Przykład:

```json
{
  ...
  "scripts": {
    "start": "gulp",
    ...
  },
  ...
}
```

Aby użyć niestandardowego *package.js* w projekcie, uruchom następujące polecenie w [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp config set --resource-group <resource-group-name> --name <app-name> --startup-file "<filename>.json"
```

### <a name="run-with-pm2"></a>Uruchom z PM2

Kontener automatycznie uruchamia aplikację z PM2, gdy w projekcie zostanie znaleziony jeden z popularnych plików Node.js:

- *bin/www*
- *server.js*
- *app.js*
- *index.js*
- *hostingstart.js*
- Jeden z następujących [plików PM2](https://pm2.keymetrics.io/docs/usage/application-declaration/#process-file): *process.json* i *ecosystem.config.js*

Możesz również skonfigurować niestandardowy plik startowy z następującymi rozszerzeniami:

- Plik *. js*
- [Plik PM2](https://pm2.keymetrics.io/docs/usage/application-declaration/#process-file) z rozszerzeniem *. JSON*, *.config.js*, *. YAML*lub *. yml*

Aby dodać niestandardowy plik startowy, uruchom następujące polecenie w [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp config set --resource-group <resource-group-name> --name <app-name> --startup-file "<filname-with-extension>"
```

## <a name="debug-remotely"></a>Debugowanie zdalne

> [!NOTE]
> Debugowanie zdalne jest obecnie w wersji zapoznawczej.

Możesz debugować aplikację Node.js zdalnie w [Visual Studio Code](https://code.visualstudio.com/) , jeśli skonfigurujesz ją do [uruchamiania z PM2](#run-with-pm2), z wyjątkiem sytuacji, gdy zostanie ona uruchomiona przy użyciu * .config.js, *. yml lub *. YAML*.

W większości przypadków dla aplikacji nie jest wymagana dodatkowa konfiguracja. Jeśli aplikacja jest uruchamiana z *process.jsw* pliku (domyślnie lub niestandardowym), musi mieć `script` Właściwość w katalogu głównym JSON. Przykład:

```json
{
  "name"        : "worker",
  "script"      : "./index.js",
  ...
}
```

Aby skonfigurować Visual Studio Code na potrzeby debugowania zdalnego, zainstaluj [rozszerzenie App Service](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azureappservice). Postępuj zgodnie z instrukcjami na stronie rozszerzenia i zaloguj się do platformy Azure w Visual Studio Code.

W Eksploratorze Azure Znajdź aplikację, którą chcesz debugować, kliknij ją prawym przyciskiem myszy i wybierz polecenie **Rozpocznij debugowanie zdalne**. Kliknij przycisk **tak** , aby włączyć go dla Twojej aplikacji. App Service uruchamia serwer proxy tunelu i dołącza debuger. Następnie można wykonać żądania do aplikacji i zobaczyć debuger wstrzymywany w punktach przerwania.

Po zakończeniu debugowania Zatrzymaj debuger, wybierając pozycję **Rozłącz**. Po wyświetleniu monitu kliknij przycisk **tak** , aby wyłączyć debugowanie zdalne. Aby wyłączyć go później, ponownie kliknij prawym przyciskiem myszy aplikację w Eksploratorze Azure, a następnie wybierz opcję **Wyłącz debugowanie zdalne**.

## <a name="access-environment-variables"></a>Uzyskiwanie dostępu do zmiennych środowiskowych

W App Service można [ustawić ustawienia aplikacji](../configure-common.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#configure-app-settings) poza kodem aplikacji. Następnie możesz uzyskać do nich dostęp przy użyciu standardowego wzorca Node.js. Aby na przykład uzyskać dostęp do ustawienia aplikacji o nazwie `NODE_ENV`, użyj następującego kodu:

```javascript
process.env.NODE_ENV
```

## <a name="run-gruntbowergulp"></a>Uruchom grunt/Bower/Gulp

Domyślnie kudu jest uruchamiany, `npm install --production` gdy rozpoznaje wdrożoną aplikację Node.js. Jeśli aplikacja wymaga dowolnego ze popularnych narzędzi do automatyzacji, takich jak grunt, Bower lub Gulp, należy podać [niestandardowy skrypt wdrożenia](https://github.com/projectkudu/kudu/wiki/Custom-Deployment-Script) , aby go uruchomić.

Aby umożliwić repozytorium uruchamianie tych narzędzi, należy dodać je do zależności w *package.js.* Przykład:

```json
"dependencies": {
  "bower": "^1.7.9",
  "grunt": "^1.0.1",
  "gulp": "^3.9.1",
  ...
}
```

W oknie terminalu lokalnego Zmień katalog na katalog główny repozytorium i uruchom następujące polecenia:

```bash
npm install kuduscript -g
kuduscript --node --scriptType bash --suppressPrompt
```

Katalog główny repozytorium ma teraz dwa dodatkowe pliki: *. Deployment* i *Deploy.sh*.

Otwórz *Deploy.sh* i Znajdź `Deployment` sekcję, która wygląda następująco:

```bash
##################################################################################################################################
# Deployment
# ----------
```

Ta sekcja kończy się z uruchomionym programem `npm install --production` . Dodaj sekcję kodu, która będzie potrzebna do uruchomienia wymaganego narzędzia *na końcu* `Deployment` sekcji:

- [Bower](#bower)
- [Gulp](#gulp)
- [Grunt](#grunt)

Zapoznaj się z [przykładem w przykładzie MEAN.js, w](https://github.com/Azure-Samples/meanjs/blob/master/deploy.sh#L112-L135)którym skrypt wdrażania również uruchamia `npm install` polecenie niestandardowe.

### <a name="bower"></a>Bower

Ten fragment kodu jest uruchamiany `bower install` .

```bash
if [ -e "$DEPLOYMENT_TARGET/bower.json" ]; then
  cd "$DEPLOYMENT_TARGET"
  eval ./node_modules/.bin/bower install
  exitWithMessageOnError "bower failed"
  cd - > /dev/null
fi
```

### <a name="gulp"></a>Gulp

Ten fragment kodu jest uruchamiany `gulp imagemin` .

```bash
if [ -e "$DEPLOYMENT_TARGET/gulpfile.js" ]; then
  cd "$DEPLOYMENT_TARGET"
  eval ./node_modules/.bin/gulp imagemin
  exitWithMessageOnError "gulp failed"
  cd - > /dev/null
fi
```

### <a name="grunt"></a>Grunt

Ten fragment kodu jest uruchamiany `grunt` .

```bash
if [ -e "$DEPLOYMENT_TARGET/Gruntfile.js" ]; then
  cd "$DEPLOYMENT_TARGET"
  eval ./node_modules/.bin/grunt
  exitWithMessageOnError "Grunt failed"
  cd - > /dev/null
fi
```

## <a name="detect-https-session"></a>Wykrywanie sesji protokołu HTTPS

W usłudze App Service [kończenie żądań SSL](https://wikipedia.org/wiki/TLS_termination_proxy) odbywa się w modułach równoważenia obciążenia sieciowego, dzięki czemu wszystkie żądania HTTPS docierają do aplikacji jako niezaszyfrowane żądania HTTP. Jeśli logika aplikacji musi sprawdzać, czy żądania użytkownika są szyfrowane, czy nie, zbadaj nagłówek `X-Forwarded-Proto`.

Popularne platformy internetowe umożliwiają dostęp do informacji `X-Forwarded-*` w standardowym wzorcu aplikacji. W programie [Express](https://expressjs.com/)można używać [zaufanych serwerów proxy](https://expressjs.com/guide/behind-proxies.html). Przykład:

```javascript
app.set('trust proxy', 1)
...
if (req.secure) {
  // Do something when HTTPS is used
}
```

## <a name="access-diagnostic-logs"></a>Uzyskiwanie dostępu do dzienników diagnostycznych

[!INCLUDE [Access diagnostic logs](../../../includes/app-service-web-logs-access-linux-no-h.md)]

## <a name="open-ssh-session-in-browser"></a>Otwórz sesję SSH w przeglądarce

[!INCLUDE [Open SSH session in browser](../../../includes/app-service-web-ssh-connect-builtin-no-h.md)]

## <a name="troubleshooting"></a>Rozwiązywanie problemów

Gdy działająca aplikacja Node.js działa inaczej w App Service lub zawiera błędy, spróbuj wykonać następujące czynności:

- [Dostęp do strumienia dzienników](#access-diagnostic-logs).
- Przetestuj aplikację lokalnie w trybie produkcyjnym. App Service uruchamia aplikacje Node.js w trybie produkcyjnym, dlatego należy się upewnić, że projekt działa zgodnie z oczekiwaniami w trybie produkcyjnym lokalnie. Przykład:
    - W zależności od *package.jsw systemie*można zainstalować różne pakiety dla trybu produkcyjnego ( `dependencies` vs. `devDependencies` ).
    - Niektóre platformy sieci Web mogą wdrażać pliki statyczne inaczej w trybie produkcyjnym.
    - Niektóre platformy sieci Web mogą używać niestandardowych skryptów uruchamiania podczas pracy w trybie produkcyjnym.
- Uruchom aplikację w App Service w trybie tworzenia. Na przykład w [MEAN.js](https://meanjs.org/)można ustawić aplikację na tryb programistyczny w czasie wykonywania, [ustawiając `NODE_ENV` ustawienie aplikacji](../configure-common.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#configure-app-settings).

[!INCLUDE [robots933456](../../../includes/app-service-web-configure-robots933456.md)]

## <a name="next-steps"></a>Następne kroki

> [!div class="nextstepaction"]
> [Samouczek: Node.js aplikacji za pomocą MongoDB](tutorial-nodejs-mongodb-app.md)

> [!div class="nextstepaction"]
> [App Service Linux — często zadawane pytania](app-service-linux-faq.md)
