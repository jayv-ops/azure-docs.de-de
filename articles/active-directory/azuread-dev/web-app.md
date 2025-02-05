---
title: Web-Apps in Azure Active Directory
description: Beschreibt, worum es sich bei Web-Apps handelt, und stellt die Grundlagen des Protokollflusses, der Registrierung und des Tokenablaufs für diesen App-Typ vor.
services: active-directory
documentationcenter: ''
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: azuread-dev
ms.workload: identity
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: ryanwi
ms.reviewer: saeeda, jmprieur
ms.custom: aaddev
ROBOTS: NOINDEX
ms.openlocfilehash: e4a7fb72d40f5db65e8e30264e9d68b2727749e4
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "80154405"
---
# <a name="web-apps"></a>Web-Apps

[!INCLUDE [active-directory-azuread-dev](../../../includes/active-directory-azuread-dev.md)]

Web-Apps sind Anwendungen, die einen Benutzer in einem Webbrowser gegenüber einer Webanwendung authentifizieren. In diesem Szenario weist die Webanwendung den Browser des Benutzers an, diesen in Azure AD anzumelden. Azure AD gibt über den Browser des Benutzers eine Anmeldeantwort mit Benutzeransprüchen in einem Sicherheitstoken zurück. Dieses Szenario unterstützt Anmeldungen über OpenID Connect, SAML 2.0 und WS-Verbundprotokolle.

## <a name="diagram"></a>Diagramm

![Authentifizierungsfluss für „Browser zu Webanwendung“](./media/authentication-scenarios/web-browser-to-web-api.png)

## <a name="protocol-flow"></a>Protokollfluss

1. Wenn ein Benutzer die Anwendung aufruft und sich anmelden muss, wird er über eine Anmeldeanforderung an den Authentifizierungsendpunkt in Azure AD umgeleitet.
1. Der Benutzer meldet sich auf der Anmeldeseite an.
1. Nach erfolgreicher Authentifizierung erstellt Azure AD ein Authentifizierungstoken und gibt eine Anmeldeantwort an die im Azure-Portal konfigurierte Antwort-URL der Anwendung zurück. Bei Produktionsanwendungen empfiehlt sich die Verwendung einer Antwort-URL mit HTTPS. Das zurückgegebene Token enthält Ansprüche für den Benutzer und Azure AD, die die Anwendung zum Überprüfen des Tokens benötigt.
1. Die Anwendung überprüft das Token mithilfe eines öffentlichen Signaturschlüssels und der Ausstellerinformationen aus dem Verbundmetadatendokument für Azure AD. Nach der Überprüfung des Tokens durch die Anwendung startet Azure AD eine neue Sitzung mit dem Benutzer. Anschließend kann der Benutzer bis zum Ablauf der Sitzung auf die Anwendung zugreifen.

## <a name="code-samples"></a>Codebeispiele

Sehen Sie sich die Codebeispiele für Szenarien vom Typ „Webbrowser zu Webanwendung“ an. Schauen Sie außerdem regelmäßig vorbei: Es kommen immer wieder neue Beispiele hinzu.

## <a name="app-registration"></a>App-Registrierung

Informationen zum Registrieren einer Web-App finden Sie unter [Registrieren einer App](../develop/quickstart-register-app.md?toc=/azure/active-directory/azuread-dev/toc.json&bc=/azure/active-directory/azuread-dev/breadcrumb/toc.json).

* Einzelner Mandant: Wenn Sie eine Anwendung nur für Ihre Organisation erstellen, muss diese über das Azure-Portal im Verzeichnis Ihres Unternehmens registriert werden.
* Mehrinstanzenfähige Anwendung: Wenn Sie eine Anwendung erstellen, die von Benutzern außerhalb Ihrer Organisation verwendet werden kann, muss diese ebenfalls im Verzeichnis Ihres Unternehmens registriert werden. Darüber hinaus muss sie aber auch im Verzeichnis jeder anderen Organisation registriert werden, von der die Anwendung verwendet wird. Um Ihre Anwendung in deren Verzeichnis verfügbar zu machen, können Sie einen Registrierungsprozess für Ihre Kunden einschließen, über den sie Ihrer Anwendung zustimmen können. Bei der Registrierung für Ihre Anwendung erscheint ein Dialogfeld mit den erforderlichen Berechtigungen für die Anwendung sowie mit einer Zustimmungsoption. Je nachdem, welche Berechtigungen erforderlich sind, wird unter Umständen die Zustimmung eines Administrators aus der anderen Organisation benötigt. Wenn der Benutzer oder Administrator seine Zustimmung gibt, wird die Anwendung in dessen Verzeichnis registriert.

## <a name="token-expiration"></a>Tokenablauf

Die Sitzung des Benutzers läuft ab, wenn die Gültigkeitsdauer des von Azure AD ausgestellten Tokens abläuft. Ihre Anwendung kann diesen Zeitraum bei Bedarf verkürzen und Benutzer beispielsweise bei zu langer Inaktivität abmelden. Nach Ablauf der Sitzung wird der Benutzer aufgefordert, sich erneut anzumelden.

## <a name="next-steps"></a>Nächste Schritte

* Weitere Informationen zu anderen [Anwendungstypen und -szenarien](app-types.md)
* Weitere Informationen zu den [Authentifizierungsgrundlagen](v1-authentication-scenarios.md) von Azure AD
