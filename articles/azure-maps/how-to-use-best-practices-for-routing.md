---
title: Najlepsze rozwiązania dotyczące Azure Maps Route Service | Mapy Microsoft Azure
description: Dowiedz się, jak kierować pojazdy przy użyciu Route Service z Microsoft Azure Maps.
author: anastasia-ms
ms.author: v-stharr
ms.date: 03/11/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: 79e9096030aada9fa368bb2e78af323139c0586c
ms.sourcegitcommit: 0e8a4671aa3f5a9a54231fea48bcfb432a1e528c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/24/2020
ms.locfileid: "87132215"
---
# <a name="best-practices-for-azure-maps-route-service"></a>Najlepsze rozwiązania dotyczące usługi Azure Maps Route Service

Wskazówki dotyczące trasy i interfejsy API macierzy trasy w Azure Maps [Route Service](https://docs.microsoft.com/rest/api/maps/route) mogą służyć do obliczania szacowanego czasu przybycia (ETAs) dla każdej żądanej trasy. Interfejsy API tras uwzględniają takie czynniki jak informacje o ruchu w czasie rzeczywistym i historyczne dane o ruchu, takie jak typowe szybkości podróży w dniu tygodnia i o porze dnia. Interfejsy API zwracają najkrótsze lub najszybsze trasy dostępne dla wielu miejsc docelowych jednocześnie w sekwencji lub w kolejności zoptymalizowanej na podstawie czasu lub odległości. Użytkownicy mogą również zażądać wyspecjalizowanych tras i szczegółów dla podejść, rowerzystów i pojazdów komercyjnych, takich jak samochody. W tym artykule udostępnimy najlepsze rozwiązania w zakresie wywoływania Azure Maps [Route Service](https://docs.microsoft.com/rest/api/maps/route)i dowiesz się, jak:

* Wybieranie między interfejsami API wskazówek dotyczących tras i interfejsem API routingu macierzy
* Żądaj historycznych i przewidywanych czasów podróży na podstawie danych o ruchu w czasie rzeczywistym i historycznych
* Żądaj szczegółowych informacji o trasie, takich jak czas i odległość, dla całej trasy i każdego etapu trasy
* Zażądaj trasy dla pojazdu komercyjnego, takiego jak ciężarówka
* Żądaj informacji o ruchu na trasie, takich jak dżemy i informacje o cle
* Zażądaj trasy, która składa się z co najmniej jednego zatrzymania (waypoints)
* Optymalizacja trasy jednego lub większej liczby przerw w celu uzyskania najlepszej kolejności do odwiedzania każdego zatrzymania (punkt nawigacyjny)
* Optymalizuj alternatywne trasy przy użyciu punktów pomocniczych. Można na przykład oferować alternatywne trasy, które przechodzą przez stację ładowania pojazdu elektrycznego.
* Używanie [Route Service](https://docs.microsoft.com/rest/api/maps/route) z zestawem SDK Azure Maps sieci Web

## <a name="prerequisites"></a>Wymagania wstępne

Aby wykonać wywołania do Azure Maps interfejsów API, musisz mieć konto Azure Maps i klucz. Aby uzyskać więcej informacji, zobacz [Tworzenie konta](quick-demo-map-app.md#create-an-azure-maps-account) i [Uzyskiwanie klucza podstawowego](quick-demo-map-app.md#get-the-primary-key-for-your-account). Klucz podstawowy jest również znany jako podstawowy klucz subskrypcji lub klucz subskrypcji.

Aby uzyskać informacje o uwierzytelnianiu w Azure Maps, zobacz [Zarządzanie uwierzytelnianiem w programie Azure Maps](./how-to-manage-authentication.md). Aby uzyskać więcej informacji na temat pokrycia Route Service, zobacz [pokrycie routingu](routing-coverage.md).

W tym artykule jest wykorzystywana [aplikacja Poster](https://www.postman.com/downloads/) do kompilowania wywołań REST, ale można wybrać dowolne środowisko deweloperskie interfejsu API.

## <a name="choose-between-route-directions-and-matrix-routing"></a>Wybór między kierunkami tras i marszrutą macierzy

Interfejsy API wskazówki dotyczące trasy zwracają instrukcje, w tym czas podróży i współrzędne ścieżki trasy. Interfejs API macierzy tras pozwala obliczyć czas podróży i odległość dla zestawu tras zdefiniowanych przez lokalizację źródłową i docelową. Dla każdego danego źródła interfejs API macierzy oblicza koszt (czas podróży i odległość) routingu z tego źródła do każdego miejsca docelowego. Wszystkie te interfejsy API umożliwiają określenie parametrów, takich jak żądany czas wyruszenia, czasy przybycia i typ pojazdu, np. samochodu lub ciężarówka. Wszyscy wykorzystują dane dotyczące ruchu w czasie rzeczywistym lub predykcyjnym odpowiednio do zwrócenia najbardziej optymalnych tras.

Rozważ wywołanie interfejsów API wskazówek dotyczących trasy, Jeśli Twój Scenariusz ma:

* Zażądaj najkrótszej lub najszybszej trasy między dwoma lub większą liczbą znanych lokalizacji, aby uzyskać precyzyjne czasy przybycia dla pojazdów dostawczych.
* Żądaj szczegółowych wskazówek dotyczących trasy, w tym geometrii tras, do wizualizacji tras na mapie
* Mając listę lokalizacji klientów, Oblicz najkrótszą możliwą trasę, aby odwiedzać poszczególne lokalizacje klientów i powrócić do źródła. Ten scenariusz jest często znany jako problem z podróżą Salesman. Można przekazać do 150 waypoints (zatrzymanie) w jednym żądaniu.
* Wysyłanie partii zapytań do interfejsu API usługi Batch wskazówek dotyczących trasy przy użyciu tylko jednego wywołania interfejsu API.

Rozważ wywołanie interfejsu API routingu macierzy, Jeśli Twój Scenariusz ma:

* Oblicz czas podróży lub odległość między zestawem źródeł i miejscami docelowymi. Na przykład masz 12 sterowników i musisz znaleźć najbliższy dostępny sterownik, aby pobrać dostawę żywności z restauracji.
* Sortuj potencjalne trasy według ich rzeczywistej odległości lub czasu. Interfejs API macierzy zwraca tylko czasy podróży i odległości dla każdej kombinacji źródłowej i docelowej.
* Dane klastra na podstawie czasu podróży lub odległości. Na przykład firma ma 50 pracowników, Znajdź wszystkich pracowników na żywo w ciągu 20 minut od momentu jego uruchomienia.

Poniżej znajduje się porównanie przedstawiające pewne możliwości dotyczące instrukcji trasy i interfejsów API macierzy:

| Interfejs API Azure Maps | Maksymalna liczba zapytań w żądaniu | Unikaj obszarów | Routing samochodu i elektrycznego | Optymalizacja waypoints i podróży Salesman | Punkty pomocnicze |
| :--------------: |  :--------------: |  :--------------: | :--------------: | :--------------: | :--------------: |
| Pobierz wskazówki dotyczące trasy | 1 | | X | X | |
| Wskazówki dotyczące trasy po trasie | 1 | X | X | X | X |
| Partia instrukcji po trasie | 700 | | X | X | |
| Macierz po trasie | 700 | | X | | |

Aby dowiedzieć się więcej o możliwościach routingu pojazdów elektrycznych, zobacz nasz samouczek dotyczący [kierowania pojazdów elektrycznych przy użyciu Azure Notebooks za pomocą języka Python](tutorial-ev-routing.md).

## <a name="request-historic-and-real-time-data"></a>Żądaj danych historycznych i w czasie rzeczywistym

Domyślnie usługa trasy zakłada, że tryb podróży jest samochodem i czas wyruszenia jest teraz. Zwraca trasę na podstawie warunków ruchu w czasie rzeczywistym, chyba że żądanie obliczenia trasy jest określone w przeciwnym razie. Stałe ograniczenia ruchu zależnego od czasu, takie jak "lewe przelewy" nie są dozwolone między 4:00 PM do 6:00 PM ", są przechwytywane i uwzględniane przez aparat routingu. Zamknięcia dróg, takie jak roadworks, będą brane pod uwagę, chyba że zażądasz trasy, która ignoruje bieżący ruch na żywo. Aby zignorować bieżący ruch, ustaw `traffic` na wartość `false` w żądaniu interfejsu API.

Wartość **travelTimeInSeconds** obliczeń trasy obejmuje opóźnienie spowodowane ruchem. Jest on generowany przy użyciu bieżących i historycznych danych czasu podróży, gdy czas wyruszenia jest ustawiony na teraz. Jeśli czas wyruszenia zostanie ustawiony w przyszłości, interfejsy API zwracają przewidywany czas podróży na podstawie danych historycznych.

Jeśli w żądaniu zostanie uwzględniony parametr **computeTravelTimeFor = All** , wówczas element Summary w odpowiedzi będzie zawierał następujące dodatkowe pola, w tym historyczne warunki ruchu:

| Element | Opis|
| :--- | :--- |
| noTrafficTravelTimeInSeconds | Szacowany czas podróży został obliczony tak, jakby nie ma opóźnień dla trasy ze względu na to, że z powodu przeciążenia |
| historicTrafficTravelTimeInSeconds | Szacowany czas podróży obliczany przy użyciu danych o ruchu historycznym zależnym od czasu |
| liveTrafficIncidentsTravelTimeInSeconds | Szacowany czas podróży obliczany przy użyciu danych szybkości w czasie rzeczywistym |

W następnych sekcjach pokazano, jak wykonywać wywołania interfejsów API tras przy użyciu omawianych parametrów.

### <a name="sample-query"></a>Przykładowe zapytanie

Pierwszy przykład poniżej czasu wyjazdu jest ustawiony na przyszłość w czasie pisania.

```http
https://atlas.microsoft.com/route/directions/json?subscription-key=<Your-Azure-Maps-Primary-Subscription-Key>&api-version=1.0&query=51.368752,-0.118332:51.385426,-0.128929&travelMode=car&traffic=true&departAt=2025-03-29T08:00:20&computeTravelTimeFor=all
```

Odpowiedź zawiera element podsumowania, podobny do przedstawionego poniżej. Ponieważ czas wyruszenia jest ustawiony na przyszłość, wartość **trafficDelayInSeconds** jest równa zero. Wartość **travelTimeInSeconds** jest obliczana przy użyciu danych o ruchu historycznym zależnym od czasu. Dlatego w tym przypadku wartość **travelTimeInSeconds** jest równa wartości **historicTrafficTravelTimeInSeconds** .

```json
"summary": {
    "lengthInMeters": 2131,
    "travelTimeInSeconds": 248,
    "trafficDelayInSeconds": 0,
    "departureTime": "2025-03-29T08:00:20Z",
    "arrivalTime": "2025-03-29T08:04:28Z",
    "noTrafficTravelTimeInSeconds": 225,
    "historicTrafficTravelTimeInSeconds": 248,
    "liveTrafficIncidentsTravelTimeInSeconds": 248
},
```

### <a name="sample-query"></a>Przykładowe zapytanie

W drugim przykładzie występuje żądanie routingu w czasie rzeczywistym, w którym jest teraz czas wyjazdu. Nie jest on jawnie określony w adresie URL, ponieważ jest wartością domyślną.

```http
https://atlas.microsoft.com/route/directions/json?subscription-key=<Your-Azure-Maps-Primary-Subscription-Key>&api-version=1.0&query=47.6422356,-122.1389797:47.6641142,-122.3011268&travelMode=car&traffic=true&computeTravelTimeFor=all
```

Odpowiedź zawiera podsumowanie, jak pokazano poniżej. Ze względu na przeciążenia wartość **trafficDelaysInSeconds** jest większa od zera. Jest ona również większa niż **historicTrafficTravelTimeInSeconds**.

```json
"summary": {
    "lengthInMeters": 16637, 
    "travelTimeInSeconds": 2905, 
    "trafficDelayInSeconds": 1604, 
    "departureTime": "2020-02-28T01:00:20+00:00",
    "arrivalTime": "2020-02-28T01:48:45+00:00", 
    "noTrafficTravelTimeInSeconds": 872, 
    "historicTrafficTravelTimeInSeconds": 1976, 
    "liveTrafficIncidentsTravelTimeInSeconds": 2905 
},
```

## <a name="request-route-and-leg-details"></a>Żądaj szczegółów trasy i etapu

Domyślnie usługa Route zwróci tablicę współrzędnych. Odpowiedź będzie zawierać współrzędne, które tworzą ścieżkę na liście o nazwie `points` . Odpowiedź trasy obejmuje również odległość od początku trasy i szacowany czas, który upłynął. Te wartości mogą służyć do obliczania średniej szybkości dla całej trasy.

Na poniższej ilustracji przedstawiono `points` element.

<center>

![Lista punktów](media/how-to-use-best-practices-for-routing/points-list-is-hidden-img.png)

</center>

Rozwiń `point` element, aby wyświetlić listę współrzędnych ścieżki:

<center>

![Lista punktów](media/how-to-use-best-practices-for-routing/points-list-img.png)

</center>

Interfejsy API wskazówek dotyczących trasy obsługują różne formaty instrukcji, które mogą być używane przez określenie parametru **instructiontype** . Aby sformatować instrukcje dotyczące łatwego przetwarzania komputera, użyj **instrukcji instructiontype = kodowane**. Użyj **instrukcji instructiontype = Tagged** , aby wyświetlić instrukcje jako tekst dla użytkownika. Ponadto instrukcje można sformatować jako tekst, w którym są oznaczone niektóre elementy instrukcji, a instrukcja jest prezentowana z formatowaniem specjalnym. Aby uzyskać więcej informacji, zobacz [listę obsługiwanych typów instrukcji](https://docs.microsoft.com/rest/api/maps/route/postroutedirections#routeinstructionstype).

Gdy wymagane są instrukcje, odpowiedź zwraca nowy element o nazwie `guidance` . `guidance`Element zawiera dwie części informacji: instrukcje włączania i podsumowywania instrukcji.

<center>

![Typ instrukcji](media/how-to-use-best-practices-for-routing/instructions-type-img.png)

</center>

`instructions`Element zawiera wskazówki dotyczące włączania i wyłączania dla podróży oraz `instructionGroups` zawiera podsumowanie instrukcji. Każde podsumowanie instrukcji obejmuje segment rejsu, który może obejmować wiele dróg. Interfejsy API mogą zwrócić szczegóły dotyczące sekcji trasy. taki jak, zakres współrzędnych zakleszczenia ruchu lub bieżącą szybkość ruchu.

<center>

![Włącz, włączając instrukcje](media/how-to-use-best-practices-for-routing/instructions-turn-by-turn-img.png)

</center>

<center>

![Instrukcje podsumowania](media/how-to-use-best-practices-for-routing/instructions-summary-img.png)

</center>

## <a name="request-a-route-for-a-commercial-vehicle"></a>Żądaj trasy dla pojazdu komercyjnego

Interfejsy API routingu Azure Maps obsługują komercyjne Routing pojazdu, obejmujący Routing ciężarów komercyjnych. Interfejsy API uwzględniają określone limity. Takie jak, Wysokość i waga pojazdu oraz, jeśli pojazd jest przewożący niebezpieczne towary. Jeśli na przykład pojazd ma łatwopalne, aparat routingu unika pewnych tuneli, które znajdują się blisko obszarów mieszkalnych.

### <a name="sample-query"></a>Przykładowe zapytanie

Przykładowe żądanie poniżej wysyła zapytanie do trasy dla ciężarówki komercyjnej. Ciężarówka jest przewożąca odpady niebezpieczne klasy 1.

```http
https://atlas.microsoft.com/route/directions/json?subscription-key=<Your-Azure-Maps-Primary-Subscription-Key>&api-version=1.0&vehicleWidth=2&vehicleHeight=2&vehicleCommercial=true&vehicleLoadType=USHazmatClass1&travelMode=truck&instructionsType=text&query=51.368752,-0.118332:41.385426,-0.128929
```

Interfejs API tras zwraca wskazówki, które uwzględniają Wymiary wózka i odpadów niebezpiecznych. Instrukcje dotyczące trasy można odczytać, rozszerzając `guidance` element.

<center>

![Ciężarówka z klasą 1 hazwaste](media/how-to-use-best-practices-for-routing/truck-with-hazwaste-img.png)

</center>

### <a name="sample-query"></a>Przykładowe zapytanie

Zmiana klasy Hazmat US (Stany Zjednoczone) z powyższego zapytania spowoduje przekroczenie tej zmiany w innej trasie.

```http
https://atlas.microsoft.com/route/directions/json?subscription-key=<Your-Azure-Maps-Primary-Subscription-Key>&api-version=1.0&vehicleWidth=2&vehicleHeight=2&vehicleCommercial=true&vehicleLoadType=USHazmatClass9&travelMode=truck&instructionsType=text&query=51.368752,-0.118332:41.385426,-0.128929
```

Poniższa odpowiedź dotyczy ciężarówki przewożącej materiał niebezpieczny klasy 9, który jest mniej bezpieczny niż materiał niebezpieczny klasy 1. Po rozwinięciu `guidance` elementu, aby przeczytać wskazówki, Zauważ, że te wskazówki nie są takie same. Istnieje więcej instrukcji dotyczących trasy dla samochodów przewożących materiał niebezpieczny klasy 1.

<center>

![Ciężarówka z klasą 9 hazwaste](media/how-to-use-best-practices-for-routing/truck-with-hazwaste9-img.png)

</center>

## <a name="request-traffic-information-along-a-route"></a>Żądaj informacji o ruchu na trasie

Dzięki interfejsom API kierunku trasy Azure Maps deweloperzy mogą żądać szczegółowych informacji dla każdego typu sekcji przez uwzględnienie `sectionType` parametru w żądaniu. Na przykład można zażądać informacji o szybkości dla każdego segmentu zakleszczenia ruchu. Zapoznaj się z [listą wartości dla klucza sectiontype](https://docs.microsoft.com/rest/api/maps/route/getroutedirections#sectiontype) , aby dowiedzieć się więcej o różnych szczegółach, które można zażądać.

### <a name="sample-query"></a>Przykładowe zapytanie

Poniższe zapytanie ustawia wartość `sectionType` na `traffic` . Żąda sekcji zawierających informacje o ruchu z Seattle do San Diego.

```http
https://atlas.microsoft.com/route/directions/json?subscription-key=<Your-Azure-Maps-Primary-Subscription-Key>&api-version=1.0&sectionType=traffic&query=47.6062,-122.3321:32.7157,-117.1611
```

Odpowiedź zawiera sekcje, które są odpowiednie dla ruchu, wzdłuż danego współrzędnych.

<center>

![sekcje ruchu](media/how-to-use-best-practices-for-routing/traffic-section-type-img.png)

</center>

Ta opcja umożliwia kolorowanie sekcji podczas renderowania mapy, jak na poniższym obrazie: 

<center>

![sekcje ruchu](media/how-to-use-best-practices-for-routing/show-traffic-sections-img.png)

</center>

## <a name="calculate-and-optimize-a-multi-stop-route"></a>Obliczanie i optymalizowanie trasy z obsługą multitransportu

Azure Maps obecnie oferuje dwie formy optymalizacji tras:

* Optymalizacje oparte na żądanym typie trasy, bez zmiany kolejności waypoints. [Obsługiwane typy tras](https://docs.microsoft.com/rest/api/maps/route/postroutedirections#routetype) można znaleźć tutaj

* Salesman Optymalizacja, która zmienia kolejność waypoints w celu uzyskania najlepszej kolejności do odwiedzania każdego zatrzymania

W przypadku routingu z obsługą wiele tras do 150 waypoints można określić w jednym żądaniu trasy. Lokalizacje współrzędnych początkowych i końcowych mogą być takie same, jak w przypadku rundy. Należy jednak podać co najmniej jedną dodatkową punkt nawigacyjnyą, aby umożliwić Obliczanie trasy. Waypoints można dodać do zapytania między współrzędne źródłowe i docelowe.

Aby zoptymalizować najlepszą kolejność do odwiedzania danego waypoints, należy określić **computeBestOrder = true**. Ten scenariusz jest również znany jako problem z optymalizacją Salesman.

### <a name="sample-query"></a>Przykładowe zapytanie

Następujące zapytanie żąda ścieżki dla sześciu waypoints z `computeBestOrder` parametrem ustawionym na `false` . Jest to również wartość domyślna `computeBestOrder` parametru.

```http
https://atlas.microsoft.com/route/directions/json?api-version=1.0&subscription-key=<Your-Azure-Maps-Primary-Subscription-Key>&computeBestOrder=false&query=47.606544,-122.336502:47.759892,-122.204821:47.670682,-122.120415:47.480133,-122.213369:47.615556,-122.193689:47.676508,-122.206054:47.495472,-122.360861
```

Odpowiedź opisuje długość ścieżki do 140 851 gazomierzy i że potrwa 9 991 sekund do podróży tej ścieżki.

<center>

![Niezoptymalizowana odpowiedź](media/how-to-use-best-practices-for-routing/non-optimized-response-img.png)

</center>

Poniższy obraz ilustruje ścieżkę podaną w wyniku tego zapytania. Ta ścieżka jest jedną z możliwych tras. Nie jest to optymalna ścieżka oparta na czasie lub odległości.

<center>

![Obraz niezoptymalizowany](media/how-to-use-best-practices-for-routing/non-optimized-image-img.png)

</center>

Ta punkt nawigacyjny ma następującą kolejność: 0, 1, 2, 3, 4, 5 i 6.

### <a name="sample-query"></a>Przykładowe zapytanie

Następujące zapytanie żąda ścieżki dla tego samego sześć waypoints, jak w powyższym przykładzie. Tym razem `computeBestOrder` parametr ustawiony na `true` (podróż Salesman).

```http
https://atlas.microsoft.com/route/directions/json?api-version=1.0&subscription-key=<Your-Azure-Maps-Primary-Subscription-Key>&computeBestOrder=true&query=47.606544,-122.336502:47.759892,-122.204821:47.670682,-122.120415:47.480133,-122.213369:47.615556,-122.193689:47.676508,-122.206054:47.495472,-122.360861
```

Odpowiedź opisuje długość ścieżki do 91 814 gazomierzy i że potrwa 7 797 sekund do podróży tej ścieżki. Odległość podróży i czas podróży są mniejsze w tym miejscu, ponieważ interfejs API zwrócił zoptymalizowaną trasę.

<center>

![Niezoptymalizowana odpowiedź](media/how-to-use-best-practices-for-routing/optimized-response-img.png)

</center>

Poniższy obraz ilustruje ścieżkę podaną w wyniku tego zapytania.

<center>

![Obraz niezoptymalizowany](media/how-to-use-best-practices-for-routing/optimized-image-img.png)

</center>

Optymalna trasa ma następującą kolejność punkt nawigacyjny: 0, 5, 1, 2, 4, 3 i 6.

>[!TIP]
> Zoptymalizowane informacje o kolejności punkt nawigacyjny z usługi routingu zapewniają zestaw indeksów. Obejmują one źródło i indeksy docelowe. Te wartości należy zwiększyć o 1 w celu uwzględnienia pochodzenia. Następnie Dodaj lokalizację docelową na końcu, aby uzyskać pełną uporządkowaną listę punkt nawigacyjny.

## <a name="calculate-and-bias-alternative-routes-using-supporting-points"></a>Obliczanie i rozliczanie alternatywnych tras przy użyciu punktów pomocniczych

Mogą wystąpić sytuacje, w których chcesz odtworzyć trasę, aby obliczyć zero lub więcej tras alternatywnych dla trasy referencyjnej. Na przykład możesz chcieć pokazać klientom alternatywne trasy, które przechodzą w sklepie detalicznym. W takim przypadku należy rozrównać lokalizację przy użyciu punktów pomocniczych. Poniżej przedstawiono kroki, które należy wykonać w celu rozdzielenia lokalizacji:

1. Obliczanie trasy jako-is i pobieranie ścieżki z odpowiedzi trasy
2. Użyj ścieżki trasy, aby znaleźć odpowiednie lokalizacje lub blisko ścieżki trasy. Można na przykład użyć [interfejsu API punktu](https://docs.microsoft.com/rest/api/maps/search/getsearchpoi) Azure Maps lub zapytania do własnych danych w bazie danych.  
3. Kolejność lokalizacji na podstawie odległości od początku trasy
4. Dodaj te lokalizacje jako punkty pomocnicze w nowym żądaniu trasy do [interfejsu API wskazówek dotyczących trasy](https://docs.microsoft.com/rest/api/maps/route/postroutedirections). Aby dowiedzieć się więcej o punktach pomocniczych, zapoznaj się z [dokumentacją interfejsu API dotyczącej instrukcji post Route](https://docs.microsoft.com/rest/api/maps/route/postroutedirections#supportingpoints). 

Podczas wywoływania [interfejsu API wskazówek dotyczących trasy](https://docs.microsoft.com/rest/api/maps/route/postroutedirections), można ustawić minimalny czas odchylenia lub ograniczenia odległości wraz z punktami pomocniczymi. Użyj tych parametrów, jeśli chcesz oferować alternatywne trasy, ale chcesz również ograniczyć czas podróży. Gdy te ograniczenia są używane, trasy alternatywne będą podążać za trasą odniesienia od punktu początkowego dla danego czasu lub odległości. Innymi słowy, pozostałe trasy są rozbieżne względem trasy odwołania według danego ograniczenia.

Poniższy obraz przedstawia przykład renderowania alternatywnych tras z określonymi limitami odchyleń dla czasu i odległości.

<center>

![Alternatywne trasy](media/how-to-use-best-practices-for-routing/alternative-routes-img.png)

</center>

## <a name="use-the-routing-service-in-a-web-app"></a>Korzystanie z usługi routingu w aplikacji sieci Web

Zestaw SDK sieci Web Azure Maps udostępnia [moduł usługi](https://docs.microsoft.com/javascript/api/azure-maps-rest/?view=azure-maps-typescript-latest). Ten moduł jest biblioteką pomocniczą, która ułatwia korzystanie z Azure Maps interfejsów API REST w aplikacjach sieci Web lub Node.js za pomocą języka JavaScript lub TypeScript. Moduł usługi może służyć do renderowania zwracanych tras na mapie. Moduł automatycznie określa, który interfejs API ma być używany z żądaniami GET i POST.

## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się więcej, zobacz:

> [!div class="nextstepaction"]
> [Usługa tras Azure Maps](https://docs.microsoft.com/rest/api/maps/route)

> [!div class="nextstepaction"]
> [Jak używać modułu usługi](https://docs.microsoft.com/azure/azure-maps/how-to-use-services-module)

> [!div class="nextstepaction"]
> [Pokaż trasę na mapie](https://docs.microsoft.com/azure/azure-maps/map-route)

> [!div class="nextstepaction"]
> [Azure Maps pakiet NPM](https://www.npmjs.com/package/azure-maps-rest  )
