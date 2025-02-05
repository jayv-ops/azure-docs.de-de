---
title: Was ist Azure Communication Services?
description: Erfahren Sie, wie Azure Communication Services Sie bei der Entwicklung umfassender Benutzererfahrungen mit Echtzeitkommunikation unterstützt.
author: mikben
manager: jken
services: azure-communication-services
ms.author: mikben
ms.date: 03/10/2021
ms.topic: overview
ms.service: azure-communication-services
ms.openlocfilehash: 40dacdf243505447e9831239ef1252602a3e0f99
ms.sourcegitcommit: 4bda786435578ec7d6d94c72ca8642ce47ac628a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/16/2021
ms.locfileid: "103491689"
---
# <a name="what-is-azure-communication-services"></a>Was ist Azure Communication Services?

[!INCLUDE [Public Preview Notice](./includes/public-preview-include.md)]

> [!IMPORTANT]
> Mithilfe von Azure Communication Services erstellte Anwendungen können mit Microsoft Teams kommunizieren. Weitere Informationen finden Sie in unserer Dokumentation zur [Teams-Interoperabilität](./quickstarts/voice-video-calling/get-started-teams-interop.md).


Azure Communication Services ermöglicht es Ihnen, Echtzeit-Multimediafunktionen für die Sprach-, Video- und IP-Telefoniekommunikation zu Ihren Anwendungen hinzuzufügen. Mit den Communication Services-Clientbibliotheken können Sie auch Chat- und SMS-Funktionen zu ihren Kommunikationslösungen hinzufügen.

<br>

> [!VIDEO https://www.youtube.com/embed/apBX7ASurgM]

<br>
<br>

Verwenden Sie Communication Services für die Sprach-, Video-, Text- und Datenkommunikation in einer Vielzahl von Szenarien:

- Browser-zu-Browser-, Browser-zu-App- und App-zu-App-Kommunikation
- Benutzer, die mit Bots oder anderen Diensten interagieren
- Benutzer und Bots, die über das Telefonfestnetz interagieren

Gemischte Szenarien werden ebenfalls unterstützt. Eine Communication Services-Anwendung kann beispielsweise über Benutzer verfügen, die gleichzeitig sowohl über Browser als auch über per Telefon kommunizieren. Communication Services kann auch mit Azure Bot Service kombiniert werden, um botgesteuerte Systeme für interaktive Sprachantworten (Interactive Voice Response, IVR) zu erstellen.

## <a name="common-scenarios"></a>Häufige Szenarien

Die folgenden Ressourcen sind ein idealer Ausgangspunkt, wenn Sie mit Azure Communication Services noch nicht vertraut sind:
<br>

| Resource                               |BESCHREIBUNG                           |
|---                                    |---                                   |
|**[Beitreten mit einer Telefonie-App zu einer Teams-Besprechung](./quickstarts/voice-video-calling/get-started-teams-interop.md)**|Azure Communication Services kann dazu verwendet werden, benutzerdefinierte Besprechungsumgebungen zu erstellen, die mit Microsoft Teams interagieren. Benutzer Ihrer Communication Services-Lösung(en) können mit den Teams-Teilnehmern über Sprache, Video, Chat und Bildschirmfreigabe interagieren.|
|**[Erstellen einer Communication Services-Ressource](./quickstarts/create-communication-resource.md)**|Beginnen Sie mit der Verwendung von Azure Communication Services, indem Sie das Azure-Portal oder die Communication Services-Clientbibliothek verwenden, um Ihre erste Communication Services-Ressource bereitzustellen. Sobald Sie über die Communication Services-Verbindungszeichenfolge verfügen, können Sie Ihre ersten Benutzerzugriffstoken bereitstellen.|
|**[Erstellen Ihrer ersten Benutzerzugriffstoken](./quickstarts/access-tokens.md)**|Benutzerzugriffstoken werden verwendet, um ihre Dienste für Ihre Azure Communication Services-Ressource zu authentifizieren. Diese Token werden mithilfe der Communication Services-Clientbibliothek bereitgestellt und erneut ausgestellt.|
|**[Beschaffen einer Telefonnummer](./quickstarts/telephony-sms/get-phone-number.md)**|Verwenden Sie Azure Communication Services zum Bereitstellen und Freigeben von Telefonnummern. Diese Telefonnummern können zum Initiieren ausgehender Anrufe und zum Erstellen von SMS-Kommunikationslösungen verwendet werden.|
|**[Versenden einer SMS von Ihrer App](./quickstarts/telephony-sms/send.md)**|Die Clientbibliothek für SMS von Azure Communication Services ermöglicht das Senden und Empfangen von SMS-Nachrichten von .NET- und JavaScript-Anwendungen.|
|**[Erste Schritte mit Sprach- und Videoanrufen](./quickstarts/voice-video-calling/getting-started-with-calling.md)**| Azure Communication Services ermöglicht es Ihnen, mithilfe der Clientbibliothek für Telefonie Sprach- und Videoanrufe zu Ihren Apps hinzuzufügen. Diese Bibliothek wird von WebRTC unterstützt und ermöglicht Ihnen das Einrichten von Peer-zu-Peer-, Multimedia- und Echtzeitkommunikation innerhalb Ihrer Anwendungen.|
|**[Erste Schritte mit dem Chat](./quickstarts/chat/get-started.md)**|Die Clientbibliothek für Chats von Azure Communication Services kann verwendet werden, um Echtzeitchat zu Ihren Anwendungen hinzuzufügen.|


## <a name="samples"></a>Proben

Die folgenden Beispiele veranschaulichen die vollständige Verwendung der Azure Communication Services-Clientbibliotheken. Nutzen Sie diese Beispiele für einen Bootstrap Ihrer Communication Services-Lösungen.
<br>

| Name des Beispiels                               | Beschreibung                           |
|---                                    |---                                   |
|**[Hero-Beispiel für Gruppenanrufe](./samples/calling-hero-sample.md)**|Erfahren Sie, wie die Communication Services-Clientbibliotheken verwendet werden können, um eine Gruppenanruffunktion zu erstellen.|
|**[Hero-Beispiel für Gruppenchats](./samples/chat-hero-sample.md)**|Erfahren Sie, wie die Communication Services-Clientbibliotheken verwendet werden können, um eine Gruppenchatfunktion zu erstellen.|


## <a name="platforms-and-client-libraries"></a>Plattformen und Clientbibliotheken

Mithilfe der folgenden Ressourcen erfahren Sie mehr über die Azure Communication Services-Clientbibliotheken:

| Resource                               | Beschreibung                           |
|---                                    |---                                   |
|**[Clientbibliotheken und REST-APIs](./concepts/sdk-options.md)**|Azure Communication Services-Funktionen sind vom Prinzip her in sechs Bereiche unterteilt, die durch jeweils eine Clientbibliothek dargestellt sind. Basierend auf Ihren Anforderungen an die Echtzeitkommunikation können Sie entscheiden, welche Clientbibliotheken verwendet werden sollen.|
|**[Übersicht über die Clientbibliothek für Telefonie](./concepts/voice-video-calling/calling-sdk-features.md)**|Lesen Sie die Übersicht über die Clientbibliothek für Telefonie von Communication Services.|
|**[Übersicht über die Clientbibliothek für Chats](./concepts/chat/sdk-features.md)**|Lesen Sie die Übersicht über die Clientbibliothek für Chats von Communication Services.|
|**[Übersicht über die Clientbibliothek für SMS](./concepts/telephony-sms/sdk-features.md)**|Lesen Sie die Übersicht über die Clientbibliothek für SMS von Communication Services.|

## <a name="compare-azure-communication-services"></a>Vergleichen von Azure Communication Services

Es gibt zwei weitere Microsoft-Kommunikationsprodukte, die Sie nutzen können, die derzeit nicht direkt mit Communication Services interoperabel sind:

 - [Microsoft Graph Cloud Communication-APIs](/graph/cloud-communications-concept-overview) ermöglichen es Organisationen, Kommunikationsfunktionen zu erstellen, die mit Azure Active Directory-Benutzern mit M365-Lizenzen verknüpft sind. Dies ist ideal für Anwendungen, die an Azure Active Directory gebunden sind, oder wenn Sie Produktivitätsumgebungen in Microsoft Teams erweitern möchten. Es gibt auch APIs zum Erstellen von Anwendungen und Anpassungen innerhalb der [Teams-Funktionen.](/microsoftteams/platform/?preserve-view=true&view=msteams-client-js-latest)

 - [Azure PlayFab Party](/gaming/playfab/features/multiplayer/networking/) vereinfacht das Hinzufügen von Chat- und Datenkommunikation mit geringer Latenz zu Spielen. Spielechats und Netzwerksysteme können zwar mit Communication Services unterstützt werden, PlayFab ist jedoch eine maßgeschneiderte Option und auf Xbox kostenlos.


## <a name="next-steps"></a>Nächste Schritte

 - [Erstellen einer Communication Services-Ressource](./quickstarts/create-communication-resource.md)
