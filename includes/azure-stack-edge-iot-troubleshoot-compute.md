---
author: v-dalc
ms.service: databox
ms.author: alkohli
ms.topic: include
ms.date: 02/05/2021
ms.openlocfilehash: ad981264a99bd48e27f745a789ebe857b7f17d80
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101750551"
---
Verwenden Sie die Runtimeantworten des IoT Edge-Agents, um auf Compute-Instanzen bezogene Fehler zu beheben. Im Folgenden sehen Sie eine Liste der möglichen Antworten:

* 200 – OK
* 400 – Die Bereitstellungskonfiguration ist falsch formatiert oder ungültig.
* 417 – Für das Gerät ist keine Bereitstellungskonfiguration festgelegt.
* 412 – Die Schemaversion der Bereitstellungskonfiguration ist ungültig.
* 406 – das IoT Edge-Gerät ist offline oder sendet keine Statusberichte.
* 500 – in der IoT Edge-Runtime ist ein Fehler aufgetreten.

Weitere Informationen finden Sie unter [IoT Edge-Agent](../articles/iot-edge/iot-edge-runtime.md?preserve-view=true&view=iotedge-2018-06#iot-edge-agent).

Der folgende Fehler steht im Zusammenhang mit dem IoT Edge-Dienst auf Ihrem Azure Stack Edge Pro-Gerät.<!--/ Data Box Gateway--> und erhalten schnell Antworten auf Ihre Fragen.

### <a name="compute-modules-have-unknown-status-and-cant-be-used"></a>Computemodule weisen den Status „Unbekannt“ auf und können nicht verwendet werden

#### <a name="error-description"></a>Fehlerbeschreibung

Alle Module auf dem Gerät weisen den Status „Unbekannt“ auf und können nicht verwendet werden. Der Status „Unbekannt“ liegt auch noch nach einem Neustart vor.<!--Original Support ticket relates to trying to deploy a container app on a Hub. Based on the work item, I assume the error description should not be that specific, and that the error applies to Azure Stack Edge Devices, which is the focus of this troubleshooting.-->

#### <a name="suggested-solution"></a>Vorgeschlagene Lösung

Löschen Sie den IoT Edge-Dienst, und stellen Sie dann das Modul bzw. die Module erneut bereit. Weitere Informationen finden Sie unter [Entfernen des IoT Edge-Diensts](../articles/databox-online/azure-stack-edge-j-series-manage-compute.md#remove-iot-edge-service).