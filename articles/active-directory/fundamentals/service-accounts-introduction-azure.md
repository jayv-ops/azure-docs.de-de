---
title: Einführung in das Sichern von Azure Active Directory-Dienstkonten
description: Erläuterung der in Azure Active Directory verfügbaren Dienstkontotypen.
services: active-directory
author: BarbaraSelden
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 3/1/2021
ms.author: baselden
ms.reviewer: ajburnle
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 049025a5d871f1dd26e5dab498756aa44d2ebfe2
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101692905"
---
# <a name="introduction-to-securing-azure-service-accounts"></a>Einführung in das Sichern von Azure-Dienstkonten

Es gibt drei Typen von nativen Dienstkonten in Azure Active Directory: verwaltete Identitäten, Dienstprinzipale und benutzerbasierte Dienstkonten. Dienstkonten sind ein spezieller Kontotyp, der eine nicht menschliche Entität, z. B. eine Anwendung, eine API oder einen anderen Dienst darstellen soll. Diese Entitäten sind innerhalb des Sicherheitskontexts aktiv, der vom Dienstkonto bereitgestellt wird. 

## <a name="types-of-azure-active-directory-service-accounts"></a>Typen von Azure Active Directory-Dienstkonten

Für in Azure gehostete Dienste sollte nach Möglichkeit eine verwaltete Identität und andernfalls ein Dienstprinzipal verwendet werden. Verwaltete Identitäten können nicht für Dienste verwendet werden, die außerhalb von Azure gehostet werden. In diesem Fall wird ein Dienstprinzipal empfohlen. Wenn Sie eine verwaltete Identität oder einen Dienstprinzipal verwenden können, sollten Sie dies tun. Sie sollten kein Azure Active Directory-Benutzerkonto als Dienstprinzipal verwenden. Eine Zusammenfassung finden Sie in der folgenden Tabelle.
 

| Diensthosting| Verwaltete Identität| Dienstprinzipal| Azure-Benutzerkonto |
| - | - | - | - |
|Der Dienst wird in Azure gehostet.| Ja. <br>Empfohlen, wenn der Dienst <br>eine verwaltete Identität unterstützt.| Ja.| Nicht empfehlenswert. |
| Der Dienst wird nicht in Azure gehostet.| Nein| Ja. Empfohlen.| Nicht empfehlenswert. |
| Der Dienst ist mehrinstanzenfähig.| Nein| Ja. Empfohlen.| Nein. |


## <a name="managed-identities"></a>Verwaltete Identitäten

Verwaltete Identitäten sind sichere Azure AD-Identitäten (Azure Active Directory), die für die Bereitstellung von Identitäten für Azure-Ressourcen erstellt wurden. Es gibt [zwei Typen von verwalteten Identitäten](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview#managed-identity-types): 
 
* Systemseitig zugewiesene verwaltete Identitäten können direkt einer Instanz eines Diensts zugewiesen werden. 

* Benutzerseitig zugewiesene verwaltete Identitäten können als eigenständige Ressourcen erstellt werden. 

Weitere Informationen finden Sie unter [Sichern von verwalteten Identitäten](service-accounts-managed-identities.md). Allgemeine Informationen zu verwalteten Identitäten finden Sie unter [Was sind verwaltete Identitäten für Azure-Ressourcen?](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview).

## <a name="service-principals"></a>Dienstprinzipale

Wenn Sie keine verwaltete Identität zur Darstellung Ihrer Anwendung verwenden können, verwenden Sie einen Dienstprinzipal. Dienstprinzipale können sowohl mit Einzelinstanz- als auch mit mehrinstanzenfähigen Anwendungen verwendet werden. 

Ein Dienstprinzipal ist die lokale Darstellung eines Anwendungsobjekts in einem einzelnen Azure AD-Mandanten. Er fungiert als Identität der Anwendungsinstanz, definiert, wer auf die Anwendung zugreifen kann und auf welche Ressourcen die Anwendung zugreifen kann. Ein Dienstprinzipal wird (lokal) in jedem Mandanten erstellt, in dem die Anwendung verwendet wird, und verweist auf das global eindeutige Anwendungsobjekt. Der Mandant sichert die Anmeldung des Dienstprinzipals und den Zugriff auf Ressourcen.

Es gibt zwei Mechanismen zur Authentifizierung mithilfe von Dienstprinzipalen – Clientzertifikate und geheime Clientschlüssel. Zertifikate sind sicherer: Verwenden Sie nach Möglichkeit Clientzertifikate. Im Gegensatz zu geheimen Clientschlüsseln können Clientzertifikate nicht versehentlich in Code eingebettet werden.

Informationen zum Sichern von Dienstprinzipalen finden Sie unter „Sichern von Dienstprinzipalen“.

 
## <a name="next-steps"></a>Nächste Schritte


Weitere Informationen zum Sichern von Azure-Dienstkonten finden Sie unter:

[Sichern von verwalteten Identitäten](service-accounts-managed-identities.md)

[Sichern von Dienstprinzipalen](service-accounts-principal.md)

[Steuern von Azure-Dienstkonten](service-accounts-governing-azure.md)



