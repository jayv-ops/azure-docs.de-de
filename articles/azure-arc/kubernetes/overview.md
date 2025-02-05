---
title: Übersicht über Kubernetes mit Azure Arc-Unterstützung
services: azure-arc
ms.service: azure-arc
ms.date: 03/03/2021
ms.topic: overview
author: mlearned
ms.author: mlearned
description: Dieser Artikel enthält eine Übersicht über Kubernetes mit Azure Arc-Unterstützung.
keywords: Kubernetes, Arc, Azure, Container
ms.custom: references_regions
ms.openlocfilehash: b3296aa1c1d5d463cc7f4cb932b44ad76632e19e
ms.sourcegitcommit: dac05f662ac353c1c7c5294399fca2a99b4f89c8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "102121591"
---
# <a name="what-is-azure-arc-enabled-kubernetes"></a>Was ist Kubernetes mit Azure Arc-Aktivierung?

Mithilfe von Kubernetes mit Azure Arc-Aktivierung können Sie Kubernetes-Cluster, die sich entweder innerhalb oder außerhalb von Azure befinden, anfügen und konfigurieren. Wenn Sie eine Verbindung von einem Kubernetes-Cluster mit Azure Arc herstellen, tritt Folgendes für den Cluster auf:
* Er wird im Azure-Portal mit einer Azure Resource Manager-ID und einer verwalteten Identität angezeigt. 
* Er wird in einem Azure-Abonnement und einer Ressourcengruppe platziert.
* Er empfängt Tags genau wie jede andere Azure-Ressource. 

Zum Herstellen einer Verbindung zwischen einem Kubernetes-Cluster und Azure muss ein Clusteradministrator entsprechende Agents bereitstellen. Folgendes gilt für diese Agents:
* Sie werden im Kubernetes-Namespace `azure-arc` als Kubernetes-Standardbereitstellungen ausgeführt.
* Sie verwalten die Konnektivität mit Azure.
* Sie sammeln Azure Arc-Protokolle und -Metriken.
* Sie achten auf Konfigurationsanforderungen. 

Kubernetes mit Azure Arc-Unterstützung unterstützt branchenübliches SSL zum Schützen von Daten während der Übertragung. Die Daten werden außerdem im Ruhezustand verschlüsselt in einer Azure Cosmos DB-Datenbank gespeichert, um ihre Vertraulichkeit zu gewährleisten.

## <a name="supported-kubernetes-distributions"></a>Unterstützte Kubernetes-Distributionen

Kubernetes mit Azure Arc-Aktivierung funktioniert mit jedem Kubernetes-Cluster, der eine CNCF-Zertifizierung (Cloud Native Computing Foundation) besitzt. Das Azure Arc-Team hat mit [wichtigen Partnern dieser Branche zusammengearbeitet](./validation-program.md), um die Konformität ihrer Kubernetes-Distributionen mit Kubernetes mit Azure Arc-Aktivierung zu überprüfen.

## <a name="supported-scenarios"></a>Unterstützte Szenarios 

Kubernetes mit Azure Arc-Aktivierung unterstützt die folgenden Szenarien: 

* Verbinden von außerhalb von Azure ausgeführtem Kubernetes für Bestand, Gruppierung und Kennzeichnung

* Bereitstellen von Anwendungen und Anwenden der Konfiguration mithilfe der GitOps-basierten Konfigurationsverwaltung 

* Anzeigen und Überwachen von Clustern mithilfe von Azure Monitor für Container 

* Anwenden von Richtlinien mit Azure Policy für Kubernetes 

[!INCLUDE [azure-lighthouse-supported-service](../../../includes/azure-lighthouse-supported-service.md)]

## <a name="supported-regions"></a>Unterstützte Regionen 

Kubernetes mit Azure Arc-Unterstützung wird derzeit in diesen Regionen unterstützt: 

* East US
* Europa, Westen
* USA, Westen-Mitte
* USA Süd Mitte
* Asien, Südosten
* UK, Süden
* USA, Westen 2
* Australien (Osten)
* USA (Ost) 2
* Nordeuropa

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie einen Cluster mit Azure Arc verbinden.
> [!div class="nextstepaction"]
> [Verbinden eines Clusters mit Azure Arc](./quickstart-connect-cluster.md)
