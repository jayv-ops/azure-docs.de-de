---
title: Konfigurieren der Überwachung des Features „Gastintegrität“ in VM Insights (Vorschau)
description: In diesem Artikel wird beschrieben, wie Sie die Standardüberwachung für das Feature „Gastintegrität“ in VM Insights (Vorschau) über das Azure-Portal ändern.
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 12/14/2020
ms.openlocfilehash: 0f6599bb9f379cf471dafbb83a7deefbb05c0dbe
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "102052210"
---
# <a name="configure-monitoring-in-vm-insights-guest-health-preview"></a>Konfigurieren der Überwachung des Features „Gastintegrität“ in VM Insights (Vorschau)
Mit dem Feature „Gastintegrität“ von VM Insights können Sie Informationen zur Integrität einer VM anzeigen. Dies wird anhand von verschiedenen Leistungsmessungen definiert, für die in regelmäßigen Abständen Stichproben genommen werden. In diesem Artikel wird beschrieben, wie Sie die Standardüberwachung mit dem Azure-Portal ändern können. Darüber hinaus enthält er auch eine Beschreibung der grundlegenden Konzepte von „Monitoren“ (Überwachungskomponenten), die zum [Konfigurieren der Überwachung per Datensammlungsregel](vminsights-health-configure-dcr.md) erforderlich sind.

## <a name="open-monitor-configuration"></a>Öffnen der Monitorkonfiguration
Öffnen Sie im Azure-Portal die Monitorkonfiguration, indem Sie den Monitor und dann die Registerkarte **Konfiguration** auswählen.

[![Monitordetails: Konfiguration](media/vminsights-health-overview/monitor-details-configuration.png)](media/vminsights-health-overview/monitor-details-configuration.png#lightbox)

## <a name="enable-or-disable-monitors"></a>Aktivieren oder Deaktivieren von Monitoren
Sowohl Einheitenmonitore als auch Aggregatmonitore verfügen über die Einstellung **Status der Integritätsüberwachung**, mit der Sie die Option aktivieren und deaktivieren können. Wenn ein Monitor aktiviert ist, wird seine Integrität angezeigt und zum Festlegen der Integrität der VM verwendet. Wenn ein Monitor deaktiviert ist, wird seine Integrität nicht berechnet und nicht verwendet, um die Integrität der VM festzulegen.

| Einstellung | BESCHREIBUNG |
|:---|:---|
| Aktiviert | Der Monitor wird unabhängig von der Einstellung des übergeordneten Elements aktiviert. |
| Disabled | Der Monitor wird unabhängig von der Einstellung des übergeordneten Elements deaktiviert. |
| Mit übergeordnetem Element identisch | Der Monitor wird in Abhängigkeit der Einstellung des übergeordneten Elements aktiviert bzw. deaktiviert. |

Wenn ein Monitor deaktiviert ist, werden alle Kriterien als nicht verfügbar angezeigt. Dies ist im Beispiel unten dargestellt.

![Deaktivierter Monitor](media/vminsights-health-configure/disabled-monitor.png)


> [!NOTE]
> Wenn ein übergeordneter Monitor deaktiviert ist, werden alle untergeordneten Monitore ebenfalls deaktiviert. Wenn Sie den untergeordneten Monitor explizit aktivieren, wird das übergeordnete Element ebenfalls aktiviert, aber sein Konfigurationsstatus bleibt unverändert. In diesem Fall wird im übergeordneten Monitor die folgende Meldung angezeigt.
>
> *Es liegt eine Diskrepanz vor, weil der konfigurierte Status des Monitors „Deaktiviert“ lautet, dies jedoch im Integritätsstatus nicht widerspiegelt wird. Dies liegt entweder daran, dass die konfigurierten Änderungen noch weitergegeben werden oder einer der untergeordneten Monitore explizit aktiviert wurde.*

## <a name="enable-or-disable-virtual-machine"></a>Aktivieren oder Deaktivieren eines virtuellen Computers
Sie können die Überwachung für einen virtuellen Computer deaktivieren, um alle Monitore vorübergehend anzuhalten. Ein Beispiel für die Deaktivierung der Überwachung einer VM ist die Durchführung von Wartungsarbeiten.

| Einstellung | BESCHREIBUNG |
|:---|:---|
| Aktiviert  | Der Integritätsstatus des Computers wird angezeigt. |
| Disabled | Der Integritätsstatus des Computers wird als **Deaktiviert** angezeigt. Es werden keine Warnungen erstellt. |

## <a name="health-state-logic"></a>Integritätsstatuslogik
Mit der Integritätsstatuslogik eines Einheitenmonitors werden die Kriterien zum Festlegen des Integritätsstatus definiert. Sie können angeben, wie viele Integritätsstatus ein Monitor hat und welcher Schwellenwert für die Berechnung des jeweiligen Integritätsstatus gelten soll.

![Beispiel für Integritätskriterien](media/vminsights-health-configure/sample-health-criteria.png)

Bei Aggregatmonitoren wird keine Integritätsstatuslogik angegeben. Der Integritätsstatus wird hierbei von den zugehörigen untergeordneten Einheitenmonitoren gemäß dem jeweiligen Integritätsrollup festgelegt.

Einheitenmonitore verfügen jeweils über zwei oder drei Integritätsstatus. Der Status „Fehlerfrei“ ist immer vorhanden, und optional ist zusätzlich der Status „Warnung“ oder „Kritisch“ vorhanden (oder beide). Für die Status „Warnung“ und „Kritisch“ sind jeweils eindeutige Kriterien erforderlich, mit denen definiert wird, wann der Monitor auf den jeweiligen Status festgelegt wird. Für den Status „Fehlerfrei“ sind keine Kriterien vorhanden. Er wird festgelegt, wenn die Kriterien für die anderen Status nicht erfüllt sind.

Im folgenden Beispiel wird die CPU-Auslastung auf die folgenden Integritätsstatus festgelegt:

- „Kritisch“, wenn der Wert größer als 90 % ist.
- „Warnung“, wenn der Wert größer oder gleich 80 % ist.
- „Fehlerfrei“, wenn der Wert unter 80 % liegt. Dies ist logisch, weil dies der Status ist, in dem keine der anderen Bedingungen zutrifft.

## <a name="next-steps"></a>Nächste Schritte

- [Konfigurieren von Monitoren im großen Stil mit Datensammlungsregeln](vminsights-health-configure-dcr.md)