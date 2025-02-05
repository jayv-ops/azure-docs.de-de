---
title: 'Versionsverwaltung für die Blockchain-App: Azure Blockchain Workbench'
description: Erfahren Sie, wie Sie Anwendungsversionen in Azure Blockchain Workbench (Vorschauversion) verwenden.
ms.date: 11/20/2019
ms.topic: how-to
ms.reviewer: brendal
ms.openlocfilehash: 2a70112fd0ab6e2f664ca48265c121936b01e58b
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "85209877"
---
# <a name="azure-blockchain-workbench-preview-application-versioning"></a>Versionsverwaltung für die Azure Blockchain Workbench-Anwendung (Vorschauversion)

Sie können mehrere Versionen einer Azure Blockchain Workbench-App (Vorschauversion) erstellen und verwenden. Wenn mehrere Versionen derselben Anwendung hochgeladen werden, ist ein Versionsverlauf verfügbar, und Benutzer können wählen, welche Version sie verwenden möchten.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Voraussetzungen

* Eine Blockchain Workbench-Bereitstellung. Weitere Informationen zu Details dieser Bereitstellung finden Sie unter [Azure Blockchain Workbench-Bereitstellung](deploy.md).
* Eine bereitgestellte Blockchainanwendung in Blockchain Workbench. Siehe [Erstellen einer Blockchainanwendung in Blockchain Workbench](create-app.md).

## <a name="add-an-app-version"></a>Hinzufügen einer Logik-App-Version

Laden Sie zum Hinzufügen einer neuen Version die neuen Konfigurations- und intelligenten Vertragsdateien in Blockchain Workbench hoch.

1. Navigieren Sie in einem Webbrowser zur Blockchain Workbench-Webadresse. Beispiel: `https://{workbench URL}.azurewebsites.net/`. Informationen darüber, wie Sie Ihre Blockchain Workbench-Webadresse finden, erhalten Sie unter [Blockchain Workbench-Web-URL](deploy.md#blockchain-workbench-web-url).
2. Melden Sie sich als [Blockchain Workbench-Administrator](manage-users.md#manage-blockchain-workbench-administrators) an.
3. Wählen Sie die Blockchainanwendung aus, die Sie mit einer anderen Version aktualisieren möchten.
4. Wählen Sie **Version hinzufügen** aus. Der Bereich **Version hinzufügen** wird angezeigt.
5. Wählen Sie die neue Versionsvertragskonfiguration aus, und verkleinern Sie Codedateien zum Hochladen. Die Konfigurationsdatei wird automatisch überprüft. Beheben Sie etwaige Überprüfungsfehler, bevor Sie die Anwendung bereitstellen.
6. Wählen Sie **Version hinzufügen** aus, um die neue Version der Blockchainanwendung hinzuzufügen.

    ![Hinzufügen einer neuen Version](media/version-app/add-version.png)

Die Bereitstellung der Blockchainanwendung kann einige Minuten in Anspruch nehmen. Wenn die Bereitstellung abgeschlossen ist, aktualisieren Sie die Anwendungsseite. Durch Auswahl der Anwendung und der **Versionsverlauf**-Schaltfläche wird der Versionsverlauf der Anwendung angezeigt.

> [!IMPORTANT]
> Vorherige Versionen der Anwendung sind deaktiviert. Sie können frühere Versionen individuell neu aktivieren.
>
> Sie müssen Mitglieder möglicherweise den Anwendungsrollen erneut hinzufügen, wenn in der neuen Version Änderungen an den Anwendungsrollen vorgenommen wurden.

## <a name="using-app-versions"></a>Verwenden von App-Versionen

Standardmäßig wird die neueste aktivierte Version der Anwendung in Blockchain Workbench verwendet. Wenn Sie eine frühere Version einer Anwendung verwenden möchten, müssen Sie zuerst die Version auf der Anwendungsseite auswählen.

1. Wählen Sie im Abschnitt der Blockchain Workbench-Anwendung das Anwendungskontrollkästchen aus, das den zu verwendenden Vertrag enthält. Wenn frühere Versionen aktiviert sind, ist die Versionsverlauf-Schaltfläche verfügbar.
2. Wählen Sie die **Versionsverlauf**-Schaltfläche aus.
3. Wählen Sie im Versionsverlaufsbereich die Version der Anwendung durch Auswahl des Links in der Spalte *Änderungsdatum* aus.

    ![Auswählen einer vorheriger Version](media/version-app/use-version.png)

    Sie können neue Verträge erstellen oder Aktionen an Verträgen vorheriger Versionen vornehmen. Die Version der Anwendung wird hinter dem Namen der Anwendung und eine Warnung über der älteren Version angezeigt.

## <a name="next-steps"></a>Nächste Schritte

* [Problembehandlung bei Azure Blockchain Workbench](troubleshooting.md)
