---
title: include file
description: include file
services: azure-communication-services
author: mikben
manager: mikben
ms.service: azure-communication-services
ms.subservice: azure-communication-services
ms.date: 03/10/2021
ms.topic: include
ms.custom: include file
ms.author: mikben
ms.openlocfilehash: 146053ffd72b24216bfa86577787727257da2516
ms.sourcegitcommit: 4bda786435578ec7d6d94c72ca8642ce47ac628a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/16/2021
ms.locfileid: "103495403"
---
## <a name="prerequisites"></a>Voraussetzungen

- Ein Azure-Konto mit einem aktiven Abonnement. Sie können [kostenlos ein Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- [Java Development Kit (JDK)](/java/azure/jdk/), Version 8 oder höher.
- [Apache Maven](https://maven.apache.org/download.cgi).
- Eine bereitgestellte Communication Services-Ressource und eine Verbindungszeichenfolge. [Erstellen Sie eine Communication Services-Ressource](../../create-communication-resource.md).
- Ein [Benutzerzugriffstoken](../../access-tokens.md). Achten Sie darauf, dass Sie den Bereich auf „Chat“ festlegen, und notieren Sie sich die Tokenzeichenfolge und die userId-Zeichenfolge.


## <a name="setting-up"></a>Einrichten

### <a name="create-a-new-java-application"></a>Erstellen einer neuen Java-Anwendung

Öffnen Sie Ihr Terminal- oder Befehlsfenster, und navigieren Sie zu dem Verzeichnis, in dem Sie Ihre Java-Anwendung erstellen möchten. Führen Sie den unten angegebenen Befehl aus, um das Java-Projekt aus der Vorlage „maven-archetype-quickstart“ zu generieren.

```console
mvn archetype:generate -DgroupId=com.communication.quickstart -DartifactId=communication-quickstart -DarchetypeArtifactId=maven-archetype-quickstart -DarchetypeVersion=1.4 -DinteractiveMode=false
```

Sie sehen, dass über das Ziel „generate“ ein Verzeichnis mit dem gleichen Namen wie für die „artifactId“ erstellt wurde. In diesem Verzeichnis enthält `src/main/java directory` den Quellcode des Projekts, das Verzeichnis `src/test/java` enthält die Testquelle, und die Datei „pom.xml“ ist das Projektobjektmodell ([POM](https://maven.apache.org/guides/introduction/introduction-to-the-pom.html)) des Projekts.

Aktualisieren Sie die POM-Datei Ihrer Anwendung für die Verwendung von Java 8 oder höher:

```xml
<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
</properties>
```

### <a name="add-the-package-references-for-the-chat-client-library"></a>Hinzufügen der Paketverweise für die Chatclientbibliothek

Verweisen Sie in Ihrer POM-Datei auf das Paket `azure-communication-chat` mit den Chat-APIs:

```xml
<dependency>
    <groupId>com.azure</groupId>
    <artifactId>azure-communication-chat</artifactId>
    <version>1.0.0-beta.4</version> 
</dependency>
```

Für die Authentifizierung muss Ihr Client auf das Paket `azure-communication-common` verweisen:

```xml
<dependency>
    <groupId>com.azure</groupId>
    <artifactId>azure-communication-common</artifactId>
    <version>1.0.0</version> 
</dependency>
```

## <a name="object-model"></a>Objektmodell

Die folgenden Klassen und Schnittstellen werden für einige der wichtigsten Features der Java-Clientbibliothek für Chats von Azure Communication Services verwendet.

| Name                                  | Beschreibung                                                  |
| ------------------------------------- | ------------------------------------------------------------ |
| ChatClient | Diese Klasse wird für die Chatfunktionalität benötigt. Sie instanziieren sie mit Ihren Abonnementinformationen und verwenden sie zum Erstellen, Abrufen und Löschen von Threads. |
| ChatAsyncClient | Diese Klasse wird für die asynchrone Chatfunktionalität benötigt. Sie instanziieren sie mit Ihren Abonnementinformationen und verwenden sie zum Erstellen, Abrufen und Löschen von Threads. |
| ChatThreadClient | Diese Klasse wird für die Chatthreadfunktionalität benötigt. Sie rufen eine Instanz über den ChatClient ab und verwenden sie zum Senden/Empfangen/Aktualisieren/Löschen von Nachrichten, Hinzufügen/Entfernen/Abrufen von Benutzern und Senden von Eingabebenachrichtigungen und Lesebestätigungen. |
| ChatThreadAsyncClient | Diese Klasse wird für die asynchrone Chatthreadfunktionalität benötigt. Sie rufen eine Instanz über den ChatAsyncClient ab und verwenden sie zum Senden/Empfangen/Aktualisieren/Löschen von Nachrichten, Hinzufügen/Entfernen/Abrufen von Benutzern und Senden von Eingabebenachrichtigungen und Lesebestätigungen. |

## <a name="create-a-chat-client"></a>Erstellen eines Chatclients
Zum Erstellen eines Chatclients verwenden Sie den Communication Services-Endpunkt und das Zugriffstoken, das im Rahmen der Schritte zur Erfüllung der Voraussetzungen generiert wurde. Mit Benutzerzugriffstoken können Sie Clientanwendungen erstellen, die gegenüber Azure Communication Services direkt authentifiziert werden. Nachdem Sie diese Token auf Ihrem Server generiert haben, übergeben Sie sie zurück an ein Clientgerät. Sie müssen die Klasse „CommunicationTokenCredential“ aus der allgemeinen Clientbibliothek verwenden, um das Token an Ihren Chatclient zu übergeben. 

Weitere Informationen zur [Chatarchitektur](../../../concepts/chat/concepts.md)

Achten Sie beim Hinzufügen der Importanweisungen darauf, nur Importe aus den Namespaces „com.azure.communication.chat“ und „com.azure.communication.chat.models“ hinzuzufügen (nicht aus dem Namespace „com.azure.communication.chat.implementation“). In der Datei „App.java“, die über Maven generiert wurde, können Sie den folgenden Code verwenden, um zu beginnen:

```Java
import com.azure.communication.chat.*;
import com.azure.communication.chat.models.*;
import com.azure.communication.common.*;
import com.azure.core.http.HttpClient;

import java.io.*;

public class App
{
    public static void main( String[] args ) throws IOException
    {
        System.out.println("Azure Communication Services - Chat Quickstart");
        
        // Your unique Azure Communication service endpoint
        String endpoint = "https://<RESOURCE_NAME>.communication.azure.com";

        // Create an HttpClient builder of your choice and customize it
        // Use com.azure.core.http.netty.NettyAsyncHttpClientBuilder if that suits your needs
        NettyAsyncHttpClientBuilder yourHttpClientBuilder = new NettyAsyncHttpClientBuilder();
        HttpClient httpClient = yourHttpClientBuilder.build();

        // User access token fetched from your trusted service
        String userAccessToken = "<USER_ACCESS_TOKEN>";

        // Create a CommunicationTokenCredential with the given access token, which is only valid until the token is valid
        CommunicationTokenCredential userCredential = new CommunicationTokenCredential(userAccessToken);

        // Initialize the chat client
        final ChatClientBuilder builder = new ChatClientBuilder();
        builder.endpoint(endpoint)
            .credential(userCredential)
            .httpClient(httpClient);
        ChatClient chatClient = builder.buildClient();
    }
}
```


## <a name="start-a-chat-thread"></a>Starten eines Chatthreads

Verwenden Sie die `createChatThread`-Methode, um einen Chatthread zu erstellen.
`createChatThreadOptions` wird zum Beschreiben der Threadanforderung verwendet.

- Verwenden Sie `topic`, um für diesen Chat ein Thema anzugeben. Nach der Erstellung des Chatthreads kann das Thema mit der Funktion `UpdateThread` aktualisiert werden.
- Verwenden Sie `participants`, um die Threadteilnehmer aufzulisten, die dem Thread hinzugefügt werden sollen. Für `ChatParticipant` wird der Benutzer verwendet, den Sie in der Schnellstartanleitung [Benutzerzugriffstoken](../../access-tokens.md) erstellt haben.

Die Antwort `chatThreadClient` wird verwendet, um Vorgänge für den erstellten Chatthread durchzuführen: Hinzufügen von Teilnehmern zum Chatthread, Senden einer Nachricht, Löschen einer Nachricht usw. Sie enthält die `chatThreadId`-Eigenschaft, bei der es sich um die eindeutige ID des Chatthreads handelt. Auf die Eigenschaft kann über die öffentliche „.getChatThreadId()“-Methode zugegriffen werden.

```Java
List<ChatParticipant> participants = new ArrayList<ChatParticipant>();

ChatParticipant firstThreadParticipant = new ChatParticipant()
    .setCommunicationIdentifier(firstUser)
    .setDisplayName("Participant Display Name 1");
    
ChatParticipant secondThreadParticipant = new ChatParticipant()
    .setCommunicationIdentifier(secondUser)
    .setDisplayName("Participant Display Name 2");

participants.add(firstThreadParticipant);
participants.add(secondThreadParticipant);

CreateChatThreadOptions createChatThreadOptions = new CreateChatThreadOptions()
    .setTopic("Topic")
    .setParticipants(participants);
ChatThreadClient chatThreadClient = chatClient.createChatThread(createChatThreadOptions);
String chatThreadId = chatThreadClient.getChatThreadId();
```

## <a name="send-a-message-to-a-chat-thread"></a>Senden einer Nachricht an einen Chatthread

Verwenden Sie die `sendMessage`-Methode zum Senden einer Nachricht an den Thread, den Sie gerade erstellt haben und der anhand der „chatThreadId“ identifiziert wird.
`sendChatMessageOptions` wird verwendet, um die Anforderung der Chatnachricht zu beschreiben.

- Verwenden Sie `content`, um den Inhalt der Chatnachricht anzugeben.
- Verwenden Sie `type`, um den Inhaltstyp der Chatnachricht (Text oder HTML) anzugeben.
- Verwenden Sie `senderDisplayName`, um den Anzeigenamen des Absenders anzugeben.

Die Antwort `sendChatMessageResult` enthält eine `id`, bei der es sich um die eindeutige ID der Nachricht handelt.

```Java
SendChatMessageOptions sendChatMessageOptions = new SendChatMessageOptions()
    .setContent("Message content")
    .setType(ChatMessageType.TEXT)
    .setSenderDisplayName("Sender Display Name");

SendChatMessageResult sendChatMessageResult = chatThreadClient.sendMessage(sendChatMessageOptions);
String chatMessageId = sendChatMessageResult.getId();
```


## <a name="get-a-chat-thread-client"></a>Abrufen eines Clients für den Chatthread

Die `getChatThreadClient`-Methode gibt einen Threadclient für einen bereits vorhandenen Thread zurück. Er kann verwendet werden, um Vorgänge wie das Hinzufügen von Teilnehmern oder das Senden einer Nachricht im erstellten Thread auszuführen. `chatThreadId` ist die eindeutige ID des vorhandenen Chatthreads.

```Java
String chatThreadId = "Id";
ChatThread chatThread = chatClient.getChatThread(chatThreadId);
```

## <a name="receive-chat-messages-from-a-chat-thread"></a>Empfangen von Chatnachrichten aus einem Chatthread

Sie können Chatnachrichten abrufen, indem Sie die `listMessages`-Methode auf dem Chatthreadclient jeweils nach dem angegebenen Intervallzeitraum abfragen.

```Java
chatThreadClient.listMessages().iterableByPage().forEach(resp -> {
    System.out.printf("Response headers are %s. Url %s  and status code %d %n", resp.getHeaders(),
        resp.getRequest().getUrl(), resp.getStatusCode());
    resp.getItems().forEach(message -> {
        System.out.printf("Message id is %s.", message.getId());
    });
});
```

`listMessages` gibt die aktuelle Version der Nachricht zurück, einschließlich aller Bearbeitungen oder Löschungen, die für die Nachricht mit „.editMessage()“ und „.deleteMessage()“ durchgeführt wurden. Für gelöschte Nachrichten wird von `chatMessage.getDeletedOn()` ein datetime-Wert zurückgegeben, mit dem der Löschzeitpunkt der Nachricht angegeben wird. Für bearbeitete Nachrichten gibt `chatMessage.getEditedOn()` einen datetime-Wert zurück, mit dem der Bearbeitungszeitpunkt der Nachricht angegeben wird. Auf den ursprünglichen Zeitpunkt der Nachrichtenerstellung kann mit `chatMessage.getCreatedOn()` zugegriffen werden. Dieses Element kann zum Sortieren der Nachrichten genutzt werden.

Mit `listMessages` werden unterschiedliche Nachrichtentypen zurückgegeben, die mit `chatMessage.getType()` identifiziert werden können. Diese Typen lauten:

- `text`: Reguläre Chatnachricht, die von einem Threadteilnehmer gesendet wurde

- `html`: HTML-Chatnachricht, die von einem Threadteilnehmer gesendet wurde

- `topicUpdated`: Systemnachricht, die angibt, dass das Thema aktualisiert wurde

- `participantAdded`: Systemnachricht mit dem Hinweis, dass dem Chatthread mindestens ein Teilnehmer hinzugefügt wurde

- `participantRemoved`: Systemnachricht mit dem Hinweis, dass ein Teilnehmer aus dem Chatthread entfernt wurde

Weitere Details finden Sie unter [Nachrichtentypen](../../../concepts/chat/concepts.md#message-types).

## <a name="add-a-user-as-participant-to-the-chat-thread"></a>Hinzufügen eines Benutzers als Teilnehmer des Chatthreads

Nach der Erstellung eines Chatthreads können Sie dafür Benutzer hinzufügen und entfernen. Indem Sie Benutzer hinzufügen, gewähren Sie ihnen Zugriff zum Senden von Nachrichten an den Chatthread und zum Hinzufügen/Entfernen anderer Teilnehmer. Zunächst müssen Sie ein neues Zugriffstoken und eine Identität für den Benutzer abrufen. Stellen Sie vor dem Aufrufen der addParticipants-Methode sicher, dass Sie ein neues Zugriffstoken und eine Identität für den Benutzer beschafft haben. Der Benutzer benötigt dieses Zugriffstoken, um den Chatclient initialisieren zu können.

Verwenden Sie die `addParticipants`-Methode zum Hinzufügen von Teilnehmern zum Thread, der durch „threadId“ identifiziert wird.

- Verwenden Sie `listParticipants`, um die Teilnehmer aufzulisten, die dem Chatthread hinzugefügt werden sollen.
- `communicationIdentifier` (erforderlich) ist das CommunicationIdentifier-Element, das Sie mit „CommunicationIdentityClient“ in der Schnellstartanleitung [Benutzerzugriffstoken](../../access-tokens.md) erstellt haben.
- `display_name` (optional) ist der Anzeigename für den Threadteilnehmer.
- `share_history_time` (optional) ist der Zeitpunkt, ab dem der Chatverlauf für den Teilnehmer freigegeben wird. Sie können den Verlauf seit dem Beginn des Chatthreads freigeben, indem Sie diese Eigenschaft auf das Datum der Threaderstellung (oder früher) festlegen. Soll der Verlauf vor dem Hinzufügezeitpunkt des Teilnehmers nicht freigegeben werden, geben Sie das aktuelle Datum an. Geben Sie ein gewünschtes Datum an, um nur einen Teil des Verlaufs freizugeben.

```Java
List<ChatParticipant> participants = new ArrayList<ChatParticipant>();

ChatParticipant firstThreadParticipant = new ChatParticipant()
    .setCommunicationIdentifier(identity1)
    .setDisplayName("Display Name 1");

ChatParticipant secondThreadParticipant = new ChatParticipant()
    .setCommunicationIdentifier(identity2)
    .setDisplayName("Display Name 2");

participants.add(firstThreadParticipant);
participants.add(secondThreadParticipant);

AddChatParticipantsOptions addChatParticipantsOptions = new AddChatParticipantsOptions()
    .setParticipants(participants);
chatThreadClient.addParticipants(addChatParticipantsOptions);
```

## <a name="remove-participant-from-a-chat-thread"></a>Entfernen eines Teilnehmers aus einem Chatthread

Teilnehmer können auch aus einem Chatthread entfernt werden. Die Vorgehensweise ist dabei ähnlich wie beim Hinzufügen zu einem Thread. Hierfür müssen Sie die Identitäten der von Ihnen hinzugefügten Teilnehmer nachverfolgen.

Verwenden Sie `removeParticipant`. Hierbei ist `identifier` das von Ihnen erstellte CommunicationIdentifier-Element.

```Java
chatThreadClient.removeParticipant(identity);
```

## <a name="run-the-code"></a>Ausführen des Codes

Navigieren Sie zum Verzeichnis, das die Datei *pom.xml* enthält, und kompilieren Sie das Projekt mit dem folgenden `mvn`-Befehl.

```console
mvn compile
```

Erstellen Sie dann das Paket.

```console
mvn package
```

Führen Sie die App mit dem folgenden `mvn`-Befehl aus.

```console
mvn exec:java -Dexec.mainClass="com.communication.quickstart.App" -Dexec.cleanupDaemonThreads=false
```