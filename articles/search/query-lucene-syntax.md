---
title: Składnia zapytań Lucene
titleSuffix: Azure Cognitive Search
description: Informacje dotyczące pełnej składni zapytań Lucene w usłudze Azure Wyszukiwanie poznawcze w przypadku symboli wieloznacznych, wyszukiwania rozmytego, wyrażenia regularnego i innych zaawansowanych konstrukcji zapytań.
manager: nitinme
author: brjohnstmsft
ms.author: brjohnst
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 06/23/2020
translation.priority.mt:
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pt-br
- ru-ru
- zh-cn
- zh-tw
ms.openlocfilehash: 3bf9dc0e69707eaed8c2a844f6ed3169e65a5342
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "85564087"
---
# <a name="lucene-query-syntax-in-azure-cognitive-search"></a>Składnia zapytań Lucene w usłudze Azure Wyszukiwanie poznawcze

Zapytania dotyczące usługi Azure Wyszukiwanie poznawcze można pisać w oparciu o rozbudowana składnia [analizatora zapytań Lucene](https://lucene.apache.org/core/6_6_1/queryparser/org/apache/lucene/queryparser/classic/package-summary.html) dla wyspecjalizowanych formularzy zapytań: symbol wieloznaczny, Wyszukiwanie rozmyte, wyszukiwanie w sąsiedztwie, wyrażenia regularne to kilka przykładów. Większość składni analizatora zapytań Lucene jest [zaimplementowana w usłudze azure wyszukiwanie poznawcze](search-lucene-query-architecture.md), z wyjątkiem *wyszukiwań zakresu* , które są zbudowane na platformie Azure wyszukiwanie poznawcze za pomocą `$filter` wyrażeń. 

> [!NOTE]
> Pełna składnia Lucene jest używana dla wyrażeń zapytania, które przechodzą w parametr **wyszukiwania** interfejsu API [dokumentów wyszukiwania](https://docs.microsoft.com/rest/api/searchservice/search-documents) , nie należy mylić ze [składnią OData](query-odata-filter-orderby-syntax.md) UŻYTĄ dla parametru [$Filter](search-filters.md) tego interfejsu API. Te różne składni mają własne reguły tworzenia zapytań, ciągów ucieczki i tak dalej.

## <a name="invoke-full-parsing"></a>Wywołaj pełną analizę

Ustaw `queryType` parametr Search, aby określić, który Analizator ma być używany. Prawidłowe wartości to `simple|full` , z wartościami `simple` domyślnymi i `full` dla Lucene. 

<a name="bkmk_example"></a> 

### <a name="example-showing-full-syntax"></a>Przykład przedstawiający pełną składnię

Poniższy przykład odnajduje dokumenty w indeksie przy użyciu składni zapytań Lucene, oczywisty dla `queryType=full` parametru. To zapytanie zwraca Hotele, w których pole Category zawiera termin "budżet" i wszystkie pola z możliwością wyszukiwania zawierające frazę "ostatnio Renovated". Dokumenty zawierające frazę "niedawno Renovated" są bardziej klasyfikowane jako wynik okresu zwiększenia wartości (3).  

`searchMode=all`Parametr jest istotny w tym przykładzie. Zawsze, gdy operatory znajdują się w zapytaniu, należy ogólnie ustawić, `searchMode=all` Aby upewnić się, że *wszystkie* kryteria są zgodne.

```
GET /indexes/hotels/docs?search=category:budget AND \"recently renovated\"^3&searchMode=all&api-version=2020-06-30&querytype=full
```

 Alternatywnie możesz użyć wpisu:  

```
POST /indexes/hotels/docs/search?api-version=2020-06-30
{
  "search": "category:budget AND \"recently renovated\"^3",
  "queryType": "full",
  "searchMode": "all"
}
```

Aby uzyskać więcej przykładów, zobacz [przykłady składni zapytań Lucene dotyczące kompilowania zapytań w usłudze Azure wyszukiwanie poznawcze](search-query-lucene-examples.md). Aby uzyskać szczegółowe informacje na temat określania pełnego uwarunkowania parametrów zapytania, zobacz [Wyszukiwanie dokumentów &#40;Azure wyszukiwanie poznawcze interfejs API REST&#41;](https://docs.microsoft.com/rest/api/searchservice/Search-Documents).

> [!NOTE]  
>  Usługa Azure Wyszukiwanie poznawcze obsługuje także [prostą składnię zapytań](query-simple-syntax.md), prosty i niezawodny język zapytań, który może służyć do prostego wyszukiwania słów kluczowych.  

##  <a name="syntax-fundamentals"></a><a name="bkmk_syntax"></a>Podstawy składni  

poniższe podstawowe informacje o składni dotyczą wszystkich zapytań, które używają składni Lucene.  

### <a name="operator-evaluation-in-context"></a>Obliczanie operatora w kontekście

Umieszczanie określa, czy symbol jest interpretowany jako operator, czy po prostu innego znaku w ciągu.

Na przykład w pełnej składni Lucene jest wyrażenie tyldy (~) używane do wyszukiwania rozmytego i wyszukiwania bliskości. Po umieszczeniu w cudzysłowie, ~ wywołuje wyszukiwanie zbliżeniowe. Po umieszczeniu na końcu okresu, ~ wywołuje rozmyte wyszukiwanie.

W ramach terminu, takiego jak "Business ~ analityk", znak nie jest obliczany jako operator. W takim przypadku zakładając, że zapytanie jest wyrażeniem terminu lub frazy, [wyszukiwanie pełnotekstowe](search-lucene-query-architecture.md) z [analizą leksykalną](search-lucene-query-architecture.md#stage-2-lexical-analysis) powoduje przełączenie ~ i przerywa okres "Business" analityka w dwóch: Business lub analityku.

Powyższym przykładem jest Tylda (~), ale ta sama zasada ma zastosowanie do każdego operatora.

### <a name="escaping-special-characters"></a>Znaki specjalne ucieczki

Aby użyć dowolnego operatora wyszukiwania jako części tekstu wyszukiwania, należy wprowadzić znak ucieczki, wpisując jego prefiks pojedynczym ukośnikiem odwrotnym ( `\` ). Na przykład dla wyszukiwania wieloznacznego `https://` , gdzie `://` jest częścią ciągu zapytania, należy określić `search=https\:\/\/*` . Podobnie wzorzec numeru telefonu może wyglądać następująco `\+1 \(800\) 642\-7676` .

Znaki specjalne, które wymagają ucieczki, są następujące:  
`+ - & | ! ( ) { } [ ] ^ " ~ * ? : \ /`  

> [!NOTE]  
> Mimo że ucieczki przechowuje tokeny, [Analiza leksykalna](search-lucene-query-architecture.md#stage-2-lexical-analysis) podczas indeksowania może je rozdzielić. Na przykład standardowy Analizator Lucene spowoduje przerwanie wyrazów w łącznikach, odstępach i innych znakach. Jeśli w ciągu zapytania są wymagane znaki specjalne, może być konieczne Analizator, który zachowuje je w indeksie. Niektóre z nich to analizatory [języka](index-add-language-analyzers.md)naturalnego firmy Microsoft, które zachowują wyrazy z wyrazami lub niestandardową, aby uzyskać bardziej złożone wzorce. Aby uzyskać więcej informacji, zobacz [częściowe terminy, wzorce i znaki specjalne](search-query-partial-matching.md).

### <a name="encoding-unsafe-and-reserved-characters-in-urls"></a>Kodowanie znaków niebezpiecznych i zastrzeżonych w adresach URL

Upewnij się, że wszystkie znaki niebezpieczne i zarezerwowane są zakodowane w adresie URL. Na przykład "#" jest niebezpiecznym znakiem, ponieważ jest identyfikatorem fragmentu/kotwicą w adresie URL. Znak musi być zakodowany `%23` w przypadku użycia w adresie URL. "&" i "=" są przykładami znaków zarezerwowanych, ponieważ oddzielają one parametry i określają wartości na platformie Azure Wyszukiwanie poznawcze. Aby uzyskać więcej informacji, zobacz [RFC1738: Uniform Resource Locators (URL)](https://www.ietf.org/rfc/rfc1738.txt) .

Niebezpieczne znaki to ``" ` < > # % { } | \ ^ ~ [ ]`` . Znaki zarezerwowane są `; / ? : @ = + &` .

###  <a name="query-size-limits"></a><a name="bkmk_querysizelimits"></a>Limity rozmiaru zapytań

 Istnieje ograniczenie rozmiaru zapytań, które można wysłać do usługi Azure Wyszukiwanie poznawcze. W szczególności można mieć co najwyżej 1024 klauzul (wyrażenia oddzielone znakami i, lub itd.). Obowiązuje również limit około 32 KB na rozmiar każdego pojedynczego okresu zapytania. Jeśli aplikacja generuje zapytania wyszukiwania programowo, zalecamy zaprojektowanie go w taki sposób, aby nie generował zapytań o nieograniczonego rozmiaru.  

### <a name="precedence-operators-grouping"></a>Operatory pierwszeństwa (grupowanie)

 Za pomocą nawiasów można tworzyć podzapytania, w tym operatory w instrukcji języka nawiasów. Program przeszuka na przykład `motel+(wifi||luxury)` dokumenty zawierające termin "Motel" i "Wi-Fi" lub "możliwość zaprojektowania" (lub oba te elementy).

Grupowanie pól jest podobne, ale zakresy grupowanie do jednego pola. Na przykład `hotelAmenities:(gym+(wifi||pool))` szuka pola "hotelAmenities" dla "treningów" i "Wi-Fi", "treningów" i "Pool".  

##  <a name="boolean-search"></a><a name="bkmk_boolean"></a>Wyszukiwanie wartości logicznych

 Zawsze określaj operatory wartości tekstowych (i, lub, nie) we wszystkich wersalikach.  

### <a name="or-operator-or-or-"></a>`OR`Operator OR lub`||`

Operator OR jest pionowym znakiem kreski lub potoku. Na przykład: `wifi || luxury` program przeszuka dokumenty zawierające "Wi-Fi" lub "możliwość zaprojektowania". Ponieważ lub jest domyślnym operatorem połączenia, można go również pozostawić, tak aby `wifi luxury` był odpowiednikiem `wifi || luxury` .

### <a name="and-operator-and--or-"></a>Operator AND `AND` `&&` lub`+`

Operator i jest znakiem handlowego "i". Na przykład: `wifi && luxury` program przeszuka dokumenty zawierające zarówno "Wi-Fi", jak i "możliwość zaprojektowania". Znak plus (+) jest używany dla wymaganych warunków. Na przykład program `+wifi +luxury` określa, że oba warunki muszą znajdować się gdzieś w polu jednego dokumentu.

### <a name="not-operator-not--or--"></a>NOT `NOT` — operator `!` lub`-`

Operator NOT jest znakiem minus. Program przeszuka na przykład `wifi –luxury` dokumenty, które mają `wifi` termin i/lub nie mają `luxury` .

Parametr **searchmode** w żądaniu zapytania kontroluje, czy termin z operatorem NOT jest ANDed lub logicznie innym warunkiem w zapytaniu (przy założeniu, że nie `+` ma `|` operatora OR w innych warunkach). Prawidłowe wartości to include `any` lub `all` .

`searchMode=any`zwiększa odwoływanie zapytań przez dołączenie większej liczby wyników i domyślnie `-` będzie interpretowane jako "lub" nie ". Na przykład program `wifi -luxury` będzie pasował do dokumentów, które zawierają termin `wifi` lub te, które nie zawierają warunków `luxury` .

`searchMode=all`zwiększa precyzję zapytań, dołączając mniejszą liczbę wyników i domyślnie — będzie interpretowana jako "i". Na przykład program `wifi -luxury` będzie pasował do dokumentów zawierających termin `wifi` i nie zawiera terminu "możliwość zaprojektowania". Jest to raczej bardziej intuicyjne zachowanie `-` operatora. W związku z tym należy rozważyć użycie zamiast tego, `searchMode=all` `searchMode=any` Jeśli chcesz zoptymalizować wyszukiwanie pod kątem precyzji zamiast odwołania, *a* użytkownicy często używają `-` operatora w wyszukiwaniach.

Podczas decydowania o ustawieniu **searchmode** należy wziąć pod uwagę wzorce interakcji użytkownika dotyczące zapytań w różnych aplikacjach. Użytkownicy poszukujący informacji mogą dołączać operator do zapytania, w przeciwieństwie do witryn handlu elektronicznego, które mają bardziej wbudowaną strukturę nawigacji.

##  <a name="fielded-search"></a><a name="bkmk_fields"></a>Wyszukiwanie polowe

Można zdefiniować operację wyszukiwania w polu z `fieldName:searchExpression` składnią, gdzie wyrażenie wyszukiwania może być pojedynczym słowem lub frazą lub bardziej skomplikowanym wyrażeniem w nawiasach, opcjonalnie z operatorami logicznymi. Oto kilka przykładów:  

- Gatunek: nie historia Jazz  

- artyści:("mile Davis" "Jan Coltrane")

Pamiętaj, aby umieścić wiele ciągów w cudzysłowie, jeśli chcesz, aby oba ciągi były oceniane jako pojedyncze jednostki, w tym przypadku wyszukiwanie dwóch odrębnych artystów w `artists` polu.  

Pole określone w elemencie `fieldName:searchExpression` musi być `searchable` polem.  Aby uzyskać szczegółowe informacje na temat sposobu używania atrybutów indeksu w definicjach pól, zobacz [create index](https://docs.microsoft.com/rest/api/searchservice/create-index) .  

> [!NOTE]
> W przypadku korzystania z wyrażeń wyszukiwania w polu nie trzeba używać `searchFields` parametru, ponieważ każde wyrażenie wyszukiwania w polu ma jawnie określoną nazwę pola. Jednak nadal można użyć `searchFields` parametru, jeśli chcesz uruchomić kwerendę, w której niektóre części są objęte zakresem określonego pola, a reszta może mieć zastosowanie do kilku pól. Na przykład zapytanie `search=genre:jazz NOT history&searchFields=description` byłoby zgodne tylko z `jazz` `genre` polem, podczas gdy byłoby zgodne `NOT history` z `description` polem. Nazwa pola podana w `fieldName:searchExpression` zawsze ma pierwszeństwo przed `searchFields` parametrem, co oznacza, że w tym przykładzie nie trzeba dołączać do `genre` `searchFields` parametru.

##  <a name="fuzzy-search"></a><a name="bkmk_fuzzy"></a>Wyszukiwanie rozmyte

Wyszukiwanie rozmyte umożliwia znalezienie dopasowań w warunkach, które mają podobną konstrukcję, i rozszerza termin do maksymalnie 50 warunków, które spełniają kryteria odległości co najmniej dwóch. Aby uzyskać więcej informacji, zobacz [Wyszukiwanie rozmyte](search-query-fuzzy.md).

 Aby wykonać Wyszukiwanie rozmyte, Użyj symbolu "~" na końcu pojedynczego słowa z opcjonalnym parametrem, liczbą z przedziału od 0 do 2 (wartość domyślna), która określa odległość edycji. Na przykład "Blue ~" lub "Blue ~ 1" zwróci "Blue", "Blues" i "Glue".

 Wyszukiwanie rozmyte może być stosowane tylko do warunków, a nie fraz, ale do każdego terminu można dołączać pojedyncze części nazwy lub frazy. Na przykład "Unviersty ~ of ~" Wshington ~ "byłoby zgodne z" University of Waszyngton ".
 
##  <a name="proximity-search"></a><a name="bkmk_proximity"></a>Wyszukiwanie w sąsiedztwie

Wyszukiwania w sąsiedztwie są używane do znajdowania terminów blisko siebie w dokumencie. Wstaw symbol tyldy "~" na końcu frazy, a po niej liczbę słów, które tworzą granicę bliskości. Na przykład `"hotel airport"~5` w dokumencie znajdą się terminy "Hotel" i "Lotnisko" w 5 wyrazach innych.  


##  <a name="term-boosting"></a><a name="bkmk_termboost"></a>Zwiększenie warunków

Zwiększenie warunków dotyczy klasyfikacji dokumentu, jeśli zawiera on podwyższony termin względem dokumentów, które nie zawierają warunków. Różni się to od profilów oceniania w tych profilach oceniania, a nie konkretnych warunków.  

Poniższy przykład pomaga zilustrować różnice. Załóżmy, że istnieje profil oceniania, który zwiększa zgodność w określonym polu, podyktuj *gatunek* w [przykładowym musicstoreindex](index-add-scoring-profiles.md#bkmk_ex). Zwiększenie okresu może służyć do dalszej promocji niektórych wyszukiwanych terminów wyższych niż inne. Na przykład program `rock^2 electronic` będzie ulepszał dokumenty, które zawierają terminy wyszukiwania w polu gatunek powyżej innych pól, które można wyszukiwać w indeksie. Ponadto dokumenty zawierające wyszukiwany termin *skały* są wyższe niż w przypadku innych wyszukiwanych warunków w postaci *elektronicznej* w wyniku okresu zwiększenia wartości (2).  

 Aby zwiększyć okres korzystania z karetki, "^", symbol z współczynnikem wzrostu (liczbą) na końcu wyszukiwanego okresu. Możesz również poprawić frazy. Im wyższy współczynnik zwiększania wydajności, tym bardziej istotny termin będzie odnosić się do innych wyszukiwanych terminów. Domyślnie współczynnik zwiększania wynosi 1. Chociaż współczynnik zwiększania wartości musi być dodatni, może być mniejszy niż 1 (na przykład 0,20).  

##  <a name="regular-expression-search"></a><a name="bkmk_regex"></a>Wyszukiwanie wyrażeń regularnych  
 Wyszukiwanie w wyrażeniu regularnym wyszukuje dopasowanie na podstawie wzorców, które są prawidłowe w ramach oprogramowania Apache Lucene, zgodnie z opisem w [klasie RegExp](https://lucene.apache.org/core/6_6_1/core/org/apache/lucene/util/automaton/RegExp.html). W Wyszukiwanie poznawcze na platformie Azure wyrażenie regularne jest ujęte między ukośnikami `/` .

 Na przykład, aby znaleźć dokumenty zawierające "Motel" lub "Hotel", określ `/[mh]otel/` . Wyszukiwania wyrażeń regularnych są dopasowywane do pojedynczych wyrazów.

Niektóre narzędzia i języki nakładają dodatkowe wymagania dotyczące znaków ucieczki. W przypadku formatu JSON ciągi zawierające ukośnik są wyprowadzane z ukośnikiem odwrotnym: "microsoft.com/azure/", `search=/.*microsoft.com\/azure\/.*/` gdzie `search=/.* <string-placeholder>.*/` konfiguruje wyrażenie regularne i `microsoft.com\/azure\/` jest ciągiem z odwróconym ukośnikiem.

##  <a name="wildcard-search"></a><a name="bkmk_wildcard"></a>Wyszukiwanie symboli wieloznacznych

Można użyć ogólnie rozpoznanej składni dla wielu `*` symboli wieloznacznych () lub pojedynczych ( `?` ). Na przykład wyrażenie zapytania `search=alpha*` zwraca wartość "alfanumeryczne" lub "alfabetyczne". Zwróć uwagę, że Analizator zapytań Lucene obsługuje używanie tych symboli z pojedynczym terminem, a nie frazą.

Pełna składnia Lucene obsługuje Dopasowywanie prefiksów, wrostkowe i sufiksów. Jednak jeśli wszystko, co jest potrzebne, jest dopasowanie prefiksów, można użyć prostej składni (dopasowanie prefiksu jest obsługiwane w obu).

Dopasowanie sufiksu, Where `*` lub `?` poprzedzające ciąg (as in `search=/.*numeric./` ) lub wrostkowe, wymaga pełnej składni Lucene, a także ogranicznika ukośnika w wyrażeniach regularnych `/` . Nie można użyć znaku * ani? Symbol jako pierwszy znak okresu lub w okresie, bez `/` . 

> [!NOTE]  
> Zgodnie z regułą dopasowanie wzorców jest powolne, dlatego warto poznać alternatywne metody, takie jak Edge n-gram tokenizacji, które tworzy tokeny dla sekwencji znaków w danym okresie. Indeks będzie większy, ale zapytania mogą działać szybciej, w zależności od konstrukcji wzorca i długości ciągów, które są indeksowane.
>
> Podczas analizowania zapytania zapytania, które są formułowane jako prefiks, sufiks, symbole wieloznaczne lub wyrażenia regularne są przesyłane jako-do drzewa zapytań, pomijając [analizę leksykalną](search-lucene-query-architecture.md#stage-2-lexical-analysis). Dopasowania będą znajdować się tylko wtedy, gdy indeks zawiera ciągi w formacie używanym przez zapytanie. W większości przypadków będzie potrzebny alternatywny Analizator podczas indeksowania, które zachowuje integralność ciągów, tak aby częściowe dopasowanie terminu i wzorca powiodło się. Aby uzyskać więcej informacji, zobacz [częściowe wyszukiwanie warunków na platformie Azure wyszukiwanie poznawcze zapytań](search-query-partial-matching.md).

##  <a name="scoring-wildcard-and-regex-queries"></a><a name="bkmk_searchscoreforwildcardandregexqueries"></a>Ocenianie symboli wieloznacznych i wyrażeń regularnych

Usługa Azure Wyszukiwanie poznawcze używa oceniania opartego na częstotliwościach ([TF-IDF](https://en.wikipedia.org/wiki/Tf%E2%80%93idf)) na potrzeby zapytań tekstowych. Jednakże w przypadku zapytań wieloznacznych i wyrażeń regularnych, w których zakres terminów może być bardzo szeroki, współczynnik częstotliwości jest ignorowany, aby zapobiec rozliczeniu na dopasowania od rzadkich warunków. Wszystkie dopasowania są traktowane równo w przypadku wyszukiwania symboli wieloznacznych i wyrażeń regularnych.

## <a name="see-also"></a>Zobacz także

+ [Przykłady zapytań dla prostego wyszukiwania](search-query-simple-examples.md)
+ [Przykłady zapytań dla pełnego wyszukiwania Lucene](search-query-lucene-examples.md)
+ [Wyszukaj dokumenty](https://docs.microsoft.com/rest/api/searchservice/Search-Documents)
+ [Składnia wyrażenia OData dla filtrów i sortowania](query-odata-filter-orderby-syntax.md)   
+ [Prosta Składnia zapytania w usłudze Azure Wyszukiwanie poznawcze](query-simple-syntax.md)   
