---
title: Entschärfungen – Microsoft Threat Modeling Tool – Azure | Microsoft-Dokumentation
description: Die Seite „Entschärfungen“ für das Microsoft Threat Modeling Tool mit möglichen Lösungen für die meisten generierten Bedrohungen.
services: security
documentationcenter: na
author: jegeib
manager: jegeib
editor: jegeib
ms.assetid: na
ms.service: security
ms.subservice: security-develop
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: rodsan
ms.openlocfilehash: 748d10b994080b667885e5d0d5f4d688269e86ab
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "68728047"
---
# <a name="microsoft-threat-modeling-tool-mitigations"></a>Microsoft Threat Modeling Tool-Entschärfungen

Das Threat Modeling Tool ist ein Kernelement im Microsoft Security Development Lifecycle (SDL). Es ermöglicht Softwarearchitekten, potenzielle Sicherheitslücken früh zu identifizieren und zu entschärfen, wenn sie relativ einfach und kostengünstig gelöst werden können. Daher werden mit dem Tool die Gesamtkosten der Entwicklung erheblich reduziert. Außerdem wurde das Tool nicht speziell für Sicherheitsexperten entwickelt. Es erleichtert die Modellierung von Bedrohungen für alle Entwickler, indem klare Leitlinien zum Erstellen und Analysieren von Gefahrenmodellen bereitgestellt werden.

**[Laden Sie das Threat Modeling Tool herunter](threat-modeling-tool.md)**, um noch heute die ersten Schritte zu unternehmen!

## <a name="mitigation-categories"></a>Kategorien von Entschärfungen

Die Gegenmaßnahmen im Threat Modeling Tool sind gemäß dem Sicherheitsrahmen für Webanwendungen in folgende Kategorien unterteilt:

| Category | BESCHREIBUNG |
| -------- | ----------- |
| **[Überwachung und Protokollierung](threat-modeling-tool-auditing-and-logging.md)** | Wer hat was wann gemacht? Überwachung und Protokollierung beziehen sich darauf, wie Ihre Anwendung Sicherheitsereignisse aufzeichnet. |
| **[Authentifizierung](threat-modeling-tool-authentication.md)** | Wer sind Sie? Authentifizierung ist der Prozess, in dem eine Entität die Identität einer anderen Entität beweist, in der Regel durch Anmeldeinformationen wie Benutzername und Kennwort. |
| **[Authorization](threat-modeling-tool-authorization.md)** | Was können Sie tun? Die Autorisierung legt fest, wie Ihre Anwendung Zugriffssteuerung für Ressourcen und Vorgänge bereitstellt. |
| **[Kommunikationssicherheit](threat-modeling-tool-communication-security.md)** | Mit wem kommunizieren Sie? Durch Kommunikationssicherheit wird gewährleistet, dass die gesamte Kommunikation so sicher wie möglich abläuft. |
| **[Konfigurationsverwaltung](threat-modeling-tool-configuration-management.md)** | Mit welchem Konto wird die Anwendung ausgeführt? Mit welchen Datenbanken stellt sie Verbindungen her? Wie wird Ihre Anwendung verwaltet? Wie werden diese Einstellungen gesichert? Die Konfigurationsverwaltung bezieht sich darauf, wie Ihre Anwendung diese Betriebsprobleme behandelt. |
| **[Kryptografie](threat-modeling-tool-cryptography.md)** | Wie werden geheime Schlüssel aufbewahrt (Vertraulichkeit)? Wie sichern Sie Ihre Daten oder Bibliotheken gegen Manipulationen (Integrität)? Wie stellen Sie Startwerte für Zufallswerte bereit, die kryptografisch sicher sein müssen? Kryptografie bezieht sich darauf, wie die Anwendung Vertraulichkeit und Integrität erzwingt. |
| **[Verwaltung von Ausnahmen](threat-modeling-tool-exception-management.md)** | Wie reagiert Ihre Anwendung, wenn beim Aufruf einer Methode in der Anwendung ein Fehler auftritt? Wie viel legen Sie offen? Geben Sie verständliche Fehlerinformationen an Endbenutzer zurück? Geben Sie wertvolle Ausnahmeinformationen an den Aufrufer zurück? Wird die Anwendung ordnungsgemäß abgebrochen? |
| **[Eingabeüberprüfung](threat-modeling-tool-input-validation.md)** | Woher wissen Sie, dass die Eingaben, die Ihre Anwendung empfängt, gültig und sicher sind? Die Eingabeüberprüfung bezieht sich darauf, wie Ihre Anwendung Eingaben vor der weiteren Verarbeitung filtert, bereinigt oder ablehnt. Sie können Eingaben über Einstiegspunkte einschränken und Ausgaben über Ausgabepunkte codieren. Vertrauen Sie Daten aus Quellen wie Datenbanken und Dateifreigaben? |
| **[Sensible Daten](threat-modeling-tool-sensitive-data.md)** | Wie werden in Ihrer Anwendung sensible Daten behandelt? Sensible Daten beziehen sich darauf, wie Ihre Anwendung Daten behandelt, die im Arbeitsspeicher, im Netzwerk oder in permanenten Speichern geschützt werden müssen. |
| **[Sitzungsverwaltung](threat-modeling-tool-session-management.md)** | Wie verarbeitet und schützt Ihre Anwendung Benutzersitzungen? Eine Sitzung bezieht sich auf eine Reihe von zusammengehörenden Interaktionen zwischen einem Benutzer und Ihre Webanwendung. |

Damit können Sie Folgendes identifizieren:

* Wo werden die häufigsten Fehler gemacht
* Wo sind die am besten umsetzbaren Verbesserungen möglich

So können Sie diese Kategorien verwenden, um Ihre Bemühungen um Sicherheit zu konzentrieren und zu priorisieren. Wenn Sie z.B. wissen, dass die häufigsten Sicherheitsprobleme in den Kategorien Eingabeüberprüfung, Authentifizierung und Autorisierung auftreten, können Sie mit diesen Kategorien beginnen. Klicken Sie auf **[diesen Link zum Patent](https://www.google.com/patents/US7818788)**, um weitere Informationen zu erhalten.

## <a name="next-steps"></a>Nächste Schritte

Besuchen Sie **[Threat Modeling Tool-Bedrohungen](threat-modeling-tool-threats.md)**, um mehr über die Bedrohungskategorien zu erfahren, die das Tool verwendet, um mögliche Entwurfsbedrohungen zu generieren.