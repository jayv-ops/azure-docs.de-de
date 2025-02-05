---
author: mikben
ms.service: azure-communication-services
ms.topic: include
ms.date: 03/10/2021
ms.author: mikben
ms.openlocfilehash: 8d4e573cefd595669d9cb2cf9a7b83595eea7971
ms.sourcegitcommit: 18a91f7fe1432ee09efafd5bd29a181e038cee05
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/16/2021
ms.locfileid: "103622109"
---
## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F). 
- Eine bereitgestellte Communication Services-Ressource. [Erstellen Sie eine Communication Services-Ressource](../../create-communication-resource.md).
- Ein `User Access Token`, um den Anrufclient zu aktivieren. Weitere Informationen zum [Abrufen eines `User Access Token`](../../access-tokens.md)
- Optional: Gehen Sie den Schnellstart [Erste Schritte beim Hinzufügen von Anruffunktionen zu einer Anwendung](../getting-started-with-calling.md) durch.

## <a name="setting-up"></a>Einrichten

### <a name="install-the-package"></a>Installieren des Pakets

> [!NOTE]
> Dieses Dokument verwendet Version 1.0.0-beta.8 der aufrufenden Clientbibliothek.

<!-- TODO: update with instructions on how to download, install and add package to project -->
Wählen Sie Ihre Datei „build.gradle“ auf Projektebene aus, und stellen Sie sicher, dass Sie `mavenCentral()` zur Liste der Repositorys unter `buildscript` und `allprojects` hinzufügen.
```groovy
buildscript {
    repositories {
    ...
        mavenCentral()
    ...
    }
}
```

```groovy
allprojects {
    repositories {
    ...
        mavenCentral()
    ...
    }
}
```
Fügen Sie dann in der Datei „build.gradle“ auf Modulebene die folgenden Zeilen zum Abschnitt „dependencies“ hinzu.

```groovy
dependencies {
    ...
    implementation 'com.azure.android:azure-communication-calling:1.0.0-beta.8'
    ...
}

```

## <a name="object-model"></a>Objektmodell

Die folgenden Klassen und Schnittstellen befassen sich mit einigen der wichtigsten Features der Azure Communication Services-Clientbibliothek „Calling“:

| Name                                  | Beschreibung                                                  |
| ------------------------------------- | ------------------------------------------------------------ |
| CallClient| CallClient ist der Haupteinstiegspunkt in die Clientbibliothek „Calling“.|
| CallAgent | CallAgent dient zum Starten und Verwalten von Anrufen. |
| CommunicationTokenCredential | „CommunicationTokenCredential“ dient als tokengestützte Anmeldeinformation zum Instanziieren von „CallAgent“.|
| CommunicationIdentifier | „CommunicationIdentifier“ wird als anderer Typ von Teilnehmer verwendet, der Teil eines Anrufs sein könnte.|

## <a name="initialize-the-callclient-create-a-callagent-and-access-the-devicemanager"></a>Initialisieren von CallClient, Erstellen von CallAgent und Zugreifen auf DeviceManager

Zum Erstellen einer `CallAgent`-Instanz müssen Sie die `createCallAgent`-Methode für eine `CallClient`-Instanz aufzurufen. Dadurch wird ein `CallAgent`-Instanzobjekt asynchron zurückgegeben.
Die `createCallAgent`-Methode verwendet `CommunicationUserCredential` als Argument, womit ein [Zugriffstoken](../../access-tokens.md) gekapselt wird.
Für den Zugriff auf `DeviceManager` muss zuerst eine CallAgent-Instanz erstellt werden. Anschließend können Sie mit der `CallClient.getDeviceManager`-Methode DeviceManager abrufen.

```java
String userToken = '<user token>';
CallClient callClient = new CallClient();
CommunicationTokenCredential tokenCredential = new CommunicationTokenCredential(userToken);
android.content.Context appContext = this.getApplicationContext(); // From within an Activity for instance
CallAgent callAgent = callClient.createCallAgent(appContext, tokenCredential).get();
DeviceManager deviceManager = callClient.getDeviceManager().get();
```
Zum Festlegen eines Anzeigenamens für den Anrufer verwenden Sie diese alternative Methode:

```java
String userToken = '<user token>';
CallClient callClient = new CallClient();
CommunicationTokenCredential tokenCredential = new CommunicationTokenCredential(userToken);
android.content.Context appContext = this.getApplicationContext(); // From within an Activity for instance
CallAgentOptions callAgentOptions = new CallAgentOptions();
callAgentOptions.setDisplayName("Alice Bob");
CallAgent callAgent = callClient.createCallAgent(appContext, tokenCredential, callAgentOptions).get();
DeviceManager deviceManager = callClient.getDeviceManager().get();
```


## <a name="place-an-outgoing-call-and-join-a-group-call"></a>Tätigen eines ausgehenden Anrufs und Teilnehmen an einem Gruppenanruf

Um einen Anruf zu erstellen und zu starten, müssen Sie die `CallAgent.startCall()`-Methode aufrufen und den `Identifier` des/der Angerufenen angeben.
Um an einem Gruppenanruf teilzunehmen, müssen Sie die `CallAgent.join()`-Methode aufrufen und die groupId angeben. Gruppen-IDs müssen das GUID- oder UUID-Format haben.

Erstellung und Start des Anrufs erfolgen synchron. Die Anrufinstanz ermöglicht Ihnen das Abonnieren aller Ereignisse im Rahmen des Anrufs.

### <a name="place-a-11-call-to-a-user"></a>Tätigen eines 1:1-Anrufs eines Benutzers
Um einen Anruf eines anderen Communication Services-Benutzers zu tätigen, rufen Sie die `call`-Methode für `callAgent` auf und übergeben ein Objekt mit `communicationUserId`-Schlüssel.
```java
StartCallOptions startCallOptions = new StartCallOptions();
Context appContext = this.getApplicationContext();
CommunicationUserIdentifier acsUserId = new CommunicationUserIdentifier(<USER_ID>);
CommunicationUserIdentifier participants[] = new CommunicationUserIdentifier[]{ acsUserId };
call oneToOneCall = callAgent.startCall(appContext, participants, startCallOptions);
```

### <a name="place-a-1n-call-with-users-and-pstn"></a>1:n-Anruf mit Benutzern und Festnetznummern
> [!WARNING]
> Zurzeit sind Anrufe über das Telefonfestnetz nicht möglich.

Für einen 1:n-Anruf eines Benutzers und einer Festnetznummer müssen Sie die Telefonnummer des Angerufenen angeben.
Die Communication Services-Ressource muss so konfiguriert sein, dass Anrufe über das Telefonfestnetz möglich sind:
```java
CommunicationUserIdentifier acsUser1 = new CommunicationUserIdentifier(<USER_ID>);
PhoneNumberIdentifier acsUser2 = new PhoneNumberIdentifier("<PHONE_NUMBER>");
CommunicationIdentifier participants[] = new CommunicationIdentifier[]{ acsUser1, acsUser2 };
StartCallOptions startCallOptions = new StartCallOptions();
Context appContext = this.getApplicationContext();
Call groupCall = callAgent.startCall(participants, startCallOptions);
```

### <a name="place-a-11-call-with-video-camera"></a>Tätigen eines 1:1-Anrufs mit Videokamera
> [!WARNING]
> Derzeit wird nur ein ausgehender lokaler Videostream unterstützt. Um einen Anruf mit Video zu tätigen, müssen Sie die lokalen Kameras mit der `getCameras`-API von `deviceManager` aufzählen.
Sobald Sie eine gewünschte Kamera ausgewählt haben, erzeugen Sie damit eine `LocalVideoStream`-Instanz und übergeben diese an `videoOptions` als Element im `localVideoStream`-Array an eine `call`-Methode.
Sobald die Anrufverbindung hergestellt ist, wird automatisch ein Videostream von der ausgewählten Kamera an andere Teilnehmer gesendet.

> [!NOTE]
> Aus Gründen des Datenschutzes wird das Video nicht für den Anruf freigegeben, wenn es nicht lokal in der Vorschau angezeigt wird.
Weitere Informationen finden Sie unter [Vorschau auf lokaler Kamera](#local-camera-preview).
```java
Context appContext = this.getApplicationContext();
VideoDeviceInfo desiredCamera = callClient.getDeviceManager().get().getCameras().get(0);
LocalVideoStream currentVideoStream = new LocalVideoStream(desiredCamera, appContext);
VideoOptions videoOptions = new VideoOptions(currentVideoStream);

// Render a local preview of video so the user knows that their video is being shared
Renderer previewRenderer = new Renderer(currentVideoStream, appContext);
View uiView = previewRenderer.createView(new RenderingOptions(ScalingMode.Fit));
// Attach the uiView to a viewable location on the app at this point
layout.addView(uiView);

CommunicationUserIdentifier[] participants = new CommunicationUserIdentifier[]{ new CommunicationUserIdentifier("<acs user id>") };
StartCallOptions startCallOptions = new StartCallOptions();
startCallOptions.setVideoOptions(videoOptions);
Call call = callAgent.startCall(context, participants, startCallOptions);
```

### <a name="join-a-group-call"></a>Teilnehmen an einem Gruppenanruf
Um einen neuen Gruppenanruf zu starten oder an einem laufenden Gruppenanruf teilzunehmen, müssen Sie die „join“-Methode aufrufen und ein Objekt mit einer `groupId`-Eigenschaft übergeben. Der Wert muss eine GUID sein.
```java
Context appContext = this.getApplicationContext();
GroupCallLocator groupCallLocator = new GroupCallLocator("<GUID>");
JoinCallOptions joinCallOptions = new JoinCallOptions();

call = callAgent.join(context, groupCallLocator, joinCallOptions);
```

### <a name="accept-a-call"></a>Annehmen eines Anrufs
Zum Annehmen eines Anrufs rufen Sie die Methode „accept“ für ein Anrufobjekt auf.

```java
Context appContext = this.getApplicationContext();
IncomingCall incomingCall = retrieveIncomingCall();
Call call = incomingCall.accept(context).get();
```

Annehmen eines Anrufs mit eingeschalteter Videokamera:

```java
Context appContext = this.getApplicationContext();
IncomingCall incomingCall = retrieveIncomingCall();
AcceptCallOptions acceptCallOptions = new AcceptCallOptions();
VideoDeviceInfo desiredCamera = callClient.getDeviceManager().get().getCameraList().get(0);
acceptCallOptions.setVideoOptions(new VideoOptions(new LocalVideoStream(desiredCamera, appContext)));
Call call = incomingCall.accept(context, acceptCallOptions).get();
```

Der eingehende Anruf kann durch Abonnieren des `onIncomingCall`-Ereignisses auf dem `callAgent`-Objekt abgerufen werden:

```java
// Assuming "callAgent" is an instance property obtained by calling the 'createCallAgent' method on CallClient instance 
public Call retrieveIncomingCall() {
    IncomingCall incomingCall;
    callAgent.addOnIncomingCallListener(new IncomingCallListener() {
        void onIncomingCall(IncomingCall inboundCall) {
            // Look for incoming call
            incomingCall = inboundCall;
        }
    });
    return incomingCall;
}
```


## <a name="push-notifications"></a>Pushbenachrichtigungen

### <a name="overview"></a>Übersicht
Mobile Pushbenachrichtigungen sind die Popupbenachrichtigungen, die auf mobilen Geräten angezeigt werden. Bei Anruffunktionen konzentrieren wir uns auf VoIP-Pushbenachrichtigungen (Voice over Internet Protocol). Wir werden uns für Pushbenachrichtigungen registrieren, Pushbenachrichtigungen bearbeiten und dann die Registrierung von Pushbenachrichtigungen aufheben.

### <a name="prerequisites"></a>Voraussetzungen

Ein Firebase-Konto, das mit aktiviertem Cloud Messaging (FCM) eingerichtet wurde und bei dem Ihr Firebase Cloud Messaging-Dienst mit einer Azure Notification Hub-Instanz verbunden ist. Weitere Informationen finden Sie unter [Communication Services-Benachrichtigungen](../../../concepts/notifications.md).
Außerdem geht das Tutorial davon aus, dass Sie zur Entwicklung Ihrer Anwendung mindestens die Android Studio-Version 3.6 einsetzen.

Für die Android-Anwendung sind eine Reihe von Berechtigungen erforderlich, um Benachrichtigungen von Firebase Cloud Messaging empfangen zu können. Fügen Sie in Ihrer Datei `AndroidManifest.xml` direkt nach dem Tag *<manifest ...>* oder unter dem Tag *</application>* den folgenden Berechtigungssatz hinzu.

```XML
    <uses-permission android:name="android.permission.INTERNET"/>
    <uses-permission android:name="android.permission.GET_ACCOUNTS"/>
    <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
```

### <a name="register-for-push-notifications"></a>Registrieren für Pushbenachrichtigungen

Um sich für Pushbenachrichtigungen zu registrieren, muss die Anwendung `registerPushNotification()` für eine *CallAgent*-Instanz mit einem Geräteregistrierungstoken aufrufen.

Um das Geräteregistrierungstoken zu erhalten, fügen Sie die Firebase-Clientbibliothek zur Datei *build.gradle* Ihres Anwendungsmoduls hinzu, indem Sie die folgenden Zeilen im Abschnitt `dependencies` hinzufügen, sofern noch nicht vorhanden:

```
    // Add the client library for Firebase Cloud Messaging
    implementation 'com.google.firebase:firebase-core:16.0.8'
    implementation 'com.google.firebase:firebase-messaging:20.2.4'
```

Fügen Sie in der Datei *build.gradle* auf Projektebene im Abschnitt `dependencies` Folgendes hinzu, sofern noch nicht vorhanden:

```
    classpath 'com.google.gms:google-services:4.3.3'
```

Fügen Sie das folgende Plug-In am Anfang der Datei hinzu, sofern noch nicht vorhanden:

```
apply plugin: 'com.google.gms.google-services'
```

Wählen Sie auf der Symbolleiste *Jetzt synchronisieren* aus. Fügen Sie den folgenden Codeausschnitt hinzu, um das Geräteregistrierungstoken abzurufen, das von der Firebase Cloud Messaging-Clientbibliothek für die Clientanwendungsinstanz generiert wird. Stellen Sie sicher, dass Sie die unten aufgeführten Importe zum Header der Hauptaktivität für die Instanz hinzufügen. Sie sind erforderlich, damit der Ausschnitt das Token abruft.

```
import com.google.android.gms.tasks.OnCompleteListener;
import com.google.android.gms.tasks.Task;
import com.google.firebase.iid.FirebaseInstanceId;
import com.google.firebase.iid.InstanceIdResult;
```

Fügen Sie diesen Ausschnitt hinzu, um das Token abzurufen:

```
        FirebaseInstanceId.getInstance().getInstanceId()
                .addOnCompleteListener(new OnCompleteListener<InstanceIdResult>() {
                    @Override
                    public void onComplete(@NonNull Task<InstanceIdResult> task) {
                        if (!task.isSuccessful()) {
                            Log.w("PushNotification", "getInstanceId failed", task.getException());
                            return;
                        }

                        // Get new Instance ID token
                        String deviceToken = task.getResult().getToken();
                        // Log
                        Log.d("PushNotification", "Device Registration token retrieved successfully");
                    }
                });
```
Registrieren Sie das Geräteregistrierungstoken bei der Communication Services-Clientbibliothek „Calling“ für Pushbenachrichtigungen zu eingehenden Anrufen.

```java
String deviceRegistrationToken = "<Device Token from previous section>";
try {
    callAgent.registerPushNotification(deviceRegistrationToken).get();
}
catch(Exception e) {
    System.out.println("Something went wrong while registering for Incoming Calls Push Notifications.")
}
```

### <a name="push-notification-handling"></a>Behandlung von Pushbenachrichtigungen

Um Pushbenachrichtigungen für eingehende Anrufe zu empfangen, rufen Sie *handlePushNotification()* für eine *CallAgent*-Instanz mit Nutzdaten auf.

Um die Nutzlast von Firebase Cloud Messaging abzurufen, beginnen Sie mit der Erstellung eines neuen Diensts (Datei > Neu > Dienst > Dienst), der die Firebase-Clientbibliotheksklasse *FirebaseMessagingService* erweitert, und setzen Sie dann die `onMessageReceived`-Methode außer Kraft. Diese Methode ist der Ereignishandler, der aufgerufen wird, wenn Firebase Cloud Messaging die Pushbenachrichtigung an die Anwendung übermittelt.

```java
public class MyFirebaseMessagingService extends FirebaseMessagingService {
    private java.util.Map<String, String> pushNotificationMessageDataFromFCM;

    @Override
    public void onMessageReceived(RemoteMessage remoteMessage) {
        // Check if message contains a notification payload.
        if (remoteMessage.getNotification() != null) {
            Log.d("PushNotification", "Message Notification Body: " + remoteMessage.getNotification().getBody());
        }
        else {
            pushNotificationMessageDataFromFCM = remoteMessage.getData();
        }
    }
}
```
Fügen Sie der Datei `AndroidManifest.xml` im Tag <application> die unten angegebene Dienstdefinition hinzu:

```
        <service
            android:name=".MyFirebaseMessagingService"
            android:exported="false">
            <intent-filter>
                <action android:name="com.google.firebase.MESSAGING_EVENT" />
            </intent-filter>
        </service>
```

- Sobald die Nutzdaten abgerufen wurden, können sie an die *Communication Services*-Clientbibliothek übergeben werden, um sie in ein internes *IncomingCallInformation*-Objekt zu analysieren, das durch Aufrufen der *handlePushNotification*-Methode für eine *CallAgent*-Instanz verarbeitet wird. Eine `CallAgent`-Instanz wird durch den Aufruf der Methode `createCallAgent(...)` für die Klasse `CallClient` erstellt.

```java
try {
    IncomingCallInformation notification = IncomingCallInformation.fromMap(pushNotificationMessageDataFromFCM);
    Future handlePushNotificationFuture = callAgent.handlePushNotification(notification).get();
}
catch(Exception e) {
    System.out.println("Something went wrong while handling the Incoming Calls Push Notifications.");
}
```

Wenn die Verarbeitung der Pushbenachrichtigung erfolgreich ist und alle Ereignishandler ordnungsgemäß registriert sind, löst die Anwendung ein Klingeln aus.

### <a name="unregister-push-notifications"></a>Aufheben der Registrierung der Pushbenachrichtigungen

Anwendungen können die Registrierung der Pushbenachrichtigung jederzeit aufheben. Rufen Sie die Methode `unregisterPushNotification()` für callAgent auf, um die Registrierung aufzuheben.

```java
try {
    callAgent.unregisterPushNotifications().get();
}
catch(Exception e) {
    System.out.println("Something went wrong while un-registering for all Incoming Calls Push Notifications.")
}
```

## <a name="call-management"></a>Anrufverwaltung
Sie können auf Anrufeigenschaften zugreifen und während eines Anrufs verschiedene Vorgänge ausführen, um Einstellungen für Video und Audio zu verwalten.

### <a name="call-properties"></a>Anrufeigenschaften

Ruft die eindeutige ID für diesen Anruf ab:

```java
String callId = call.getId();
```

Um mehr über andere Anrufteilnehmer zu erfahren, sehen Sie sich die `remoteParticipant`-Sammlung für die `call`-Instanz an:

```java
List<RemoteParticipant> remoteParticipants = call.getRemoteParticipants();
```

Die Identität des Anrufers bei einem eingehenden Anruf:

```java
CommunicationIdentifier callerId = call.getCallerId();
```

Ruft den Zustand des Anrufs ab: 

```java
CallState callState = call.getState();
```

Gibt eine Zeichenfolge zurück, die den aktuellen Zustand eines Anrufs darstellt:
* None: ursprünglicher Anrufzustand
* Connecting: anfänglicher Übergangszustand, sobald ein Anruf getätigt oder angenommen wird
* Ringing: für einen ausgehenden Anruf; gibt an, dass für den Anruf bei Remoteteilnehmern ein Klingeln ertönt.
* EarlyMedia: gibt einen Zustand an, bei dem eine Ansage wiedergegeben wird, bevor der Anruf verbunden wird
* Connected: der Anruf wurde verbunden
* LocalHold: Der Anruf wird vom lokalen Teilnehmer gehalten; es werden keine Medien zwischen lokalem Endpunkt und Remoteteilnehmern übertragen.
* RemoteHold: Der Anruf wird von einem Remoteteilnehmer gehalten; es werden keine Medien zwischen lokalem Endpunkt und Remoteteilnehmern übertragen.
* Disconnecting: Übergangsstatus, ehe der Anruf in den Zustand „Disconnected“ wechselt.
* Disconnected: der Endzustand des Anrufs


Um zu erfahren, warum ein Anruf beendet wurde, überprüfen Sie die `callEndReason`-Eigenschaft. Sie enthält Code/Subcode: 

```java
CallEndReason callEndReason = call.getCallEndReason();
int code = callEndReason.getCode();
int subCode = callEndReason.getSubCode();
```

Um festzustellen, ob der aktuelle Anruf ein- oder ausgehend ist, untersuchen Sie die `callDirection`-Eigenschaft:

```java
CallDirection callDirection = call.getCallDirection(); 
// callDirection == CallDirection.Incoming for incoming call
// callDirection == CallDirection.Outgoing for outgoing call
```

Um festzustellen, ob das aktuelle Mikrofon stummgeschaltet ist, prüfen Sie die `muted`-Eigenschaft:

```java
boolean muted = call.getIsMicrophoneMuted();
```

Um herauszufinden, ob der aktuelle Anruf aufgezeichnet wird, untersuchen Sie die `isRecordingActive`-Eigenschaft:

```java
boolean recordinggActive = call.getIsRecordingActive();
```

Um aktive Videostreams zu prüfen, sehen Sie sich die `localVideoStreams`-Sammlung an:

```java
List<LocalVideoStream> localVideoStreams = call.getLocalVideoStreams();
```

### <a name="mute-and-unmute"></a>Stummschalten und Aufheben der Stummschaltung

Zum Stummschalten oder Aufheben der Stummschaltung des lokalen Endpunkts können Sie die asynchronen APIs `mute` und `unmute` verwenden:

```java
call.mute().get();
call.unmute().get();
```

### <a name="start-and-stop-sending-local-video"></a>Starten und Beenden des Sendens von lokalem Video

Um ein Video zu starten, müssen Sie die Kameras mit der `getCameraList`-API für das `deviceManager`-Objekt aufzählen. Erstellen Sie dann eine neue Instanz von `LocalVideoStream` zur Übergabe der gewünschten Kamera, und übergeben Sie sie in der `startVideo`-API als Argument:

```java
VideoDeviceInfo desiredCamera = <get-video-device>;
Context appContext = this.getApplicationContext();
LocalVideoStream currentLocalVideoStream = new LocalVideoStream(desiredCamera, appContext);
VideoOptions videoOptions = new VideoOptions(currentLocalVideoStream);
Future startVideoFuture = call.startVideo(currentLocalVideoStream);
startVideoFuture.get();
```

Sobald Sie erfolgreich mit dem Senden von Video beginnen, wird eine `LocalVideoStream`-Instanz der `localVideoStreams`-Sammlung für die Anrufinstanz hinzugefügt.

```java
currentLocalVideoStream == call.getLocalVideoStreams().get(0);
```

Um das lokale Video anzuhalten, übergeben Sie die `LocalVideoStream`-Instanz, die in der `localVideoStreams`-Sammlung verfügbar ist:

```java
call.stopVideo(currentLocalVideoStream).get();
```

Sie können während des Sendens von Video auf ein anderes Kameragerät umschalten, indem Sie `switchSource` für eine `LocalVideoStream`-Instanz aufrufen:
```java
currentLocalVideoStream.switchSource(source).get();
```

## <a name="remote-participants-management"></a>Verwaltung von Remoteteilnehmern

Alle Remoteteilnehmer werden durch den Typ `RemoteParticipant` dargestellt und sind über die `remoteParticipants`-Sammlung für eine Anrufinstanz verfügbar.

### <a name="list-participants-in-a-call"></a>Auflisten der Teilnehmer an einem Anruf
Die `remoteParticipants`-Sammlung gibt eine Liste der Remoteteilnehmer an einem angegebenen Anruf zurück:
```java
List<RemoteParticipant> remoteParticipants = call.getRemoteParticipants(); // [remoteParticipant, remoteParticipant....]
```

### <a name="remote-participant-properties"></a>Eigenschaften von Remoteteilnehmern
Einem Remoteteilnehmer sind eine Reihe von Eigenschaften und Sammlungen zugeordnet:

* Ruft den Bezeichner dieses Remoteteilnehmers ab.
„Identity“ ist einer der „Identifier“-Typen
```java
CommunicationIdentifier participantIdentifier = remoteParticipant.getIdentifier();
```

* Ruft den Zustand dieses Remoteteilnehmers ab.
```java
ParticipantState state = remoteParticipant.getState();
```
Folgende Werte sind hierfür möglich:
* Idle: Anfangszustand
* EarlyMedia: Ansage wird wiedergegeben, bevor der Teilnehmer mit dem Anruf verbunden ist
* Ringing: Der Teilnehmeranruf klingelt.
* Connecting: Übergangszustand, während sich der Teilnehmer mit dem Anruf verbindet
* Connected: Teilnehmer ist mit dem Anruf verbunden
* Hold: Teilnehmer wird gehalten
* InLobby: Der Teilnehmer wartet im Wartebereich darauf, zugelassen zu werden. Wird derzeit nur in Teams-Interoperabilitätsszenarien verwendet.
* Disconnected: In diesem Endzustand ist der Teilnehmer vom Anruf getrennt


* Um zu erfahren, warum ein Teilnehmer den Anruf verlassen hat, prüfen Sie die `callEndReason`-Eigenschaft:
```java
CallEndReason callEndReason = remoteParticipant.getCallEndReason();
```

* Um zu prüfen, ob dieser Remoteteilnehmer stummgeschaltet ist oder nicht, prüfen Sie die `isMuted`-Eigenschaft:
```java
boolean isParticipantMuted = remoteParticipant.getIsMuted();
```

* Um zu prüfen, ob dieser Remoteteilnehmer spricht oder nicht, prüfen Sie die `isSpeaking`-Eigenschaft:
```java
boolean isParticipantSpeaking = remoteParticipant.getIsSpeaking();
```

* Um alle Videostreams zu prüfen, die ein bestimmter Teilnehmer im Rahmen dieses Anrufs sendet, sehen Sie sich die `videoStreams`-Sammlung an:
```java
List<RemoteVideoStream> videoStreams = remoteParticipant.getVideoStreams(); // [RemoteVideoStream, RemoteVideoStream, ...]
```


### <a name="add-a-participant-to-a-call"></a>Hinzufügen eines Teilnehmers zu einem Anruf

Um einen Teilnehmer einem Anruf hinzuzufügen (entweder als Benutzer oder Telefonnummer), können Sie `addParticipant` aufrufen. Dadurch wird die Instanz des Remoteteilnehmers synchron zurückgegeben.

```java
const acsUser = new CommunicationUserIdentifier("<acs user id>");
const acsPhone = new PhoneNumberIdentifier("<phone number>");
RemoteParticipant remoteParticipant1 = call.addParticipant(acsUser);
AddPhoneNumberOptions addPhoneNumberOptions = new AddPhoneNumberOptions(new PhoneNumberIdentifier("<alternate phone number>"));
RemoteParticipant remoteParticipant2 = call.addParticipant(acsPhone, addPhoneNumberOptions);
```

### <a name="remove-participant-from-a-call"></a>Entfernen eines Teilnehmers aus einem Anruf
Um einen Teilnehmer aus einem Anruf zu entfernen (entweder als Benutzer oder Telefonnummer), können Sie `removeParticipant` aufrufen.
Dieser Vorgang wird asynchron aufgelöst, sobald der Teilnehmer aus dem Anruf entfernt wurde.
Der Teilnehmer wird auch aus der `remoteParticipants`-Sammlung entfernt.
```java
RemoteParticipant acsUserRemoteParticipant = call.getParticipants().get(0);
RemoteParticipant acsPhoneRemoteParticipant = call.getParticipants().get(1);
call.removeParticipant(acsUserRemoteParticipant).get();
call.removeParticipant(acsPhoneRemoteParticipant).get();
```

## <a name="render-remote-participant-video-streams"></a>Rendern von Videostreams von Remoteteilnehmern
Um die Video- und Bildschirmübertragungs-Streams der Remoteteilnehmer aufzulisten, sehen Sie sich die `videoStreams`-Sammlungen an:
```java
RemoteParticipant remoteParticipant = call.getRemoteParticipants().get(0);
RemoteVideoStream remoteParticipantStream = remoteParticipant.getVideoStreams().get(0);
MediaStreamType streamType = remoteParticipantStream.getType(); // of type MediaStreamType.Video or MediaStreamType.ScreenSharing
```
 
Um einen `RemoteVideoStream` von einem Remoteteilnehmer zu rendern, müssen Sie ein `OnVideoStreamsUpdated`-Ereignis abonnieren.

Innerhalb des Ereignisses zeigt der Wechsel der `isAvailable`-Eigenschaft zu „true“ an, dass der Remoteteilnehmer gerade einen Stream sendet. Sobald dies geschieht, erstellen Sie eine neue Instanz eines `Renderer`, dann erstellen Sie eine neue `RendererView` mithilfe der asynchronen `createView`-API, und fügen Sie `view.target` an beliebiger Stelle in der Benutzeroberfläche Ihrer Anwendung an.

Immer wenn sich die Verfügbarkeit eines Remotestreams ändert, können Sie wählen, ob Sie den gesamten Renderer, eine bestimmte `RendererView` zerstören oder sie behalten möchten, was jedoch zur Anzeige leerer Videobilder führt.

```java
Renderer remoteVideoRenderer = new Renderer(remoteParticipantStream, appContext);
View uiView = remoteVideoRenderer.createView(new RenderingOptions(ScalingMode.Fit));
layout.addView(uiView);

remoteParticipant.addOnVideoStreamsUpdatedListener(e -> onRemoteParticipantVideoStreamsUpdated(p, e));

void onRemoteParticipantVideoStreamsUpdated(RemoteParticipant participant, RemoteVideoStreamsEvent args) {
    for(RemoteVideoStream stream : args.getAddedRemoteVideoStreams()) {
        if(stream.getIsAvailable()) {
            startRenderingVideo();
        } else {
            renderer.dispose();
        }
    }
}
```

### <a name="remote-video-stream-properties"></a>Eigenschaften von Remotevideostreams
Ein Remotevideostream hat verschiedene Eigenschaften.

* `Id`: ID eines Remotevideostreams
```java
int id = remoteVideoStream.getId();
```

* `MediaStreamType`: Kann „Video“ oder „ScreenSharing“ sein
```java
MediaStreamType type = remoteVideoStream.getType();
```

* `isAvailable`: gibt an, ob der Endpunkt des Remoteteilnehmers einen Stream aktiv sendet
```java
boolean availability = remoteVideoStream.getIsAvailable();
```

### <a name="renderer-methods-and-properties"></a>Methoden und Eigenschaften des Renderers
Rendererobjekt nach APIs

* Erstellt eine `RendererView`-Instanz, die später auf der Benutzeroberfläche der Anwendung angefügt werden kann, um Remotevideostreams zu rendern.
```java
// Create a view for a video stream
renderer.createView()
```
* Löscht den Renderer und alle mit ihm verknüpften `RendererView`-Instanzen. Wird aufgerufen, wenn Sie alle zugehörigen Ansichten aus der Benutzeroberfläche entfernt haben.
```java
renderer.dispose()
```

* `StreamSize`: Größe (Breite/Höhe) eines Remotevideostreams
```java
StreamSize renderStreamSize = remoteVideoStream.getSize();
int width = renderStreamSize.getWidth();
int height = renderStreamSize.getHeight();
```


### <a name="rendererview-methods-and-properties"></a>Methoden und Eigenschaften von RendererView
Beim Erstellen einer `RendererView` können Sie die Eigenschaften `scalingMode` und `mirrored` angeben, die für diese Ansicht gelten sollen: Das Skalierungsmodus kann entweder „Stretch“, „Crop“ oder „Fit“ sein. Wenn `mirrored` auf `true` festgelegt ist, wird der gerenderte Stream vertikal gespiegelt.

```java
Renderer remoteVideoRenderer = new Renderer(remoteVideoStream, appContext);
RendererView rendererView = remoteVideoRenderer.createView(new RenderingOptions(ScalingMode.Fit));
```

Das erstellte RendererView-Objekt kann dann mit dem folgenden Ausschnitt an die Benutzeroberfläche der Anwendung angefügt werden:
```java
layout.addView(rendererView);
```

Sie können den Skalierungsmodus später durch Aufrufen der `updateScalingMode`-API für das RendererView-Objekt mit entweder ScalingMode.Stretch, ScalingMode.Crop oder ScalingMode.Fit als Argument aktualisieren.
```java
// Update the scale mode for this view.
rendererView.updateScalingMode(ScalingMode.Crop)
```


## <a name="device-management"></a>Geräteverwaltung

`DeviceManager` ermöglicht Ihnen das Aufzählen lokaler Geräte, die in einem Anruf zur Übertragung Ihrer Audio-/Videostreams verwendet werden können. DeviceManager erlaubt Ihnen auch, von einem Benutzer die Berechtigung für den Zugriff auf sein Mikrofon und seine Kamera über die native Browser-API anzufordern.

Sie können auf `deviceManager` zugreifen, indem Sie die `callClient.getDeviceManager()`-Methode aufrufen.
> [!WARNING]
> Derzeit muss zuerst ein `callAgent`-Objekt instanziiert werden, um Zugriff auf DeviceManager zu erhalten.

```java
DeviceManager deviceManager = callClient.getDeviceManager().get();
```

### <a name="enumerate-local-devices"></a>Aufzählen lokaler Geräte

Für den Zugriff auf lokale Geräte können Sie Enumerationsmethoden für den Geräte-Manager verwenden. Enumeration ist ein synchroner Vorgang.

```java
//  Get a list of available video devices for use.
List<VideoDeviceInfo> localCameras = deviceManager.getCameras(); // [VideoDeviceInfo, VideoDeviceInfo...]

// Get a list of available microphone devices for use.
List<AudioDeviceInfo> localMicrophones = deviceManager.getMicrophones(); // [AudioDeviceInfo, AudioDeviceInfo...]

// Get a list of available speaker devices for use.
List<AudioDeviceInfo> localSpeakers = deviceManager.getSpeakers(); // [AudioDeviceInfo, AudioDeviceInfo...]
```

### <a name="set-default-microphonespeaker"></a>Festlegen des Standardmikrofons/-lautsprechers

Mit dem Geräte-Manager können Sie ein Standardgerät festlegen, das beim Starten eines Anrufs verwendet wird.
Wenn keine Clientstandardwerte festgelegt sind, nutzt Communication Services die Standardeinstellungen des Betriebssystems.

```java

// Get the microphone device that is being used.
AudioDeviceInfo defaultMicrophone = deviceManager.getMicrophones().get(0);

// Set the microphone device to use.
deviceManager.setMicrophone(defaultMicrophone);

// Get the speaker device that is being used.
AudioDeviceInfo defaultSpeaker = deviceManager.getSpeakers().get(0);

// Set the speaker device to use.
deviceManager.setSpeaker(defaultSpeaker);
```

### <a name="local-camera-preview"></a>Vorschau auf lokaler Kamera

Sie können `DeviceManager` und `Renderer` verwenden, um mit dem Rendern von Streams über Ihre lokale Kamera zu beginnen. Dieser Stream wird nicht an andere Teilnehmer gesendet. Es handelt sich um einen lokalen Vorschaufeed. Dies ist ein asynchroner Vorgang.

```java
VideoDeviceInfo videoDevice = <get-video-device>;
Context appContext = this.getApplicationContext();
currentVideoStream = new LocalVideoStream(videoDevice, appContext);
videoOptions = new VideoOptions(currentVideoStream);

Renderer previewRenderer = new Renderer(currentVideoStream, appContext);
View uiView = previewRenderer.createView(new RenderingOptions(ScalingMode.Fit));

// Attach the uiView to a viewable location on the app at this point
layout.addView(uiView);
```

## <a name="eventing-model"></a>Ereignismodell
Sie können die meisten Objekte und Sammlungen abonnieren, um bei Änderungen von Werten benachrichtigt zu werden.

### <a name="properties"></a>Eigenschaften
So abonnieren Sie `property changed`-Ereignisse

```java
// subscribe
PropertyChangedListener callStateChangeListener = new PropertyChangedListener()
{
    @Override
    public void onPropertyChanged(PropertyChangedEvent args)
    {
        Log.d("The call state has changed.");
    }
}
call.addOnStateChangedListener(callStateChangeListener);

//unsubscribe
call.removeOnStateChangedListener(callStateChangeListener);
```

### <a name="collections"></a>Auflistungen
So abonnieren Sie `collection updated`-Ereignisse

```java
LocalVideoStreamsChangedListener localVideoStreamsChangedListener = new LocalVideoStreamsChangedListener()
{
    @Override
    public void onLocalVideoStreamsUpdated(LocalVideoStreamsEvent localVideoStreamsEventArgs) {
        Log.d(localVideoStreamsEventArgs.getAddedStreams().size());
        Log.d(localVideoStreamsEventArgs.getRemovedStreams().size());
    }
}
call.addOnLocalVideoStreamsChangedListener(localVideoStreamsChangedListener);
// To unsubscribe
call.removeOnLocalVideoStreamsChangedListener(localVideoStreamsChangedListener);
```
