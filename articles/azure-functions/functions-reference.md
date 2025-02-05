---
title: Leitfaden zur Entwicklung von Azure Functions
description: Lernen Sie die Konzepte und Techniken der Azure Functions kennen, die Sie benötigen, um alle Programmiersprachen und Bindungen übergreifend Funktionen in Azure zu entwickeln.
ms.assetid: d8efe41a-bef8-4167-ba97-f3e016fcd39e
ms.topic: conceptual
ms.date: 10/12/2017
ms.openlocfilehash: fdc898c02cfd20ecfdd72dece4fb1e92d803dbb0
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2021
ms.locfileid: "100386899"
---
# <a name="azure-functions-developer-guide"></a>Azure Functions: Entwicklerhandbuch
In Azure Functions nutzen bestimmte Funktionen einige wichtige technische Konzepte und Komponenten gemeinsam, unabhängig von der verwendeten Sprache oder Bindung. Bevor Sie sich mit den spezifischen Details einer bestimmten Sprache oder Bindung beschäftigen, sollten Sie diese Übersicht lesen, die für alle Funktionen gilt.

Dieser Artikel setzt voraus, dass Sie die [Einführung in Azure Functions](functions-overview.md) bereits gelesen haben.

## <a name="function-code"></a>Funktionscode
Ein *Funktion* ist das primäre Konzept in Azure Functions. Eine Funktion enthält zwei wichtige Komponenten: Ihren Code, der in einer Vielzahl von Programmiersprachen geschrieben sein kann, und die Datei „function.json“ mit Konfigurationsinformationen. Für kompilierte Sprachen wird diese Konfigurationsdatei automatisch aus Anmerkungen im Code generiert. Für Skriptsprachen müssen Sie die Konfigurationsdatei selbst bereitstellen.

Die Datei „function.json“ definiert den Trigger, die Bindungen und weitere Konfigurationseinstellungen der Funktion. Jede Funktion verfügt nur über einen einzigen Trigger. Anhand dieser Konfigurationsdatei ermittelt die Runtime, welche Ereignisse überwacht werden sollen und wie Daten in die Funktionsausführung übergeben und aus dieser zurückgegeben werden. Im Folgenden finden Sie eine function.json-Beispieldatei.

```json
{
    "disabled":false,
    "bindings":[
        // ... bindings here
        {
            "type": "bindingType",
            "direction": "in",
            "name": "myParamName",
            // ... more depending on binding
        }
    ]
}
```

Weitere Informationen finden Sie unter [Konzepte für Azure Functions-Trigger und -Bindungen](functions-triggers-bindings.md).

In der Eigenschaft `bindings` werden sowohl Trigger als auch Bindungen konfiguriert. Einige Einstellungen gelten für alle Bindungen, einige Einstellungen sind spezifisch für einen bestimmten Bindungstyp. Jede Bindung erfordert folgende Einstellungen:

| Eigenschaft    | Werte | Typ | Kommentare|
|---|---|---|---|
| type  | Der Name der Bindung.<br><br>Beispiel: `queueTrigger`. | Zeichenfolge | |
| direction | `in`, `out`  | Zeichenfolge | Gibt an, ob die Bindung zum Empfangen von Daten in der Funktion oder zum Senden von Daten aus der Funktion dient. |
| name | Der Funktionsbezeichner.<br><br>Beispiel: `myQueue`. | Zeichenfolge | Der Name, der für die gebundenen Daten in der Funktion verwendet wird. In C# ist dies ein Argumentname, in JavaScript ist es der Schlüssel in einer Schlüssel-Wert-Liste. |

## <a name="function-app"></a>Funktionen-App
Eine Funktions-App bietet einen Ausführungskontext in Azure, in dem Ihre Funktionen ausgeführt werden. Daher ist dies die Bereitstellungs- und Verwaltungseinheit für Ihre Funktionen. Eine Funktions-App besteht aus einer oder mehreren individuellen Funktionen, die zusammen verwaltet, bereitgestellt und skaliert werden. Der Tarif, die Bereitstellungsmethode und die Runtimeversion sind für alle Funktionen in einer Funktions-App gleich. Eine Funktionen-App ist somit eine Möglichkeit, mit der Sie Ihre Funktionen organisieren und kollektiv verwalten können. Weitere Informationen finden Sie unter [Verwalten einer Funktions-App](functions-how-to-use-azure-function-app-settings.md). 

> [!NOTE]
> Alle Funktionen in einer Funktions-App müssen in der gleichen Programmiersprache erstellt werden. In [Vorgängerversionen](functions-versions.md) der Azure Functions-Runtime war dies nicht erforderlich.

## <a name="folder-structure"></a>Ordnerstruktur
[!INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

Oben sehen Sie die standardmäßige (und empfohlene) Ordnerstruktur für eine Funktions-App. Falls Sie den Dateispeicherort des Codes einer Funktion ändern möchten, ändern Sie den Abschnitt `scriptFile` der Datei _function.json_. Außerdem empfehlen wir, das Projekt mithilfe der [Paketbereitstellung](deployment-zip-push.md) in Ihrer Funktions-App in Azure bereitzustellen. Sie können auch vorhandene Tools wie [Continuous Integration und Continuous Deployment](functions-continuous-deployment.md) und Azure DevOps verwenden.

> [!NOTE]
> Wenn Sie ein Paket manuell bereitstellen, müssen die Datei _host.json_ und die Funktionsordner direkt im Ordner `wwwroot` bereitgestellt werden. Schließen Sie den Ordner `wwwroot` nicht in Ihre Bereitstellungen ein. Andernfalls erhalten Sie `wwwroot\wwwroot`-Ordner.

#### <a name="use-local-tools-and-publishing"></a>Verwenden von lokalen Tools und Veröffentlichung
Funktions-Apps können mit verschiedenen Tools erstellt und veröffentlicht werden, darunter [Visual Studio](./functions-develop-vs.md), [Visual Studio Code](./create-first-function-vs-code-csharp.md), [IntelliJ](./functions-create-maven-intellij.md), [Eclipse](./functions-create-maven-eclipse.md) und [Azure Functions Core Tools](./functions-develop-local.md). Weitere Informationen finden Sie unter [Lokales Codieren und Testen von Azure Functions](./functions-develop-local.md).

<!--NOTE: I've removed documentation on FTP, because it does not sync triggers on the consumption plan --glenga -->

## <a name="how-to-edit-functions-in-the-azure-portal"></a><a id="fileupdate"></a> Bearbeiten von Funktionen im Azure-Portal
Mit dem integrierten Functions-Editor im Azure-Portal können Sie Ihren Code und die Datei *function.json* direkt inline aktualisieren. Dies wird nur für kleine Änderungen oder Proof of Concept-Projekte empfohlen. Die bewährte Methode ist die Verwendung eines lokalen Entwicklungstools wie Visual Studio Code.

## <a name="parallel-execution"></a>Parallele Ausführung
Wenn die Auslösung mehrerer Ereignisse schneller erfolgt als die Runtime einer Singlethreadfunktion sie verarbeiten kann, kann die Runtime die Funktion mehrmals parallel aufrufen.  Wenn eine Funktionen-App den [verbrauchsbasierten Hostingplan](event-driven-scaling.md) verwendet, kann die App automatisch aufskaliert werden.  Jede Instanz der Funktionen-App – unabhängig davon, ob die App im verbrauchsbasierten Hostingplan oder einem regulären [App Service-Hostingplan](../app-service/overview-hosting-plans.md) ausgeführt wird – kann gleichzeitige Funktionsaufrufe über mehrere Threads parallel verarbeiten.  Die maximale Anzahl gleichzeitiger Funktionsaufrufe in jeder Funktionen-App-Instanz variiert je nach Art des verwendeten Triggers sowie je nach den Ressourcen, die von anderen Funktionen innerhalb der Funktionen-App verwendet werden.

## <a name="functions-runtime-versioning"></a>Versionsverwaltung der Functions-Runtime

Sie können die Version der Functions-Runtime mit der App-Einstellung `FUNCTIONS_EXTENSION_VERSION` konfigurieren. Der Wert „~3“ bedeutet beispielsweise, dass für Ihre Funktions-App „3.x“ als Hauptversion verwendet wird. Funktions-Apps werden auf jede neue Nebenversion aktualisiert, wenn sie freigegeben werden. Weitere Informationen finden Sie unter [Einstellen von Runtimeversionen von Azure Functions als Ziel](set-runtime-version.md), einschließlich der Informationen zum Anzeigen der genauen Version Ihrer Funktions-App.

## <a name="repositories"></a>Repositorys
Der Code für Azure Functions ist Open Source und in GitHub-Repositorys gespeichert:

* [Azure-Funktionen](https://github.com/Azure/Azure-Functions)
* [Azure Functions-Host](https://github.com/Azure/azure-functions-host/)
* [Azure Functions-Portal](https://github.com/azure/azure-functions-ux)
* [Azure Functions-Vorlagen](https://github.com/azure/azure-functions-templates)
* [Azure WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/)
* [Azure WebJobs SDK-Erweiterungen](https://github.com/Azure/azure-webjobs-sdk-extensions/)

## <a name="bindings"></a>Bindungen
Hier finden Sie eine Tabelle aller unterstützten Bindungen.

[!INCLUDE [dynamic compute](../../includes/functions-bindings.md)]

Haben Sie Probleme mit Fehlern, die aus den Bindungen stammen? Lesen Sie die Dokumentation [Fehlerbehandlung in Azure Functions](functions-bindings-error-pages.md).


## <a name="connections"></a>Verbindungen

Ihr Funktionsprojekt verweist anhand des Namens auf Verbindungsinformationen aus seinem Konfigurationsanbieter. Die Verbindungsdetails werden nicht direkt akzeptiert, sodass sie in verschiedenen Umgebungen geändert werden können. Eine Triggerdefinition kann z. B. eine `connection`-Eigenschaft enthalten. Dies kann sich auf eine Verbindungszeichenfolge beziehen, aber Sie können die Verbindungszeichenfolge nicht direkt in einer Datei `function.json` festlegen. Stattdessen würden Sie `connection` auf den Namen einer Umgebungsvariablen festlegen, die die Verbindungszeichenfolge enthält.

Der Standardkonfigurationsanbieter verwendet Umgebungsvariablen. Diese werden möglicherweise von [Anwendungseinstellungen](./functions-how-to-use-azure-function-app-settings.md?tabs=portal#settings) festgelegt, wenn Sie im Azure Functions-Dienst ausgeführt werden, oder aus der [lokalen Einstellungsdatei](functions-run-local.md#local-settings-file) bei der lokalen Entwicklung.

### <a name="connection-values"></a>Verbindungswerte

Wenn der Verbindungsname in einen einzelnen exakten Wert aufgelöst wird, identifiziert die Laufzeit den Wert als _Verbindungszeichenfolge_, die in der Regel ein Geheimnis enthält. Die Details einer Verbindungszeichenfolge werden durch den Dienst definiert, mit dem Sie eine Verbindung herstellen möchten.

Ein Verbindungsname kann jedoch auch auf eine Sammlung mehrerer Konfigurationselemente verweisen. Umgebungsvariablen können als Sammlung behandelt werden, indem ein gemeinsames Präfix verwendet wird, das auf doppelte Unterstriche (`__`) endet. Auf die Gruppe kann dann verwiesen werden, indem der Verbindungsname auf dieses Präfix festgelegt wird.

Beispielsweise kann die `connection`-Eigenschaft für eine Azure-Blobtriggerdefinition `Storage1` lauten. Solange kein einzelner Zeichenfolgenwert mit `Storage1` als Name konfiguriert ist, wird `Storage1__serviceUri` für die `serviceUri`-Eigenschaft der Verbindung verwendet. Die Verbindungseigenschaften unterscheiden sich für jeden Dienst. Weitere Informationen finden Sie in der Dokumentation für die Erweiterung, die die Verbindung verwendet.

### <a name="configure-an-identity-based-connection"></a>Konfigurieren einer identitätsbasierten Verbindung

Einige Verbindungen in Azure Functions werden so konfiguriert, dass anstelle eines Geheimnisses eine Identität verwendet wird. Die Unterstützung hängt von der Erweiterung ab, die die Verbindung verwendet. In einigen Fällen ist möglicherweise weiterhin eine Verbindungszeichenfolge in Functions erforderlich, obwohl der Dienst, mit dem Sie eine Verbindung herstellen, identitätsbasierte Verbindungen unterstützt.

> [!IMPORTANT]
> Auch wenn eine Bindungserweiterung identitätsbasierte Verbindungen unterstützt, wird diese Konfiguration möglicherweise noch nicht im Verbrauchsplan unterstützt. Weitere Informationen finden Sie in der Unterstützungstabelle unten.

Identitätsbasierte Verbindungen werden von den folgenden Trigger- und Bindungserweiterungen unterstützt:

| Name der Erweiterung | Erweiterungsversion                                                                                     | Unterstützt identitätsbasierte Verbindungen im Verbrauchsplan |
|----------------|-------------------------------------------------------------------------------------------------------|---------------------------------------|
| Azure Blob     | [Version 5.0.0-beta1 oder höher](./functions-bindings-storage-blob.md#storage-extension-5x-and-higher)  | Nein                                    |
| Azure Queue    | [Version 5.0.0-beta1 oder höher](./functions-bindings-storage-queue.md#storage-extension-5x-and-higher) | Nein                                    |

> [!NOTE]
> Unterstützung für identitätsbasierte Verbindungen ist noch nicht für Speicherverbindungen verfügbar, die von der Functions-Laufzeit für Kernverhalten verwendet werden. Dies bedeutet, dass die `AzureWebJobsStorage`-Einstellung eine Verbindungszeichenfolge sein muss.

#### <a name="connection-properties"></a>Verbindungseigenschaften

Eine identitätsbasierte Verbindung für einen Azure-Dienst akzeptiert die folgenden Eigenschaften:

| Eigenschaft    | Umgebungsvariable | Ist erforderlich | BESCHREIBUNG |
|---|---|---|---|
| Dienst-URI | `<CONNECTION_NAME_PREFIX>__serviceUri` | Ja | Der URI der Datenebene des Diensts, mit dem Sie eine Verbindung herstellen. |

Für einen bestimmten Verbindungstyp können weitere Optionen unterstützt werden. Weitere Informationen finden Sie in der Dokumentation zu der Komponente, die die Verbindung herstellt.

Identitätsbasierte Verbindungen verwenden eine [verwaltete Identität](../app-service/overview-managed-identity.md?toc=%2fazure%2fazure-functions%2ftoc.json), wenn sie im Azure Functions-Dienst gehostet werden. Die vom System zugewiesene Identität wird standardmäßig verwendet. Bei Ausführung in anderen Kontexten (z. B. bei der lokalen Entwicklung) wird stattdessen Ihre Entwickleridentität verwendet, obwohl dies mithilfe alternativer Verbindungsparameter angepasst werden kann.

##### <a name="local-development"></a>Lokale Entwicklung

Bei lokaler Ausführung weist die Konfiguration oben die Laufzeit an, Ihre lokale Entwickleridentität zu verwenden. Die Verbindung versucht, ein Token von den folgenden Speicherorten in der angegebenen Reihenfolge abzurufen:

- Lokaler Cache, der von Microsoft-Anwendungen gemeinsam genutzt wird
- Aktueller Benutzerkontext in Visual Studio
- Aktueller Benutzerkontext in Visual Studio Code
- Aktueller Benutzerkontext in der Azure CLI

Wenn keine dieser Optionen erfolgreich ist, tritt ein Fehler auf.

In einigen Fällen möchten Sie möglicherweise die Verwendung einer anderen Identität angeben. Sie können Konfigurationseigenschaften für die Verbindung hinzufügen, die auf die alternative Identität verweisen.

> [!NOTE]
> Die folgenden Konfigurationsoptionen werden nicht unterstützt, wenn das Hosting im Azure Functions-Dienst erfolgt.

Um eine Verbindung mithilfe eines Azure Active Directory-Dienstprinzipals mit einer Client-ID und einem Geheimnis herzustellen, definieren Sie die Verbindung mit den folgenden Eigenschaften:

| Eigenschaft    | Umgebungsvariable | Ist erforderlich | BESCHREIBUNG |
|---|---|---|---|
| Dienst-URI | `<CONNECTION_NAME_PREFIX>__serviceUri` | Ja | Der URI der Datenebene des Diensts, mit dem Sie eine Verbindung herstellen. |
| Mandanten-ID | `<CONNECTION_NAME_PREFIX>__tenantId` | Ja | Die ID des Azure Active Directory-Mandanten (Verzeichnis). |
| Client-ID | `<CONNECTION_NAME_PREFIX>__clientId` | Ja |  Die Client-ID (Anwendungs-ID) einer App-Registrierung im Mandanten. |
| Geheimer Clientschlüssel | `<CONNECTION_NAME_PREFIX>__clientSecret` | Ja | Ein Clientgeheimnis, das für die App-Registrierung generiert wurde. |

#### <a name="grant-permission-to-the-identity"></a>Erteilen der Berechtigung für die Identität

Unabhängig davon, welche Identität verwendet wird, muss diese über Berechtigungen zum Ausführen der vorgesehenen Aktionen verfügen. Dies erfolgt in der Regel durch Zuweisen einer Rolle in Azure RBAC oder durch Angeben der Identität in einer Zugriffsrichtlinie, abhängig vom Dienst, mit dem Sie eine Verbindung herstellen. Informationen dazu, welche Berechtigungen erforderlich sind und wie diese festgelegt werden können, finden Sie in der Dokumentation zum jeweiligen Dienst.

> [!IMPORTANT]
> Einige Berechtigungen werden möglicherweise vom Dienst verfügbar gemacht, die nicht für alle Kontexte erforderlich sind. Befolgen Sie nach Möglichkeit das **Prinzip der geringsten Berechtigung**, und gewähren Sie der Identität nur die erforderlichen Berechtigungen. Wenn die App z. B. nur aus einem Blob lesen muss, verwenden Sie die Rolle [Speicherblob-Datenleser](../role-based-access-control/built-in-roles.md#storage-blob-data-reader), da die Rolle [Besitzer der Speicherblobdaten](../role-based-access-control/built-in-roles.md#storage-blob-data-owner) über zu umfangreiche Berechtigungen für einen Lesevorgang verfügt.


## <a name="reporting-issues"></a>Melden von Problemen
[!INCLUDE [Reporting Issues](../../includes/functions-reporting-issues.md)]

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie in den folgenden Ressourcen:

* [Trigger und Bindungen in Azure Functions](functions-triggers-bindings.md)
* [Lokales Codieren und Testen von Azure Functions](./functions-develop-local.md)
* [Bewährte Methoden für Azure Functions](functions-best-practices.md)
* [C#-Entwicklerreferenz zu Azure Functions](functions-dotnet-class-library.md)
* [Node.js-Entwicklerreferenz zu Azure Functions](functions-reference-node.md)
