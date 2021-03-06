---
title: Informacje o certyfikatach Azure Key Vault — Azure Key Vault
description: Omówienie Azure Key Vault interfejsem REST i certyfikatami.
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: certificates
ms.topic: overview
ms.date: 09/04/2019
ms.author: mbaldwin
ms.openlocfilehash: 74007f5d10d58cf9680d4531304098cabe9b6d8b
ms.sourcegitcommit: 0820c743038459a218c40ecfb6f60d12cbf538b3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/23/2020
ms.locfileid: "87115666"
---
# <a name="about-azure-key-vault-certificates"></a>Informacje o certyfikatach usługi Azure Key Vault

Obsługa certyfikatów Key Vault umożliwia zarządzanie certyfikatami x509 i następującymi zachowaniami:  

-   Umożliwia właścicielowi certyfikatu utworzenie certyfikatu za pomocą procesu tworzenia Key Vault lub przez zaimportowanie istniejącego certyfikatu. Obejmuje certyfikaty wygenerowane zarówno z podpisem własnym, jak i urzędu certyfikacji.
-   Umożliwia właścicielowi certyfikatu Key Vault implementowanie bezpiecznego magazynu i zarządzanie certyfikatami x509 bez interakcji z materiałem klucza prywatnego.  
-   Umożliwia właścicielowi certyfikatu utworzenie zasad, które kierują Key Vault do zarządzania cyklem życia certyfikatu.  
-   Umożliwia właścicielom certyfikatu udostępnianie informacji kontaktowych w celu powiadomienia o zdarzeniach cyklu życia i odnowieniu certyfikatu.  
-   Obsługuje automatyczne odnawianie z wybranymi wystawcami — dostawcy certyfikatów x509 Key Vault partner/urzędy certyfikacji.

>[!Note]
>Niebędący dostawcami/urzędy są również dozwolone, ale nie obsługują funkcji autoodnawiania.

## <a name="composition-of-a-certificate"></a>Składanie certyfikatu

Po utworzeniu certyfikatu Key Vault zostanie również utworzony klucz adresowy i wpis tajny o tej samej nazwie. Klucz Key Vault pozwala na wykonywanie kluczowych operacji, a klucz tajny Key Vault umożliwia pobieranie wartości certyfikatu jako klucza tajnego. Certyfikat Key Vault zawiera również publiczne metadane certyfikatu x509.  

Identyfikator i wersja certyfikatów są podobne do tych kluczy i wpisów tajnych. W odpowiedzi na certyfikat Key Vault jest dostępna określona wersja klucza i wpisu tajnego, który został utworzony przy użyciu wersji certyfikatu Key Vault.
 
![Certyfikaty są obiektami złożonymi](../media/azure-key-vault.png)

## <a name="exportable-or-non-exportable-key"></a>Klucz możliwy do eksportu lub niemożliwy do eksportu

Po utworzeniu certyfikatu Key Vault można go pobrać ze klucza prywatnego z adresami w formacie PFX lub PEM. Zasady użyte do utworzenia certyfikatu muszą wskazywać, że klucz jest eksportowalny. Jeśli zasady wskazują, że nie można eksportować, klucz prywatny nie jest częścią wartości, gdy zostanie pobrany jako wpis tajny.  

Klucz adresowania jest bardziej istotny dla niemożliwych do eksportu certyfikatów KV. Operacje na kluczu KV z adresami są mapowane z pola *użycie* klucza dla zasad certyfikatu KV użytych do utworzenia certyfikatu kV.  

Obsługiwane są dwa typy kluczy — moduł HSM *RSA* lub *RSA* z certyfikatami. Eksport jest dozwolony tylko za pomocą RSA, ale nie jest obsługiwany przez moduł HSM RSA.  

## <a name="certificate-attributes-and-tags"></a>Atrybuty i Tagi certyfikatu

Oprócz metadanych certyfikatu, klucz adresowy i wpis tajny adresowane, certyfikat Key Vault również zawiera atrybuty i Tagi.  

### <a name="attributes"></a>Atrybuty

Atrybuty certyfikatu są duplikowane do atrybutów klucza, który można adresować i wpisu tajnego utworzonych podczas tworzenia certyfikatu KV.  

Certyfikat Key Vault ma następujące atrybuty:  

-   *włączone*: wartość logiczna, opcjonalna, **wartość**domyślna to true. Można określić, aby wskazać, czy dane certyfikatu mogą być pobierane jako tajne, czy jako klucz. Używany także w połączeniu z *NBF* i *EXP* , gdy operacja występuje między *NBF* i *EXP*, i będzie dozwolona tylko wtedy, gdy wartość Enabled jest ustawiona na true. Operacje poza oknem *NBF* i *EXP* są automatycznie niedozwolone.  

Istnieją dodatkowe atrybuty tylko do odczytu, które znajdują się w odpowiedzi:

-   *utworzono*: IntDate: wskazuje, kiedy ta wersja certyfikatu została utworzona.  
-   *Zaktualizowano*: IntDate: wskazuje, kiedy ta wersja certyfikatu została zaktualizowana.  
-   *EXP*: IntDate: zawiera wartość daty wygaśnięcia certyfikatu x509.  
-   *NBF*: IntDate: zawiera wartość daty certyfikatu x509.  

> [!Note] 
> Jeśli certyfikat usługi Key Vault wygaśnie, adresowany klucz i wpis tajny przestaną działać.  

### <a name="tags"></a>Tagi

 Określony przez klienta słownik par wartości klucza, podobny do tagów w kluczach i wpisach tajnych.  

 > [!Note]
> Znaczniki są odczytywane przez obiekt wywołujący, jeśli mają *listę* lub *uzyskują* uprawnienia do tego typu obiektu (klucze, wpisy tajne lub certyfikaty).

## <a name="certificate-policy"></a>Zasady certyfikatów

Zasady dotyczące certyfikatów zawierają informacje dotyczące sposobu tworzenia i zarządzania cyklem życia certyfikatu Key Vault. Gdy certyfikat z kluczem prywatnym zostanie zaimportowany do magazynu kluczy, zasady domyślne są tworzone przez odczytanie certyfikatu x509.  

Po utworzeniu certyfikatu Key Vault od podstaw należy podać zasady. Zasady określają sposób tworzenia tej wersji certyfikatu Key Vault lub następnej wersji certyfikatu Key Vault. Po ustanowieniu zasad nie jest to wymagane z kolejnymi operacjami tworzenia dla przyszłych wersji. Istnieje tylko jedno wystąpienie zasad dla wszystkich wersji certyfikatu Key Vault.  

Na wysokim poziomie zasady certyfikatów zawierają następujące informacje (ich definicje można znaleźć [tutaj](https://docs.microsoft.com/powershell/module/az.keyvault/set-azkeyvaultcertificatepolicy?view=azps-4.4.0)):  

-   Właściwości certyfikatu x509: zawiera nazwę podmiotu, alternatywne nazwy podmiotu i inne właściwości używane do tworzenia żądania certyfikatu x509.  
-   Właściwości klucza: zawiera typ klucza, Długość klucza, możliwe do eksportu i pola ReuseKeyOnRenewal. Te pola instruują Magazyn kluczy, aby utworzyć klucz.  
-   Właściwości klucza tajnego: zawiera właściwości tajne, takie jak typ zawartości klucza tajnego adresowanego do wygenerowania wartości klucza tajnego w celu pobierania certyfikatu jako klucza tajnego.  
-   Akcje okresu istnienia: zawiera akcje okresu istnienia dla certyfikatu KV. Każda akcja okresu istnienia zawiera:  

     - Wyzwalacz: określony przez dni przed upływem wartości procentowej wygaśnięcia lub okresu istnienia  

     - Akcja: Określanie typu akcji — *emailContacts* lub *autorenew*  

-   Wystawca: parametry dotyczące wystawcy certyfikatu do użycia w celu wystawiania certyfikatów x509.  
-   Atrybuty zasad: zawiera atrybuty skojarzone z zasadami  

### <a name="x509-to-key-vault-usage-mapping"></a>Mapowanie użycia x509 do Key Vault

Poniższa tabela przedstawia mapowanie zasad użycia klucza x509 do efektywnych operacji kluczowych klucza utworzonego w ramach tworzenia certyfikatu Key Vault.

|**Flagi użycia klucza x509**|**Key Vault kluczowe operacje**|**Zachowanie domyślne**|
|----------|--------|--------|
|DataEncipherment|szyfrowanie, odszyfrowywanie| Nie dotyczy |
|DecipherOnly|zawartości| Nie dotyczy  |
|Bity digitalSignature|Podpisz, zweryfikuj| Key Vault domyślne bez określenia użycia podczas tworzenia certyfikatu | 
|EncipherOnly|encrypt| Nie dotyczy |
|KeyCertSign|Podpisz, zweryfikuj|Nie dotyczy|
|KeyEncipherment|wrapKey, unwrapKey| Key Vault domyślne bez określenia użycia podczas tworzenia certyfikatu | 
|Niemożność wyparcia|Podpisz, zweryfikuj| Nie dotyczy |
|crlsign bit|Podpisz, zweryfikuj| Nie dotyczy |

## <a name="certificate-issuer"></a>Wystawca certyfikatu

Obiekt certyfikatu Key Vault zawiera konfigurację służącą do komunikowania się z wybranym dostawcą wystawcy certyfikatu w celu uporządkowania certyfikatów x509.  

-   Key Vault partnerów z następującymi dostawcami wystawcy certyfikatów dla certyfikatów TLS/SSL

|**Nazwa dostawcy**|**Lokalizacje**|
|----------|--------|
|DigiCert|Obsługiwane we wszystkich lokalizacjach usługi magazynu kluczy w chmurze publicznej i Azure Government|
|GlobalSign|Obsługiwane we wszystkich lokalizacjach usługi magazynu kluczy w chmurze publicznej i Azure Government|

Przed utworzeniem wystawcy certyfikatu w Key Vault należy pomyślnie wykonać następujące czynności wstępne 1 i 2.  

1. Dołączanie do dostawców urzędu certyfikacji  

    -   Administrator organizacji musi być na pokładzie firmy (np. Firma Contoso) z co najmniej jednym dostawcą urzędu certyfikacji.  

2. Administrator tworzy poświadczenia żądającego w celu Key Vault rejestracji (i odnowienia) certyfikatów TLS/SSL  

    -   Zapewnia konfigurację, która ma zostać użyta do utworzenia obiektu wystawcy dostawcy w magazynie kluczy  

Aby uzyskać więcej informacji na temat tworzenia obiektów wystawcy z poziomu portalu certyfikatów, zobacz [blog Key Vault Certificates](https://aka.ms/kvcertsblog)  

Key Vault umożliwia tworzenie wielu obiektów wystawcy z inną konfiguracją dostawcy wystawcy. Po utworzeniu obiektu Issuer jego nazwy można odwoływać się w jednej lub wielu zasadach certyfikatów. Odwołujący się do obiektu wystawcy instruuje Key Vault, aby użyć konfiguracji określonej w obiekcie Issuer podczas żądania certyfikatu x509 od dostawcy urzędu certyfikacji podczas tworzenia i odnawiania certyfikatu.  

Obiekty wystawcy są tworzone w magazynie i mogą być używane tylko z certyfikatami KV w tym samym magazynie.  

## <a name="certificate-contacts"></a>Kontakty certyfikatów

Kontakty certyfikatów zawierają informacje kontaktowe do wysyłania powiadomień wyzwalanych przez zdarzenia okresu istnienia certyfikatu. Informacje o kontaktach są współużytkowane przez wszystkie certyfikaty w magazynie kluczy. Powiadomienie zostanie wysłane do wszystkich określonych kontaktów dla zdarzenia dowolnego certyfikatu w magazynie kluczy.  

Jeśli zasada certyfikatu jest ustawiona na automatyczne odnawianie, zostanie wysłane powiadomienie dotyczące następujących zdarzeń.  

- Przed odnowieniem certyfikatu
- Po odnowieniu certyfikatu, informując o tym, czy certyfikat został pomyślnie odnowiony, czy wystąpił błąd, wymagając ręcznego odnowienia certyfikatu.  

  Gdy zasady certyfikatu ustawione do ręcznego odnawiania (tylko wiadomości e-mail), po chwili odnowienia certyfikatu zostanie wysłane powiadomienie.  

## <a name="certificate-access-control"></a>Access Control certyfikatów

 Kontrola dostępu do certyfikatów jest zarządzana przez usługę Key Vault i jest dostarczana przez magazyn kluczy, który zawiera te certyfikaty. Zasady kontroli dostępu dla certyfikatów różnią się od zasad kontroli dostępu dla kluczy i wpisów tajnych w tym samym Key Vault. Użytkownicy mogą utworzyć jeden lub więcej magazynów w celu przechowywania certyfikatów, aby zachować scenariusz odpowiedniej segmentacji i zarządzania certyfikatami.  

 Następujące uprawnienia mogą być używane dla każdego podmiotu zabezpieczeń w pozycji kontrola dostępu do kluczy tajnych w magazynie kluczy i ściśle odzwierciedlają operacje dozwolone na obiekcie tajnym:  

- Uprawnienia do operacji zarządzania certyfikatami
  - *Pobierz*: Pobierz bieżącą wersję certyfikatu lub dowolną wersję certyfikatu 
  - *Lista*: lista bieżących certyfikatów lub wersji certyfikatu  
  - *Aktualizacja*: aktualizowanie certyfikatu
  - *Tworzenie*: tworzenie certyfikatu Key Vault
  - *Import*: Importowanie materiału certyfikatu do certyfikatu Key Vault
  - *usuwanie*: Usuwanie certyfikatu, jego zasad i wszystkich wersji  
  - *odzyskiwanie*: Odzyskiwanie usuniętego certyfikatu
  - *kopia zapasowa*: wykonywanie kopii zapasowej certyfikatu w magazynie kluczy
  - *przywracanie*: Przywracanie certyfikatu z kopią zapasową do magazynu kluczy
  - *managecontacts*: Zarządzaj kontaktami z certyfikatem Key Vault  
  - *manageissuers*: Zarządzanie Key Vault urzędów certyfikacji/wystawców
  - *getemitencis*: pobieranie urzędów certyfikacji/wystawców certyfikatów
  - *listissuers*: Wyświetl listę urzędów certyfikacji/wystawców certyfikatów  
  - *setemitencis*: Tworzenie lub aktualizowanie urzędów/wystawców certyfikatu Key Vault  
  - *deleteissuers*: Usuwanie urzędów certyfikacji i wystawców certyfikatów Key Vault  
 
- Uprawnienia dla operacji uprzywilejowanych
  - *przeczyszczanie*: przeczyszczanie (trwałe usuwanie) usuniętego certyfikatu

Aby uzyskać więcej informacji, zobacz [operacje na certyfikatach w dokumentacji interfejsu API REST Key Vault](/rest/api/keyvault). Aby uzyskać informacje dotyczące ustanawiania uprawnień, zobacz temat [magazyny — Tworzenie lub aktualizowanie](/rest/api/keyvault/vaults/createorupdate) i [magazyny — zasady dostępu aktualizacji](/rest/api/keyvault/vaults/updateaccesspolicy).

## <a name="next-steps"></a>Następne kroki

- [Informacje o usłudze Key Vault](../general/overview.md)
- [Informacje o kluczach, wpisach tajnych i certyfikatach](../general/about-keys-secrets-certificates.md)
- [Informacje o kluczach](../keys/about-keys.md)
- [Informacje o wpisach tajnych](../secrets/about-secrets.md)
- [Uwierzytelnianie, żądania i odpowiedzi](../general/authentication-requests-and-responses.md)
- [Przewodnik dewelopera usługi Key Vault](../general/developers-guide.md)
