---
title: Dynamiczna zmiana poziomu usługi woluminu dla Azure NetApp Files | Microsoft Docs
description: Opisuje sposób dynamicznego zmieniania poziomu usługi woluminu.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 07/24/2020
ms.author: b-juche
ms.openlocfilehash: e19db61efbf93e3191d5780d07952f3d195c7a59
ms.sourcegitcommit: 3d56d25d9cf9d3d42600db3e9364a5730e80fa4a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/03/2020
ms.locfileid: "87533063"
---
# <a name="dynamically-change-the-service-level-of-a-volume"></a>Dynamiczna zmiana poziomu usługi woluminu

Możesz zmienić poziom usług istniejącego woluminu, przenosząc wolumin do innej puli pojemności, która używa żądanego [poziomu usługi](azure-netapp-files-service-levels.md) dla woluminu. Ta zmiana poziomu usługi w miejscu dla woluminu nie wymaga migracji danych. Nie ma to również wpływu na dostęp do woluminu.  

Ta funkcja pozwala sprostać wymaganiom związanym z obciążeniem.  Możesz zmienić istniejący wolumin, aby użyć wyższego poziomu usługi w celu uzyskania lepszej wydajności lub użyć niższego poziomu usługi do optymalizacji kosztu. Na przykład jeśli wolumin znajduje się obecnie w puli pojemności, która używa *standardowego* poziomu usługi i chcesz, aby wolumin używał poziomu usługi *Premium* , można przenieść wolumin dynamicznie do puli pojemności korzystającej z poziomu usługi *Premium* .  

Pula pojemności, do której ma zostać przeniesiony wolumin, musi już istnieć. Pula pojemności może zawierać inne woluminy.  Jeśli chcesz przenieść wolumin do puli pojemności nowej marki, musisz [utworzyć pulę pojemności](azure-netapp-files-set-up-capacity-pool.md) przed przeniesieniem woluminu.  

## <a name="considerations"></a>Zagadnienia do rozważenia

* Po przeniesieniu woluminu do innej puli pojemności nie będziesz mieć już dostępu do poprzednich dzienników aktywności woluminu i metryk woluminów. Wolumin rozpocznie pracę od nowych dzienników aktywności i metryk w ramach nowej puli pojemności.

* W przypadku przenoszenia woluminu do puli pojemności wyższego poziomu usługi (na przykład przechodzenia z warstwy *standardowa* do *Premium* lub *Ultra* Service Level), należy odczekać co najmniej siedem dni, zanim będzie można ponownie przenieść wolumin do puli pojemności niższego poziomu usługi (na przykład przechodzenie od wersji *Ultra* do *Premium* lub *Standard*).  
Ten okres oczekiwania nie ma zastosowania, Jeśli przenosisz wolumin do puli pojemności, która ma ten sam poziom usług lub niższy poziom usług.

## <a name="register-the-feature"></a>Rejestrowanie funkcji

Funkcja przenoszenia woluminu do innej puli pojemności jest obecnie w wersji zapoznawczej. Jeśli używasz tej funkcji po raz pierwszy, musisz najpierw zarejestrować funkcję.

1. Zarejestruj funkcję: 

    ```azurepowershell-interactive
    Register-AzProviderFeature -ProviderNamespace Microsoft.NetApp -FeatureName ANFTierChange
    ```

2. Sprawdź stan rejestracji funkcji: 

    > [!NOTE]
    > **RegistrationState** może być w `Registering` stanie przez kilka minut przed zmianą na `Registered` . Przed kontynuowaniem Zaczekaj na **zarejestrowanie** stanu.

    ```azurepowershell-interactive
    Get-AzProviderFeature -ProviderNamespace Microsoft.NetApp -FeatureName ANFTierChange
    ```

## <a name="move-a-volume-to-another-capacity-pool"></a>Przenoszenie woluminu do innej puli pojemności

1.  Na stronie woluminy kliknij prawym przyciskiem myszy wolumin, którego poziom usługi chcesz zmienić. Wybierz pozycję **Zmień pulę**.

    ![Kliknij prawym przyciskiem myszy wolumin](../media/azure-netapp-files/right-click-volume.png)

2. W oknie zmiana puli wybierz pulę pojemności, do której chcesz przenieść wolumin. 

    ![Pula zmian](../media/azure-netapp-files/change-pool.png)

3.  Kliknij przycisk **OK**.


## <a name="next-steps"></a>Następne kroki  

* [Poziomy usług dla usługi Azure NetApp Files](azure-netapp-files-service-levels.md)
* [Konfigurowanie puli pojemności](azure-netapp-files-set-up-capacity-pool.md)
