---
title: Ciągłej integracji/ciągłego wdrażania w chmurze platformy Azure za pomocą akcji GitHub
description: Jak utworzyć przepływ pracy ciągłej integracji/ciągłego wdrażania w chmurze Azure z akcjami GitHub
author: MikeDodaro
ms.author: barbkess
ms.service: spring-cloud
ms.topic: how-to
ms.date: 01/15/2019
ms.custom: devx-track-java
ms.openlocfilehash: 960e480bc3271a24f1fa898e1af6c55bcaaada7b
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/23/2020
ms.locfileid: "87037563"
---
# <a name="azure-spring-cloud-cicd-with-github-actions"></a>Ciągłej integracji/ciągłego wdrażania w chmurze platformy Azure za pomocą akcji GitHub

Akcje usługi GitHub obsługują zautomatyzowany przepływ pracy tworzenia oprogramowania. Dzięki akcjom GitHub dla chmury Azure wiosny możesz tworzyć przepływy pracy w repozytorium, aby kompilować, testować, wdrażać, wydawania i wdrożyć je na platformie Azure. 

## <a name="prerequisites"></a>Wymagania wstępne
Ten przykład wymaga [interfejsu wiersza polecenia platformy Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="set-up-github-repository-and-authenticate"></a>Skonfiguruj repozytorium GitHub i uwierzytelniaj
Potrzebujesz poświadczenia jednostki usługi platformy Azure, aby autoryzować akcję logowania platformy Azure. Aby uzyskać poświadczenia platformy Azure, wykonaj następujące polecenia na komputerze lokalnym:
```
az login
az ad sp create-for-rbac --role contributor --scopes /subscriptions/<SUBSCRIPTION_ID> --sdk-auth 
```
Aby uzyskać dostęp do określonej grupy zasobów, można zmniejszyć zakres:
```
az ad sp create-for-rbac --role contributor --scopes /subscriptions/<SUBSCRIPTION_ID>/resourceGroups/<RESOURCE_GROUP> --sdk-auth
```
Polecenie powinno wykonać wyjście obiektu JSON:
```JSON
{
    "clientId": "<GUID>",
    "clientSecret": "<GUID>",
    "subscriptionId": "<GUID>",
    "tenantId": "<GUID>",
    ...
}
```

Ten przykład używa przykładu [metryk Piggy](https://github.com/Azure-Samples/piggymetrics) w serwisie GitHub.  Rozwidlenie przykładu, Otwórz stronę repozytorium GitHub, a następnie kliknij kartę **Ustawienia** . Otwórz menu **klucze tajne** i kliknij pozycję **Dodaj nowy wpis tajny**:

 ![Dodaj nowy wpis tajny](./media/github-actions/actions1.png)

W polu Nazwa klucza tajnego Ustaw `AZURE_CREDENTIALS` wartość i jej wartości na ciąg JSON znaleziony w nagłówku *Skonfiguruj repozytorium GitHub i Uwierzytelnij*.

 ![Ustawianie danych tajnych](./media/github-actions/actions2.png)

Możesz również uzyskać poświadczenia logowania platformy Azure z Key Vault w ramach akcji usługi GitHub, jak wyjaśniono w temacie [uwierzytelnianie Azure wiosna z Key Vault w akcjach GitHub](./spring-cloud-github-actions-key-vault.md).

## <a name="provision-service-instance"></a>Inicjowanie obsługi administracyjnej wystąpienia usługi
Aby zainicjować obsługę administracyjną wystąpienia usługi w chmurze ze sprężyną Azure, uruchom następujące polecenia przy użyciu interfejsu wiersza polecenia platformy Azure.
```
az extension add --name spring-cloud
az group create --location eastus --name <resource group name>
az spring-cloud create -n <service instance name> -g <resource group name>
az spring-cloud config-server git set -n <service instance name> --uri https://github.com/xxx/piggymetrics --label config
```
## <a name="build-the-workflow"></a>Kompilowanie przepływu pracy
Przepływ pracy jest definiowany przy użyciu następujących opcji.

### <a name="prepare-for-deployment-with-azure-cli"></a>Przygotowanie do wdrożenia za pomocą interfejsu wiersza polecenia platformy Azure
Polecenie `az spring-cloud app create` nie jest obecnie idempotentne.  Zalecamy korzystanie z tego przepływu pracy w istniejących aplikacjach i wystąpieniach w chmurze usługi Azure wiosennej.

Użyj następujących poleceń interfejsu wiersza polecenia platformy Azure na potrzeby przygotowania:
```
az configure --defaults group=<service group name>
az configure --defaults spring-cloud=<service instance name>
az spring-cloud app create --name gateway
az spring-cloud app create --name auth-service
az spring-cloud app create --name account-service
```

### <a name="deploy-with-azure-cli-directly"></a>Bezpośrednie wdrażanie za pomocą interfejsu wiersza polecenia platformy Azure
Utwórz `.github/workflow/main.yml` plik w repozytorium:

```
name: AzureSpringCloud
on: push

env:
  GROUP: <resource group name>
  SERVICE_NAME: <service instance name>

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
    
    - uses: actions/checkout@master
    
    - name: Set up JDK 1.8
      uses: actions/setup-java@v1
      with:
        java-version: 1.8
    
    - name: maven build, clean
      run: |
        mvn clean package -DskipTests
    
    - name: Azure Login
      uses: azure/login@v1
      with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}
      
    - name: Install ASC AZ extension
      run: az extension add --name spring-cloud
   
    - name: Deploy with AZ CLI commands
      run: |
        az configure --defaults group=$GROUP
        az configure --defaults spring-cloud=$SERVICE_NAME
        az spring-cloud app deploy -n gateway --jar-path ${{ github.workspace }}/gateway/target/gateway.jar
        az spring-cloud app deploy -n account-service --jar-path ${{ github.workspace }}/account-service/target/account-service.jar
        az spring-cloud app deploy -n auth-service --jar-path ${{ github.workspace }}/auth-service/target/auth-service.jar
```
### <a name="deploy-with-azure-cli-action"></a>Wdrażanie za pomocą akcji interfejsu wiersza polecenia platformy Azure
AZ `run` polecenie będzie używać najnowszej wersji interfejsu wiersza polecenia platformy Azure. Jeśli istnieją istotne zmiany, możesz również użyć określonej wersji interfejsu wiersza polecenia platformy Azure z platformą Azure/CLI `action` . 

> [!Note] 
> To polecenie zostanie uruchomione w nowym kontenerze, więc `env` nie będzie działać, a dostęp do plików między akcjami może mieć dodatkowe ograniczenia.

Utwórz plik GitHub/Workflow/Main. yml w repozytorium:
```
name: AzureSpringCloud
on: push

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
    
    - uses: actions/checkout@master
    
    - name: Set up JDK 1.8
      uses: actions/setup-java@v1
      with:
        java-version: 1.8
    
    - name: maven build, clean
      run: |
        mvn clean package -DskipTests
        
    - name: Azure Login
      uses: azure/login@v1
      with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}
              
    - name: Azure CLI script
      uses: azure/CLI@v1
      with:
        azcliversion: 2.0.75
        inlineScript: |
          az extension add --name spring-cloud
          az configure --defaults group=<service group name>
          az configure --defaults spring-cloud=<service instance name>
          az spring-cloud app deploy -n gateway --jar-path $GITHUB_WORKSPACE/gateway/target/gateway.jar
          az spring-cloud app deploy -n account-service --jar-path $GITHUB_WORKSPACE/account-service/target/account-service.jar
          az spring-cloud app deploy -n auth-service --jar-path $GITHUB_WORKSPACE/auth-service/target/auth-service.jar
```

## <a name="deploy-with-maven-plugin"></a>Wdrażanie przy użyciu wtyczki Maven
Innym rozwiązaniem jest użycie [wtyczki Maven](https://docs.microsoft.com/azure/spring-cloud/spring-cloud-quickstart-launch-app-maven) do wdrożenia ustawień aplikacji jar i aktualizującej. Polecenie `mvn azure-spring-cloud:deploy` jest idempotentne i w razie konieczności automatycznie utworzy aplikacje. Nie musisz tworzyć odpowiednich aplikacji z wyprzedzeniem.

```
name: AzureSpringCloud
on: push

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
    
    - uses: actions/checkout@master
    
    - name: Set up JDK 1.8
      uses: actions/setup-java@v1
      with:
        java-version: 1.8
    
    - name: maven build, clean
      run: |
        mvn clean package -DskipTests
        
    # Maven plugin can cosume this authentication method automatically
    - name: Azure Login
      uses: azure/login@v1
      with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}
    
    # Maven deploy, make sure you have correct configurations in your pom.xml
    - name: deploy to Azure Spring Cloud using Maven
      run: |
        mvn azure-spring-cloud:deploy
```

## <a name="run-the-workflow"></a>Uruchamianie przepływu pracy
**Akcje** usługi GitHub powinny być włączane automatycznie po wypchnięciu `.github/workflow/main.yml` do serwisu GitHub. Akcja zostanie wyzwolona po wypchnięciu nowego zatwierdzenia. Jeśli utworzysz ten plik w przeglądarce, Twoja akcja powinna być już uruchomiona.

Aby sprawdzić, czy akcja została włączona, kliknij kartę **Akcje** na stronie repozytorium GitHub:

 ![Sprawdź, czy włączono akcję](./media/github-actions/actions3.png)

Jeśli akcja zostanie uruchomiona z błędem, na przykład jeśli nie ustawiono poświadczeń platformy Azure, można ponownie uruchomić testy po rozwiązaniu błędu. Na stronie repozytorium GitHub kliknij pozycję **Akcje**, wybierz określone zadanie przepływu pracy, a następnie kliknij przycisk **Uruchom ponownie testy** , aby ponownie uruchomić testy:

 ![Uruchom ponownie sprawdzenia](./media/github-actions/actions4.png)

## <a name="next-steps"></a>Następne kroki
* [Key Vault dla akcji GitHub w chmurze](./spring-cloud-github-actions-key-vault.md)
* [Azure Active Directory jednostek usługi](https://docs.microsoft.com/cli/azure/ad/sp?view=azure-cli-latest#az-ad-sp-create-for-rbac)
* [Akcje GitHub Actions dla platformy Azure](https://github.com/Azure/actions/)
