---
title: 'Tutorial: Device Update for Azure IoT Hub unter Verwendung des Raspberry Pi 3 B+ Yocto-Referenzimages | Microsoft-Dokumentation'
description: Erste Schritte mit Device Update for Azure IoT Hub unter Verwendung des Raspberry Pi 3 B+ Yocto-Referenzimages.
author: valls
ms.author: valls
ms.date: 2/11/2021
ms.topic: tutorial
ms.service: iot-hub-device-update
ms.openlocfilehash: ca689df97e7268a5c0f7c0479e6514b98ffda9f2
ms.sourcegitcommit: 5bbc00673bd5b86b1ab2b7a31a4b4b066087e8ed
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2021
ms.locfileid: "102443453"
---
# <a name="device-update-for-azure-iot-hub-tutorial-using-the-raspberry-pi-3-b-reference-image"></a>Device Update for Azure IoT Hub unter Verwendung des Raspberry Pi 3 B+-Referenzimages

Device Update for IoT Hub unterstützt zwei Arten von Updates: imagebasiert und paketbasiert.

Imageupdates bieten ein höheres Maß an Vertrauen in den Endzustand des Geräts. Es ist in der Regel einfacher, die Ergebnisse eines Imageupdates zwischen einer Vorproduktions- und einer Produktionsumgebung zu replizieren, da dies nicht die gleichen Herausforderungen wie bei Paketen und deren Abhängigkeiten mit sich bringt. Aufgrund ihrer atomischen Natur kann auch ein A/B-Failovermodell problemlos übernommen werden.

In diesem Tutorial erfahren Sie Schritt für Schritt, wie Sie ein imagebasiertes End-to-End-Update mithilfe von Device Update for IoT Hub ausführen. 

In diesem Lernprogramm lernen Sie Folgendes:
> [!div class="checklist"]
> * Herunterladen eines Images
> * Hinzufügen eines Tags zu Ihrem IoT-Gerät
> * Importieren eines Updates
> * Erstellen einer Gerätegruppe
> * Bereitstellen eines Imageupdates
> * Überwachen der Updatebereitstellung

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen
* Zugriff auf einen IoT Hub. Es wird empfohlen, einen S1-Tarif (Standard) oder höher zu verwenden.

## <a name="download-image"></a>Herunterladen eines Images

Es gibt drei Images, die als Teil der „Ressourcen“ in einer bestimmten [GitHub-Version von Device Update](https://github.com/Azure/iot-hub-device-update/releases) verfügbar sind. Das Basisimage (adu-base-image) und ein Updateimage (adu-update-image) werden bereitgestellt, sodass Sie Rollouts für verschiedene Versionen testen können, ohne die SD-Karte im Gerät einspielen zu müssen. Dazu müssen Sie die Updateimages im Rahmen des Imports in den Device Update for IoT Hub-Dienst hochladen.

## <a name="flash-sd-card-with-image"></a>Einspielen des Images auf die SD-Karte

Installieren Sie mithilfe Ihres bevorzugten Tools zum Einspielen von Betriebssystemen das Device Update-Basisimage (adu-base-image) auf der SD-Karte, die im Raspberry Pi 3 B+-Gerät verwendet werden soll.

### <a name="using-bmaptool-to-flash-sd-card"></a>Verwenden von bmaptool zum Einspielen der SD-Karte

1. Falls noch nicht geschehen, installieren Sie das Hilfsprogramm `bmaptool`.

   ```shell
   sudo apt-get install bmap-tools
   ```

2. Suchen Sie den Pfad für die SD-Karte in `/dev`. Der Pfad sollte in etwa wie `/dev/sd*` oder `/dev/mmcblk*` aussehen. Sie können das Hilfsprogramm `dmesg` verwenden, um den richtigen Pfad zu finden.

3. Sie müssen die Einbindung aller eingebundenen Partitionen vor dem Einspielen aufheben.

   ```shell
   sudo umount /dev/<device>
   ```

4. Stellen Sie sicher, dass Sie über Schreibberechtigungen für das Gerät verfügen.

   ```shell
   sudo chmod a+rw /dev/<device>
   ```

5. Optional. Zum schnelleren Einspielen laden Sie die bimap-Datei zusammen mit der Imagedatei herunter und legen sie in demselben Verzeichnis ab.

6. Spielen Sie die SD-Karte ein.

   ```shell
   sudo bmaptool copy <path to image> /dev/<device>
   ```
   
Die Software Device Update for Azure IoT Hub unterliegt den folgenden Lizenzbedingungen:
   * [Lizenz für Device Update für IoT Hub](https://github.com/Azure/iot-hub-device-update/blob/main/LICENSE.md)
   * [Lizenz für den Übermittlungsoptimierungs-Client](https://github.com/microsoft/do-client/blob/main/LICENSE.md)
   
Lesen Sie vor Verwendung des Agents die Lizenzbedingungen. Durch die Installation und Verwendung erklären Sie Ihre Zustimmung zu diesen Bestimmungen. Wenn Sie den Lizenzbedingungen nicht zustimmen, verwenden Sie den Device Update for IoT Hub-Agent nicht.

## <a name="create-device-in-iot-hub-and-get-connection-string"></a>Erstellen des Geräts in IoT Hub und Abrufen der Verbindungszeichenfolge

Jetzt muss das Gerät zu Azure IoT Hub hinzugefügt werden.  In Azure IoT Hub wird eine Verbindungszeichenfolge für das Gerät generiert.

1. Starten Sie im Azure-Portal Device Update for IoT Hub.
2. Erstellen Sie ein neues Gerät.
3. Navigieren Sie auf der linken Seite der Seite zu „Explorers > IoT Devices“ (Explorers > IoT-Geräte), und wählen Sie „New“ (Neu) aus.
4. Geben Sie unter „Device ID“ (Geräte-ID) einen Namen für das Gerät an. Stellen Sie sicher, dass das Kontrollkästchen „Autogenerate keys“(Schlüssel automatisch generieren) aktiviert ist.
5. Wählen Sie „Save“ (Speichern) aus.
6. Nun wird die Seite „Devices“ (Geräte) erneut angezeigt, und das von Ihnen erstellte Gerät sollte in der Liste enthalten sein. Wählen Sie dieses Gerät aus.
7. Wählen Sie in der Ansicht „Gerät“ das Symbol „Kopieren“ neben „Primary Connection String“ (Primäre Verbindungszeichenfolge) aus.
8. Fügen Sie die kopierten Zeichen irgendwo zur späteren Verwendung in den folgenden Schritte ein.
   **Diese kopierte Zeichenfolge ist Ihre Geräteverbindungszeichenfolge**.

## <a name="provision-connection-string-on-sd-card"></a>Bereitstellen der Verbindungszeichenfolge auf der SD-Karte

1. Stellen Sie sicher, dass der Raspberry Pi3 mit dem Netzwerk verbunden ist.
2. Verwenden Sie in PowerShell den folgenden Befehl, um sich per ssh in das Gerät einzuwählen.
   ```markdown
   ssh raspberrypi3 -l root
      ```
4. Melden Sie sich als „root“ an, und lassen Sie das Kennwort leer.
5. Nachdem Sie sich erfolgreich per ssh beim Gerät angemeldet haben, führen Sie die folgenden Befehle aus:
 
Ersetzen Sie `<device connection string>` durch Ihre Verbindungszeichenfolge.
 ```markdown
    echo "connection_string=<device connection string>" > adu-conf.txt  
    echo "aduc_manufacturer=ADUTeam" >> adu-conf.txt
    echo "aduc_model=RefDevice" >> adu-conf.txt
   ```

## <a name="connect-the-device-in-device-update-iot-hub"></a>Verbinden Sie das Gerät in Device Update IoT Hub.

1. Wählen Sie auf der linken Seite unter „Explorers“ (Explorer) die Option „IoT Devices“ (IoT-Geräte) aus.
2. Wählen Sie den Link mit Ihrem Gerätenamen aus.
3. Wählen Sie oben auf der Seite „Device Twin“ (Gerätezwilling) aus.
4. Suchen Sie unter dem Abschnitt „reported“ (gemeldet) der Eigenschaften des Gerätezwillings nach der Linux-Kernelversion.
Für ein neues Gerät, das noch kein Update von Device Update erhalten hat, stellt der Wert [DeviceManagement:DeviceInformation:1.swVersion](device-update-plug-and-play.md) die auf dem Gerät aktive Firmwareversion dar.  Sobald ein Update auf ein Gerät angewendet wurde, verwendet Device Update den Eigenschaftswert [AzureDeviceUpdateCore:ClientMetadata:4.installedUpdateId](device-update-plug-and-play.md), um die auf dem Gerät aktive Firmwareversion darzustellen.
5. Die Dateien für Basis- und Updateimage weisen eine Versionsnummer im Dateinamen auf.

   ```markdown
   adu-<image type>-image-<machine>-<version number>.<extension>
   ```

Verwenden Sie diese Versionsnummer im nachfolgenden Schritt „Import Update“ (Importupdate).

## <a name="add-a-tag-to-your-device"></a>Hinzufügen eines Tags zu Ihrem Gerät

1. Melden Sie sich am [Azure-Portal](https://portal.azure.com) an, und navigieren Sie zum IoT Hub.

2. Suchen Sie im linken Navigationsbereich unter „IoT Devices“ (IoT-Geräte) oder „IoT Edge“ nach Ihrem IoT-Gerät, und navigieren Sie zum Gerätezwilling.

3. Löschen Sie im Gerätezwilling alle vorhandenen Device Update-Tagwerte, indem Sie diese auf NULL festlegen.

4. Fügen Sie wie unten gezeigt einen neuen Device Update-Tagwert hinzu.

```JSON
    "tags": {
            "ADUGroup": "<CustomTagValue>"
            }
```

## <a name="import-update"></a>Importieren des Updates

1. Erstellen Sie anhand [dieser Anweisungen](import-update.md) ein Importmanifest.
2. Wählen Sie in der linken Navigationsleiste unter „Automatic Device Management“ (Automatische Geräteverwaltung) die Option „Device Updates“ aus.
3. Wählen Sie die Registerkarte „Updates“ aus.
4. Wählen Sie „+ Import New Update“ (Neues Update importieren) aus.
5. Wählen Sie das Ordnersymbol oder das Textfeld unter „Select an Import Manifest File“ (Importmanifestdatei auswählen) aus. Daraufhin wird ein Dialogfeld für Dateiauswahl angezeigt. Wählen Sie das weiter oben erstellte Importmanifest aus.  Wählen Sie als Nächstes das Ordnersymbol oder das Textfeld unter „Select one or more update files“ (Mindestens eine Updatedatei auswählen) aus. Daraufhin wird ein Dialogfeld für Dateiauswahl angezeigt. Wählen Sie die Updatedatei aus, die Sie auf Ihren IoT-Geräten bereitstellen möchten.
   
   :::image type="content" source="media/import-update/select-update-files.png" alt-text="Screenshot: Auswahl der Updatedatei." lightbox="media/import-update/select-update-files.png":::

5. Wählen Sie unter „Select a storage container“ (Speichercontainer auswählen) das Ordnersymbol oder das Textfeld aus. Wählen Sie dann das entsprechende Speicherkonto aus.

6. Wenn Sie bereits einen Container erstellt haben, können Sie ihn wiederverwenden. (Wählen Sie andernfalls „+ Container“ aus, um einen neuen Speichercontainer für Updates zu erstellen.)  Wählen Sie den Container aus, den Sie verwenden möchten, und klicken Sie auf „Select“ (Auswählen).
  
  :::image type="content" source="media/import-update/container.png" alt-text="Screenshot: Auswahl des Containers." lightbox="media/import-update/container.png":::

7. Wählen Sie „Submit“ (Übermitteln) aus, um den Importvorgang zu starten.

8. Der Importvorgang beginnt, und der Bildschirm ändert sich in den Abschnitt „Import History“ (Importverlauf). Wählen Sie „Refresh“ (Aktualisieren) aus, um den Fortschritt anzuzeigen, bis der Importvorgang abgeschlossen ist. Abhängig von der Größe des Updates kann dieser Vorgang in wenigen Minuten abgeschlossen werden, er kann aber auch länger dauern.
   
   :::image type="content" source="media/import-update/update-publishing-sequence-2.png" alt-text="Screenshot: Updateimportsequenz." lightbox="media/import-update/update-publishing-sequence-2.png":::

9. Wenn in der Spalte „Status“ angezeigt wird, dass der Import erfolgreich war, wählen Sie den Header „Ready to Deploy“ (Bereit für die Bereitstellung) aus. Ihr importiertes Update sollte nun in der Liste angezeigt werden.

[Weitere Informationen](import-update.md) zum Importieren von Updates.

## <a name="create-update-group"></a>Erstellen der Updategruppe

1. Navigieren Sie zu dem IoT Hub, den Sie zuvor mit Ihrer Device Update-Instanz verbunden haben.

2. Wählen Sie in der linken Navigationsleiste unter „Automatic Device Management“ (Automatische Geräteverwaltung) die Option „Device Updates“ aus.

3. Wählen Sie oben auf der Seite die Registerkarte „Groups“ (Gruppen) aus. 

4. Wählen Sie die Schaltfläche „Add“ (Hinzufügen) aus, um eine neue Gruppe zu erstellen.

5. Wählen Sie das IoT Hub-Tag aus der Liste aus, das Sie im vorherigen Schritt erstellt haben. Wählen Sie „Create update group“ (Updategruppe erstellen) aus.

   :::image type="content" source="media/create-update-group/select-tag.PNG" alt-text="Screenshot: Tagauswahl." lightbox="media/create-update-group/select-tag.PNG":::

[Weitere Informationen](create-update-group.md) zum Hinzufügen von Tags und zum Erstellen von Updategruppen


## <a name="deploy-update"></a>Bereitstellen des Updates

1. Nachdem die Gruppe erstellt wurde, sollte ein neues Update für Ihre Gerätegruppe mit einem Link zum Update unter „Pending Updates“ (Ausstehende Updates) angezeigt werden. Möglicherweise müssen Sie die Anzeige ein Mal aktualisieren. 

2. Klicken Sie auf das verfügbare Update.

3. Vergewissern Sie sich, dass die richtige Gruppe als Zielgruppe ausgewählt ist. Planen Sie die Bereitstellung, und wählen Sie dann „Deploy update“ (Update bereitstellen) aus.

   :::image type="content" source="media/deploy-update/select-update.png" alt-text="Auswählen des Updates" lightbox="media/deploy-update/select-update.png":::

4. Zeigen Sie das Konformitätsdiagramm an. Es sollte angezeigt werden, dass das Update jetzt ausgeführt wird. 

   :::image type="content" source="media/deploy-update/update-in-progress.png" alt-text="Update wird ausgeführt." lightbox="media/deploy-update/update-in-progress.png":::

5. Nachdem das Gerät erfolgreich aktualisiert wurde, sollten Sie sehen, dass Ihr Konformitätsdiagramm und die Details der Bereitstellung so aktualisiert werden, dass sie übereinstimmen. 

   :::image type="content" source="media/deploy-update/update-succeeded.png" alt-text="Update erfolgreich" lightbox="media/deploy-update/update-succeeded.png":::

## <a name="monitor-an-update-deployment"></a>Überwachen einer Updatebereitstellung

1. Wählen Sie oben auf der Seite die Registerkarte „Deployments“ (Bereitstellungen) aus.

   :::image type="content" source="media/deploy-update/deployments-tab.png" alt-text="Registerkarte „Deployments“ (Bereitstellungen)" lightbox="media/deploy-update/deployments-tab.png":::

2. Wählen Sie die erstellte Bereitstellung aus, um die Bereitstellungsdetails anzuzeigen.

   :::image type="content" source="media/deploy-update/deployment-details.png" alt-text="Bereitstellungsdetails" lightbox="media/deploy-update/deployment-details.png":::

3. Wählen Sie „Refresh“ (Aktualisieren) aus, um die neuesten Statusdetails anzuzeigen. Setzen Sie diesen Prozess fort, bis der Status in „Succeeded“ (Erfolgreich) geändert wurde.

Sie haben nun ein erfolgreiches End-to-End-Imageupdate mit Device Update for IoT Hub auf einem Raspberry Pi 3 B+-Gerät abgeschlossen. 

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie es nicht mehr benötigten, bereinigen Sie Ihr Konto für das Geräteupdate sowie die IoT Hub-Instanz und das IoT-Gerät. 

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"]
> [Simulator Reference Agent](device-update-simulator.md)
