---
title: Zrozumienie i rozwiązywanie błędów WebHCat w usłudze HDInsight — Azure
description: Dowiedz się, jak informacje o typowych błędach zwracanych przez WebHCat w usłudze HDInsight i sposobach ich rozwiązywania.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.custom: hdinsightactive
ms.date: 04/14/2020
ms.openlocfilehash: 021bfc0b87b0da800728eda26d9f5222bd52bc1e
ms.sourcegitcommit: 124f7f699b6a43314e63af0101cd788db995d1cb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/08/2020
ms.locfileid: "86086963"
---
# <a name="understand-and-resolve-errors-received-from-webhcat-on-hdinsight"></a>Understand and resolve errors received from WebHCat on HDInsight (Opis błędów i rozwiązywanie problemów dotyczących interfejsu API WebHCat w usłudze HDInsight)

Dowiedz się więcej o błędach odebranych podczas korzystania z usługi HDInsight w programie WebHCat i sposobach ich rozwiązywania. WebHCat jest używany wewnętrznie przez narzędzia po stronie klienta, takie jak Azure PowerShell i Data Lake Tools for Visual Studio.

## <a name="what-is-webhcat"></a>Co to jest WebHCat

[WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) to interfejs API REST dla warstwy zarządzania [HCatalog](https://cwiki.apache.org/confluence/display/Hive/HCatalog), tabeli i magazynu dla Apache Hadoop. Usługa WebHCat jest domyślnie włączona w klastrach usługi HDInsight i jest używana przez różne narzędzia do przesyłania zadań, uzyskiwania stanu zadania i tak dalej, bez konieczności logowania się do klastra.

## <a name="modifying-configuration"></a>Modyfikowanie konfiguracji

Niektóre błędy wymienione w tym dokumencie występują z powodu przekroczenia skonfigurowanego maksymalnego limitu. Gdy w kroku rozwiązanie wskażesz, że możesz zmienić wartość, Użyj usługi Apache Ambari (interfejs API sieci Web lub REST), aby zmodyfikować tę wartość. Aby uzyskać więcej informacji, zobacz [Zarządzanie usługą HDInsight przy użyciu platformy Apache Ambari](hdinsight-hadoop-manage-ambari.md)

### <a name="default-configuration"></a>Konfiguracja domyślna

W przypadku przekroczenia następujących wartości domyślnych można obniżyć wydajność WebHCat lub przyczynić się do błędów:

| Ustawienie | Wyniki działania | Wartość domyślna |
| --- | --- | --- |
| [przędzy. Scheduler. pojemność. Maximum — aplikacje][maximum-applications] |Maksymalna liczba zadań, które mogą być aktywne współbieżnie (oczekujące lub uruchomione) |10 000 |
| [templeton.exec. Max-procs][max-procs] |Maksymalna liczba żądań, które mogą być obsługiwane współbieżnie |20 |
| [MapReduce. jobhistory. max-age-MS][max-age-ms] |Liczba dni zachowywania historii zadań |7 dni |

## <a name="too-many-requests"></a>Zbyt wiele żądań

**Kod stanu HTTP**: 429

| Przyczyna | Rozwiązanie |
| --- | --- |
| Przekroczono maksymalną liczbę równoczesnych żądań obsłużonych przez WebHCat na minutę (domyślnie 20) |Zmniejsz obciążenie, aby upewnić się, że nie jest przesyłanych więcej niż maksymalna liczba równoczesnych żądań lub Zwiększ limit liczby żądań współbieżnych przez modyfikację `templeton.exec.max-procs` . Aby uzyskać więcej informacji, zobacz [modyfikowanie konfiguracji](#modifying-configuration) |

## <a name="server-unavailable"></a>Serwer niedostępny

**Kod stanu HTTP**: 503

| Przyczyna | Rozwiązanie |
| --- | --- |
| Ten kod stanu zwykle występuje podczas przejścia w tryb failover między podstawowym i pomocniczym węzła głównego klastra |Odczekaj dwie minuty, a następnie spróbuj ponownie wykonać operację. |

## <a name="bad-request-content-could-not-find-job"></a>Zła zawartość żądania: nie można odnaleźć zadania

**Kod stanu HTTP**: 400

| Przyczyna | Rozwiązanie |
| --- | --- |
| Szczegóły zadania zostały oczyszczone przez oczyszczarkę historii zadań |Domyślny okres przechowywania dla historii zadań wynosi 7 dni. Domyślny okres przechowywania można zmienić, modyfikując `mapreduce.jobhistory.max-age-ms` . Aby uzyskać więcej informacji, zobacz [modyfikowanie konfiguracji](#modifying-configuration) |
| Zadanie zostało przerwane z powodu przejścia w tryb failover |Przesyłanie zadania ponowienia przez maksymalnie dwie minuty |
| Użyto nieprawidłowego identyfikatora zadania |Sprawdź, czy identyfikator zadania jest poprawny |

## <a name="bad-gateway"></a>Zła brama

**Kod stanu HTTP**: 502

| Przyczyna | Rozwiązanie |
| --- | --- |
| Wewnętrzne wyrzucanie elementów bezużytecznych występuje w procesie WebHCat |Poczekaj na zakończenie odzyskiwania pamięci lub Uruchom ponownie usługę WebHCat |
| Przekroczono limit czasu podczas oczekiwania na odpowiedź z usługi ResourceManager. Ten błąd może wystąpić, gdy liczba aktywnych aplikacji przejdzie do skonfigurowanej wartości maksymalnej (domyślnie 10 000). |Zaczekaj na ukończenie aktualnie uruchomionych zadań lub Zwiększ limit liczby zadań współbieżnych przez modyfikację `yarn.scheduler.capacity.maximum-applications` . Aby uzyskać więcej informacji, zobacz sekcję [modyfikowanie konfiguracji](#modifying-configuration) . |
| Próba pobrania wszystkich zadań za pomocą wywołania [Get/Jobs](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference+Jobs) , gdy `Fields` jest ustawiona na`*` |Nie pobieraj *wszystkich* szczegółów zadania. Zamiast tego należy użyć `jobid` , aby pobrać szczegóły dotyczące zadań większych niż określony identyfikator zadania. Lub nie używaj`Fields` |
| Usługa WebHCat nie działa podczas pracy w trybie failover węzła głównego |Poczekaj dwie minuty i spróbuj ponownie wykonać operację |
| Liczba oczekujących zadań przesłanych przez WebHCat przekracza 500 |Zaczekaj na ukończenie aktualnie oczekujących zadań przed przesłaniem większej liczby zadań |

## <a name="next-steps"></a>Następne kroki

Jeśli problem nie został wyświetlony lub nie można rozwiązać problemu, odwiedź jeden z następujących kanałów, aby uzyskać więcej pomocy:

* Uzyskaj odpowiedzi od ekspertów platformy Azure za pośrednictwem [pomocy technicznej dla społeczności platformy Azure](https://azure.microsoft.com/support/community/).

* Połącz się z programem [@AzureSupport](https://twitter.com/azuresupport) — oficjalnego konta Microsoft Azure, aby zwiększyć komfort obsługi klienta. Połączenie społeczności platformy Azure z właściwymi zasobami: odpowiedziami, wsparciem i ekspertami.

* Jeśli potrzebujesz więcej pomocy, możesz przesłać żądanie pomocy technicznej z [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Na pasku menu wybierz pozycję **Obsługa** , a następnie otwórz Centrum **pomocy i obsługi technicznej** . Aby uzyskać szczegółowe informacje, zapoznaj [się z tematem jak utworzyć żądanie pomocy technicznej platformy Azure](https://docs.microsoft.com/azure/azure-portal/supportability/how-to-create-azure-support-request). Dostęp do pomocy w zakresie zarządzania subskrypcjami i rozliczeń jest dostępny w ramach subskrypcji Microsoft Azure, a pomoc techniczna jest świadczona za pomocą jednego z [planów pomocy technicznej systemu Azure](https://azure.microsoft.com/support/plans/).

[maximum-applications]: https://docs.cloudera.com/HDPDocuments/HDP2/HDP-2.1.3/bk_system-admin-guide/content/setting_application_limits.html
[max-procs]: https://cwiki.apache.org/confluence/display/Hive/WebHCat+Configure#WebHCatConfigure-WebHCatConfiguration
[max-age-ms]: https://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.6.0/ds_Hadoop/hadoop-mapreduce-client/hadoop-mapreduce-client-core/mapred-default.xml
