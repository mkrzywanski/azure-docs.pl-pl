---
title: Skonfiguruj połączenie do Azure SQL Database przy użyciu tożsamości zarządzanej (wersja zapoznawcza)
titleSuffix: Azure Cognitive Search
description: Dowiedz się, jak skonfigurować połączenie indeksatora do Azure SQL Database przy użyciu tożsamości zarządzanej (wersja zapoznawcza)
manager: luisca
author: markheff
ms.author: maheff
ms.devlang: rest-api
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 05/18/2020
ms.openlocfilehash: d0933f5305007bc4a8238adb2b6b949ab0c11edf
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "85559930"
---
# <a name="set-up-an-indexer-connection-to-azure-sql-database-using-a-managed-identity-preview"></a>Skonfiguruj połączenie indeksatora, aby Azure SQL Database przy użyciu tożsamości zarządzanej (wersja zapoznawcza)

> [!IMPORTANT] 
> Obsługa konfigurowania połączenia ze źródłem danych przy użyciu tożsamości zarządzanej jest obecnie w publicznej wersji zapoznawczej. Funkcje wersji zapoznawczej są dostępne bez umowy dotyczącej poziomu usług i nie są zalecane w przypadku obciążeń produkcyjnych.
> Możesz zażądać dostępu do wersji zapoznawczej, wypełniając [ten formularz](https://aka.ms/azure-cognitive-search/mi-preview-request).

Na tej stronie opisano sposób konfigurowania połączenia indeksatora w celu Azure SQL Database przy użyciu tożsamości zarządzanej zamiast podawania poświadczeń w parametrach połączenia obiektu źródła danych.

Aby dowiedzieć się więcej o tej funkcji, zalecamy zapoznanie się z tym, co to jest indeksator i jak skonfigurować indeksator dla źródła danych. Więcej informacji można znaleźć w następujących linkach:

* [Omówienie indeksatora](search-indexer-overview.md)
* [Indeksator usługi Azure SQL](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)

## <a name="set-up-a-connection-using-a-managed-identity"></a>Konfigurowanie połączenia przy użyciu tożsamości zarządzanej

### <a name="1---turn-on-system-assigned-managed-identity"></a>1 — Włączanie tożsamości zarządzanej przypisanej przez system

Gdy jest włączona tożsamość zarządzana przypisana przez system, platforma Azure tworzy tożsamość usługi wyszukiwania, która może służyć do uwierzytelniania w innych usługach platformy Azure w ramach tej samej dzierżawy i subskrypcji. Tej tożsamości można następnie użyć w przypisaniach kontroli dostępu opartej na rolach (RBAC), które umożliwiają dostęp do danych podczas indeksowania.

![Włącz tożsamość zarządzaną przypisaną przez system](./media/search-managed-identities/turn-on-system-assigned-identity.png "Włącz tożsamość zarządzaną przypisaną przez system")

Po wybraniu pozycji **Zapisz** zostanie wyświetlony identyfikator obiektu przypisany do usługi wyszukiwania.

![Identyfikator obiektu](./media/search-managed-identities/system-assigned-identity-object-id.png "Identyfikator obiektu")

### <a name="2---provision-azure-active-directory-admin-for-sql-server"></a>2 — Inicjowanie obsługi administracyjnej Azure Active Directory administratora SQL Server

Podczas łączenia się z bazą danych w następnym kroku należy nawiązać połączenie z kontem usługi Azure Active Directory (Azure AD), które ma dostęp administratora do bazy danych, aby umożliwić usłudze wyszukiwania uprawnienie dostępu do bazy danych.

Postępuj zgodnie z instrukcjami znajdującymi się [tutaj](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure?tabs=azure-powershell#provision-an-azure-active-directory-administrator-for-your-azure-sql-database-server) , aby nadać kontu usługi Azure AD uprawnienia dostępu do bazy danych.

### <a name="3---assign-the-search-service-permissions"></a>3 — Przypisywanie uprawnień usługi wyszukiwania

Wykonaj poniższe kroki, aby przypisać uprawnienie usługi wyszukiwania do odczytu bazy danych.

1. Łączenie z programem Visual Studio

    ![Łączenie z programem Visual Studio](./media/search-managed-identities/connect-with-visual-studio.png "Łączenie z programem Visual Studio")

2. Uwierzytelnianie za pomocą konta usługi Azure AD

    ![Uwierzytelnianie](./media/search-managed-identities/visual-studio-authentication.png "Uwierzytelnianie")

3. Wykonaj następujące polecenia:

    Uwzględnij nawiasy wokół nazwy usługi wyszukiwania.
    
    ```
    CREATE USER [your search service name here] FROM EXTERNAL PROVIDER;
    EXEC sp_addrolemember 'db_datareader', [your search service name here];
    ```

    ![Nowe zapytanie](./media/search-managed-identities/visual-studio-new-query.png "Nowe zapytanie")

    ![Wykonaj zapytanie](./media/search-managed-identities/visual-studio-execute-query.png "Wykonaj zapytanie")

>[!NOTE]
> Jeśli tożsamość usługi wyszukiwania z kroku 1 została zmieniona po zakończeniu tego kroku, należy usunąć członkostwo w roli i usunąć użytkownika z bazy danych SQL, a następnie ponownie dodać uprawnienia, ponownie wykonując krok 3.
> Usunięcie członkostwa w roli i użytkownika można wykonać, uruchamiając następujące polecenia:
> ```
> sp_droprolemember 'db_datareader', [your search service name];
> DROP USER IF EXISTS [your search service name];
> ```

### <a name="4---add-a-role-assignment"></a>4 — Dodawanie przypisania roli

W tym kroku nadajesz usłudze Azure Wyszukiwanie poznawcze uprawnienia do odczytywania danych z SQL Server.

1. W Azure Portal przejdź do strony SQL Server platformy Azure.
2. Wybieranie **kontroli dostępu (IAM)**
3. Wybierz pozycję **Dodaj** , a następnie **Dodaj przypisanie roli**

    ![Dodaj przypisanie roli](./media/search-managed-identities/add-role-assignment-sql-server.png "Dodaj przypisanie roli")

4. Wybierz odpowiednią rolę **czytnika** .
5. Pozostaw **uprawnienia do przypisywania** jako **użytkownik usługi Azure AD, Grupa lub nazwa główna usługi**
6. Wyszukaj swoją usługę wyszukiwania, wybierz ją, a następnie wybierz pozycję **Zapisz** .

    ![Dodaj przypisanie roli czytnika](./media/search-managed-identities/add-role-assignment-sql-server-reader-role.png "Dodaj przypisanie roli czytnika")

### <a name="5---create-the-data-source"></a>5 — Tworzenie źródła danych

Podczas indeksowania z bazy danych SQL źródło danych musi mieć następujące wymagane właściwości:

* **Nazwa** jest unikatową nazwą źródła danych w ramach usługi wyszukiwania.
* **Typ** to`azuresql`
* **uwierzytelniające**
    * Podczas uwierzytelniania przy użyciu tożsamości zarządzanej format **poświadczeń** różni się od czasu, gdy nie jest używana zarządzana tożsamość. Tutaj utworzysz katalog początkowy lub nazwę bazy danych oraz identyfikator zasobu, który nie ma klucza konta ani hasła. ResourceId musi zawierać identyfikator subskrypcji Azure SQL Database, grupę zasobów SQL Database i nazwę bazy danych SQL. 
    * Format parametrów połączenia tożsamości zarządzanej:
        * *Katalog początkowy | Baza danych =**Nazwa bazy**danych; ResourceId =/subscriptions/**Identyfikator subskrypcji**/resourceGroups/**nazwę grupy zasobów**/Providers/Microsoft.SQL/Servers/**swoją nazwę SQL Server**/; Limit czasu połączenia =**Długość limitu czasu połączenia**;*
* **kontener** określa nazwę tabeli lub widoku, który ma być indeksowany.

Przykład sposobu tworzenia obiektu źródła danych SQL Azure przy użyciu [interfejsu API REST](https://docs.microsoft.com/rest/api/searchservice/create-data-source):

```
POST https://[service name].search.windows.net/datasources?api-version=2020-06-30
Content-Type: application/json
api-key: [admin key]

{
    "name" : "sql-datasource",
    "type" : "azuresql",
    "credentials" : { "connectionString" : "Database=sql-database;ResourceId=/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/azure-sql-resource-group/providers/Microsoft.Sql/servers/sql-server-search-demo;Connection Timeout=30;" },
    "container" : { "name" : "my-table" }
} 
```

Azure Portal i [zestaw .NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.datasource?view=azure-dotnet) obsługują również parametry połączenia zarządzanych tożsamości. Azure Portal wymaga flagi funkcji, która zostanie udostępniona podczas rejestrowania się w celu korzystania z wersji zapoznawczej przy użyciu linku w górnej części tej strony. 

### <a name="6---create-the-index"></a>6 — Tworzenie indeksu

Indeks określa pola w dokumencie, atrybuty i inne konstrukcje, które kształtują środowisko wyszukiwania.

Oto jak utworzyć indeks z polem z możliwością wyszukiwania `booktitle` :   

```
POST https://[service name].search.windows.net/indexes?api-version=2020-06-30
Content-Type: application/json
api-key: [admin key]

{
    "name" : "my-target-index",
    "fields": [
    { "name": "id", "type": "Edm.String", "key": true, "searchable": false },
    { "name": "booktitle", "type": "Edm.String", "searchable": true, "filterable": false, "sortable": false, "facetable": false }
    ]
}
```

Aby uzyskać więcej informacji na temat tworzenia indeksów, zobacz [Tworzenie indeksu](https://docs.microsoft.com/rest/api/searchservice/create-index)

### <a name="7---create-the-indexer"></a>7. Tworzenie indeksatora

Indeksator łączy źródło danych z docelowym indeksem wyszukiwania i zawiera harmonogram do automatyzowania odświeżania danych.

Po utworzeniu indeksu i źródła danych można przystąpić do tworzenia indeksatora.

Przykładowa definicja indeksatora dla indeksatora usługi Azure SQL:

```
POST https://[service name].search.windows.net/indexers?api-version=2020-06-30
Content-Type: application/json
api-key: [admin key]

{
    "name" : "sql-indexer",
    "dataSourceName" : "sql-datasource",
    "targetIndexName" : "my-target-index",
    "schedule" : { "interval" : "PT2H" }
}
```    

Ten indeksator będzie uruchamiany co dwie godziny (interwał harmonogramu jest ustawiony na wartość "PT2H"). Aby uruchomić indeksator co 30 minut, ustaw interwał na wartość "PT30M". Najkrótszy obsługiwany interwał to 5 minut. Harmonogram jest opcjonalny — w przypadku pominięcia, indeksator jest uruchamiany tylko raz, gdy zostanie utworzony. Można jednak uruchomić indeksator na żądanie w dowolnym momencie.   

Aby uzyskać więcej informacji na temat interfejsu API tworzenia indeksatora, zapoznaj się z tematem [Tworzenie indeksatora](https://docs.microsoft.com/rest/api/searchservice/create-indexer).

Więcej informacji o definiowaniu harmonogramów indeksatorów znajduje się w temacie [jak zaplanować indeksatory dla platformy Azure wyszukiwanie poznawcze](search-howto-schedule-indexers.md).

## <a name="troubleshooting"></a>Rozwiązywanie problemów

Jeśli zostanie wyświetlony komunikat o błędzie, gdy indeksator podejmie próbę nawiązania połączenia ze źródłem danych, że klient nie może uzyskać dostępu do serwera, zapoznaj się z [typowymi błędami indeksatora](https://docs.microsoft.com/azure/search/search-indexer-troubleshooting).

## <a name="see-also"></a>Zobacz także

Dowiedz się więcej o usłudze Azure SQL indeksator:
* [Indeksator usługi Azure SQL](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
