---
title: Tworzenie elementu członkowskiego usługi Azure łańcucha bloków przy użyciu szablonu Azure Resource Manager
description: Dowiedz się, jak utworzyć element członkowski usługi Azure łańcucha bloków przy użyciu szablonu Azure Resource Manager.
services: azure-resource-manager
ms.service: azure-resource-manager
ms.topic: quickstart
ms.custom: subject-armqs, references_regions
ms.date: 07/16/2020
ms.openlocfilehash: fb7f91a0670d99170656c0c3a2c1b73d96095762
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/20/2020
ms.locfileid: "86513034"
---
# <a name="quickstart-create-an-azure-blockchain-service-member-using-an-arm-template"></a>Szybki Start: Tworzenie elementu członkowskiego usługi Azure łańcucha bloków przy użyciu szablonu ARM

W tym przewodniku szybki start wdrożono nowego członka łańcucha bloków i konsorcjum w usłudze Azure łańcucha bloków przy użyciu szablonu Azure Resource Manager (szablon ARM). Członek usługi Azure łańcucha bloków to węzeł łańcucha bloków w sieci prywatnej łańcucha bloków. Podczas aprowizacji elementu członkowskiego można utworzyć sieć konsorcjum lub dołączyć do niej. Potrzebujesz co najmniej jednego elementu członkowskiego dla sieci konsorcjum. Liczba członków łańcucha bloków wymaganych przez uczestników zależy od danego scenariusza. Uczestnicy konsorcjum mogą mieć co najmniej jednego członka łańcucha bloków lub mogą współdzielić członków z innymi uczestnikami. Aby uzyskać więcej informacji na temat konsorcjów, zobacz [Azure łańcucha bloków Service Consortium](consortium.md).

[!INCLUDE [About Azure Resource Manager](../../../includes/resource-manager-quickstart-introduction.md)]

Jeśli Twoje środowisko spełnia wymagania wstępne i masz doświadczenie w korzystaniu z szablonów ARM, wybierz przycisk **Wdróż na platformie Azure**. Szablon zostanie otwarty w witrynie Azure Portal.

[![Wdrażanie na platformie Azure](../../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-blockchain-asaservice%2Fazuredeploy.json)

## <a name="prerequisites"></a>Wymagania wstępne

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem Utwórz [bezpłatne](https://azure.microsoft.com/free/) konto.

## <a name="review-the-template"></a>Przegląd szablonu

Szablon używany w tym przewodniku Szybki start jest jednym z [szablonów szybkiego startu platformy Azure](https://azure.microsoft.com/resources/templates/201-blockchain-asaservice/).

:::code language="json" source="~/quickstart-templates/201-blockchain-asaservice/azuredeploy.json" range="1-84" highlight="52-80":::

Zasoby platformy Azure zdefiniowane w szablonie:

* [**Microsoft. łańcucha bloków/blockchainMembers**](/azure/templates/microsoft.blockchain/blockchainmembers)

## <a name="deploy-the-template"></a>Wdrożenie szablonu

1. Wybierz poniższy link, aby zalogować się do platformy Azure i otworzyć szablon.

    [![Wdrażanie na platformie Azure](../../media/template-deployments/deploy-to-azure.svg)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-blockchain-asaservice%2Fazuredeploy.json)

1. Określ ustawienia dla elementu członkowskiego usługi Azure łańcucha bloków.

    Ustawienie | Opis
    --------|------------
    Subskrypcja | Wybierz subskrypcję platformy Azure, która ma być używana dla usługi. Jeśli masz wiele subskrypcji, wybierz tę, w ramach której są naliczane opłaty za ten zasób.
    Grupa zasobów | Utwórz nową nazwę grupy zasobów lub wybierz istniejącą z subskrypcji.
    Lokalizacja | Wybierz lokalizację, w której ma zostać utworzony element członkowski. Wszyscy członkowie konsorcjum muszą znajdować się w tej samej lokalizacji. Dostępne lokalizacje wdrożenia to *westeurope, wschodnie, southeastasia, westeurope, northeurope, westus2*i *japaneast*. Funkcje mogą być niedostępne w niektórych regionach. Usługa Azure łańcucha bloków Data Manager jest dostępna w następujących regionach świadczenia usługi Azure: Wschodnie stany USA i Europa Zachodnia.
    Nazwa elementu członkowskiego | Wybierz unikatową nazwę elementu członkowskiego usługi Azure łańcucha bloków. Nazwa elementu członkowskiego łańcucha bloków może zawierać tylko małe litery i cyfry. Pierwszy znak musi być literą. Wartość musi mieć długość od 2 do 20 znaków.
    Nazwa konsorcjum | Wprowadź unikatową nazwę. Aby uzyskać więcej informacji na temat konsorcjów, zobacz [Azure łańcucha bloków Service Consortium](consortium.md).
    Hasło elementu członkowskiego | Hasło konta elementu członkowskiego służy do szyfrowania klucza prywatnego dla konta Ethereum utworzonego dla elementu członkowskiego. Do zarządzania konsorcjum używasz konta elementu członkowskiego i hasła konta elementu członkowskiego.
    Warstwa SKU | Warstwa cenowa nowej usługi. Wybierz warstwę **standardowa** i **podstawowa** . Skorzystaj z warstwy *podstawowa* na potrzeby tworzenia, testowania i sprawdzania poprawności koncepcji. Użyj warstwy *standardowa* dla wdrożeń klasy produkcyjnej. W przypadku korzystania z łańcucha bloków Data Manager lub wysyłania dużej liczby transakcji prywatnych należy również użyć warstwy *standardowa* . Zmiana warstwy cenowej między podstawowa i Standardowa po utworzeniu elementu członkowskiego nie jest obsługiwana.
    Nazwa jednostki SKU | Konfiguracja węzła i koszt nowej usługi.

1. Wybierz pozycję **Kup** , aby wdrożyć szablon.

  Azure Portal jest używany tutaj do wdrożenia szablonu. Można również użyć Azure PowerShell, interfejsu wiersza polecenia platformy Azure i API REST. Aby poznać inne metody wdrażania, zobacz [wdrażanie szablonów](../../azure-resource-manager/templates/deploy-powershell.md).

## <a name="review-deployed-resources"></a>Przejrzyj wdrożone zasoby

Za pomocą Azure Portal można wyświetlić szczegóły wdrożonego elementu członkowskiego usługi Azure łańcucha bloków. W portalu przejdź do grupy zasobów zawierającej członka usługi Azure łańcucha bloków. Wybierz utworzony element członkowski łańcucha bloków.

![Wdrożono szczegóły przeglądu elementu członkowskiego usługi Azure łańcucha bloków w Azure Portal](./media/create-member-template/deployed-member.png)

## <a name="clean-up-resources"></a>Czyszczenie zasobów

Możesz użyć elementu członkowskiego łańcucha bloków utworzonego dla następnego przewodnika Szybki start lub samouczka. Gdy zasoby nie będą już potrzebne, można je usunąć przez usunięcie grupy zasobów utworzonej dla przewodnika Szybki Start.

Aby usunąć grupę zasobów:

1. W Azure Portal przejdź do **grupy zasobów** w okienku nawigacji po lewej stronie i wybierz grupę zasobów, którą chcesz usunąć.
2. Wybierz pozycję **Usuń grupę zasobów**. Sprawdź usuwanie, wprowadzając nazwę grupy zasobów i wybierz pozycję **Usuń**.

## <a name="next-steps"></a>Następne kroki

W tym przewodniku szybki start wdrożono członka usługi Azure łańcucha bloków i nowego konsorcjum. Skorzystaj z następnego przewodnika Szybki Start, aby użyć usługi Azure łańcucha bloków Development Kit dla Ethereum w celu dołączenia do członka usług Azure łańcucha bloków.

> [!div class="nextstepaction"]
> [Użyj Visual Studio Code, aby nawiązać połączenie z usługą Azure łańcucha bloków](connect-vscode.md)