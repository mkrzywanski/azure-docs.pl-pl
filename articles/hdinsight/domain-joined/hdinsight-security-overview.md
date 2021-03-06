---
title: Omówienie zabezpieczeń przedsiębiorstwa w usłudze Azure HDInsight
description: Poznaj różne metody zapewniające bezpieczeństwo przedsiębiorstw w usłudze Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: overview
ms.custom: seoapr2020
ms.date: 04/20/2020
ms.openlocfilehash: 1869671b465b7175cf3160c41debc66cbd0818ad
ms.sourcegitcommit: bf8c447dada2b4c8af017ba7ca8bfd80f943d508
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/25/2020
ms.locfileid: "85367108"
---
# <a name="overview-of-enterprise-security-in-azure-hdinsight"></a>Omówienie zabezpieczeń przedsiębiorstwa w usłudze Azure HDInsight

Usługa Azure HDInsight oferuje różne metody zaspokajania potrzeb związanych z bezpieczeństwem przedsiębiorstwa. Większość z tych rozwiązań nie jest domyślnie aktywowana. Ta elastyczność umożliwia wybranie funkcji zabezpieczeń, które są najważniejsze dla Ciebie i pomaga uniknąć płacenia za niepotrzebne funkcje. Taka elastyczność oznacza również, że odpowiedzialność za zapewnienie, że poprawne rozwiązania zostały włączone dla instalacji i środowiska.

Ten artykuł dotyczy rozwiązań zabezpieczeń dzielących rozwiązania zabezpieczeń na cztery tradycyjne filary zabezpieczeń: zabezpieczenia obwodowe, uwierzytelnianie, autoryzacja i szyfrowanie.

W tym artykule wprowadzono również **pakiet Enterprise Security usługi Azure HDInsight (ESP)**, która zapewnia uwierzytelnianie oparte na Active Directory, obsługę wieloużytkownikom oraz kontrolę dostępu opartą na rolach dla klastrów usługi HDInsight.

## <a name="enterprise-security-pillars"></a>Filary zabezpieczeń przedsiębiorstwa

Jednym ze sposobów na wyszukanie zabezpieczeń przedsiębiorstwa jest dzielenie rozwiązań zabezpieczeń na cztery główne grupy w oparciu o typ formantu. Te grupy są również nazywane filarami zabezpieczeń i są następującymi typami: zabezpieczenia obwodowe, uwierzytelnianie, autoryzacja i szyfrowanie.

### <a name="perimeter-security"></a>Zabezpieczenia obwodu

Zabezpieczenia obwodowe w usłudze HDInsight są realizowane za poorednictwem [sieci wirtualnych](../hdinsight-plan-virtual-network-deployment.md). Administrator przedsiębiorstwa może utworzyć klaster w sieci wirtualnej (VNET) i użyć sieciowych grup zabezpieczeń (sieciowej grupy zabezpieczeń) w celu ograniczenia dostępu do sieci wirtualnej. Tylko dozwolone adresy IP w regułach sieciowej grupy zabezpieczeń dla ruchu przychodzącego mogą komunikować się z klastrem usługi HDInsight. Ta konfiguracja zapewnia ochronę obwodową.

Wszystkie klastry wdrożone w sieci wirtualnej również mają prywatny punkt końcowy. Punkt końcowy jest rozpoznawany jako prywatny adres IP w sieci wirtualnej na potrzeby prywatnego dostępu HTTP do bram klastra.

### <a name="authentication"></a>Authentication

[Pakiet Enterprise Security](apache-domain-joined-architecture.md) z usługi HDInsight zapewnia uwierzytelnianie oparte na Active Directoryach, obsługa przez wiele użytkowników oraz kontrolę dostępu opartą na rolach. Integracja Active Directory jest realizowana przy użyciu [Azure Active Directory Domain Services](../../active-directory-domain-services/overview.md). Dzięki tym funkcjom można utworzyć klaster usługi HDInsight przyłączony do domeny Active Directory. Następnie skonfiguruj listę pracowników w przedsiębiorstwie, którzy mogą uwierzytelniać się w klastrze.

W przypadku tej konfiguracji pracownicy przedsiębiorstwa mogą zalogować się do węzłów klastra przy użyciu ich poświadczeń domeny. Mogą również używać poświadczeń domeny do uwierzytelniania z innymi zatwierdzonymi punktami końcowymi. Takie jak Apache Ambari views, ODBC, JDBC, PowerShell i interfejsy API REST do współpracy z klastrem.

### <a name="authorization"></a>Autoryzacja

Najlepszym rozwiązaniem w przypadku większości przedsiębiorstw jest upewnienie się, że nie każdy pracownik ma pełny dostęp do wszystkich zasobów przedsiębiorstwa. Analogicznie, administrator może definiować zasady kontroli dostępu opartej na rolach dla zasobów klastra. Ta akcja jest dostępna tylko w klastrach ESP.

Administrator usługi Hadoop może skonfigurować kontrolę dostępu opartą na rolach (RBAC). Konfiguracje Secure [Hive](apache-domain-joined-run-hive.md), [HBase](apache-domain-joined-run-hbase.md)i [Kafka](apache-domain-joined-run-kafka.md) z dodatkami Apache Ranger. Skonfigurowanie zasad RBAC umożliwia kojarzenie uprawnień z rolą w organizacji. Ta warstwa abstrakcji ułatwia zapewnienie, że osoby mają tylko uprawnienia niezbędne do wykonania swoich obowiązków służbowych. Ranger umożliwia także inspekcję dostępu do danych pracowników i wszelkich zmian dokonanych w zasadach kontroli dostępu.

Na przykład administrator może skonfigurować środowisko [Apache Ranger](https://ranger.apache.org/) do ustawiania zasad kontroli dostępu dla usługi Hive. Ta funkcja zapewnia filtrowanie na poziomie wiersza i na poziomie kolumny (Maskowanie danych). I filtruje dane poufne przed nieautoryzowanymi użytkownikami.

### <a name="auditing"></a>Inspekcja

Inspekcja dostępu do zasobów klastra jest niezbędna do śledzenia nieautoryzowanego lub niezamierzonego dostępu do zasobów. Jest to ważne, aby chronić zasoby klastra przed nieautoryzowanym dostępem.

Administrator może wyświetlić i zgłosić cały dostęp do zasobów i danych klastra usługi HDInsight. Administrator może wyświetlać i raportować zmiany zasad kontroli dostępu.

Aby uzyskać dostęp do dzienników inspekcji oprogramowania Apache Ranger i Ambari oraz dzienników dostępu SSH, [włącz Azure monitor](../hdinsight-hadoop-oms-log-analytics-tutorial.md#cluster-auditing) i Wyświetl tabele, które udostępniają rekordy inspekcji.

### <a name="encryption"></a>Szyfrowanie

Ochrona danych jest istotna dla spełnienia wymagań dotyczących zabezpieczeń i zgodności w organizacji. Wraz z ograniczaniem dostępu do danych przed nieautoryzowanymi pracownikami należy go zaszyfrować.

Usługa Azure Storage i Data Lake Storage Gen1/Gen2 obsługują przezroczyste [szyfrowanie](../../storage/common/storage-service-encryption.md) po stronie serwera w stanie spoczynku. Bezpieczne klastry usługi HDInsight bezproblemowo współpracują z szyfrowaniem danych po stronie serwera.

### <a name="compliance"></a>Zgodność

Oferty zgodności z platformą Azure są oparte na różnych typach gwarancji, w tym w formalnych certyfikatach. Ponadto poświadczenia, walidacje i autoryzacje. Oceny opracowane przez niezależne firmy audytorów innych firm. Umowne zmiany, samooceny i dokumenty wskazówki klienta utworzone przez firmę Microsoft. Informacje o zgodności usługi HDInsight można znaleźć w [Centrum zaufania firmy Microsoft](https://www.microsoft.com/trust-center) i [Omówienie zgodności Microsoft Azure](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942).

## <a name="shared-responsibility-model"></a>Model wspólnej odpowiedzialności

Poniższy obraz podsumowuje główne obszary zabezpieczeń systemu i dostępne dla Ciebie rozwiązania zabezpieczeń. Wyróżnia także, które obszary zabezpieczeń są odpowiedzialne za klienta. I które obszary są odpowiedzialne za usługę HDInsight jako usługodawcę.

![Udostępniony diagram obowiązków usługi HDInsight](./media/hdinsight-security-overview/hdinsight-shared-responsibility.png)

Poniższa tabela zawiera linki do zasobów dla każdego typu rozwiązania zabezpieczeń.

| Obszar zabezpieczeń | Dostępne rozwiązania | Osoba odpowiedzialna |
|---|---|---|
| Zabezpieczenia dostępu do danych | Konfigurowanie [kontroli dostępu listy ACL](../../storage/blobs/data-lake-storage-access-control.md) dla Azure Data Lake Storage Gen1 i Gen2  | Klient |
|  | Włącz Właściwość ["wymagany bezpieczny transfer"](../../storage/common/storage-require-secure-transfer.md) na kontach magazynu. | Klient |
|  | Konfigurowanie zapór i sieci wirtualnych [usługi Azure Storage](../../storage/common/storage-network-security.md) | Klient |
|  | Konfigurowanie [punktów końcowych usługi sieci wirtualnej platformy Azure](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview) dla Cosmos DB i [usługi Azure SQL DB](https://docs.microsoft.com/azure/sql-database/sql-database-vnet-service-endpoint-rule-overview) | Klient |
|  | Upewnij się, że [szyfrowanie TLS](../../storage/common/storage-security-tls.md) jest włączone na potrzeby przesyłania danych. | Klient |
|  | Konfigurowanie [kluczy zarządzanych przez klienta](../../storage/common/storage-encryption-keys-portal.md) do szyfrowania za pomocą usługi Azure Storage | Klient |
|  | Kontroluj dostęp do danych przez pomoc techniczną platformy Azure przy użyciu [skrytki klienta](https://docs.microsoft.com/azure/security/fundamentals/customer-lockbox-overview) | Klient |
| Zabezpieczenia aplikacji i oprogramowania pośredniczącego | Integracja z usługą AAD — DS i [Konfigurowanie uwierzytelniania](apache-domain-joined-configure-using-azure-adds.md) | Klient |
|  | Konfigurowanie zasad [autoryzacji Apache Ranger](apache-domain-joined-run-hive.md) | Klient |
|  | Korzystanie z [dzienników Azure monitor](../hdinsight-hadoop-oms-log-analytics-tutorial.md) | Klient |
| Zabezpieczenia systemu operacyjnego | Tworzenie klastrów z najnowszym bezpiecznym obrazem podstawowym | Klient |
|  | Zapewnianie [stosowania poprawek systemu operacyjnego](../hdinsight-os-patching.md) w regularnych odstępach czasu | Klient |
| Bezpieczeństwo sieci | Konfigurowanie [sieci wirtualnej](../hdinsight-plan-virtual-network-deployment.md) |
|  | Skonfiguruj [reguły sieciowej grupy zabezpieczeń (sieciowej grupy zabezpieczeń) dla ruchu przychodzącego](../control-network-traffic.md) | Klient |
|  | Konfigurowanie [ograniczenia ruchu wychodzącego](../hdinsight-restrict-outbound-traffic.md) za pomocą zapory | Klient |
| Zwirtualizowana infrastruktura | Nie dotyczy | HDInsight (dostawca usług w chmurze) |
| Zabezpieczenia infrastruktury fizycznej | Nie dotyczy | HDInsight (dostawca usług w chmurze) |

## <a name="next-steps"></a>Następne kroki

* [Planowanie klastrów usługi HDInsight przy użyciu ESP](apache-domain-joined-architecture.md)
* [Konfigurowanie klastrów usługi HDInsight przy użyciu ESP](apache-domain-joined-configure.md)
* [Zarządzanie klastrami usługi HDInsight przy użyciu protokołu ESP](apache-domain-joined-manage.md)
