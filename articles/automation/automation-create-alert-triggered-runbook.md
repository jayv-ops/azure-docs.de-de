---
title: Verwenden einer Warnung zum Auslösen eines Azure Automation-Runbooks
description: In diesem Artikel wird beschrieben, wie Sie bei Auslösung einer Azure-Warnung ein Runbook auslösen.
services: automation
ms.subservice: process-automation
ms.date: 02/14/2021
ms.topic: conceptual
ms.openlocfilehash: ea7979ad4a401d317ec126b7abfe354690475235
ms.sourcegitcommit: ac035293291c3d2962cee270b33fca3628432fac
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/24/2021
ms.locfileid: "104953101"
---
# <a name="use-an-alert-to-trigger-an-azure-automation-runbook"></a>Verwenden einer Warnung zum Auslösen eines Azure Automation-Runbooks

Sie können [Azure Monitor](../azure-monitor/overview.md) verwenden, um in Azure grundlegende Metriken und Protokolle für die meisten Dienste zu überwachen. Sie können Azure Automation-Runbooks aufrufen, indem Sie [Aktionsgruppen](../azure-monitor/alerts/action-groups.md) verwenden, um Aufgaben basierend auf Warnungen zu automatisieren. In diesem Artikel erfahren Sie, wie Sie ein Runbook mit Warnungen konfigurieren und ausführen.

## <a name="alert-types"></a>Warnungstypen

Sie können Automation-Runbooks mit drei Warnungstypen verwenden:

* Allgemeine Warnungen
* Aktivitätsprotokollwarnungen
* Metrikwarnungen nahezu in Echtzeit

> [!NOTE]
> Mit dem allgemeinen Warnungsschema wird die Benutzeroberfläche für Warnungsbenachrichtigungen in Azure standardisiert. Bisher wurden für die Warnungstypen von Azure (Metrik, Protokoll und Aktivitätsprotokoll) eigene E-Mail-Vorlagen, Webhookschemas usw. verwendet. Weitere Informationen finden Sie unter [Allgemeines Warnungsschema](../azure-monitor/alerts/alerts-common-schema.md).

Wenn eine Warnung ein Runbook aufruft, erfolgt der eigentliche Aufruf in Form einer HTTP POST-Anforderung an den Webhook. Der Text der POST-Anforderung enthält ein JSON-formatiertes Objekt, das nützliche Eigenschaften im Zusammenhang mit der Warnung enthält. In der folgenden Tabelle sind Links zu den Nutzlastschemas der einzelnen Warnungstypen angegeben:

|Warnung  |BESCHREIBUNG|Nutzlast und Schema  |
|---------|---------|---------|
|[Allgemeines Warnungsschema](../azure-monitor/alerts/alerts-common-schema.md)|Mit dem allgemeinen Warnungsschema wird die Benutzeroberfläche für Warnungsbenachrichtigungen in Azure standardisiert.|Nutzlastschema von allgemeinen Warnungen|
|[Aktivitätsprotokollwarnung](../azure-monitor/alerts/activity-log-alerts.md)    |Sendet eine Benachrichtigung, wenn ein beliebiges neues Ereignis im Azure-Aktivitätsprotokoll bestimmte Bedingungen erfüllt. Beispiel: Wenn ein `Delete VM`-Vorgang in **myProductionResourceGroup** auftritt oder wenn ein neues Azure Service Health-Ereignis mit dem Status „Aktiv“ angezeigt wird.| [Nutzlastschema vom Typ „Aktivitätsprotokollwarnung“](../azure-monitor/alerts/activity-log-alerts-webhook.md)        |
|[Near Real-Time Metric Alerts](../azure-monitor/alerts/alerts-metric-near-real-time.md)    |Sendet eine Benachrichtigung schneller als Metrikwarnungen, wenn mindestens eine Metrik auf Plattformebene bestimmte Bedingungen erfüllt. Beispiel: Wenn der Wert für **CPU in %** auf einer VM größer als „90“ ist und der Wert für **Netzwerk eingehend** in den letzten fünf Minuten über „500 MB“ gelegen hat.| [Nutzlastschema vom Typ „Near Real-Time Metric Alert“](../azure-monitor/alerts/alerts-webhooks.md#payload-schema)          |

Da sich die Daten unterscheiden, die von den einzelnen Typen von Warnungen bereitgestellt werden, wird jeder Warnungstyp anders behandelt. Im nächsten Abschnitt erfahren Sie, wie Sie ein Runbook erstellen, um verschiedene Warnungstypen zu behandeln.

## <a name="create-a-runbook-to-handle-alerts"></a>Erstellen eines Runbooks zum Behandeln von Warnungen

Um bei Warnungen Automation einzusetzen, benötigen Sie ein Runbook, das über geeignete Logik verfügt, um die an das Runbook übergebene JSON-Nutzlast der Warnungen zu verarbeiten. Das folgende Beispielrunbook muss von einer Azure-Warnung aufgerufen werden.

Wie im vorherigen Abschnitt beschrieben, weist jeder Typ von Warnung ein anderes Schema auf. Mit dem Skript werden die Webhookdaten im Runbook-Eingabeparameter `WebhookData` einer Warnung verwendet. Anschließend wertet das Skript die JSON-Nutzlast aus, um zu bestimmen, welcher Warnungstyp verwendet wird.

In diesem Beispiel wird eine Warnung von einem virtuellen Computer verwendet. Es ruft die VM-Daten aus der Nutzlast ab und verwendet diese Informationen dann, um den virtuellen Computer zu beenden. Die Verbindung muss in dem Automation-Konto eingerichtet sein, unter dem das Runbook ausgeführt wird. Wenn Warnungen zum Auslösen von Runbooks verwendet werden, muss unbedingt der Status der Warnung im ausgelösten Runbook überprüft werden. Das Runbook wird bei jeder Zustandsänderung der Warnung ausgelöst. Warnungen verfügen über mehrere Zustände, deren beiden häufigsten „Aktiviert“ und „Aufgelöst“ sind. Prüfen Sie in Ihrer Runbooklogik auf diesen Zustand, um sicherzustellen, dass Ihr Runbook nur einmal ausgeführt wird. Das Beispiel in diesem Artikel zeigt, wie Sie die Suche auf Warnungen mit dem Zustand „Aktiviert“ einschränken.

Das Runbook verwendet das [ausführende Konto](./automation-security-overview.md) für die Verbindungsressource `AzureRunAsConnection` für die Authentifizierung bei Azure, um die Verwaltungsaktion für die VM durchzuführen.

Verwenden Sie dieses Beispiel, um ein Runbook mit dem Namen **Stop-AzureVmInResponsetoVMAlert** zu erstellen. Sie können das PowerShell-Skript ändern und mit vielen verschiedenen Ressourcen nutzen.

1. Wechseln Sie zu Ihrem Azure Automation-Konto.
2. Wählen Sie unter **Prozessautomatisierung** die Option **Runbooks** aus.
3. Wählen Sie oben in der Liste mit den Runbooks die Option **+ Runbook erstellen** aus.
4. Geben Sie auf der Seite **Runbook hinzufügen** als Runbookname **Stop-AzureVmInResponsetoVMAlert** ein. Wählen Sie als Runbooktyp **PowerShell** aus. Wählen Sie anschließend **Erstellen**.  
5. Kopieren Sie das folgende PowerShell-Beispiel auf die Seite **Bearbeiten**.

    ```powershell-interactive
    [OutputType("PSAzureOperationResponse")]
    param
    (
        [Parameter (Mandatory=$false)]
        [object] $WebhookData
    )
    $ErrorActionPreference = "stop"

    if ($WebhookData)
    {
        # Get the data object from WebhookData
        $WebhookBody = (ConvertFrom-Json -InputObject $WebhookData.RequestBody)

        # Get the info needed to identify the VM (depends on the payload schema)
        $schemaId = $WebhookBody.schemaId
        Write-Verbose "schemaId: $schemaId" -Verbose
        if ($schemaId -eq "azureMonitorCommonAlertSchema") {
            # This is the common Metric Alert schema (released March 2019)
            $Essentials = [object] ($WebhookBody.data).essentials
            # Get the first target only as this script doesn't handle multiple
            $alertTargetIdArray = (($Essentials.alertTargetIds)[0]).Split("/")
            $SubId = ($alertTargetIdArray)[2]
            $ResourceGroupName = ($alertTargetIdArray)[4]
            $ResourceType = ($alertTargetIdArray)[6] + "/" + ($alertTargetIdArray)[7]
            $ResourceName = ($alertTargetIdArray)[-1]
            $status = $Essentials.monitorCondition
        }
        elseif ($schemaId -eq "AzureMonitorMetricAlert") {
            # This is the near-real-time Metric Alert schema
            $AlertContext = [object] ($WebhookBody.data).context
            $SubId = $AlertContext.subscriptionId
            $ResourceGroupName = $AlertContext.resourceGroupName
            $ResourceType = $AlertContext.resourceType
            $ResourceName = $AlertContext.resourceName
            $status = ($WebhookBody.data).status
        }
        elseif ($schemaId -eq "Microsoft.Insights/activityLogs") {
            # This is the Activity Log Alert schema
            $AlertContext = [object] (($WebhookBody.data).context).activityLog
            $SubId = $AlertContext.subscriptionId
            $ResourceGroupName = $AlertContext.resourceGroupName
            $ResourceType = $AlertContext.resourceType
            $ResourceName = (($AlertContext.resourceId).Split("/"))[-1]
            $status = ($WebhookBody.data).status
        }
        elseif ($schemaId -eq $null) {
            # This is the original Metric Alert schema
            $AlertContext = [object] $WebhookBody.context
            $SubId = $AlertContext.subscriptionId
            $ResourceGroupName = $AlertContext.resourceGroupName
            $ResourceType = $AlertContext.resourceType
            $ResourceName = $AlertContext.resourceName
            $status = $WebhookBody.status
        }
        else {
            # Schema not supported
            Write-Error "The alert data schema - $schemaId - is not supported."
        }

        Write-Verbose "status: $status" -Verbose
        if (($status -eq "Activated") -or ($status -eq "Fired"))
        {
            Write-Verbose "resourceType: $ResourceType" -Verbose
            Write-Verbose "resourceName: $ResourceName" -Verbose
            Write-Verbose "resourceGroupName: $ResourceGroupName" -Verbose
            Write-Verbose "subscriptionId: $SubId" -Verbose

            # Determine code path depending on the resourceType
            if ($ResourceType -eq "Microsoft.Compute/virtualMachines")
            {
                # This is an Resource Manager VM
                Write-Verbose "This is an Resource Manager VM." -Verbose

                # Authenticate to Azure with service principal and certificate and set subscription
                Write-Verbose "Authenticating to Azure with service principal and certificate" -Verbose
                $ConnectionAssetName = "AzureRunAsConnection"
                Write-Verbose "Get connection asset: $ConnectionAssetName" -Verbose
                $Conn = Get-AutomationConnection -Name $ConnectionAssetName
                if ($Conn -eq $null)
                {
                    throw "Could not retrieve connection asset: $ConnectionAssetName. Check that this asset exists in the Automation account."
                }
                Write-Verbose "Authenticating to Azure with service principal." -Verbose
                Add-AzAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint | Write-Verbose
                Write-Verbose "Setting subscription to work against: $SubId" -Verbose
                Set-AzContext -SubscriptionId $SubId -ErrorAction Stop | Write-Verbose

                # Stop the Resource Manager VM
                Write-Verbose "Stopping the VM - $ResourceName - in resource group - $ResourceGroupName -" -Verbose
                Stop-AzVM -Name $ResourceName -ResourceGroupName $ResourceGroupName -Force
                # [OutputType(PSAzureOperationResponse")]
            }
            else {
                # ResourceType not supported
                Write-Error "$ResourceType is not a supported resource type for this runbook."
            }
        }
        else {
            # The alert status was not 'Activated' or 'Fired' so no action taken
            Write-Verbose ("No action taken. Alert status: " + $status) -Verbose
        }
    }
    else {
        # Error
        Write-Error "This runbook is meant to be started from an Azure alert webhook only."
    }
    ```

6. Wählen Sie die Option **Veröffentlichen**, um das Runbook zu speichern und zu veröffentlichen.

## <a name="create-the-alert"></a>Erstellen der Warnung

Warnungen verwenden Aktionsgruppen, die Sammlungen von Aktionen sind, die von der Warnung ausgelöst werden. Runbooks stellen lediglich eine der vielen Aktionen dar, die Sie mit Aktionsgruppen verwenden können.

1. Wählen Sie in Ihrem Automation-Konto unter **Überwachung** die Option **Warnungen** aus.
1. Wählen Sie **+ Neue Warnungsregel** aus.
1. Klicken Sie unter **Ressource** auf **Auswählen**. Wählen Sie auf der Seite **Ressource auswählen** Ihren virtuellen Computer aus, damit Warnungen von ihm ausgehen können, und klicken Sie auf **Fertig**.
1. Klicken Sie unter **Bedingung** auf **Bedingung hinzufügen**. Wählen Sie das Signal aus, das Sie verwenden möchten, z.B. **CPU in Prozent**, und klicken Sie auf **Fertig**.
1. Geben Sie auf der Seite **Signallogik konfigurieren** Ihren **Schwellenwert** unter **Warnungslogik** ein, und klicken Sie auf **Fertig**.
1. Wählen Sie unter **Aktionsgruppen** die Option **Neu erstellen** aus.
1. Geben Sie auf der Seite **Aktionsgruppe hinzufügen** Ihrer Aktionsgruppe einen Namen und einen kurzen Namen.
1. Geben Sie der Aktion einen Namen. Wählen Sie als Aktionstyp **Automation-Runbook** aus.
1. Wählen Sie **Details bearbeiten** aus. Wählen Sie auf der Seite **Runbook konfigurieren** unter **Runbook-Quelle** die Option **Benutzer**.  
1. Wählen Sie Ihr **Abonnement** und **Automation-Konto** und anschließend das Runbook **Stop-AzureVmInResponsetoVMAlert** aus.  
1. Wählen Sie **Ja** für **Allgemeines Warnungsschema aktivieren** aus.
1. Wählen Sie **OK**, um die Aktionsgruppe zu erstellen.

    ![Seite „Aktionsgruppe hinzufügen“](./media/automation-create-alert-triggered-runbook/add-action-group.png)

    Sie können diese Aktionsgruppe in den [Aktivitätsprotokollwarnungen](../azure-monitor/alerts/activity-log-alerts.md) und [Near Real-Time Alerts](../azure-monitor/alerts/alerts-overview.md) verwenden, die Sie erstellen.

1. Fügen Sie unter **Warnungsdetails** einen Warnungsregelnamen und eine Beschreibung hinzu, und klicken Sie auf **Warnungsregel erstellen**.

## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Starten eines Runbooks mithilfe eines Webhook finden Sie unter [Starten eines Runbooks mit einem Webhook](automation-webhooks.md).
* Informationen zu verschiedenen Methoden zum Starten eines Runbooks finden Sie unter [Starten eines Runbooks](./start-runbooks.md).
* Informationen zum Erstellen von Aktivitätsprotokollwarnungen finden Sie unter [Erstellen von Aktivitätsprotokollwarnungen](../azure-monitor/alerts/activity-log-alerts.md).
* Informationen zum Erstellen von Near Real-Time Alerts finden Sie unter [Erstellen einer Warnungsregel im Azure-Portal](../azure-monitor/alerts/alerts-metric.md?toc=/azure/azure-monitor/toc.json).
* Eine Referenz zu den PowerShell-Cmdlets finden Sie unter [Az.Automation](/powershell/module/az.automation).