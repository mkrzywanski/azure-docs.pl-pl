---
title: Przesyłaj strumieniowo dane jako dane wejściowe do Azure Stream Analytics
description: Dowiedz się więcej na temat konfigurowania połączenia danych w Azure Stream Analytics. Wejścia obejmują strumień danych z zdarzeń, a także dane referencyjne.
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 01/17/2020
ms.openlocfilehash: 52f333a8e39dfd8f68666e6438a7d40414b6f958
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "83701419"
---
# <a name="stream-data-as-input-into-stream-analytics"></a>Przesyłaj strumieniowo dane jako dane wejściowe do Stream Analytics

Stream Analytics ma integrację pierwszej klasy z strumieniami danych platformy Azure jako dane wejściowe z trzech rodzajów zasobów:

- [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/)
- [Azure IoT Hub](https://azure.microsoft.com/services/iot-hub/) 
- [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/) 

Te zasoby wejściowe mogą znajdować się w tej samej subskrypcji platformy Azure co Stream Analytics zadania lub innej subskrypcji.

### <a name="compression"></a>Kompresja

Stream Analytics obsługuje kompresję między wszystkimi źródłami wejściowymi strumieni danych. Obsługiwane typy kompresji: brak, GZip i Wklęśnięcie kompresji. Obsługa kompresji nie jest dostępna dla danych referencyjnych. Jeśli format wejściowy to dane Avro, które są skompresowane, są obsługiwane w sposób niewidoczny dla użytkownika. Nie musisz określać typu kompresji z serializacji Avro. 

## <a name="create-edit-or-test-inputs"></a>Tworzenie, edytowanie lub testowanie danych wejściowych

Za pomocą [Azure Portal](stream-analytics-quick-create-portal.md), [Visual Studio](stream-analytics-quick-create-vs.md)i [Visual Studio Code](quick-create-vs-code.md) można dodawać i edytować istniejące dane wejściowe zadania przesyłania strumieniowego oraz wyświetlać je. Możesz również testować połączenia wejściowe i [testować zapytania](stream-analytics-manage-job.md#test-your-query) z przykładowych danych z Azure Portal, [Visual Studio](stream-analytics-vs-tools-local-run.md)i [Visual Studio Code](visual-studio-code-local-run.md). Podczas pisania zapytania należy wyświetlić listę danych wejściowych w klauzuli FROM. Listę dostępnych danych wejściowych można uzyskać ze strony **zapytania** w portalu. Jeśli chcesz użyć wielu danych wejściowych, możesz `JOIN` je lub napisać wiele `SELECT` zapytań.


## <a name="stream-data-from-event-hubs"></a>Przesyłanie strumieniowe danych z usługi Event Hubs

Usługa Azure Event Hubs zapewnia do wysoce skalowalnych, wyskakujących zdarzeń publikowania i subskrybowania. Centrum zdarzeń może zbierać miliony zdarzeń na sekundę, dzięki czemu można przetwarzać i analizować ogromne ilości danych wytworzonych przez połączone urządzenia i aplikacje. Razem Event Hubs i Stream Analytics zapewniają kompleksowe rozwiązanie do analizy w czasie rzeczywistym. Event Hubs umożliwia podawanie zdarzeń na platformie Azure w czasie rzeczywistym, a zadania Stream Analytics mogą przetwarzać te zdarzenia w czasie rzeczywistym. Można na przykład wysyłać do Event Hubs kliknięcia sieci Web, odczyty czujników lub zdarzenia dziennika online. Następnie można utworzyć Stream Analytics zadania, aby użyć Event Hubs jako strumieni danych wejściowych dla filtrowania w czasie rzeczywistym, agregowania i korelacji.

`EventEnqueuedUtcTime`to sygnatura czasowa przybycia zdarzenia w centrum zdarzeń i jest domyślną sygnaturą czasową zdarzeń pochodzących z Event Hubs do Stream Analytics. Aby przetworzyć dane jako strumień przy użyciu sygnatury czasowej w ładunku zdarzenia, należy użyć słowa kluczowego [timestamp by](https://docs.microsoft.com/stream-analytics-query/timestamp-by-azure-stream-analytics) .

### <a name="event-hubs-consumer-groups"></a>Event Hubs grupy konsumentów

Każde Stream Analytics dane wejściowe centrum zdarzeń należy skonfigurować tak, aby miało własną grupę odbiorców. Gdy zadanie zawiera samosprzężenie lub ma wiele danych wejściowych, niektóre dane wejściowe mogą zostać odczytane przez więcej niż jeden czytnik podrzędny. Ta sytuacja ma wpływ na liczbę czytników w pojedynczej grupie odbiorców. Aby uniknąć przekroczenia limitu Event Hubs pięciu czytelników na każdą partycję, najlepszym rozwiązaniem jest wyznaczanie grupy odbiorców dla każdego zadania Stream Analytics. Istnieje również limit 20 grup odbiorców dla centrum zdarzeń warstwy Standardowa. Aby uzyskać więcej informacji, zobacz [Rozwiązywanie problemów Azure Stream Analytics danych wejściowych](stream-analytics-troubleshoot-input.md).

### <a name="create-an-input-from-event-hubs"></a>Utwórz dane wejściowe z Event Hubs

W poniższej tabeli opisano każdą właściwość na stronie **nowe dane wejściowe** w Azure Portal do przesyłania strumieniowego danych wejściowych z centrum zdarzeń:

| Właściwość | Opis |
| --- | --- |
| **Alias wejściowy** |Przyjazna nazwa używana w zapytaniu zadania do odwoływania się do tych danych wejściowych. |
| **Subskrypcja** | Wybierz subskrypcję, w której znajduje się zasób centrum zdarzeń. | 
| **Przestrzeń nazw centrum zdarzeń** | Przestrzeń nazw centrum zdarzeń to kontener dla zestawu jednostek obsługi komunikatów. Podczas tworzenia nowego centrum zdarzeń należy również utworzyć przestrzeń nazw. |
| **Nazwa centrum zdarzeń** | Nazwa centrum zdarzeń do użycia jako dane wejściowe. |
| **Nazwa zasad centrum zdarzeń** | Zasady dostępu współdzielonego zapewniające dostęp do centrum zdarzeń. Każda zasada dostępu współdzielonego ma określoną nazwę, uprawnienia oraz klucze dostępu. Ta opcja jest wypełniana automatycznie, chyba że zostanie wybrana opcja ręcznego dostarczania ustawień centrum zdarzeń.|
| **Grupa odbiorców centrum zdarzeń** (zalecane) | Zdecydowanie zaleca się użycie odrębnej grupy odbiorców dla każdego zadania Stream Analytics. Ten ciąg identyfikuje grupę odbiorców, która ma być używana do pozyskiwania danych z centrum zdarzeń. Jeśli grupa odbiorców nie zostanie określona, zadanie Stream Analytics używa $Default grupy odbiorców.  |
| **Klucz partycji** | Jeśli dane wejściowe są podzielone na partycje przez właściwość, można dodać nazwę tej właściwości. Klucze partycji są opcjonalne i są używane do poprawiania wydajności zapytania, jeśli zawiera klauzulę PARTITION BY lub GROUP BY dla tej właściwości. |
| **Format serializacji zdarzeń** | Format serializacji (JSON, CSV, Avro lub [inny (protobuf, XML, własny,...)](custom-deserializer.md)) przychodzącego strumienia danych.  Upewnij się, że format JSON jest wyrównany ze specyfikacją i nie zawiera wiodących wartości 0 dla liczb dziesiętnych. |
| **Encoding** | UTF-8 jest obecnie jedynym obsługiwanym formatem kodowania. |
| **Typ kompresji zdarzenia** | Typ kompresji używany do odczytywania przychodzącego strumienia danych, na przykład None (wartość domyślna), GZip lub Wklęśnięcie. |

Gdy dane pochodzą z danych wejściowych strumienia centrum zdarzeń, masz dostęp do następujących pól metadanych w kwerendzie Stream Analytics:

| Właściwość | Opis |
| --- | --- |
| **EventProcessedUtcTime** |Data i godzina przetworzenia zdarzenia przez Stream Analytics. |
| **EventEnqueuedUtcTime** |Data i godzina odebrania zdarzenia przez Event Hubs. |
| **PartitionId** |Identyfikator partycji na podstawie zera dla karty wejściowej. |

Na przykład przy użyciu tych pól można napisać zapytanie podobne do poniższego przykładu:

```sql
SELECT
    EventProcessedUtcTime,
    EventEnqueuedUtcTime,
    PartitionId
FROM Input
```

> [!NOTE]
> W przypadku używania centrum zdarzeń jako punktu końcowego dla IoT Hub tras można uzyskać dostęp do metadanych IoT Hub przy użyciu [funkcji GetMetadataPropertyValue](https://docs.microsoft.com/stream-analytics-query/getmetadatapropertyvalue).
> 

## <a name="stream-data-from-iot-hub"></a>Przesyłanie strumieniowe danych z IoT Hub

IoT Hub platformy Azure to wysoce skalowalna usługa do obsługi zdarzeń publikowania/subskrybowania, zoptymalizowana pod kątem scenariuszy IoT.

Domyślna sygnatura czasowa zdarzeń pochodzących z IoT Hub w Stream Analytics jest sygnaturą czasową, jaką zdarzenie dotarło do IoT Hub, czyli `EventEnqueuedUtcTime` . Aby przetworzyć dane jako strumień przy użyciu sygnatury czasowej w ładunku zdarzenia, należy użyć słowa kluczowego [timestamp by](https://docs.microsoft.com/stream-analytics-query/timestamp-by-azure-stream-analytics) .

### <a name="iot-hub-consumer-groups"></a>Grupy konsumentów Centrum IoT Hub

Należy skonfigurować wszystkie Stream Analytics IoT Hub dane wejściowe, aby miały własną grupę odbiorców. Gdy zadanie zawiera samosprzężenie lub gdy ma wiele danych wejściowych, niektóre dane wejściowe mogą zostać odczytane przez więcej niż jeden czytnik podrzędny. Ta sytuacja ma wpływ na liczbę czytników w pojedynczej grupie odbiorców. Aby uniknąć przekroczenia limitu IoT Hub platformy Azure dla pięciu czytelników na jedną partycję, najlepszym rozwiązaniem jest wyznaczanie grupy odbiorców dla każdego zadania Stream Analytics.

### <a name="configure-an-iot-hub-as-a-data-stream-input"></a>Skonfiguruj IoT Hub jako dane wejściowe strumienia danych

W poniższej tabeli opisano każdą właściwość na stronie **nowe dane wejściowe** w Azure Portal podczas konfigurowania IoT Hub jako danych wejściowych strumienia.

| Właściwość | Opis |
| --- | --- |
| **Alias wejściowy** | Przyjazna nazwa używana w zapytaniu zadania do odwoływania się do tych danych wejściowych.|
| **Subskrypcja** | Wybierz subskrypcję, w ramach której istnieje zasób IoT Hub. | 
| **IoT Hub** | Nazwa IoT Hub do użycia jako dane wejściowe. |
| **Punkt końcowy** | Punkt końcowy IoT Hub.|
| **Nazwa zasad dostępu współdzielonego** | Zasady dostępu współdzielonego zapewniające dostęp do IoT Hub. Każda zasada dostępu współdzielonego ma określoną nazwę, uprawnienia oraz klucze dostępu. |
| **Klucz zasad dostępu współdzielonego** | Współużytkowany klucz dostępu używany do autoryzacji dostępu do IoT Hub.  Ta opcja jest wypełniana automatycznie, chyba że zostanie wybrana opcja ręcznego dostarczania ustawień Centrum IoT. |
| **Grupa konsumentów** | Zdecydowanie zaleca się użycie innej grupy odbiorców dla każdego zadania Stream Analytics. Grupa konsumentów służy do pozyskiwania danych z IoT Hub. Stream Analytics używa grupy konsumentów $Default, chyba że określisz inaczej.  |
| **Klucz partycji** | Jeśli dane wejściowe są podzielone na partycje przez właściwość, można dodać nazwę tej właściwości. Klucze partycji są opcjonalne i są używane do poprawiania wydajności zapytania, jeśli zawiera klauzulę PARTITION BY lub GROUP BY dla tej właściwości. |
| **Format serializacji zdarzeń** | Format serializacji (JSON, CSV, Avro lub [inny (protobuf, XML, własny,...)](custom-deserializer.md)) przychodzącego strumienia danych.  Upewnij się, że format JSON jest wyrównany ze specyfikacją i nie zawiera wiodących wartości 0 dla liczb dziesiętnych. |
| **Encoding** | UTF-8 jest obecnie jedynym obsługiwanym formatem kodowania. |
| **Typ kompresji zdarzenia** | Typ kompresji używany do odczytywania przychodzącego strumienia danych, na przykład None (wartość domyślna), GZip lub Wklęśnięcie. |


W przypadku korzystania z danych przesyłanych strumieniowo z IoT Hub masz dostęp do następujących pól metadanych w zapytaniu Stream Analytics:

| Właściwość | Opis |
| --- | --- |
| **EventProcessedUtcTime** | Data i godzina przetworzenia zdarzenia. |
| **EventEnqueuedUtcTime** | Data i godzina odebrania zdarzenia przez IoT Hub. |
| **PartitionId** | Identyfikator partycji na podstawie zera dla karty wejściowej. |
| **IoTHub. MessageId** | Identyfikator używany do skorelowania komunikacji dwukierunkowej w IoT Hub. |
| **IoTHub. identyfikator korelacji** | Identyfikator, który jest używany w odpowiedziach komunikatów i opiniach w IoT Hub. |
| **IoTHub. ConnectionDeviceId** | Identyfikator uwierzytelniania używany do wysłania tej wiadomości. Ta wartość jest podawana w komunikatach servicebound przez IoT Hub. |
| **IoTHub. ConnectionDeviceGenerationId** | Identyfikator generacji urządzenia uwierzytelnionego, które zostało użyte do wysłania tej wiadomości. Ta wartość jest podawana w komunikatach servicebound przez IoT Hub. |
| **IoTHub. EnqueuedTime** | Czas odebrania komunikatu przez IoT Hub. |


## <a name="stream-data-from-blob-storage"></a>Przesyłanie strumieniowe danych z usługi BLOB Storage
W przypadku scenariuszy zawierających duże ilości danych bez struktury do przechowywania w chmurze usługa Azure Blob Storage oferuje ekonomiczne i skalowalne rozwiązanie. Dane w magazynie obiektów BLOB są zwykle uznawane za dane w spoczynku; Jednak dane obiektów BLOB mogą być przetwarzane jako strumień danych przez Stream Analytics. 

Przetwarzanie dziennika jest często używanym scenariuszem do używania danych wejściowych usługi BLOB Storage z Stream Analytics. W tym scenariuszu pliki danych telemetrycznych zostały przechwycone z systemu i muszą zostać przeanalizowane i przetworzone w celu wyodrębnienia istotnych danych.

Domyślna sygnatura czasowa zdarzeń magazynu obiektów BLOB w Stream Analytics to sygnatura czasowa ostatniej modyfikacji obiektu BLOB, czyli `BlobLastModifiedUtcTime` . Jeśli obiekt BLOB zostanie przekazany do konta magazynu o godzinie 13:00, a zadanie Azure Stream Analytics zostanie uruchomione przy użyciu opcji *teraz* o godzinie 13:01, obiekt BLOB nie zostanie pobrany jako zmodyfikowany czas poza okresem wykonywania zadania.

Jeśli obiekt BLOB zostanie przekazany do kontenera konta magazynu o godzinie 13:00, a zadanie Azure Stream Analytics zostanie rozpoczęte przy użyciu *czasu niestandardowego* o godzinie 13:00 lub starszej, obiekt BLOB zostanie pobrany jako jego zmodyfikowany czas w okresie wykonywania zadania.

Jeśli zadanie Azure Stream Analytics zostało uruchomione przy użyciu *teraz* o godzinie 13:00, a obiekt BLOB zostanie przekazany do kontenera konta magazynu o godzinie 13:01, Azure Stream Analytics pobierze obiekt BLOB. Sygnatura czasowa przypisana do każdego obiektu BLOB jest oparta wyłącznie na `BlobLastModifiedTime` . Folder, w którym znajduje się obiekt BLOB, nie ma związku z przypisaną sygnaturą czasową. Na przykład jeśli istnieje obiekt BLOB *2019/10-01/00/b1.txt* z `BlobLastModifiedTime` 2019-11-11, sygnatura czasowa przypisana do tego obiektu BLOB to 2019-11-11.

Aby przetworzyć dane jako strumień przy użyciu sygnatury czasowej w ładunku zdarzenia, należy użyć słowa kluczowego [timestamp by](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference) . Zadanie Stream Analytics pobiera dane z danych wejściowych z usługi Azure Blob Storage co sekundę, jeśli plik BLOB jest dostępny. Jeśli plik BLOB jest niedostępny, istnieje wykładnicza wycofywania z maksymalnym opóźnieniem wynoszącym 90 sekund.

Dane wejściowe w formacie CSV wymagają wiersza nagłówka, aby zdefiniować pola dla zestawu danych, a wszystkie pola wiersza nagłówka muszą być unikatowe.

> [!NOTE]
> Stream Analytics nie obsługuje dodawania zawartości do istniejącego pliku obiektu BLOB. Stream Analytics będzie przeglądać każdy plik tylko raz, a wszelkie zmiany wprowadzone w pliku po odczytaniu zadania nie są przetwarzane. Najlepszym rozwiązaniem jest przekazanie wszystkich danych dla pliku obiektu BLOB jednocześnie, a następnie dodanie dodatkowych zdarzeń do innego, nowego pliku obiektu BLOB.

W scenariuszach, w których wiele obiektów BLOB jest w sposób ciągły dodawane i Stream Analytics przetwarza obiekty blob w miarę ich dodawania, istnieje możliwość, że niektóre obiekty blob są pomijane w rzadkich przypadkach ze względu na stopień szczegółowości `BlobLastModifiedTime` . Można to ograniczyć przez przekazywanie obiektów blob z co najmniej dwiema sekundami. Jeśli ta opcja nie jest możliwa, można użyć Event Hubs do przesyłania strumieniowego dużych ilości zdarzeń.

### <a name="configure-blob-storage-as-a-stream-input"></a>Konfigurowanie magazynu obiektów BLOB jako danych wejściowych strumienia 

W poniższej tabeli opisano każdą właściwość na stronie **nowe dane wejściowe** w Azure Portal podczas konfigurowania magazynu obiektów BLOB jako danych wejściowych strumienia.

| Właściwość | Opis |
| --- | --- |
| **Alias wejściowy** | Przyjazna nazwa używana w zapytaniu zadania do odwoływania się do tych danych wejściowych. |
| **Subskrypcja** | Wybierz subskrypcję, w ramach której istnieje zasób IoT Hub. | 
| **Konto magazynu** | Nazwa konta magazynu, w którym znajdują się pliki obiektów BLOB. |
| **Klucz konta magazynu** | Klucz tajny skojarzony z kontem magazynu. Ta opcja jest wypełniana automatycznie, chyba że zostanie wybrana opcja ręcznego dostarczania ustawień magazynu obiektów BLOB. |
| **Kontener** | Kontener dla danych wejściowych obiektu BLOB. Kontenery zapewniają logiczne grupowanie obiektów BLOB przechowywanych w Blob service Microsoft Azure. Podczas przekazywania obiektu BLOB do usługi Azure Blob Storage należy określić kontener dla tego obiektu BLOB. Możesz wybrać opcję **Użyj istniejącego** kontenera lub **utworzyć nowy** , aby utworzyć nowy kontener.|
| **Wzorzec ścieżki** (opcjonalnie) | Ścieżka pliku używana do lokalizowania obiektów BLOB w określonym kontenerze. Jeśli chcesz odczytywać obiekty blob z katalogu głównego kontenera, nie ustawiaj wzorca ścieżki. W ścieżce można określić jedno lub więcej wystąpień następujących trzech zmiennych: `{date}` , `{time}` , lub`{partition}`<br/><br/>Przykład 1:`cluster1/logs/{date}/{time}/{partition}`<br/><br/>Przykład 2:`cluster1/logs/{date}`<br/><br/>`*`Znak nie jest dozwoloną wartością dla prefiksu ścieżki. Dozwolone są tylko poprawne <a HREF="https://msdn.microsoft.com/library/azure/dd135715.aspx">znaki obiektów blob platformy Azure</a> . Nie dołączaj nazw kontenerów ani nazw plików. |
| **Format daty** (opcjonalnie) | Jeśli używasz zmiennej daty w ścieżce, format daty, w którym są zorganizowane pliki. Przykład: `YYYY/MM/DD` <br/><br/> Gdy dane wejściowe obiektu BLOB mają `{date}` lub `{time}` w swojej ścieżce, foldery są przeszukiwane w kolejności rosnącej.|
| **Format czasu** (opcjonalnie) |  Jeśli używasz zmiennej czasowej w ścieżce, format czasu, w którym są zorganizowane pliki. Obecnie jedyną obsługiwaną wartością jest `HH` dla godzin. |
| **Klucz partycji** | Jeśli dane wejściowe są podzielone na partycje przez właściwość, można dodać nazwę tej właściwości. Klucze partycji są opcjonalne i są używane do poprawiania wydajności zapytania, jeśli zawiera klauzulę PARTITION BY lub GROUP BY dla tej właściwości. |
| **Format serializacji zdarzeń** | Format serializacji (JSON, CSV, Avro lub [inny (protobuf, XML, własny,...)](custom-deserializer.md)) przychodzącego strumienia danych.  Upewnij się, że format JSON jest wyrównany ze specyfikacją i nie zawiera wiodących wartości 0 dla liczb dziesiętnych. |
| **Encoding** | W przypadku woluminów CSV i JSON UTF-8 jest obecnie jedynym obsługiwanym formatem kodowania. |
| **Kompresja** | Typ kompresji używany do odczytywania przychodzącego strumienia danych, na przykład None (wartość domyślna), GZip lub Wklęśnięcie. |

Gdy dane pochodzą ze źródła magazynu obiektów blob, masz dostęp do następujących pól metadanych w kwerendzie Stream Analytics:

| Właściwość | Opis |
| --- | --- |
| **BlobName** |Nazwa wejściowego obiektu BLOB, z którego pochodzi zdarzenie. |
| **EventProcessedUtcTime** |Data i godzina przetworzenia zdarzenia przez Stream Analytics. |
| **BlobLastModifiedUtcTime** |Data i godzina ostatniej modyfikacji obiektu BLOB. |
| **PartitionId** |Identyfikator partycji na podstawie zera dla karty wejściowej. |

Na przykład przy użyciu tych pól można napisać zapytanie podobne do poniższego przykładu:

```sql
SELECT
    BlobName,
    EventProcessedUtcTime,
    BlobLastModifiedUtcTime
FROM Input
```

## <a name="next-steps"></a>Następne kroki
> [!div class="nextstepaction"]
> [Szybki start: tworzenie zadania usługi Stream Analytics przy użyciu witryny Azure Portal](stream-analytics-quick-create-portal.md)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.query.language.reference]: https://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: https://go.microsoft.com/fwlink/?LinkId=517301
