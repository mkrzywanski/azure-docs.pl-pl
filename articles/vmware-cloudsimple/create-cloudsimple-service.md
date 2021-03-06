---
title: Rozwiązanie VMware firmy Azure według CloudSimple — Tworzenie usługi CloudSimple
description: Opisuje sposób tworzenia usługi CloudSimple w Azure Portal
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/19/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 8648f2c9cc0175050d4b7642f5235d47159ecfaf
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "77024827"
---
# <a name="create-the-azure-vmware-solution-by-cloudsimple-service"></a>Tworzenie rozwiązania Azure VMware według usługi CloudSimple

Aby rozpocząć pracę z rozwiązaniem VMware platformy Azure przez CloudSimple, Utwórz rozwiązanie Azure VMware według usługi CloudSimple w Azure Portal.

## <a name="before-you-begin"></a>Przed rozpoczęciem

Przydziel blok CIDR/28 dla podsieci bramy. Podsieć bramy jest wymagana dla usługi CloudSimple i jest unikatowa dla regionu, w którym została utworzona. Podsieć bramy jest używana dla usług sieci brzegowej i wymaga bloku/28 CIDR. Przestrzeń adresowa podsieci bramy musi być unikatowa. Nie może pokrywać się z żadną siecią, która komunikuje się ze środowiskiem CloudSimple. Sieci, które komunikują się z usługą CloudSimple, obejmują sieci lokalne i sieci wirtualne platformy Azure.

## <a name="sign-in-to-azure"></a>Logowanie do platformy Azure

Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).

## <a name="create-the-service"></a>Tworzenie usługi

1. Wybierz pozycję **Wszystkie usługi**.
2. Wyszukaj **usługi CloudSimple Services**.
    ![Wyszukaj usługę CloudSimple](media/create-cloudsimple-service-search.png)
3. Wybierz pozycję **CloudSimple Services**.
4. Kliknij przycisk **Dodaj** , aby utworzyć nową usługę.
    ![Dodaj usługę CloudSimple](media/create-cloudsimple-service-add.png)
5. Wybierz subskrypcję, w której chcesz utworzyć usługę CloudSimple.
6. Wybierz grupę zasobów dla usługi. Aby dodać nową grupę zasobów, kliknij pozycję **Utwórz nową**.
7. Wprowadź nazwę identyfikującą usługę.
8. Wprowadź wartość CIDR dla bramy usługi. Określ podsieć/28, która nie nakłada się na żadną z podsieci lokalnych, podsieci platformy Azure lub podsieci planowanych CloudSimple. Po utworzeniu usługi nie można zmienić CIDR.

    ![Tworzenie usługi CloudSimple](media/create-cloudsimple-service.png)
9. Kliknij przycisk **OK**.

Usługa zostanie utworzona i dodana do listy usług.

## <a name="next-steps"></a>Następne kroki

* Dowiedz się, jak [inicjować obsługę węzłów](create-nodes.md)
* Dowiedz się, jak [utworzyć chmurę prywatną](create-private-cloud.md)
* Dowiedz się, jak [skonfigurować środowisko chmury prywatnej](quickstart-create-private-cloud.md)
