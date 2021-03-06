---
title: Połącz dane CEF z podglądem platformy Azure — Wersja zapoznawcza | Microsoft Docs
description: Połącz rozwiązanie zewnętrzne, które wysyła komunikaty Common Event format (CEF) do funkcji wskaźnikowej platformy Azure przy użyciu maszyny z systemem Linux jako serwera proxy.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/26/2019
ms.author: yelevin
ms.openlocfilehash: 34091e0c9f18cb87a240054f534f474710eb421d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "85563933"
---
# <a name="connect-your-external-solution-using-common-event-format"></a>Łączenie rozwiązania zewnętrznego przy użyciu typowego formatu zdarzeń


Po podłączeniu rozwiązania zewnętrznego, które wysyła komunikaty CEF, istnieją trzy kroki, które należy wykonać, aby nawiązać połączenie z platformą Azure — wskaźnik:

Krok 1. [łączenie CEF przez wdrożenie agenta](connect-cef-agent.md) krok 2: [wykonywanie kroków specyficznych dla rozwiązania](connect-cef-solution-config.md) krok 3: [Weryfikowanie łączności](connect-cef-verify.md)

W tym artykule opisano, jak działa połączenie, zawiera wymagania wstępne i przedstawiono kroki wdrażania agenta na rozwiązaniach zabezpieczeń, które wysyłają komunikaty w formacie Common Event format (CEF) na podstawie dziennika systemowego. 

> [!NOTE] 
> Dane są przechowywane w lokalizacji geograficznej obszaru roboczego, w którym jest uruchamiany wskaźnik platformy Azure.

Aby to połączenie było możliwe, należy wdrożyć agenta na dedykowanym komputerze z systemem Linux (maszynie wirtualnej lub lokalnie) do obsługi komunikacji między urządzeniem a platformą Azure. Na poniższym diagramie opisano konfigurację w przypadku maszyny wirtualnej z systemem Linux na platformie Azure.

 ![CEF na platformie Azure](./media/connect-cef/cef-syslog-azure.png)

Ta konfiguracja będzie również dostępna w przypadku korzystania z maszyny wirtualnej w innej chmurze lub na maszynie lokalnej. 

 ![CEF lokalnie](./media/connect-cef/cef-syslog-onprem.png)


## <a name="security-considerations"></a>Zagadnienia związane z zabezpieczeniami

Upewnij się, że skonfigurowano zabezpieczenia maszyny zgodnie z zasadami zabezpieczeń organizacji. Można na przykład skonfigurować sieć do dopasowania do zasad zabezpieczeń sieci firmowej i zmienić porty i protokoły w demoum, aby dostosować je do swoich wymagań. Aby ulepszyć konfigurację zabezpieczeń komputera, można użyć następujących instrukcji:  [bezpieczna maszyna wirtualna na platformie Azure](../virtual-machines/linux/security-policy.md), [najlepsze rozwiązania dotyczące zabezpieczeń sieci](../security/fundamentals/network-best-practices.md).

Aby można było korzystać z komunikacji TLS między rozwiązaniem zabezpieczeń a maszyną dziennika systemowego, należy skonfigurować demona dziennika systemu (rsyslog lub dziennika systemowego) do komunikacji w protokole TLS: [szyfrowanie ruchu dziennika systemu przy użyciu protokołu TLS-rsyslog](https://www.rsyslog.com/doc/v8-stable/tutorials/tls_cert_summary.html), [szyfrowanie komunikatów dzienników przy użyciu protokołu TLS — dziennik systemu](https://support.oneidentity.com/technical-documents/syslog-ng-open-source-edition/3.22/administration-guide/60#TOPIC-1209298).

 
## <a name="prerequisites"></a>Wymagania wstępne
Upewnij się, że maszyna z systemem Linux używaną jako serwer proxy ma jeden z następujących systemów operacyjnych:

- 64-bitowa
  - CentOS 6 i 7
  - Amazon Linux 2017,09
  - Oracle Linux 6 i 7
  - Red Hat Enterprise Linux Server 6 i 7
  - Debian GNU/Linux 8 i 9
  - Ubuntu Linux 14,04 LTS, 16,04 LTS i 18,04 LTS
  - SUSE Linux Enterprise Server 12
- 32-bitowa
   - CentOS 6
   - Oracle Linux 6
   - Red Hat Enterprise Linux Server 6
   - Debian GNU/Linux 8 i 9
   - Ubuntu Linux 14,04 LTS i 16,04 LTS
 
 - Wersje demona
   - Dziennik systemu — NG: 2,1-3.22.1
   - Rsyslog: V8
  
 - Obsługiwane są specyfikacje RFC dziennika systemowego
   - Dziennik systemowy RFC 3164
   - Dziennik systemowy RFC 5424
 
Upewnij się, że komputer spełnia również następujące wymagania: 
- Uprawnienia
    - Musisz mieć podwyższony poziom uprawnień (sudo) na swojej maszynie. 
- Wymagania dotyczące oprogramowania
    - Upewnij się, że na maszynie jest uruchomiony Język Python



## <a name="next-steps"></a>Następne kroki
W tym dokumencie przedstawiono sposób łączenia urządzeń CEF z platformą Azure — wskaźnikiem. Aby dowiedzieć się więcej na temat platformy Azure, zobacz następujące artykuły:
- Dowiedz się [, jak uzyskać wgląd w dane oraz potencjalne zagrożenia](quickstart-get-visibility.md).
- Rozpocznij [wykrywanie zagrożeń za pomocą platformy Azure — wskaźnik](tutorial-detect-threats.md).

