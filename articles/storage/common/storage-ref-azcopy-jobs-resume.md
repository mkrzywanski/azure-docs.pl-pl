---
title: wznowienie zadań AzCopy | Microsoft Docs
description: Ten artykuł zawiera informacje dotyczące polecenia AzCopy zadania Resume.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 07/24/2020
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: dd25bec04d651c01d622f0652a29a65069421786
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/28/2020
ms.locfileid: "87281960"
---
# <a name="azcopy-jobs-resume"></a>azcopy jobs resume

Wznawia istniejące zadanie z danym IDENTYFIKATORem zadania.

## <a name="synopsis"></a>Streszczenie

```azcopy
azcopy jobs resume [jobID] [flags]
```

## <a name="related-conceptual-articles"></a>Pokrewne artykuły koncepcyjne

- [Wprowadzenie do narzędzia AzCopy](storage-use-azcopy-v10.md)
- [Transferowanie danych za pomocą AzCopy i magazynu obiektów BLOB](storage-use-azcopy-blobs.md)
- [Transferowanie danych za pomocą narzędzia AzCopy i magazynu plików](storage-use-azcopy-files.md)
- [Konfigurowanie, optymalizowanie i rozwiązywanie problemów z AzCopy](storage-use-azcopy-configure.md)

## <a name="options"></a>Opcje

|Opcja|Opis|
|--|--|
|--ciąg docelowy-sygnatura dostępu współdzielonego|Docelowe sygnatury dostępu współdzielonego miejsca docelowego dla danego identyfikatora zadania.|
|--Wyklucz ciąg|Filtr: wykluczanie nieudanych transferów podczas wznawiania zadania. Pliki powinny być rozdzielone znakami ";".|
|-h,--pomoc|Pokaż zawartość pomocy dla polecenia Resume.|
|--include String|Filtr: Uwzględnij tylko te nieudane transfery podczas wznawiania zadania. Pliki powinny być rozdzielone znakami ";".|
|--ciąg źródłowy-SAS |Źródło SAS źródła dla danego identyfikatora zadania.|

## <a name="options-inherited-from-parent-commands"></a>Opcje dziedziczone z poleceń nadrzędnych

|Opcja|Opis|
|---|---|
|--Cap-MB/s|Szybkość transferu w megabitach na sekundę. Przepływność czasu na chwilę może się nieco różnić od końca. Jeśli ta opcja jest ustawiona na zero lub zostanie pominięta, przepływność nie zostanie ograniczona.|
|--ciąg typu wyjściowego|Format danych wyjściowych polecenia. Dostępne opcje to: text, JSON. Wartość domyślna to "text".|
|--Zaufane — ciąg sufiksów firmy Microsoft   |Określa dodatkowe sufiksy domeny, w których mogą być wysyłane Azure Active Directory tokeny logowania.  Wartość domyślna to "*. Core.Windows.NET;*. core.chinacloudapi.cn; *. Core.cloudapi.de;*. core.usgovcloudapi.net '. Wszystkie wymienione tutaj są dodawane do ustawień domyślnych. W celu zapewnienia bezpieczeństwa należy tu umieścić tylko domeny Microsoft Azure. Rozdziel wiele wpisów średnikami.|

## <a name="see-also"></a>Zobacz także

- [azcopy jobs](storage-ref-azcopy-jobs.md)
