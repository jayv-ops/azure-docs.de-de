---
title: Verwenden der Azure Digital Twins-Befehlszeilenschnittstelle
titleSuffix: Azure Digital Twins
description: Informieren Sie sich über die ersten Schritte mit der Azure Digital Twins-Befehlszeilenschnittstelle und deren Verwendung.
author: baanders
ms.author: baanders
ms.date: 05/25/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: a24b8b18dd109f1d8ed5acaa7de55ce5a3cc1eb9
ms.sourcegitcommit: dda0d51d3d0e34d07faf231033d744ca4f2bbf4a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2021
ms.locfileid: "102201106"
---
# <a name="use-the-azure-digital-twins-cli"></a>Verwenden der Azure Digital Twins-Befehlszeilenschnittstelle

Zusätzlich zur Verwaltung Ihrer Azure Digital Twins-Instanz im Azure-Portal verfügt Azure Digital Twins über einen **Befehlssatz für die [Azure CLI](/cli/azure/what-is-azure-cli)** , über den Sie die meisten wichtigen Aktionen mit dem Dienst ausführen können, einschließlich:
* Verwalten einer Azure Digital Twins-Instanz
* Verwalten von Modellen
* Verwalten von digitalen Zwillingen
* Verwalten von Zwillingsbeziehungen
* Konfigurieren von Endpunkten
* Verwalten von [Routen](concepts-route-events.md)
* Konfigurieren der [Sicherheit](concepts-security.md) über die rollenbasierte Zugriffssteuerung in Azure (Role-Based Access Control, Azure RBAC)

Der Befehlssatz besitzt die Bezeichnung **az dt** und ist Teil der [Azure IoT-Erweiterung für die Azure CLI](https://github.com/Azure/azure-iot-cli-extension). Die vollständige Liste der Befehle und deren Verwendung finden Sie in der Referenzdokumentation für den `az iot`-Befehlssatz: [*az dt*-Befehlsreferenz](/cli/azure/ext/azure-iot/dt).

## <a name="uses-deploy-and-validate"></a>Verwendungen (bereitstellen und validieren)

Zusätzlich zur allgemeinen Verwaltung Ihrer Instanz ist die CLI auch ein nützliches Tool für die Bereitstellung und Validierung.
* Mithilfe der Befehle auf Steuerungsebene kann die Bereitstellung einer neuen Instanz wiederholbar oder automatisiert werden.
* Mit den Befehlen auf Datenebene können Sie schnell Werte in Ihrer Instanz und den erwartungsgemäßen Abschluss von Vorgängen überprüfen.

## <a name="get-the-command-set"></a>Abrufen des Befehlssatzes

Die Azure Digital Twins-Befehle sind Teil der [Azure IoT-Erweiterung für die Azure CLI (azure-iot)](https://github.com/Azure/azure-iot-cli-extension), sodass Sie mit den **az dt**-Befehlen über die neueste `azure-iot`-Erweiterung verfügen.

### <a name="cli-version-requirements"></a>CLI-Versionsanforderungen

Wenn Sie die Azure CLI mit PowerShell verwenden, erfordert das Erweiterungspaket, dass Ihre Azure CLI-Version **2.3.1** oder höher ist.

Mit diesem CLI-Befehl können Sie die Version Ihrer Azure CLI überprüfen:
```azurecli
az --version
```

Anweisungen zum Installieren oder Aktualisieren der Azure CLI auf eine neuere Version finden Sie unter [*Installieren der Azure CLI*](/cli/azure/install-azure-cli).

### <a name="get-the-extension"></a>Erhalten der Erweiterung

Mit diesen Schritten können Sie sicherstellen, dass Sie über die neueste Version der `azure-iot`-Erweiterung verfügen. Sie können diese Befehle in [Azure Cloud Shell](../cloud-shell/overview.md) oder einer [lokalen Azure-Befehlszeilenschnittstelle](/cli/azure/install-azure-cli) ausführen.

[!INCLUDE [digital-twins-cloud-shell-extensions.md](../../includes/digital-twins-cloud-shell-extensions.md)]

## <a name="next-steps"></a>Nächste Schritte

Machen Sie sich anhand der Referenzdokumentation mit der Befehlszeilenschnittstelle und ihrem vollständigen Befehlssatz vertraut:
* [*az dt*-Befehlsreferenz](/cli/azure/ext/azure-iot/dt)