---
title: 'Zusammenarbeit mit anderen: QnA Maker'
description: Hier erfahren Sie, wie Sie mithilfe der rollenbasierten Zugriffssteuerung in Azure mit anderen Autoren und Editoren zusammenarbeiten.
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 05/15/2020
ms.openlocfilehash: 5d5a580e2b7be4699933b43687dcf164bf8f4a4a
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101700081"
---
# <a name="collaborate-with-other-authors-and-editors"></a>Zusammenarbeiten mit anderen Autoren und Redakteuren

Nutzen Sie die rollenbasierte Zugriffssteuerung in Azure (Azure RBAC) für Ihre QnA Maker-Ressource, um mit anderen Autoren und Redakteuren zusammenzuarbeiten.

## <a name="access-is-provided-on-the-qna-maker-resource"></a>Zugriff auf die QnA Maker-Ressource

Alle Berechtigungen werden durch die Berechtigungen gesteuert, die für die QnA Maker-Ressource festgelegt wurden. Bei diesen Berechtigungen handelt es sich um Berechtigungen für Lese-, Schreib-, Veröffentlichungs- und Vollzugriff.

Dieses Azure RBAC-Feature umfasst Folgendes:
* Azure Active Directory (AAD) ist vollständig mit der schlüsselbasierten Authentifizierung für Besitzer und Mitwirkende abwärtskompatibel. Kunden können in ihren Anforderungen entweder die schlüsselbasierte Authentifizierung oder die Azure RBAC-basierte Authentifizierung verwenden.
* Da die Steuerung auf der Ressourcenebene und nicht auf der Ebene der Wissensdatenbank erfolgt, können Autoren und Redakteure schnell allen Wissensdatenbanken in der Ressource hinzugefügt werden.

> [!NOTE]
> Stellen Sie sicher, dass eine benutzerdefinierte Unterdomäne für die Ressource hinzugefügt wird. [Eine benutzerdefinierte Unterdomäne](../../cognitive-services-custom-subdomains.md) sollte standardmäßig vorhanden sein. Wenn dies nicht der Fall ist, fügen Sie eine hinzu.

## <a name="access-is-provided-by-a-defined-role"></a>Zugriff durch eine definierte Rolle

[!INCLUDE [Azure RBAC permissions table](../includes/role-based-access-control.md)]

## <a name="authentication-flow"></a>Authentifizierungsfluss

Das folgende Diagramm zeigt den Ablauf für die Anmeldung beim QnA Maker-Portal und die Verwendung der Erstellungs-APIs aus Sicht des Autors:

> [!div class="mx-imgBorder"]
> ![Dieses Diagramm zeigt den Ablauf für die Anmeldung beim QnA Maker-Portal und die Verwendung der Erstellungs-APIs aus Sicht des Autors.](../media/qnamaker-how-to-collaborate-knowledge-base/rbac-flow-from-portal-to-service.png)

|Schritte|Beschreibung|
|--|--|
|1|Vom Portal wird ein Token für die QnA Maker-Ressource abgerufen.|
|2|Vom Portal wird die entsprechende QnA Maker-Erstellungs-API (APIM) aufgerufen und das Token übergeben (anstelle von Schlüsseln).|
|3|Das Token wird von der QnA Maker-API überprüft.|
|4 |Von der QnA Maker-API wird der QnAMaker-Dienst aufgerufen.|

Wenn Sie die [Erstellungs-APIs](../index.yml) aufrufen möchten, informieren Sie sich ausführlicher über die Einrichtung der Authentifizierung.

## <a name="authenticate-by-qna-maker-portal"></a>Authentifizieren über das QnA Maker-Portal

Wenn Sie das QnA Maker-Portal verwenden, um Inhalte zu erstellen und mit anderen zusammenzuarbeiten, werden sämtliche Zugriffsberechtigungen durch das QnA Maker-Portal verwaltet, nachdem Sie der Ressource die entsprechende Rolle für einen Projektmitarbeiter hinzugefügt haben.

## <a name="authenticate-by-qna-maker-apis-and-sdks"></a>Authentifizieren über QnA Maker-APIs und -SDKs

Wenn Sie zum Erstellen von Inhalten sowie für die Zusammenarbeit mit anderen die APIs verwenden (entweder per REST oder über die SDKs), müssen Sie zur Verwaltung der Authentifizierung [einen Dienstprinzipal erstellen](../../authentication.md#assign-a-role-to-a-service-principal).

## <a name="next-step"></a>Nächster Schritt

* Entwerfen Sie eine Wissensdatenbank für Sprachen und für Clientanwendungen.