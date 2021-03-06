---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 03/25/2020
ms.author: trbye
ms.openlocfilehash: be60a2f371148fabf73fc7fcdce114295775d71c
ms.sourcegitcommit: 34a6fa5fc66b1cfdfbf8178ef5cdb151c97c721c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/28/2020
ms.locfileid: "80986254"
---
## <a name="prerequisites"></a>Wymagania wstępne

W tym artykule przyjęto założenie, że masz konto platformy Azure i subskrypcję usługi mowy. Jeśli nie masz konta i subskrypcji, [Wypróbuj usługę mowy bezpłatnie](../../../get-started.md).

## <a name="install-the-speech-sdk"></a>Instalowanie zestawu SDK usługi Mowa

Przed wykonaniem jakichkolwiek czynności należy zainstalować zestaw Speech SDK.

```Python
pip install azure-cognitiveservices-speech
```

Jeśli pracujesz w programie macOS i masz problemy z instalacją, może być konieczne uruchomienie tego polecenia jako pierwsze.

```Python
python3 -m pip install --upgrade pip
```

Po zainstalowaniu zestawu Speech SDK należy uwzględnić następujące instrukcje importu u góry skryptu.

```Python
from azure.cognitiveservices.speech import AudioDataStream, SpeechConfig, SpeechSynthesizer, SpeechSynthesisOutputFormat
from azure.cognitiveservices.speech.audio import AudioOutputConfig
```

## <a name="create-a-speech-configuration"></a>Tworzenie konfiguracji mowy

Aby wywołać usługę mowy przy użyciu zestawu Speech SDK, należy utworzyć [`SpeechConfig`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechconfig?view=azure-python). Ta klasa zawiera informacje o subskrypcji, takie jak klucz i skojarzony region, punkt końcowy, Host lub Token autoryzacji.

> [!NOTE]
> Bez względu na to, czy wykonujesz rozpoznawanie mowy, synteza mowy, tłumaczenie czy rozpoznawanie intencji, zawsze utworzysz konfigurację.

Istnieje kilka sposobów na zainicjowanie [`SpeechConfig`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechconfig?view=azure-python):

* Z subskrypcją: Przekaż klucz i skojarzony region.
* Z punktem końcowym: Pass w punkcie końcowym usługi mowy. Klucz lub Token autoryzacji jest opcjonalny.
* Z hostem: Przekaż adres hosta. Klucz lub Token autoryzacji jest opcjonalny.
* Z tokenem autoryzacji: Przekaż Token autoryzacji i skojarzony region.

W tym przykładzie utworzysz [`SpeechConfig`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechconfig?view=azure-python) przy użyciu klucza subskrypcji i regionu. Aby znaleźć identyfikator regionu, zobacz stronę [Obsługa regionów](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#speech-sdk) .

```python
speech_config = SpeechConfig(subscription="YourSubscriptionKey", region="YourServiceRegion")
```

## <a name="synthesize-speech-to-a-file"></a>Wyrównać mowę do pliku

Następnie utworzysz [`SpeechSynthesizer`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechsynthesizer?view=azure-python) obiekt, który wykonuje konwersje zamiany tekstu na mowę i wyjście na głośniki, pliki lub inne strumienie wyjściowe. [`SpeechSynthesizer`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechsynthesizer?view=azure-python) Akceptuje jako params [`SpeechConfig`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechconfig?view=azure-python) obiekt utworzony w poprzednim kroku oraz [`AudioOutputConfig`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.audio.audiooutputconfig?view=azure-python) obiekt, który określa sposób obsługi wyników.

Aby rozpocząć, Utwórz element `AudioOutputConfig` , aby automatycznie zapisywać dane wyjściowe do `.wav` pliku przy użyciu parametru `filename` konstruktora.

```python
audio_config = AudioOutputConfig(filename="path/to/write/file.wav")
```

Następnie Utwórz wystąpienie a `SpeechSynthesizer` przez przekazanie `speech_config` obiektu i `audio_config` obiektu jako parametrów. Następnie wykonanie syntezy mowy i zapis w pliku jest tak proste jak uruchamianie `speak_text_async()` z ciągiem tekstu.

```python
synthesizer = SpeechSynthesizer(speech_config=speech_config, audio_config=audio_config)
synthesizer.speak_text_async("A simple test to write to a file.")
```

Uruchom program, a `.wav` plik z syntezą jest zapisywana w określonej lokalizacji. Jest to dobry przykład typowego użycia, ale następnym zapoznaj się z tematem dostosowywania danych wyjściowych i obsługi odpowiedzi wyjściowej jako strumienia znajdującego się w pamięci na potrzeby pracy z niestandardowymi scenariuszami.

## <a name="synthesize-to-speaker-output"></a>Synteza danych wyjściowych prezentera

W niektórych przypadkach można chcieć bezpośrednio wyprowadzać dane z głosu do osoby mówiącej. Aby to zrobić, użyj przykładu w poprzedniej sekcji, ale zmień go `AudioOutputConfig` , usuwając `filename` parametry i ustaw. `use_default_speaker=True` To wyjście do bieżącego aktywnego urządzenia wyjściowego.

```python
audio_config = AudioOutputConfig(use_default_speaker=True)
```

## <a name="get-result-as-an-in-memory-stream"></a>Pobierz wynik jako strumień w pamięci

W przypadku wielu scenariuszy tworzenia aplikacji mowy potrzebne są wyniki danych audio jako strumień w pamięci zamiast bezpośredniego zapisywania do pliku. Pozwoli to na tworzenie zachowań niestandardowych, w tym:

* Abstrakcyjna w efekcie tablica bajtów jako strumień umożliwiający wyszukiwanie niestandardowych usług podrzędnych.
* Zintegruj wynik z innymi interfejsami API lub usługami.
* Modyfikowanie danych audio, zapisywanie niestandardowych `.wav` nagłówków itp.

Jest to proste, aby wprowadzić tę zmianę z poprzedniego przykładu. Najpierw usuń `AudioConfig`, ponieważ będziesz zarządzać zachowaniem danych wyjściowych ręcznie z tego punktu, aby zwiększyć kontrolę. Następnie Przekaż `None` `AudioConfig` w `SpeechSynthesizer` konstruktorze. 

> [!NOTE]
> W `None` przypadku, `AudioConfig`gdy nie zostanie pominięty w powyższym przykładzie danych wyjściowych prezentera, nie będzie odtwarzany dźwięk domyślnie na bieżącym aktywnym urządzeniu wyjściowym.

Tym razem można zapisać wynik w [`SpeechSynthesisResult`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechsynthesisresult?view=azure-python) zmiennej. `audio_data` Właściwość zawiera `bytes` obiekt danych wyjściowych. Możesz współpracować z tym obiektem ręcznie lub użyć [`AudioDataStream`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.audiodatastream?view=azure-python) klasy do zarządzania strumieniem znajdującym się w pamięci. W tym przykładzie użyjesz konstruktora `AudioDataStream` , aby uzyskać strumień z wyniku.

```python
synthesizer = SpeechSynthesizer(speech_config=speech_config, audio_config=None)
result = synthesizer.speak_text_async("Getting the response as an in-memory stream.").get()
stream = AudioDataStream(result)
```

W tym miejscu można zaimplementować dowolne zachowanie niestandardowe przy użyciu obiektu `stream` wyniku.

## <a name="customize-audio-format"></a>Dostosuj format audio

W poniższej sekcji pokazano, jak dostosować atrybuty wyjściowe audio, w tym:

* Typ pliku audio
* Szybkość próbkowania
* Bit — Głębokość

Aby zmienić format dźwięku, należy użyć `set_speech_synthesis_output_format()` funkcji dla `SpeechConfig` obiektu. Ta funkcja oczekuje `enum` typu [`SpeechSynthesisOutputFormat`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechsynthesisoutputformat?view=azure-python), którego można użyć do wybrania formatu danych wyjściowych. [Listę dostępnych formatów audio](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.speechsynthesisoutputformat?view=azure-python) można znaleźć w dokumentacji referencyjnej.

Istnieją różne opcje dla różnych typów plików, w zależności od wymagań. Należy pamiętać, że zgodnie z definicją `Raw24Khz16BitMonoPcm` , formaty nieprzetworzone, takie jak nie obejmują nagłówków audio. Używaj formatów nieprzetworzonych tylko wtedy, gdy wiesz, że wdrożenie podrzędne może zdekodować surową Bitstream lub jeśli planujesz ręczne tworzenie nagłówków na podstawie głębi bitowej, szybkości próbkowania, liczby kanałów itd.

W tym przykładzie należy określić format `Riff24Khz16BitMonoPcm` RIFF o wysokiej wierności, ustawiając `SpeechSynthesisOutputFormat` dla `SpeechConfig` obiektu. Podobnie jak w przypadku przykładu w poprzedniej sekcji, należy [`AudioDataStream`](https://docs.microsoft.com/python/api/azure-cognitiveservices-speech/azure.cognitiveservices.speech.audiodatastream?view=azure-python) użyć, aby uzyskać strumień w pamięci wyniku, a następnie zapisać go do pliku.


```python
speech_config.set_speech_synthesis_output_format(SpeechSynthesisOutputFormat["Riff24Khz16BitMonoPcm"])
synthesizer = SpeechSynthesizer(speech_config=speech_config, audio_config=None)

result = synthesizer.speak_text_async("Customizing audio output format.").get()
stream = AudioDataStream(result)
stream.save_to_wav_file("path/to/write/file.wav")
```

Ponowne uruchomienie programu zapisze dostosowany `.wav` plik do określonej ścieżki.

## <a name="use-ssml-to-customize-speech-characteristics"></a>Użyj SSML, aby dostosować charakterystykę mowy

Język znaczników syntezy mowy (SSML) umożliwia precyzyjne dostosowanie wielkości liter, wymowy, liczby głosu i większej liczby danych wyjściowych zamiany tekstu na mowę przez przesłanie żądań ze schematu XML. W tej sekcji przedstawiono kilka praktycznych przykładów użycia, ale w celu uzyskania bardziej szczegółowego przewodnika zapoznaj się z [artykułem How to SSML](../../../speech-synthesis-markup.md).

Aby rozpocząć korzystanie z SSML do dostosowywania, należy wprowadzić prostą zmianę, która przełącza głos.
Najpierw utwórz nowy plik XML dla konfiguracji SSML w katalogu głównym projektu, w tym przykładzie `ssml.xml`. Element główny jest zawsze `<speak>`, a Zawijanie tekstu w `<voice>` elemencie pozwala na zmianę głosu przy użyciu `name` parametru. Ten przykład zmienia głos na styk brytyjski (Zjednoczone Królestwo). Należy zauważyć, że ten głos jest **standardowym** głosem, który ma inne ceny i dostępność niż **neuronowych** głosów. Zapoznaj się z [pełną listą](https://docs.microsoft.com/azure/cognitive-services/speech-service/language-support#standard-voices) obsługiwanych głosów **standardowych** .

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
  <voice name="en-GB-George-Apollo">
    When you're on the motorway, it's a good idea to use a sat-nav.
  </voice>
</speak>
```

Następnie musisz zmienić żądanie syntezy mowy, aby odwołać się do pliku XML. Żądanie jest w większości takie samo, ale zamiast przy użyciu `speak_text_async()` funkcji, używasz. `speak_ssml_async()` Ta funkcja oczekuje ciągu XML, więc najpierw należy odczytać konfigurację SSML jako ciąg. W tym miejscu obiekt wynik jest dokładnie taki sam jak w poprzednich przykładach.

> [!NOTE]
> Jeśli element `ssml_string` zawiera `ï»¿` na początku ciągu, należy wyłączyć format BOM lub usługa zwróci błąd. W tym celu należy ustawić `encoding` parametr w następujący sposób: `open("ssml.xml", "r", encoding="utf-8-sig")`.

```python
synthesizer = SpeechSynthesizer(speech_config=speech_config, audio_config=None)

ssml_string = open("ssml.xml", "r").read()
result = synthesizer.speak_ssml_async(ssml_string).get()

stream = AudioDataStream(result)
stream.save_to_wav_file("path/to/write/file.wav")
```

Dane wyjściowe działają, ale wprowadzono kilka prostych dodatkowych zmian, które ułatwiają bardziej naturalny dźwięk. Ogólna szybkość mówienia jest nieco zbyt szybka, dlatego dodamy `<prosody>` znacznik i obniży szybkość do **90%** częstotliwości domyślnej. Ponadto wstrzymanie po przecinku w zdaniu jest nieco zbyt krótkie i nienaturalne. Aby rozwiązać ten problem, Dodaj `<break>` znacznik, aby opóźnić mowę i ustawić parametry czasu na **200ms**. Uruchom ponowną syntezę, aby zobaczyć, jak te dostosowania wpłynęły na dane wyjściowe.

```xml
<speak version="1.0" xmlns="https://www.w3.org/2001/10/synthesis" xml:lang="en-US">
  <voice name="en-GB-George-Apollo">
    <prosody rate="0.9">
      When you're on the motorway,<break time="200ms"/> it's a good idea to use a sat-nav.
    </prosody>
  </voice>
</speak>
```

## <a name="neural-voices"></a>Głosy neuronowych

Głosy neuronowych są algorytmami syntezy mowy obsługiwanymi przez głębokie sieci neuronowych. W przypadku korzystania z głosu neuronowych, synteza mowy jest niemal nieczytelna w odróżnieniu od nagrań ludzkich. Podobnie jak naturalna prosodya i wyraźny zbiór wyrazów, głosy neuronowych znacząco zmniejszają zmęczenie nasłuchiwania, gdy użytkownicy współpracują z systemami AI.

Aby przełączyć się na głos neuronowych, Zmień `name` na jedną z [opcji głosu neuronowych](https://docs.microsoft.com/azure/cognitive-services/speech-service/language-support#neural-voices). Następnie Dodaj przestrzeń nazw XML dla `mstts`i zawiń tekst w `<mstts:express-as>` tagu. Użyj parametru `style` , aby dostosować styl mówiący. Ten przykład używa `cheerful`, ale spróbuje ustawić `customerservice` lub `chat` , aby zobaczyć różnicę w stylu mówiącym.

> [!IMPORTANT]
> Głosy neuronowych są obsługiwane **tylko** w przypadku zasobów mowy utworzonych w regionach *Wschodnie stany usa*, *Południowe Azja Wschodnia*i *Europa Zachodnia* .

```xml
<speak version="1.0" xmlns="http://www.w3.org/2001/10/synthesis" xmlns:mstts="https://www.w3.org/2001/mstts" xml:lang="en-US">
  <voice name="en-US-AriaNeural">
    <mstts:express-as style="cheerful">
      This is awesome!
    </mstts:express-as>
  </voice>
</speak>
```
