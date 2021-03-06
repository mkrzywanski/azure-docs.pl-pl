---
title: OpenShift na platformie Azure — omówienie
description: Omówienie OpenShift na platformie Azure.
author: haroldwongms
manager: mdotson
ms.service: virtual-machines-linux
ms.subservice: workloads
ms.topic: how-to
ms.workload: infrastructure
ms.date: 05/7/2019
ms.author: haroldw
ms.openlocfilehash: 989c67d0d1b1cd8257da089ec9c31f247228018e
ms.sourcegitcommit: f353fe5acd9698aa31631f38dd32790d889b4dbb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/29/2020
ms.locfileid: "87367949"
---
# <a name="openshift-in-azure"></a>OpenShift na platformie Azure

OpenShift to otwarta i rozszerzalna platforma aplikacji kontenerów, która udostępnia platformę Docker i Kubernetes do przedsiębiorstwa.  

OpenShift obejmuje Kubernetes do organizowania i zarządzania kontenerami. Dodaje narzędzia skoncentrowane na deweloperach i oparte na operacjach, które umożliwiają:

- Szybkie opracowywanie aplikacji.
- Łatwe wdrażanie i skalowanie.
- Długoterminowa konserwacja dla zespołów i aplikacji.

Dostępnych jest wiele wersji OpenShift.  W tych wersjach już dziś są dostępne tylko dwie wersje, które klienci mogą wdrażać na platformie Azure: OpenShift kontenera platform i OKD (dawniej pochodzenie OpenShift).

## <a name="azure-red-hat-openshift"></a>Azure Red Hat OpenShift

Microsoft Azure Red Hat OpenShift to w pełni zarządzana oferta OpenShift działającego na platformie Azure. Ta usługa jest wspólnie zarządzana i obsługiwana przez firmę Microsoft i Red Hat. Aby uzyskać więcej informacji, zobacz dokumentację [usługi Azure Red Hat OpenShift](../../openshift/index.yml) .

## <a name="openshift-container-platform"></a>OpenShift Container Platform

Platforma kontenerów to [wersja komercyjna](https://www.openshift.com) przeznaczona dla przedsiębiorstw, obsługiwana przez firmę Red Hat. W tej wersji klienci kupują niezbędne uprawnienia dla platformy kontenera OpenShift i są odpowiedzialni za instalację i zarządzanie całą infrastrukturą.

Ze względu na to, że klienci są własnością całej platformy, mogą ją zainstalować w lokalnych centrach danych lub w chmurze publicznej (na przykład na platformie Azure).

## <a name="okd"></a>OKD

OKD to projekt typu ["open source"](https://www.okd.io/) OpenShift, który jest obsługiwany przez społeczność. OKD można zainstalować w CentOS lub Red Hat Enterprise Linux (RHEL).

## <a name="next-steps"></a>Następne kroki

- [Konfigurowanie typowych wymagań wstępnych dotyczących programu OpenShift na platformie Azure](./openshift-container-platform-3x-prerequisites.md)
- [Wdrażanie platformy kontenera OpenShift na platformie Azure](./openshift-container-platform-3x.md)
- [Wdróż niezarządzaną ofertę portalu OpenShift platformy kontenera](./openshift-container-platform-3x-marketplace-self-managed.md)
- [Wdróż OpenShift w Azure Stack](./openshift-azure-stack.md)
- [Zadania po wdrożeniu](./openshift-container-platform-3x-post-deployment.md)
- [Rozwiązywanie problemów z wdrażaniem OpenShift](./openshift-container-platform-3x-troubleshooting.md)
