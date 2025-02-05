---
title: Erstellen von erweiterten Codierungsworkflows mit Workflow-Designer | Microsoft Docs
description: Erfahren Sie, wie Sie mit dem Workflow-Designer erweiterte Codierungsworkflows erstellen.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.assetid: 004815f2-0761-4706-87a1-675ba36e0322
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/10/2021
ms.author: anilmur
ms.reviewer: juliako;johndeu
ms.openlocfilehash: 8173da37792948e267aae2078fee9f864bf7bdc9
ms.sourcegitcommit: 225e4b45844e845bc41d5c043587a61e6b6ce5ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/11/2021
ms.locfileid: "103011152"
---
# <a name="create-advanced-encoding-workflows-with-workflow-designer"></a>Erstellen von erweiterten Codierungsworkflows mit Workflow-Designer

[!INCLUDE [media services api v2 logo](./includes/v2-hr.md)]

## <a name="overview"></a>Übersicht
Der **Workflow-Designer** ist ein Windows-Desktoptool, das zum Entwerfen und Erstellen benutzerdefinierter Workflows für die Codierung mit dem **Medienencoder-Premium-Workflow** verwendet wird.
Dank der Leistungsfähigkeit des Workflow-Designer-Tools können Sie komplexe Workflows entwerfen und erstellen, die in **Medienencoder-Premium** ausgeführt werden.  

Workflows können Entscheidungsfindungslogik von Kunden und Verzweigungen enthalten, die auf den Eigenschaften der Eingabequelldatei basieren. Sie können Workflows mit überschreibbaren Eigenschaften und dynamischen Werten erstellen, um auch für sehr komplexe Codieraufgaben zu erreichen, dass sie leicht wiederholt und in der Cloud angepasst werden können.

Beispiele für Workflows, die Sie erstellen können:

* Entscheidungsbasierte Workflows, bei denen der Quellinhalt für die Auflösung untersucht wird und nur die gewünschten Ausgabespuren codiert werden.  Dies ist hilfreich, da die überflüssigen Spuren beseitigt werden, die generiert werden, wenn der Quellinhalt versehentlich horizontal hochskaliert wird.
* Es können mehrere Eingabedateien verwendet werden, um Untertitel, Überlagerungen und die Verknüpfung von Inhalten zu unterstützen. 

Dieses Tool kann auch verwendet werden, um unsere [veröffentlichten Workflows](media-services-workflow-designer.md#existing_workflows)zu ändern. 

> [!NOTE]
> Wenden Sie sich an mepd@microsoft.com, um eine Kopie des Workflow-Designer-Tools zu erhalten.

Sobald eine Workflowdatei erstellt wurde, kann sie als Medienobjekt hochgeladen werden und anschließend zum Codieren von Mediendateien verwendet werden. Informationen zum Codieren mit dem **Medienencoder-Premium-Workflow** unter Verwendung von **.NET** finden Sie im Thema [Erweiterte Codierung mit dem Medienencoder-Premium-Workflow](media-services-encode-with-premium-workflow.md).

## <a name="modify-existing-workflows"></a><a id="existing_workflows"></a>Ändern vorhandener Workflows
Die standardmäßigen [veröffentlichten Workflows](media-services-workflow-designer.md#existing_workflows) können mit dem Designer-Tool geändert werden. Sie können die standardmäßigen Workflowdateien [hier](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows)abrufen. Der Ordner enthält auch Beschreibungen dieser Dateien.

In den folgenden Videos wird die Verwendung des Designers veranschaulicht.

### <a name="day-1--getting-started"></a>Tag 1 – erste Schritte
Im Video von Tag 1 werden folgende Themen behandelt:

* Übersicht über Designer
* Grundlegende Workflows – „Hello World“
* Erstellen mehrerer MP4-Ausgabedateien für die Verwendung mit Azure Media Services-Streaming

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-Premium-Encoder-Workflow-Designer-Training-Videos-Day-1/player]
> 
> 

### <a name="day-2"></a>Tag 2
Im Video von Tag 2 werden folgende Themen behandelt:

* Unterschiedliche Quelldateiszenarien – Verarbeiten von Audiodaten
* Workflows mit erweiterter Logik
* Graph-Phasen

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-Premium-Encoder-Workflow-Designer-Training-Videos-Day-2/player]
> 
> 

### <a name="day-3"></a>Tag 3
Im Video von Tag 3 werden folgende Themen behandelt:

* Skripterstellung innerhalb von Workflows/Blaupausen
* Einschränkungen des aktuellen Encoders
* Fragen und Antworten

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-Premium-Encoder-Workflow-Designer-Training-Videos-Day-3/player]
> 
> 

## <a name="need-help"></a>Sie brauchen Hilfe?

Sie können ein Supportticket erstellen, indem Sie zu [Neue Supportanfrage ](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest) navigieren.

## <a name="next-step"></a>Nächster Schritt
Überprüfen Sie die Media Services-Lernpfade.

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geben
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Weitere Informationen
[Azure Premium Encoder Workflow-Designer – Schulungsvideos](http://johndeutscher.com/2015/07/06/azure-premium-encoder-workflow-designer-training-videos/)

