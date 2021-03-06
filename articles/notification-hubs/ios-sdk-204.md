---
title: Wysyłanie powiadomień wypychanych do systemu iOS przy użyciu usługi Azure Notification Hubs i zestawu iOS SDK w wersji 2.0.4
description: W tym samouczku dowiesz się, jak wysyłać powiadomienia wypychane do urządzeń z systemem iOS przy użyciu usługi Azure Notification Hubs i usług powiadomień wypychanych firmy Apple.
author: sethmanheim
ms.author: sethm
ms.date: 06/19/2020
ms.topic: tutorial
ms.service: notification-hubs
ms.reviewer: thsomasu
ms.lastreviewed: 06/01/2020
ms.openlocfilehash: 80a9a98043a39b654f0d62a84ae81e12797b15b9
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/23/2020
ms.locfileid: "87022365"
---
# <a name="tutorial-send-push-notifications-to-ios-apps-using-azure-notification-hubs-version-204"></a>Samouczek: wysyłanie powiadomień wypychanych do aplikacji systemu iOS przy użyciu usługi Azure Notification Hubs (wersja 2.0.4)

W tym samouczku pokazano, jak za pomocą usługi Azure Notification Hubs wysyłać powiadomienia wypychane do aplikacji dla systemu iOS przy użyciu usługi Azure Notification Hubs SDK w wersji 2.0.4.

Ten samouczek obejmuje następujące kroki:

- Utwórz przykładową aplikację dla systemu iOS.
- Połącz aplikację systemu iOS z usługą Azure Notification Hubs.
- Wysyłanie testowych powiadomień wypychanych.
- Sprawdź, czy aplikacja otrzymuje powiadomienia.

Możesz pobrać kompletny kod dla tego samouczka [z usługi GitHub](https://github.com/Azure/azure-notificationhubs-ios/tree/v3-preview2/Samples).

## <a name="prerequisites"></a>Wymagania wstępne

Do ukończenia tego samouczka potrzebne są następujące wymagania wstępne:

- Na komputerze Mac z systemem [Xcode](https://go.microsoft.com/fwLink/p/?LinkID=266532), wraz z prawidłowym certyfikatem dewelopera zainstalowanym w łańcuchu kluczy.
- IPhone lub iPad z systemem iOS w wersji 10 lub nowszej.
- Urządzenie fizyczne zarejestrowane w [portalu firmy Apple](https://developer.apple.com/)i skojarzone z Twoim certyfikatem.

Przed kontynuowaniem przejdź do poprzedniego samouczka na temat rozpoczynania pracy z [usługą Azure Notification Hubs dla aplikacji systemu iOS](ios-sdk-get-started.md), aby skonfigurować i skonfigurować poświadczenia wypychania w centrum powiadomień. Nawet jeśli nie masz doświadczenia w tworzeniu systemu iOS, należy wykonać następujące czynności.

> [!NOTE]
> Ze względu na wymagania dotyczące konfiguracji powiadomień wypychanych należy wdrożyć i przetestować powiadomienia wypychane na fizycznym urządzeniu z systemem iOS (telefonie iPhone lub tablecie iPad), a nie emulatorze systemu iOS.

## <a name="connect-your-ios-app-to-notification-hubs"></a>Łączenie aplikacji dla systemu iOS z usługą Notification Hubs

1. W Xcode Utwórz nowy projekt dla systemu iOS i wybierz szablon **aplikacji pojedynczego widoku**   .

   :::image type="content" source="media/ios-sdk/image1.png" alt-text="Wybierz szablon":::

2. Podczas ustawiania opcji dla nowego projektu upewnij się, że używasz tej samej **nazwy produktu**   i **identyfikatora organizacji**,   która została użyta podczas ustawiania identyfikatora pakietu w portalu dla deweloperów firmy Apple.

3. W obszarze Projekt nawigatora wybierz nazwę projektu w obszarze **obiekty docelowe**, a następnie wybierz kartę **możliwości podpisywania &**   . Upewnij się, że wybrano odpowiedni **zespół**   dla konta dewelopera firmy Apple. Środowisko Xcode powinno automatycznie ściągnąć utworzony wcześniej profil aprowizowania na podstawie identyfikatora pakietu.

   Jeśli nowy profil aprowizowania utworzony w programie Xcode nie jest widoczny, odśwież profile dla Twojej tożsamości podpisywania. Na pasku menu kliknij pozycję **Xcode**   , kliknij pozycję **Preferencje**, kliknij **Account**   kartę konto, kliknij przycisk **Wyświetl szczegóły**   , kliknij swoją tożsamość podpisującą, a następnie kliknij przycisk Odśwież w prawym dolnym rogu.

   :::image type="content" source="media/ios-sdk/image2.png" alt-text="Wyświetl szczegóły":::

4. Na karcie **możliwości & podpisywania**   Wybierz pozycję **+ możliwość**. Kliknij dwukrotnie pozycję **powiadomienia wypychane**   , aby ją włączyć.

   :::image type="content" source="media/ios-sdk/image3.png" alt-text="Funkcja":::

5. Dodaj moduły zestawu Azure Notification Hubs SDK.

   Zestaw SDK platformy Azure Notification Hubs można zintegrować z aplikacją za pomocą [Cocoapods](https://cocoapods.org/)   lub ręcznie dodając pliki binarne do projektu.

   - Integracja za pośrednictwem Cocoapods: Dodaj następujące zależności do plik podfile, aby uwzględnić w aplikacji usługę Azure Notification Hubs SDK:

      ```ruby
      pod 'AzureNotificationHubs-iOS'
      ```

      - Uruchom instalację pod, aby zainstalować nowo zdefiniowany pod i otworzyć xcworkspace.

         Jeśli zobaczysz błąd, na przykład **nie można znaleźć specyfikacji dla AzureNotificationHubs-iOS** podczas instalacji pod instalacją, uruchom polecenie, `pod repo update` Aby uzyskać najnowsze Zasobniki z repozytorium Cocoapods, a następnie uruchom instalację pod kontrolą.

   - Integracja za pośrednictwem Carthage: Dodaj następujące zależności do Cartfile, aby uwzględnić zestaw Azure Notification Hubs SDK w aplikacji:

      ```ruby
      github "Azure/azure-notificationhubs-ios"
      ```

      - Następnie zaktualizuj zależności kompilacji:

      ```shell
      $ carthage update
      ```

      Aby uzyskać więcej informacji na temat korzystania z programu Carthage, zobacz [repozytorium GitHub Carthage](https://github.com/Carthage/Carthage).

   - Integracja przez kopiowanie plików binarnych do projektu: można przeprowadzić integrację, kopiując pliki binarne do projektu w następujący sposób:

        - Pobierz platformę [zestawu SDK usługi Azure Notification Hubs](https://github.com/Azure/azure-notificationhubs-android/releases)w   postaci pliku zip i rozpakuj ją.

        - W Xcode kliknij prawym przyciskiem myszy projekt i kliknij opcję **Dodaj pliki do**,   Aby dodać folder **WindowsAzureMessaging. Framework**   do projektu Xcode. Wybierz opcję **Opcje**   i upewnij się, że opcja **Kopiuj elementy w razie konieczności**   jest zaznaczona, a następnie kliknij przycisk **Dodaj**.

          :::image type="content" source="media/ios-sdk/image4.png" alt-text="Dodaj strukturę":::

6. Dodaj nowy plik nagłówkowy do projektu o nazwie **stałychs. h**. Aby to zrobić, kliknij prawym przyciskiem myszy nazwę projektu i wybierz pozycję **nowy plik.**... Następnie wybierz pozycję **plik nagłówka**. Ten plik zawiera stałe używane przez centrum powiadomień. Następnie wybierz przycisk **dalej**. Nazwij plik **stałe. h**.

7. Dodaj następujący kod do pliku stałych. h:

   ```objc
   #ifndef Constants_h
   #define Constants_h
   extern NSString* const NHInfoConnectionString;
   extern NSString* const NHInfoHubName;
   extern NSString* const NHUserDefaultTags;
   #endif /* Constants_h */
   ```

8. Dodaj plik implementacji dla stałych. h. Aby to zrobić, kliknij prawym przyciskiem myszy nazwę projektu i wybierz pozycję **nowy plik.**... Wybierz pozycję **plik zamierzenia C**, a następnie wybierz przycisk **dalej**. Nazwij stałe plik **. m**.

   :::image type="content" source="media/ios-sdk/image5.png" alt-text="Dodaj plik implementacji":::

9. Otwórz plik **stałe. m**   i Zastąp jego zawartość następującym kodem. Zastąp symbole zastępcze literałów ciągów  `NotificationHubConnectionString`   i  `NotificationHubConnectionString`   odpowiednio nazwą centrum i **DefaultListenSharedAccessSignature**, jak poprzednio uzyskano z portalu:

   ```objc
   #import <Foundation/Foundation.h>
   #import "Constants.h"

   NSString* const NHInfoConnectionString = @"NotificationHubConnectionString";
   NSString* const NHInfoHubName = @"NotificationHubName";NSString* const NHUserDefaultTags = @"notification_tags";
   ```

10. Otwórz plik projektu **AppDelegate. h**   i Zastąp jego zawartość następującym kodem:

    ```objc
    #import <UIKit/UIKit.h>
    #import <WindowsAzureMessaging/WindowsAzureMessaging.h>
    #import <UserNotifications/UserNotifications.h> 

    @interface AppDelegate : UIResponder <UIApplicationDelegate,   UNUserNotificationCenterDelegate>

    @property (strong, nonatomic) UIWindow *window;

    - (void)handleRegister;
    - (void)handleUnregister;

    @end
    ```

11. W pliku **AppDelegate. m**projektu   Dodaj następujące  `import`   instrukcje:

    ```objc
    #import "Constants.h"
    #import "NotificationDetailViewController.h"
    ```

12. Również w pliku **AppDelegate. m**   Dodaj następujący wiersz kodu w  `didFinishLaunchingWithOptions`   metodzie, w zależności od używanej wersji systemu iOS. Ten kod rejestruje dojście do urządzenia w usłudze APNs:

    ```objc
    [[UNUserNotificationCenter currentNotificationCenter] setDelegate:self];
    ```

13. W tym samym pliku **AppDelegate. m**   Zastąp cały kod następującym kodem  `didFinishLaunchingWithOptions`   :

    ```objc
    // Tells the app that a remote notification arrived that indicates there is data to be fetched.

    - (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo fetchCompletionHandler:(void (^)(UIBackgroundFetchResult result))completionHandler {
    NSLog(@"Received remote (silent) notification");
    [self logNotificationDetails:userInfo];

    //
    // Let the system know the silent notification has been processed.
    //
    completionHandler(UIBackgroundFetchResultNoData);
    }

    // Tells the delegate that the app successfully registered with Apple Push Notification service (APNs).

    - (void) application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken {
    NSMutableSet *tags = [[NSMutableSet alloc] init];

    // Load and parse stored tags
    NSString *unparsedTags = [[NSUserDefaults standardUserDefaults] valueForKey:NHUserDefaultTags];
    if (unparsedTags.length > 0) {
        NSArray *tagsArray = [unparsedTags componentsSeparatedByString: @","];
        [tags addObjectsFromArray:tagsArray];
    }

    // Register the device with the Notification Hub.
    // If the device has not already been registered, this will create the registration.
    // If the device has already been registered, this will update the existing registration.
    //
    SBNotificationHub* hub = [self getNotificationHub];
    [hub registerNativeWithDeviceToken:deviceToken tags:tags completion:^(NSError* error) {
        if (error != nil) {
            NSLog(@"Error registering for notifications: %@", error);
        } else {
            [self showAlert:@"Registered" withTitle:@"Registration Status"];
        }
    }];
    }

    // UNUserNotificationCenterDelegate methods
    //
    // Asks the delegate how to handle a notification that arrived while the app was running in the  foreground.

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler {
    NSLog(@"Received notification while the application is in the foreground");

    // The system calls this delegate method when the app is in the foreground. This allows the app to handle the notification
    // itself (and potentially modify the default system behavior).

    // Handle the notification by displaying custom UI.
    //
    [self showNotification:notification.request.content.userInfo];

    // Use 'options' to specify which default behaviors to enable.
    // - UNAuthorizationOptionBadge: Apply the notification's badge value to the app’s icon.
    // - UNAuthorizationOptionSound: Play the sound associated with the notification.
    // - UNAuthorizationOptionAlert: Display the alert using the content provided by the notification.
    //
    // In this case, do not pass UNAuthorizationOptionAlert because the notification was handled by the app.
    //
    completionHandler(UNAuthorizationOptionBadge | UNAuthorizationOptionSound);
    }

    // Asks the delegate to process the user's response to a delivered notification.
    //

    - (void)userNotificationCenter:(UNUserNotificationCenter *)center didReceiveNotificationResponse:(UNNotificationResponse *)response withCompletionHandler:(void(^)(void))completionHandler {
    NSLog(@"Received notification while the application is in the background");

    // The system calls this delegate method when the user taps or responds to the system notification.

    // Handle the notification response by displaying custom UI
    //
    [self showNotification:response.notification.request.content.userInfo];

    // Let the system know the response has been processed.
    //
    completionHandler();
    }

    // App logic and helpers

    - (SBNotificationHub *)getNotificationHub {
    NSString *hubName = [[NSBundle mainBundle] objectForInfoDictionaryKey:NHInfoHubName];
    NSString *connectionString = [[NSBundle mainBundle] objectForInfoDictionaryKey:NHInfoConnectionString];

    return [[SBNotificationHub alloc] initWithConnectionString:connectionString notificationHubPath:hubName];
    }

    - (void)handleRegister {
    UNUserNotificationCenter *center = [UNUserNotificationCenter currentNotificationCenter];

    UNAuthorizationOptions options =  UNAuthorizationOptionAlert | UNAuthorizationOptionSound | UNAuthorizationOptionBadge;
    [center requestAuthorizationWithOptions:(options) completionHandler:^(BOOL granted, NSError * _Nullable error) {
        if (error != nil) {
            NSLog(@"Error requesting for authorization: %@", error);
        }
    }];
    [[UIApplication sharedApplication] registerForRemoteNotifications];
    }

    - (void)handleUnregister {
    //
    // Unregister the device with the Notification Hub.
    //
    SBNotificationHub *hub = [self getNotificationHub];
    [hub unregisterNativeWithCompletion:^(NSError* error) {
        if (error != nil) {
            NSLog(@"Error unregistering for push: %@", error);
        } else {
            [self showAlert:@"Unregistered" withTitle:@"Registration Status"];
        }
    }];
    }

    - (void)logNotificationDetails:(NSDictionary *)userInfo {
    if (userInfo != nil) {
        UIApplicationState state = [UIApplication sharedApplication].applicationState;
        BOOL background = state != UIApplicationStateActive;
        NSLog(@"Received %@notification: \n%@", background ? @"(background) " : @"", userInfo);
    }
    }

    - (void)showAlert:(NSString *)message withTitle:(NSString *)title {
    if (title == nil) {
        title = @"Alert";
    }
    UIAlertController *alert = [UIAlertController alertControllerWithTitle:title message:message preferredStyle:UIAlertControllerStyleAlert];
    [alert addAction:[UIAlertAction actionWithTitle:@"OK" style:UIAlertActionStyleDefault handler:nil]];
    [[[[UIApplication sharedApplication] keyWindow] rootViewController] presentViewController:alert animated:YES completion:nil];
    }

    - (void)showNotification:(NSDictionary *)userInfo {
    [self logNotificationDetails:userInfo];

    NotificationDetailViewController *notificationDetail = [[NotificationDetailViewController alloc] initWithUserInfo:userInfo];
    [[[[UIApplication sharedApplication] keyWindow] rootViewController] presentViewController:notificationDetail animated:YES completion:nil];
    }

    @end
    ```

    Ten kod nawiązuje połączenie z centrum powiadomień przy użyciu informacji o połączeniu określonych w polu **stałe. h**. Następnie przekazuje token urządzenia do centrum powiadomień, aby umożliwić wysyłanie powiadomień przez centrum.

### <a name="create-notificationdetailviewcontroller-header-file"></a>Utwórz plik nagłówka NotificationDetailViewController

1. Podobnie jak w poprzednich instrukcjach, Dodaj kolejny plik nagłówka o nazwie **NotificationDetailViewController. h**. Zastąp zawartość nowego pliku nagłówka następującym kodem:

   ```objc
   #import <UIKit/UIKit.h>

   NS_ASSUME_NONNULL_BEGIN

   @interface NotificationDetailViewController : UIViewController

   @property (strong, nonatomic) IBOutlet UILabel *titleLabel;
   @property (strong, nonatomic) IBOutlet UILabel *bodyLabel;
   @property (strong, nonatomic) IBOutlet UIButton *dismissButton;

   @property (strong, nonatomic) NSDictionary *userInfo;

   - (id)initWithUserInfo:(NSDictionary *)userInfo;

   @end

   NS_ASSUME_NONNULL_END
   ```

2. Dodaj plik implementacji **NotificationDetailViewController. m**. Zastąp zawartość pliku następującym kodem, który implementuje metody UIViewController:

   ```objc
   #import "NotificationDetailViewController.h"

   @interface NotificationDetailViewController ()

   @end

   @implementation NotificationDetailViewController

   - (id)initWithUserInfo:(NSDictionary *)userInfo {
    self = [super initWithNibName:@"NotificationDetail" bundle:nil];
    if (self) {
        _userInfo = userInfo;
    }
    return self;
   }

   - (void)viewDidLayoutSubviews {
    [self.titleLabel sizeToFit];
    [self.bodyLabel sizeToFit];
   }

   - (void)viewDidLoad {
    [super viewDidLoad];

    NSString *title = nil;
    NSString *body = nil;

    NSDictionary *aps = [_userInfo valueForKey:@"aps"];
    NSObject *alertObject = [aps valueForKey:@"alert"];
    if (alertObject != nil) {
        if ([alertObject isKindOfClass:[NSDictionary class]]) {
            NSDictionary *alertDict = (NSDictionary *)alertObject;
            title = [alertDict valueForKey:@"title"];
            body = [alertObject valueForKey:@"body"];
        } else if ([alertObject isKindOfClass:[NSString class]]) {
            body = (NSString *)alertObject;
        } else {
            NSLog(@"Unable to parse notification content. Unexpected format: %@", alertObject);
        }
    }

    if (title == nil) {
        title = @"<unset>";
    }

    if (body == nil) {
        body = @"<unset>";
    }

    self.titleLabel.text = title;
    self.bodyLabel.text = body;
   }

   - (IBAction)handleDismiss:(id)sender {
    [self dismissViewControllerAnimated:YES completion:nil];
   }

   @end
   ```

### <a name="viewcontroller"></a>Plik viewcontroller

1. W pliku Project **plik viewcontroller. h**   Dodaj następujące  `import`   instrukcje:

   ```objc
   #import <WindowsAzureMessaging/WindowsAzureMessaging.h>
   #import <UserNotifications/UserNotifications.h>
   ```

2. Również w **plik viewcontroller. h**Dodaj następujące deklaracje właściwości po  `@interface`   deklaracji:

   ```objc
   @property (strong, nonatomic) IBOutlet UITextField *tagsTextField;
   @property (strong, nonatomic) IBOutlet UIButton *registerButton;
   @property (strong, nonatomic) IBOutlet UIButton *unregisterButton;
   ```

3. W pliku implementacji **plik viewcontroller. m**projektu   Zastąp zawartość pliku następującym kodem:

   ```objc
   #import "ViewController.h"
   #import "Constants.h"
   #import "AppDelegate.h"

   @interface ViewController ()

   @end

   @implementation ViewController

   // UIViewController methods

   - (void)touchesBegan:(NSSet<UITouch *> *)touches withEvent:(UIEvent *)event {
    // Simple method to dismiss keyboard when user taps outside of the UITextField.
    [self.view endEditing:YES];
   }

   - (void)viewDidLoad {
    [super viewDidLoad];

    // Load raw tags text from storage and initialize the text field
    self.tagsTextField.text = [[NSUserDefaults standardUserDefaults] valueForKey:NHUserDefaultTags];
   }

   - (IBAction)handleRegister:(id)sender {
    // Save raw tags text in storage
    [[NSUserDefaults standardUserDefaults] setValue:self.tagsTextField.text forKey:NHUserDefaultTags];

   // Delegate processing the register action to the app delegate.
   [[[UIApplication sharedApplication] delegate] performSelector:@selector(handleRegister)];
   }

   - (IBAction)handleUnregister:(id)sender {
    [[[UIApplication sharedApplication] delegate] performSelector:@selector(handleUnregister)];
   }

   @end
   ```

4. Skompiluj i uruchom aplikację na urządzeniu, aby upewnić się, że nie występują żadne błędy.

## <a name="send-test-push-notifications"></a>Wysyłanie testowych powiadomień wypychanych

Możesz testować otrzymywanie powiadomień w aplikacji za pomocą opcji **Wyślij test**   w [Azure Portal](https://portal.azure.com/). Powoduje to wysłanie testowego powiadomienia push na urządzenie.

:::image type="content" source="media/ios-sdk/image6.png" alt-text="Wysyłanie testowe":::

Powiadomienia wypychane są zwykle wysyłane za pośrednictwem usługi wewnętrznej bazy danych, takiej jak Mobile Apps czy ASP.NET, przy użyciu zgodnej biblioteki. Jeśli biblioteka nie jest dostępna dla zaplecza, można także użyć interfejsu API REST bezpośrednio do wysyłania komunikatów powiadomień.

Poniżej znajduje się lista innych samouczków, które warto przejrzeć w celu wysłania powiadomień:

- Azure Mobile Apps: Aby zapoznać się z przykładem wysyłania powiadomień z Mobile Apps zaplecza zintegrowanego z usługą Notification Hubs, zobacz [Dodawanie powiadomień wypychanych do aplikacji systemu iOS](/azure/app-service-mobile/app-service-mobile-ios-get-started-push).
- ASP.NET: [użyj Notification Hubs, aby wysyłać powiadomienia wypychane do użytkowników](notification-hubs-aspnet-backend-ios-apple-apns-notification.md).
- Azure Notification Hubs Java SDK: zobacz [, jak używać Notification Hubs języka Java](notification-hubs-java-push-notification-tutorial.md)   do wysyłania powiadomień z języka Java. To rozwiązanie przetestowano w programie Eclipse pod kątem tworzenia aplikacji dla systemu Android.
- PHP: [jak używać Notification Hubs w języku PHP](notification-hubs-php-push-notification-tutorial.md).

## <a name="verify-that-your-app-receives-push-notifications"></a>Sprawdzanie, czy aplikacja odbiera powiadomienia push

Aby przetestować powiadomienia wypychane w systemie iOS, należy wdrożyć aplikację na fizycznym urządzeniu z systemem iOS. Powiadomień wypychanych firmy Apple nie można wysyłać za pomocą symulatora systemu iOS.

1. Uruchom aplikację i sprawdź, czy rejestracja zakończyła się pomyślnie, a następnie naciśnij przycisk **OK**.

   :::image type="content" source="media/ios-sdk/image7.png" alt-text="Zarejestruj":::

2. Następnie Wyślij testowe Powiadomienie wypychane z [Azure Portal](https://portal.azure.com/), zgodnie z opisem w poprzedniej sekcji.

3. Powiadomienie wypychane jest wysyłane do wszystkich urządzeń zarejestrowanych w celu otrzymywania powiadomień z danego centrum powiadomień.

   :::image type="content" source="media/ios-sdk/image8.png" alt-text="Wyślij test":::

## <a name="next-steps"></a>Następne kroki

W tym prostym przykładzie wysłano powiadomienia wypychane do wszystkich zarejestrowanych urządzeń z systemem iOS. Aby dowiedzieć się, jak wysyłać powiadomienia wypychane do określonych urządzeń z systemem iOS, przejdź do następującego samouczka:

[Samouczek: powiadomienia wypychane do określonych urządzeń](notification-hubs-ios-xplat-segmented-apns-push-notification.md)

Aby uzyskać więcej informacji, zobacz następujące artykuły:

- [Omówienie usługi Azure Notification Hubs](notification-hubs-push-notification-overview.md)
- [Interfejsy API REST Notification Hubs](/rest/api/notificationhubs/)
- [Notification Hubs SDK dla operacji zaplecza](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)
- [Notification Hubs SDK w witrynie GitHub](https://github.com/Azure/azure-notificationhubs)
- [Rejestrowanie przy użyciu zaplecza aplikacji](notification-hubs-ios-aspnet-register-user-from-backend-to-push-notification.md)
- [Zarządzanie rejestracją](notification-hubs-push-notification-registration-management.md)
- [Praca ze znacznikami](notification-hubs-tags-segment-push-message.md)
- [Praca z szablonami niestandardowymi](notification-hubs-templates-cross-platform-push-messages.md)
- [Service Bus kontroli dostępu z sygnaturami dostępu współdzielonego](../service-bus-messaging/service-bus-sas.md)
- [Programowe generowanie tokenów SAS](/rest/api/eventhub/generate-sas-token)
- [Zabezpieczenia firmy Apple: typowe Kryptografia](https://developer.apple.com/security/)
- [Czas epoki systemu UNIX](https://en.wikipedia.org/wiki/Unix_time)
- [FUNKCJE](https://en.wikipedia.org/wiki/HMAC)
