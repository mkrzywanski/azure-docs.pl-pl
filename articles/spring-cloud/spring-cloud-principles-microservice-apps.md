---
title: Java i podstawowy system operacyjny dla aplikacji mikrousług w chmurze ze sprężyną Azure
description: Zasady dotyczące utrzymania zdrowego środowiska Java i podstawowego systemu operacyjnego na potrzeby aplikacji mikrousług w chmurze ze sprężyną Azure
author: MikeDodaro
ms.author: brendm
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 05/27/2020
ms.custom: devx-track-java
ms.openlocfilehash: a8e1d43138e0b7481ebb89d747fa26df9470a09f
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/23/2020
ms.locfileid: "87037139"
---
# <a name="java-and-base-os-for-spring-microservice-apps"></a>Java i podstawowy system operacyjny dla aplikacji mikrousług usługi Spring
Poniżej znajdują się zasady dotyczące utrzymania zdrowego środowiska Java i podstawowego systemu operacyjnego pod kątem aplikacji z mikrousług.
## <a name="principles-for-healthy-java-and-base-os"></a>Zasady dotyczące zdrowego środowiska Java i podstawowego systemu operacyjnego
* Jest to ten sam podstawowy system operacyjny w warstwach — Podstawowa | Standardowa | Tytułu.
    * Obecnie aplikacje w chmurze z systemem Azure wiosną używają kombinacji Debian 10 i Ubuntu 18,04.
    * Usługa kompilacji VMware używa Ubuntu 18,04.
* Jest to ten sam podstawowy system operacyjny niezależnie od punktu początkowego wdrożenia — Źródło | Słoik
    * Obecnie aplikacje w chmurze z systemem Azure wiosną używają kombinacji Debian 10 i Ubuntu 18,04.
* Podstawowy system operacyjny nie może korzystać z luk w zabezpieczeniach.
    * Podstawowy system operacyjny Debian 10 ma 147 Otwórz CVEs.
    * Podstawowy system operacyjny Ubuntu 18,04 ma 132 Otwórz CVEs.
* Należy używać środowiska JRE — bezobsługowy.
    * Obecnie aplikacje w chmurze ze sprężyną Azure używają JDK. JRE — bezobsługowy obraz.
* Należy używać najnowszych kompilacji języka Java.
    * Obecnie aplikacje w chmurze Azure wiosną używają środowiska Java 8 Build 242. To jest nieaktualna kompilacja.
 
Systemy Azul będą stale skanowane pod kątem zmian w podstawowych systemach operacyjnych i zachować Aktualności ostatnio utworzonych obrazów. Chmura sprężynowa platformy Azure szuka zmian w obrazach i ciągle aktualizuje je w ramach wdrożeń.
 
## <a name="faq-for-azure-spring-cloud"></a>Często zadawane pytania dotyczące usługi Azure Spring Cloud

* Które wersje środowiska Java są obsługiwane? Wersja główna i numer kompilacji.
    * Obsługa wersji LTS — Java 8 i 11.
    * Używa najnowszej kompilacji — na przykład teraz, Java 8 kompilacja 252 i Java 11 kompilacja 7.
* Kto utworzył te środowiska uruchomieniowe Java?
    * Systemy Azul.
* Co to jest podstawowy system operacyjny dla obrazów?
    * Ubuntu 20,04 LTS (Fossay główne). Aplikacje będą nadal znajdować się w najnowszej wersji LTS Ubuntu.
    * Zobacz [Ubuntu 20,04 LTS (Fossay główne)](http://releases.ubuntu.com/focal/)
* Jak można pobrać obsługiwane środowisko uruchomieniowe języka Java na potrzeby lokalnego tworzenia oprogramowania? 
    * Zobacz [Instalowanie JDK dla platformy Azure i Azure Stack](https://docs.microsoft.com/azure/developer/java/fundamentals/java-jdk-install)
* Jak mogę uzyskać pomoc techniczną dotyczącą problemów na poziomie środowiska uruchomieniowego Java?
    * Otwórz bilet pomocy technicznej z pomocą techniczną platformy Azure.
 
## <a name="default-deployment-on-azure-spring-cloud"></a>Wdrożenie domyślne w chmurze Azure wiosennej

> ![Wdrożenie domyślne](media/spring-cloud-principles/spring-cloud-default-deployment.png)
 
## <a name="next-steps"></a>Następne kroki
* [Szybki Start: uruchamianie istniejącej aplikacji w chmurze platformy Azure przy użyciu Azure Portal](spring-cloud-quickstart-launch-app-portal.md)
* [Długoterminowa obsługa języka Java dla platformy Azure i Azure Stack](https://docs.microsoft.com/azure/developer/java/fundamentals/java-jdk-long-term-support)
