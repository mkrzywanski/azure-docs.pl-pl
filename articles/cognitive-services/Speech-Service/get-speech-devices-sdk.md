---
title: Pobieranie zestawu Speech Devices SDK
titleSuffix: Azure Cognitive Services
description: Usługa mowy współpracuje z wieloma urządzeniami i źródłami audio. Teraz możesz robić aplikacje mowy na następnym poziomie przy użyciu dopasowanego sprzętu i oprogramowania. W tym artykule dowiesz się, jak uzyskać dostęp do zestawu SDK urządzeń mowy i rozpocząć tworzenie aplikacji.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 04/14/2019
ms.author: erhopf
ms.openlocfilehash: 0bc1a7b5e443de0c1a95fa209d2e5a280cf28ef2
ms.sourcegitcommit: 5b8fb60a5ded05c5b7281094d18cf8ae15cb1d55
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/29/2020
ms.locfileid: "87385844"
---
# <a name="get-the-cognitive-services-speech-devices-sdk"></a>Pobierz zestaw SDK urządzeń Cognitive Services Speech

Zestaw SDK urządzeń mowy jest wstępnie dostroją biblioteką opracowaną w celu pracy z utworzonymi zestawami deweloperskimi i różnymi konfiguracjami macierzy mikrofonów.

## <a name="choose-a-development-kit"></a>Wybierz zestaw deweloperski

|Urządzenia|Specyfikacja|Opis|Scenariusze|
|--|--|--|--|
|[Urbetter dev Kit](http://www.urbetter.com/products_56/278.html) ![ URbetter DDK](media/speech-devices-sdk/device-urbetter.jpg)|7. tablica mikrofonu, ARM SOC, Wi-Fi, Ethernet, HDMI, Kamera USB. <br>Linux|Zestaw SDK urządzeń typu "Speech", który dostosowuje macierz Microsoft MIC i obsługuje rozszerzone we/wy, takie jak HDMI/Ethernet i więcej urządzeń peryferyjnych USB <br> [Skontaktuj się z Urbetter](http://www.urbetter.com/products_56/278.html)|Transkrypcja konwersacji, edukacja, szpital, roboty, OTT, Agent głosowy, dysk przez|
|[Roobo Smart audio dev Kit](http://ddk.roobo.com)<br>[Konfiguracja](speech-devices-sdk-roobo-v1.md)  /  [Szybki Start](speech-devices-sdk-android-quickstart.md) ![ Roobo Smart audio dev Kit](media/speech-devices-sdk/device-roobo-v1.jpg)|7, tablica MIC, w usłudze ARM, Sieć Wi-Fi, wyjście z operacji we/wy. <br>[Android](speech-devices-sdk-android-quickstart.md)|Pierwszy zestaw SDK usługi Speech Devices do adaptacji zestawu SDK oprogramowania Microsoft MIC Array i Front Processing|Transkrypcja konwersacji, inteligentny głośnik, Agent głosowy, wearable|
|[Azure Kinect DK](https://azure.microsoft.com/services/kinect-dk/)<br>[Konfiguracja](https://docs.microsoft.com/azure/Kinect-dk/set-up-azure-kinect-dk)  /  [Szybki Start](speech-devices-sdk-windows-quickstart.md) ![ Azure urządzenia Kinect DK](media/speech-devices-sdk/device-azure-kinect-dk.jpg)|7 aparaty RGB i głębokości macierzy mikrofonów. <br>[System Windows](speech-devices-sdk-windows-quickstart.md) / System [Linux](speech-devices-sdk-linux-quickstart.md)|Zestaw deweloperów z zaawansowanymi czujnikami sztucznej analizy (AI) do tworzenia zaawansowanych modeli przetwarzania i przetwarzania mowy. Łączy on najlepszą w swojej klasie macierzową tablicę i głębokości, z kamerą wideo i czujnikiem orientacji — wszystko to w jednym niewielkim urządzeniu z wieloma trybami, opcjami i zestawami SDK do obsługi różnych typów obliczeniowych.|Transkrypcja konwersacji, robotica, inteligentne Kompilowanie|
|Roobo Smart audio dev Kit 2<br>[Instalacja](speech-devices-sdk-roobo-v2.md)<br>![Roobo Smart audio dev Kit 2](media/speech-devices-sdk/device-roobo-v2.jpg)|7 z tablicą MIC, ARM SOC, Wi-Fi, Bluetooth, we/wy. <br>Linux|Zestaw SDK urządzeń z informacjami o drugiej generacji, który oferuje alternatywny system operacyjny i więcej funkcji w ramach ekonomicznych projektów referencyjnych.|Transkrypcja konwersacji, inteligentny głośnik, Agent głosowy, wearable|


## <a name="download-the-speech-devices-sdk"></a>Pobierz zestaw SDK urządzeń mowy

Pobierz [zestaw SDK urządzeń mowy](https://aka.ms/sdsdk-download).

## <a name="next-steps"></a>Następne kroki

> [!div class="nextstepaction"]
> [Wprowadzenie do zestawu SDK urządzeń mowy](https://aka.ms/sdsdk-quickstart)
