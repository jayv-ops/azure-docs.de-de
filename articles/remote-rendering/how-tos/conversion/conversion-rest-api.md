---
title: REST-API für die Medienobjektkonvertierung
description: Beschreibt, wie Sie ein Medienobjekt über die REST-API konvertieren.
author: florianborn71
ms.author: flborn
ms.date: 02/04/2020
ms.topic: how-to
ms.openlocfilehash: f33e5717cd5556e72d996e7e943867c16805e71b
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101705176"
---
# <a name="use-the-model-conversion-rest-api"></a>Verwenden der REST-API für die Modellkonvertierung

Der [Modellkonvertierungsdienst](model-conversion.md) wird über eine [REST-API](https://en.wikipedia.org/wiki/Representational_state_transfer) gesteuert. Diese API kann zum Erstellen von Konvertierungen, zum Abrufen von Konvertierungseigenschaften und zum Auflisten vorhandener Konvertierungen verwendet werden.

## <a name="rest-api-reference"></a>REST-API-Referenz

Informationen zur REST-API für Remote Rendering finden Sie [hier](/rest/api/mixedreality/2021-01-01preview/remoterendering). Informationen zu den Swagger-Definitionen finden Sie [hier](https://github.com/Azure/azure-rest-api-specs/tree/master/specification/mixedreality/data-plane/Microsoft.MixedReality).

Wir stellen ein PowerShell-Skript im [ARR-Beispielrepository](https://github.com/Azure/azure-remote-rendering) im Ordner *Scripts* bereit, das den Namen *Conversion.ps1* trägt. Dieses Skript veranschaulicht die Verwendung unseres Diensts. Das Skript und seine Konfiguration werden hier beschrieben: [PowerShell-Beispielskripts](../../samples/powershell-example-scripts.md) Es stehen auch SDKs für [.NET](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/mixedreality/Azure.MixedReality.RemoteRendering), Java und Python bereit.

## <a name="next-steps"></a>Nächste Schritte

- [Verwenden von Azure Blob Storage für die Modellkonvertierung](blob-storage.md)
- [Modellkonvertierung](model-conversion.md)