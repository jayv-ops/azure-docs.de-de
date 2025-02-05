---
title: Verwenden von Azure Automation-Runbooks und -Modulen im PowerShell-Katalog
description: In diesem Artikel wird erläutert, wie Sie Runbooks und Module von Microsoft und der Community im PowerShell-Katalog verwenden.
services: automation
ms.subservice: process-automation
ms.date: 03/04/2021
ms.topic: conceptual
ms.openlocfilehash: c38a6236fe3ad9164d11d94e5563a7dddf5b4b32
ms.sourcegitcommit: 6386854467e74d0745c281cc53621af3bb201920
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2021
ms.locfileid: "102452780"
---
# <a name="use-runbooks-and-modules-in-powershell-gallery"></a>Verwenden von Runbooks und Modulen im PowerShell-Katalog

Statt eigene Runbooks und Module in Azure Automation zu erstellen, können Sie Szenarien nutzen, die bereits von Microsoft und der Community entwickelt wurden. Sie können PowerShell-Runbooks und -[Module](#modules-in-powershell-gallery) aus dem PowerShell-Katalog sowie [Python-Runbooks](#use-python-runbooks) von der Azure Automation-GitHub-Organisation abrufen. Sie können auch etwas zur Community beitragen, indem Sie [von Ihnen entwickelte Szenarien](#add-a-powershell-runbook-to-the-gallery) zur Verfügung stellen.

> [!NOTE]
> Das TechNet Script Center (TechNet-Skriptcenter) wird eingestellt. Alle Runbooks aus dem Script Center im Runbook-Katalog wurden in unsere [Automation-GitHub-Organisation](https://github.com/azureautomation) verschoben. Weitere Informationen finden Sie [hier](https://techcommunity.microsoft.com/t5/azure-governance-and-management/azure-automation-runbooks-moving-to-github/ba-p/2039337).

## <a name="runbooks-in-powershell-gallery"></a>Runbooks im PowerShell-Katalog

Der [PowerShell-Katalog](https://www.powershellgallery.com/packages) bietet zahlreiche Runbooks von Microsoft und der Community, die Sie in Azure Automation importieren können. Um eins zu verwenden, können Sie ein Runbook aus dem Katalog herunterladen, oder Sie können Runbooks direkt aus dem Katalog oder aus Ihrem Automation-Konto im Azure-Portal importieren.

> [!NOTE]
> Grafische Runbooks werden im PowerShell-Katalog nicht unterstützt.

Das direkte Importieren aus dem PowerShell-Katalog ist nur über das Azure-Portal möglich. Diese Funktion kann nicht mithilfe von PowerShell ausgeführt werden.

> [!NOTE]
> Überprüfen Sie unbedingt den Inhalt der aus dem PowerShell-Katalog heruntergeladenen Runbooks, und seien Sie äußerst vorsichtig, wenn Sie sie in einer Produktionsumgebung installieren und ausführen.

## <a name="modules-in-powershell-gallery"></a>Module im PowerShell-Katalog

PowerShell-Module enthalten Cmdlets, die Sie in Ihren Runbooks verwenden können. Vorhandene Module, die Sie in Azure Automation installieren können, sind im [PowerShell-Katalog](https://www.powershellgallery.com) verfügbar. Sie starten diesen Katalog über das Azure-Portal und installieren die Module direkt in Azure Automation. Sie können sie auch manuell herunterladen und installieren.

## <a name="common-scenarios-available-in-powershell-gallery"></a>Allgemeine im PowerShell-Katalog verfügbare Szenarien

Die nachstehende Liste enthält einige Runbooks, die gängige Szenarien unterstützen. Eine vollständige Liste der vom Azure Automation-Team erstellten Runbooks finden Sie im [AzureAutomationTeam-Profil](https://www.powershellgallery.com/profiles/AzureAutomationTeam).

   * [Update-ModulesInAutomationToLatestVersion:](https://www.powershellgallery.com/packages/Update-ModulesInAutomationToLatestVersion/) importiert die neueste Version aller Module aus dem PowerShell-Katalog in ein Automation-Konto.
   * [Enable-AzureDiagnostics:](https://www.powershellgallery.com/packages/Enable-AzureDiagnostics/) konfiguriert Azure-Diagnose und Log Analytics, sodass Azure Automation-Protokolle mit Auftragsstatus- und Auftragsdatenströmen empfangen werden.
   * [Copy-ItemFromAzureVM:](https://www.powershellgallery.com/packages/Copy-ItemFromAzureVM/) kopiert eine Remotedatei von einem virtuellen Microsoft Azure-Computer.
   * [Copy-ItemToAzureVM:](https://www.powershellgallery.com/packages/Copy-ItemToAzureVM/) kopiert eine lokale Datei auf einen virtuellen Azure-Computer.

## <a name="import-a-powershell-runbook-from-the-runbook-gallery-with-the-azure-portal"></a>Importieren eines PowerShell-Runbooks aus dem Runbookkatalog im Azure-Portal

1. Öffnen Sie im Azure-Portal Ihr Automation-Konto.
1. Wählen Sie unter **Prozessautomatisierung** die Option **Runbookkatalog** aus.
1. Wählen Sie **Quelle: PowerShell-Katalog** aus. Dadurch wird eine Liste der verfügbaren Runbooks angezeigt, die Sie durchsuchen können.
1. Sie können das Suchfeld über der Liste verwenden, um die Liste einzugrenzen, oder Sie können die Filter verwenden, um die Anzeige nach Herausgeber, Typ und Sortierung einzuschränken. Suchen Sie das gewünschte Katalogelement, und wählen Sie es zum Anzeigen der Details aus.

   :::image type="content" source="media/automation-runbook-gallery/browse-gallery-sm.png" alt-text="Durchsuchen des Runbook-Katalogs." lightbox="media/automation-runbook-gallery/browse-gallery-lg.png":::

1. Um ein Element zu importieren, klicken Sie auf dem Blatt „Details“ auf **Importieren**.

   :::image type="content" source="media/automation-runbook-gallery/gallery-item-detail-sm.png" alt-text="Anzeigen von Details eines Runbook-Katalogelements." lightbox="media/automation-runbook-gallery/gallery-item-detail-lg.png":::

1. Ändern Sie optional den Namen des Runbooks, und klicken Sie zum Importieren des Runbooks auf **OK** .
1. Das Runbook wird auf der Registerkarte **Runbooks** des Automation-Kontos angezeigt.

## <a name="import-a--powershell-runbook-from-github-with-the-azure-portal"></a>Importieren eines PowerShell-Runbooks von GitHub über das Azure-Portal

1. Öffnen Sie im Azure-Portal Ihr Automation-Konto.
1. Wählen Sie unter **Prozessautomatisierung** die Option **Runbookkatalog** aus.
1. Wählen Sie **Quelle: GitHub** aus.
1. Sie können die Filter über der Liste verwenden, um die Anzeige nach Herausgeber, Typ und Sortierung einzuschränken. Suchen Sie das gewünschte Katalogelement, und wählen Sie es zum Anzeigen der Details aus.

   :::image type="content" source="media/automation-runbook-gallery/browse-gallery-github-sm.png" alt-text="Durchsuchen des GitHub-Katalogs." lightbox="media/automation-runbook-gallery/browse-gallery-github-lg.png":::

1. Um ein Element zu importieren, klicken Sie auf dem Blatt „Details“ auf **Importieren**.

   :::image type="content" source="media/automation-runbook-gallery/gallery-item-details-blade-github-sm.png" alt-text="Detaillierte Ansicht eines Runbooks aus dem GitHub-Katalog." lightbox="media/automation-runbook-gallery/gallery-item-details-blade-github-lg.png":::

1. Ändern Sie optional den Namen des Runbooks, und klicken Sie zum Importieren des Runbooks auf **OK** .
1. Das Runbook wird auf der Registerkarte **Runbooks** des Automation-Kontos angezeigt.

## <a name="add-a-powershell-runbook-to-the-gallery"></a>Hinzufügen eines PowerShell-Runbooks zum Katalog

Microsoft empfiehlt, Runbooks aus dem PowerShell-Katalog hinzuzufügen, die für andere Kunden nützlich sein könnten. Der PowerShell-Katalog akzeptiert PowerShell-Module und PowerShell-Skripts. Sie können ein Runbook hinzufügen, indem Sie [es in den PowerShell-Katalog hochladen](/powershell/scripting/gallery/how-to/publishing-packages/publishing-a-package).

## <a name="import-a-module-from-the-module-gallery-with-the-azure-portal"></a>Importieren eines Moduls aus dem Modulkatalog im Azure-Portal

1. Öffnen Sie im Azure-Portal Ihr Automation-Konto.
1. Wählen Sie **Module** unter **Freigegebene Ressourcen** aus, um die Liste der Module zu öffnen.
1. Klicken Sie oben auf der Seite auf **Katalog durchsuchen**.

      :::image type="content" source="media/automation-runbook-gallery/modules-blade-sm.png" alt-text="Ansicht des Modulkatalogs." lightbox="media/automation-runbook-gallery/modules-blade-lg.png":::

1. Auf der Seite „Katalog durchsuchen“ können Sie das Suchfeld verwenden, um Übereinstimmungen in einem der folgenden Felder zu finden:

   * Name des Moduls
   * `Tags`
   * Autor
   * Cmdlet-/DSC-Ressourcenname

1. Suchen Sie das gewünschte Modul, und wählen Sie es aus, um seine Details anzuzeigen.

   Wenn Sie einen Drilldown in einem bestimmten Modul ausführen, können Sie weitere Informationen anzeigen. Diese Informationen umfassen einen Link zurück zum PowerShell-Katalog, alle erforderlichen Abhängigkeiten und alle Cmdlets oder DSC-Ressourcen, die das Modul enthält.

   :::image type="content" source="media/automation-runbook-gallery/gallery-item-details-blade-sm.png" alt-text="Detaillierte Ansicht eines Moduls aus dem Katalog." lightbox="media/automation-runbook-gallery/gallery-item-details-blade-lg.png":::

1. Um das Modul direkt in Azure Automation zu installieren, klicken Sie auf **Importieren**.
1. Im Bereich „Importieren“ sehen Sie den Namen des zu importierenden Moduls. Wenn alle Abhängigkeiten installiert sind, ist die Schaltfläche **OK** aktiv. Falls Abhängigkeiten fehlen, müssen diese Abhängigkeiten importiert werden, bevor dieses Modul importiert werden kann.
1. Klicken Sie im Bereich „Importieren“ auf **OK**, um das Modul zu importieren. Wenn Azure Automation ein Modul in Ihr Konto importiert, werden Metadaten zum Modul und den Cmdlets extrahiert. Dieser Vorgang kann einige Minuten dauern, da jede Aktivität extrahiert werden muss.
1. Sie erhalten jeweils eine Benachrichtigung, wenn das Modul bereitgestellt wird und wenn der Vorgang abgeschlossen ist.
1. Nachdem das Modul importiert wurde, können Sie die verfügbaren Aktivitäten anzeigen. Sie können Modulressourcen in Ihren Runbooks und DSC-Ressourcen verwenden.

> [!NOTE]
> Module, die nur PowerShell Core unterstützen, werden in Azure Automation nicht unterstützt und können nicht in das Azure-Portal importiert werden oder direkt über den PowerShell-Katalog bereitgestellt werden.

## <a name="use-python-runbooks"></a>Verwenden von Python-Runbooks

Python-Runbooks finden Sie in der [Azure Automation-GitHub-Organisation](https://github.com/azureautomation). Wenn Sie an unserem GitHub-Repository mitwirken, fügen Sie das Tag **(GitHub-Thema) hinzu: Python3**, wenn Sie Ihren Beitrag hochladen.

## <a name="request-a-runbook-or-module"></a>Anfordern eines Runbooks oder Moduls

Sie können Anforderungen an [User Voice](https://feedback.azure.com/forums/246290-azure-automation/)senden.  Wenn Sie Hilfe beim Schreiben eines Runbooks benötigen oder eine Frage zu PowerShell haben, stellen Sie eine Frage auf der [Frageseite von Microsoft Q&A (Fragen und Antworten)](/answers/topics/azure-automation.html).

## <a name="next-steps"></a>Nächste Schritte

* Informationen zu den ersten Schritten mit einem PowerShell-Runbook finden Sie unter [Tutorial: Erstellen eines PowerShell-Runbooks](learn/automation-tutorial-runbook-textual-powershell.md).
* Informationen zur Arbeit mit Runbooks finden Sie unter [Verwalten eines Runbooks in Azure Automation](manage-runbooks.md).
* Details zu PowerShell finden Sie in der [PowerShell-Dokumentation](/powershell/scripting/overview).
* Eine Referenz zu den PowerShell-Cmdlets finden Sie unter [Az.Automation](/powershell/module/az.automation).
