---
title: Skonfiguruj preferencję routingu sieciowego (wersja zapoznawcza)
titleSuffix: Azure Storage
description: Skonfiguruj preferencję routingu sieciowego (wersja zapoznawcza) dla konta usługi Azure Storage, aby określić sposób kierowania ruchu sieciowego do konta z klientów przez Internet.
services: storage
author: santoshc
ms.service: storage
ms.topic: conceptual
ms.date: 05/12/2020
ms.author: santoshc
ms.reviewer: tamram
ms.subservice: common
ms.openlocfilehash: 5b4a1b1f27dff059090d78e24a6a0eca0bbbf01f
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "85514205"
---
# <a name="configure-network-routing-preference-for-azure-storage-preview"></a>Konfigurowanie preferencji routingu sieciowego dla usługi Azure Storage (wersja zapoznawcza)

Możesz skonfigurować [preferencję routingu](../../virtual-network/routing-preference-overview.md) sieciowego (wersja zapoznawcza) dla konta usługi Azure Storage, aby określić, w jaki sposób ruch sieciowy jest kierowany do konta z klientów przez Internet. Domyślnie ruch z Internetu jest kierowany do publicznego punktu końcowego konta magazynu za pośrednictwem [sieci globalnej firmy Microsoft](../../networking/microsoft-global-network.md). Usługa Azure Storage udostępnia dodatkowe opcje konfigurowania sposobu kierowania ruchu do konta magazynu.

Skonfigurowanie preferencji routingu pozwala elastycznie optymalizować ruch w ramach wydajności sieci w warstwie Premium lub w kosztach. Konfigurując preferencję routingu, należy określić, jak domyślnie ruch będzie kierowany do publicznego punktu końcowego dla konta magazynu. Możesz również opublikować punkty końcowe specyficzne dla trasy dla konta magazynu.

## <a name="microsoft-global-network-versus-internet-routing"></a>Microsoft Global Network a Routing internetowy

Domyślnie klienci spoza środowiska platformy Azure uzyskują dostęp do konta magazynu za pośrednictwem sieci globalnej firmy Microsoft. Sieć globalna firmy Microsoft jest zoptymalizowana pod kątem opcji ścieżki o małym opóźnieniu, aby zapewnić wysoką niezawodność wydajności sieci w warstwie Premium. Ruch przychodzący i wychodzący jest kierowany przez punkt obecności (POP) znajdujący się najbliżej klienta. Ta domyślna konfiguracja routingu zapewnia, że ruch do i z konta magazynu przechodzi przez sieć globalną firmy Microsoft w celu uzyskania zbiorczej ścieżki, co maksymalizuje wydajność sieci.

Można zmienić konfigurację routingu dla konta magazynu, aby ruch przychodzący i wychodzący był kierowany do i z klientów spoza środowiska platformy Azure za pośrednictwem punktu obecności najbliższego kontu magazynu. Ta trasa minimalizuje przechodzenie ruchu przez sieć globalną firmy Microsoft w celu przełączenia jej do tranzytowego dostawcy usług w najwcześniejszym możliwym czasie. Wykorzystanie tej konfiguracji routingu obniża koszty sieci.

Na poniższym diagramie przedstawiono sposób przepływu ruchu między klientem a kontem magazynu dla każdej preferencji routingu:

![Omówienie opcji routingu dla usługi Azure Storage](media/network-routing-preference/routing-options-diagram.png)

Aby uzyskać więcej informacji o preferencjach routingu na platformie Azure, zobacz [co to jest preferencja routingu (wersja zapoznawcza)?](../../virtual-network/routing-preference-overview.md).

## <a name="routing-configuration"></a>Konfiguracja routingu

Jako domyślne preferencje routingu dla publicznego punktu końcowego konta magazynu można wybrać między siecią globalną i routingiem internetowym firmy Microsoft. Domyślne preferencja routingu ma zastosowanie do całego ruchu klientów spoza platformy Azure i ma wpływ na punkty końcowe dla Azure Data Lake Storage Gen2, usługi BLOB Storage, Azure Files i statycznych witryn sieci Web. Konfigurowanie preferencji routingu nie jest obsługiwane w przypadku kolejek platformy Azure ani tabel platformy Azure.

Możesz również opublikować punkty końcowe specyficzne dla trasy dla konta magazynu. Gdy publikujesz punkty końcowe specyficzne dla trasy, usługa Azure Storage tworzy nowe publiczne punkty końcowe dla konta magazynu, które kierują ruchem do odpowiedniej ścieżki. Ta elastyczność umożliwia kierowanie ruchu do konta magazynu przez określoną trasę bez zmiany domyślnego preferencji routingu.

Na przykład opublikowanie punktu końcowego specyficznego dla trasy internetowej dla elementu "StorageAccountA" spowoduje opublikowanie następujących punktów końcowych dla konta magazynu:

| Usługa magazynu        | Punkt końcowy dotyczący trasy                                  |
| :--------------------- | :------------------------------------------------------- |
| Blob service           | `StorageAccountA-internetrouting.blob.core.windows.net`  |
| Data Lake Storage Gen2 | `StorageAccountA-internetrouting.dfs.core.windows.net`   |
| Usługa plików           | `StorageAccountA-internetrouting.file.core.windows.net`  |
| Statyczne witryny sieci Web        | `StorageAccountA-internetrouting.web.core.windows.net`   |

Jeśli masz magazyn Geograficznie nadmiarowy do odczytu (RA-GRS) lub konto magazynu geograficznie nadmiarowego dostępnego do odczytu (Ra-GZRS), publikowanie punktów końcowych specyficznych dla trasy spowoduje również automatyczne utworzenie odpowiednich punktów końcowych w regionie pomocniczym na potrzeby dostępu do odczytu.

| Usługa magazynu        | Pomocniczy punkt końcowy tylko do odczytu dotyczący trasy                        |
| :--------------------- | :----------------------------------------------------------------- |
| Blob service           | `StorageAccountA-internetrouting-secondary.blob.core.windows.net`  |
| Data Lake Storage Gen2 | `StorageAccountA-internetrouting-secondary.dfs.core.windows.net`   |
| Usługa plików           | `StorageAccountA-internetrouting-secondary.file.core.windows.net`  |
| Statyczne witryny sieci Web        | `StorageAccountA-internetrouting-secondary.web.core.windows.net`   |

Parametry połączenia dla opublikowanych punktów końcowych określonych tras można skopiować za pośrednictwem [Azure Portal](https://portal.azure.com). Te parametry połączenia mogą służyć do autoryzacji klucza udostępnienia wszystkim istniejącym zestawom SDK i interfejsom API usługi Azure Storage.

## <a name="about-the-preview"></a>Informacje o wersji zapoznawczej

Preferencja routingu dla usługi Azure Storage jest dostępna w następujących regionach:

- Francja Południowa
- Północno-środkowe stany USA
- Zachodnio-środkowe stany USA

Następujące znane problemy mają wpływ na podgląd preferencji routingu usługi Azure Storage:

- Żądania dostępu dla punktu końcowego specyficznego dla trasy dla sieci globalnej firmy Microsoft kończą się niepowodzeniem z błędem HTTP 404 lub odpowiednikiem. Routing przez sieć globalną firmy Microsoft działa zgodnie z oczekiwaniami, gdy jest ustawiony jako domyślne preferencje routingu dla publicznego punktu końcowego.

## <a name="pricing-and-billing"></a>Cennik i rozliczenia

Aby uzyskać szczegółowe informacje dotyczące cen i rozliczeń, zobacz sekcję dotyczącą **cennika** w [preferencjach routingu (wersja zapoznawcza)?](../../virtual-network/routing-preference-overview.md#pricing).

## <a name="next-steps"></a>Następne kroki

- [Co to jest preferencja routingu (wersja zapoznawcza)?](../../virtual-network/routing-preference-overview.md)
- [Konfigurowanie zapór i sieci wirtualnych usługi Azure Storage](storage-network-security.md)
- [Zalecenia dotyczące zabezpieczeń usługi BLOB Storage](../blobs/security-recommendations.md)
