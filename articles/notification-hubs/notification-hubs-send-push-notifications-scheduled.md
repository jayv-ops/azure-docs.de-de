---
title: 'Gewusst wie: Senden geplanter Benachrichtigungen | Microsoft Docs'
description: In diesem Thema wird beschrieben, wie Sie geplante Benachrichtigungen mit Azure Notification Hubs verwenden.
services: notification-hubs
documentationcenter: .net
keywords: Pushbenachrichtigungen,Pushbenachrichtigung,Planen von Pushbenachrichtigungen
author: sethmanheim
manager: femila
editor: jwargo
ms.assetid: 6b718c75-75dd-4c99-aee3-db1288235c1a
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: dotnet
ms.topic: article
ms.date: 01/04/2019
ms.author: sethm
ms.reviewer: jowargo
ms.lastreviewed: 01/04/2019
ms.custom: devx-track-csharp
ms.openlocfilehash: 5d27ad1f15e9f46f0bc67e62b79606828efb85e6
ms.sourcegitcommit: 6386854467e74d0745c281cc53621af3bb201920
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2021
ms.locfileid: "102453154"
---
# <a name="how-to-send-scheduled-notifications"></a>Gewusst wie: Senden von geplanten Benachrichtigungen

Stellen Sie sich ein Szenario vor, bei dem Sie zu einem späteren Zeitpunkt eine Benachrichtigung senden möchten, aber keine einfache Möglichkeit zur Aktivierung des Back-End-Codes zum Senden der Benachrichtigung besteht. Notification Hubs des Standard-Tarifs unterstützen ein Feature, mit dem Sie Benachrichtigungen bis zu sieben Tage im Voraus planen können.


## <a name="schedule-your-notifications"></a>Planen Ihrer Benachrichtigungen
Verwenden Sie beim Senden einer Benachrichtigung einfach die [`ScheduledNotification`-Klasse](/dotnet/api/microsoft.azure.notificationhubs.schedulednotification#microsoft_azure_notificationhubs_schedulednotification) im Notification Hubs SDK, wie im folgenden Beispiel veranschaulicht:

```csharp
Notification notification = new AppleNotification("{\"aps\":{\"alert\":\"Happy birthday!\"}}");
var scheduled = await hub.ScheduleNotificationAsync(notification, new DateTime(2014, 7, 19, 0, 0, 0));
```

## <a name="cancel-scheduled-notifications"></a>Abbrechen geplanter Benachrichtigungen
Sie können eine zuvor geplante Benachrichtigung auch abbrechen, indem Sie die entsprechende notificationId verwenden:

```csharp
await hub.CancelNotificationAsync(scheduled.ScheduledNotificationId);
```

Es gibt keine Beschränkung der Anzahl von geplanten Benachrichtigungen, die Sie senden können.

## <a name="next-steps"></a>Nächste Schritte

Arbeiten Sie die folgenden Tutorials durch:

 - [Senden von Pushbenachrichtigungen an alle registrierten Geräte](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md)
 - [Senden von Pushbenachrichtigungen an bestimmte Geräte](notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md)
 - [Senden von lokalisierten Pushbenachrichtigungen](notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification.md)
 - [Senden von Pushbenachrichtigungen an bestimmte Benutzer](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md) 
 - [Senden standortbasierter Pushbenachrichtigungen](notification-hubs-push-bing-spatial-data-geofencing-notification.md)
