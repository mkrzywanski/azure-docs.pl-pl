---
title: 'Szybki Start: wysyłanie zdarzeń niestandardowych do funkcji platformy Azure — Event Grid'
description: 'Szybki Start: Użyj Azure Event Grid i interfejsu wiersza polecenia platformy Azure w celu opublikowania tematu i zasubskrybowania tego zdarzenia. Funkcja platformy Azure jest używana w punkcie końcowym.'
ms.date: 07/07/2020
ms.topic: quickstart
ms.openlocfilehash: 26ddfd1aeb61d3786edcdfca1acf5e293e4145ae
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/08/2020
ms.locfileid: "86115098"
---
# <a name="quickstart-route-custom-events-to-an-azure-function-with-event-grid"></a>Szybki Start: kierowanie zdarzeń niestandardowych do funkcji platformy Azure za pomocą Event Grid

Azure Event Grid to usługa obsługi zdarzeń dla chmury. Azure Functions jest jednym z obsługiwanych obsługi zdarzeń. W tym artykule omówiono użycie witryny Azure Portal w celu utworzenia tematu niestandardowego, zasubskrybowania go i wyzwolenia zdarzenia pozwalającego na wyświetlenie wyniku. Zdarzenia są wysyłane do funkcji platformy Azure.

[!INCLUDE [quickstarts-free-trial-note.md](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-azure-function"></a>Tworzenie funkcji platformy Azure

Przed zasubskrybowaniem tematu niestandardowego Utwórzmy funkcję do obsługi zdarzeń. W Azure Portal kliknij pozycję "Utwórz zasób" i wpisz "funkcja", a następnie wybierz opcję "aplikacja funkcji" i kliknij przycisk Utwórz. Wybierz pozycję "Utwórz nowy" w obszarze Grupa zasobów i nadaj jej nazwę. Ta funkcja zostanie użyta w pozostałej części samouczka. Nadaj aplikacja funkcji nazwę, pozostaw przełącznik "Publikuj" w "kodzie", Wybierz dowolne środowisko uruchomieniowe i region, a następnie kliknij przycisk Utwórz.

Gdy aplikacja funkcji będzie gotowa, przejdź do niego i kliknij pozycję "+ Nowa funkcja". Wybierz pozycję "w portalu" dla środowiska deweloperskiego i naciśnij przycisk Kontynuuj. W obszarze Utwórz funkcję wybierz pozycję "Więcej szablonów", aby wyświetlić więcej szablonów, a następnie wyszukaj ciąg "Azure Event Grid Trigger" i wybierz go. Jeśli ten wyzwalacz jest używany po raz pierwszy, może być konieczne kliknięcie przycisku "Zainstaluj", aby zainstalować rozszerzenie.

![Wyzwalacz Event Grid funkcji](./media/custom-event-to-function/grid-trigger.png)

Po zainstalowaniu rozszerzenia kliknij przycisk Kontynuuj, nadaj funkcji nazwę, a następnie naciśnij przycisk Utwórz.

[!INCLUDE [event-grid-register-provider-portal.md](../../includes/event-grid-register-provider-portal.md)]

## <a name="create-a-custom-topic"></a>Tworzenie tematu niestandardowego

Temat usługi Event Grid udostępnia zdefiniowany przez użytkownika punkt końcowy, w którym publikowane są zdarzenia. 

1. Zaloguj się do [Azure Portal](https://portal.azure.com/).
2. Wybierz pozycję **wszystkie usługi** w menu nawigacji po lewej stronie, wyszukaj pozycję **Event Grid**i wybierz pozycję **Event Grid tematy**. 

    ![Wybierz tematy Event Grid](./media/custom-event-to-function/select-event-grid-topics.png)
3. Na stronie **tematy Event Grid** wybierz pozycję **+ Dodaj** na pasku narzędzi. 

    ![Dodaj przycisk tematu Event Grid](./media/custom-event-to-function/add-event-grid-topic-button.png)

4. Na stronie **Tworzenie tematu** wykonaj następujące kroki:

    1. Podaj unikatową **nazwę** tematu niestandardowego. Nazwa tematu musi być unikatowa, ponieważ jest reprezentowana przez wpis DNS. Nie należy używać nazwy widocznej na obrazie. Zamiast tego utwórz własną nazwę — musi mieć od 3 do 50 znaków i może zawierać wyłącznie wartości a–z, A–Z, 0–9 i „-”.
    2. Wybierz swoją **subskrypcję** platformy Azure.
    3. Wybierz tę samą grupę zasobów z poprzednich kroków.
    4. Wybierz **lokalizację** tematu usługi Event Grid.
    5. Zachowaj wartość domyślną **Event Grid schemacie** dla pola **schemat zdarzenia** . 

       ![Utwórz stronę tematu](./media/custom-event-to-function/create-custom-topic.png)
    6. Wybierz pozycję **Utwórz**. 

5. Po utworzeniu tematu niestandardowego zostanie wyświetlone powiadomienie z informacją o powodzeniu. Wybierz pozycję **Przejdź do grupy zasobów**. 

   ![Wyświetlanie powiadomienia z informacją o powodzeniu](./media/custom-event-to-function/success-notification.png)

6. Na stronie **Grupa zasobów** wybierz temat siatka zdarzeń. 

   ![Wybierz zasób tematu z siatką zdarzeń](./media/custom-event-to-function/select-event-grid-topic.png)

7. Zostanie wyświetlona strona **tematu Event Grid** dla usługi Event Grid. Pozostaw Tę stronę otwartą. Używasz go później w przewodniku Szybki Start. 

    ![Strona główna tematu Event Grid](./media/custom-event-to-function/event-grid-topic-home-page.png)

## <a name="subscribe-to-custom-topic"></a>Subskrybowanie do tematu niestandardowego

Zasubskrybowanie tematu pozwala poinformować usługę Event Grid o tym, które zdarzenia chcesz śledzić i gdzie mają być one wysyłane.

1. Teraz na stronie **tematu Event Grid** tematu niestandardowego wybierz pozycję **+ subskrypcja zdarzeń** na pasku narzędzi.

   ![Dodawanie subskrypcji zdarzeń](./media/custom-event-to-function/new-event-subscription.png)

2. Na stronie **Tworzenie subskrypcji zdarzeń** wykonaj następujące kroki:
    1. Wprowadź **nazwę** subskrypcji zdarzeń.
    3. Wybierz **funkcję platformy Azure** dla **typu punktu końcowego**. 
    4. Wybierz **pozycję Wybierz punkt końcowy**. 

       ![Podawanie wartości związanych z subskrypcją zdarzeń](./media/custom-event-to-function/provide-subscription-values.png)

    5. Dla punktu końcowego funkcji wybierz subskrypcję platformy Azure i grupę zasobów, w której znajduje się aplikacja funkcji, a następnie wybierz utworzone wcześniej aplikacja funkcji i funkcję. Wybierz pozycję **Potwierdź wybór**.

       ![Podawanie adresu URL punktu końcowego](./media/custom-event-to-function/provide-endpoint.png)

    6. Wróć na stronę **Tworzenie subskrypcji zdarzeń** , a następnie wybierz pozycję **Utwórz**.

## <a name="send-an-event-to-your-topic"></a>Wysyłanie zdarzenia do tematu

Teraz wyzwólmy zdarzenie, aby zobaczyć, jak usługa Event Grid dystrybuuje komunikat do punktu końcowego. Możesz wysłać zdarzenie testowe do niestandardowego tematu za pomocą interfejsu wiersza polecenia platformy Azure lub programu PowerShell. Zwykle dane zdarzenia są wysyłane przez aplikację lub usługę platformy Azure.

W pierwszym przykładzie użyto interfejsu wiersza polecenia platformy Azure. Pobierany jest adres URL i klucz dla tematu niestandardowego oraz przykładowe dane zdarzenia. Użyj nazwy tematu niestandardowego dla wartości `<topic name>`. Tworzy ona przykładowe dane zdarzenia. Element `data` danych JSON to ładunek zdarzenia. W tym polu można umieścić dowolną poprawnie sformułowaną zawartość JSON. Można też używać pola tematu do zaawansowanego routingu i filtrowania. CURL to narzędzie, które wysyła żądania HTTP.


### <a name="azure-cli"></a>Interfejs wiersza polecenia platformy Azure
1. W Azure Portal wybierz pozycję **Cloud Shell**. W lewym górnym rogu okna Cloud Shell wybierz pozycję **bash** . 

    ![Cloud Shell — bash](./media/custom-event-quickstart-portal/cloud-shell-bash.png)
1. Uruchom następujące polecenie, aby uzyskać **punkt końcowy** dla tematu: po skopiowaniu i wklejeniu polecenia zaktualizuj **nazwę tematu** i **nazwę grupy zasobów** przed uruchomieniem polecenia. 

    ```azurecli
    endpoint=$(az eventgrid topic show --name <topic name> -g <resource group name> --query "endpoint" --output tsv)
    ```
2. Uruchom następujące polecenie, aby uzyskać **klucz** tematu niestandardowego: po skopiowaniu i wklejeniu polecenia zaktualizuj **nazwę tematu** i nazwę **grupy zasobów** przed uruchomieniem polecenia. 

    ```azurecli
    key=$(az eventgrid topic key list --name <topic name> -g <resource group name> --query "key1" --output tsv)
    ```
3. Skopiuj poniższą instrukcję z definicją zdarzenia i naciśnij klawisz **Enter**. 

    ```json
    event='[ {"id": "'"$RANDOM"'", "eventType": "recordInserted", "subject": "myapp/vehicles/motorcycles", "eventTime": "'`date +%Y-%m-%dT%H:%M:%S%z`'", "data":{ "make": "Ducati", "model": "Monster"},"dataVersion": "1.0"} ]'
    ```
4. Uruchom następujące polecenie **zwinięcie** , aby ogłosić zdarzenie:

    ```
    curl -X POST -H "aeg-sas-key: $key" -d "$event" $endpoint
    ```

### <a name="azure-powershell"></a>Azure PowerShell
Drugi przykład obejmuje wykonanie podobnych kroków przy użyciu programu PowerShell.

1. W Azure Portal wybierz pozycję **Cloud Shell** (Alternatywnie przejdź do `https://shell.azure.com/` ). W lewym górnym rogu okna Cloud Shell wybierz pozycję **PowerShell** . Zapoznaj się z przykładowym obrazem okna **Cloud Shell** w sekcji interfejsu wiersza polecenia platformy Azure.
2. Ustaw następujące zmienne. Po skopiowaniu i wklejeniu każdego polecenia zaktualizuj **nazwę tematu** i **nazwę grupy zasobów** przed uruchomieniem polecenia:

    ```powershell
    $resourceGroupName = <resource group name>
    $topicName = <topic name>
    ```
3. Uruchom następujące polecenia, aby uzyskać **punkt końcowy** i **klucze** dla tematu:

    ```powershell
    $endpoint = (Get-AzEventGridTopic -ResourceGroupName $resourceGroupName -Name $topicName).Endpoint
    $keys = Get-AzEventGridTopicKey -ResourceGroupName $resourceGroupName -Name $topicName
    ```
4. Przygotuj zdarzenie. Skopiuj i uruchom instrukcje w oknie Cloud Shell. 

    ```powershell
    $eventID = Get-Random 99999

    #Date format should be SortableDateTimePattern (ISO 8601)
    $eventDate = Get-Date -Format s

    #Construct body using Hashtable
    $htbody = @{
        id= $eventID
        eventType="recordInserted"
        subject="myapp/vehicles/motorcycles"
        eventTime= $eventDate   
        data= @{
            make="Ducati"
            model="Monster"
        }
        dataVersion="1.0"
    }
    
    #Use ConvertTo-Json to convert event body from Hashtable to JSON Object
    #Append square brackets to the converted JSON payload since they are expected in the event's JSON payload syntax
    $body = "["+(ConvertTo-Json $htbody)+"]"
    ```
5. Użyj polecenia cmdlet **Invoke-WebRequest** , aby wysłać zdarzenie. 

    ```powershell
    Invoke-WebRequest -Uri $endpoint -Method POST -Body $body -Headers @{"aeg-sas-key" = $keys.Key1}
    ```

### <a name="verify-in-the-event-grid-viewer"></a>Sprawdź w podglądzie Event Grid
Zdarzenie zostało wyzwolone, a usługa Event Grid wysłała komunikat do punktu końcowego skonfigurowanego podczas subskrybowania. Przejdź do funkcji wyzwalanej przez Event Grid i Otwórz dzienniki. W dziennikach powinna zostać wyświetlona kopia ładunku danych zdarzenia. Jeśli nie chcesz, aby najpierw otworzyć okno dzienniki, lub naciśnij przycisk ponownie nawiąż połączenie, a następnie ponów próbę wysłania zdarzenia testowego.

![Dziennik wyzwalacza funkcji zakończonych powodzeniem](./media/custom-event-to-function/successful-function.png)

## <a name="clean-up-resources"></a>Czyszczenie zasobów
Jeśli zamierzasz kontynuować pracę z tym zdarzeniem, nie usuwaj zasobów utworzonych w tym artykule. W przeciwnym razie usuń zasoby utworzone w ramach tego artykułu.

1. W menu po lewej stronie wybierz pozycję **grupy zasobów** . Jeśli nie widzisz go w menu po lewej stronie, wybierz pozycję **wszystkie usługi** w menu po lewej stronie, a następnie wybierz pozycję **grupy zasobów**. 
2. Wybierz grupę zasobów, aby uruchomić stronę **Grupa zasobów** . 
3. Na pasku narzędzi wybierz pozycję **Usuń grupę zasobów** . 
4. Potwierdź usunięcie, wprowadzając nazwę grupy zasobów, a następnie wybierz pozycję **Usuń**. 

    ![Grupy zasobów](./media/custom-event-to-function/delete-resource-groups.png)

    Inna grupa zasobów widoczna w obrazie została utworzona i użyta przez okno Cloud Shell. Usuń ten element, jeśli nie planujesz użyć okna Cloud Shell w późniejszym czasie. 

## <a name="next-steps"></a>Następne kroki

Wiesz już, jak tworzyć tematy i subskrypcje zdarzeń. Dowiedz się więcej na temat tego, co może Ci ułatwić usługa Event Grid:

- [Event Grid — informacje](overview.md)
- [Kierowanie zdarzeń usługi Blob Storage do niestandardowego internetowego punktu końcowego](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json)
- [Monitorowanie zmian maszyn wirtualnych za pomocą usług Azure Event Grid i Logic Apps](monitor-virtual-machine-changes-event-grid-logic-app.md)
- [Przesyłanie strumieniowe danych Big Data do magazynu danych](event-grid-event-hubs-integration.md)
