---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 02/19/2020
ms.author: glenga
ms.openlocfilehash: 753a3d16792e0dbaffe318cd188ea3f0d40bd5cb
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "78191031"
---
Fügen Sie Code hinzu, der das Cmdlet `Push-OutputBinding` verwendet, um Text unter Verwendung der Ausgabebindung `msg` in die Warteschlange zu schreiben. Fügen Sie diesen Code hinzu, bevor Sie den OK-Status in der `if`-Anweisung festlegen.

:::code language="powershell" range="18-19" source="~/functions-docs-powershell/functions-add-output-binding-storage-queue-cli/HttpExample/run.ps1":::

Die Funktion sollte nun wie folgt aussehen:

:::code language="powershell" source="~/functions-docs-powershell/functions-add-output-binding-storage-queue-cli/HttpExample/run.ps1":::
