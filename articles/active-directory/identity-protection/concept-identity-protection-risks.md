---
title: Co to jest ryzyko? Usługa Azure AD Identity Protection
description: Wyjaśnienie ryzyka w Azure AD Identity Protection
services: active-directory
ms.service: active-directory
ms.subservice: identity-protection
ms.topic: conceptual
ms.date: 06/26/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahandle
ms.collection: M365-identity-device-management
ms.openlocfilehash: de905c61642c36a07c7f87e0be910b0f035bffc1
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "85555262"
---
# <a name="what-is-risk"></a>Co to jest ryzyko?

Wykrycia ryzyka w Azure AD Identity Protection zawierają wszystkie zidentyfikowane podejrzane działania związane z kontami użytkowników w katalogu.

Ochrona tożsamości zapewnia organizacjom dostęp do zaawansowanych zasobów i szybkie reagowanie na podejrzane działania. 

![Omówienie zabezpieczeń przedstawiające ryzykownych użytkowników i logowania](./media/concept-identity-protection-risks/identity-protection-security-overview.png)

## <a name="risk-types-and-detection"></a>Typy ryzyka i wykrywanie

Istnieją dwa typy **użytkowników** ryzyka i **logowania** oraz dwa typy wykrywania lub obliczeń w czasie **rzeczywistym** i **w trybie offline**.

### <a name="user-risk"></a>Ryzyko użytkownika

Ryzyko użytkownika reprezentuje prawdopodobieństwo naruszenia bezpieczeństwa tożsamości lub konta. 

Te zagrożenia są obliczane w trybie offline przy użyciu wewnętrznych i zewnętrznych źródeł analizy zagrożeń firmy Microsoft, w tym badaczy bezpieczeństwa, specjalistów ds. bezpieczeństwa, zespołów zabezpieczeń w firmie Microsoft i innych zaufanych źródeł.

| Wykrywanie ryzyka | Opis |
| --- | --- |
| Nieujawnione poświadczenia | Ten typ wykrywania zagrożeń wskazuje, że wykryto przeciek prawidłowych poświadczeń użytkownika. Gdy cybernetycznymi naruszają prawidłowe hasła dla uprawnionych użytkowników, często udostępniają te poświadczenia. Takie udostępnianie jest zwykle realizowane przez ogłaszanie publicznie w witrynie sieci Web, wklejanie witryn lub przez handel i sprzedawanie poświadczeń na czarnym rynku. Gdy usługa nieujawnione poświadczenia firmy Microsoft uzyskuje poświadczenia użytkownika z ciemnej witryny sieci Web, wklejania witryn lub innych źródeł, są one sprawdzane względem bieżących ważnych poświadczeń użytkowników usługi Azure AD w celu znalezienia prawidłowych dopasowań. Aby uzyskać więcej informacji na temat przecieków poświadczeń, zobacz [często zadawane pytania](#common-questions). |
| Analiza zagrożeń usługi Azure AD | Ten typ wykrywania zagrożeń wskazuje aktywność użytkownika nietypową dla danego użytkownika lub jest zgodna ze znanymi wzorcami ataków na podstawie wewnętrznych i zewnętrznych źródeł analizy zagrożeń firmy Microsoft. |

### <a name="sign-in-risk"></a>Ryzyko związane z logowaniem

Ryzyko związane z logowaniem reprezentuje prawdopodobieństwo, że dane żądanie uwierzytelnienia nie jest autoryzowane przez właściciela tożsamości. 

Te zagrożenia mogą być obliczane w czasie rzeczywistym lub obliczane w trybie offline przy użyciu wewnętrznych i zewnętrznych źródeł analizy zagrożeń firmy Microsoft, w tym badaczy bezpieczeństwa, specjalistów wymuszania praw, zespołów ds. zabezpieczeń w firmie Microsoft i innych zaufanych źródeł.

| Wykrywanie ryzyka | Typ wykrywania | Opis |
| --- | --- | --- |
| Anonimowy adres IP | Przesyłanie w czasie rzeczywistym | Ten typ wykrywania ryzyka oznacza logowanie z anonimowego adresu IP (na przykład przeglądarki tor lub anonimowych sieci VPN). Te adresy IP są zwykle używane przez aktorów, którzy chcą ukryć swoje dane telemetryczne logowania (adres IP, lokalizacja, urządzenie itp.) dla potencjalnie złośliwego celu. |
| Nietypowe podróże | W trybie offline | Ten typ wykrywania ryzyka identyfikuje dwa logowania pochodzące z lokalizacji geograficznie odległych, gdzie co najmniej jedna z tych lokalizacji może być nietypowa dla użytkownika, pod kątem wcześniejszego zachowania. Ten algorytm uczenia maszynowego uwzględnia między innymi różne czynniki czas między dwoma logowaniami i czas, jaki zajęł użytkownikowi w podróży od pierwszej lokalizacji do drugiego, co oznacza, że inny użytkownik korzysta z tych samych poświadczeń. <br><br> Algorytm ignoruje oczywiste "fałszywie dodatnie" przyczyniające się do niemożliwych warunków podróży, takich jak sieci VPN i lokalizacje regularnie używane przez innych użytkowników w organizacji. System ma początkowy okres uczenia z najwcześniej 14 dni lub 10 logowań, podczas którego uczy się o zachowanie logowania nowego użytkownika. |
| Połączony adres IP złośliwego oprogramowania | W trybie offline | Ten typ wykrywania zagrożeń wskazuje logowania z adresów IP zainfekowanych złośliwym oprogramowaniem, które jest znane, aby aktywnie komunikować się z serwerem bot. To wykrywanie jest określane przez skorelowanie adresów IP urządzenia użytkownika z adresami IP, które były w kontakcie z serwerem bot, gdy serwer bot był aktywny. |
| Nieznane właściwości logowania | Przesyłanie w czasie rzeczywistym | Ten typ wykrywania ryzyka uwzględnia wcześniejszą historię logowania (IP, szerokości geograficznej i ASN), aby wyszukać nietypowe logowania. System przechowuje informacje o poprzednich lokalizacjach używanych przez użytkownika i uwzględnia te "znane" lokalizacje. Wykrywanie ryzyka jest wyzwalane, gdy logowanie następuje z lokalizacji, która nie znajduje się na liście znanych lokalizacji. Nowo utworzeni użytkownicy będą w trybie uczenia się przez okres, w którym nieznane wykrycia ryzyka związanego z logowaniem zostaną wyłączone w czasie, gdy algorytmy wiedzą o zachowaniu użytkownika. Czas trwania trybu uczenia jest dynamiczny i zależy od tego, ile czasu zajmuje algorytm zbierania wystarczającej ilości informacji o wzorcach logowania użytkownika. Minimalny czas trwania wynosi pięć dni. Użytkownik może wrócić do trybu uczenia po długim czasie braku aktywności. System ignoruje logowania ze znanych urządzeń i lokalizacje, które znajdują się geograficznie blisko znanej lokalizacji. <br><br> Uruchamiamy również to wykrywanie uwierzytelniania podstawowego (lub starszych protokołów). Ponieważ te protokoły nie mają nowoczesnych właściwości, takich jak identyfikator klienta, jest ograniczona liczba danych telemetrycznych, aby zmniejszyć liczbę fałszywie dodatnich. Zalecamy, aby nasi klienci mogli przejść do nowoczesnego uwierzytelniania. |
| Administrator zatwierdził naruszenie zabezpieczeń | W trybie offline | To wykrywanie wskazuje, że administrator zaznaczył "Potwierdzanie naruszenia przez użytkownika" w interfejsie użytkownika ryzykownych użytkowników lub przy użyciu interfejsu API riskyUsers. Aby sprawdzić, który administrator został naruszony, należy sprawdzić historię ryzyka użytkownika (za pośrednictwem interfejsu użytkownika lub interfejsu API). |
| Złośliwy adres IP | W trybie offline | To wykrywanie wskazuje, że logowanie jest ze złośliwego adresu IP. Adres IP jest uznawany za złośliwy na podstawie częstych awarii z powodu nieprawidłowych poświadczeń odebranych z adresu IP lub innych źródeł reputacji adresów IP. |
| Podejrzane reguły manipulowania skrzynką odbiorczą | W trybie offline | To wykrywanie jest odnajdywane przez [Microsoft Cloud App Security (MCAS)](/cloud-app-security/anomaly-detection-policy#suspicious-inbox-manipulation-rules). To wykrywanie powoduje profilowanie środowiska i wyzwala alerty w przypadku podejrzanych reguł, które usuwają lub przenoś wiadomości lub foldery są ustawiane w skrzynce odbiorczej użytkownika. To wykrywanie może wskazywać na naruszenie zabezpieczeń konta użytkownika, a tym samym celowe ukrycie komunikatów oraz użycie tej skrzynki pocztowej do dystrybucji spamu lub złośliwego oprogramowania w organizacji. |
| Niemożliwa podróż | W trybie offline | To wykrywanie jest odnajdywane przez [Microsoft Cloud App Security (MCAS)](/cloud-app-security/anomaly-detection-policy#impossible-travel). To wykrywanie identyfikuje dwie działania użytkownika (jest to jedna lub wiele sesji) pochodzące z lokalizacji geograficznie odległych w przedziale czasowym krótszym niż czas, w którym użytkownik przejdzie od pierwszej lokalizacji do drugiego, co oznacza, że inny użytkownik korzysta z tych samych poświadczeń. |

### <a name="other-risk-detections"></a>Inne wykrycia ryzyka

| Wykrywanie ryzyka | Typ wykrywania | Opis |
| --- | --- | --- |
| Wykryto dodatkowe ryzyko | W czasie rzeczywistym lub w trybie offline | To wykrywanie wskazuje, że wykryto jedno z powyższych wykryć w warstwie Premium. Ponieważ wykrycia Premium są widoczne tylko dla Azure AD — wersja Premium klientów P2, są zatytułowane "dodatkowe ryzyko wykryte" dla klientów bez licencji na Azure AD — wersja Premium P2. |

## <a name="common-questions"></a>Często zadawane pytania

### <a name="leaked-credentials"></a>Nieujawnione poświadczenia

#### <a name="where-does-microsoft-find-leaked-credentials"></a>Gdzie firma Microsoft znalazła ujawnione poświadczenia?

Firma Microsoft wyszukuje nieujawnione poświadczenia w różnych miejscach, w tym:

- Publiczne witryny wklejania, takie jak pastebin.com i paste.ca, gdzie niewłaściwe podmioty zwykle publikują taki materiał. Ta lokalizacja jest najbardziej nieprawidłowymi aktorami — najpierw Zatrzymaj na nich, aby znaleźć skradzione poświadczenia.
- Agencje egzekwowania prawa.
- Inne grupy w firmie Microsoft przeprowadzają ciemne badania w sieci Web.

#### <a name="why-arent-i-seeing-any-leaked-credentials"></a>Dlaczego nie widzę żadnych przecieków poświadczeń?

Nieujawnione poświadczenia są przetwarzane, gdy firma Microsoft odnajdzie nową, publicznie dostępną partię. Ze względu na poufną naturę nieujawnione poświadczenia są usuwane wkrótce po przetworzeniu. Po włączeniu synchronizacji skrótów haseł (PHS) zostaną przetworzone tylko nowe nieujawnione poświadczenia. Weryfikowanie przed poprzednio znalezionymi parami poświadczeń nie jest wykonywane. 

#### <a name="i-havent-seen-any-leaked-credential-risk-events-for-quite-some-time"></a>Nie widzę żadnych nieoczekiwanych zdarzeń dotyczących ryzyka związanego z poświadczeniami przez jakiś czas?

Jeśli nie widzisz żadnych nieujawnionych zdarzeń ryzyka związanego z poświadczeniami, wynika z następujących powodów:

- Nie masz PHS włączonych dla Twojej dzierżawy.
- Firma Microsoft nie znalazła żadnych nieujawnionych par poświadczeń, które pasują do użytkowników.

#### <a name="how-often-does-microsoft-process-new-credentials"></a>Jak często firma Microsoft przetwarza nowe poświadczenia?

Poświadczenia są przetwarzane natychmiast po ich znalezieniu, zwykle w wielu partiach dziennie.

## <a name="next-steps"></a>Następne kroki

- [Zasady dostępne w celu ograniczenia ryzyka](concept-identity-protection-policies.md)
- [Omówienie zabezpieczeń](concept-identity-protection-security-overview.md)
