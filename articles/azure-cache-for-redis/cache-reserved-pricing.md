---
title: Przedpłata za obliczenia z zarezerwowaną pojemnością — pamięć podręczna platformy Azure dla Redis
description: Przedpłata za usługę Azure cache dla zasobów obliczeniowych Redis z zarezerwowaną pojemnością
author: yegu-ms
ms.author: yegu
ms.service: cache
ms.topic: conceptual
ms.date: 02/20/2020
ms.openlocfilehash: c4ea8db464c7cae57b80837ced9f8886a8b3cf4a
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/23/2020
ms.locfileid: "87093021"
---
# <a name="prepay-for-azure-cache-for-redis-compute-resources-with-reserved-capacity"></a>Przedpłata za usługę Azure cache dla zasobów obliczeniowych Redis z zarezerwowaną pojemnością

Usługa Azure cache for Redis teraz pomaga zaoszczędzić pieniądze dzięki wykorzystaniu zasobów obliczeniowych w porównaniu z cenami płatności zgodnie z rzeczywistym użyciem. Dzięki usłudze Azure cache for Redis zarezerwowanej pojemności można wykonać zobowiązanie z góry w pamięci podręcznej przez jeden lub trzeci okres, aby uzyskać znaczący rabat w kosztach obliczeniowych. Aby kupić pamięć podręczną platformy Azure pod kątem zarezerwowanej pojemności Redis, należy określić region platformy Azure, warstwę usługi i termin.

Nie trzeba przypisywać rezerwacji do określonych pamięci podręcznej platformy Azure dla wystąpień Redis. Już uruchomiona pamięć podręczna systemu Azure dla Redis lub nowo wdrożonych, automatycznie będzie korzystać z zarezerwowanych cen, do zarezerwowanego rozmiaru pamięci podręcznej. Zakup rezerwacji polega na przedpłaceniu za koszt obliczeń przez okres jeden lub trzy lata. Po zakupie rezerwacji w pamięci podręcznej platformy Azure dla Redis obliczeniowych, które pasują do atrybutów rezerwacji, nie są już naliczane opłaty według stawek płatności zgodnie z rzeczywistym użyciem. Rezerwacja nie obejmuje opłat za sieć ani magazyn skojarzonych z pamięcią podręczną. Na koniec okresu rezerwacji korzyść wynikająca z rozliczeń wygaśnie, a usługa Azure cache for Redis jest rozliczana według ceny płatności zgodnie z rzeczywistym użyciem. Rezerwacje nie są autoodnawiane. Aby uzyskać informacje o cenach, zapoznaj się z [usługą Azure cache for Redis — Oferta zarezerwowana](https://azure.microsoft.com/pricing/details/cache).

W [Azure Portal](https://portal.azure.com/)można kupić pamięć podręczną platformy Azure pod kątem zarezerwowanych pojemności Redis. Aby kupić zarezerwowaną pojemność:

* Musisz być w roli właściciela dla co najmniej jednej subskrypcji przedsiębiorstwa lub indywidualnej, która ma stawki płatność zgodnie z rzeczywistym użyciem.
* W przypadku subskrypcji Enterprise w witrynie [EA Portal](https://ea.azure.com/) musi być włączona opcja **Dodaj wystąpienia zarezerwowane**. Jeśli to ustawienie jest wyłączone, musisz być administratorem EA w subskrypcji.
* W przypadku programu Cloud Solution Provider (CSP) tylko agenci administracyjni lub agenci sprzedaży mogą kupować pamięć podręczną platformy Azure pod kątem zarezerwowanej pojemności Redis.

Aby uzyskać szczegółowe informacje dotyczące sposobu, w jaki Klienci korporacyjni i klienci korzystający z płatnej zgodnie z rzeczywistym użyciem są obciążani zakupami rezerwacji, zobacz [Opis użycia usługi Azure Reservation na potrzeby rejestracji w przedsiębiorstwie](https://docs.microsoft.com/azure/billing/billing-understand-reserved-instance-usage-ea) i informacje [dotyczące użycia rezerwacji na platformie Azure dla subskrypcji płatnej zgodnie z rzeczywistym wykorzystaniem](https://docs.microsoft.com/azure/billing/billing-understand-reserved-instance-usage).


## <a name="determine-the-right-cache-size-before-purchase"></a>Określ właściwy rozmiar pamięci podręcznej przed zakupem

Rozmiar rezerwacji powinien opierać się na łącznej ilości pamięci używanej przez istniejącą lub natychmiastowo wdrożoną pamięć podręczną w określonym regionie i korzystając z tej samej warstwy usług.

Załóżmy na przykład, że korzystasz z dwóch pamięci podręcznych — jeden w 13 GB, a drugi przy 26 GB. Wymagana jest co najmniej jeden rok. Ponadto Załóżmy, że planujesz skalować istniejące pamięci podręczne o wielkości 13 GB do 26 GB przez miesiąc, aby zaspokoić zapotrzebowanie sezonowe, a następnie przeskalować je ponownie. W takim przypadku można zakupić 1-krotną pamięć podręczną i 1 pamięć podręczną P3 lub 3 P2 pamięci podręcznej w rezerwacji jednorocznej, aby zmaksymalizować oszczędności. Otrzymasz rabat na łączną ilość pamięci podręcznej, która jest rezerwowana, niezależnie od tego, jak ta kwota jest przypisana w pamięci podręcznej.


## <a name="buy-azure-cache-for-redis-reserved-capacity"></a>Kupowanie pamięci podręcznej platformy Azure pod kątem zarezerwowanej pojemności Redis

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com/).
2. Wybierz pozycję **Wszystkie usługi** > **Rezerwacje**.
3. Wybierz pozycję **Dodaj** , a następnie w okienku rezerwacje zakupu wybierz pozycję **Azure cache for Redis** , aby zakupić nową rezerwację dla pamięci podręcznych.
4. Wypełnij pola wymagane. Istniejące lub nowe bazy danych zgodne z wybranymi atrybutami kwalifikują się do uzyskania rabatu zarezerwowanej pojemności. Rzeczywista liczba wystąpień usługi Azure cache for Redis, które pobierają rabat, zależą od wybranego zakresu i ilości.


![Przegląd cen zarezerwowanych](media/cache-reserved-pricing/cache-reserved-price.png)


W poniższej tabeli opisano wymagane pola.

| Pole | Opis |
| :------------ | :------- |
| Subskrypcja   | Subskrypcja używana do płacenia za pamięć podręczną platformy Azure na potrzeby rezerwacji zarezerwowanej pojemności Redis. W ramach metody płatności w ramach subskrypcji jest naliczana opłata za koszty z góry pamięci podręcznej platformy Azure na potrzeby rezerwacji zarezerwowanej pojemności Redis. Typ subskrypcji musi być umową Enterprise Agreement (Numer oferty: MS-AZR-0017P lub MS-AZR-0148P) lub indywidualna umowa z cennikiem z opcją płatność zgodnie z rzeczywistym użyciem (numery ofert: MS-AZR-0003P lub MS-AZR-0023P). W przypadku subskrypcji Enterprise opłaty są odliczane od salda zobowiązania pieniężnego rejestracji lub naliczane jako nadwyżka. W przypadku indywidualnej subskrypcji z płatnością zgodnie z rzeczywistym użyciem opłaty są naliczane za kartę kredytową lub formę płatności faktury dla subskrypcji.
| Zakres | Zakres rezerwacji może obejmować jedną subskrypcję lub wiele subskrypcji (zakres udostępniony). W przypadku wybrania opcji: </br></br> **Udostępniony**rabat w ramach rezerwacji jest stosowany do usługi Azure cache dla wystąpień Redis uruchomionych w każdej subskrypcji w kontekście rozliczania. W przypadku klientów korporacyjnych zakresem udostępnionym jest rejestracja i uwzględnianie wszystkich subskrypcji w ramach rejestracji. W przypadku klientów z płatnością zgodnie z rzeczywistym użyciem zakresem udostępnionym są wszystkie subskrypcje z opcją płatności zgodnie z rzeczywistym użyciem utworzone przez administratora konta.</br></br> **Pojedyncza subskrypcja**rabat jest stosowany do usługi Azure cache dla wystąpień Redis w tej subskrypcji. </br></br> **Pojedynczej grupy zasobów**, Rabat rezerwacji jest stosowany do usługi Azure cache dla wystąpień Redis w ramach wybranej subskrypcji i wybranej grupy zasobów w ramach tej subskrypcji.
| Region | Region świadczenia usługi Azure, który jest objęty pamięcią podręczną platformy Azure na potrzeby rezerwacji zarezerwowanej pojemności Redis.
| Warstwa cenowa | Warstwa usługi dla serwerów Redis w pamięci podręcznej platformy Azure.
| Termin | Jeden rok lub trzy lata
| Ilość | Ilość zasobów obliczeniowych zakupionych w pamięci podręcznej platformy Azure na potrzeby rezerwacji zarezerwowanej pojemności Redis. Ilość to liczba pamięci podręcznych w wybranym regionie platformy Azure i w warstwie usług, które są zarezerwowane i uzyskają rabat na rozliczenia. Na przykład w przypadku uruchamiania lub planowania uruchamiania pamięci podręcznej platformy Azure dla serwerów Redis z łączną pojemnością pamięci podręcznej wynoszącą 26 GB w regionie Wschodnie stany USA należy określić liczbę zapewniającą równowartość 26 GB, aby zmaksymalizować korzyść dla wszystkich pamięci podręcznych. Może to być 1 P3 lub 2 pamięci podręcznych.

## <a name="cancel-exchange-or-refund-reservations"></a>Anulowanie, wymiana lub zwrot rezerwacji

Rezerwacje można anulować, wymieniać lub zwracać, jednak obowiązują przy tym pewne ograniczenia. Aby uzyskać więcej informacji, zobacz temat [Self-service exchanges and refunds for Azure Reservations](https://docs.microsoft.com/azure/billing/billing-azure-reservations-self-service-exchange-and-refund) (Samoobsługowe wymiany i zwroty kosztów dla rezerwacji platformy Azure).

## <a name="cache-size-flexibility"></a>Elastyczność rozmiaru pamięci podręcznej

Elastyczność rozmiaru pamięci podręcznej ułatwia skalowanie w górę i w dół w ramach warstwy i regionu usług, bez utraty korzyści związanych z pojemnością.

## <a name="need-help-contact-us"></a>Potrzebujesz pomocy? Skontaktuj się z nami

Jeśli masz pytania lub potrzebujesz pomocy, [utwórz wniosek o pomoc techniczną](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="next-steps"></a>Następne kroki

Rabat rezerwacji jest automatycznie stosowany do pamięci podręcznej platformy Azure dla wystąpień Redis, które pasują do zakresu rezerwacji i atrybutów. Zakres rezerwacji można zaktualizować za pomocą Azure Portal, programu PowerShell, interfejsu wiersza polecenia platformy Azure lub interfejsu API.

*  Aby dowiedzieć się, jak zarezerwowane rabaty pojemności są stosowane do usługi Azure cache for Redis, zobacz [Opis rabatu rezerwacji platformy Azure](../cost-management-billing/reservations/understand-azure-cache-for-redis-reservation-charges.md)

* Aby dowiedzieć się więcej na temat rezerwacji platformy Azure, zobacz następujące artykuły:

    * [Co to jest Azure Reservations?](https://docs.microsoft.com/azure/billing/billing-save-compute-costs-reservations)
    * [Zarządzanie usługą Azure Reservations](https://docs.microsoft.com/azure/billing/billing-manage-reserved-vm-instance)
    * [Informacje na temat rabatu na rezerwacje platformy Azure](https://docs.microsoft.com/azure/billing/billing-understand-reservation-charges)
    * [Understand reservation usage for your Pay-As-You-Go subscription (Informacje na temat użycia wystąpień zarezerwowanych w przypadku subskrypcji z płatnością zgodnie z rzeczywistym użyciem)](https://docs.microsoft.com/azure/billing/billing-understand-reservation-charges-mysql)
    * [Understand reservation usage for your Enterprise enrollment (Informacje na temat użycia wystąpień zarezerwowanych w przypadku rejestracji Enterprise)](https://docs.microsoft.com/azure/billing/billing-understand-reserved-instance-usage-ea)
    * [Azure Reservations in Partner Center Cloud Solution Provider (CSP) program](https://docs.microsoft.com/partner-center/azure-reservations) (Rezerwacje platformy Azure w programie Cloud Solution Provider w Centrum partnerskim)

