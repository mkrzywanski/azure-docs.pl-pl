---
title: Łączenie się z interfejsem API Azure Cosmos DB w usłudze MongoDB przy użyciu programu Studio 3T
description: Dowiedz się, jak nawiązać połączenie z interfejsem API Azure Cosmos DB MongoDB przy użyciu programu Studio 3T.
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.topic: how-to
ms.date: 03/20/2020
author: timsander1
ms.author: tisande
ms.custom: seodec18
ms.openlocfilehash: 18a9f97d2cee9dd17345a1c8c0ae0efe442d79a4
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/02/2020
ms.locfileid: "85799398"
---
# <a name="connect-to-an-azure-cosmos-account-using-studio-3t"></a>Nawiązywanie połączenia z kontem usługi Azure Cosmos za pomocą programu Studio 3T

Aby nawiązać połączenie z interfejsem API Azure Cosmos DB MongoDB za pomocą programu Studio 3T, należy:

* Pobierz i zainstaluj program [Studio 3T](https://studio3t.com/).
* Zawierają informacje o [parametrach połączenia](connect-mongodb-account.md) konta usługi Azure Cosmos.

## <a name="create-the-connection-in-studio-3t"></a>Utwórz połączenie w programie Studio 3T

Aby dodać konto usługi Azure Cosmos do Menedżera połączeń programu Studio 3T, wykonaj następujące czynności:

1. Pobierz informacje o połączeniu dla interfejsu API Azure Cosmos DB dla konta MongoDB, korzystając z instrukcji w temacie [Connect a MongoDB Application to Azure Cosmos DB](connect-mongodb-account.md) article.

    :::image type="content" source="./media/mongodb-mongochef/ConnectionStringBlade.png" alt-text="Zrzut ekranu strony parametrów połączenia":::

2. Kliknij przycisk **Połącz** , aby otworzyć Menedżera połączeń, a następnie kliknij przycisk **nowe połączenie** .

    :::image type="content" source="./media/mongodb-mongochef/ConnectionManager.png" alt-text="Zrzut ekranu Menedżera połączeń programu Studio 3T":::
3. W oknie **nowe połączenie** na karcie **serwer** wprowadź nazwę hosta (FQDN) konta usługi Azure Cosmos i port.

    :::image type="content" source="./media/mongodb-mongochef/ConnectionManagerServerTab.png" alt-text="Zrzut ekranu przedstawiający kartę serwer Menedżera połączeń programu Studio 3T":::
4. W oknie **nowe połączenie** na karcie **uwierzytelnianie** wybierz pozycję uwierzytelnianie **podstawowe (MONGODB-CR lub SCARM-SHA-1)** , a następnie wprowadź nazwę użytkownika i hasło.  Zaakceptuj domyślną bazę danych uwierzytelniania (Administrator) lub podaj własną wartość.

    :::image type="content" source="./media/mongodb-mongochef/ConnectionManagerAuthenticationTab.png" alt-text="Zrzut ekranu karty uwierzytelnianie Menedżera połączeń programu Studio 3T":::
5. W oknie **nowe połączenie** na karcie **SSL** zaznacz pole wyboru **Użyj protokołu SSL do połączenia** i przycisk radiowy **Zaakceptuj certyfikaty SSL z podpisem własnym serwera** .

    :::image type="content" source="./media/mongodb-mongochef/ConnectionManagerSSLTab.png" alt-text="Zrzut ekranu przedstawiający kartę SSL Menedżera połączeń programu Studio 3T":::
6. Kliknij przycisk **Testuj połączenie** , aby sprawdzić poprawność informacji o połączeniu, kliknij przycisk **OK** , aby powrócić do okna nowe połączenie, a następnie kliknij przycisk **Zapisz**.

    :::image type="content" source="./media/mongodb-mongochef/TestConnectionResults.png" alt-text="Zrzut ekranu przedstawiający okno połączenia testowego programu Studio 3T":::

## <a name="use-studio-3t-to-create-a-database-collection-and-documents"></a>Tworzenie bazy danych, kolekcji i dokumentów przy użyciu programu Studio 3T
Aby utworzyć bazę danych, kolekcję i dokumenty przy użyciu programu Studio 3T, wykonaj następujące czynności:

1. W **Menedżerze połączeń**zaznacz połączenie i kliknij pozycję **Połącz**.

    :::image type="content" source="./media/mongodb-mongochef/ConnectToAccount.png" alt-text="Zrzut ekranu Menedżera połączeń programu Studio 3T":::
2. Kliknij prawym przyciskiem myszy hosta i wybierz polecenie **Dodaj bazę danych**.  Podaj nazwę bazy danych i kliknij przycisk **OK**.

    :::image type="content" source="./media/mongodb-mongochef/AddDatabase1.png" alt-text="Zrzut ekranu opcji Dodaj bazę danych programu Studio 3T":::
3. Kliknij prawym przyciskiem myszy bazę danych i wybierz polecenie **Dodaj kolekcję**.  Podaj nazwę kolekcji, a następnie kliknij przycisk **Utwórz**.

    :::image type="content" source="./media/mongodb-mongochef/AddCollection.png" alt-text="Zrzut ekranu przedstawiający opcję dodawania kolekcji programu Studio 3T":::
4. Kliknij element menu **kolekcji** , a następnie kliknij pozycję **Dodaj dokument**.

    :::image type="content" source="./media/mongodb-mongochef/AddDocument1.png" alt-text="Zrzut ekranu przedstawiający element menu Dodawanie dokumentu programu Studio 3T":::
5. W oknie dialogowym Dodawanie dokumentu Wklej następujące elementy, a następnie kliknij pozycję **Dodaj dokument**.

    ```json
    {
        "_id": "AndersenFamily",
        "lastName": "Andersen",
        "parents": [
            { "firstName": "Thomas" },
            { "firstName": "Mary Kay"}
        ],
        "children": [
            {
                "firstName": "Henriette Thaulow", "gender": "female", "grade": 5,
                "pets": [{ "givenName": "Fluffy" }]
            }
        ],
        "address": { "state": "WA", "county": "King", "city": "seattle" },
        "isRegistered": true
    }
    ```
    
6. Dodaj kolejny dokument, tym razem z następującą zawartością:

    ```json
    {
        "_id": "WakefieldFamily",
        "parents": [
            { "familyName": "Wakefield", "givenName": "Robin" },
            { "familyName": "Miller", "givenName": "Ben" }
        ],
        "children": [
            {
                "familyName": "Merriam",
                "givenName": "Jesse",
                "gender": "female", "grade": 1,
                "pets": [
                    { "givenName": "Goofy" },
                    { "givenName": "Shadow" }
                ]
            },
            {
                "familyName": "Miller",
                "givenName": "Lisa",
                "gender": "female",
                "grade": 8 }
        ],
        "address": { "state": "NY", "county": "Manhattan", "city": "NY" },
        "isRegistered": false
    }
    ```

7. Wykonaj przykładowe zapytanie. Na przykład wyszukaj rodziny o nazwie "Andersen" i zwróć pola nadrzędne i stanowe.

    :::image type="content" source="./media/mongodb-mongochef/QueryDocument1.png" alt-text="Zrzut ekranu przedstawiający wyniki zapytania Mongo Chef":::

## <a name="next-steps"></a>Następne kroki

- Dowiedz się, jak [korzystać z programu Robo 3T](mongodb-robomongo.md) za pomocą interfejsu API usługi Azure Cosmos DB dla bazy danych MongoDB.
- Eksploruj [przykłady](mongodb-samples.md) bazy danych MongoDB za pomocą interfejsu API usługi Azure Cosmos DB dla bazy danych MongoDB.
