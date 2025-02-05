---
title: Übersicht über Backup Center
description: Dieser Artikel enthält eine Übersicht über Backup Center für Azure.
ms.topic: conceptual
ms.date: 09/30/2020
ms.openlocfilehash: b42bb2719d03108212f62428dc97ed814899c63c
ms.sourcegitcommit: 15d27661c1c03bf84d3974a675c7bd11a0e086e6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2021
ms.locfileid: "102506049"
---
# <a name="overview-of-backup-center"></a>Übersicht über Backup Center

Backup Center bietet eine **zentralisierte einheitliche Verwaltungsumgebung** in Azure, mit der Unternehmen Sicherungen in großem Maßstab steuern, überwachen, betreiben und analysieren können. Als solches ist Backup Center konsistent mit den nativen Verwaltungsumgebungen von Azure.

Zu den wichtigsten Vorteilen von Backup Center gehören:

* **Zentralisierte Benutzeroberfläche zum Verwalten von Sicherungen:** Backup Center ist für die Verwendung in einer großen und verteilten Azure-Umgebung ausgelegt. Mit Backup Center können Sie Sicherungen, die mehrere Workloadtypen, Tresore, Abonnements, Regionen und [Azure Lighthouse](../lighthouse/overview.md)-Mandanten umfassen, effizient verwalten.
* **Datenquellenzentrierte Verwaltung:** Backup Center bietet Ansichten und Filter, die auf die zu sichernden Datenquellen (z. B. VMs und Datenbanken) ausgerichtet sind. Dies ermöglicht es Ressourcenbesitzern oder Sicherungsadministratoren, Sicherungen von Elementen zu überwachen und betreiben, ohne sich damit befassen zu müssen, in welchem Tresor ein Element gesichert wird. Ein wesentliches Merkmal dieses Designs ist die Möglichkeit, Ansichten nach datenquellenspezifischen Eigenschaften zu filtern, wie z. B. nach Datenquellenabonnement, Datenquellen-Ressourcengruppe oder Datenquellentags. Wenn Ihre Organisation beispielsweise üblicherweise VMs, die verschiedenen Abteilungen angehören, unterschiedliche Tags zuweist, können Sie Backup Center verwenden, um Sicherungsinformationen auf der Grundlage der Tags der zugrunde liegenden VMs, die gesichert werden, zu filtern, ohne sich auf das Tag des Tresors konzentrieren müssen.
* **Verbundene Erfahrungen:** Backup Center bietet native Integrationen in bestehende Azure-Dienste, die eine Verwaltung in großem Maßstab ermöglichen. Beispielsweise nutzt Backup Center die [Azure Policy](../governance/policy/overview.md)-Umgebung, um Ihnen bei der Steuerung Ihrer Sicherungen zu helfen. Es nutzt auch [Azure-Arbeitsmappen](../azure-monitor/visualize/workbooks-overview.md) und [Azure Monitor-Protokolle](../azure-monitor/logs/data-platform-logs.md), um Ihnen zu helfen, detaillierte Berichte über Sicherungen anzuzeigen. Sie müssen also keine neuen Prinzipien erlernen, um die vielfältigen Features, die Backup Center bietet, nutzen zu können. Sie können über Backup Center auch Communityressourcen ermitteln.

## <a name="supported-scenarios"></a>Unterstützte Szenarios

* Backup Center wird derzeit für Sicherungen folgender Produkte unterstützt: Azure-VMs, SQL auf Azure-VMs, SAP HANA auf Azure-VMs, Azure Files, Azure Blobs, Azure Managed Disks und Azure Database for PostgreSQL-Server.
* Eine ausführliche Liste der unterstützten und nicht unterstützten Szenarien finden Sie in der [Supportmatrix](backup-center-support-matrix.md).

## <a name="get-started"></a>Erste Schritte

Um mit der Verwendung von Backup Center zu beginnen, suchen Sie im Azure-Portal nach **Backup Center**, und navigieren Sie zum Dashboard **Backup Center**.

![Backup Center-Suche](./media/backup-center-overview/backup-center-search.png)

Der erste Bildschirm, der angezeigt wird, ist der Bildschirm **Übersicht**. Er enthält zwei Kacheln – **Aufträge** und **Sicherungsinstanzen**.

![Backup Center-Kacheln](./media/backup-center-overview/backup-center-overview-widgets.png)

Über die Kachel **Aufträge** erhalten Sie eine zusammenfassende Ansicht aller Sicherungs- und Wiederherstellungsaufträge, die in den letzten 24 Stunden in Ihrem gesamten Sicherungsbestand ausgelöst wurden. Sie können Informationen über die Anzahl der abgeschlossenen, fehlgeschlagenen und in Bearbeitung befindlichen Aufträge anzeigen. Wenn Sie eine der Zahlen auf dieser Kachel auswählen, können Sie weitere Informationen zu Aufträgen für einen bestimmten Datenquellentyp, Vorgangstyp und Status anzeigen.

Über die Kachel **Sicherungsinstanzen** erhalten Sie eine zusammenfassende Ansicht aller Sicherungsinstanzen in Ihrem Sicherungsbestand. Sie können z. B. die Anzahl der Sicherungsinstanzen sehen, die sich im Zustand „Vorläufig gelöscht“ befinden, und der Anzahl der Instanzen gegenüberstellen, die noch für den Schutz konfiguriert sind. Wenn Sie eine der Zahlen auf dieser Kachel auswählen, können Sie weitere Informationen zu Sicherungsinstanzen für einen bestimmten Datenquellentyp und Schutzzustand anzeigen. Sie können auch alle Sicherungsinstanzen anzeigen, deren zugrunde liegende Datenquelle nicht gefunden wurde (die Datenquelle wurde möglicherweise gelöscht, oder Sie haben keinen Zugriff auf die Datenquelle).

Sehen Sie sich das folgende Video an, um die Funktionen von Backup Center zu verstehen:

> [!VIDEO https://www.youtube.com/embed/pFRMBSXZcUk?t=497]

Führen Sie die [nächsten Schritte](#next-steps) aus, um die verschiedenen Funktionen von Backup Center zu verstehen und zu erfahren, wie Sie diese Funktionen zur effizienten Verwaltung Ihres Sicherungsbestands nutzen können.

## <a name="next-steps"></a>Nächste Schritte

* [Überwachen und Betreiben von Sicherungen](backup-center-monitor-operate.md)
* [Steuern Ihres Sicherungsbestands](backup-center-govern-environment.md)
* [Gewinnen von Einblicken in Ihre Sicherungen](backup-center-obtain-insights.md)
* [Ausführen von Aktionen mithilfe von Backup Center](backup-center-actions.md)