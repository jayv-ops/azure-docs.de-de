---
title: Architektur für die VMware-/physische Notfallwiederherstellung an einem sekundären Standort mit Azure Site Recovery
description: Dieser Artikel enthält eine Übersicht über die Komponenten und die Architektur, die bei der Notfallwiederherstellung für lokale VMware-VMs oder physische Server unter Windows oder Linux an einem sekundären VMware-Standort mit Azure Site Recovery verwendet werden.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 11/12/2019
ms.author: raynew
ms.openlocfilehash: d400e6bcda0a2114d798a3289f01f52b677a6f94
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "97656494"
---
# <a name="architecture-for-vmwarephysical-server-replication-to-a-secondary-on-premises-site"></a>Architektur für die Replikation von VMware- oder physischen Servern an einem sekundären lokalen Standort

Dieser Artikel beschreibt die Architektur und Prozesse, die beim Einrichten der Replikation, der Ausführung eines Failovers und dem Wiederherstellen von lokalen virtuellen VMware-Computern (VMs) oder physischen Windows-/Linux-Servern für die Notfallwiederherstellung an einem sekundären VMware-Standort mithilfe von [Azure Site Recovery](site-recovery-overview.md) verwendet werden.


## <a name="architectural-components"></a>Komponenten der Architektur

**Bereich** | **Komponente** | **Details**
--- | --- | ---
**Azure** | Sie stellen dieses Szenario mit InMage Scout bereit. | Zum Herunterladen von InMage Scout benötigen Sie ein Azure-Abonnement.<br/><br/> Nachdem Sie einen Recovery Services-Tresor erstellt haben, laden Sie InMage Scout herunter und installieren die aktuellen Updates, um die Bereitstellung einzurichten.
**Prozessserver** | Befindet sich am primären Standort. | Der Prozessserver wird zur Abwicklung von Zwischenspeicherung, Komprimierung und Datenoptimierung bereitgestellt.<br/><br/> Außerdem erfolgt hierüber die Pushinstallation des vereinheitlichten Agents auf Maschinen, die Sie schützen möchten.
**Konfigurationsserver** | Befindet sich am sekundären Standort. | Der Konfigurationsserver dient zum Verwalten, Konfigurieren und Überwachen Ihrer Bereitstellung (entweder über die Verwaltungswebsite oder über die vContinuum-Konsole).
**vContinuum-Server** | Optional. Wird an demselben Standort wie der Konfigurationsserver installiert. | Er enthält eine Konsole zum Verwalten und Überwachen Ihrer geschützten Umgebung.
**Masterzielserver** | Befindet sich am sekundären Standort. | Der Masterzielserver enthält replizierte Daten. Er empfängt Daten vom Prozessserver und erstellt einen Replikatcomputer am sekundären Standort. Außerdem enthält er die Punkte für die Beibehaltung der Daten.<br/><br/> Die Anzahl der benötigten Masterzielserver richtet sich nach der Anzahl der Computer, die Sie schützen.<br/><br/> Wenn Sie ein Failback zum primären Standort durchführen möchten, benötigen Sie dort ebenfalls einen Masterzielserver. Der vereinheitlichte Agent wird auf diesem Server installiert.
**VMware ESX/ESXi und vCenter-Server** |  Virtuelle Computer werden auf ESX/ESXi-Hosts gehostet. Hosts werden mit einem vCenter-Server verwaltet. | Sie benötigen eine VMware-Infrastruktur, um VMware-VMs zu replizieren.
**VMs/physische Server** |  Vereinheitlichter Agent, installiert auf virtuellen VMware-Computern und auf physischen Servern, die Sie replizieren möchten. | Der Agent fungiert als Kommunikationsanbieter zwischen allen Komponenten.

## <a name="set-up-outbound-network-connectivity"></a>Einrichten der ausgehenden Netzwerkkonnektivität

Damit Site Recovery erwartungsgemäß funktioniert, müssen Sie die ausgehende Netzwerkkonnektivität ändern, um Ihrer Umgebung das Replizieren zu ermöglichen.

> [!NOTE]
> Site Recovery unterstützt die Verwendung eines Authentifizierungsproxys zur Steuerung der Netzwerkkonnektivität nicht.

### <a name="outbound-connectivity-for-urls"></a>Ausgehende Konnektivität für URLs

Lassen Sie den Zugriff auf die folgenden URLs zu, wenn Sie einen URL-basierten Firewallproxy zum Steuern der ausgehenden Konnektivität verwenden:

| **Name**                  | **Kommerziell**                               | **Behörden**                                 | **Beschreibung** |
| ------------------------- | -------------------------------------------- | ---------------------------------------------- | ----------- |
| Storage                   | `*.blob.core.windows.net`                  | `*.blob.core.usgovcloudapi.net` | Ermöglicht das Schreiben von Daten aus der VM in das Cachespeicherkonto in der Quellregion |
| Azure Active Directory    | `login.microsoftonline.com`                | `login.microsoftonline.us`                   | Stellt die Autorisierung und Authentifizierung für Site Recovery-Dienst-URLs bereit. |
| Replikation               | `*.hypervrecoverymanager.windowsazure.com` | `*.hypervrecoverymanager.windowsazure.com`   | Ermöglicht die Kommunikation der VM mit Site Recovery |
| Service Bus               | `*.servicebus.windows.net`                 | `*.servicebus.usgovcloudapi.net`             | Ermöglicht es der VM, die Site Recovery-Überwachung und -Diagnosedaten zu schreiben |

## <a name="replication-process"></a>Replikationsprozess

1. Sie richten die Komponentenserver an jedem Standort ein (Konfiguration, Prozess, Masterziel) und installieren den vereinheitlichten Agent auf Computern, die Sie replizieren möchten.
2. Nach der ersten Replikation sendet der Agent auf jedem Computer die Deltareplikationsänderungen an den Prozessserver.
3. Der Prozessserver optimiert die Daten und überträgt sie an den Masterzielserver am sekundären Standort. Der Konfigurationsserver verwaltet den Replikationsprozess.

![Diagramm der Replikation von VMware-VMs und physischen Servern an ein sekundäres Rechenzentrum](./media/site-recovery-components/vmware-to-vmware.png)



## <a name="next-steps"></a>Nächste Schritte

[Richten Sie die Notfallwiederherstellung](vmware-physical-secondary-disaster-recovery.md) von virtuellen VMware-Computern und physischen Servern an einem sekundären Standort ein.
