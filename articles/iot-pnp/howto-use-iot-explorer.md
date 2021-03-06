---
title: Instalowanie i używanie programu Azure IoT Explorer | Microsoft Docs
description: Zainstaluj narzędzie Azure IoT Explorer i użyj go do współpracy z urządzeniami z programem IoT Plug and Play w wersji zapoznawczej podłączonym do usługi IoT Hub.
author: rido-min
ms.author: rmpablos
ms.date: 05/06/2020
ms.topic: how-to
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: 083dcde44e56af34f17d952c46e554b234818f27
ms.sourcegitcommit: 46f8457ccb224eb000799ec81ed5b3ea93a6f06f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/28/2020
ms.locfileid: "87352364"
---
# <a name="install-and-use-azure-iot-explorer"></a>Instalowanie i korzystanie z programu Azure IoT Explorer

Program Azure IoT Explorer jest graficznym narzędziem do współdziałania z urządzeniami w wersji zapoznawczej IoT Plug and Play i testowania. Po zainstalowaniu narzędzia na komputerze lokalnym można używać go do nawiązywania połączenia z koncentratorem. Można użyć narzędzia, aby wyświetlić dane telemetryczne wysyłane przez urządzenia, korzystać z właściwości urządzenia i wywoływać polecenia.

W tym artykule wyjaśniono, jak:

- Zainstaluj i skonfiguruj narzędzie Azure IoT Explorer.
- Użyj narzędzia, aby korzystać z i testować urządzenia.

## <a name="prerequisites"></a>Wymagania wstępne

Aby korzystać z narzędzia Azure IoT Explorer, potrzebne są:

- Usługa Azure IoT Hub. Istnieje wiele sposobów dodawania usługi IoT Hub do subskrypcji platformy Azure, takich jak [Tworzenie Centrum IoT Hub przy użyciu interfejsu wiersza polecenia platformy Azure](../iot-hub/iot-hub-create-using-cli.md). Do uruchomienia narzędzia Azure IoT Explorer potrzebne są parametry połączenia z usługą IoT Hub. Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Urządzenie zarejestrowane w usłudze IoT Hub. Aby zarejestrować urządzenie, można użyć poniższego polecenia interfejsu CLI platformy Azure. Pamiętaj, aby zastąpić `{YourIoTHubName}` `{YourDeviceID}` symbole zastępcze i wartościami:

    ```azurecli-interactive
    az iot hub device-identity create --hub-name {YourIoTHubName} --device-id {YourDeviceID}
    ```

## <a name="install-azure-iot-explorer"></a>Instalowanie programu Azure IoT Explorer

Przejdź do [wersji programu Azure IoT Explorer](https://github.com/Azure/azure-iot-explorer/releases) i rozwiń listę zasobów dla najnowszej wersji. Pobierz i zainstaluj najnowszą wersję aplikacji.

>[!Important]
>Z wersji 0.11.0 IoT Explorer obsługuje tylko Plug and Play IoT 2020 — wersja zapoznawcza. Aby skorzystać z funkcji dostępnych w poprzedniej wersji zapoznawczej — wydanie z sierpnia 2019, należy zainstalować wersję 0.10. x.

## <a name="use-azure-iot-explorer"></a>Korzystanie z programu Azure IoT Explorer

W przypadku urządzenia można podłączyć własne urządzenie lub użyć jednego z przykładowych symulowanych urządzeń. Wykonaj [te instrukcje](https://github.com/Azure/azure-iot-sdk-c/tree/public-preview/iothub_client/samples) , aby uruchomić przykład symulowanego urządzenia.

### <a name="connect-to-your-hub"></a>Nawiązywanie połączenia z centrum

Przy pierwszym uruchomieniu programu Azure IoT Explorer zostanie wyświetlony monit o podanie parametrów połączenia Centrum IoT. Po dodaniu parametrów połączenia wybierz pozycję **Połącz**. Możesz użyć ustawień narzędzia, aby przełączyć się do innego Centrum IoT Hub przez zaktualizowanie parametrów połączenia.

Definicje modeli dla urządzenia IoT Plug and Play są przechowywane w repozytorium publicznym, podłączonym urządzeniu lub w folderze lokalnym. Domyślnie narzędzie szuka definicji modelu w repozytorium publicznym i podłączonym urządzeniu. Możesz dodawać i usuwać źródła albo konfigurować priorytety źródeł w **ustawieniach**:

Aby dodać Źródło:

1. Przejdź do **ustawień Plug and Play domu/IoT**
2. Wybierz pozycję **Dodaj** , a następnie wybierz źródło, z repozytorium lub folderu lokalnego.

Aby usunąć Źródło:

1. Przejdź do **ustawień Plug and Play domu/IoT**
2. Znajdź źródło, które chcesz usunąć.
3. Wybierz pozycję **X** , aby ją usunąć.

Zmień priorytety źródłowe:

Możesz przeciągać i upuszczać jeden ze źródeł definicji modelu do innej klasyfikacji na liście. 

### <a name="view-devices"></a>Wyświetlanie urządzeń

Po połączeniu narzędzia z Centrum IoT zostanie wyświetlona strona lista **urządzeń** z listą tożsamości urządzeń zarejestrowanych w centrum IoT Hub. Możesz wybrać dowolny wpis na liście, aby wyświetlić więcej informacji.

Na stronie lista **urządzeń** można wykonać następujące instrukcje:

- Wybierz pozycję **Nowy** , aby zarejestrować nowe urządzenie w centrum. Następnie wprowadź identyfikator urządzenia. Użyj ustawień domyślnych, aby automatycznie generować klucze uwierzytelniania i włączyć połączenie z centrum.
- Wybierz urządzenie, a następnie wybierz pozycję **Usuń** , aby usunąć tożsamość urządzenia. Przed ukończeniem tej akcji Przejrzyj szczegóły urządzenia, aby upewnić się, że usuwasz właściwą tożsamość urządzenia.

## <a name="interact-with-a-device"></a>Korzystanie z urządzenia

Na stronie lista **urządzeń** wybierz wartość w kolumnie **Identyfikator urządzenia** , aby wyświetlić stronę szczegółów dla zarejestrowanego urządzenia. Dla każdego urządzenia znajdują się dwie sekcje: **Device** i **Digital bliźniaczy**.

### <a name="device"></a>Urządzenie

Ta sekcja obejmuje **tożsamość urządzenia**, **sznurki urządzenia**, **dane telemetryczne**, **metodę bezpośrednią**, **komunikat z chmury do urządzenia**, karty **tożsamości modułów** .

- Informacje o [tożsamości urządzenia](../iot-hub/iot-hub-devguide-identity-registry.md) można wyświetlić i zaktualizować na karcie **tożsamość urządzenia** .
- Dostęp do informacji o [bliźniaczych urządzeniach](../iot-hub/iot-hub-devguide-device-twins.md) można uzyskać na karcie **sznurki urządzenia** .
- Jeśli urządzenie jest połączone i aktywnie wysyła dane, [można je wyświetlić na karcie](../iot-hub/iot-hub-devguide-messages-read-builtin.md) **telemetrii** .
- [Metodę bezpośrednią](../iot-hub/iot-hub-devguide-direct-methods.md) można wywołać na urządzeniu na karcie **Metoda bezpośrednia** .
- [Komunikat z chmury do urządzenia](../iot-hub/iot-hub-devguide-messages-c2d.md) można wysłać na karcie **komunikaty z chmury do urządzenia** .
- Możesz uzyskać dostęp do informacji o [bliźniaczych modułach](../iot-hub/iot-hub-devguide-module-twins.md) .

### <a name="iot-plug-and-play-components"></a>Składniki Plug and Play IoT

Jeśli urządzenie jest połączone z centrum przy użyciu **identyfikatora modelu**, narzędzie wyświetli kartę **składniki Plug and Play IoT** "", w której można zobaczyć **Identyfikator modelu**.

Jeśli **Identyfikator modelu** jest dostępny w jednym ze skonfigurowanych źródeł — publicznego repozytorium lub folderu lokalnego, zostanie wyświetlona lista składników. Wybranie składnika powoduje wyświetlenie dostępnych właściwości, poleceń i danych telemetrycznych.

Na stronie **składnik** można wyświetlić właściwości tylko do odczytu, zaktualizować właściwości zapisywalne, wywołać polecenia i wyświetlić komunikaty telemetryczne wytwarzane przez ten składnik.

:::image type="content" source="media/howto-use-iot-explorer/components-iot-explorer.png" alt-text="Wyświetlanie składników w programie Azure IoT Explorer":::

#### <a name="properties"></a>Właściwości

:::image type="content" source="media/howto-use-iot-explorer/properties-iot-explorer.png" alt-text="Wyświetlanie właściwości w programie Azure IoT Explorer":::

Właściwości tylko do odczytu zdefiniowane w interfejsie można wyświetlić na karcie **właściwości (tylko do odczytu)** . Można zaktualizować zapisywalne właściwości zdefiniowane w interfejsie na karcie **właściwości (zapisywalne)** :

1. Przejdź do karty **właściwości (zapisywalne)** .
1. Kliknij właściwość, którą chcesz zaktualizować.
1. Wprowadź nową wartość właściwości.
1. Wyświetlenie podglądu ładunku do wysłania na urządzenie.
1. Prześlij zmianę.

Po przesłaniu zmiany można śledzić stan aktualizacji: **Synchronizacja**, **sukces**lub **błąd**. Po zakończeniu synchronizacji zostanie wyświetlona nowa wartość właściwości w kolumnie **raportowana Właściwość** . Jeśli przejdziesz do innych stron przed ukończeniem synchronizacji, narzędzie będzie nadal powiadamiane po zakończeniu aktualizacji. Możesz również użyć Centrum powiadomień narzędzia, aby wyświetlić historię powiadomień.

#### <a name="commands"></a>Polecenia

Aby wysłać polecenie do urządzenia, przejdź do karty **polecenia** :

1. Na liście poleceń Rozwiń polecenie, które ma zostać wyzwolone.
1. Wprowadź wszelkie wymagane wartości dla polecenia.
1. Wyświetlenie podglądu ładunku do wysłania na urządzenie.
1. Prześlij polecenie.

#### <a name="telemetry"></a>Telemetry

Aby wyświetlić dane telemetryczne dla wybranego interfejsu, przejdź do karty **telemetrii** .

## <a name="next-steps"></a>Następne kroki

W tym artykule poznasz sposób instalowania i używania programu Azure IoT Explorer do współpracy z urządzeniami Plug and Play IoT. Sugerowanym następnym krokiem jest nauczenie się, jak używać [poleceń Plug and Play interfejsu wiersza polecenia platformy Azure](./howto-use-iot-pnp-cli.md).
