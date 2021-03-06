---
title: Azure Portal — Ogranicz dostęp do programu Import/Export do dysków zarządzanych przy użyciu linków prywatnych (wersja zapoznawcza)
description: Włącz prywatne linki (wersja zapoznawcza) dla dysków zarządzanych za pomocą Azure Portal. Umożliwienie bezpiecznego eksportowania i importowania dysków tylko w obrębie sieci wirtualnej.
author: roygara
ms.service: virtual-machines
ms.topic: overview
ms.date: 07/15/2020
ms.author: rogarana
ms.subservice: disks
ms.custom: references_regions
ms.openlocfilehash: 75b5ba995ff87649ec8a7a96a7c816bf2bec7e44
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/20/2020
ms.locfileid: "86535776"
---
# <a name="azure-portal---restrict-importexport-access-for-managed-disks-with-private-links-preview"></a>Azure Portal — Ogranicz dostęp do usługi Import/Export w przypadku dysków zarządzanych z linkami prywatnymi (wersja zapoznawcza)

Można wygenerować identyfikator URI powiązanego sygnatury dostępu współdzielonego (SAS) dla niedołączonych dysków zarządzanych i migawek do eksportowania danych do innego regionu na potrzeby rozszerzania regionalnego, odzyskiwania po awarii i odczytywania danych do analizy śledczej. Możesz również użyć identyfikatora URI sygnatury dostępu współdzielonego, aby bezpośrednio przekazać dysk VHD do pustego dysku z lokalnego.  Teraz można korzystać z [linków prywatnych](../private-link/private-link-overview.md) (wersja zapoznawcza) w celu ograniczenia eksportu i importu do Managed disks tylko z sieci wirtualnej platformy Azure. Ponadto zapewniasz, że dane nigdy nie przechodzą przez publiczny Internet i zawsze podróżują w bezpiecznej sieci szkieletowej firmy Microsoft, gdy używasz linków prywatnych. 

Można utworzyć zasób dostępu do dysku i połączyć go z siecią wirtualną w tej samej subskrypcji, tworząc prywatny punkt końcowy. Należy skojarzyć dysk lub migawkę z dostępem do dysku do eksportowania i importowania danych za pośrednictwem linków prywatnych. Ponadto należy ustawić właściwość NetworkAccessPolicy dysku lub migawki na `AllowPrivate` . 

Właściwość NetworkAccessPolicy można ustawić tak, aby `DenyAll` uniemożliwić każdyowi generowanie identyfikatora URI sygnatury dostępu współdzielonego dla dysku lub migawki. Wartość domyślna właściwości NetworkAccessPolicy to `AllowAll` .

## <a name="limitations"></a>Ograniczenia

[!INCLUDE [virtual-machines-disks-private-links-limitations](../../includes/virtual-machines-disks-private-links-limitations.md)]

## <a name="regional-availability"></a>Dostępność regionalna

[!INCLUDE [virtual-machines-disks-private-links-regions](../../includes/virtual-machines-disks-private-links-regions.md)]

## <a name="prerequisites"></a>Wymagania wstępne

Aby można było używać prywatnych punktów końcowych do eksportowania i importowania dysków zarządzanych, funkcja musi zostać włączona w Twojej subskrypcji. Wyślij wiadomość e-mail do mdprivatelinks@microsoft . com z identyfikatorami subskrypcji, aby włączyć funkcję dla subskrypcji.

Należy zwrócić uwagę na sieć wirtualną maszyny wirtualnej, do której są dołączone dyski. Sieć wirtualna jest wymagana podczas konfigurowania prywatnego punktu końcowego.

## <a name="create-a-disk-access-resource"></a>Tworzenie zasobu dostępu do dysku

1. Zaloguj się do Azure Portal i przejdź do **dostępu do dysku** za pomocą [tego linku](https://aka.ms/disksprivatelinks).

    > [!IMPORTANT]
    > Aby przejść do bloku dostęp do dysku, należy użyć [podanego linku](https://aka.ms/disksprivatelinks) . Nie jest on obecnie widoczny w portalu publicznym bez użycia linku.

1. Wybierz pozycję **+ Dodaj** , aby utworzyć nowy zasób dostępu do dysku.
1. W bloku Tworzenie wybierz swoją subskrypcję, grupę zasobów, wprowadź nazwę i wybierz region.
1. Wybierz pozycję **Przeglądanie + tworzenie**.

    :::image type="content" source="media/disks-enable-private-links-for-import-export-portal/disk-access-create-basics.png" alt-text="Zrzut ekranu bloku tworzenia dostępu do dysku. Wypełnij odpowiednią nazwę, wybierz region, wybierz grupę zasobów i wykonaj operację":::

Po utworzeniu zasobu przejdź bezpośrednio do niego.

:::image type="content" source="media/disks-enable-private-links-for-import-export-portal/screenshot-resource-button.png" alt-text="Zrzut ekranu przedstawiający przycisk Przejdź do zasobu w portalu":::

## <a name="create-a-private-endpoint"></a>Tworzenie prywatnego punktu końcowego

Teraz, gdy masz zasób dostępu do dysku, możesz go użyć, aby obsługiwać dostęp do eksportu/importu dysku, odbywa się to za pomocą prywatnych punktów końcowych. W związku z tym należy utworzyć prywatny punkt końcowy i skonfigurować go na potrzeby dostępu do dysku.

1. Z zasobów dostępu do dysku wybierz pozycję **połączenia prywatne punktów końcowych**.
1. Wybierz pozycję **+ prywatny punkt końcowy**.

    :::image type="content" source="media/disks-enable-private-links-for-import-export-portal/disk-access-main-private-blade.png" alt-text="Zrzut ekranu przedstawiający blok przegląd dla zasobu dostępu do dysku. Są wyróżnione prywatne połączenia punktów końcowych.":::

1. Wybieranie grupy zasobów
1. Wypełnij pola Nazwa i wybierz ten sam region, w którym został utworzony zasób dostępu do dysku.
1. Wybierz pozycję **Dalej: zasób >**

    :::image type="content" source="media/disks-enable-private-links-for-import-export-portal/disk-access-private-endpoint-first-blade.png" alt-text="Zrzut ekranu przedstawiający przepływ pracy tworzenia prywatnego punktu końcowego, pierwszy blok. Jeśli nie wybierzesz odpowiedniego regionu, możesz napotkać problemy później.":::

1. W bloku **zasób** wybierz pozycję **Połącz z zasobem platformy Azure w moim katalogu**.
1. W obszarze **Typ zasobu** wybierz pozycję **Microsoft. COMPUTE/diskAccesses**
1. Dla **zasobu** wybierz utworzony wcześniej zasób dostępu do dysku
1. Pozostaw **docelowy zasób podrzędny** jako **dyski**
1. Wybierz kolejno pozycje **Dalej: Configuration >**.

    :::image type="content" source="media/disks-enable-private-links-for-import-export-portal/disk-access-private-endpoint-second-blade.png" alt-text="Zrzut ekranu przedstawiający przepływ pracy tworzenia prywatnego punktu końcowego, drugi blok. Ze wszystkimi wyróżnionymi wartościami (typ zasobu, zasób, cel podrzędny)":::

1. Wybierz sieć wirtualną, do której chcesz ograniczyć eksport dysku do programu, inne sieci wirtualne nie będą mogły wyeksportować dysku.

    > [!NOTE]
    > Jeśli dla wybranej podsieci jest włączona Grupa zabezpieczeń sieci, zostanie ona wyłączona tylko dla prywatnych punktów końcowych w tej podsieci. Inne zasoby w tej podsieci nadal będą mieć wymuszanie sieciowej grupy zabezpieczeń.

1. Wybierz odpowiednią podsieć
1. Wybierz pozycję **Przeglądanie + tworzenie**.

    :::image type="content" source="media/disks-enable-private-links-for-import-export-portal/disk-access-private-endpoint-third-blade.png" alt-text="Zrzut ekranu przedstawiający prywatny przepływ pracy tworzenia punktów końcowych, trzeci blok. Wyróżniono sieć wirtualną i podsieć.":::

## <a name="enable-private-endpoint-on-your-disk"></a>Włącz prywatny punkt końcowy na dysku

1. Przejdź do dysku, który chcesz skonfigurować
1. Wybieranie **sieci**
1. Wybierz pozycję **prywatny punkt końcowy (za pomocą dostępu do dysku)** i wybierz utworzony wcześniej dostęp do dysku.
1. Wybierz pozycję **Zapisz**.

    :::image type="content" source="media/disks-enable-private-links-for-import-export-portal/disk-access-managed-disk-networking-blade.png" alt-text="Zrzut ekranu przedstawiający blok sieć dysku zarządzanego. Wyróżnij wybór prywatnego punktu końcowego oraz dostęp do wybranego dysku. Zapisanie spowoduje skonfigurowanie dysku dla tego dostępu.":::

Ukończono Konfigurowanie linków prywatnych, których można użyć podczas importowania/eksportowania dysku zarządzanego.

## <a name="next-steps"></a>Następne kroki

- [Często zadawane pytania dotyczące linków prywatnych](linux/faq-for-disks.md#private-links-for-securely-exporting-and-importing-managed-disks)
- [Eksportowanie/kopiowanie zarządzanych migawek jako dysku VHD do konta magazynu w innym regionie przy użyciu programu PowerShell](scripts/virtual-machines-windows-powershell-sample-copy-snapshot-to-storage-account.md)