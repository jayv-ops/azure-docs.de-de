---
title: Zuschneiden von Videos mit Media Encoder Standard – Azure | Microsoft-Dokumentation
description: Zuschneiden ist der Prozess der Auswahl eines rechteckigen Fensters innerhalb des Videoframes und die ausschließliche Codierung der Pixel innerhalb dieses Fensters. In diesem Artikel wird gezeigt, wie Videos mit Media Encoder Standard zugeschnitten werden.
services: media-services
documentationcenter: ''
author: IngridAtMicrosoft
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/10/2021
ms.author: anilmur
ms.reviewer: juliako
ms.openlocfilehash: 6a15e57884f85e2d56b77c3e5eec1267133fe96d
ms.sourcegitcommit: 225e4b45844e845bc41d5c043587a61e6b6ce5ae
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/11/2021
ms.locfileid: "103016728"
---
# <a name="crop-videos-with-media-encoder-standard"></a>Zuschneiden von Videos mit Media Encoder Standard

[!INCLUDE [media services api v2 logo](./includes/v2-hr.md)] 

Mit Media Encoder Standard (MES) können Sie Ihr Eingabevideo zuschneiden. Zuschneiden ist der Prozess der Auswahl eines rechteckigen Fensters innerhalb des Videoframes und die ausschließliche Codierung der Pixel innerhalb dieses Fensters. Das folgende Diagramm veranschaulicht diesen Prozess.

![Zuschneiden eines Videos](./media/media-services-crop-video/media-services-crop-video01.png)

Angenommen Sie, Ihr Eingabevideo hat eine Auflösung von 1920 x 1080 Pixel (das Seitenverhältnis 16:9), aber links und rechts schwarze Balken (sog. Pillarboxes), weshalb nur ein Fenster mit dem Seitenverhältnis 4:3 oder 1440 x 1080 Pixeln aktives Video enthält. Mit dem MES können Sie die schwarzen Balken zuschneiden oder herausschneiden und den Bereich mit der Größe 1440 x 1080 codieren.

Das Zuschneiden im MES ist eine Vorverarbeitungsphase, weshalb die Zuschneideparameter in der Codierungsvoreinstellung für das ursprüngliche Eingabevideo gelten. Die Codierung ist eine nachfolgende Phase, weshalb die Einstellungen für Breite und Höhe für das *vorverarbeitete Video* und nicht für das Originalvideo gelten. Beim Bestimmen Ihrer Voreinstellung müssen Sie die folgenden Schritte ausführen: a.) die Zuschneideparameter basierend auf dem ursprünglichen Eingabevideo auswählen und b.) Ihre Codierungseinstellungen basierend auf dem zugeschnittenen Video auswählen. Wenn Ihre Codierungseinstellungen nicht mit dem zugeschnittenen Video übereinstimmen, ist die Ausgabe nicht wie erwartet.

Im [folgenden](media-services-custom-mes-presets-with-dotnet.md#encoding_with_dotnet) Thema wird gezeigt, wie Sie mit MES einen Codierungsauftrag erstellen und für die Codierungsaufgabe eine benutzerdefinierte Voreinstellung angeben. 

## <a name="creating-a-custom-preset"></a>Erstellen einer benutzerdefinierten Voreinstellung
Für das im Diagramm gezeigte Beispiel gilt Folgendes:

1. Die ursprüngliche Eingabe ist 1920 x 1080.
2. Sie muss für eine Ausgabe von 1440 x 1080 zugeschnitten werden, die im Eingabeframe zentriert wird.
3. Dies bedeutet den X-Offset (1920-1440):2 = 240 und den Y-Offset = 0 (null).
4. Breite und Höhe des Zuschnittrechtecks sind 1440 bzw. 1080.
5. Die Aufgabe in der Codierungsphase ist das Erstellen von drei Ebenen mit den Auflösungen 1440 x 1080, 960 x 720 und 480 x 360.

### <a name="json-preset"></a>JSON-Voreinstellung

```json
{
  "Version": 1.0,
  "Sources": [
    {
      "Streams": [],
      "Filters": {
        "Crop": {
          "X": 240,
          "Y": 0,
          "Width": 1440,
          "Height": 1080
        }
      },
      "Pad": true
    }
  ],
  "Codecs": [
    {
      "KeyFrameInterval": "00:00:02",
      "H264Layers": [
        {
          "Profile": "Auto",
          "Level": "auto",
          "Bitrate": 3400,
          "MaxBitrate": 3400,
          "BufferWindow": "00:00:05",
          "Width": 1440,
          "Height": 1080,
          "BFrames": 3,
          "ReferenceFrames": 3,
          "AdaptiveBFrame": true,
          "Type": "H264Layer",
          "FrameRate": "0/1"
        },
        {
          "Profile": "Auto",
          "Level": "auto",
          "Bitrate": 2250,
          "MaxBitrate": 2250,
          "BufferWindow": "00:00:05",
          "Width": 960,
          "Height": 720,
          "BFrames": 3,
          "ReferenceFrames": 3,
          "AdaptiveBFrame": true,
          "Type": "H264Layer",
          "FrameRate": "0/1"
        },
        {
          "Profile": "Auto",
          "Level": "auto",
          "Bitrate": 1250,
          "MaxBitrate": 1250,
          "BufferWindow": "00:00:05",
          "Width": 480,
          "Height": 360,
          "BFrames": 3,
          "ReferenceFrames": 3,
          "AdaptiveBFrame": true,
          "Type": "H264Layer",
          "FrameRate": "0/1"
        }
      ],
      "Type": "H264Video"
    },
    {
      "Profile": "AACLC",
      "Channels": 2,
      "SamplingRate": 48000,
      "Bitrate": 128,
      "Type": "AACAudio"
    }
  ],
  "Outputs": [
    {
      "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",
      "Format": {
        "Type": "MP4Format"
      }
    }
  ]
}
```

## <a name="restrictions-on-cropping"></a>Einschränkungen beim Zuschneiden
Das Zuschneiden ist eine manuelle Aufgabe. Sie müssen Ihr Eingabevideo in ein geeignetes Schneidetool hochladen, in dem Sie die gewünschten Frames auswählen, den Cursor zum Bestimmen der Offsets für das Zuschnittrechteck positionieren, die Codierungsvoreinstellung bestimmen, die für das jeweilige Video optimiert ist, usw. Aufgabe dieses Features ist nicht das Ermöglichen von Aufgaben wie automatische Erkennung und Entfernen von schwarzen Letterbox-/Pillarbox-Rändern in Ihrem Eingabevideo.

Folgende Einschränkungen gelten für das Zuschnittfeature. Wenn diese nicht berücksichtigt werden, kann entweder die Codierungsaufgabe misslingen oder es zu einer unerwarteten Ausgabe kommen.

1. Die Koordinaten und Größe des Zuschnittrechtecks müssen in das Eingabevideo passen.
2. Wie bereits erwähnt, müssen Breite und Höhe in den Codierungseinstellungen dem zugeschnittenen Video entsprechen.
3. Das Zuschneiden ist nur für im Querformat aufgezeichnete Videos möglich (d.h. nicht für mit einem Smartphone im Hochformat aufgezeichnete Videos).
4. Es funktioniert am besten mit progressiven Video, das mit quadratischen Pixeln aufgezeichnet wurde.

## <a name="provide-feedback"></a>Feedback geben
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="next-step"></a>Nächster Schritt
Unter den Azure Media Services-Lernpfaden finden Sie Informationen zu weiteren AMS-Features.  

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]
