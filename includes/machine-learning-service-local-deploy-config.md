---
author: Blackmist
ms.service: machine-learning
ms.topic: include
ms.date: 03/16/2020
ms.author: larryfr
ms.openlocfilehash: 0eeb82245a53c93af75fc3ce3f37cb588295e5b7
ms.sourcegitcommit: e6de1702d3958a3bea275645eb46e4f2e0f011af
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102508094"
---
Die Einträge im Dokument `deploymentconfig.json` werden den Parametern für [LocalWebservice.deploy_configuration](/python/api/azureml-core/azureml.core.webservice.local.localwebservicedeploymentconfiguration) zugeordnet. In der folgenden Tabelle wird die Zuordnung zwischen den Entitäten im JSON-Dokument und den Parametern für die Methode beschrieben:

| JSON-Entität | Methodenparameter | Beschreibung |
| ----- | ----- | ----- |
| `computeType` | Nicht verfügbar | Das Computeziel. Für lokale Ziele ist der Wert `local` erforderlich. |
| `port` | `port` | Der lokale Port, auf dem der HTTP-Endpunkt des Diensts verfügbar gemacht werden soll. |

Dieser JSON-Code ist ein Beispiel für eine Bereitstellungskonfiguration für die Verwendung mit der CLI:

```json
{
    "computeType": "local",
    "port": 32267
}
```