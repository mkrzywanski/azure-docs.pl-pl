---
author: trevorbye
ms.service: cognitive-services
ms.topic: include
ms.date: 03/27/2020
ms.author: trbye
ms.openlocfilehash: 138a9ef9d483ca0d460bab7185d646669650f83e
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/29/2020
ms.locfileid: "81400037"
---
:::row:::
    :::column span="3":::
        Zestaw Speech SDK obsługuje systemy Windows 10 i Windows Server 2016 lub nowsze. Wcześniejsze wersje **nie** są oficjalnie obsługiwane. Istnieje możliwość użycia części zestawu Speech SDK z wcześniejszymi wersjami systemu Windows, chociaż nie jest to zalecane.
    :::column-end:::
    :::column:::
        <br>
        <div class="icon is-large">
            <img alt="Windows" src="https://docs.microsoft.com/media/logos/logo_Windows.svg" width="60px">
        </div>
    :::column-end:::
:::row-end:::

### <a name="system-requirements"></a>Wymagania systemowe

Zestaw Speech SDK w systemie Windows wymaga <a href="https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads" target="_blank">Microsoft Visual C++ pakietu redystrybucyjnego dla programu <span class="docon docon-navigate-external x-hidden-focus"></span> Visual Studio 2019</a> w systemie.

- <a href="https://aka.ms/vs/16/release/vc_redist.x86.exe" target="_blank">Zainstaluj dla architektury x86<span class="docon docon-navigate-external x-hidden-focus"></span></a>
- <a href="https://aka.ms/vs/16/release/vc_redist.x64.exe" target="_blank">Zainstaluj dla x64<span class="docon docon-navigate-external x-hidden-focus"></span></a>
- <a href="https://aka.ms/vs/16/release/vc_redist.arm64.exe" target="_blank">Zainstaluj dla ARMx64<span class="docon docon-navigate-external x-hidden-focus"></span></a>

### <a name="c"></a>C#

[!INCLUDE [Get .NET Speech SDK](get-speech-sdk-dotnet.md)]

W przypadku danych wejściowych mikrofonu należy zainstalować biblioteki platforma Media Foundation. Te biblioteki są częścią systemów Windows 10 i Windows Server 2016. Istnieje możliwość użycia zestawu Speech SDK bez tych bibliotek, o ile mikrofon nie jest używany jako urządzenie wejściowe audio.

Wymagane pliki zestawu Speech SDK można wdrożyć w tym samym katalogu, w którym znajduje się aplikacja. Dzięki temu aplikacja może bezpośrednio uzyskiwać dostęp do bibliotek. Upewnij się, że wybrano poprawną wersję (x86/x64) zgodną z aplikacją.

| Nazwa                                            | Funkcja                                             |
|-------------------------------------------------|------------------------------------------------------|
| `Microsoft.CognitiveServices.Speech.core.dll`   | Podstawowy zestaw SDK wymagany do wdrożenia natywnego i zarządzanego |
| `Microsoft.CognitiveServices.Speech.csharp.dll` | Wymagane do wdrożenia zarządzanego                      |

> [!NOTE]
> Począwszy od wersji 1.3.0 plik `Microsoft.CognitiveServices.Speech.csharp.bindings.dll` (dostarczony w poprzednich wersjach) nie jest już wymagany. Ta funkcja jest teraz zintegrowana z podstawowym zestawem SDK.

> [!IMPORTANT]
> W przypadku projektu C# aplikacji Windows Forms (.NET Framework) Upewnij się, że biblioteki są uwzględnione w ustawieniach wdrażania projektu. Możesz to sprawdzić w sekcji `Properties -> Publish Section`. Kliknij `Application Files` przycisk i Znajdź odpowiednie biblioteki z listy rozwijanej. Upewnij się, że wartość jest ustawiona `Included`na. Program Visual Studio uwzględni plik, gdy projekt jest publikowany/wdrażany.

### <a name="c"></a>C++

[!INCLUDE [Get C++ Speech SDK](get-speech-sdk-cpp.md)]

### <a name="python"></a>Python

[!INCLUDE [Get Python Speech SDK](get-speech-sdk-python.md)]

### <a name="java"></a>Java

[!INCLUDE [Get Java Speech SDK](get-speech-sdk-java.md)]
