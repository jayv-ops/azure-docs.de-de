---
title: Azure CLI-Beispielskript – Rotieren von Speicherkonto-Zugriffsschlüsseln | Microsoft-Dokumentation
description: Erstellen Sie ein Azure Storage-Konto, und rufen Sie dann die Zugriffsschlüssel für das Konto ab, und rotieren Sie sie.
services: storage
author: tamram
ms.service: storage
ms.subservice: blobs
ms.devlang: cli
ms.topic: sample
ms.date: 10/20/2020
ms.author: tamram
ms.custom: devx-track-azurecli
ms.openlocfilehash: 08e1b3837863b197f8463a0d969e78afab2b9858
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "92370405"
---
# <a name="create-a-storage-account-and-rotate-its-account-access-keys"></a>Erstellen eines Speicherkontos und Rotieren der Zugriffsschlüssel für das Konto

Mit diesem Skript wird ein Azure Storage-Konto erstellt, die Zugriffsschlüssel des neuen Speicherkontos werden angezeigt, und dann werden die Schlüssel erneuert (rotiert).

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Beispielskript

[!code-azurecli-interactive[main](../../../cli_scripts/storage/rotate-storage-account-keys/rotate-storage-account-keys.sh "Rotate storage account keys")]

## <a name="clean-up-deployment"></a>Bereinigen der Bereitstellung

Führen Sie den folgenden Befehl aus, um die Ressourcengruppe, das Speicherkonto und alle zugehörigen Ressourcen zu entfernen.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Erläuterung des Skripts

In diesem Skript werden die folgenden Befehle verwendet, um das Speicherkonto zu erstellen und die Zugriffsschlüssel abzurufen und zu rotieren. Jedes Element in der Tabelle ist mit der Dokumentation des jeweiligen Befehls verknüpft.

| Get-Help | Notizen |
|---|---|
| [az group create](/cli/azure/group) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az storage account create](/cli/azure/storage/account) | Erstellt ein Azure Storage-Konto in der angegebenen Ressourcengruppe. |
| [az storage account keys list](/cli/azure/storage/account/keys) | Zeigt die Zugriffsschlüssel für das angegebene Konto an. |
| [az storage account keys renew](/cli/azure/storage/account/keys) | Generiert den primären oder sekundären Zugriffsschlüssel für das Speicherkonto neu. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](/cli/azure).

Weitere CLI-Skriptbeispiele für Speicher finden Sie in den [Azure CLI-Beispielen für Azure Blob Storage](../blobs/storage-samples-blobs-cli.md).
