---
title: Migrowanie z biblioteki procesora źródła zmian do zestawu SDK platformy Azure Cosmos DB .NET v3
description: Dowiedz się, w jaki sposób przeprowadzić migrację aplikacji z używania biblioteki procesora źródła zmian do zestawu Azure Cosmos DB SDK v3
author: ealsur
ms.service: cosmos-db
ms.topic: how-to
ms.date: 09/17/2019
ms.author: maquaran
ms.openlocfilehash: 9640800bb53fe2fd5b27cb6e232e09c72158f8da
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "85261413"
---
# <a name="migrate-from-the-change-feed-processor-library-to-the-azure-cosmos-db-net-v3-sdk"></a>Migrowanie z biblioteki procesora źródła zmian do zestawu SDK platformy Azure Cosmos DB .NET v3

W tym artykule opisano czynności wymagane do przeprowadzenia migracji kodu istniejącej aplikacji korzystającej z [biblioteki procesora źródła zmian](https://github.com/Azure/azure-documentdb-changefeedprocessor-dotnet) do funkcji [kanału informacyjnego zmiany](change-feed.md) w najnowszej wersji zestawu .NET SDK (nazywanej także zestawem SDK dla platformy .NET v3).

## <a name="required-code-changes"></a>Wymagane zmiany kodu

Zestaw SDK dla platformy .NET v3 zawiera kilka istotnych zmian: należy wykonać następujące czynności, aby przeprowadzić migrację aplikacji.

1. Przekonwertuj `DocumentCollectionInfo` wystąpienia na `Container` odwołania do kontenerów monitorowane i dzierżawione.
1. Dostosowania, które `WithProcessorOptions` powinny być używane do użycia `WithLeaseConfiguration` i `WithPollInterval` dla interwałów, `WithStartTime` [czasu rozpoczęcia](how-to-configure-change-feed-start-time.md)i `WithMaxItems` definiowania maksymalnej liczby elementów.
1. Ustaw `processorName` wartość na na `GetChangeFeedProcessorBuilder` zgodną z wartością skonfigurowaną `ChangeFeedProcessorOptions.LeasePrefix` lub Użyj `string.Empty` innej.
1. Zmiany nie są już dostarczane jako, ale jest `IReadOnlyList<Document>` `IReadOnlyCollection<T>` typem, `T` który należy zdefiniować, nie istnieje już Klasa elementu podstawowego.
1. Aby obsłużyć zmiany, nie potrzebujesz już implementacji, zamiast tego musisz [zdefiniować delegata](change-feed-processor.md#implementing-the-change-feed-processor). Delegat może być funkcją statyczną lub, jeśli trzeba zachować stan w ramach wykonywania, można utworzyć własną klasę i przekazać metodę wystąpienia jako delegat.

Na przykład, jeśli oryginalny kod, aby skompilować procesor źródła zmian, wygląda następująco:

[!code-csharp[Main](~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos.Samples/Usage/ChangeFeed/Program.cs?name=ChangeFeedProcessorLibrary)]

Migrowany kod będzie wyglądać następująco:

[!code-csharp[Main](~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos.Samples/Usage/ChangeFeed/Program.cs?name=ChangeFeedProcessorMigrated)]

I delegat, może być metodą statyczną:

[!code-csharp[Main](~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos.Samples/Usage/ChangeFeed/Program.cs?name=Delegate)]

## <a name="state-and-lease-container"></a>Kontener stanu i dzierżawy

Podobnie jak w przypadku biblioteki procesora kanału informacyjnego zmiany funkcja kanału informacyjnego w programie .NET v3 SDK używa [kontenera dzierżawy](change-feed-processor.md#components-of-the-change-feed-processor) do przechowywania stanu. Jednak schematy są różne.

Procesor kanału informacyjnego zmian zestawu SDK v3 wykrywa stary stan biblioteki i migruje go do nowego schematu automatycznie przy pierwszym wykonaniu zmigrowanego kodu aplikacji. 

Możesz bezpiecznie zatrzymać aplikację przy użyciu starego kodu, zmigrować kod do nowej wersji, uruchomić zmigrowane aplikacje i wszelkie zmiany, które wystąpiły podczas zatrzymania aplikacji, zostaną pobrane i przetworzone przez nową wersję.

> [!NOTE]
> Migracje z aplikacji korzystających z biblioteki programu .NET v3 SDK są jednokierunkowe, ponieważ stan (dzierżawy) zostanie zmigrowany do nowego schematu. Migracja nie jest zgodna z poprzednimi wersjami.


## <a name="additional-resources"></a>Zasoby dodatkowe

* [Zestaw SDK Azure Cosmos DB](sql-api-sdk-dotnet.md)
* [Przykłady użycia w witrynie GitHub](https://github.com/Azure/azure-cosmos-dotnet-v3/tree/master/Microsoft.Azure.Cosmos.Samples/Usage/ChangeFeed)
* [Dodatkowe przykłady w witrynie GitHub](https://github.com/Azure-Samples/cosmos-dotnet-change-feed-processor)

## <a name="next-steps"></a>Następne kroki

Teraz można dowiedzieć się więcej o procesorze źródła zmian w następujących artykułach:

* [Omówienie procesora kanału informacyjnego zmiany](change-feed-processor.md)
* [Korzystanie z estymatora zestawienia zmian](how-to-use-change-feed-estimator.md)
* [Czas rozpoczęcia procesora zestawienia zmian](how-to-configure-change-feed-start-time.md)
