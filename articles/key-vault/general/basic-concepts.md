---
title: Co to jest usługa Azure Key Vault? | Microsoft Docs
description: Dowiedz się, w jaki sposób Azure Key Vault zabezpiecza klucze kryptograficzne i wpisy tajne używane przez aplikacje i usługi w chmurze.
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: conceptual
ms.date: 01/18/2019
ms.author: mbaldwin
ms.openlocfilehash: 7c64835ced558727718690138c3e7a7666cf0809
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "84167302"
---
# <a name="azure-key-vault-basic-concepts"></a>Azure Key Vault podstawowe pojęcia

Usługa Azure Key Vault jest narzędziem do bezpiecznego przechowywania wpisów tajnych i uzyskiwania do nich dostępu. Wpis tajny to dowolny zasób, do którego dostęp powinien być ściśle kontrolowany. Przykładowe wpisy tajne to klucze interfejsu API, hasła i certyfikaty. Magazyn jest logiczną grupą kluczy tajnych.

Oto inne ważne terminy:

- **Dzierżawa** — dzierżawa to organizacja, która jest właścicielem konkretnego wystąpienia usług firmy Microsoft w chmurze i zarządza nim. Jest to najczęściej używane do odwoływania się do zestawu usług platformy Azure i pakietu Office 365 dla organizacji.

- **Właściciel magazynu** — właściciel magazynu może utworzyć magazyn kluczy, ma do niego pełny dostęp i kontrolę nad nim. Właściciel magazynu może też skonfigurować inspekcję, aby rejestrować, kto uzyskuje dostęp do wpisów tajnych i kluczy. Administratorzy mogą kontrolować cykl życia klucza. Mogą oni wdrażać nowe wersje klucza, tworzyć jego kopie zapasowe i wykonywać powiązane zadania.

- **Użytkownik magazynu** — użytkownik magazynu może wykonywać akcje na zasobach wewnątrz magazynu kluczy, jeśli właściciel magazynu udzieli mu dostępu. Dostępne akcje zależą od przyznanych uprawnień.

- **Zasób** — zasób to dostępny za pośrednictwem platformy Azure element, którym można zarządzać. Typowe przykłady to maszyna wirtualna, konto magazynu, aplikacja sieci Web, baza danych i Sieć wirtualna. Istnieje wiele innych.

- **Grupa zasobów** — grupa zasobów to kontener, który zawiera powiązane zasoby dla rozwiązania platformy Azure. Grupa zasobów może zawierać wszystkie zasoby dla rozwiązania lub tylko te zasoby, które mają być zarządzane jako grupa. Użytkownik decyduje o sposobie przydziału zasobów do grup zasobów pod kątem tego, co jest najbardziej odpowiednie dla danej organizacji.

- Nazwa **główna usługi**: jednostka usługi platformy Azure to tożsamość zabezpieczeń, którą aplikacje, usługi i narzędzia automatyzacji zostały utworzone przez użytkownika w celu uzyskania dostępu do określonych zasobów platformy Azure. Należy je traktować jako "tożsamość użytkownika" (nazwę użytkownika i hasło lub certyfikat) z określoną rolą i ściśle kontrolowanymi uprawnieniami. W odróżnieniu od ogólnej tożsamości użytkownika, jednostka usługi powinna wykonywać tylko określone czynności. Zwiększa zabezpieczenia, jeśli przyznasz mu tylko minimalny poziom uprawnień potrzebny do wykonywania zadań zarządzania.

- [Azure Active Directory (Azure AD)](../../active-directory/active-directory-whatis.md): Azure AD to usługa Active Directory dla dzierżawy. Każdy katalog ma co najmniej jedną domenę. Katalog może mieć wiele skojarzonych subskrypcji, ale tylko jedną dzierżawę.

- **Identyfikator dzierżawy Azure** — identyfikator dzierżawy to unikatowy sposób identyfikacji wystąpienia usługi Azure AD w ramach subskrypcji platformy Azure.

- **Zarządzane tożsamości**: Azure Key Vault zapewnia sposób bezpiecznego przechowywania poświadczeń i innych kluczy i wpisów tajnych, ale kod wymaga uwierzytelnienia, aby Key Vault je pobrać. Korzystanie z tożsamości zarządzanej ułatwia rozwiązanie tego problemu, oferując usługi platformy Azure, które automatycznie zarządza tożsamością w usłudze Azure AD. Za pomocą tej tożsamości można uwierzytelnić się w usłudze Key Vault lub dowolnej innej usłudze obsługującej uwierzytelnianie usługi Azure AD bez konieczności przechowywania poświadczeń w kodzie. Aby uzyskać więcej informacji, zobacz poniższy obraz oraz [Omówienie zarządzanych tożsamości dla zasobów platformy Azure](../../active-directory/managed-identities-azure-resources/overview.md).

    ![Diagram przedstawiający sposób działania zarządzanych tożsamości dla zasobów platformy Azure](../media/key-vault-whatis/msi.png)

## <a name="authentication"></a>Authentication
Aby wykonać wszelkie operacje z Key Vault, musisz najpierw przeprowadzić do nich uwierzytelnienie. Istnieją trzy sposoby uwierzytelniania do Key Vault:

- [Zarządzane tożsamości dla zasobów platformy Azure](../../active-directory/managed-identities-azure-resources/overview.md): podczas wdrażania aplikacji na maszynie wirtualnej na platformie Azure można przypisać tożsamość do maszyny wirtualnej, która ma dostęp do Key Vault. Możesz również przypisywać tożsamości do [innych zasobów platformy Azure](../../active-directory/managed-identities-azure-resources/overview.md). Zaletą tego podejścia jest to, że aplikacja lub usługa nie zarządza rotacją pierwszego klucza tajnego. Platforma Azure automatycznie obraca tożsamość. Zalecamy to podejście jako najlepsze rozwiązanie. 
- Nazwa **główna usługi i certyfikat**: można użyć jednostki usługi i skojarzonego certyfikatu, który ma dostęp do Key Vault. Nie zalecamy tego podejścia, ponieważ właściciel aplikacji lub Deweloper musi obrócić certyfikat.
- Nazwa **główna usługi i klucz tajny**: Chociaż do uwierzytelniania w Key Vault można użyć jednostki usługi i klucza tajnego, nie zalecamy tego. Trudno jest automatycznie obrócić wpis tajny Bootstrap używany do uwierzytelniania w Key Vault.


## <a name="key-vault-roles"></a>Role usługi Key Vault

Użyj poniższej tabeli, aby lepiej zrozumieć, w jaki sposób usługa Key Vault może pomóc deweloperom i administratorom zabezpieczeń w spełnianiu ich potrzeb.

| Rola | Opis problemu | Rozwiązanie usługi Azure Key Vault |
| --- | --- | --- |
| Deweloper aplikacji platformy Azure |"Chcę napisać aplikację dla platformy Azure, która używa kluczy do podpisywania i szyfrowania. Ale chcę, aby te klucze były zewnętrzne od mojej aplikacji, aby rozwiązanie było odpowiednie dla aplikacji, która jest geograficznie dystrybuowana. <br/><br/>Chcę także, aby klucze i wpisy tajne były chronione, ale bez konieczności samodzielnego pisania kodu. Ponadto chcę, aby te klucze i wpisy tajne były łatwe do użycia w aplikacjach z optymalną wydajnością ". |√ Klucze są przechowywane w magazynie i w razie potrzeby wywoływane przez identyfikator URI.<br/><br/> √ Klucze są chronione przez platformę Azure przy użyciu branżowych standardów dotyczących algorytmów, długości klucza i sprzętowych modułów zabezpieczeń (HSM, hardware security module).<br/><br/> √ Klucze są przetwarzane w modułach HSM, które znajdują się w tych samych centrach danych platformy Azure co aplikacje. Ta metoda zapewnia większą niezawodność i mniejsze opóźnienia niż klucze przechowywane w osobnej lokalizacji, na przykład lokalnie. |
| Deweloper oprogramowania jako usługi (SaaS) |"Nie chcę odpowiedzialności ani potencjalnej odpowiedzialności za klucze dzierżawy klientów i ich wpisy tajne. <br/><br/>Chcę, aby klienci i zarządzali swoimi kluczami, aby móc skoncentrować się na tym, co robię najlepiej, która zapewnia podstawowe funkcje oprogramowania ". |√ Klienci mogą importować własne klucze do platformy Azure i zarządzać nimi. Gdy aplikacja SaaS musi wykonywać operacje kryptograficzne przy użyciu kluczy klientów, Key Vault wykonuje te operacje w imieniu aplikacji. Aplikacja nie widzi kluczy klientów. |
| Chief Security Officer (CSO) |"Chcę wiedzieć, że nasze aplikacje są zgodne z standardem FIPS 140-2 Level 2 sprzętowych modułów zabezpieczeń na potrzeby bezpiecznego zarządzania kluczami. <br/><br/>Chcę się upewnić, że moja organizacja kontroluje cykl życia klucza i monitoruje jego użycie. <br/><br/>Chociaż korzystamy z wielu usług i zasobów platformy Azure, chcę zarządzać kluczami z pojedynczej lokalizacji na platformie Azure ". |√ Moduły HSM są zweryfikowane w trybie FIPS 140-2 poziom 2.<br/><br/>√ Usługa Key Vault jest zaprojektowana w taki sposób, aby firma Microsoft nie miała wglądu w Twoje klucze ani nie mogła ich wyodrębnić.<br/><br/>√ Użycie klucza jest rejestrowane w czasie niemal rzeczywistym.<br/><br/>√ Magazyn zapewnia jeden interfejs, niezależnie od tego, jak wiele magazynów masz na platformie Azure, które regiony są przez nie obsługiwane oraz które aplikacje ich używają. |

Każdy posiadacz subskrypcji Azure może tworzyć magazyny kluczy i z nich korzystać. Chociaż Key Vault korzyści dla deweloperów i administratorów zabezpieczeń, można je wdrożyć i zarządzać przez administratora organizacji, który zarządza innymi usługami platformy Azure. Na przykład ten administrator może zalogować się przy użyciu subskrypcji platformy Azure, utworzyć magazyn dla organizacji, w której będą przechowywane klucze, a następnie być odpowiedzialny za zadania operacyjne podobne do tych:

- Tworzenie lub importowanie klucza lub klucza tajnego
- Odwoływanie lub usuwanie klucza lub klucza tajnego
- Zezwalanie użytkownikom lub aplikacjom na dostęp do magazynu kluczy, aby mogli zarządzać kluczami i kluczami tajnymi lub używać ich
- Konfigurowanie użycia klucza (na przykład rejestrowanie lub szyfrowanie)
- Monitorowanie użycia klucza

Umożliwia to administratorom identyfikatory URI do wywołania z aplikacji. Ten administrator zapewnia również informacje o rejestrowaniu użycia klucza dla administratora zabezpieczeń. 

![Przegląd sposobu działania Azure Key Vault][1]

Deweloperzy mogą również zarządzać kluczami bezpośrednio za pomocą interfejsów API. Aby uzyskać więcej informacji, zobacz artykuł [Przewodnik dewelopera usługi Key Vault](developers-guide.md).

## <a name="next-steps"></a>Następne kroki

Dowiedz się, jak [zabezpieczyć swój magazyn](secure-your-key-vault.md).

<!--Image references-->
[1]: ../media/key-vault-whatis/AzureKeyVault_overview.png
Usługa Azure Key Vault jest dostępna w większości regionów. Aby uzyskać więcej informacji, zobacz stronę [Cennik usługi Key Vault](https://azure.microsoft.com/pricing/details/key-vault/).
