---
title: Konfiguracja lokalna agenta zabezpieczeń (C#)
description: Dowiedz się więcej na temat Azure Security Center dla usługi IoT Security, plik konfiguracji lokalnej agenta zabezpieczeń dla języka C#.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: 2cf6a49b-5d35-491f-abc3-63ec24eb4bc2
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/26/2019
ms.author: mlottner
ms.openlocfilehash: fc869a8ab905275c8082c4fd375f8f6d6d48d97e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "85205462"
---
# <a name="understanding-the-local-configuration-file-c-agent"></a>Omówienie lokalnego pliku konfiguracji (Agent C#)

Azure Security Center usługi IoT Security Agent używa konfiguracji z lokalnego pliku konfiguracji.

Agent zabezpieczeń odczytuje plik konfiguracji, gdy zostanie uruchomiony agent. Konfiguracje Znalezione w lokalnym pliku konfiguracji zawierają konfigurację uwierzytelniania i inne konfiguracje powiązane z agentem.

Agent zabezpieczeń języka C# używa wielu plików konfiguracji:

- **General.config** — konfiguracje powiązane z agentem.
- Konfiguracja związana z uwierzytelnianiem **Authentication.config** (w tym szczegóły uwierzytelniania).
- **SecurityIotInterface.config** — konfiguracje związane z IoT.

Pliki konfiguracji zawierają konfigurację domyślną. Konfiguracja uwierzytelniania jest wypełniana podczas instalacji agenta, a zmiany w pliku konfiguracji są wykonywane po ponownym uruchomieniu agenta.

## <a name="configuration-file-location"></a>Lokalizacja pliku konfiguracji

Dla systemu Linux:

- Pliki konfiguracji systemu operacyjnego znajdują się w temacie `/var/ASCIoTAgent` .

W przypadku systemu Windows:

- Pliki konfiguracji systemu operacyjnego znajdują się w katalogu agenta zabezpieczeń.

### <a name="generalconfig-configurations"></a>Konfiguracje General.config

| Nazwa konfiguracji | Możliwe wartości | Szczegóły |
|:-----------|:---------------|:--------|
| Identyfikator agenta | GUID | Unikatowy identyfikator agenta |
| readRemoteConfigurationTimeout | przedział_czasu | Okres pobierania konfiguracji zdalnej z IoT Hub. Jeśli Agent nie może pobrać konfiguracji w określonym czasie, zostanie przekroczony limit czasu operacji.|
| schedulerInterval | przedział_czasu | Wewnętrzny interwał harmonogramu. |
| producerInterval | przedział_czasu | Interwał procesu roboczego producenta zdarzeń. |
| consumerInterval | przedział_czasu | Interwał procesu roboczego odbiorcy zdarzeń. |
| highPriorityQueueSizePercentage | 0 < numer < 1 | Część całkowitej pamięci podręcznej dedykowana dla komunikatów o wysokim priorytecie. |
| logLevel | "Off", "krytyczny", "błąd", "ostrzeżenie", "informacje", "Debugowanie"  | Komunikaty dziennika równe i powyżej tej ważności są rejestrowane w konsoli debugowania (Dziennik systemowy w systemie Linux). |
| fileLogLevel |  "Off", "krytyczny", "błąd", "ostrzeżenie", "informacje", "Debugowanie"| Komunikaty dziennika równe i powyżej ważności są rejestrowane w pliku (Dziennik systemowy w systemie Linux). |
| diagnosticVerbosityLevel | "Brak", "niektóre", "wszystkie", | Poziom szczegółowości zdarzeń diagnostycznych. Brak — zdarzenia diagnostyczne nie są wysyłane, a wszystkie tylko zdarzenia diagnostyczne o wysokiej ważności są wysyłane, wszystkie — wszystkie dzienniki są również wysyłane jako zdarzenia diagnostyczne. |
| logFilePath | Ścieżka do pliku | Jeśli fileLogLevel > wyłączone, dzienniki są zapisywane w tym pliku. |
| defaultEventPriority | "High", "Low", "off" | Domyślny priorytet zdarzenia. |

### <a name="generalconfig-example"></a>Przykład General.config

```xml
<?xml version="1.0" encoding="utf-8"?>
<General>
  <add key="agentId" value="da00006c-dae9-4273-9abc-bcb7b7b4a987" />
  <add key="readRemoteConfigurationTimeout" value="00:00:30" />
  <add key="schedulerInterval" value="00:00:01" />
  <add key="producerInterval" value="00:02:00" />
  <add key="consumerInterval" value="00:02:00" />
  <add key="highPriorityQueueSizePercentage" value="0.5" />
  <add key="logLevel" value="Information" />
  <add key="fileLogLevel" value="Off"/>
  <add key="diagnosticVerbosityLevel" value="Some" />
  <add key="logFilePath" value="IotAgentLog.log" />
  <add key="defaultEventPriority" value="Low"/>
</General>
```

### <a name="authenticationconfig"></a>Authentication.config

| Nazwa konfiguracji | Możliwe wartości | Szczegóły |
|:-----------|:---------------|:--------|
| moduleName | ciąg | Nazwa tożsamości modułu zabezpieczeń. Ta nazwa musi odpowiadać nazwie tożsamości modułu w urządzeniu. |
| deviceId | ciąg | Identyfikator urządzenia (zgodnie z zarejestrowaniem w usłudze Azure IoT Hub). || schedulerInterval | Ciąg TimeSpan | Wewnętrzny interwał harmonogramu. |
| gatewayHostname | ciąg | Nazwa hosta usługi Azure IoT Hub. Zwykle <my-Hub>. azure-devices.net |
| Parametr | ścieżka ciągu do pliku | Ścieżka do pliku, który zawiera klucz tajny uwierzytelniania.|
| typ | "SymmetricKey", "SelfSignedCertificate" | Klucz tajny użytkownika do uwierzytelniania. Wybierz opcję *SymmetricKey* , jeśli klucz tajny użytkownika jest kluczem symetrycznym, wybierz opcję *certyfikat z* podpisem własnym, jeśli klucz tajny jest certyfikatem z podpisem własnym. |
| identity | "DPS", "moduł", "urządzenie" | Tożsamość uwierzytelniania — DPS Jeśli uwierzytelnianie odbywa się za pośrednictwem usługi DPS, moduł, jeśli uwierzytelnianie jest wykonywane przy użyciu poświadczeń modułu, lub urządzenie, jeśli uwierzytelnianie jest nawiązywane przy użyciu poświadczeń urządzenia.
| certificateLocationKind |  "Plik_lokalny", "magazyn" | Plik_lokalny Jeśli certyfikat jest przechowywany w pliku, należy go zapisać, jeśli certyfikat znajduje się w magazynie certyfikatów. |
| idScope | ciąg | Zakres identyfikatorów DPS |
| Identyfikator rejestracji | ciąg  | Usługa DPS Identyfikator rejestracji urządzenia. |
|

### <a name="authenticationconfig-example"></a>Przykład Authentication.config

```xml
<?xml version="1.0" encoding="utf-8"?>
<Authentication>
  <add key="moduleName" value="azureiotsecurity"/>
  <add key="deviceId" value="d1"/>
  <add key="gatewayHostname" value=""/>
  <add key="filePath" value="c:\p-dps-d1.pfx"/>
  <add key="type" value="SelfSignedCertificate" />                     <!-- SymmetricKey, SelfSignedCertificate-->
  <add key="identity" value="DPS" />                 <!-- Device, Module, DPS -->
  <add key="certificateLocationKind" value="LocalFile" />  <!-- LocalFile, Store -->
  <add key="idScope" value="0ne0005335B"/>
  <add key="registrationId" value="d1"/>
</Authentication>
```

### <a name="securityiotinterfaceconfig"></a>SecurityIotInterface.config

| Nazwa konfiguracji | Możliwe wartości | Szczegóły |
|:-----------|:---------------|:--------|
| transportType | "Ampq" "MQTT" | Typ transportu IoT Hub. |
|

### <a name="securityiotinterfaceconfig-example"></a>Przykład SecurityIotInterface.config

```xml
<ExternalInterface>
  <add key="facadeType"  value="Microsoft.Azure.Security.IoT.Agent.Common.SecurityIoTHubInterface, Security.Common" />
  <add key="transportType" value="Amqp"/>
</ExternalInterface>
```

## <a name="next-steps"></a>Następne kroki

- Przeczytaj Azure Security Center dla usługi IoT [— Omówienie](overview.md)
- Dowiedz się więcej o [architekturze](architecture.md) Azure Security Center dla usługi IoT
- Włączanie Azure Security Center dla [usługi](quickstart-onboard-iot-hub.md) IoT
- Przeczytaj Azure Security Center dla usługi IoT [— często zadawane pytania](resources-frequently-asked-questions.md)
- Dowiedz się, jak uzyskać dostęp do [danych pierwotnych zabezpieczeń](how-to-security-data-access.md)
- Omówienie [zaleceń](concept-recommendations.md)
- Informacje o [alertach](concept-security-alerts.md) zabezpieczeń
