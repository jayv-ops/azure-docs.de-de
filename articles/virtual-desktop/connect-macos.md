---
title: 'Herstellen einer Verbindung mit Windows Virtual Desktop unter macOS: Azure'
description: Vorgehensweise beim Herstellen einer Verbindung mit Windows Virtual Desktop mithilfe des macOS-Clients.
author: Heidilohr
ms.topic: how-to
ms.date: 04/08/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: f437c43c1e9ad960910e7576db4b3ddf3f6623ec
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "89230873"
---
# <a name="connect-to-windows-virtual-desktop-with-the-macos-client"></a>Herstellen einer Verbindung mit Windows Virtual Desktop mithilfe des macOS-Clients

> Gilt für: macOS 10.12 oder höher

>[!IMPORTANT]
>Dieser Inhalt gilt für Windows Virtual Desktop mit Windows Virtual Desktop-Objekten für Azure Resource Manager. Wenn Sie Windows Virtual Desktop (klassisch) ohne Azure Resource Manager-Objekte verwenden, finden Sie weitere Informationen in [diesem Artikel](./virtual-desktop-fall-2019/connect-macos-2019.md).

Mit unserem herunterladbaren Client können Sie über Ihre macOS-Geräte auf Windows Virtual Desktop-Ressourcen zugreifen. In dieser Anleitung erfahren Sie, wie Sie den-Client einrichten.

## <a name="install-the-client"></a>Installieren des Clients

Beginnen Sie, indem Sie den Client [herunterladen](https://apps.apple.com/app/microsoft-remote-desktop/id1295203466?mt=12) und auf Ihrem macOS-Gerät installieren.

## <a name="subscribe-to-a-feed"></a>Abonnieren eines Feeds

Abonnieren Sie den Feed, den Ihr Administrator Ihnen mitgeteilt hat, um die Liste der verwalteten Ressourcen abzurufen, die Ihnen auf Ihrem macOS-Gerät zur Verfügung stehen.

Abonnieren Sie einen Feed wie folgt:

1. Wählen Sie auf der Hauptseite **Arbeitsbereich hinzufügen** aus, um eine Verbindung mit dem Dienst herzustellen und Ihre Ressourcen abzurufen.
2. Geben Sie die Feed-URL ein. Dies kann eine URL oder E-Mail-Adresse sein:
   - Wenn Sie eine URL verwenden, verwenden Sie die URL, die Ihr Administrator Ihnen mitgeteilt hat. Normalerweise lautet die URL <https://rdweb.wvd.microsoft.com/api/arm/feeddiscovery>.
   - Um E-Mail zu verwenden, gib deine E-Mail-Adresse ein. Dies weist den Client an, nach einer URL zu suchen, die Ihrer E-Mail-Adresse zugeordnet ist, wenn der Administrator den Server auf diese Weise konfiguriert hat.
   - Verwenden Sie <https://rdweb.wvd.azure.us/api/arm/feeddiscovery>, um eine Verbindung über das US Gov-Portal herzustellen.
3. Wählen Sie **Hinzufügen**.
4. Melden Sie sich mit Ihrem Benutzerkonto an, wenn Sie dazu aufgefordert werden.

Nachdem du dich angemeldet hast, sollte eine Liste der verfügbaren Ressourcen angezeigt werden.

Nachdem du einen Feed abonniert hast, wird der Inhalt dieses Feeds automatisch regelmäßig aktualisiert. Ressourcen können basierend auf Änderungen durch Ihren Administrator hinzugefügt, geändert oder entfernt werden.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum macOS-Client finden Sie in der Dokumentation [Erste Schritte mit dem macOS-Client](/windows-server/remote/remote-desktop-services/clients/remote-desktop-mac/).
