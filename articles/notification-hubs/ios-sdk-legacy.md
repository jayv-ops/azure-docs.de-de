---
title: Senden von Pushbenachrichtigungen an iOS mit Azure Notification Hubs und der iOS SDK-Version 2.0.4
description: In diesem Tutorial erfahren Sie, wie Sie mithilfe von Azure Notification Hubs und dem Apple Push Notification Service Pushbenachrichtigungen an iOS-Geräte senden (Version 2.0.4).
author: sethmanheim
ms.author: sethm
ms.date: 06/19/2020
ms.topic: tutorial
ms.service: notification-hubs
ms.reviewer: thsomasu
ms.lastreviewed: 06/01/2020
ms.openlocfilehash: a1017f7c7aa0ede9e3c91acd3dba510618e15fb6
ms.sourcegitcommit: e559daa1f7115d703bfa1b87da1cf267bf6ae9e8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/17/2021
ms.locfileid: "100597979"
---
# <a name="tutorial-send-push-notifications-to-ios-apps-using-azure-notification-hubs-legacy-api"></a>Tutorial: Senden von Pushbenachrichtigungen an iOS-Apps mithilfe von Azure Notification Hubs (Legacy-API)

In diesem Tutorial erfahren Sie, wie Sie mithilfe von Azure Notification Hubs und den Legacy-APIs des Azure Notification Hubs SDK für Apple Pushbenachrichtigungen an eine iOS-Anwendung senden.

Dieses Tutorial enthält die folgenden Schritte:

- Erstellen einer iOS-Beispiel-App
- Verbinden der iOS-App mit Azure Notification Hubs
- Senden von Pushbenachrichtigungen zu Testzwecken
- Überprüfen, ob die App Benachrichtigungen empfängt

Den vollständigen Code für dieses Tutorial können Sie von [GitHub](https://github.com/Azure/azure-notificationhubs-ios/tree/main/SampleNHAppLegacyObjC) herunterladen.

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

- Einen Mac mit [Xcode](https://go.microsoft.com/fwLink/p/?LinkID=266532) zusammen mit einem gültigen in Ihrer Keychain installierten Entwicklerzertifikat.
- Ein iPhone oder iPad, auf dem iOS Version 10 oder höher ausgeführt wird.
- Registrierung Ihres physischen Geräts im [Apple-Portal](https://developer.apple.com/) und Zuordnung zu Ihrem Zertifikat.

Bevor Sie fortfahren, vergewissern Sie sich, dass Sie das vorherige Tutorial zu den ersten Schritten mit [Azure Notification Hubs für iOS-Apps](ios-sdk-get-started.md) durchlaufen und Pushanmeldeinformationen in Ihrem Notification Hub eingerichtet und konfiguriert haben. Selbst wenn Sie bisher keine Erfahrung mit iOS-Entwicklung haben, sollten Sie diesen Anweisungen folgen können.

> [!NOTE]
> Pushbenachrichtigungen müssen aufgrund von Konfigurationsanforderungen auf einem physischen iOS-Gerät (iPhone oder iPad) anstatt im iOS-Emulator bereitgestellt und getestet werden.

## <a name="connect-your-ios-app-to-notification-hubs"></a>Verbinden der iOS-App mit Notification Hubs

1. Erstellen Sie in Xcode ein neues iOS-Projekt, und wählen Sie die Vorlage **Single View Application** aus.

   :::image type="content" source="media/ios-sdk/image1.png" alt-text="Auswählen einer Vorlage":::

2. Achten Sie beim Festlegen der Optionen für das neue Projekt darauf, dass Sie den gleichen **Produktnamen** und die gleiche **Organisations-ID** verwenden wie beim Festlegen der Paket-ID im Apple-Entwicklerportal.

3. Wählen Sie im Projektnavigator unter **Ziele** Ihren Projektnamen und anschließend die Registerkarte **Signing & Capabilities** (Signieren und Funktionen) aus. Wählen Sie das passende **Team** für Ihr Apple-Entwicklerkonto aus. Xcode sollte auf der Grundlage Ihrer Paket-ID automatisch das Bereitstellungsprofil abrufen, das Sie zuvor erstellt haben.

   Wenn das neue in Xcode erstellte Bereitstellungsprofil nicht angezeigt wird, versuchen Sie, die Profile für Ihre Signaturidentität zu aktualisieren. Klicken Sie in der Menüleiste auf **Xcode**, dann auf **Voreinstellungen**, auf die Registerkarte **Konto**, auf die Schaltfläche **Details anzeigen**, auf die Signaturidentität und anschließend rechts unten auf die Schaltfläche „Aktualisieren“.

   :::image type="content" source="media/ios-sdk/image2.png" alt-text="Details anzeigen":::

4. Wählen Sie auf der Registerkarte **Signing & Capabilities** (Signieren und Funktionen) die Option **+ Funktion** aus. Doppelklicken Sie zum Aktivieren auf **Pushbenachrichtigungen**.

   :::image type="content" source="media/ios-sdk/image3.png" alt-text="Funktion":::

5. Fügen Sie die Module für das Azure Notification Hubs SDK hinzu.

   Sie können das Azure Notification Hubs SDK in Ihre App integrieren. Verwenden Sie dazu [CocoaPods](https://cocoapods.org/), oder fügen Sie Ihrem Projekt die Binärdateien manuell hinzu.

   - Integration über CocoaPods: Fügen Sie Ihrer Podfile-Datei die folgenden Abhängigkeiten hinzu, um das Azure Notification Hubs SDK in Ihre App zu integrieren:

      ```ruby
      pod 'AzureNotificationHubs-iOS'
      ```

      - Führen Sie „pod install“ aus, um Ihren neu definierten Pod zu installieren, und öffnen Sie „.xcworkspace“.

         Wenn beim Ausführen von „pod install“ ein Fehler wie **Es konnte keine Spezifikation für AzureNotificationHubs-iOS gefunden werden** angezeigt wird, führen Sie `pod repo update` aus, um die neuesten Pods aus dem CocoaPods-Repository abzurufen, und führen Sie dann „pod install“ aus.

   - Integration über Carthage: Fügen Sie Ihrer Cartfile-Datei die folgenden Abhängigkeiten hinzu, um das Azure Notification Hubs SDK in Ihre App zu integrieren:

      ```ruby
      github "Azure/azure-notificationhubs-ios"
      ```

      - Aktualisieren und erstellen Sie als Nächstes die Abhängigkeiten:

      ```shell
      $ carthage update
      ```

      Weitere Informationen zur Verwendung von Carthage finden Sie im [GitHub-Repository für Carthage](https://github.com/Carthage/Carthage).

   - Integration durch Kopieren der Binärdateien in Ihr Projekt: Sie können die Integration durch Kopieren der Binärdateien in Ihr Projekt vornehmen. Gehen Sie dazu folgendermaßen vor:

        - Laden Sie das als ZIP-Datei bereitgestellte Framework für das [Azure Notification Hubs SDK](https://github.com/Azure/azure-notificationhubs-android/releases) herunter, und entzippen Sie es.

        - Klicken Sie in Xcode mit der rechten Maustaste auf Ihr Projekt, und klicken Sie dann auf die Option **Add Files to** zum Hinzufügen des Ordners **WindowsAzureMessaging.framework** zum Xcode-Projekt. Klicken Sie auf **Optionen**, vergewissern Sie sich, dass die Option **Copy items if needed** (Elemente kopieren, wenn nötig) aktiviert ist, und klicken Sie dann auf **Hinzufügen**.

          :::image type="content" source="media/ios-sdk/image4.png" alt-text="Hinzufügen des Frameworks":::

6. Fügen Sie die Informationen zum Herstellen einer Verbindung mit Azure Notification Hubs im entsprechenden Abschnitt `<string></string>` hinzu.  Ersetzen Sie die Platzhalter für die Zeichenfolgenliterale `--HUB-NAME--` und `--CONNECTION-STRING--` durch den Hubnamen bzw. durch **DefaultListenSharedAccessSignature**, die Sie zuvor aus dem Portal abgerufen haben:

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
    <plist version="1.0">
    <dict>
        <key>HUB_NAME</key>
        <string>--HUB-NAME--</string>
        <key>CONNECTION_STRING</key>
        <string>--CONNECTION-STRING--</string>
    </dict>
    </plist>
    ```

7. Öffnen Sie die Datei **AppDelegate.h** des Projekts, und ersetzen Sie ihren Inhalt durch den folgenden Code:

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

8. Öffnen Sie die Datei **AppDelegate.m** des Projekts, und fügen Sie die folgenden `import`-Anweisungen hinzu:

    ```objc
    #import "NotificationDetailViewController.h"
    ```

12. Fügen Sie ebenfalls in der Datei **AppDelegate.m** der Methode `didFinishLaunchingWithOptions` die folgende Codezeile hinzu (abhängig von Ihrer iOS-Version). Dieser Code registriert Ihr Gerätehandle bei APNS:

    ```objc
    [[UNUserNotificationCenter currentNotificationCenter] setDelegate:self];
    ```

13. Ersetzen Sie in derselben Datei **AppDelegate.m** den gesamten Code nach `didFinishLaunchingWithOptions` durch den folgenden Code:

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
        // - UNNotificationPresentationOptionsBadge: Apply the notification's badge value to the app’s icon.
        // - UNNotificationPresentationOptionList: Show in the Notification Center
        // - UNNotificationPresentationOptionsSound: Play the sound associated with the notification.
        //
        completionHandler(UNNotificationPresentationOptionsBadge | UNNotificationPresentationOptionsSound);
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
        NSString *path = [[NSBundle mainBundle] pathForResource:@"DevSettings" ofType:@"plist"];
        NSDictionary *configValues = [NSDictionary dictionaryWithContentsOfFile:path];
        
        NSString *connectionString = [configValues objectForKey:@"CONNECTION_STRING"];
        NSString *hubName = [configValues objectForKey:@"HUB_NAME"];
        
        return [[SBNotificationHub alloc] initWithConnectionString:connectionString notificationHubPath:hubName];
    }

    - (void)handleRegister {
        UNUserNotificationCenter *center = [UNUserNotificationCenter currentNotificationCenter];
        
        UNAuthorizationOptions options =  UNAuthorizationOptionAlert | UNAuthorizationOptionSound | UNAuthorizationOptionBadge;
        [center requestAuthorizationWithOptions:(options) completionHandler:^(BOOL granted, NSError * _Nullable error) {
            if (error != nil) {
                NSLog(@"Error requesting for authorization: %@", error);
            }

            if (granted) {
                NSLog(@"Authorization granted");
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

    Mit diesem Code wird eine Verbindung mit dem Benachrichtigungs-Hub mithilfe der in **Constants.h** angegebenen Verbindungsinformationen hergestellt. Dieser Code übergibt dann das Gerätetoken an den Notification Hub, sodass dieser Benachrichtigungen senden kann.

### <a name="create-notificationdetailviewcontroller-header-file"></a>Erstellen der Headerdatei „NotificationDetailViewController“

1. Fügen Sie wie in den vorherigen Anweisungen eine weitere Headerdatei mit dem Namen **NotificationDetailViewController.h** hinzu. Ersetzen Sie den Inhalt der neuen Headerdatei durch den folgenden Code:

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

2. Fügen Sie die Implementierungsdatei **NotificationDetailViewController.m** hinzu. Ersetzen Sie den Inhalt der Datei durch den folgenden Code. Dadurch werden die UIViewController-Methoden implementiert:

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

### <a name="viewcontroller"></a>ViewController

1. Fügen Sie in der Datei **ViewController.h** des Projekts die folgenden `import`-Anweisungen hinzu:

   ```objc
   #import <WindowsAzureMessaging/WindowsAzureMessaging.h>
   #import <UserNotifications/UserNotifications.h>
   ```

2. Fügen Sie außerdem in **ViewController.h** die folgenden Eigenschaftendeklarationen nach der Deklaration `@interface` hinzu:

   ```objc
   @property (strong, nonatomic) IBOutlet UITextField *tagsTextField;
   @property (strong, nonatomic) IBOutlet UIButton *registerButton;
   @property (strong, nonatomic) IBOutlet UIButton *unregisterButton;
   ```

3. Ersetzen Sie in der Implementierungsdatei **ViewController.m** des Projekts ihren Inhalt durch den folgenden Code:

   ```objc
   #import "ViewController.h"
   #import "AppDelegate.h"

    static NSString *const kNHUserDefaultTags = @"notification_tags";

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
        self.tagsTextField.text = [[NSUserDefaults standardUserDefaults] valueForKey:kNHUserDefaultTags];
   }

   - (IBAction)handleRegister:(id)sender {
        // Save raw tags text in storage
        [[NSUserDefaults standardUserDefaults] setValue:self.tagsTextField.text forKey:kNHUserDefaultTags];
        
        // Delegate processing the register action to the app delegate.
        [[[UIApplication sharedApplication] delegate] performSelector:@selector(handleRegister)];
   }

   - (IBAction)handleUnregister:(id)sender {
        [[[UIApplication sharedApplication] delegate] performSelector:@selector(handleUnregister)];
   }

   @end
   ```

4. Erstellen Sie die App auf Ihrem Gerät, und führen Sie sie dort aus, um Fehler auszuschließen.

## <a name="send-test-push-notifications"></a>Senden von Pushbenachrichtigungen zu Testzwecken

Mit der Option **Testsendevorgang** im [Azure portal](https://portal.azure.com/) können Sie den Empfang von Benachrichtigungen in Ihrer App testen. Diese Option sendet zu Testzwecken eine Pushbenachrichtigung an Ihr Gerät.

:::image type="content" source="media/ios-sdk/image6.png" alt-text="Testsendevorgang":::

Pushbenachrichtigungen werden normalerweise mithilfe einer kompatiblen Bibliothek über einen Back-End-Dienst wie Mobile Apps oder ASP.NET gesendet. Falls für Ihr Back-End keine Bibliothek verfügbar ist, können Sie Benachrichtigungen auch direkt über die REST-API senden.

Es folgen einige andere Tutorials, die Informationen zum Senden von Benachrichtigungen enthalten:

- Azure Mobile Apps: Ein Beispiel für das Senden von Benachrichtigungen von einem mit Notification Hubs integrierten Mobile Apps-Back-End finden Sie unter [Hinzufügen von Pushbenachrichtigungen zu Ihrer iOS-App](/previous-versions/azure/app-service-mobile/app-service-mobile-ios-get-started-push).
- ASP.NET: [Verwenden von Notification Hubs zum Senden von Pushbenachrichtigungen an Benutzer](notification-hubs-aspnet-backend-ios-apple-apns-notification.md).
- Azure Notification Hubs Java-SDK: Informationen zum Senden von Benachrichtigungen über Java finden Sie unter [Verwenden von Notification Hubs von Java aus](notification-hubs-java-push-notification-tutorial.md) . Dies wurde für die Android-Entwicklung in Eclipse getestet.
- PHP: [Verwenden von Notification Hubs von PHP aus](notification-hubs-php-push-notification-tutorial.md).

## <a name="verify-that-your-app-receives-push-notifications"></a>Überprüfen, ob die App Pushbenachrichtigungen empfängt

Zum Testen von Pushbenachrichtigungen unter iOS müssen Sie die App auf einem physischen iOS-Gerät bereitstellen. Sie können keine Apple-Pushbenachrichtigungen mit dem iOS-Simulator senden.

1. Führen Sie die App aus, und überprüfen Sie, ob die Registrierung erfolgreich ist. Drücken Sie dann **OK**.

   :::image type="content" source="media/ios-sdk/image7.png" alt-text="Registrieren":::

2. Senden Sie nun zu Testzwecken eine Pushbenachrichtigung über das[Azure-Portal](https://portal.azure.com/), wie im vorherigen Abschnitt beschrieben.

3. Die Pushbenachrichtigung wird an alle Geräte gesendet, die dafür registriert sind, die Benachrichtigungen vom angegebenen Notification Hub zu erhalten.

   :::image type="content" source="media/ios-sdk/image8.png" alt-text="Testsendevorgang":::

## <a name="next-steps"></a>Nächste Schritte

In diesem einfachen Beispiel haben Sie Pushbenachrichtigungen an alle Ihre registrierten iOS-Geräte übertragen. Um zu erfahren, wie Sie Pushbenachrichtigungen an bestimmte iOS-Geräte senden, fahren Sie mit dem folgenden Tutorial fort:

[Tutorial: Senden von Pushbenachrichtigungen an bestimmte Geräte](notification-hubs-ios-xplat-segmented-apns-push-notification.md)

Weitere Informationen finden Sie in den folgenden Artikeln:

- [Übersicht über Azure Notification Hubs](notification-hubs-push-notification-overview.md)
- [REST-APIs für Notification Hubs](/rest/api/notificationhubs/)
- [Notification Hubs SDK für Back-End-Vorgänge](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)
- [Notification Hubs-SDK auf GitHub](https://github.com/Azure/azure-notificationhubs)
- [Registrieren des aktuellen Benutzers für Pushbenachrichtigungen mit ASP.NET](notification-hubs-ios-aspnet-register-user-from-backend-to-push-notification.md)
- [Registrierungsverwaltung](notification-hubs-push-notification-registration-management.md)
- [Arbeiten mit Tags](notification-hubs-tags-segment-push-message.md)
- [Arbeiten mit benutzerdefinierten Vorlagen](notification-hubs-templates-cross-platform-push-messages.md)
- [Service Bus-Zugriffssteuerung mit Shared Access Signatures](../service-bus-messaging/service-bus-sas.md)
- [Programmgesteuertes Generieren von SAS-Token](/rest/api/eventhub/generate-sas-token)
- [Apple-Sicherheit: allgemeine Kryptografie](https://developer.apple.com/security/)
- [UNIX-ZEIT (Epoch)](https://en.wikipedia.org/wiki/Unix_time)
- [HMAC](https://en.wikipedia.org/wiki/HMAC)