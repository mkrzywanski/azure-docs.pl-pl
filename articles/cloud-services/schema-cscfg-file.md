---
title: Schemat definicji Cloud Services platformy Azure (plik cscfg) | Microsoft Docs
description: Plik konfiguracji usługi (. cscfg) określa liczbę wystąpień roli do wdrożenia dla każdej roli, wartości konfiguracji i odcisków palców certyfikatów dla roli.
services: cloud-services
ms.custom: ''
ms.date: 12/07/2016
ms.service: cloud-services
ms.topic: reference
caps.latest.revision: 35
author: tgore03
ms.author: tagore
ms.openlocfilehash: cb77181e00c97b7f426429793f17af3cb5e84ebe
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "79534749"
---
# <a name="azure-cloud-services-config-schema-cscfg-file"></a>Schemat konfiguracji usługi Azure Cloud Services (plik cscfg)
Plik konfiguracji usługi określa liczbę wystąpień roli do wdrożenia dla każdej roli w usłudze, wartości wszelkich ustawień konfiguracji i odcisków palców dla wszystkich certyfikatów skojarzonych z rolą. Jeśli usługa jest częścią Virtual Network, informacje o konfiguracji sieci muszą być podane w pliku konfiguracji usługi, a także w pliku konfiguracji sieci wirtualnej. Domyślnym rozszerzeniem dla pliku konfiguracji usługi jest. cscfg.

Model usług został opisany w [schemacie definicji usługi w chmurze (klasycznej)](schema-csdef-file.md).

Domyślnie plik schematu konfiguracji Diagnostyka Azure jest instalowany w `C:\Program Files\Microsoft SDKs\Windows Azure\.NET SDK\<version>\schemas` katalogu. Zamień na `<version>` zainstalowaną wersję [zestawu Azure SDK](https://azure.microsoft.com/downloads/).

Aby uzyskać więcej informacji na temat konfigurowania ról w usłudze, zobacz [co to jest model usługi w chmurze](cloud-services-model-and-package.md).

## <a name="basic-service-configuration-schema"></a>Schemat konfiguracji usługi podstawowej
Podstawowy format pliku konfiguracji usługi jest następujący:.

```xml
<ServiceConfiguration serviceName="<service-name>" osFamily="<osfamily-number>" osVersion="<os-version>" schemaVersion="<schema-version>">

  <Role …>
    …
  </Role>

  <NetworkConfiguration>
    …
  </NetworkConfiguration>

</ServiceConfiguration>
```

## <a name="schema-definitions"></a>Definicje schematu
W poniższych tematach opisano schemat dla `ServiceConfiguration` elementu:

- [Schemat Role](schema-cscfg-role.md)
- [Schemat NetworkConfiguration](schema-cscfg-networkconfiguration.md)

## <a name="service-configuration-namespace"></a>Przestrzeń nazw konfiguracji usługi
Przestrzeń nazw XML dla pliku konfiguracji usługi: `http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration` .

##  <a name="serviceconfiguration-element"></a><a name="ServiceConfiguration"></a>Element ServiceConfiguration
`ServiceConfiguration`Element jest elementem najwyższego poziomu w pliku konfiguracji usługi.

W poniższej tabeli opisano atrybuty `ServiceConfiguration` elementu. Wszystkie wartości atrybutów są typami ciągu.

| Atrybut | Opis |
| --------- | ----------- |
|serviceName|Wymagany. Nazwa usługi w chmurze. Nazwa podana w tym miejscu musi być zgodna z nazwą określoną w pliku definicji usługi.|
|osFamily|Opcjonalny. Określa system operacyjny gościa, który będzie uruchamiany w wystąpieniach roli w usłudze w chmurze. Aby uzyskać informacje na temat obsługiwanych wersji systemu operacyjnego gościa, zobacz [wersje systemu operacyjnego gościa platformy Azure i macierz zgodności zestawu SDK](cloud-services-guestos-update-matrix.md).<br /><br /> Jeśli nie dołączysz `osFamily` wartości i nie ustawisz `osVersion` atrybutu dla określonej wersji systemu operacyjnego gościa, zostanie użyta domyślna wartość 1.|
|osVersion|Opcjonalny. Określa wersję systemu operacyjnego gościa, który będzie uruchamiany w wystąpieniach roli w usłudze w chmurze. Aby uzyskać więcej informacji na temat wersji systemu operacyjnego gościa, zobacz wersje [systemu operacyjnego gościa platformy Azure i macierz zgodności zestawu SDK](cloud-services-guestos-update-matrix.md).<br /><br /> Można określić, że system operacyjny gościa ma być automatycznie uaktualniany do najnowszej wersji. W tym celu ustaw wartość `osVersion` atrybutu na `*` . Po ustawieniu na `*` , wystąpienia roli są wdrażane przy użyciu najnowszej wersji systemu operacyjnego gościa dla określonej rodziny systemów operacyjnych i zostaną automatycznie uaktualnione po wydaniu nowych wersji systemu operacyjnego gościa.<br /><br /> Aby określić konkretną wersję ręcznie, należy skorzystać `Configuration String` z tabeli w sekcji **przyszła, bieżąca i przejściowa wersja systemu operacyjnego gościa** w artykule wersje [systemu operacyjnego gościa platformy Azure i macierz zgodności zestawu SDK](cloud-services-guestos-update-matrix.md).<br /><br /> Wartość domyślna dla `osVersion` atrybutu to `*` .|
|schemaVersion|Opcjonalny. Określa wersję schematu konfiguracji usługi. Wersja schematu umożliwia programowi Visual Studio wybranie poprawnych narzędzi zestawu SDK, które będą używane do sprawdzania poprawności schematu w przypadku, gdy więcej niż jedna wersja zestawu SDK jest zainstalowana obok siebie. Aby uzyskać więcej informacji na temat zgodności schematów i wersji, zobacz [wersje systemu operacyjnego gościa platformy Azure i macierz zgodności zestawu SDK](cloud-services-guestos-update-matrix.md)|

Plik konfiguracji usługi musi zawierać jeden `ServiceConfiguration` element. `ServiceConfiguration`Element może zawierać dowolną liczbę `Role` elementów i zero lub 1 `NetworkConfiguration` elementów.