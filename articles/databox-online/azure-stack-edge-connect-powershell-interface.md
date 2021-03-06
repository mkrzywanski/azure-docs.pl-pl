---
title: Nawiązywanie połączenia z urządzeniem Microsoft Azure Stack Edge i zarządzanie nim za pomocą interfejsu programu Windows PowerShell | Microsoft Docs
description: Opisuje sposób nawiązywania połączenia z Azure Stack Edge i zarządzania nim za pomocą interfejsu programu Windows PowerShell.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 06/25/2019
ms.author: alkohli
ms.openlocfilehash: 973c618b46d1b6be902d9629ca63ee120cae6855
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "85313202"
---
# <a name="manage-an-azure-stack-edge-device-via-windows-powershell"></a>Zarządzanie urządzeniem brzegowym Azure Stack za pomocą programu Windows PowerShell

Rozwiązanie Azure Stack Edge pozwala na przetwarzanie danych i wysyłanie ich przez sieć do platformy Azure. W tym artykule opisano niektóre zadania związane z konfiguracją i zarządzaniem dla Azure Stack urządzenia brzegowego. Aby zarządzać urządzeniem, można użyć Azure Portal, lokalnego interfejsu użytkownika sieci Web lub interfejsu programu Windows PowerShell.

Ten artykuł koncentruje się na zadaniach, które można wykonać za pomocą interfejsu programu PowerShell. 

Ten artykuł zawiera następujące procedury:

- Nawiązywanie połączenia z interfejsem programu PowerShell
- Tworzenie pakietu dla pomocy technicznej
- Przekazywanie certyfikatu
- Zresetuj urządzenie
- Wyświetl informacje o urządzeniu
- Pobieranie dzienników obliczeniowych
- Monitorowanie i rozwiązywanie problemów z modułami obliczeniowymi

## <a name="connect-to-the-powershell-interface"></a>Nawiązywanie połączenia z interfejsem programu PowerShell

[!INCLUDE [Connect to admin runspace](../../includes/data-box-edge-gateway-connect-minishell.md)]

## <a name="create-a-support-package"></a>Tworzenie pakietu dla pomocy technicznej

[!INCLUDE [Create a support package](../../includes/data-box-edge-gateway-create-support-package.md)]

## <a name="upload-certificate"></a>Przekazywanie certyfikatu

[!INCLUDE [Upload certificate](../../includes/data-box-edge-gateway-upload-certificate.md)]

Można również przekazać certyfikaty IoT Edge, aby umożliwić bezpieczne połączenie między urządzeniem IoT Edge i urządzeniami podrzędnymi, które mogą się z nim połączyć. Istnieją trzy IoT Edge certyfikaty (format*PEM* ), które należy zainstalować:

- Certyfikat głównego urzędu certyfikacji lub urząd certyfikacji właściciela
- Certyfikat urzędu certyfikacji urządzenia
- Certyfikat klucza urządzenia

W poniższym przykładzie pokazano użycie tego polecenia cmdlet w celu zainstalowania IoT Edge certyfikatów:

```
Set-HcsCertificate -Scope IotEdge -RootCACertificateFilePath "\\hcfs\root-ca-cert.pem" -DeviceCertificateFilePath "\\hcfs\device-ca-cert.pem\" -DeviceKeyFilePath "\\hcfs\device-key-cert.pem" -Credential "username"
```
Po uruchomieniu tego polecenia cmdlet zostanie wyświetlony monit o podanie hasła dla udziału sieciowego.

Aby uzyskać więcej informacji na temat certyfikatów, przejdź do pozycji [Azure IoT Edge Certificates](https://docs.microsoft.com/azure/iot-edge/iot-edge-certs) lub [Zainstaluj certyfikaty na bramie](https://docs.microsoft.com/azure/iot-edge/how-to-create-transparent-gateway).

## <a name="view-device-information"></a>Wyświetl informacje o urządzeniu
 
[!INCLUDE [View device information](../../includes/data-box-edge-gateway-view-device-info.md)]

## <a name="reset-your-device"></a>Resetowanie urządzenia

[!INCLUDE [Reset your device](../../includes/data-box-edge-gateway-deactivate-device.md)]

## <a name="get-compute-logs"></a>Pobieranie dzienników obliczeniowych

Jeśli na urządzeniu skonfigurowano rolę obliczeniową, można także uzyskać dzienniki obliczeń za pomocą interfejsu programu PowerShell.

1. [Nawiąż połączenie z interfejsem programu PowerShell](#connect-to-the-powershell-interface).
2. Użyj, `Get-AzureDataBoxEdgeComputeRoleLogs` Aby pobrać dzienniki obliczeniowe dla Twojego urządzenia.

    W poniższym przykładzie pokazano użycie tego polecenia cmdlet:

    ```powershell
    Get-AzureDataBoxEdgeComputeRoleLogs -Path "\\hcsfs\logs\myacct" -Credential "username" -FullLogCollection
    ```

    Poniżej znajduje się opis parametrów używanych dla polecenia cmdlet:
    - `Path`: Podaj ścieżkę sieciową do udziału, w którym chcesz utworzyć pakiet dziennika obliczeń.
    - `Credential`: Podaj nazwę użytkownika dla udziału sieciowego. Po uruchomieniu tego polecenia cmdlet konieczne będzie podanie hasła udziału.
    - `FullLogCollection`: Ten parametr zapewnia, że pakiet dziennika będzie zawierać wszystkie dzienniki obliczeń. Domyślnie pakiet dziennika zawiera tylko podzestaw dzienników.

## <a name="monitor-and-troubleshoot-compute-modules"></a>Monitorowanie i rozwiązywanie problemów z modułami obliczeniowymi

[!INCLUDE [Monitor and troubleshoot compute modules](../../includes/azure-stack-edge-monitor-troubleshoot-compute.md)]

## <a name="exit-the-remote-session"></a>Zakończ sesję zdalną

Aby wyjść z zdalnej sesji programu PowerShell, Zamknij okno programu PowerShell.

## <a name="next-steps"></a>Następne kroki

- Wdróż [Azure Stack Edge](azure-stack-edge-deploy-prep.md) w Azure Portal.
