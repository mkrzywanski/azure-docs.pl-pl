---
title: Typy proste schematu XML modelu usługi Service Fabric platformy Azure
description: Opisuje typy proste w schemacie XML modelu usługi Service Fabric.
ms.topic: reference
ms.date: 12/10/2018
ms.openlocfilehash: 3ed028bc255fa9561316a655da6edfacceff1c55
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "75466220"
---
<!-- This article was generated by the Python script found in the service-fabric-service-model-schema.md file -->

# <a name="service-model-xml-schema-simple-types"></a>Typy proste schematu XML modelu usługi

## <a name="fabricuri-simpletype"></a>FabricUri simpleType
Identyfikator URI, który jest używany jako stabilny identyfikator usług przez Microsoft Azure Service Fabric system nazewnictwa. 

### <a name="xml-source"></a>Źródło XML
```xml
<xs:simpleType xmlns:xs="https://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/2011/01/fabric" name="FabricUri">
    <xs:annotation>
      <xs:documentation>A URI that is used as a stable identifier of services by Microsoft Azure Service Fabric Naming system. </xs:documentation>
    </xs:annotation>
    <xs:restriction base="xs:anyURI"/>
  </xs:simpleType>
  
```
