---
title: Zarządzanie miejscem na dysku w usłudze Azure HDInsight
description: Kroki rozwiązywania problemów i możliwe rozwiązania problemów podczas pracy z klastrami usługi Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: troubleshooting
ms.date: 02/17/2020
ms.openlocfilehash: 577bed7ce342be14a50077a3ffd841cd901b5b31
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "77473014"
---
# <a name="manage-disk-space-in-azure-hdinsight"></a>Zarządzanie miejscem na dysku w usłudze Azure HDInsight

W tym artykule opisano kroki rozwiązywania problemów oraz możliwe rozwiązania problemów występujących w przypadku współpracy z klastrami usługi Azure HDInsight.

## <a name="hive-log-configurations"></a>Konfiguracje dzienników Hive

1. W przeglądarce sieci Web przejdź do `https://CLUSTERNAME.azurehdinsight.net` lokalizacji, gdzie `CLUSTERNAME` jest nazwą klastra.

1. Przejdź do **Hive**  >  **konfiguracji**Hive Advanced  >  **Advanced**  >  **Advanced Hive-Log4J**. Zapoznaj się z następującymi ustawieniami:

    * `hive.root.logger=DEBUG,RFA`. Jest to wartość domyślna, modyfikując [poziom dziennika](https://logging.apache.org/log4j/2.x/log4j-api/apidocs/org/apache/logging/log4j/Level.html) w `INFO` celu drukowania mniejszych wpisów dzienników.

    * `log4jhive.log.maxfilesize=1024MB`. Jest to wartość domyślna, a w razie potrzeby zmodyfikuj ją.

    * `log4jhive.log.maxbackupindex=10`. Jest to wartość domyślna, a w razie potrzeby zmodyfikuj ją. Jeśli parametr został pominięty, wygenerowane pliki dziennika będą nieograniczone.

## <a name="yarn-log-configurations"></a>Konfiguracje dzienników przędzy

Przejrzyj następujące konfiguracje:

* Apache Ambari

    1. W przeglądarce sieci Web przejdź do `https://CLUSTERNAME.azurehdinsight.net` lokalizacji, gdzie `CLUSTERNAME` jest nazwą klastra.

    1. Przejdź do konfiguracji programu **Hive**—  >  **Configs**  >  **Zaawansowane**  >  **Menedżer zasobów**. Upewnij się, że jest zaznaczone pole wyboru **Włącz agregację dzienników** . Jeśli ta funkcja jest wyłączona, węzły nazw przechowują dzienniki lokalnie i nie agregują ich w magazynie zdalnym po ukończeniu lub zakończeniu działania aplikacji.

* Upewnij się, że rozmiar klastra jest odpowiedni dla obciążenia. Obciążenie mogło być niedawno zmienione lub zmieniono rozmiar klastra. [Skaluj](../hdinsight-scaling-best-practices.md) klaster w górę w celu dopasowania go do większego obciążenia.

* `/mnt/resource`mogą być wypełnione oddzielone pliki (tak jak w przypadku ponownego uruchomienia Menedżera zasobów). W razie potrzeby ręczne czyszczenie `/mnt/resource/hadoop/yarn/log` i `/mnt/resource/hadoop/yarn/local` .

## <a name="next-steps"></a>Następne kroki

Jeśli problem nie został wyświetlony lub nie można rozwiązać problemu, odwiedź jeden z następujących kanałów, aby uzyskać więcej pomocy:

* Uzyskaj odpowiedzi od ekspertów platformy Azure za pośrednictwem [pomocy technicznej dla społeczności platformy Azure](https://azure.microsoft.com/support/community/).

* Połącz się z programem [@AzureSupport](https://twitter.com/azuresupport) — oficjalnego konta Microsoft Azure, aby zwiększyć komfort obsługi klienta. Połączenie społeczności platformy Azure z właściwymi zasobami: odpowiedziami, wsparciem i ekspertami.

* Jeśli potrzebujesz więcej pomocy, możesz przesłać żądanie pomocy technicznej z [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Na pasku menu wybierz pozycję **Obsługa** , a następnie otwórz Centrum **pomocy i obsługi technicznej** . Aby uzyskać szczegółowe informacje, zapoznaj [się z tematem jak utworzyć żądanie pomocy technicznej platformy Azure](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). Dostęp do pomocy w zakresie zarządzania subskrypcjami i rozliczeń jest dostępny w ramach subskrypcji Microsoft Azure, a pomoc techniczna jest świadczona za pomocą jednego z [planów pomocy technicznej systemu Azure](https://azure.microsoft.com/support/plans/).
