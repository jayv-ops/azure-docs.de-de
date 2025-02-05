---
title: Mithilfe einer ARM-Vorlage können Sie fortlaufende Sicherungen und Zeitpunktwiederherstellungen in Azure Cosmos DB konfigurieren.
description: Hier erfahren Sie, wie Sie ein Konto mit fortlaufenden Sicherungs- und Wiederherstellungsdaten mithilfe von Azure Resource Manager-Vorlagen bereitstellen können.
author: kanshiG
ms.service: cosmos-db
ms.topic: how-to
ms.date: 02/01/2021
ms.author: govindk
ms.reviewer: sngun
ms.openlocfilehash: 4abfdd0209bd9f13fb7bd902b27a53f65156da2e
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/14/2021
ms.locfileid: "100381816"
---
# <a name="configure-and-manage-continuous-backup-and-point-in-time-restore-preview---using-azure-resource-manager-templates"></a>Konfigurieren und Verwalten von fortlaufender Sicherung und Zeitpunktwiederherstellung (Vorschau) – Verwenden von Azure Resource Manager-Vorlagen
[!INCLUDE[appliesto-sql-mongodb-api](includes/appliesto-sql-mongodb-api.md)]

> [!IMPORTANT]
> Das Feature „Zeitpunktwiederherstellung“ (fortlaufender Sicherungsmodus) für Azure Cosmos DB befindet sich zurzeit in der öffentlichen Vorschau.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Mithilfe des Azure Cosmos DB-Features „Zeitpunktwiederherstellung“ (Vorschau) können Sie nach einer versehentlichen Änderung in einem Container ein gelöschtes Konto, eine Datenbank oder einen Container wiederherstellen oder aber eine Wiederherstellung in einer beliebigen Region (in der es Sicherungen gab) durchführen. Der fortlaufende Sicherungsmodus ermöglicht Ihnen die Wiederherstellung auf jeden beliebigen Zeitpunkt innerhalb der letzten 30 Tage.

In diesem Artikel wird beschrieben, wie Sie ein Konto mit fortlaufenden Sicherungs- und Wiederherstellungsdaten mithilfe von Azure Resource Manager-Vorlagen bereitstellen können.

## <a name="provision-an-account-with-continuous-backup"></a><a id="provision"></a>Bereitstellen eines Kontos mit fortlaufender Sicherung

Sie können mithilfe von Azure Resource Manager-Vorlagen ein Azure Cosmos DB-Konto im kontinuierlichen Modus bereitstellen. Wenn Sie die Vorlage für die Bereitstellung eines Kontos definieren, beziehen Sie den `backupPolicy`-Parameter mit ein, wie im folgenden Beispiel gezeigt:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "resources": [
    {
      "name": "ademo-pitr1",
      "type": "Microsoft.DocumentDB/databaseAccounts",
      "apiVersion": "2016-03-31",
      "location": "West US",
      "properties": {
        "locations": [
          {
            "locationName": "West US"
          }
        ],
        "backupPolicy": {
          "type": "Continuous"
        },
        "databaseAccountOfferType": "Standard"
      }
    }
  ]
}
```

Stellen Sie als Nächstes die Vorlage mithilfe von Azure PowerShell oder der CLI bereit. Im folgenden Beispiel wird gezeigt, wie die Vorlage mit einem CLI-Befehl bereitgestellt wird:

```azurecli-interactive
az group deployment create -g <ResourceGroup> --template-file <ProvisionTemplateFilePath>
```

## <a name="restore-using-the-resource-manager-template"></a><a id="restore"></a>Wiederherstellen mithilfe der Resource Manager-Vorlage

Sie können ein Konto auch mithilfe der Resource Manager-Vorlage wiederherstellen. Beziehen Sie beim Definieren der Vorlage die folgenden Parameter mit ein:

* Festlegen des `createMode`-Parameters auf *Restore*
* Definieren Sie `restoreParameters`, und beachten Sie, dass der Wert `restoreSource` aus der Ausgabe des Befehls `az cosmosdb restorable-database-account list` für Ihr Quellkonto extrahiert wird. Das Attribut „Instance ID“ (Instanz-ID) für Ihren Kontonamen wird für die Wiederherstellung verwendet.
* Legen Sie den `restoreMode`-Parameter auf *PointInTime* fest, und konfigurieren Sie den `restoreTimestampInUtc`-Wert.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "resources": [
    {
      "name": "vinhpitrarmrestore-kal3",
      "type": "Microsoft.DocumentDB/databaseAccounts",
      "apiVersion": "2016-03-31",
      "location": "West US",
      "properties": {
        "locations": [
          {
            "locationName": "West US"
          }
        ],
        "databaseAccountOfferType": "Standard",
        "createMode": "Restore",
        "restoreParameters": {
            "restoreSource": "/subscriptions/2296c272-5d55-40d9-bc05-4d56dc2d7588/providers/Microsoft.DocumentDB/locations/West US/restorableDatabaseAccounts/6a18ecb8-88c2-4005-8dce-07b44b9741df",
            "restoreMode": "PointInTime",
            "restoreTimestampInUtc": "6/24/2020 4:01:48 AM"
        }
      }
    }
  ]
}
```

Stellen Sie als Nächstes die Vorlage mithilfe von Azure PowerShell oder der CLI bereit. Im folgenden Beispiel wird gezeigt, wie die Vorlage mit einem CLI-Befehl bereitgestellt wird:  

```azurecli-interactive
az group deployment create -g <ResourceGroup> --template-file <RestoreTemplateFilePath> 
```

## <a name="next-steps"></a>Nächste Schritte

* Konfigurieren und verwalten Sie die fortlaufende Sicherung mithilfe von [Azure CLI](continuous-backup-restore-command-line.md), [PowerShell](continuous-backup-restore-command-line.md) oder über das [Azure-Portal](continuous-backup-restore-portal.md).
* [Ressourcenmodell des fortlaufenden Sicherungsmodus](continuous-backup-restore-resource-model.md)
* [Verwalten Sie Berechtigungen](continuous-backup-restore-permissions.md), die zum Wiederherstellen von Daten mit dem fortlaufenden Sicherungsmodus erforderlich sind.