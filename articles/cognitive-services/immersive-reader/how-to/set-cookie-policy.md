---
title: Ustaw zasady plików cookie czytnika immersyjny
titleSuffix: Azure Cognitive Services
description: W tym artykule przedstawiono sposób ustawiania zasad dotyczących plików cookie dla czytnika immersyjny.
services: cognitive-services
author: pasta
manager: guillasi
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: conceptual
ms.date: 01/06/2020
ms.author: pasta
ms.custom: devx-track-javascript
ms.openlocfilehash: 6bc55416c335f370151e54d2b09ccf00bd799fdb
ms.sourcegitcommit: 42107c62f721da8550621a4651b3ef6c68704cd3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/29/2020
ms.locfileid: "87406898"
---
# <a name="how-to-set-the-cookie-policy-for-the-immersive-reader"></a>Jak ustawić zasady dotyczące plików cookie dla czytnika immersyjny

Czytnik immersyjny domyślnie wyłącza użycie plików cookie. W przypadku włączenia użycia plików cookie, czytnik immersyjny może używać plików cookie do obsługi preferencji użytkownika i śledzenia użycia funkcji. W przypadku włączenia użycia plików cookie w czytniku immersyjny należy wziąć pod uwagę wymagania zasad zgodności plików cookie UE. Aplikacja hosta jest odpowiedzialna za uzyskanie wszelkich niezbędnych wyrazów zgody użytkownika zgodnie z zasadami zgodności plików cookie UE.

Zasady dotyczące plików cookie można ustawić za pomocą [opcji](../reference.md#options)czytnika immersyjny.

## <a name="enable-cookie-usage"></a>Włącz użycie plików cookie

```javascript
var options = {
    'cookiePolicy': ImmersiveReader.CookiePolicy.Enable
};

ImmersiveReader.launchAsync(YOUR_TOKEN, YOUR_SUBDOMAIN, YOUR_DATA, options);
```

## <a name="disable-cookie-usage"></a>Wyłącz użycie plików cookie

```javascript
var options = {
    'cookiePolicy': ImmersiveReader.CookiePolicy.Disable
};

ImmersiveReader.launchAsync(YOUR_TOKEN, YOUR_SUBDOMAIN, YOUR_DATA, options);
```

## <a name="next-steps"></a>Następne kroki

* Wyświetl [Node.js przewodnika Szybki Start](../quickstarts/client-libraries.md?pivots=programming-language-nodejs) , aby zobaczyć, co jeszcze można zrobić z zestawem SDK "immersyjny" przy użyciu Node.js
* Obejrzyj [Samouczek systemu Android](../tutorial-android.md) , aby zobaczyć, co jeszcze można zrobić z zestawem SDK "immersyjny" przy użyciu języka Java lub Kotlin dla systemu Android
* Wyświetl [Samouczek systemu iOS](../tutorial-ios.md) , aby zobaczyć, co jeszcze można zrobić z zestawem SDK "immersyjny" przy użyciu SWIFT dla systemu iOS
* Obejrzyj [samouczek języka Python](../tutorial-python.md) , aby zobaczyć, co jeszcze można zrobić z zestawem SDK dla czytnika immersyjny przy użyciu języka Python
* Poznaj [zestaw SDK czytnika immersyjny](https://github.com/microsoft/immersive-reader-sdk) i [Kompendium zestawu SDK czytnika immersyjny](../reference.md)