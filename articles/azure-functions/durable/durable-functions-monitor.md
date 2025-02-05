---
title: Monitore in Durable Functions – Azure
description: In diesem Artikel wird erläutert, wie Sie mithilfe der Durable Functions-Erweiterung für Azure Functions einen Statusmonitor implementieren.
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: azfuncdf
ms.openlocfilehash: 8ef32ecfb6f69b71d29578d3b8314f568fd9386a
ms.sourcegitcommit: ba676927b1a8acd7c30708144e201f63ce89021d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2021
ms.locfileid: "102431073"
---
# <a name="monitor-scenario-in-durable-functions---weather-watcher-sample"></a>Überwachungsszenario in Durable Functions – Beispiel einer Wetterbeobachtungsstation

Das Monitormuster bezieht sich auf einen flexiblen *wiederkehrenden* Prozess in einem Workflow – wenn beispielsweise bestimmte Elemente solange abgerufen werden, bis bestimmte Bedingungen erfüllt sind. In diesem Artikel wird ein Beispiel beschrieben, in dem [Durable Functions](durable-functions-overview.md) zum Implementieren der Überwachung verwendet wird.

## <a name="prerequisites"></a>Voraussetzungen

# <a name="c"></a>[C#](#tab/csharp)

* [Schließen Sie den Schnellstartartikel ab.](durable-functions-create-first-csharp.md)
* [Klonen Sie das Beispielprojekt auf GitHub, oder laden Sie es herunter.](https://github.com/Azure/azure-functions-durable-extension/tree/main/samples/precompiled)

# <a name="javascript"></a>[JavaScript](#tab/javascript)

* [Schließen Sie den Schnellstartartikel ab.](quickstart-js-vscode.md)
* [Klonen Sie das Beispielprojekt auf GitHub, oder laden Sie es herunter.](https://github.com/Azure/azure-functions-durable-extension/tree/main/samples/javascript)

---

## <a name="scenario-overview"></a>Übersicht über das Szenario

Dieses Beispiel überwacht die aktuellen Wetterbedingungen an einem bestimmten Ort und benachrichtigt einen Benutzer per SMS, wenn das Wetter schön ist. Sie könnten eine reguläre, per Timer ausgelöste Funktion verwenden, um das Wetter zu beobachten und Benachrichtigungen zu senden. Ein Problem bei diesem Ansatz ist jedoch die **Lebensdauerverwaltung**. Wenn nur eine einzige Benachrichtigung gesendet werden soll, muss der Monitor sich selbst deaktivieren, nachdem schönes Wetter erkannt wurde. Das Überwachungsmuster bietet neben weiteren Vorteilen den Vorzug, dass es die eigene Ausführung beenden kann:

* Monitore werden in Intervallen, nicht nach Zeitplänen ausgeführt: Ein Timertrigger wird jede Stunde *ausgeführt*; ein Monitor *wartet* zwischen Aktionen eine Stunde. Die Aktionen eines Monitors überschneiden sich nicht, sofern dies nicht explizit angegeben wird. Dies kann bei Tasks mit langer Ausführungsdauer wichtig sein.
* Monitore können in dynamischen Intervallen ausgeführt werden: Die Wartezeit kann sich aufgrund einer Bedingung ändern.
* Monitore können beendet werden, wenn eine bestimmte Bedingung erfüllt ist. Sie können auch von einem anderen Prozess beendet werden.
* Für Monitore können Parameter angegeben werden. Das Beispiel zeigt, wie ein und derselbe Wetterbeobachtungsprozess auf jeden gewünschten Standort und jede gewünschte Telefonnummer angewendet werden kann.
* Monitore sind skalierbar. Da jeder Monitor eine Orchestrierungsinstanz ist, können mehrere Monitore erstellt werden, ohne neue Funktionen erstellen oder weiteren Code definieren zu müssen.
* Monitore lassen sich problemlos in größere Workflows integrieren. Ein Monitor kann Teil einer komplexeren Orchestrierungsfunktion oder eine [untergeordnete Orchestrierung](durable-functions-sub-orchestrations.md) sein.

## <a name="configuration"></a>Konfiguration

### <a name="configuring-twilio-integration"></a>Konfigurieren der Twilio-Integration

[!INCLUDE [functions-twilio-integration](../../../includes/functions-twilio-integration.md)]

### <a name="configuring-weather-underground-integration"></a>Konfigurieren der Integration von Weather Underground

In diesem Beispiel wird die Weather Underground-API verwenden, um die Wetterbedingungen an einem Standort zu beobachten.

Als Erstes benötigen Sie ein Weather Underground-Konto. Unter [https://www.wunderground.com/signup](https://www.wunderground.com/signup) können Sie kostenlos eines erstellen. Sobald Sie über das Konto verfügen, müssen Sie einen API-Schlüssel abrufen. Öffnen Sie dazu die Website [https://www.wunderground.com/weather/api](https://www.wunderground.com/weather/api/?MR=1), und wählen Sie „Key Settings“ (Schlüsseleinstellungen) aus. Der Stratus Developer-Plan ist kostenlos und für dieses Beispiel ausreichend.

Wenn Sie den API-Schlüssel haben, fügen Sie die folgenden **App-Einstellungen** zu Ihrer Funktions-App hinzu.

| Name der App-Einstellung | Wertbeschreibung |
| - | - |
| **WeatherUndergroundApiKey**  | Ihr Weather Underground-API-Schlüssel. |

## <a name="the-functions"></a>Die Funktionen

In diesem Artikel werden die folgenden Funktionen in der Beispiel-App beschrieben:

* `E3_Monitor`: Eine [Orchestratorfunktion](durable-functions-bindings.md#orchestration-trigger), die `E3_GetIsClear` in regelmäßigen Abständen aufruft. Sie ruft `E3_SendGoodWeatherAlert` auf, wenn `E3_GetIsClear` „true“ zurückgibt.
* `E3_GetIsClear`: Eine [Aktivitätsfunktion](durable-functions-bindings.md#activity-trigger), die die aktuellen Wetterbedingungen an einem Ort beobachtet.
* `E3_SendGoodWeatherAlert`: Eine Aktivitätsfunktion, die eine SMS-Nachricht über Twilio sendet.

### <a name="e3_monitor-orchestrator-function"></a>Orchestratorfunktion „E3_Monitor“

# <a name="c"></a>[C#](#tab/csharp)

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/Monitor.cs?range=41-78,97-115)]

Der Orchestrator benötigt einen zu überwachenden Standort sowie eine Telefonnummer, an die eine Nachricht gesendet wird, wenn das Wetter an diesem Ort aufklart. Diese Daten werden als stark typisiertes `MonitorRequest`-Objekt an den Orchestrator übergeben.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Die Funktion **E3_Monitor** verwendet die Standarddatei *function.json* für Orchestratorfunktionen.

[!code-json[Main](~/samples-durable-functions/samples/javascript/E3_Monitor/function.json)]

Im Folgenden wird der Code dargestellt, der die Funktion implementiert:

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/E3_Monitor/index.js)]

---

Diese Orchestratorfunktion führt die folgenden Aktionen aus:

1. Sie ruft die **MonitorRequest**-Anforderung ab, die aus dem zu beobachtenden *Standort* und der *Telefonnummer* besteht, an die eine SMS-Benachrichtigung gesendet werden soll.
2. Sie bestimmt den Ablaufzeitpunkt des Monitors. Aus Gründen der Übersichtlichkeit verwendet dieses Beispiel einen hartcodierten Wert.
3. Sie ruft **E3_GetIsClear** auf, um zu ermitteln, ob das Wetter am angeforderten Standort schön ist.
4. Wenn dies der Fall ist, ruft die Funktion **E3_SendGoodWeatherAlert** auf, um eine SMS-Benachrichtigung an die angeforderte Telefonnummer zu senden.
5. Sie erstellt einen permanenten Timer, um die Orchestrierung im nächsten Abrufintervall fortzusetzen. Aus Gründen der Übersichtlichkeit verwendet dieses Beispiel einen hartcodierten Wert.
6. Sie wird ausgeführt, bis ^die aktuelle UTC-Zeit den Ablaufzeitpunkt des Monitors überschreitet oder eine SMS-Benachrichtigung gesendet wird.

Mehrere Orchestratorinstanzen können gleichzeitig ausgeführt werden, indem die Orchestratorfunktion mehrmals aufgerufen wird. Der zu überwachende Standort und die Telefonnummer, an die eine SMS-Benachrichtigung gesendet werden soll, können angegeben werden. Beachten Sie schließlich, dass die Orchestratorfunktion während des Wartens auf den Timer *nicht* ausgeführt wird, daher werden Ihnen keine Gebühren dafür berechnet.
### <a name="e3_getisclear-activity-function"></a>Aktivitätsfunktion „E3_GetIsClear“

Bei den Hilfsaktivitätsfunktionen handelt es sich, wie bei anderen Beispielen, um reguläre Funktionen, die die Triggerbindung `activityTrigger` verwenden. Die Funktion **E3_GetIsClear** ruft mithilfe der Weather Underground-API die aktuellen Wetterbedingungen ab und ermittelt, ob das Wetter schön ist.

# <a name="c"></a>[C#](#tab/csharp)

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/Monitor.cs?range=80-85)]

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Die Datei *function.json* wird wie folgt definiert:

[!code-json[Main](~/samples-durable-functions/samples/javascript/E3_GetIsClear/function.json)]

Hier ist die Implementierung.

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/E3_GetIsClear/index.js)]

---

### <a name="e3_sendgoodweatheralert-activity-function"></a>Aktivitätsfunktion „E3_SendGoodWeatherAlert“

Die Funktion **E3_SendGoodWeatherAlert** verwendet die Twilio-Bindung, um eine SMS zu senden, die den Endbenutzer darüber informiert, dass jetzt ein guter Zeitpunkt für einen Spaziergang ist.

# <a name="c"></a>[C#](#tab/csharp)

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/Monitor.cs?range=87-96,140-205)]

> [!NOTE]
> Sie müssen das NuGet-Paket `Microsoft.Azure.WebJobs.Extensions.Twilio` installieren, um den Beispielcode auszuführen.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

Die *function.json* ist einfach:

[!code-json[Main](~/samples-durable-functions/samples/javascript/E3_SendGoodWeatherAlert/function.json)]

Hier sehen Sie den Code, der die SMS-Nachricht sendet:

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/E3_SendGoodWeatherAlert/index.js)]

---

## <a name="run-the-sample"></a>Ausführen des Beispiels

Wenn Sie die über HTTP ausgelösten Funktionen verwenden, die im Beispiel enthalten sind, können Sie mit der Orchestrierung beginnen, indem Sie folgende HTTP POST-Anforderung senden:

```
POST https://{host}/orchestrators/E3_Monitor
Content-Length: 77
Content-Type: application/json

{ "location": { "city": "Redmond", "state": "WA" }, "phone": "+1425XXXXXXX" }
```

```
HTTP/1.1 202 Accepted
Content-Type: application/json; charset=utf-8
Location: https://{host}/runtime/webhooks/durabletask/instances/f6893f25acf64df2ab53a35c09d52635?taskHub=SampleHubVS&connection=Storage&code={SystemKey}
RetryAfter: 10

{"id": "f6893f25acf64df2ab53a35c09d52635", "statusQueryGetUri": "https://{host}/runtime/webhooks/durabletask/instances/f6893f25acf64df2ab53a35c09d52635?taskHub=SampleHubVS&connection=Storage&code={systemKey}", "sendEventPostUri": "https://{host}/runtime/webhooks/durabletask/instances/f6893f25acf64df2ab53a35c09d52635/raiseEvent/{eventName}?taskHub=SampleHubVS&connection=Storage&code={systemKey}", "terminatePostUri": "https://{host}/runtime/webhooks/durabletask/instances/f6893f25acf64df2ab53a35c09d52635/terminate?reason={text}&taskHub=SampleHubVS&connection=Storage&code={systemKey}"}
```

Die Instanz **E3_Monitor** wird gestartet und ruft die aktuellen Wetterbedingungen für den angeforderten Standort ab. Wenn das Wetter schön ist, ruft die Instanz eine Aktivitätsfunktion auf, die eine Benachrichtigung sendet. Andernfalls legt sie einen Timer fest. Wenn der Timer abläuft, wird die Orchestrierung fortgesetzt.

Sie können sich die Aktivität der Orchestrierung in den Funktionsprotokollen im Azure Functions-Portal ansehen.

```
2018-03-01T01:14:41.649 Function started (Id=2d5fcadf-275b-4226-a174-f9f943c90cd1)
2018-03-01T01:14:42.741 Started orchestration with ID = '1608200bb2ce4b7face5fc3b8e674f2e'.
2018-03-01T01:14:42.780 Function completed (Success, Id=2d5fcadf-275b-4226-a174-f9f943c90cd1, Duration=1111ms)
2018-03-01T01:14:52.765 Function started (Id=b1b7eb4a-96d3-4f11-a0ff-893e08dd4cfb)
2018-03-01T01:14:52.890 Received monitor request. Location: Redmond, WA. Phone: +1425XXXXXXX.
2018-03-01T01:14:52.895 Instantiating monitor for Redmond, WA. Expires: 3/1/2018 7:14:52 AM.
2018-03-01T01:14:52.909 Checking current weather conditions for Redmond, WA at 3/1/2018 1:14:52 AM.
2018-03-01T01:14:52.954 Function completed (Success, Id=b1b7eb4a-96d3-4f11-a0ff-893e08dd4cfb, Duration=189ms)
2018-03-01T01:14:53.226 Function started (Id=80a4cb26-c4be-46ba-85c8-ea0c6d07d859)
2018-03-01T01:14:53.808 Function completed (Success, Id=80a4cb26-c4be-46ba-85c8-ea0c6d07d859, Duration=582ms)
2018-03-01T01:14:53.967 Function started (Id=561d0c78-ee6e-46cb-b6db-39ef639c9a2c)
2018-03-01T01:14:53.996 Next check for Redmond, WA at 3/1/2018 1:44:53 AM.
2018-03-01T01:14:54.030 Function completed (Success, Id=561d0c78-ee6e-46cb-b6db-39ef639c9a2c, Duration=62ms)
```

Die Orchestrierung wird abgeschlossen, sobald das Zeitlimit erreicht ist oder schönes Wetter erkannt wird. Sie können auch die `terminate`-API in einer anderen Funktion verwenden oder den HTTP POST-Webhook **terminatePostUri** aufrufen, auf den oben in der 202-Antwort verwiesen wird. Ersetzen Sie `{text}` durch den Grund für die vorzeitige Beendigung, um den Webhook zu verwenden. Die HTTP POST-URL sieht in etwa wie folgt aus:

```
POST https://{host}/runtime/webhooks/durabletask/instances/f6893f25acf64df2ab53a35c09d52635/terminate?reason=Because&taskHub=SampleHubVS&connection=Storage&code={systemKey}
```

## <a name="next-steps"></a>Nächste Schritte

Dieses Beispiel veranschaulichte, wie Sie Durable Functions verwenden, um den Status einer externen Quelle mithilfe von [permanenten Timern](durable-functions-timers.md) und Bedingungslogik zu überwachen. Das nächste Beispiel zeigt, wie externe Ereignisse und [permanente Timer](durable-functions-timers.md) verwendet werden, um Benutzerinteraktionen zu verarbeiten.

> [!div class="nextstepaction"]
> [Ausführen des Beispiels für Benutzerinteraktion](durable-functions-phone-verification.md)