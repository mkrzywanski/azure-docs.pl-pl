---
title: Włączanie tożsamości zarządzanej przypisanej przez system dla aplikacji w usłudze Azure Spring Cloud
description: Jak włączyć tożsamość zarządzaną przypisaną przez system dla aplikacji.
author: MikeDodaro
ms.author: brendm
ms.service: spring-cloud
ms.topic: how-to
ms.date: 05/13/2020
ms.custom: devx-track-java
ms.openlocfilehash: 1b9d7326ec13176fbe65ba430a8a33bb93a48f74
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/23/2020
ms.locfileid: "87091457"
---
# <a name="how-to-enable-system-assigned-managed-identity-for-azure-spring-cloud-application"></a>Jak włączyć tożsamość zarządzaną przypisaną przez system dla aplikacji w chmurze platformy Azure
Zarządzane tożsamości dla zasobów platformy Azure zapewniają automatycznie zarządzaną tożsamość w Azure Active Directory do zasobów platformy Azure, takich jak aplikacja w chmurze ze sprężyną Azure. Tej tożsamości można użyć do uwierzytelniania w dowolnej usłudze, która obsługuje uwierzytelnianie usługi Azure AD, bez poświadczeń w kodzie.

W tym artykule pokazano, jak włączyć i wyłączyć zarządzane tożsamości przypisane do systemu dla aplikacji w chmurze platformy Azure przy użyciu Azure Portal i interfejsu wiersza polecenia (dostępne w wersji 0.2.4).

## <a name="prerequisites"></a>Wymagania wstępne
Jeśli nie znasz tożsamości zarządzanych dla zasobów platformy Azure, zobacz [sekcję Omówienie](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview).
Będziesz potrzebować wdrożonego wystąpienia chmury Azure wiosennej. Postępuj zgodnie z [przewodnikiem Szybki Start, aby wdrożyć za pomocą interfejsu wiersza polecenia platformy Azure](spring-cloud-quickstart-launch-app-cli.md).

## <a name="add-a-system-assigned-identity"></a>Dodawanie tożsamości przypisanej do systemu
Tworzenie aplikacji z tożsamością przypisaną do systemu wymaga ustawienia dodatkowej właściwości aplikacji.

### <a name="using-azure-portal"></a>Korzystanie z witryny Azure Portal
Aby skonfigurować tożsamość zarządzaną w [Azure Portal](https://portal.azure.com/), należy najpierw utworzyć aplikację, a następnie włączyć tę funkcję.

1. Utwórz aplikację w portalu, jak zwykle. Przejdź do niej w portalu.
2. Przewiń w dół do grupy **ustawień** w okienku nawigacji po lewej stronie.
3. Wybierz pozycję **tożsamość**.
4. W ramach karty **przypisanej do systemu** Przełącz pozycję **stan** na wartość *włączone*. Kliknij pozycję **Zapisz**.

 ![Tożsamość zarządzana w portalu](./media/spring-cloud-managed-identity/identity-1.png)

### <a name="using-azure-cli"></a>Korzystanie z interfejsu wiersza polecenia platformy Azure
Tożsamość zarządzaną przypisaną przez system można włączyć podczas tworzenia aplikacji lub w istniejącej aplikacji.

**Włącz tożsamość zarządzaną przypisaną przez system podczas tworzenia aplikacji**

Poniższy przykład tworzy aplikację o nazwie *APP_NAME* z tożsamością zarządzaną przez system, zgodnie z żądaniem `--assign-identity` parametru.

```azurecli
az spring-cloud app create -n app_name -s service_name -g resource_group_name --assign-identity
```

**Włącz tożsamość zarządzaną przypisaną przez system w istniejącej aplikacji** Użyj `az spring-cloud app identity assign` polecenia, aby włączyć tożsamość przypisaną przez system w istniejącej aplikacji.

```azurecli
az spring-cloud app identity assign -n app_name -s service_name -g resource_group_name
```

## <a name="obtain-tokens-for-azure-resources"></a>Uzyskaj tokeny dla zasobów platformy Azure
Aplikacja może używać swojej tożsamości zarządzanej, aby uzyskiwać tokeny umożliwiające dostęp do innych zasobów chronionych przez Azure Active Directory, takich jak Azure Key Vault. Te tokeny reprezentują aplikację próbującą uzyskać dostęp do zasobu, a nie określonego użytkownika aplikacji.

Może być konieczne [skonfigurowanie zasobu docelowego, aby zezwalać na dostęp z poziomu aplikacji](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/howto-assign-access-portal). Jeśli na przykład zażądano tokenu dostępu do Key Vault, upewnij się, że dodano zasady dostępu zawierające tożsamość aplikacji. W przeciwnym razie wywołania Key Vault będą odrzucane, nawet jeśli zawierają token. Aby dowiedzieć się więcej o tym, które zasoby obsługują tokeny Azure Active Directory, zobacz [usługi platformy Azure, które obsługują uwierzytelnianie usługi Azure AD](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/services-support-managed-identities#azure-services-that-support-azure-ad-authentication).

Chmura sprężynowa platformy Azure ma ten sam punkt końcowy do pozyskiwania tokenów z maszyną wirtualną platformy Azure. Zalecamy użycie zestawu SDK języka Java lub sprężyny uruchomieniowych rozruchu w celu uzyskania tokenu.  Zapoznaj [się z tematem jak używać tokenu maszyny wirtualnej](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/how-to-use-vm-token) , aby poznać różne przykłady kodu i skryptów oraz wskazówki dotyczące ważnych tematów, takich jak obsługa wygaśnięcia tokenu i błędów HTTP.

Zalecane: Aby uzyskać tokeny, należy użyć zestawu Java SDK lub sprężynowego uruchamiania.  Zapoznaj się z przykładami w [następnych krokach](#next-steps).

## <a name="disable-system-assigned-identity-from-an-app"></a>Wyłączanie tożsamości przypisanej do systemu z poziomu aplikacji
Usunięcie tożsamości przypisanej do systemu spowoduje również usunięcie jej z usługi Azure AD. Usunięcie zasobu aplikacji powoduje automatyczne usunięcie tożsamości przypisanych do systemu z usługi Azure AD.

### <a name="using-azure-portal"></a>Korzystanie z witryny Azure Portal
Aby usunąć tożsamość zarządzaną przypisaną przez system z aplikacji, która nie jest już potrzebna:

1. Zaloguj się do [Azure Portal](https://portal.azure.com/) przy użyciu konta skojarzonego z subskrypcją platformy Azure, która zawiera wystąpienie chmury Azure wiosennej.
1. Przejdź do odpowiedniej maszyny wirtualnej i wybierz pozycję **tożsamość**.
1. W **System assigned**obszarze / **stan**przypisane do systemu wybierz pozycję **wyłączone** , a następnie kliknij pozycję **Zapisz**:

 ![Tożsamość zarządzana w portalu](./media/spring-cloud-managed-identity/remove-identity.png)

### <a name="using-azure-cli"></a>Korzystanie z interfejsu wiersza polecenia platformy Azure
Aby usunąć tożsamość zarządzaną przypisaną przez system z aplikacji, która nie jest już potrzebna, użyj następującego polecenia:
```azurecli
az spring-cloud app identity remove -n app_name -s service_name -g resource_group_name
```

## <a name="next-steps"></a>Następne kroki
* [Jak używać tożsamości zarządzanych z zestawem SDK języka Java](https://github.com/Azure-Samples/Azure-Spring-Cloud-Samples)
* [Dostęp do Azure Key Vault z tożsamościami zarządzanymi w rozruchie sprężynowym Starter](https://github.com/Azure/azure-sdk-for-java/blob/master/sdk/spring/azure-spring-boot-starter-keyvault-secrets/README.md#use-msi--managed-identities)
* [Dowiedz się więcej o tożsamościach zarządzanych dla zasobów platformy Azure](https://github.com/MicrosoftDocs/azure-docs/blob/master/articles/active-directory/managed-identities-azure-resources/overview.md)

