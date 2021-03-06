---
title: Zarządzanie replikami odczytu — interfejs wiersza polecenia platformy Azure, interfejs API REST — Azure Database for MySQL
description: Dowiedz się, jak skonfigurować repliki odczytu i zarządzać nimi w Azure Database for MySQL przy użyciu interfejsu wiersza polecenia platformy Azure lub interfejsu API REST.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: how-to
ms.date: 6/10/2020
ms.custom: devx-track-azurecli
ms.openlocfilehash: 41c12dcb2e4c5e3b280bb349e81bcb58a0bafd01
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/31/2020
ms.locfileid: "87495765"
---
# <a name="how-to-create-and-manage-read-replicas-in-azure-database-for-mysql-using-the-azure-cli-and-rest-api"></a>Jak tworzyć repliki odczytu i zarządzać nimi w Azure Database for MySQL przy użyciu interfejsu wiersza polecenia platformy Azure i API REST

W tym artykule dowiesz się, jak tworzyć repliki odczytu i zarządzać nimi w usłudze Azure Database for MySQL przy użyciu interfejsu wiersza polecenia platformy Azure. Aby dowiedzieć się więcej o replikach odczytu, zobacz [Omówienie](concepts-read-replicas.md).

## <a name="azure-cli"></a>Interfejs wiersza polecenia platformy Azure
Można tworzyć repliki odczytu i zarządzać nimi za pomocą interfejsu wiersza polecenia platformy Azure.

### <a name="prerequisites"></a>Wymagania wstępne

- [Instalowanie interfejsu wiersza polecenia platformy Azure 2,0](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)
- [Serwer Azure Database for MySQL](quickstart-create-mysql-server-database-using-azure-portal.md) , który będzie używany jako serwer główny. 

> [!IMPORTANT]
> Funkcja odczytu repliki jest dostępna tylko dla serwerów Azure Database for MySQL w warstwach cenowych Ogólnego przeznaczenia lub zoptymalizowanych pod kątem pamięci. Upewnij się, że serwer główny znajduje się w jednej z tych warstw cenowych.

### <a name="create-a-read-replica"></a>Tworzenie repliki odczytu

> [!IMPORTANT]
> Gdy tworzysz replikę dla wzorca, który nie ma istniejących replik, wzorzec zostanie najpierw uruchomiony ponownie w celu przygotowania się do replikacji. Należy wziąć pod uwagę i wykonać te operacje w okresie poza szczytem.

Serwer repliki odczytu można utworzyć przy użyciu następującego polecenia:

```azurecli-interactive
az mysql server replica create --name mydemoreplicaserver --source-server mydemoserver --resource-group myresourcegroup
```

`az mysql server replica create`Polecenie wymaga następujących parametrów:

| Ustawienie | Przykładowa wartość | Opis  |
| --- | --- | --- |
| resource-group |  myresourcegroup |  Grupa zasobów, w której zostanie utworzony serwer repliki.  |
| name | mydemoreplicaserver | Nazwa nowego serwera repliki, który został utworzony. |
| source-server | mydemoserver | Nazwa lub identyfikator istniejącego serwera głównego, z którego ma być wykonywana replikacja. |

Aby utworzyć replikę odczytu między regionami, użyj `--location` parametru. Poniższy przykład interfejsu wiersza polecenia tworzy replikę w regionie zachodnie stany USA.

```azurecli-interactive
az mysql server replica create --name mydemoreplicaserver --source-server mydemoserver --resource-group myresourcegroup --location westus
```

> [!NOTE]
> Aby dowiedzieć się więcej na temat regionów, w których można utworzyć replikę, zapoznaj się z [artykułem dotyczącym pojęć dotyczących repliki](concepts-read-replicas.md). 

> [!NOTE]
> Repliki odczytu są tworzone z tą samą konfiguracją serwera co serwer główny. Konfigurację serwera repliki można zmienić po jego utworzeniu. Zaleca się, aby konfiguracja serwera repliki była utrzymywana z równymi lub większymi wartościami niż wzorzec, aby upewnić się, że replika jest w stanie utrzymać się z serwerem głównym.


### <a name="list-replicas-for-a-master-server"></a>Wyświetlanie listy replik dla serwera głównego

Aby wyświetlić wszystkie repliki dla danego serwera głównego, uruchom następujące polecenie: 

```azurecli-interactive
az mysql server replica list --server-name mydemoserver --resource-group myresourcegroup
```

`az mysql server replica list`Polecenie wymaga następujących parametrów:

| Ustawienie | Przykładowa wartość | Opis  |
| --- | --- | --- |
| resource-group |  myresourcegroup |  Grupa zasobów, w której zostanie utworzony serwer repliki.  |
| nazwa-serwera | mydemoserver | Nazwa lub identyfikator serwera głównego. |

### <a name="stop-replication-to-a-replica-server"></a>Zatrzymaj replikację do serwera repliki

> [!IMPORTANT]
> Zatrzymywanie replikacji na serwerze jest nieodwracalne. Po zatrzymaniu replikacji między serwerem głównym a repliką nie można jej cofnąć. Serwer repliki stał się serwerem autonomicznym i obsługuje teraz zarówno odczyt, jak i zapis. Nie można ponownie wykonać tego serwera w replice.

Replikację na serwer repliki odczytu można zatrzymać przy użyciu następującego polecenia:

```azurecli-interactive
az mysql server replica stop --name mydemoreplicaserver --resource-group myresourcegroup
```

`az mysql server replica stop`Polecenie wymaga następujących parametrów:

| Ustawienie | Przykładowa wartość | Opis  |
| --- | --- | --- |
| resource-group |  myresourcegroup |  Grupa zasobów, w której istnieje serwer repliki.  |
| name | mydemoreplicaserver | Nazwa serwera repliki, na którym ma zostać zatrzymana replikacja. |

### <a name="delete-a-replica-server"></a>Usuwanie serwera repliki

Usuwanie serwera repliki odczytu można wykonać, uruchamiając polecenie **[AZ MySQL Server Delete](/cli/azure/mysql/server)** .

```azurecli-interactive
az mysql server delete --resource-group myresourcegroup --name mydemoreplicaserver
```

### <a name="delete-a-master-server"></a>Usuwanie serwera głównego

> [!IMPORTANT]
> Usunięcie serwera głównego powoduje zatrzymanie replikacji do wszystkich serwerów repliki i usunięcie samego serwera głównego. Serwery repliki stają się serwerami autonomicznymi, które teraz obsługują zarówno odczyt, jak i zapis.

Aby usunąć serwer główny, można uruchomić polecenie **[AZ MySQL Server Delete](/cli/azure/mysql/server)** .

```azurecli-interactive
az mysql server delete --resource-group myresourcegroup --name mydemoserver
```


## <a name="rest-api"></a>Interfejs API REST
Można tworzyć repliki odczytu i zarządzać nimi za pomocą [interfejsu API REST platformy Azure](/rest/api/azure/).

### <a name="create-a-read-replica"></a>Tworzenie repliki odczytu
Replikę odczytu można utworzyć przy użyciu [interfejsu API tworzenia](/rest/api/mysql/servers/create):

```http
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DBforMySQL/servers/{replicaName}?api-version=2017-12-01
```

```json
{
  "location": "southeastasia",
  "properties": {
    "createMode": "Replica",
    "sourceServerId": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DBforMySQL/servers/{masterServerName}"
  }
}
```

> [!NOTE]
> Aby dowiedzieć się więcej na temat regionów, w których można utworzyć replikę, zapoznaj się z [artykułem dotyczącym pojęć dotyczących repliki](concepts-read-replicas.md). 

Jeśli nie ustawisz `azure.replication_support` parametru na **replikę** na serwerze głównym ogólnego przeznaczenia lub zoptymalizowanym pod kątem pamięci i ponownie uruchomiono serwer, zostanie wyświetlony komunikat o błędzie. Przed utworzeniem repliki wykonaj te dwa kroki.

Replika jest tworzona przy użyciu tych samych ustawień obliczeniowych i magazynu co główny. Po utworzeniu repliki można zmienić kilka ustawień niezależnie od serwera głównego: generowanie obliczeń, rdzeni wirtualnych, magazyn i okres przechowywania zapasowego. Warstwę cenową można także zmienić niezależnie, z wyjątkiem warstwy Podstawowa lub z niej.


> [!IMPORTANT]
> Przed zaktualizowaniem ustawienia serwera głównego do nowej wartości należy zaktualizować ustawienie repliki na wartość równą lub większą. Ta akcja ułatwia przeprowadzenie replikacji ze wszystkimi zmianami wprowadzonymi do wzorca.

### <a name="list-replicas"></a>Wyświetlanie listy replik
Listę replik serwera głównego można wyświetlić za pomocą [interfejsu API listy replik](/rest/api/mysql/replicas/listbyserver):

```http
GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DBforMySQL/servers/{masterServerName}/Replicas?api-version=2017-12-01
```

### <a name="stop-replication-to-a-replica-server"></a>Zatrzymaj replikację do serwera repliki
Można zatrzymać replikację między serwerem głównym a repliką odczytu przy użyciu [interfejsu API aktualizacji](/rest/api/mysql/servers/update).

Po zatrzymaniu replikacji na serwer główny i replikę odczytu nie można jej cofnąć. Replika odczytu jest autonomicznym serwerem, który obsługuje zarówno operacje odczytu, jak i zapisu. Serwer autonomiczny nie może zostać ponownie utworzony w replice.

```http
PATCH https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DBforMySQL/servers/{masterServerName}?api-version=2017-12-01
```

```json
{
  "properties": {
    "replicationRole":"None"  
   }
}
```

### <a name="delete-a-master-or-replica-server"></a>Usuwanie serwera głównego lub repliki
Aby usunąć serwer główny lub serwera repliki, należy użyć [interfejsu API usuwania](/rest/api/mysql/servers/delete):

Po usunięciu serwera głównego replikacja do wszystkich replik odczytu zostanie zatrzymana. Repliki odczytu stają się autonomicznymi serwerami, które teraz obsługują operacje odczytu i zapisu.

```http
DELETE https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.DBforMySQL/servers/{serverName}?api-version=2017-12-01
```


## <a name="next-steps"></a>Następne kroki

- Dowiedz się więcej na temat [odczytu replik](concepts-read-replicas.md)
