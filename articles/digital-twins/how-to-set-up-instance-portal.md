---
title: Konfigurowanie wystąpienia i uwierzytelniania (Portal)
titleSuffix: Azure Digital Twins
description: Zobacz jak skonfigurować wystąpienie usługi Azure Digital bliźniaczych reprezentacji przy użyciu Azure Portal
author: baanders
ms.author: baanders
ms.date: 7/23/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 73b7171b89b26926992e95f77e376e7bb7731eff
ms.sourcegitcommit: 42107c62f721da8550621a4651b3ef6c68704cd3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/29/2020
ms.locfileid: "87408413"
---
# <a name="set-up-an-azure-digital-twins-instance-and-authentication-portal"></a>Konfigurowanie wystąpienia i uwierzytelniania usługi Azure Digital bliźniaczych reprezentacji (Portal)

[!INCLUDE [digital-twins-setup-selector.md](../../includes/digital-twins-setup-selector.md)]

W tym artykule opisano procedurę **konfigurowania nowego wystąpienia usługi Azure Digital bliźniaczych reprezentacji**, w tym tworzenia wystąpienia i konfigurowania uwierzytelniania. Po ukończeniu tego artykułu będzie dostępne wystąpienie usługi Azure Digital bliźniaczych reprezentacji gotowe do rozpoczęcia programowania.

W tej wersji tego artykułu przechodzą te kroki ręcznie, po jednym z nich przy użyciu Azure Portal. Witryna Azure Portal to ujednolicona, oparta na sieci Web konsola, która stanowi alternatywę dla narzędzi wiersza polecenia.
* Aby wykonać te kroki ręcznie przy użyciu interfejsu wiersza polecenia, zobacz wersja interfejsu wiersza polecenia w tym artykule: [*instrukcje: Konfigurowanie wystąpienia i uwierzytelniania (CLI)*](how-to-set-up-instance-cli.md).
* Aby przeprowadzić instalację zautomatyzowaną za pomocą przykładowego skryptu wdrożenia, zobacz wersję skryptu w tym artykule: [*instrukcje: Konfigurowanie wystąpienia i uwierzytelniania (skrypty)*](how-to-set-up-instance-scripted.md).

[!INCLUDE [digital-twins-setup-steps.md](../../includes/digital-twins-setup-steps.md)]
 
Następnie zaloguj się do [Azure Portal](https://ms.portal.azure.com/) przy użyciu swoich poświadczeń.

## <a name="prerequisites-permission-requirements"></a>Warunki wstępne: wymagania dotyczące uprawnień

Aby móc wykonać wszystkie kroki opisane w tym artykule, musisz być sklasyfikowany jako właściciel w ramach subskrypcji platformy Azure. 

Poziom uprawnień można sprawdzić na [stronie Subskrypcje](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) w Azure Portal (można użyć tego linku lub wyszukać *subskrypcje* na pasku wyszukiwania portalu). Poszukaj nazwy używanej subskrypcji i Wyświetl jej rolę w kolumnie *My role* . Jeśli jesteś właścicielem, ta wartość jest *właścicielem*:

:::image type="content" source="media/how-to-set-up-instance/portal/subscriptions-role.png" alt-text="Widok strony subskrypcji w Azure Portal, który pokazuje użytkownika jako właściciela" lightbox="media/how-to-set-up-instance/portal/subscriptions-role.png":::

Jeśli okaże się, że wartość to *współautor* lub coś innego niż *właściciel*, można wykonać jedną z następujących czynności:
* Skontaktuj się z właścicielem subskrypcji i poproś właściciela o wykonanie kroków opisanych w tym artykule w Twoim imieniu
* Skontaktuj się z właścicielem subskrypcji lub osobą z rolą administratora dostępu użytkowników w ramach subskrypcji i poproś o podniesienie uprawnień do właściciela subskrypcji, aby móc kontynuować pracę. Niezależnie od tego, czy jest to odpowiednie, zależy od organizacji i Twojej roli.

## <a name="create-the-azure-digital-twins-instance"></a>Tworzenie wystąpienia usługi Azure Digital bliźniaczych reprezentacji

W tej sekcji utworzysz **nowe wystąpienie usługi Azure Digital bliźniaczych reprezentacji** przy użyciu Azure Portal.

Po zalogowaniu się do [Azure Portal](https://ms.portal.azure.com/)Zacznij od wybrania pozycji _Utwórz zasób_ w menu strony głównej usług platformy Azure.

:::image type="content" source= "media/how-to-set-up-instance/portal/create-resource.png" alt-text="Wybieranie pozycji "Utwórz zasób" ze strony głównej Azure Portal":::

Wyszukaj pozycję *Azure Digital bliźniaczych reprezentacji* w polu wyszukiwania i wybierz usługę **Azure Digital bliźniaczych reprezentacji (wersja zapoznawcza)** z wyników. Wybierz przycisk _Utwórz_ , aby utworzyć nowe wystąpienie usługi.

:::image type="content" source= "media/how-to-set-up-instance/portal/create-azure-digital-twins.png" alt-text="Wybieranie pozycji "Utwórz" na stronie usługi Azure Digital bliźniaczych reprezentacji":::

Na poniższej stronie *Tworzenie zasobów* Wypełnij wartości podanych poniżej:
* **Subskrypcja**: subskrypcja platformy Azure, której używasz
  - **Grupa zasobów**: Grupa zasobów, w której ma zostać wdrożone wystąpienie. Jeśli nie masz jeszcze istniejącej grupy zasobów, możesz ją utworzyć, wybierając pozycję *Utwórz nowe* łącze i wprowadzając nazwę nowej grupy zasobów.
* **Location (lokalizacja**): bliźniaczych reprezentacji na potrzeby wdrożenia przy użyciu cyfrowego obszaru platformy Azure. Aby uzyskać więcej informacji na temat pomocy technicznej regionalnej, odwiedź stronę [*usługi platformy Azure dostępne według regionów (usługa Azure Digital bliźniaczych reprezentacji)*](https://azure.microsoft.com/global-infrastructure/services/?products=digital-twins).
* **Nazwa zasobu**: nazwa wystąpienia usługi Azure Digital bliźniaczych reprezentacji. Nazwa nowego wystąpienia musi być unikatowa w obrębie regionu subskrypcji (co oznacza, że jeśli subskrypcja ma inne wystąpienie usługi Azure Digital bliźniaczych reprezentacji w regionie, w którym już korzystasz z wybranej nazwy, zostanie wyświetlony monit o wybranie innej nazwy).

:::image type="content" source= "media/how-to-set-up-instance/portal/create-azure-digital-twins-2.png" alt-text="Wypełnianie opisanych wartości, aby utworzyć zasób usługi Azure Digital bliźniaczych reprezentacji":::

Po zakończeniu wybierz pozycję _Przegląd + Utwórz_. Spowoduje to przejście do strony podsumowania, na której można przejrzeć wprowadzone informacje o wystąpieniu i przycisk _Utwórz_. 

### <a name="verify-success"></a>Weryfikowanie sukcesu

Po wypchnięciu polecenia *Create*można wyświetlić stan wdrożenia wystąpienia w obszarze powiadomień platformy Azure na pasku ikon portalu. Powiadomienie będzie wskazywać, kiedy wdrożenie zakończyło się pomyślnie, i będzie można wybrać przycisk _Przejdź do zasobu_ , aby wyświetlić utworzone wystąpienie.

:::image type="content" source="media/how-to-set-up-instance/portal/notifications-deployment.png" alt-text="Widok powiadomień platformy Azure przedstawiający pomyślne wdrożenie i wyróżnienie przycisku "przejdź do zasobu"":::

Alternatywnie, jeśli wdrożenie nie powiedzie się, powiadomienie wskaże przyczynę. Zwróć uwagę na komunikat o błędzie i ponów próbę utworzenia wystąpienia.

>[!TIP]
>Po utworzeniu wystąpienia możesz powrócić do jego strony w dowolnym momencie, wyszukując nazwę wystąpienia na pasku wyszukiwania Azure Portal.

Na stronie *Przegląd* wystąpienia Zanotuj jego *nazwę*, *grupę zasobów*i *nazwę hosta*. Są to wszystkie ważne wartości, które mogą być potrzebne w przypadku kontynuowania pracy z wystąpieniem usługi Azure Digital bliźniaczych reprezentacji. Jeśli inni użytkownicy będą programowanie względem wystąpienia, należy udostępnić te wartości.

:::image type="content" source="media/how-to-set-up-instance/portal/instance-important-values.png" alt-text="Wyróżnianie ważnych wartości na stronie przeglądowej wystąpienia":::

Masz teraz już gotowe do użycia wystąpienie usługi Azure Digital bliźniaczych reprezentacji. Następnie uzyskasz odpowiednie uprawnienia użytkownika platformy Azure do zarządzania nim.

## <a name="set-up-user-access-permissions"></a>Konfigurowanie uprawnień dostępu użytkowników

[!INCLUDE [digital-twins-setup-role-assignment.md](../../includes/digital-twins-setup-role-assignment.md)]

Najpierw otwórz stronę wystąpienia usługi Azure Digital bliźniaczych reprezentacji w Azure Portal. Z menu wystąpienia wybierz pozycję *Kontrola dostępu (IAM)*. Wybierz przycisk *Dodaj* w obszarze *Dodawanie przypisania roli*.

:::image type="content" source="media/how-to-set-up-instance/portal/add-role-assignment-1.png" alt-text="Wybieranie, aby dodać przypisanie roli na stronie "kontrola dostępu (IAM)"":::

Na poniższej stronie *Dodawanie przypisania roli* Wypełnij wartości (musi zostać wykonane przez właściciela subskrypcji platformy Azure):
* **Rola**: wybierz pozycję *Azure Digital bliźniaczych reprezentacji Owner (wersja zapoznawcza)* z menu rozwijanego
* **Przypisz dostęp do**: Wybierz *użytkownika, grupę lub jednostkę usługi Azure AD* z menu rozwijanego
* **Wybierz**: Wyszukaj nazwę lub adres e-mail użytkownika do przypisania. Po wybraniu wyniku użytkownik zostanie wyświetlony w sekcji *wybrane elementy członkowskie* .

:::row:::
    :::column:::
        :::image type="content" source="media/how-to-set-up-instance/portal/add-role-assignment-2.png" alt-text="Wypełnianie pól na liście w oknie dialogowym Dodawanie przypisania roli":::
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

Po zakończeniu wprowadzania szczegółów naciśnij przycisk *Zapisz* .

### <a name="verify-success"></a>Weryfikowanie sukcesu

Można wyświetlić przypisaną rolę, która została skonfigurowana w obszarze *Kontrola dostępu (IAM) > przypisań ról*. Użytkownik powinien zostać wyświetlony na liście z rolą *właściciela Digital bliźniaczych reprezentacji (wersja zapoznawcza)*. 

:::image type="content" source="media/how-to-set-up-instance/portal/verify-role-assignment.png" alt-text="Widok przypisań roli dla wystąpienia usługi Azure Digital bliźniaczych reprezentacji w Azure Portal":::

Masz teraz już gotowe do użycia wystąpienie usługi Azure Digital bliźniaczych reprezentacji i masz przypisane uprawnienia do zarządzania nim. Następnie skonfigurujesz uprawnienia do aplikacji klienckiej w celu uzyskania do niej dostępu.

## <a name="set-up-access-permissions-for-client-applications"></a>Konfigurowanie uprawnień dostępu dla aplikacji klienckich

[!INCLUDE [digital-twins-setup-app-registration.md](../../includes/digital-twins-setup-app-registration.md)]

Zacznij od przechodzenia do [Azure Active Directory](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) w Azure Portal (możesz użyć tego linku lub znaleźć go na pasku wyszukiwania portalu). Wybierz *rejestracje aplikacji* z menu usługi, a następnie pozycję *+ Nowa rejestracja*.

:::image type="content" source="media/how-to-set-up-instance/portal/new-registration.png" alt-text="Wyświetl stronę usługi Azure AD w Azure Portal, podświetl opcję menu "Rejestracje aplikacji" i przycisk "+ Nowa rejestracja"":::

Na stronie *zarejestruj aplikację* w następującej kolejności wprowadź żądane wartości:
* **Nazwa**: Nazwa wyświetlana aplikacji usługi Azure AD, która ma zostać skojarzona z rejestracją
* **Obsługiwane typy kont**: Wybierz *tylko konta w tym katalogu organizacji (tylko katalog domyślny — pojedynczy dzierżawca)*
* **Identyfikator URI przekierowania**: *adres URL odpowiedzi aplikacji usługi Azure AD* dla aplikacji usługi Azure AD. Możesz użyć `http://localhost` .

Po zakończeniu kliknij przycisk *zarejestruj* .

:::image type="content" source="media/how-to-set-up-instance/portal/register-an-application.png" alt-text="Widok strony "Zarejestruj aplikację" przy użyciu opisanych wartości wypełnionych":::

Po zakończeniu konfigurowania rejestracji Portal przekieruje Cię do strony szczegółów.

### <a name="provide-azure-digital-twins-api-permission"></a>Udostępnianie cyfrowego interfejsu API usługi Azure Digital bliźniaczych reprezentacji

Następnie skonfiguruj rejestrację aplikacji utworzoną za pomocą uprawnień linii bazowej do interfejsów API Digital bliźniaczych reprezentacji platformy Azure.

Na stronie portalu rejestracji aplikacji wybierz pozycję *uprawnienia interfejsu API* z menu. Na następującej stronie Uprawnienia kliknij przycisk *+ Dodaj uprawnienie* .

:::image type="content" source="media/how-to-set-up-instance/portal/add-permission.png" alt-text="Wyświetl rejestrację aplikacji w Azure Portal, wyróżnij opcję menu "uprawnienia interfejsu API" i przycisk "+ Dodaj uprawnienie"":::

Na stronie *uprawnienia do interfejsu API żądania* przejdź do opcji *interfejsy API Moja organizacja używa* karty i Wyszukaj w *usłudze Azure Digital bliźniaczych reprezentacji*. Wybierz pozycję *Azure Digital bliźniaczych reprezentacji* z wyników wyszukiwania, aby kontynuować przypisywanie uprawnień do interfejsów API Digital bliźniaczych reprezentacji platformy Azure.

:::image type="content" source="media/how-to-set-up-instance/portal/request-api-permissions-1.png" alt-text="Widok wyniku wyszukiwania strony "uprawnienia interfejsu API żądania" z informacjami o usłudze Azure Digital bliźniaczych reprezentacji":::

Następnie wybierz uprawnienia, które mają zostać przyznane dla tych interfejsów API. Rozwiń uprawnienie **Odczyt (1)** , a następnie zaznacz pole wyboru *Odczytaj. Zapisz* , aby udzielić uprawnienia czytelnikowi rejestracji aplikacji i składnika zapisywania.

:::image type="content" source="media/how-to-set-up-instance/portal/request-api-permissions-2.png" alt-text="Widok strony "uprawnienia interfejsu API żądania" Wybieranie uprawnień "Odczyt. zapis" dla interfejsów API Digital bliźniaczych reprezentacji platformy Azure":::

Po zakończeniu kliknij przycisk *Dodaj uprawnienia* .

### <a name="verify-success"></a>Weryfikowanie sukcesu

Na stronie *uprawnienia interfejsu API* Sprawdź, czy istnieje już wpis do usługi Azure Digital bliźniaczych reprezentacji odzwierciedlający uprawnienia do odczytu/zapisu:

:::image type="content" source="media/how-to-set-up-instance/portal/verify-api-permissions.png" alt-text="Widok portalu uprawnień interfejsu API dla rejestracji aplikacji usługi Azure AD, pokazujący uprawnienie do odczytu/zapisu dla usługi Azure Digital bliźniaczych reprezentacji":::

Możesz również zweryfikować połączenie z usługą Azure Digital bliźniaczych reprezentacji w *manifest.js*rejestracji aplikacji, która została automatycznie zaktualizowana przy użyciu informacji o usłudze Azure Digital bliźniaczych reprezentacji po dodaniu uprawnień interfejsu API.

W tym celu wybierz pozycję *manifest* z menu, aby wyświetlić kod manifestu rejestracji aplikacji. Przewiń w dół okna kod i poszukaj tych pól w obszarze `requiredResourceAccess` . Wartości powinny być zgodne z poniższymi zrzutu ekranu:

:::image type="content" source="media/how-to-set-up-instance/portal/verify-manifest.png" alt-text="Widok portalu dla rejestracji aplikacji usługi Azure AD. Zagnieżdżone w "requiredResourceAccess" ma wartość "resourceAppId" elementu 0b07f429-9f4b-4714-9392-cc5e8e80c8b0, a wartość "resourceAccess > ID" elementu 4589bd03-58cb-4e6c-b17f-b580e39652f8":::

### <a name="collect-important-values"></a>Zbierz ważne wartości

Następnie na pasku menu wybierz pozycję *Przegląd* , aby wyświetlić szczegóły rejestracji aplikacji:

:::image type="content" source="media/how-to-set-up-instance/portal/app-important-values.png" alt-text="Widok portalu ważnych wartości rejestracji aplikacji":::

Zanotuj *Identyfikator* *aplikacji (klienta)* na stronie **użytkownika** . Te wartości będą później konieczne do [uwierzytelnienia aplikacji klienckiej względem interfejsów API Digital bliźniaczych reprezentacji platformy Azure](how-to-authenticate-client.md). Jeśli nie jesteś osobą, która będzie pisać kod dla takich aplikacji, należy udostępnić te wartości osobie, która będzie.

### <a name="other-possible-steps-for-your-organization"></a>Inne możliwe kroki dla organizacji

[!INCLUDE [digital-twins-setup-additional-requirements.md](../../includes/digital-twins-setup-additional-requirements.md)]

## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się, jak połączyć aplikację kliencką z wystąpieniem, pisząc kod uwierzytelniania aplikacji klienta:
* [*Instrukcje: zapisywanie kodu uwierzytelniania aplikacji*](how-to-authenticate-client.md)