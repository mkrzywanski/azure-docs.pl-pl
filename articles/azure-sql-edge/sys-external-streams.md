---
title: sys. external_streams (Transact-SQL) — Azure SQL Edge (wersja zapoznawcza)
description: Dowiedz się więcej o używaniu wykazu sys. external_streams w usłudze Azure SQL Edge (wersja zapoznawcza)
keywords: sys. external_streams, SQL Edge
services: sql-edge
ms.service: sql-edge
ms.topic: reference
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 05/19/2019
ms.openlocfilehash: 8200d1814537a76db357704d6baf3bf482c587e7
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "84235121"
---
# <a name="sysexternal_streams-transact-sql"></a>sys.external_streams (Transact-SQL)

Zwraca wiersz dla każdego zewnętrznego obiektu strumienia utworzonego w zakresie bazy danych.

|Nazwa kolumny|Typ danych|Opis|  
|-----------------|---------------|-----------------|
|**Nazwij**|**bazy**|Nazwa strumienia. Jest unikatowa w ramach bazy danych.|
|**object_id**|**int**|Numer identyfikacyjny obiektu strumienia. Jest unikatowa w ramach bazy danych.|
|**principal_id**|**int**|Identyfikator podmiotu zabezpieczeń, który jest właścicielem tego zestawu|
|**schema_id**|**int**| Identyfikator schematu, który zawiera obiekt.|
|**parent_object_id**|**#c1**| Numer identyfikacyjny obiektu dla obiektu nadrzędnego dla tego strumienia. W bieżącej implementacji ta wartość jest zawsze równa null.|
|**Wprowadź**|**char (2)**|Typ obiektu. W przypadku obiektów strumienia typ ma zawsze wartość "ES"|
|**type_desc**|**nvarchar (60)**| Opis typu obiektu. W przypadku obiektów strumienia typ jest zawsze "EXTERNAL_STREAM"|
|**create_date**|**datetime**| Data utworzenia obiektu.|
|**modify_date**|**datetime**| Data ostatniej modyfikacji obiektu przy użyciu instrukcji ALTER.|
|**is_ms_shipped**|**bit**| Obiekt utworzony przez składnik wewnętrzny.|  
|**is_published**|**bit**|Obiekt został opublikowany.|  
|**is_schema_published**|**bit**|Tylko schemat obiektu jest publikowany.|
|**max_column_id_used**|**bit**| Ta kolumna jest używana do celów wewnętrznych i zostanie usunięta w przyszłości|  
|**uses_ansi_nulls**|**bit**| Obiekt Stream został utworzony przy użyciu opcji Ustaw bazę danych ANSI_NULLS|
|**data_source_id**|**int**| Identyfikator obiektu zewnętrznego źródła danych reprezentowanego przez obiekt Stream |  
|**file_format_id**|**int**| Identyfikator obiektu dla formatu pliku zewnętrznego używany przez obiekt strumienia. Format pliku zewnętrznego jest wymagany do określenia rzeczywistego układu danych, do których odwołuje się strumień zewnętrzny.| 
|**przeniesienie**|**varchar(max)**| Obiekt docelowy obiektu strumienia zewnętrznego. Aby uzyskać więcej informacji, zobacz [Tworzenie strumienia zewnętrznego](overview.md) |
|**input_option**|**varchar(max)**| Opcje wejściowe używane podczas tworzenia strumienia zewnętrznego. Aby uzyskać więcej informacji, zobacz [Tworzenie strumienia zewnętrznego](overview.md) |
|**output_option**|**varchar(max)**| Opcje wyjściowe używane podczas tworzenia strumienia zewnętrznego. Aby uzyskać więcej informacji, zobacz [Tworzenie strumienia zewnętrznego](overview.md) | 

## <a name="permissions"></a>Uprawnienia

Widoczność metadanych w widokach wykazu jest ograniczona do zabezpieczania, do których użytkownik należy lub z którym użytkownik przyznał pewne uprawnienia. Aby uzyskać więcej informacji, zobacz [Konfiguracja widoczności metadanych](/sql/relational-databases/security/metadata-visibility-configuration/).

## <a name="see-also"></a>Zobacz także

- [Widoki wykazu (Transact-SQL)](/sql/relational-databases/system-catalog-views/catalog-views-transact-sql/)
- [Widoki systemowe (Transact-SQL)](/sql/t-sql/language-reference/)
