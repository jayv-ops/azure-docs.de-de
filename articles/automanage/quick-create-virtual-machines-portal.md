---
title: 'Schnellstart: Aktivieren der automatischen Azure-Verwaltung für virtuelle Computer über das Azure-Portal'
description: Hier erfahren Sie, wie Sie über das Azure-Portal schnell die automatische Verwaltung für virtuelle Computer auf einem neuen oder vorhandenen virtuellen Computer aktivieren.
author: ju-shim
ms.service: virtual-machines
ms.subservice: automanage
ms.workload: infrastructure
ms.topic: quickstart
ms.date: 02/17/2021
ms.author: jushiman
ms.openlocfilehash: 5f5f1e70d9ae309c90291ccac1e6dd61e7a9d056
ms.sourcegitcommit: f3ec73fb5f8de72fe483995bd4bbad9b74a9cc9f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "102038383"
---
# <a name="quickstart-enable-azure-automanage-for-virtual-machines-in-the-azure-portal"></a>Schnellstart: Aktivieren der automatischen Azure-Verwaltung für virtuelle Computer im Azure-Portal

Beginnen Sie mit der automatischen Azure-Verwaltung für virtuelle Computer, indem Sie über das Azure-Portal die automatische Verwaltung für einen neuen oder vorhandenen virtuellen Computer aktivieren.


## <a name="prerequisites"></a>Voraussetzungen

Wenn Sie kein Azure-Abonnement besitzen, [erstellen Sie ein Konto](https://azure.microsoft.com/pricing/purchase-options/pay-as-you-go/), bevor Sie beginnen.

> [!NOTE]
> Mit kostenlosen Testkonten ist kein Zugriff auf die virtuellen Computer möglich, die in diesem Tutorial verwendet werden. Führen Sie ein Upgrade auf ein Abonnement mit nutzungsbasierter Zahlung durch.

> [!IMPORTANT]
> Sie müssen über die Rolle **Mitwirkender** für die Ressourcengruppe verfügen, die Ihre virtuellen Computer enthält, um Automanage mithilfe eines vorhandenen Automanage-Kontos zu aktivieren. Wenn Sie automatische Verwaltung mit einem neuen Konto für die automatische Verwaltung aktivieren, benötigen Sie die folgenden Berechtigungen: Rolle **Besitzer** oder **Mitwirkender** sowie Rollen vom Typ **Benutzerzugriffsadministrator** für Ihr Abonnement.


## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich beim [Azure-Portal](https://aka.ms/AutomanagePortal-Ignite21) an.

## <a name="enable-automanage-for-a-single-vm"></a>Aktivieren von Automanage für eine einzelne VM

1. Navigieren Sie zu der VM, die Sie aktivieren möchten.

2. Klicken Sie im Inhaltsverzeichnis unter **Vorgänge** auf den Eintrag **Automanage (Vorschau)** .

3. Wählen Sie **Erste Schritte** aus.

    :::image type="content" source="media\quick-create-virtual-machine-portal\vmmanage-getstartedbutton.png" alt-text="„Erste Schritte“ auf einer einzelnen VM.":::

4. Wählen Sie Ihre Automanage-Einstellungen aus (Umgebung, Voreinstellungen, Automanage-Konto), und klicken Sie auf **Aktivieren**.

    :::image type="content" source="media\quick-create-virtual-machine-portal\vmmanage-enablepane.png" alt-text="„Aktivieren“ auf einer einzelnen VM.":::

## <a name="enable-automanage-for-multiple-vms"></a>Aktivieren von Automanage für mehrere VMs

1. Suchen Sie über die Suchleiste nach **Automanage – Best Practices für Azure-VMs**, und wählen Sie die Option aus.

2. Wählen Sie **Für vorhandene VM aktivieren** aus.

    :::image type="content" source="media\quick-create-virtual-machine-portal\zero-vm-list-view.png" alt-text="Für vorhandene VM aktivieren":::

3. Gehen Sie auf dem Blatt **Computer auswählen** wie folgt vor:
    1. Filtern Sie die Liste der virtuellen Computer nach Ihrem **Abonnement** und nach der **Ressourcengruppe**.
    1. Aktivieren Sie das Kontrollkästchen für jeden virtuellen Computer, den Sie integrieren möchten.
    1. Klicken Sie auf die Schaltfläche **Auswählen**.

    :::image type="content" source="media\quick-create-virtual-machine-portal\existing-vm-select-machine.png" alt-text="Auswählen eines vorhandenen virtuellen Computers aus der Liste verfügbarer virtueller Computer":::

4. Wählen Sie unter **Umgebung** Ihren Umgebungstyp aus: **Dev/Test** oder **Produktion**. 

    :::image type="content" source="media\quick-create-virtual-machine-portal\existing-vm-quick-create.png" alt-text="Auswählen der Umgebung":::

   Klicken Sie auf **Compare Environment Details** (Umgebungsdetails vergleichen), um die Unterschiede zwischen den Umgebungen anzuzeigen.
    1. Wählen Sie in der Dropdownliste eine Umgebung aus: *Dev/Test* für Tests oder *Produktion* für die Produktion.
    1. Klicken Sie auf die Schaltfläche **OK** .

    :::image type="content" source="media\quick-create-virtual-machine-portal\browse-production-profile.png" alt-text="Durchsuchen der Produktionsumgebung":::

5. Standardmäßig ist die Einstellung **Azure Best Practices** für die Konfigurationseinstellungen ausgewählt. Um dies zu ändern, erstellen Sie eine neue Voreinstellung, oder wählen Sie eine vorhandene aus. 

    :::image type="content" source="media\quick-create-virtual-machine-portal\create-preference.png" alt-text="Erstellen der Voreinstellung":::

6. Klicken Sie auf die Schaltfläche **Aktivieren**.


## <a name="enable-automanage-for-a-new-vm"></a>Aktivieren von Automanage für eine neue VM

Melden Sie sich [hier](https://aka.ms/AzureAutomanagePreview) beim Azure-Portal an, um einen neuen virtuellen Computer zu erstellen und automatische Verwaltung zu aktivieren.

1. Füllen Sie die Registerkarte **Grundlagen** mit Ihren VM-Details aus.

> [!NOTE]
> Überprüfen Sie die für Automanage [unterstützten Regionen](automanage-virtual-machines.md#supported-regions) sowie die für Automanage unterstützten [Linux-Distributionen](automanage-linux.md#supported-linux-distributions-and-versions) und [Windows Server-Versionen](automanage-windows-server.md#supported-windows-server-versions).

2. Navigieren Sie zur Registerkarte **Verwaltung**, und wählen Sie Ihre **Automanage-Umgebung** aus.

    :::image type="content" source="media\quick-create-virtual-machine-portal\vmcreate-managementtab.png" alt-text="„Automanage aktivieren“ auf der Registerkarte „Verwaltung“.":::

3. Belassen Sie die übrigen Standardeinstellungen, und wählen Sie dann die Schaltfläche **Überprüfen + erstellen** am unteren Rand der Seite aus.

4. Wenn eine Meldung über die erfolgreiche Validierung angezeigt wird, wählen Sie **Erstellen** aus.

## <a name="disable-automanage-for-vms"></a>Deaktivieren der automatischen Verwaltung für virtuelle Computer

Sie können die Verwendung der automatischen Azure-Verwaltung für virtuelle Computer schnell beenden, indem Sie die automatische Verwaltung deaktivieren.

:::image type="content" source="media\automanage-virtual-machines\disable-step-1.png" alt-text="Deaktivieren der automatischen Verwaltung auf einem virtuellen Computer":::

1. Navigieren Sie zur Seite **Automatische Verwaltung – Best Practices für Azure-VMs**, auf der alle automatisch verwalteten virtuellen Computer aufgeführt sind.
1. Aktivieren Sie das Kontrollkästchen neben dem virtuellen Computer, den Sie deaktivieren möchten.
1. Klicken Sie auf die Schaltfläche **Automatische Verwaltung deaktivieren**.
1. Lesen Sie die Meldung im angezeigten Popupelement sorgfältig durch, bevor Sie dem **Deaktivieren** zustimmen.


## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie eine neue Ressourcengruppe erstellt haben, um die automatische Azure-Verwaltung für virtuelle Computer zu testen, und die Ressourcengruppe nicht mehr benötigen, können Sie sie löschen. Wenn Sie die Gruppe löschen, werden auch der virtuelle Computer und alle Ressourcen in der Ressourcengruppe gelöscht.

Die automatische Azure-Verwaltung erstellt Standardressourcengruppen zum Speichern von Ressourcen. Überprüfen Sie die Ressourcengruppen mit der Namenskonvention „DefaultResourceGroupRegionName“ und „AzureBackupRGRegionName“, um alle Ressourcen zu bereinigen.

1. Wählen Sie die **Ressourcengruppe** aus.
1. Wählen Sie auf der Seite für die Ressourcengruppe die Option **Löschen** aus.
1. Geben Sie bei entsprechender Aufforderung den Namen der Ressourcengruppe ein, und wählen Sie dann **Löschen** aus.


## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie die automatische Azure-Verwaltung für virtuelle Computer aktiviert.

Erfahren Sie, wie Sie angepasste Einstellungen erstellen und anwenden können, wenn Sie die automatische Verwaltung auf virtuellen Computern aktivieren.

> [!div class="nextstepaction"]
> [Azure Automanage für VMs: benutzerdefinierte Konfigurationseinstellungen](virtual-machines-custom-preferences.md)
