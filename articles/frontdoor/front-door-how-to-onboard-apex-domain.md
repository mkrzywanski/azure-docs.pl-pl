---
title: Dołączanie domeny głównej lub wierzchołka do istniejących przednich drzwi Azure Portal
description: Dowiedz się, jak dołączyć domenę główną lub Apex do istniejących przednich drzwi przy użyciu Azure Portal.
services: front-door
author: sharad4u
ms.service: frontdoor
ms.topic: how-to
ms.date: 5/21/2019
ms.author: sharadag
ms.openlocfilehash: d8f08f7cde54aaf705872c8c45bc18eb4a27df77
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "84743596"
---
# <a name="onboard-a-root-or-apex-domain-on-your-front-door"></a>Dołączanie domeny głównej lub domeny wierzchołkowej do usługi Front Door
Drzwi frontonu platformy Azure używają rekordów CNAME do weryfikowania własności domeny do dołączania domen niestandardowych. Ponadto drzwi tylne nie ujawniają adresu IP frontonu skojarzonego z Twoim profilem drzwi i dlatego nie można zmapować domeny Apex na adres IP, jeśli zamiarem jest dołączenie go do zewnętrznych drzwi platformy Azure.

Protokół DNS uniemożliwia przypisanie rekordów CNAME w wierzchołku strefy. Na przykład jeśli Twoja domena to `contoso.com` ; można utworzyć rekordy CNAME dla `somelabel.contoso.com` , ale nie można utworzyć rekordu CNAME dla `contoso.com` samego siebie. To ograniczenie powoduje problem dla właścicieli aplikacji, którzy mają aplikacje o zrównoważonym obciążeniu za drzwi platformy Azure. Ze względu na to, że użycie profilu front-drzwi wymaga utworzenia rekordu CNAME, nie jest możliwe wskazanie w profilu przednim drzwi ze wierzchołka strefy.

Ten problem jest rozwiązywany przy użyciu rekordów aliasów na Azure DNS. W przeciwieństwie do rekordów CNAME, rekordy aliasów są tworzone w wierzchołku strefy, a właściciele aplikacji mogą jej używać do wskazywania rekordu wierzchołka strefy w profilu przednim, który ma publiczne punkty końcowe. Właściciele aplikacji wskazują ten sam profil przedni, który jest używany przez dowolną inną domenę w ramach strefy DNS. Na przykład, `contoso.com` i `www.contoso.com` może wskazywać na ten sam profil przedni drzwi. 

Mapowanie wierzchołka lub domeny katalogu głównego na profil dla drzwi przednich zasadniczo wymaga spłaszczenia CNAME lub kartach DNS, który jest mechanizmem, w którym dostawca DNS rekursywnie rozpoznaje wpis CNAME do momentu trafienia adresu IP. Ta funkcja jest obsługiwana przez Azure DNS dla punktów końcowych z przodu. 

> [!NOTE]
> Istnieją również inni dostawcy DNS, którzy obsługują spłaszczanie CNAME lub kartach systemu DNS, ale przede wszystkim klienci na platformie Azure zalecają korzystanie z Azure DNS klientom do hostowania swoich domen.

Możesz użyć Azure Portal do dołączenia domeny wierzchołka na swoich drzwiach i włączenia protokołu HTTPS na ten temat, kojarząc go z certyfikatem dla zakończenia protokołu TLS. Domeny Apex są również nazywane domenami głównymi lub wykorzystanymi.

W tym artykule omówiono sposób wykonywania następujących zadań:

> [!div class="checklist"]
> * Tworzenie rekordu aliasu, który wskazuje na profil przedni
> * Dodaj domenę główną do drzwi przednich
> * Konfigurowanie protokołu HTTPS w domenie głównej

> [!NOTE]
> Ten samouczek wymaga już utworzenia profilu frontonu drzwi. Zapoznaj się z innymi samouczkami, takimi jak [Szybki Start: Tworzenie drzwi do przodu](./quickstart-create-front-door.md) lub [Tworzenie czołowych drzwi z przekierowaniami http do https](./front-door-how-to-redirect-https.md) , aby rozpocząć pracę.

## <a name="create-an-alias-record-for-zone-apex"></a>Utwórz rekord aliasu dla wierzchołka strefy

1. Otwórz **Azure DNS** konfigurację dla domeny, która ma zostać dołączona.
2. Utwórz lub Edytuj rekord wierzchołka strefy.
3. Wybierz **Typ** _rekordu jako rekord, a_ następnie wybierz pozycję _tak_ dla **zestawu rekordów aliasu**. **Typ aliasu** powinien być ustawiony na _zasób platformy Azure_.
4. Wybierz subskrypcję platformy Azure, w której znajduje się profil z drzwiami wstępnymi, a następnie wybierz zasób z drzwiami z listy rozwijanej **zasobów platformy Azure** .
5. Kliknij przycisk **OK** , aby przesłać zmiany.

    ![Rekord aliasu dla wierzchołka strefy](./media/front-door-apex-domain/front-door-apex-alias-record.png)

6. Powyższy krok spowoduje utworzenie rekordu wierzchołka strefy wskazującego na zasób z Drzwiem, a także mapowania rekordu CNAME "afdverify" (przykład- `afdverify.contosonews.com` ), do `afdverify.<name>.azurefd.net` którego będzie używany do dołączania domeny w profilu przednim drzwi.

## <a name="onboard-the-custom-domain-on-your-front-door"></a>Dołącz domenę niestandardową do swoich drzwi

1. Na karcie Projektant drzwi przednich kliknij ikonę "+" w sekcji hosty frontonu, aby dodać nową domenę niestandardową.
2. Wprowadź nazwę domeny głównej lub Apex w polu Nazwa hosta niestandardowego, na przykład `contosonews.com` .
3. Po sprawdzeniu poprawności mapowania CNAME z domeny do swoich pierwszych drzwi kliknij przycisk **Dodaj** , aby dodać domenę niestandardową.
4. Kliknij przycisk **Zapisz** , aby przesłać zmiany.

![Menu domen niestandardowych](./media/front-door-apex-domain/front-door-onboard-apex-domain.png)

## <a name="enable-https-on-your-custom-domain"></a>Włączanie protokołu HTTPS w domenie niestandardowej

1. Kliknij domenę niestandardową, która została dodana, i poniżej sekcji adres **https domeny niestandardowej**Zmień stan na **włączone**.
2. Wybierz **Typ zarządzania certyfikatami** , aby _użyć swojego certyfikatu_.

> [!WARNING]
> Typ zarządzania certyfikatami z przodu nie jest obecnie obsługiwany dla wierzchołków lub domen głównych. Jedyną opcją dostępną w przypadku włączania protokołu HTTPS na wierzchołku lub domenie głównej dla drzwi zewnętrznych jest użycie własnego niestandardowego certyfikatu TLS/SSL hostowanego na Azure Key Vault.

3. Przed przejściem do następnego kroku upewnij się, że skonfigurowano odpowiednie uprawnienia do czołowych drzwi w celu uzyskania dostępu do magazynu kluczy, który został wskazany w interfejsie użytkownika.
4. Wybierz **konto Key Vault** z bieżącej subskrypcji, a następnie wybierz odpowiednią wersję **klucza tajnego** i **wpisu tajnego** w celu zamapowania na odpowiedni certyfikat.
5. Kliknij przycisk **Aktualizuj** , aby zapisać zaznaczenie, a następnie kliknij przycisk **Zapisz**.
6. Kliknij przycisk **Odśwież** po kilku minutach, a następnie ponownie kliknij domenę niestandardową, aby zobaczyć postęp aprowizacji certyfikatów. 

> [!WARNING]
> Upewnij się, że utworzono odpowiednie reguły routingu dla domeny Apex lub dodano domenę do istniejących reguł routingu.

## <a name="next-steps"></a>Następne kroki

- Dowiedz się, jak [utworzyć usługę Front Door](quickstart-create-front-door.md).
- Dowiedz się, [jak działa usługa Front Door](front-door-routing-architecture.md).