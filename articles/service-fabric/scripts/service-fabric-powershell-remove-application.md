---
title: Entfernen einer Anwendung aus einem Cluster in PowerShell
description: Azure PowerShell-Skriptbeispiel – Entfernen einer Anwendung aus einem Service Fabric-Cluster.
services: service-fabric
documentationcenter: ''
author: athinanthny
manager: chackdan
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: service-fabric
ms.workload: multiple
ms.topic: sample
ms.date: 01/18/2018
ms.author: atsenthi
ms.custom: mvc
ms.openlocfilehash: 686afa791df88382e3e5e1b2d233317c36bf1dd6
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "98791305"
---
# <a name="remove-an-application-from-a-service-fabric-cluster-using-powershell"></a>Entfernen einer Anwendung aus einem Service Fabric-Cluster mithilfe von PowerShell

Dieses Beispielskript löscht eine ausgeführte Service Fabric-Anwendungsinstanz und hebt die Registrierung für einen Anwendungstyp und eine Version im Cluster auf.  Durch das Löschen der Anwendungsinstanz werden auch alle ausgeführten Dienstinstanzen gelöscht, die dieser Anwendung zugeordnet sind. Passen Sie die Parameter nach Bedarf an. 

Wenn Sie das Service Fabric-PowerShell-Modul benötigen, installieren Sie es zusammen mit dem [Service Fabric SDK](../service-fabric-get-started.md). 

## <a name="sample-script"></a>Beispielskript

[!code-powershell[main](../../../powershell_scripts/service-fabric/remove-application/remove-application.ps1 "Remove an application from a cluster")]

## <a name="script-explanation"></a>Erläuterung des Skripts

Das Skript verwendet die folgenden Befehle. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Get-Help | Notizen |
|---|---|
| [Remove-ServiceFabricApplication](/powershell/module/servicefabric/remove-servicefabricapplication) | Entfernt eine ausgeführte Service Fabric-Anwendungsinstanz aus dem Cluster  |
| [Unregister-ServiceFabricApplicationType](/powershell/module/servicefabric/unregister-servicefabricapplicationtype) | Hebt die Registrierung eines Service Fabric-Anwendungstyps und einer Version aus dem Cluster auf |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Service Fabric-PowerShell-Modul finden Sie in der [Azure PowerShell-Dokumentation](/powershell/azure/service-fabric/overview).

Zusätzliche PowerShell-Beispiele für Azure Service Fabric finden Sie unter [Azure PowerShell-Beispiele](../service-fabric-powershell-samples.md).
