---
title: Migrieren von Verbindungsmonitor zu Verbindungsmonitor
titleSuffix: Azure Network Watcher
description: Es wird beschrieben, wie Sie eine Migration von Verbindungsmonitor zu Verbindungsmonitor durchführen.
services: network-watcher
documentationcenter: na
author: vinynigam
ms.service: network-watcher
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/07/2021
ms.author: vinigam
ms.openlocfilehash: d4ab5361d245ad1ee10d43184cc0a2d65fed2054
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101730030"
---
# <a name="migrate-to-connection-monitor-from-connection-monitor-classic"></a>Migrieren von Verbindungsmonitor (klassisch) zu Verbindungsmonitor

> [!IMPORTANT]
> Ab dem 1. Juli 2021 können Sie keine neuen Verbindungsmonitore in Verbindungsmonitor (klassisch) mehr hinzufügen, vorhandene Verbindungsmonitore, die vor dem 1. Juli 2021 erstellt wurden, können Sie jedoch weiterhin verwenden. [Migrieren Sie vor dem 29. Februar 2024 vom Verbindungsmonitor (klassisch) zum neuen Verbindungsmonitor](migrate-to-connection-monitor-from-connection-monitor-classic.md) in Azure Network Watcher, um Dienstunterbrechungen im Zusammenhang mit Ihren aktuellen Workloads zu minimieren.

Sie können vorhandene Verbindungsmonitore mit wenigen Mausklicks und ohne Ausfallzeit zum neuen, verbesserten Verbindungsmonitor migrieren. Weitere Informationen zu den Vorteilen finden Sie unter [Verbindungsmonitor](./connection-monitor-overview.md).

## <a name="key-points-to-note"></a>Wichtige Hinweise

Die Migration hilft beim Liefern der folgenden Ergebnisse:

* Die Funktionen von Agents und Firewalleinstellungen bleiben unverändert erhalten. Es sind keine Änderungen erforderlich. 
* Vorhandene Verbindungsmonitore werden „Verbindungsmonitor > Testgruppe > Testformat“ zugeordnet. Sie können **Bearbeiten** auswählen, um die Eigenschaften des neuen Verbindungsmonitors anzuzeigen und zu ändern. Laden Sie eine Vorlage herunter, um Änderungen am Verbindungsmonitor vorzunehmen und diese über Azure Resource Manager zu übermitteln. 
* Virtuelle Azure-Computer mit der Network Watcher-Erweiterung senden Daten an den Arbeitsbereich und die Metriken. Verbindungsmonitore stellen die Daten über die neuen Metriken „ChecksFailedPercent“ und „RoundTripTimeMs“ anstatt über die veralteten Metriken „ProbesFailedPercent“ und „AverageRoundtripMs“ zur Verfügung. Die alten Metriken werden zu neuen Metriken migriert, also ProbesFailedPercent zu ChecksFailedPercent und AverageRoundtripMs zu RoundTripTimeMs.
* Datenüberwachung:
   * **Warnungen**: Wurden automatisch zu den neuen Metriken migriert.
   * **Analysen und Integrationen**: Erfordern die manuelle Bearbeitung des Satzes der Metriken. 
    
## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie einen benutzerdefinierten Arbeitsbereich verwenden, stellen Sie sicher, dass Network Watcher im Abonnement und in der Region des Log Analytics-Arbeitsbereichs aktiviert ist. 

## <a name="migrate-the-connection-monitors"></a>Migrieren der Verbindungsmonitore

1. Wählen Sie **Verbindungsmonitor** und dann **Verbindungsmonitore migrieren** aus, um die älteren Verbindungsmonitore zu den neueren zu migrieren.

    ![Screenshot: Migration von Verbindungsmonitoren zu Verbindungsmonitor](./media/connection-monitor-2-preview/migrate-cm-to-cm-preview.png)
    
1. Wählen Sie Ihr Abonnement und die Verbindungsmonitore aus, die Sie migrieren möchten, und wählen Sie dann **Ausgewählte migrieren** aus. 

Sie haben mit nur wenigen Klicks die Migration von vorhandenen Verbindungsmonitoren zu Verbindungsmonitor durchgeführt. Nach der Migration vom Verbindungsmonitor (klassisch) zum Verbindungsmonitor wird kein Monitor unter „Verbindungsmonitor (klassisch)“ angezeigt.

Sie können jetzt die Verbindungsmonitor-Eigenschaften anpassen, den Standardarbeitsbereich ändern, Vorlagen herunterladen und den Migrationsstatus überprüfen. 

Nach dem Starten der Migration werden die folgenden Änderungen durchgeführt: 
* Aus der Azure Resource Manager-Ressource wird der neuere Verbindungsmonitor.
    * Der Name, die Region und das Abonnement des Verbindungsmonitors bleiben unverändert erhalten. Die Ressourcen-ID wird nicht geändert.
    * Sofern der Verbindungsmonitor nicht angepasst wurde, wird im Abonnement und in der Region des Verbindungsmonitors ein Log Analytics-Standardarbeitsbereich erstellt. In diesem Arbeitsbereich werden die Überwachungsdaten gespeichert. Zudem werden in den Metriken die Testergebnisdaten gespeichert.
    * Jeder Test wird zu einer Testgruppe mit dem Namen *defaultTestGroup* migriert.
    * Quell- und Zielendpunkte werden in der neuen Testgruppe erstellt und verwendet. Die Standardnamen lauten *defaultSourceEndpoint* und *defaultDestinationEndpoint*.
    * Der Zielport und das Testintervall werden in eine Testkonfiguration mit dem Namen *defaultTestConfiguration* verschoben. Das Protokoll wird anhand der Portwerte festgelegt. Erfolgsschwellenwerte und andere optionale Eigenschaften bleiben leer.
* Metrikwarnungen werden zu den Metrikwarnungen von Verbindungsmonitor migriert. Da die Metriken unterschiedlich sind, ist diese Änderung erforderlich. Weitere Informationen finden Sie unter [Überwachung der Netzwerkkonnektivität mit dem Verbindungsmonitor: Metriken in Azure Monitor](./connection-monitor-overview.md#metrics-in-azure-monitor).
* Die migrierten Verbindungsmonitore werden nicht mehr als ältere Verbindungsmonitor-Lösung angezeigt. Sie sind jetzt nur noch im Verbindungsmonitor-Feature verfügbar.
* Externe Integrationen, z. B. Dashboards in Power BI und Grafana, sowie Integrationen mit SIEM-Systemen (Security Information and Event Management) müssen manuell migriert werden. Dies ist der einzige manuelle Schritt, den Sie zum Migrieren dieser Einrichtung ausführen müssen.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu Verbindungsmonitor finden Sie in den folgenden Artikeln:
* [Migrieren von Netzwerkleistungsmonitor zu Verbindungsmonitor](./migrate-to-connection-monitor-from-network-performance-monitor.md)
* [Erstellen eines Verbindungsmonitors mithilfe des Azure-Portals](./connection-monitor-create-using-portal.md)
