---
title: 'Samouczek: Konfigurowanie powiadomień e-mail Apache Ambari w usłudze Azure HDInsight'
description: W tym artykule opisano sposób korzystania z usługi SendGrid z usługą Apache Ambari na potrzeby powiadomień e-mail.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: hrasheed
ms.service: hdinsight
ms.topic: tutorial
ms.date: 03/10/2020
ms.openlocfilehash: 21376eb40fb40abe67f7e03d15aabd7d89ea62f8
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/29/2020
ms.locfileid: "80082314"
---
# <a name="tutorial-configure-apache-ambari-email-notifications-in-azure-hdinsight"></a>Samouczek: Konfigurowanie powiadomień e-mail Apache Ambari w usłudze Azure HDInsight

W tym samouczku skonfigurujesz powiadomienia e-mail Apache Ambari za pomocą usługi SendGrid. Usługa [Apache Ambari](./hdinsight-hadoop-manage-ambari.md) upraszcza zarządzanie klastrem usługi HDInsight i ich monitorowanie, zapewniając łatwy w użyciu interfejs użytkownika sieci Web i interfejs API REST. Usługa Ambari jest dołączana do klastrów usługi HDInsight i służy do monitorowania klastra i wprowadzania zmian w konfiguracji. [SendGrid](https://sendgrid.com/solutions/) to bezpłatna usługa poczty e-mail oparta na chmurze, która zapewnia niezawodne dostarczanie transakcyjnych wiadomości e-mail, skalowalność i analizę w czasie rzeczywistym oraz elastyczne interfejsy API, które ułatwiają integrację niestandardową. W każdym miesiącu klienci platformy Azure mogą odblokować 25 000 bezpłatnych wiadomości e-mail.

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Uzyskaj nazwę użytkownika SendGrid
> * Konfigurowanie powiadomień e-mail Apache Ambari

## <a name="prerequisites"></a>Wymagania wstępne

* Konto e-mail SendGrid. Instrukcje można znaleźć w temacie [jak wysyłać pocztą E-mail przy użyciu SendGrid z platformą Azure](https://docs.microsoft.com/azure/sendgrid-dotnet-how-to-send-email) .

* HDInsight An klaster. Zobacz [Tworzenie klastrów Apache Hadoop przy użyciu Azure Portal](./hdinsight-hadoop-create-linux-clusters-portal.md).

## <a name="obtain-sendgrid-username"></a>Uzyskaj nazwę użytkownika SendGrid

1. W [Azure Portal](https://portal.azure.com)przejdź do zasobu SendGrid.

1. Na stronie Przegląd wybierz pozycję **Zarządzaj**, aby przejść do strony sieci Web SendGrid dla Twojego konta.

    ![Omówienie SendGrid w witrynie Azure Portal](./media/apache-ambari-email/azure-portal-sendgrid-manage.png)

1. W menu po lewej stronie przejdź do swojej nazwy konta, a następnie **szczegóły konta**.

    ![Nawigacja pulpitu nawigacyjnego SendGrid](./media/apache-ambari-email/sendgrid-dashboard-navigation.png)

1. Na stronie **szczegóły konta** Zapisz **nazwę użytkownika**.

    ![Szczegóły konta SendGrid](./media/apache-ambari-email/sendgrid-account-details.png)

## <a name="configure-ambari-e-mail-notification"></a>Konfigurowanie powiadomienia e-mail Ambari

1. W przeglądarce sieci Web przejdź do `https://CLUSTERNAME.azurehdinsight.net/#/main/alerts`lokalizacji, gdzie `CLUSTERNAME` jest nazwą klastra.

1. Z listy rozwijanej **Akcje** wybierz pozycję **Zarządzaj powiadomieniami**.

1. W oknie **Zarządzanie powiadomieniami o alertach** wybierz **+** ikonę.

    ![Ambari — powiadomienie o utworzeniu alertu](./media/apache-ambari-email/azure-portal-create-notification.png)

1. W oknie dialogowym **Tworzenie powiadomienia o alercie** podaj następujące informacje:

    |Właściwość |Opis |
    |---|---|
    |Nazwa|Podaj nazwę powiadomienia.|
    |Grupy|Skonfiguruj je zgodnie z potrzebami.|
    |Ważność|Skonfiguruj je zgodnie z potrzebami.|
    |Opis|Opcjonalny.|
    |Metoda|Pozostaw **wiadomość e-mail**.|
    |Wyślij wiadomość e-mail do|Podaj wiadomości e-mail na potrzeby otrzymywania powiadomień, rozdzielając je przecinkami.|
    |Serwer SMTP|`smtp.sendgrid.net`|
    |Port SMTP|25 lub 587 (dla nieszyfrowanych/TLS połączeń).|
    |Wyślij wiadomość e-mail z|Podaj adres e-mail. Adres nie musi być autentyczny.|
    |Użyj uwierzytelniania|Zaznacz to pole wyboru.|
    |Nazwa użytkownika|Podaj nazwę użytkownika SendGrid.|
    |Hasło|Podaj hasło, które zostało użyte podczas tworzenia zasobu SendGrid na platformie Azure.|
    |Potwierdzenie hasła|Ponownie wprowadź hasło.|
    |Uruchom protokół TLS|Zaznacz to pole wyboru|

    ![Ambari — powiadomienie o utworzeniu alertu](./media/apache-ambari-email/ambari-create-alert-notification.png)

    Wybierz pozycję **Zapisz**. Powrócisz do okna **Zarządzanie powiadomieniami o alertach** .

1. W oknie **Zarządzanie powiadomieniami o alertach** wybierz pozycję **Zamknij**.

## <a name="next-steps"></a>Następne kroki

W tym samouczku przedstawiono sposób konfigurowania powiadomień e-mail Apache Ambari za pomocą usługi SendGrid. Aby dowiedzieć się więcej o platformie Apache Ambari, Skorzystaj z następujących informacji:

* [Zarządzanie klastrami HDInsight przy użyciu internetowego interfejsu użytkownika systemu Apache Ambari](./hdinsight-hadoop-manage-ambari.md)

* [Tworzenie powiadomienia o alercie](https://docs.cloudera.com/HDPDocuments/Ambari-latest/managing-and-monitoring-ambari/content/amb_create_an_alert_notification.html)
