---
title: Plik dyrektywy include
description: Plik dyrektywy include
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.date: 02/14/2020
ms.topic: include
ms.author: diberry
ms.openlocfilehash: 8901f107ca2af609059670a0b31184080849d2b6
ms.sourcegitcommit: 46f8457ccb224eb000799ec81ed5b3ea93a6f06f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/28/2020
ms.locfileid: "87374831"
---
Jednostka Pattern.any umożliwia wyszukiwanie danych o swobodnym formacie, gdzie treść jednostki sprawia, że trudno rozróżnić koniec jednostki od reszty wypowiedzi.

Aplikacja kadry pomaga pracownikom w znalezieniu firmowych form.

|Wypowiedź|
|--|
|Where is **HRF-123456**? (Gdzie jest formularz HRF-123456?)|
|Who authored **HRF-123234**? (Kto jest autorem formularza HRF-123456?)|
|**HRF-456098** is published in French? (Czy formularz HRF-123456 jest publikowany w języku francuskim?)|

Jednak każdy formularz ma zarówno nazwę sformatowaną, używaną w powyższej tabeli, jak i nazwę przyjazną, taką jak `Request relocation from employee new to the company 2018 version 5`.

Wypowiedzi z przyjazną formą wyglądają następująco:

|Wypowiedź|
|--|
|Where is **Request relocation from employee new to the company 2018 version 5**? (Gdzie jest formularz Żądanie przeniesienia nowego pracownika do firmy 2018 wersja 5?)|
|Who authored **Request relocation from employee new to the company 2018 version 5**? (Kto jest autorem formularza Żądanie przeniesienia nowego pracownika do firmy 2018 wersja 5?)|
|**Request relocation from employee new to the company 2018 version 5** is published in French? (Czy formularz Żądanie przeniesienia nowego pracownika do firmy 2018 wersja 5 jest publikowany w języku francuskim?)|

Różna długość zawiera wyrazy, które mogą wprowadzać usługę LUIS w błąd co do tego, gdzie kończy się dana jednostka. Użycie jednostki Pattern.any we wzorcu umożliwia określenie początku i końca nazwy formularza, dzięki czemu usługa LUIS poprawnie wyodrębnia nazwę formularza.

|Przykładowa wypowiedź szablonu|
|--|
|Where is {FormName}[?] (Gdzie jest {Nazwa_formularza}[?])|
|Who authored {FormName}[?] (Kto jest autorem {Nazwa_formularza}[?])|
|{FormName} is published in French[?] (Czy {Nazwa_formularza} jest publikowany w języku francuskim [?])|

Wzorzec przeglądu. wszystkie informacje [referencyjne](../reference-entity-pattern-any.md)