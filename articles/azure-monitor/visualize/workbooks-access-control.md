---
title: Zugriffssteuerung für Azure Monitor-Arbeitsmappen
description: Vereinfachen der komplexen Berichterstellung mit vordefinierten und benutzerdefiniert parametrisierten Arbeitsmappen mit rollenbasierter Zugriffssteuerung
services: azure-monitor
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 10/23/2019
ms.openlocfilehash: 7d3bc13dc373cda510153099859cf4cd61b3dd69
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101710820"
---
# <a name="access-control"></a>Zugriffssteuerung

Die Zugriffssteuerung in Arbeitsmappen bezieht sich auf zwei Dinge:

* Zugriff, der zum Lesen von Daten in einer-Arbeitsmappe erforderlich ist. Dieser Zugriff wird durch standardmäßige [Azure-Rollen](../../role-based-access-control/overview.md) für die in der Arbeitsmappe verwendeten Ressourcen gesteuert. In Arbeitsmappen ist der Zugriff auf diese Ressourcen weder angegeben noch konfiguriert. Benutzer erhalten diesen Zugriff in der Regel über die Rolle [Überwachungsleser](../../role-based-access-control/built-in-roles.md#monitoring-reader) für diese Ressourcen.

* Zugriff, der zum Speichern von Arbeitsmappen erforderlich ist.

    - Zum Speichern privater Arbeitsmappen `("My")` sind keine zusätzlichen Berechtigungen erforderlich. Alle Benutzer können private Arbeitsmappen speichern, und nur sie können diese Arbeitsmappen sehen.
    - Zum Speichern freigegebener Arbeitsmappen sind Schreibberechtigungen in einer Ressourcengruppe erforderlich, um die Arbeitsmappe zu speichern. Diese Berechtigungen werden in der Regel durch die Rolle [Überwachungsmitwirkender](../../role-based-access-control/built-in-roles.md#monitoring-contributor) festgelegt, können aber auch über die Rolle *Arbeitsmappenmitwirkender* festgelegt werden.
    
## <a name="standard-roles-with-workbook-related-privileges"></a>Standardrollen mit arbeitsmappenbezogenen Berechtigungen

[Überwachungsleser](../../role-based-access-control/built-in-roles.md#monitoring-reader) umfasst standardmäßige „/read“-Berechtigungen, die von Überwachungstools (einschließlich Arbeitsmappen) zum Lesen von Daten aus Ressourcen verwendet werden.

[Überwachungsmitwirkender](../../role-based-access-control/built-in-roles.md#monitoring-contributor) umfasst allgemeine `/write`-Berechtigungen, die von verschiedenen Überwachungstools zum Speichern von Elementen verwendet werden (einschließlich `workbooks/write`-Berechtigung zum Speichern freigegebener Arbeitsmappen).
„Arbeitsmappenmitwirkender“ fügt einem Objekt „workbooks/write“-Berechtigungen hinzu, um freigegebene Arbeitsmappen zu speichern.
Benutzer benötigen zum Speichern privater Arbeitsmappen, die nur ihnen angezeigt werden, keine speziellen Berechtigungen.

Benutzerdefinierte Rollen:

Fügen Sie `microsoft.insights/workbooks/write` zum Speichern freigegebener Arbeitsmappen hinzu. Weitere Informationen finden Sie unter der Rolle [Arbeitsmappenmitwirkender](../../role-based-access-control/built-in-roles.md#monitoring-contributor).

## <a name="next-steps"></a>Nächste Schritte

* [Erfahren Sie mehr](./workbooks-overview.md#visualizations) über die vielen umfassenden Visualisierungsoptionen für Arbeitsmappen.