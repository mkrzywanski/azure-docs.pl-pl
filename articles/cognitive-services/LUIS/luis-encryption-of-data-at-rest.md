---
title: Language Understanding szyfrowanie usługi danych w spoczynku
titleSuffix: Azure Cognitive Services
description: Language Understanding szyfrowanie usługi danych magazynowanych.
author: erindormier
manager: venkyv
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 03/13/2020
ms.author: egeaney
ms.openlocfilehash: 4fc816c3894120a5d1b356d91ebebbc56f21b530
ms.sourcegitcommit: ff19f4ecaff33a414c0fa2d4c92542d6e91332f8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/18/2020
ms.locfileid: "85052697"
---
# <a name="language-understanding-service-encryption-of-data-at-rest"></a>Language Understanding szyfrowanie usługi danych w spoczynku

Usługa Language Understanding automatycznie szyfruje dane, gdy zostaną utrwalone w chmurze. Szyfrowanie usługi Language Understanding chroni dane i pomaga sprostać wymaganiom bezpieczeństwa i zgodności w organizacji.

## <a name="about-cognitive-services-encryption"></a>Informacje o szyfrowaniu Cognitive Services

Dane są szyfrowane i odszyfrowywane przy użyciu zgodności ze standardem [FIPS 140-2](https://en.wikipedia.org/wiki/FIPS_140-2) [256-bitowego szyfrowania AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) . Szyfrowanie i odszyfrowywanie są przezroczyste, co oznacza, że szyfrowanie i dostęp są zarządzane przez Ciebie. Dane są domyślnie bezpieczne i nie trzeba modyfikować kodu ani aplikacji, aby korzystać z szyfrowania.

## <a name="about-encryption-key-management"></a>Informacje o zarządzaniu kluczami szyfrowania

Domyślnie Twoja subskrypcja używa kluczy szyfrowania zarządzanych przez firmę Microsoft. Istnieje również możliwość zarządzania subskrypcją przy użyciu własnych kluczy. Klucze zarządzane przez klienta (CMK) zapewniają większą elastyczność tworzenia, obracania, wyłączania i odwoływania kontroli dostępu. Możesz również przeprowadzać inspekcję kluczy szyfrowania używanych do ochrony danych.

## <a name="customer-managed-keys-with-azure-key-vault"></a>Klucze zarządzane przez klienta za pomocą usługi Azure Key Vault

Istnieje również możliwość zarządzania subskrypcją przy użyciu własnych kluczy. Klucze zarządzane przez klienta (CMK), znane także jako dające własny klucz (BYOK), zapewniają większą elastyczność tworzenia, obracania, wyłączania i odwoływania kontroli dostępu. Możesz również przeprowadzać inspekcję kluczy szyfrowania używanych do ochrony danych.

Aby przechowywać klucze zarządzane przez klienta, należy użyć Azure Key Vault. Możesz utworzyć własne klucze i zapisać je w magazynie kluczy lub użyć Azure Key Vault interfejsów API do wygenerowania kluczy. Zasób Cognitive Services i Magazyn kluczy muszą znajdować się w tym samym regionie i w tej samej dzierżawie Azure Active Directory (Azure AD), ale mogą znajdować się w różnych subskrypcjach. Aby uzyskać więcej informacji na temat Azure Key Vault, zobacz [co to jest Azure Key Vault?](https://docs.microsoft.com/azure/key-vault/key-vault-overview).

### <a name="customer-managed-keys-for-language-understanding"></a>Klucze zarządzane przez klienta dla Language Understanding

Aby zażądać możliwości korzystania z kluczy zarządzanych przez klienta, Wypełnij i prześlij [formularz żądania klucza zarządzanego przez klienta usługi Luis](https://aka.ms/cogsvc-cmk). Potrwa około 3-5 dni roboczych, aby poznać stan Twojego żądania. W zależności od popytu można umieścić w kolejce i zatwierdzić, że jest ona dostępna. Po zatwierdzeniu do korzystania z CMK z LUIS należy utworzyć nowy zasób Language Understanding z Azure Portal i wybrać E0 jako warstwę cenową. Nowa jednostka SKU będzie działać tak samo, jak jednostka SKU F0, która jest już dostępna z wyjątkiem CMK. Użytkownicy nie będą mogli przeprowadzić uaktualnienia z F0 do nowej jednostki SKU E0.

![Obraz subskrypcji LUIS](../media/cognitive-services-encryption/luis-subscription.png)

### <a name="regional-availability"></a>Dostępność regionalna

Klucze zarządzane przez klienta są dostępne we wszystkich [regionach tworzenia](luis-reference-regions.md). 

### <a name="limitations"></a>Ograniczenia

Istnieją pewne ograniczenia dotyczące korzystania z warstwy E0 z istniejącymi/utworzonymi wcześniej aplikacjami:

* Migracja do zasobu E0 zostanie zablokowana. Użytkownicy będą mogli migrować tylko swoje aplikacje do zasobów F0. Po przeprowadzeniu migracji istniejącego zasobu do F0 można utworzyć nowy zasób w warstwie E0. Więcej informacji na temat [migracji znajdziesz tutaj](https://docs.microsoft.com/azure/cognitive-services/luis/luis-migration-authoring).  
* Przeniesienie aplikacji do lub z zasobu E0 zostanie zablokowane. Obejście tego ograniczenia polega na wyeksportowaniu istniejącej aplikacji i zaimportowaniu jej jako zasobu E0.
* Funkcja sprawdzania pisowni Bing nie jest obsługiwana.
* Rejestrowanie ruchu użytkowników końcowych jest wyłączone, jeśli aplikacja jest E0.
* Funkcja napełnianiu mowy z usługi Azure bot nie jest obsługiwana w przypadku aplikacji w warstwie E0. Ta funkcja jest dostępna za pośrednictwem Azure Bot Service, który nie obsługuje CMK.
* Funkcja napełnianiu mowy w portalu wymaga platformy Azure Blob Storage. Aby uzyskać więcej informacji, zobacz temat [przenoszenie własnego magazynu](../Speech-Service/speech-encryption-of-data-at-rest.md#bring-your-own-storage-byos-for-customization-and-logging).

### <a name="enable-customer-managed-keys"></a>Włącz klucze zarządzane przez klienta

Nowy zasób Cognitive Services jest zawsze szyfrowany przy użyciu kluczy zarządzanych przez firmę Microsoft. W momencie utworzenia zasobu nie można włączyć kluczy zarządzanych przez klienta. Klucze zarządzane przez klienta są przechowywane w Azure Key Vault, a Magazyn kluczy musi być zainicjowany przy użyciu zasad dostępu, które przyznają kluczowe uprawnienia do zarządzanej tożsamości skojarzonej z zasobem Cognitive Services. Tożsamość zarządzana jest dostępna tylko po utworzeniu zasobu za pomocą warstwy cenowej dla CMK.

Aby dowiedzieć się, jak używać kluczy zarządzanych przez klienta do Azure Key Vault szyfrowania Cognitive Services, zobacz:

- [Skonfiguruj klucze zarządzane przez klienta przy użyciu Key Vault na potrzeby szyfrowania Cognitive Services z Azure Portal](../Encryption/cognitive-services-encryption-keys-portal.md)

Włączenie kluczy zarządzanych przez klienta spowoduje również włączenie tożsamości zarządzanej przypisanej do systemu, funkcji usługi Azure AD. Gdy jest włączona tożsamość zarządzana przypisana przez system, ten zasób zostanie zarejestrowany w Azure Active Directory. Po zarejestrowaniu tożsamość zarządzana będzie mieć dostęp do Key Vault wybranej podczas instalacji klucza zarządzanego przez klienta. Możesz dowiedzieć się więcej o [tożsamościach zarządzanych](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview).

> [!IMPORTANT]
> Jeśli wyłączysz tożsamości zarządzane przypisane przez system, dostęp do magazynu kluczy zostanie usunięty, a wszystkie dane zaszyfrowane za pomocą kluczy klienta nie będą już dostępne. Wszystkie funkcje zależne od tych danych przestaną działać.

> [!IMPORTANT]
> Tożsamości zarządzane nie obsługują obecnie scenariuszy między katalogami. W przypadku konfigurowania kluczy zarządzanych przez klienta w Azure Portal, zarządzana tożsamość zostanie automatycznie przypisana w obszarze okładek. Jeśli później przeniesiesz subskrypcję, grupę zasobów lub zasób z jednego katalogu usługi Azure AD do innego, zarządzana tożsamość skojarzona z zasobem nie zostanie przetransferowana do nowej dzierżawy, więc klucze zarządzane przez klienta mogą przestać działać. Aby uzyskać więcej informacji, zobacz **transfer subskrypcji między katalogami usługi Azure AD** w [często zadawanych pytaniach i znanych problemach z tożsamościami zarządzanymi dla zasobów platformy Azure](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/known-issues#transferring-a-subscription-between-azure-ad-directories).  

### <a name="store-customer-managed-keys-in-azure-key-vault"></a>Przechowuj klucze zarządzane przez klienta w Azure Key Vault

Aby włączyć klucze zarządzane przez klienta, należy użyć Azure Key Vault do przechowywania kluczy. Należy włączyć zarówno właściwości **nietrwałego usuwania** , jak i **nie przeczyszczania** w magazynie kluczy.

Szyfrowanie Cognitive Services obsługuje tylko klucze RSA o rozmiarze 2048. Aby uzyskać więcej informacji na temat kluczy, zobacz **Key Vault klucze** w temacie [informacje Azure Key Vault klucze, wpisy tajne i certyfikaty](https://docs.microsoft.com/azure/key-vault/about-keys-secrets-and-certificates#key-vault-keys).

### <a name="rotate-customer-managed-keys"></a>Obróć klucze zarządzane przez klienta

Klucz zarządzany przez klienta można obrócić w Azure Key Vault zgodnie z zasadami zgodności. Gdy klucz jest obrócony, należy zaktualizować zasób Cognitive Services, aby używał nowego identyfikatora URI klucza. Aby dowiedzieć się, jak zaktualizować zasób w celu używania nowej wersji klucza w Azure Portal, zapoznaj się z sekcją **Aktualizacja wersji klucza** w artykule [Konfigurowanie kluczy zarządzanych przez klienta dla Cognitive Services przy użyciu Azure Portal](../Encryption/cognitive-services-encryption-keys-portal.md).

Obracanie klucza nie wyzwala ponownego szyfrowania danych w zasobie. Od użytkownika nie są wymagane żadne dalsze działania.

### <a name="revoke-access-to-customer-managed-keys"></a>Odwołaj dostęp do kluczy zarządzanych przez klienta

Aby odwołać dostęp do kluczy zarządzanych przez klienta, należy użyć programu PowerShell lub interfejsu wiersza polecenia platformy Azure. Aby uzyskać więcej informacji, zobacz [Azure Key Vault PowerShell](https://docs.microsoft.com/powershell/module/az.keyvault//) lub [interfejs wiersza polecenia Azure Key Vault](https://docs.microsoft.com/cli/azure/keyvault). Odwoływanie dostępu skutecznie blokuje dostęp do wszystkich danych w zasobie Cognitive Services, ponieważ klucz szyfrowania jest niedostępny przez Cognitive Services.

## <a name="next-steps"></a>Następne kroki

* [Formularz żądania klucza zarządzanego przez klienta usługi LUIS](https://aka.ms/cogsvc-cmk)
* [Dowiedz się więcej o Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-overview)
