---
title: Grundlegendes zur Auftragsüberwachung in Azure Stream Analytics
description: In diesem Artikel wird das Überwachen von Azure Stream Analytics-Aufträgen im Azure-Portal beschrieben.
author: sidramadoss
ms.author: sidram
ms.service: stream-analytics
ms.topic: how-to
ms.date: 03/08/2021
ms.custom: seodec18
ms.openlocfilehash: 6d3558511721a91c3a195cb510a1a00d5d8a9a51
ms.sourcegitcommit: 8d1b97c3777684bd98f2cfbc9d440b1299a02e8f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2021
ms.locfileid: "102487877"
---
# <a name="understand-stream-analytics-job-monitoring-and-how-to-monitor-queries"></a>Grundlegendes zur Stream Analytics-Auftragsüberwachung und zum Überwachen von Abfragen

## <a name="introduction-the-monitor-page"></a>Einführung: Seite „Monitor“
Im Azure-Portal werden wichtige Leistungsmetriken angezeigt, die zum Überwachen der Leistung Ihrer Abfragen und Aufträge sowie für die Problembehandlung verwendet werden können. Navigieren Sie zum Anzeigen dieser Metriken zum Stream Analytics-Auftrag, dessen Metriken Sie interessieren, und zeigen Sie auf der Seite „Übersicht“ den Abschnitt **Überwachung** an.  

![Link zur Stream Analytics-Auftragsüberwachung](./media/stream-analytics-monitoring/02-stream-analytics-monitoring-block.png)

Das Fenster wird wie folgt angezeigt:

![Dashboard der Stream Analytics-Auftragsüberwachung](./media/stream-analytics-monitoring/01-stream-analytics-monitoring.png)  

## <a name="metrics-available-for-stream-analytics"></a>Verfügbare Metriken für Stream Analytics
| Metrik                 | Definition                               |
| ---------------------- | ---------------------------------------- |
| Eingabeereignisse im Rückstand       | Anzahl der Eingabeereignisse, die im Rückstand sind. Ein Wert ungleich Null für diese Metrik bedeutet, dass Ihr Auftrag nicht in der Lage ist, mit der Anzahl der eingehenden Ereignisse Schritt zu halten. Wenn dieser Wert langsam ansteigt oder konstant ungleich Null ist, sollten Sie Ihren Auftrag aufskalieren. Weitere Informationen finden Sie unter [Verstehen und Anpassen von Streamingeinheiten](stream-analytics-streaming-unit-consumption.md). |
| Konvertierungsfehler | Anzahl der Ausgabeereignisse, die nicht in das erwartete Ausgabeschema konvertiert werden konnten. Die Fehlerrichtlinie kann auf „Drop“ geändert werden, um Ereignisse zu löschen, bei denen dieses Szenario auftritt. |
| CPU-Auslastung in Prozent (Vorschau)       | Hierbei handelt es sich um die CPU-Auslastung Ihres Auftrags in Prozent. Selbst wenn dieser Wert sehr hoch ist (90 % oder höher), sollten Sie die Anzahl der SUs nicht allein aufgrund dieser Metrik erhöhen. Wenn die Anzahl der Eingabeereignisse im Rückstand oder die Wasserzeichenverzögerung zunimmt, können Sie diese CPU%-Auslastungsmetrik verwenden, um festzustellen, ob die CPU der Engpass ist. Es ist möglich, dass diese Metrik zeitweilig Spitzenwerte aufweist. Es wird empfohlen, Skalierungstests durchzuführen, um die Obergrenze Ihres Auftrags zu bestimmen, nach der die Eingaben in Rückstand geraten können oder die Wasserzeichenverzögerung aufgrund von CPU-Engpässen zunimmt. |
| Frühe Eingabeereignisse       | Ereignisse, deren Anwendungszeitstempel mehr als fünf Minuten vor dem Zeitpunkt ihres Eintreffens liegt. |
| Fehlerhafte Funktionsanforderungen | Anzahl der fehlerhaften Aufrufe an die Azure Machine Learning-Funktion (falls vorhanden) |
| Funktionsereignisse        | Anzahl der an die Azure Machine Learning-Funktion gesendeten Ereignisse (falls vorhanden) |
| Funktionsanforderungen      | Anzahl der Aufrufe an die Azure Machine Learning-Funktion (falls vorhanden) |
| Eingabefehler bei Deserialisierung       | Anzahl der Eingabeereignisse, die nicht deserialisiert werden konnten.  |
| Eingabeereignisbytes      | Vom Stream Analytics-Auftrag erhaltene Datenmenge, in Byte. Kann verwendet werden, um sicherzustellen, dass Ereignisse an die Eingabequelle gesendet werden. |
| Eingabeereignisse           | Anzahl der Datensätze, die aus den Eingabeereignissen deserialisiert wurden. Diese Anzahl umfasst nicht eingehende Ereignisse, die zu Deserialisierungsfehlern führen. Die gleichen Ereignisse können in Szenarios wie internen Wiederherstellungen und Selbstverknüpfungen erfasst werden. Daher wird empfohlen, keine Übereinstimmungen der Metriken für Eingabe- und Ausgabeereignisse zu erwarten, wenn Ihr Auftrag eine einfache Pass-Through-Abfrage enthält. |
| Empfangene Eingabequellen       | Anzahl der vom Auftrag empfangenen Nachrichten. Für Event Hub besteht eine Nachricht aus einem einzelnen EventData. Für ein Blob ist eine Nachricht ein einzelnes Blob. Beachten Sie, dass die Eingabequellen vor der Deserialisierung gezählt werden. Bei Deserialisierungsfehlern können die Eingabequellen größer sein als die Eingabeereignisse. Andernfalls können sie kleiner als oder gleich den Eingabeereignissen sein, da jede Nachricht mehrere Ereignisse enthalten kann. |
| Ereignisse bei verspäteter Eingabe      | Ereignisse, die später als das konfigurierte Toleranzfenster für Eingangsverzögerung eingetroffen sind. Weitere Informationen finden Sie unter [Überlegungen zur Ereignisreihenfolge in Azure Stream Analytics](./stream-analytics-time-handling.md). |
| Ereignisse für falsche Reihenfolge    | Anzahl der Ereignisse, die in falscher Reihenfolge empfangen und anhand der Richtlinie für die Ereignissortierung entweder verworfen oder mit einem angepassten Zeitstempel versehen wurden. Dies kann von der Konfiguration der Einstellung „Toleranzfenster für Fehlordnung“ beeinflusst werden. |
| Ausgabeereignisse          | Vom Stream Analytics-Auftrag an das Ausgabeziel gesendete Datenmenge, ausgedrückt in der Anzahl von Ereignissen. |
| Laufzeitfehler         | Gesamtanzahl von Fehlern im Zusammenhang mit der Abfrageverarbeitung (mit Ausnahme von Fehlern, die beim Untersuchen von Ereignissen oder beim Ausgeben von Ergebnissen ermittelt werden) |
| Speichereinheitnutzung in %       | Hierbei handelt es sich um die Arbeitsspeicherauslastung Ihres Auftrags in Prozent. Wenn die SU-Auslastung konstant mehr als 80 Prozent beträgt, die Verzögerung für einen Grenzwert steigt und die Anzahl der protokollierten Ereignisse zunimmt, ziehen Sie eine Erhöhung der Streamingeinheiten in Erwägung. Eine hohe Auslastung deutet darauf hin, dass der Auftrag fast die Höchstzahl der bereitgestellten Ressourcen verwendet. |
| Wasserzeichenverzögerung       | Die maximale Wasserzeichenverzögerung für alle Partitionen aller Ausgaben im Auftrag. |

Sie können diese Metriken zum [Überwachen der Leistung des Stream Analytics-Auftrags](./stream-analytics-set-up-alerts.md#scenarios-to-monitor) verwenden. 

## <a name="customizing-monitoring-in-the-azure-portal"></a>Anpassen der Überwachung im Azure-Portal
Sie können den Diagrammtyp, die angezeigten Metriken und den Uhrzeitbereich in den Einstellungen unter "Diagramm bearbeiten" anpassen. Weitere Informationen finden Sie unter [Anpassen der Überwachung](../azure-monitor/data-platform.md).

  ![Zeitdiagramm für die Stream Analytics-Abfrageüberwachung](./media/stream-analytics-monitoring/08-stream-analytics-monitoring.png)  


## <a name="latest-output"></a>Letzte Ausgabe
Ein weiterer interessanter Datenpunkt für die Überwachung Ihres Auftrags ist der Zeitpunkt der letzten Ausgabe, der auf der Übersichtsseite angezeigt wird.
Bei dieser Zeitangabe handelt es sich um den Anwendungszeitpunkt (d.h. den Zeitpunkt mit dem Zeitstempel aus den Ereignisdaten) der letzten Ausgabe Ihres Auftrags.

## <a name="get-help"></a>Hier erhalten Sie Hilfe
Weitere Unterstützung finden Sie auf der [Frageseite von Microsoft Q&A (Fragen und Antworten) zu Azure Stream Analytics](/answers/topics/azure-stream-analytics.html).

## <a name="next-steps"></a>Nächste Schritte
* [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
* [Erste Schritte mit Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Skalieren von Azure Stream Analytics-Aufträgen](stream-analytics-scale-jobs.md)
* [Stream Analytics Query Language Reference (in englischer Sprache)](/stream-analytics-query/stream-analytics-query-language-reference)
* [Referenz zur Azure Stream Analytics-Verwaltungs-REST-API](/rest/api/streamanalytics/)
