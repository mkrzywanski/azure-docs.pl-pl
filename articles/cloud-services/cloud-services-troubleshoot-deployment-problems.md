---
title: Rozwiązywanie problemów z wdrażaniem usługi w chmurze | Microsoft Docs
description: Istnieje kilka typowych problemów, które można napotkać podczas wdrażania usługi w chmurze na platformie Azure. Ten artykuł zawiera rozwiązania dla niektórych z nich.
services: cloud-services
documentationcenter: ''
author: simonxjx
manager: dcscontentpm
editor: ''
tags: top-support-issue
ms.assetid: a18ae415-0d1c-4bc4-ab6c-c1ddea02c870
ms.service: cloud-services
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 06/15/2018
ms.author: v-six
ms.openlocfilehash: ccb08f853ae0f941dd5f9c0eca8c77f0f650905a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "71122750"
---
# <a name="troubleshoot-cloud-service-deployment-problems"></a>Rozwiązywanie problemów z wdrażaniem usługi w chmurze
Podczas wdrażania pakietu aplikacji usługi w chmurze na platformie Azure można uzyskać informacje dotyczące wdrożenia z okienka **Właściwości** w Azure Portal. Możesz użyć szczegółów w tym okienku, aby pomóc w rozwiązywaniu problemów z usługą w chmurze, a także podać te informacje do pomocy technicznej platformy Azure podczas otwierania nowego żądania pomocy technicznej.

Okienko **Właściwości** można znaleźć w następujący sposób:

* W Azure Portal kliknij wdrożenie usługi w chmurze, kliknij pozycję **wszystkie ustawienia**, a następnie kliknij pozycję **Właściwości**.

> [!NOTE]
> Zawartość okienka **Właściwości** można skopiować do schowka, klikając ikonę w prawym górnym rogu okienka.
>
>

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="problem-i-cannot-access-my-website-but-my-deployment-is-started-and-all-role-instances-are-ready"></a>Problem: nie można uzyskać dostępu do witryny sieci Web, ale wdrożenie zostało uruchomione i wszystkie wystąpienia roli są gotowe
Link adresu URL witryny sieci Web wyświetlany w portalu nie zawiera portu. Domyślnym portem dla witryn sieci Web jest 80. Jeśli aplikacja jest skonfigurowana do uruchamiania w innym porcie, podczas uzyskiwania dostępu do witryny sieci Web należy dodać poprawny numer portu.

1. W Azure Portal kliknij wdrożenie usługi w chmurze.
2. W okienku **właściwości** Azure Portal Sprawdź porty wystąpień roli (w obszarze **wejściowe punkty końcowe**).
3. Jeśli port nie jest 80, Dodaj poprawną wartość portu do adresu URL podczas uzyskiwania dostępu do aplikacji. Aby określić port inny niż domyślny, wpisz adres URL, po którym następuje dwukropek (:), a następnie numer portu, bez spacji.

## <a name="problem-my-role-instances-recycled-without-me-doing-anything"></a>Problem: moje wystąpienia roli są odtwarzane bez wykonywania jakichkolwiek czynności
Naprawianie usługi odbywa się automatycznie, gdy platforma Azure wykrywa węzły problemu i w związku z tym przenosi wystąpienia ról do nowych węzłów. W takim przypadku może być automatycznie wyświetlane odtwarzanie wystąpień ról. Aby dowiedzieć się, czy nastąpiła próba naprawy usługi:

1. W Azure Portal kliknij wdrożenie usługi w chmurze.
2. W okienku **właściwości** Azure Portal zapoznaj się z informacjami i określ, czy nastąpiło Naprawianie usługi w czasie, w którym zaobserwowano odtwarzanie ról.

Role również będą odtwarzane w trybie około raz miesięcznie w trakcie aktualizacji systemu operacyjnego hosta i systemu operacyjnego gościa.  
Aby uzyskać więcej informacji, zobacz wpis w blogu [Ponowne uruchamianie wystąpienia roli z powodu uaktualnień systemu operacyjnego](https://blogs.msdn.com/b/kwill/archive/2012/09/19/role-instance-restarts-due-to-os-upgrades.aspx)

## <a name="problem-i-cannot-do-a-vip-swap-and-receive-an-error"></a>Problem: nie można dokonać zamiany adresu VIP i otrzymać błąd
Zamiana adresu VIP nie jest dozwolona, jeśli aktualizacja wdrożenia jest w toku. Aktualizacje wdrożenia mogą odbywać się automatycznie, gdy:

* Dostępny jest nowy system operacyjny gościa, który jest konfigurowany do aktualizacji automatycznych.
* Naprawianie usługi.

Aby dowiedzieć się, czy aktualizacja automatyczna uniemożliwia przeprowadzenie wymiany adresu VIP:

1. W Azure Portal kliknij wdrożenie usługi w chmurze.
2. W okienku **właściwości** Azure Portal Sprawdź wartość **stan**. Jeśli jest **gotowa**, sprawdź **ostatnią operację** , aby zobaczyć, czy niedawno wystąpił, co może uniemożliwić zamianę adresu VIP.
3. Powtórz kroki 1 i 2 w przypadku wdrożenia produkcyjnego.
4. Jeśli aktualizacja automatyczna jest w toku, poczekaj na jej zakończenie przed podjęciem próby wykonania zamiany adresu VIP.

## <a name="problem-a-role-instance-is-looping-between-started-initializing-busy-and-stopped"></a>Problem: wystąpienie roli zawiera pętlę między uruchomioną, inicjowanie, zajętą i zatrzymaną
Ten stan może wskazywać na problem z kodem, pakietem lub plikiem konfiguracyjnym aplikacji. W takim przypadku powinno być możliwe wyświetlenie stanu co kilka minut, a Azure Portal może powiedzieć, jak **odtwarzanie**, **zajętość**lub **Inicjowanie**. Oznacza to, że wystąpił problem z aplikacją, która uniemożliwia uruchomienie wystąpienia roli.

Aby uzyskać więcej informacji na temat rozwiązywania tego problemu, zobacz wpis na blogu [dane diagnostyczne usługi Azure PaaS COMPUTE](https://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx) i [typowe problemy, które powodują](cloud-services-troubleshoot-common-issues-which-cause-roles-recycle.md)odtworzenie ról.

## <a name="problem-my-application-stopped-working"></a>Problem: moja aplikacja przestała działać
1. W Azure Portal kliknij wystąpienie roli.
2. W okienku **właściwości** Azure Portal należy wziąć pod uwagę następujące warunki, aby rozwiązać problem:
   * Jeśli wystąpienie roli zostało ostatnio zatrzymane (można sprawdzić wartość **licznika przerwań**), wdrożenie może zostać zaktualizowane. Poczekaj, aż wystąpienie roli wznowi działanie.
   * Jeśli wystąpienie roli jest **zajęte**, sprawdź kod aplikacji, aby sprawdzić, czy zdarzenie [StatusCheck](/previous-versions/azure/reference/ee758135(v=azure.100)) jest obsługiwane. Być może trzeba będzie dodać lub naprawić jakiś kod, który obsługuje to zdarzenie.
   * Zapoznaj się z scenariuszami dotyczącymi danych diagnostycznych i rozwiązywania problemów w blogu Publikuj [dane diagnostyki usługi Azure PaaS COMPUTE](https://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).

> [!WARNING]
> W przypadku odtwarzania usługi w chmurze należy zresetować właściwości wdrożenia, co skutecznie wymazuje informacje dotyczące oryginalnego problemu.
>
>

## <a name="next-steps"></a>Następne kroki
Zobacz więcej [artykułów do rozwiązywania problemów](https://docs.microsoft.com/azure/cloud-services/cloud-services-allocation-failures) dotyczących usług Cloud Services.

Aby dowiedzieć się, jak rozwiązywać problemy z rolą usługi w chmurze, korzystając z danych diagnostycznych dotyczących komputerów z usługą Azure PaaS, zobacz [Seria blogów Piotr Williamson](https://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).
