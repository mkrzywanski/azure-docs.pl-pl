---
title: Współpraca z bazą wiedzy — QnA Maker
description: QnA Maker umożliwia wielu osobom współpracę w bazie wiedzy. Ta funkcja jest dostępna w ramach kontroli dostępu opartej na rolach platformy Azure (Azure RBAC).
ms.topic: conceptual
ms.date: 07/13/2020
ms.openlocfilehash: 20eb58f346322cb78bff85af3d6a0d366090763d
ms.sourcegitcommit: cee72954f4467096b01ba287d30074751bcb7ff4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/30/2020
ms.locfileid: "87446938"
---
# <a name="collaboration-with-authors-and-editors"></a>Współpraca z autorami i edytorami

Na poziomie zasobów QnA Maker jest dostępna współpraca umożliwiająca ograniczenie dostępu współpracowników na podstawie roli współpracownika. Dowiedz się więcej o [zagadnieniach związanych](../Concepts/role-based-access-control.md)z uwierzytelnianiem QNA Maker współpracownikami.

## <a name="add-role-based-access-rbac-to-your-qna-maker-resource"></a>Dodawanie dostępu opartego na rolach (RBAC) do zasobu QnA Maker

QnA Maker umożliwia wielu osobom współpracę nad wszystkimi bazami wiedzy w tym samym QnA Maker zasobach. Ta funkcja jest dostępna z [Access Control opartych na rolach](../../../active-directory/role-based-access-control-configure.md)platformy Azure.

## <a name="access-at-the-qna-maker-resource-level"></a>Dostęp na poziomie zasobów QnA Maker

Nie można udostępnić konkretnej bazy wiedzy w usłudze QnA Maker. Jeśli potrzebujesz bardziej szczegółowej kontroli dostępu, rozważ rozmieszczenie baz wiedzy w różnych QnA Maker zasobach, a następnie Dodaj role do każdego zasobu.

## <a name="add-role-to-resource"></a>Dodawanie roli do zasobu

### <a name="add-a-user-account-to-the-qna-maker-resource"></a>Dodawanie konta użytkownika do zasobu QnA Maker

Poniższe kroki wykorzystują rolę współpracownika, ale dowolne [role](../reference-role-based-access-control.md) można dodać przy użyciu tych kroków

1. Zaloguj się do witryny [Azure](https://portal.azure.com/) Portal i przejdź do zasobu QNA Maker.

    ![Lista zasobów QnA Maker](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-resource-list.png)

1. Przejdź do karty **Access Control (IAM)** .

    ![QnA Maker IAM](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam.png)

1. Wybierz pozycję **Dodaj**.

    ![Dodawanie QnA Maker IAM](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam-add.png)

1. Wybierz rolę z następującej listy:

    |Rola|
    |--|
    |Właściciel|
    |Współautor|
    |Cognitive Services QnA Maker Reader|
    |Edytor QnA Maker Cognitive Services|
    |Cognitive Services użytkownika|

    :::image type="content" source="../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-add-role-iam.png" alt-text="QnA Maker Dodaj rolę w usłudze IAM.":::

1. Wprowadź adres e-mail użytkownika i naciśnij pozycję **Zapisz**.

    ![QnA Maker Dodaj adres e-mail w usłudze IAM](../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-iam-add-email.png)

### <a name="view-qna-maker-knowledge-bases"></a>Wyświetl QnA Maker bazy wiedzy

Gdy osoba, do której udostępniono usługę QnA Maker, loguje się do [portalu QNA Maker](https://qnamaker.ai), zobaczy wszystkie bazy wiedzy w tej usłudze na podstawie ich roli.

Po wybraniu bazy wiedzy jej bieżąca rola dla tego zasobu QnA Maker będzie widoczna obok nazwy bazy wiedzy.

:::image type="content" source="../media/qnamaker-how-to-collaborate-knowledge-base/qnamaker-knowledge-base-role-name.png" alt-text="Zrzut ekranu bazy wiedzy w trybie edycji z nazwą roli w nawiasach obok nazwy bazy wiedzy w lewym górnym rogu strony sieci Web.":::

## <a name="next-steps"></a>Następne kroki

> [!div class="nextstepaction"]
> [Testowanie bazy wiedzy](./test-knowledge-base.md)

Dowiedz się więcej o współpracy:
* Kontrola dostępu oparta na rolach na [platformie Azure](../../../active-directory/role-based-access-control-configure.md)
* QnA Maker [koncepcji](../Concepts/role-based-access-control.md) kontroli dostępu opartej na rolach
