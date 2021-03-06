---
title: Środowisko użytkownika logowania do programu SMS dla numeru telefonu (wersja zapoznawcza) — Azure AD
description: Dowiedz się więcej na temat środowiska użytkownika logowania do programu SMS dla nowych lub istniejących numerów telefonów
services: active-directory
author: curtand
manager: daveba
ms.service: active-directory
ms.subservice: user-help
ms.workload: identity
ms.topic: end-user-help
ms.date: 04/14/2020
ms.author: curtand
ms.reviewer: kasimpso
ms.custom: user-help, seo-update-azuread-jan
ms.openlocfilehash: 864225238c10ab2fda96e95448790201cc8a16ae
ms.sourcegitcommit: e71da24cc108efc2c194007f976f74dd596ab013
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/29/2020
ms.locfileid: "87423001"
---
# <a name="use-your-phone-number-as-a-user-name-preview"></a>Użyj swojego numeru telefonu jako nazwy użytkownika (wersja zapoznawcza)

Zarejestrowanie urządzenia zapewnia dostęp do usług organizacji i nie pozwala organizacji na dostęp do telefonu. Jeśli jesteś administratorem, możesz znaleźć więcej informacji w temacie [Konfigurowanie i umożliwienie użytkownikom uwierzytelniania opartego na programie SMS](../authentication/howto-authentication-sms-signin.md).

Jeśli Twoja organizacja nie zakończyła logowania za pomocą programu SMS, nie zobaczysz dla niej opcji podczas rejestrowania telefonu przy użyciu konta.  

## <a name="when-you-have-a-new-phone-number"></a>Jeśli masz nowy numer telefonu

Jeśli otrzymujesz nowy numer telefonu lub nowy i zarejestrujesz go w organizacji, dla której jest dostępne logowanie za pomocą programu SMS, będziesz mieć zwykły proces rejestracji telefonu:

1. Wybierz pozycję **Dodaj metodę**.
1. Wybierz pozycję **telefon**.
1. Wprowadź numer telefonu i wybierz pozycję **tekst ja kod**.
1. Po wprowadzeniu kodu wybierz pozycję **dalej**.
1. Zostanie wyświetlony monit z informacją o zweryfikowaniu wiadomości SMS. Twój telefon został pomyślnie zarejestrowany. "

> [!Important]
> Ze względu na znany problem w wersji zapoznawczej przez krótki czas dodawania numeru telefonu nie jest rejestrowany numer logowania do programu SMS. Musisz zalogować się przy użyciu dodanego numeru, a następnie postępować zgodnie z monitami, aby zarejestrować numer logowania do programu SMS.

### <a name="when-the-phone-number-is-in-use"></a>Gdy numer telefonu jest używany

Jeśli spróbujesz użyć numeru telefonu, który jest używany przez kogoś innego w organizacji, zobaczysz następujący komunikat:

![Komunikat o błędzie, gdy numer telefonu jest już używany](media/sms-sign-in-explainer/sms-sign-in-error.png)

Skontaktuj się z administratorem, aby rozwiązać problem.

## <a name="when-you-have-an-existing-number"></a>Jeśli masz istniejącą liczbę

Jeśli używasz już numeru telefonu w organizacji i używasz numeru telefonu jako nazwy użytkownika, następujące kroki mogą pomóc Ci w zalogowaniu się.

1. Gdy logowanie za pomocą programu SMS jest dostępne, na transparencie zostanie wyświetlony monit z pytaniem, czy chcesz włączyć numer telefonu na potrzeby logowania do programu SMS:

    [![Baner, aby włączyć logowanie do programu SMS w ramach numeru telefonu.](media/sms-sign-in-explainer/sms-sign-in-banner.png)](media/sms-sign-in-explainer/sms-sign-in-banner.png#lightbox)

1. Ponadto przycisk **Włącz** jest wyświetlany, jeśli wybierzesz karetkę na kafelku metody telefonu:

    [![Baner, aby włączyć logowanie do programu SMS w ramach numeru telefonu.](media/sms-sign-in-explainer/sms-sign-in-phone-method.png)](media/sms-sign-in-explainer/sms-sign-in-phone-method.png#lightbox)

1. Aby włączyć metodę, wybierz pozycję **Włącz**. Zostanie wyświetlony monit o potwierdzenie akcji:

    ![Okno dialogowe potwierdzania umożliwiające logowanie do numeru telefonu w programie SMS](media/sms-sign-in-explainer/sms-sign-in-confirmation.png)

1. Wybierz pozycję **Włącz**.

## <a name="when-you-remove-your-phone-number"></a>Po usunięciu numeru telefonu

1. Aby usunąć numer telefonu, wybierz przycisk Usuń na kafelku metoda logowania do wiadomości SMS.

    [![Baner, aby usunąć logowanie za pomocą programu SMS dla numeru telefonu.](media/sms-sign-in-explainer/sms-sign-in-delete-method.png)](media/sms-sign-in-explainer/sms-sign-in-delete-method.png#lightbox)

2. Po wyświetleniu monitu o potwierdzenie akcji wybierz **przycisk OK**.

Nie można usunąć numeru telefonu, który jest używany jako domyślna metoda logowania. Aby usunąć tę liczbę, należy zmienić domyślną metodę logowania, a następnie ponownie usunąć numer telefonu.
