---
title: Używanie AI do zrozumienia danych magazynu obiektów BLOB
titleSuffix: Azure Cognitive Search
description: Dodaj semantykę, przetwarzanie języka naturalnego i analizę obrazu do obiektów blob platformy Azure przy użyciu potoku wzbogacania AI na platformie Azure Wyszukiwanie poznawcze.
manager: nitinme
author: HeidiSteen
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 591437eb3951164d53388b6164103948e9ad65e0
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "73496431"
---
# <a name="use-ai-to-understand-blob-storage-data"></a>Używanie AI do zrozumienia danych magazynu obiektów BLOB

Dane w usłudze Azure Blob Storage to często wiele zawartości bez struktury, takich jak obrazy, długie teksty, pliki PDF i dokumenty pakietu Office. Korzystając z funkcji AI w usłudze Azure Wyszukiwanie poznawcze, można zrozumieć i wyodrębnić cenne informacje z obiektów BLOB na wiele sposobów. Przykłady zastosowania AI do zawartości obiektów BLOB obejmują:

+ Wyodrębnij tekst z obrazów przy użyciu optycznego rozpoznawania znaków (OCR)
+ Tworzenie opisu sceny lub tagów ze zdjęcia
+ Wykrywaj język i Tłumacz tekst na różne języki
+ Przetwarzaj tekst za pomocą nazwanego rozpoznawania jednostek (NER), aby znaleźć odwołania do osób, dat, miejsc lub organizacji 

Chociaż może być wymagana tylko jedna z tych funkcji AI, często można połączyć wiele z nich w ten sam potok (na przykład Wyodrębnianie tekstu z zeskanowanego obrazu, a następnie znalezienie wszystkich dat i miejsc, do których się odwołuje). 

Wzbogacanie AI tworzy nowe informacje przechwycone jako tekst, przechowywane w polach. Po wzbogaceniu można uzyskać dostęp do tych informacji z indeksu wyszukiwania za pośrednictwem wyszukiwania pełnotekstowego lub wysłać wzbogacone dokumenty z powrotem do usługi Azure Storage, aby uzyskać nowe środowiska aplikacji, które obejmują Eksplorowanie danych w scenariuszach odnajdywania lub analizy. 

W tym artykule przedstawiono wzbogacenie AI za pośrednictwem szerokiego obiektywu, dzięki czemu można szybko opanujesz cały proces od przekształcenia nieprzetworzonych danych w obiektach Blob do Queryable informacji w indeksie wyszukiwania lub w sklepie z bazami wiedzy.

## <a name="what-it-means-to-enrich-blob-data-with-ai"></a>Co oznacza "wzbogacanie" danych obiektów blob z AI

*Wzbogacanie AI* jest częścią architektury indeksowania platformy Azure wyszukiwanie poznawcze, która integruje wbudowane AI z firmy Microsoft lub niestandardowego AI, który zapewniasz. Ułatwia ona implementowanie kompleksowych scenariuszy, w których należy przetwarzać obiekty blob (zarówno istniejące, jak i nowe), ponieważ są one dostępne lub są aktualizowane), pęknięcie otwiera wszystkie formaty plików w celu wyodrębnienia obrazów i tekstu, wyodrębnienie wymaganych informacji przy użyciu różnych funkcji AI i indeksowanie ich w indeksie wyszukiwania w celu szybkiego wyszukiwania, pobierania i eksploracji. 

Dane wejściowe są obiektami BLOB w jednym kontenerze w usłudze Azure Blob Storage. Obiekty blob mogą być niemal dowolnym rodzajem danych tekstowych lub graficznych. 

Wyjście jest zawsze indeksem wyszukiwania używanym do szybkiego wyszukiwania tekstu, pobierania i eksploracji w aplikacjach klienckich. Ponadto dane wyjściowe mogą być również *magazynem wiedzy* , który projektuje wzbogacone dokumenty do obiektów blob platformy Azure lub tabel platformy Azure w celu przeprowadzenia analizy podrzędnej w przypadku narzędzi takich jak Power BI lub obciążeń związanych z nauką danych.

Między programem jest samą architekturą potoku. Potok jest oparty na funkcji *indeksatora* , do której można przypisać *zestawu umiejętności*, która składa się z co najmniej jednej *umiejętności* zapewniającej AI. Potok jest przeznaczony do tworzenia *wzbogaconych dokumentów* , które wprowadzają jako nieprzetworzoną zawartość, ale pobierają dodatkową strukturę, kontekst i informacje podczas poruszania się po potoku. Wzbogacone dokumenty są używane podczas indeksowania do tworzenia odwróconych indeksów i innych struktur używanych w wyszukiwaniu pełnotekstowym lub eksploracji i analizie.

## <a name="start-with-services"></a>Zacznij od usług

Potrzebujesz platformy Azure Wyszukiwanie poznawcze i usługi Azure Blob Storage. W magazynie obiektów BLOB wymagany jest kontener, który udostępnia zawartość źródłową.

Możesz rozpocząć bezpośrednio na stronie portalu konta magazynu. Na lewej stronie nawigacyjnej w obszarze **BLOB Service** kliknij pozycję **Dodaj wyszukiwanie poznawcze platformy Azure** , aby utworzyć nową usługę, lub wybierz istniejącą. 

Po dodaniu Wyszukiwanie poznawcze platformy Azure do konta magazynu można wykonać standardowy proces, aby wzbogacić dane w dowolnym źródle danych platformy Azure. Zaleca się, aby Kreator **importu danych** na platformie Azure wyszukiwanie poznawczeł łatwy początkowy wprowadzenie do wzbogacania AI. Ten przewodnik Szybki Start przeprowadzi Cię przez kroki: [Tworzenie potoku wzbogacania AI w portalu](cognitive-search-quickstart-blob.md). 

W poniższych sekcjach omówiono więcej składników i koncepcji.

## <a name="use-a-blob-indexer"></a>Używanie indeksatora obiektów BLOB

Wzbogacanie AI jest dodatkiem do potoku indeksowania i w usłudze Azure Wyszukiwanie poznawcze te potoki są zbudowane na podstawie *indeksatora*. Indeksator to podusługa z obsługą źródła danych, która jest wyposażona w wewnętrzną logikę do próbkowania danych, odczytywanie danych metadanych, pobieranie danych i Serializowanie danych z formatów natywnych do dokumentów JSON w celu późniejszego zaimportowania. Indeksatory są często używane do importowania, oddziel je od AI, ale jeśli chcesz skompilować potok wzbogacania AI, będziesz potrzebować indeksatora i zestawu umiejętności. Ta sekcja wyróżnia indeksator; Następna sekcja koncentruje się na umiejętności.

Obiekty blob w usłudze Azure Storage są indeksowane przy użyciu [indeksatora usługi azure wyszukiwanie poznawcze BLOB Storage](search-howto-indexing-azure-blob-storage.md). Można wywołać ten indeksator przy użyciu kreatora **importu danych** , interfejsu API REST lub zestawu .NET SDK. W kodzie, używasz tego indeksatora, ustawiając typ i dostarczając informacje o połączeniu, które obejmują konto usługi Azure Storage wraz z kontenerem obiektów BLOB. Obiekty blob można podzbiórować przez utworzenie katalogu wirtualnego, który można następnie przekazać jako parametr lub przez filtrowanie według rozszerzenia typu pliku.

Indeksator wykonuje "pęknięcia dokumentu", otwierając obiekt BLOB w celu przeprowadzenia inspekcji zawartości. Po nawiązaniu połączenia ze źródłem danych jest to pierwszy krok w potoku. W przypadku danych obiektów BLOB są wykrywane pliki PDF, dokumenty pakietu Office, obrazy i inne typy zawartości. Łamanie dokumentów przy użyciu wyodrębniania tekstu nie jest naliczane. W przypadku rozpakowywania dokumentu przy użyciu wyodrębniania obrazów opłaty są naliczone według stawek, które można znaleźć na [stronie cennika](https://azure.microsoft.com/pricing/details/search/).

Chociaż wszystkie dokumenty będą rozpękane, wzbogaca się tylko wtedy, gdy użytkownik jawnie udostępni odpowiednie umiejętności. Na przykład jeśli potok składa się wyłącznie z analizy obrazów, tekst w kontenerze lub dokumentach jest ignorowany.

Indeksator obiektów BLOB zawiera parametry konfiguracji i obsługuje śledzenie zmian, jeśli dane podstawowe zawierają wystarczające informacje. Aby dowiedzieć się więcej na temat podstawowych funkcji w [usłudze Azure wyszukiwanie poznawcze indeksator magazynu obiektów BLOB](search-howto-indexing-azure-blob-storage.md), należy zapoznać się z tematem.

## <a name="add-ai-components"></a>Dodaj składniki AI

Wzbogacanie AI odnosi się do modułów, które szukają wzorców lub cech, a następnie wykonać odpowiednią operację. Rozpoznawanie twarzy na zdjęciach, tekstowe opisy zdjęć, wykrywanie kluczowych fraz w dokumencie, a także OCR (lub rozpoznawanie tekstu napisanego lub odręcznego w plikach binarnych) to przykłady ilustrujące.

Na platformie Azure Wyszukiwanie poznawcze *umiejętności* są indywidualnymi składnikami przetwarzania AI, których można używać autonomicznie lub w połączeniu z innymi umiejętnościami. 

+ Wbudowane umiejętności są obsługiwane przez Cognitive Services, z analizą obrazów na podstawie przetwarzanie obrazów i przetwarzania w języku naturalnym opartym na analiza tekstu. Aby zapoznać się z pełną listą, zobacz [wbudowaną umiejętność wzbogacania zawartości](cognitive-search-predefined-skills.md).

+ Niestandardowe umiejętności to kod niestandardowy, opakowany w [definicję interfejsu](cognitive-search-custom-skill-interface.md) , która umożliwia integrację z potokiem. W rozwiązaniach klientów powszechną techniką jest korzystanie z nich zarówno z niestandardowymi umiejętnościami, jak i modułami AI "open source", innych firm lub firm.

*Zestawu umiejętności* to zbiór umiejętności użytych w potoku i jest wywoływany po fazie pękania dokumentów. Indeksator może używać dokładnie jednego zestawu umiejętności, ale zestawu umiejętności istnieje niezależnie od indeksatora, aby można go było ponownie wykorzystać w innych scenariuszach.

Umiejętności niestandardowe mogą być solidnie skomplikowane, ale mogą być proste i łatwe w realizacji. Jeśli masz istniejące pakiety, które zapewniają dopasowania wzorców lub modele klasyfikacji, zawartość wyodrębnianą z obiektów BLOB może zostać przeniesiona do tych modeli w celu przetworzenia. Ponieważ wzbogacanie AI korzysta z platformy Azure, model powinien również znajdować się na platformie Azure. Niektóre typowe metody hostingu obejmują używanie [Azure Functions](cognitive-search-create-custom-skill-example.md) lub [kontenerów](https://github.com/Microsoft/SkillsExtractorCognitiveSearch).

Wbudowane umiejętności, które są obsługiwane przez Cognitive Services wymagają [dołączonego Cognitive Services](cognitive-search-attach-cognitive-services.md) klucz subskrypcji All-in-one, który zapewnia dostęp do zasobu. Klucz "wszystko w jednym" umożliwia analizę obrazu, wykrywanie języka, tłumaczenie tekstu i analizę tekstu. Inne wbudowane umiejętności to funkcje platformy Azure Wyszukiwanie poznawcze i nie wymagają dodatkowych usług ani kluczy. Kształtowanie tekstu, rozdzielacz i scalanie to przykłady umiejętności pomocnika, które są czasami niezbędne podczas projektowania potoku.

Jeśli używasz tylko umiejętności niestandardowych i wbudowanych narzędzi, nie ma żadnej zależności ani kosztów związanych z Cognitive Services.

<!-- ## Order of operations

Now we've covered indexers, content extraction, and skills, we can take a closer look at pipeline mechanisms and order of operations.

A skillset is a composition of one or more skills. When multiple skills are involved, the skillset operates as sequential pipeline, producing dependency graphs, where output from one skill becomes input to another. 

For example, given a large blob of unstructured text, a sample order of operations for text analytics might be as follows:

1. Use Text Splitter to break the blob into smaller parts.
1. Use Language Detection to determine if content is English or another language.
1. Use Text Translator to get all text into a common language.
1. Run Entity Recognition, Key Phrase Extraction, or Sentiment Analysis on chunks of text. In this step, new fields are created and populated. Entities might be location, people, organization, dates. Key phrases are short combinations of words that appear to belong together. Sentiment score is a rating on continuum of negative (0) to positive (1) sentiment.
1. Use Text Merger to reconstitute the document from the smaller chunks. -->

## <a name="consume-ai-enriched-output-in-downstream-solutions"></a>Korzystanie z ulepszonych danych wyjściowych AI w rozwiązaniach podrzędnych

Dane wyjściowe wzbogacania AI to indeks wyszukiwania w usłudze Azure Wyszukiwanie poznawcze lub [Magazyn wiedzy](knowledge-store-concept-intro.md) w usłudze Azure Storage.

Na platformie Azure Wyszukiwanie poznawcze indeks wyszukiwania jest używany na potrzeby interaktywnej eksploracji przy użyciu dowolnego tekstu i filtrowanych zapytań w aplikacji klienckiej. Wzbogacone dokumenty utworzone za pomocą AI są sformatowane w formacie JSON i indeksowane w taki sam sposób, jak wszystkie dokumenty są indeksowane na platformie Azure Wyszukiwanie poznawcze, przy użyciu wszystkich korzyści oferowanych przez indeksator. Na przykład podczas indeksowania indeksator obiektu BLOB odwołuje się do parametrów i ustawień konfiguracji w celu wykorzystania dowolnego mapowania pól lub logiki wykrywania zmian. Takie ustawienia są w pełni dostępne dla regularnego indeksowania i wzbogaconych obciążeń AI. Indeksowanie końcowe, gdy zawartość jest przechowywana w usłudze Azure Wyszukiwanie poznawcze, można tworzyć rozbudowane zapytania i wyrażenia filtru, aby zrozumieć zawartość.

W usłudze Azure Storage magazyn wiedzy ma dwie manifesty: kontener obiektów blob lub tabele w magazynie tabel. 

+ Kontener obiektów BLOB przechwytuje w całości wzbogacone dokumenty, co jest przydatne, jeśli chcesz uzyskać strumieniowe źródło danych do innych procesów. 

+ W przeciwieństwie do magazynu tabel można obsłużyć fizyczne projekcje ulepszonych dokumentów. Można tworzyć wycinki lub warstwy wzbogaconych dokumentów, które zawierają lub wykluczają określone części. Do analizy w Power BI tabele w usłudze Azure Table Storage stają się źródłem danych w celu przeprowadzenia dalszej wizualizacji i eksploracji.

Wzbogacony dokument na końcu potoku różni się od oryginalnej wersji wejściowej przez obecność dodatkowych pól zawierających nowe informacje, które zostały wyodrębnione lub wygenerowane podczas wzbogacania. W związku z tym można korzystać z kombinacji oryginalnej i utworzonej zawartości niezależnie od używanej struktury danych wyjściowych.

## <a name="next-steps"></a>Następne kroki

Istnieje dużo więcej możliwości dzięki wzbogacaniu AI, aby maksymalnie wykorzystać swoje dane w usłudze Azure Storage, w tym łączenie Cognitive Services na różne sposoby i tworzenie niestandardowych umiejętności w przypadkach, gdy nie ma istniejącej usługi poznawczej dla tego scenariusza. Więcej informacji można uzyskać, korzystając z poniższych linków.

+ [Przekazywanie, pobieranie i wyświetlanie listy obiektów BLOB za pomocą Azure Portal (Azure Blob Storage)](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal)
+ [Konfigurowanie indeksatora obiektów BLOB (Azure Wyszukiwanie poznawcze)](search-howto-indexing-azure-blob-storage.md) 
+ [Omówienie wzbogacenia AI (Azure Wyszukiwanie poznawcze)](cognitive-search-concept-intro.md) 
+ [Tworzenie zestawu umiejętności (Azure Wyszukiwanie poznawcze)](cognitive-search-defining-skillset.md)
+ [Mapowanie węzłów w drzewie adnotacji (Wyszukiwanie poznawcze platformy Azure)](cognitive-search-output-field-mapping.md)
