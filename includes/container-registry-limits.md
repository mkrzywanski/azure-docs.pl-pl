---
title: Plik dyrektywy include
description: Plik dyrektywy include
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: include
ms.date: 06/18/2020
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 3f68ca0fc577e6cf3f896ede0418f11f59756701
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/20/2020
ms.locfileid: "86512621"
---
| Zasób | Podstawowa | Standardowa (Standard) | Premium |
|---|---|---|---|
| Uwzględniony magazyn<sup>1</sup> (GIB) | 10 | 100 | 500 |
| Limit magazynowania (TiB) | 20| 20 | 20 |
| Maksymalny rozmiar warstwy obrazu (GiB) | 200 | 200 | 200 |
| ReadOps na minutę<sup>2, 3</sup> | 1000 | 3000 | 10 000 |
| WriteOps na minutę<sup>2, 4</sup> | 100 | 500 | 2000 |
| Pobierz przepustowość<sup>2</sup> MB/s | 30 | 60 | 100 |
| Przepustowość przekazywania<sup>2</sup> MB/s | 10 | 20 | 50 |
| Elementy webhook | 2 | 10 | 500 |
| Replikacja geograficzna | Nie dotyczy | Nie dotyczy | [Obsługiwane][geo-replication] |
| Zaufanie do zawartości | Nie dotyczy | Nie dotyczy | [Obsługiwane][content-trust] |
| Prywatny link z prywatnymi punktami końcowymi | Nie dotyczy | Nie dotyczy | [Obsługiwane][plink] |
| &bull;Prywatne punkty końcowe | Nie dotyczy | Nie dotyczy | 10 |
| Dostęp do sieci wirtualnej punktu końcowego usługi | Nie dotyczy | Nie dotyczy | [Wersja zapoznawcza][vnet] |
| Klucze zarządzane przez klienta | Nie dotyczy | Nie dotyczy | [Obsługiwane][cmk] |
| Uprawnienia w zakresie repozytorium | Nie dotyczy | Nie dotyczy | [Wersja zapoznawcza][token]|
| &bull;Żeton | Nie dotyczy | Nie dotyczy | 20 000 |
| &bull;Mapy zakresu | Nie dotyczy | Nie dotyczy | 20 000 |
| &bull;Repozytoria na mapę zakresu | Nie dotyczy | Nie dotyczy | 500 |


<sup>1</sup> magazyn uwzględniony w dziennej stawce dla każdej warstwy. W przypadku dodatkowego magazynu opłata jest naliczana za dodatkową dzienną stawkę za GiB, aż do limitu magazynu. Aby uzyskać informacje o stawkach, zobacz [Cennik usługi Azure Container Registry][pricing].

<sup>2</sup>*ReadOps*, *WriteOps*i *przepustowość* są minimalnymi oszacowaniami. Azure Container Registry dąży do poprawy wydajności, ponieważ wymaga użycia.

<sup>3</sup> [Docker pull](https://docs.docker.com/registry/spec/api/#pulling-an-image) Wykonuje translację na wiele operacji odczytu na podstawie liczby warstw w obrazie oraz pobierania manifestu.

<sup>4</sup> [Wypychanie Docker](https://docs.docker.com/registry/spec/api/#pushing-an-image) jest tłumaczone na wiele operacji zapisu na podstawie liczby warstw, które muszą zostać wypchnięte. A `docker push` obejmuje *ReadOps* do pobrania manifestu dla istniejącego obrazu.

<!-- LINKS - External -->
[pricing]: https://azure.microsoft.com/pricing/details/container-registry/

<!-- LINKS - Internal -->
[geo-replication]: ../articles/container-registry/container-registry-geo-replication.md
[content-trust]: ../articles/container-registry/container-registry-content-trust.md
[vnet]: ../articles/container-registry/container-registry-vnet.md
[plink]: ../articles/container-registry/container-registry-private-link.md
[cmk]: ../articles/container-registry/container-registry-customer-managed-keys.md
[token]: ../articles/container-registry/container-registry-repository-scoped-permissions.md