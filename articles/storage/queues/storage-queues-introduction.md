---
title: Wprowadzenie do kolejek platformy Azure — Azure Storage
description: Wprowadzenie do kolejek platformy Azure
author: mhopkins-msft
ms.author: mhopkins
ms.date: 03/18/2020
ms.service: storage
ms.subservice: queues
ms.topic: overview
ms.reviewer: dineshm
ms.openlocfilehash: 678ab8a3f3dace17671c242939c20cb5499b2e39
ms.sourcegitcommit: ad66392df535c370ba22d36a71e1bbc8b0eedbe3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/16/2020
ms.locfileid: "84805125"
---
# <a name="what-are-azure-queues"></a>Czym są kolejki platformy Azure?

Azure Queue Storage to usługa służąca do przechowywania dużej liczby komunikatów. Użytkownik uzyskuje dostęp do komunikatów z dowolnego miejsca na świecie za pośrednictwem uwierzytelnionych połączeń przy użyciu protokołu HTTP lub HTTPS. Komunikat w kolejce może mieć rozmiar do 64 KB. Kolejka może zawierać miliony komunikatów, do łącznego limitu pojemności konta magazynu. Kolejki są często używane do tworzenia zaległości prac do przetwarzania asynchronicznego.

## <a name="queue-service-concepts"></a>Koncepcje usługa kolejki

Usługa kolejki zawiera następujące składniki:

![Diagram przedstawiający relację między kontem magazynu, kolejkami i komunikatami](./media/storage-queues-introduction/queue1.png)

* **Format adresu URL:** adresy URL kolejek mają następujący format:

    `https://<storage account>.queue.core.windows.net/<queue>`
  
    Następujący adres URL dotyczy kolejki w schemacie:  
  
    `https://myaccount.queue.core.windows.net/images-to-download`

* **Konto magazynu:** Cały dostęp do usługi Azure Storage odbywa się za pomocą konta magazynu. Aby uzyskać informacje na temat pojemności konta magazynu, zobacz [cele skalowalności i wydajności dla kont magazynu w warstwie Standardowa](../common/scalability-targets-standard-account.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json).

* **Kolejka:** kolejka zawiera zestaw komunikatów. Nazwa kolejki **musi** zawierać tylko małe litery. Informacje dotyczące nazewnictwa kolejek można znaleźć w temacie [Naming Queues and Metadata](https://msdn.microsoft.com/library/azure/dd179349.aspx) (Nazewnictwo kolejek i metadanych).

* **Komunikat**: komunikat w dowolnym formacie, o maksymalnym rozmiarze 64 KB. Przed wersjami 2017-07-29 maksymalny dozwolony czas wygaśnięcia wynosi siedem dni. W wersji 2017-07-29 lub nowszej maksymalny czas wygaśnięcia może być dowolną liczbą dodatnią lub-1 oznacza, że komunikat nie wygasa. Jeśli ten parametr zostanie pominięty, domyślny czas wygaśnięcia wynosi siedem dni.

## <a name="next-steps"></a>Następne kroki

* [Tworzenie konta magazynu](../storage-create-storage-account.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json)
* [Wprowadzenie do kolejek przy użyciu platformy .NET](storage-dotnet-how-to-use-queues.md)
