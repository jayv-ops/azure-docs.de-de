---
title: Skalieren der Medienverarbeitung durch Hinzufügen von Codierungseinheiten – Azure | Microsoft-Dokumentation
description: In diesem Artikel wird demonstriert, wie Sie mit Azure Media Services .NET Codierungseinheiten hinzufügen.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.assetid: 33f7625a-966a-4f06-bc09-bccd6e2a42b5
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/10/2021
ms.author: inhenkel
ms.reviewer: milangada
ms.custom: devx-track-csharp
ms.openlocfilehash: 6feb28858271f11bef0933a352f86d7545ee5bd3
ms.sourcegitcommit: 225e4b45844e845bc41d5c043587a61e6b6ce5ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/11/2021
ms.locfileid: "103014076"
---
# <a name="how-to-scale-encoding-with-net-sdk"></a>Skalieren der Codierung mit .NET SDK

[!INCLUDE [media services api v2 logo](./includes/v2-hr.md)]

> [!div class="op_single_selector"]
> * [Portal](media-services-portal-scale-media-processing.md)
> * [.NET](media-services-dotnet-encoding-units.md)
> * [REST](/rest/api/media/operations/encodingreservedunittype)
> * [Java](https://github.com/rnrneverdies/azure-sdk-for-media-services-java-samples)
> * [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
> 
> 

## <a name="overview"></a>Übersicht
> [!IMPORTANT]
> Lesen Sie unbedingt die [Übersicht](media-services-scale-media-processing-overview.md), um weitere Informationen zum Skalieren der Medienverarbeitung zu erhalten.
> 
> 

Führen Sie folgende Schritte aus, um den Typ reservierter Einheiten und die Anzahl reservierter Einheiten für die Codierung mit dem .NET SDK zu ändern:

```csharp
IEncodingReservedUnit encodingS1ReservedUnit = _context.EncodingReservedUnits.FirstOrDefault();
encodingS1ReservedUnit.ReservedUnitType = ReservedUnitType.Basic; // Corresponds to S1
encodingS1ReservedUnit.Update();
Console.WriteLine("Reserved Unit Type: {0}", encodingS1ReservedUnit.ReservedUnitType);

encodingS1ReservedUnit.CurrentReservedUnits = 2;
encodingS1ReservedUnit.Update();

Console.WriteLine("Number of reserved units: {0}", encodingS1ReservedUnit.CurrentReservedUnits);
```

## <a name="opening-a-support-ticket"></a>Öffnen eines Supporttickets

Standardmäßig kann jedes Media Services-Konto auf bis zu 10 reservierte S2- oder S3-Einheiten für Medien (MRUs) oder 25 S1-MRUs und 5 für das bedarfsgesteuerte Streaming reservierte Einheiten skaliert werden. Sie können einen höheren Grenzwert durch Öffnen eines Supporttickets anfordern.

## <a name="media-services-learning-paths"></a>Media Services-Lernpfade
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geben
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]
