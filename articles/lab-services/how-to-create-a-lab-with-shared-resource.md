---
title: Jak utworzyć laboratorium z udostępnionym zasobem | Azure Lab Services
description: Dowiedz się, jak utworzyć laboratorium, które wymaga zasobu współdzielonego z uczniami.
author: emaher
ms.topic: article
ms.date: 06/26/2020
ms.author: enewman
ms.openlocfilehash: 9cb5698f95aa220208fb02a35a52ff5363a173ac
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "85443370"
---
# <a name="how-to-create-a-lab-with-a-shared-resource-in-azure-lab-services"></a>Jak utworzyć laboratorium z udostępnionym zasobem w Azure Lab Services

Czasami podczas tworzenia laboratorium zajęć mogą istnieć pewne zasoby, które muszą być współużytkowane przez wszystkich uczniów w laboratorium.  Na przykład masz serwer licencjonowania lub SQL Server klasy bazy danych.  W tym artykule omówiono procedurę włączania udostępnionego zasobu dla laboratorium.  Porozmawiamy również o tym, jak ograniczyć dostęp do tego zasobu udostępnionego.

## <a name="architecture"></a>Architektura

Jak pokazano na poniższym diagramie, będziemy mieć konto laboratorium z laboratorium.  Konto laboratorium będzie miało ustawienia komunikacji równorzędnej sieci wirtualnej, aby Sieć wirtualna dla laboratorium była połączona z siecią zasobu udostępnionego.  Na poniższym diagramie znajdują się dwie sieci wirtualne z nienakładanymi zakresami adresów IP.  Te zakresy adresów IP to tylko przykładowe zakresy.  Należy również pamiętać, że sieć wirtualna zasobów udostępnionych znajduje się w tej samej subskrypcji co konto laboratorium.

![Usługi laboratoryjne z udostępnioną architekturą zasobów](./media/how-to-create-a-lab-with-shared-resource/shared-resource-architecture.png)

## <a name="setup-shared-resource"></a>Skonfiguruj zasób udostępniony

Aby można było utworzyć laboratorium, należy utworzyć sieć wirtualną dla zasobu udostępnionego.  Aby uzyskać więcej informacji na temat tworzenia sieci wirtualnej, zobacz [Tworzenie sieci wirtualnej](../virtual-network/quick-create-portal.md).  Planowanie zakresów sieci wirtualnej, dzięki czemu nie nakładają się na adres IP maszyn laboratorium.  Aby uzyskać więcej informacji na temat planowania sieci, zobacz artykuł [Planowanie sieci wirtualnych](../virtual-network/virtual-network-vnet-plan-design-arm.md) . W naszym przykładzie zasób udostępniony znajduje się w sieci wirtualnej z zakresem 10.2.0.0/16.  Jeśli jeszcze tego nie zrobiono, [Utwórz podsieć](../virtual-network/virtual-network-manage-subnet.md#add-a-subnet) do przechowywania zasobu udostępnionego.  W tym przykładzie używamy zakresu 10.2.0.0/24, ale zakres może się różnić w zależności od potrzeb sieci.

Udostępniony zasób może być oprogramowaniem uruchomionym na maszynie wirtualnej lub w usłudze udostępnionej przez platformę Azure. Zasób udostępniony powinien być dostępny za pomocą prywatnego adresu IP.  Udostępniając zasób udostępniony tylko za pomocą prywatnego adresu IP, możesz ograniczyć dostęp do tego zasobu udostępnionego.

Na diagramie przedstawiono również sieciową grupę zabezpieczeń (sieciowej grupy zabezpieczeń), która może służyć do ograniczania ruchu pochodzącego z maszyny wirtualnej ucznia.  Można na przykład napisać regułę zabezpieczeń, która określa ruch z adresów IP dla ucznia, może uzyskać dostęp tylko do jednego zasobu udostępnionego i nic innego.  Aby uzyskać więcej informacji na temat ustawiania reguł zabezpieczeń, zobacz [Manage Network Security Group](../virtual-network/manage-network-security-group.md#work-with-security-rules). Jeśli chcesz ograniczyć dostęp do udostępnionego zasobu do określonego laboratorium, Pobierz adres IP dla laboratorium z [ustawień laboratorium z konta laboratorium](manage-labs.md#view-labs-in-a-lab-account) i ustaw regułę ruchu przychodzącego zezwalającą na dostęp tylko z tego adresu IP.  Nie zapomnij zezwolić na porty 49152 do 65535 dla tego adresu IP.  Opcjonalnie możesz znaleźć prywatny adres IP dla maszyn wirtualnych ucznia, korzystając ze [strony Pula maszyn wirtualnych](how-to-set-virtual-machine-passwords.md).

Jeśli zasób udostępniony jest maszyną wirtualną platformy Azure, na której działa wymagane oprogramowanie, może być konieczne zmodyfikowanie domyślnych reguł zapory dla maszyny wirtualnej.

## <a name="lab-account"></a>Konto laboratorium

Aby można było użyć udostępnionego zasobu, należy skonfigurować konto laboratorium do korzystania z [równorzędnej sieci wirtualnej](how-to-connect-peer-virtual-network.md).  W takim przypadku zostanie nawiązana Komunikacja równorzędna z siecią wirtualną, która przechowuje udostępniony zasób.

>[!WARNING]
>Laboratorium dla swojej klasy należy utworzyć **po** połączeniu komunikacji równorzędnej z siecią wirtualną zasobów udostępnionych w ramach konta laboratorium.  
Komputer szablonu

Gdy konto laboratorium jest połączone z siecią wirtualną, komputer szablonu powinien teraz mieć dostęp do zasobu udostępnionego.  Może być konieczne zaktualizowanie reguł zapory w zależności od dostępnego zasobu udostępnionego.
