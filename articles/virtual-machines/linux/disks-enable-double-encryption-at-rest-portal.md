---
title: Włącz podwójne szyfrowanie na dyskach zarządzanych w Azure Portal REST
description: Włącz podwójne szyfrowanie dla danych dysku zarządzanego przy użyciu Azure Portal.
author: roygara
ms.date: 07/23/2020
ms.topic: how-to
ms.author: rogarana
ms.service: virtual-machines-linux
ms.subservice: disks
ms.custom: references_regions
ms.openlocfilehash: 7fefaa3b13b73db119da696738af66e84f8bdd63
ms.sourcegitcommit: 0e8a4671aa3f5a9a54231fea48bcfb432a1e528c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/24/2020
ms.locfileid: "87136174"
---
# <a name="azure-portal---enable-double-encryption-at-rest-on-your-managed-disks"></a>Azure Portal — Włącz podwójne szyfrowanie na dyskach zarządzanych

Azure Disk Storage obsługuje podwójne szyfrowanie dla dysków zarządzanych. Aby uzyskać informacje koncepcyjne na temat podwójnego szyfrowania w stanie spoczynku, a także innych typów szyfrowania dysków zarządzanych, zobacz sekcję [podwójne szyfrowanie w spoczynku](disk-encryption.md#double-encryption-at-rest) artykułu szyfrowanie dysków.

[!INCLUDE [virtual-machines-double-encryption-at-rest-portal](../../../includes/virtual-machines-disks-double-encryption-at-rest-portal.md)]

## <a name="next-steps"></a>Następne kroki

- [Przykłady szablonów usługi Azure Resource Manager](https://github.com/Azure-Samples/managed-disks-powershell-getting-started/tree/master/DoubleEncryption)
- [Włącz klucze zarządzane przez klienta przy użyciu szyfrowania po stronie serwera — przykłady](disks-enable-customer-managed-keys-cli.md#examples)