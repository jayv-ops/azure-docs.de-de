---
title: Versionshinweise zum Azure-Dateisynchronisierungs-Agent | Microsoft-Dokumentation
description: Lesen Sie die Versionshinweise zum Azure-Dateisynchronisierungs-Agent, mit dem Sie die Dateifreigaben Ihrer Organisation in Azure Files zentralisieren können.
services: storage
author: wmgries
ms.service: storage
ms.topic: conceptual
ms.date: 3/3/2021
ms.author: wgries
ms.subservice: files
ms.openlocfilehash: 5549fc3b63b76c6158ae7399e6d94a43d2d4f28f
ms.sourcegitcommit: ba676927b1a8acd7c30708144e201f63ce89021d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2021
ms.locfileid: "102435187"
---
# <a name="release-notes-for-the-azure-file-sync-agent"></a>Versionshinweise zum Azure-Dateisynchronisierungs-Agent
Mit der Azure-Dateisynchronisierung können Sie Dateifreigaben Ihrer Organisation in Azure Files zentralisieren, ohne auf die Flexibilität, Leistung und Kompatibilität eines lokalen Dateiservers verzichten zu müssen. Ihre Windows Server-Installationen werden in einen schnellen Cache Ihrer Azure-Dateifreigabe transformiert. Sie können ein beliebiges Protokoll verwenden, das unter Windows Server verfügbar ist, um lokal auf Ihre Daten zuzugreifen (z.B. SMB, NFS und FTPS). Sie können weltweit so viele Caches wie nötig nutzen.

Dieser Artikel enthält die Versionshinweise für die unterstützten Versionen des Azure-Dateisynchronisierungs-Agents.

## <a name="supported-versions"></a>Unterstützte Versionen
Die folgenden Versionen des Agents für die Azure-Dateisynchronisierung werden unterstützt:

| Meilenstein | Agent-Versionsnummer | Veröffentlichungsdatum | Status |
|----|----------------------|--------------|------------------|
| V11.2-Release: [KB4539952](https://support.microsoft.com/topic/azure-file-sync-agent-v11-2-release-february-2021-c956eaf0-cd8e-4511-98c0-e5a1f2c84048)| 11.2.0.0 | 2\. Februar 2021 | Unterstützt |
| V11.1-Release: [KB4539951](https://support.microsoft.com/help/4539951)| 11.1.0.0 | 4\. November 2020 | Unterstützt |
| V10.1-Release: [KB4522411](https://support.microsoft.com/help/4522411)| 10.1.0.0 | 5\. Juni 2020 | Unterstützt – Agent-Version läuft am 7. Juni 2021 ab |
| Mai 2020 Updaterollup – [KB4522412](https://support.microsoft.com/help/4522412)| 10.0.2.0 | 19. Mai 2020 | Unterstützt – Agent-Version läuft am 7. Juni 2021 ab |
| V10-Release: [KB4522409](https://support.microsoft.com/help/4522409)| 10.0.0.0 | 9\. April 2020 | Unterstützt – Agent-Version läuft am 7. Juni 2021 ab |

## <a name="unsupported-versions"></a>Nicht unterstützte Versionen
Die folgenden Versionen des Agents für die Azure-Dateisynchronisierung sind abgelaufen und werden nicht mehr unterstützt:

| Meilenstein | Agent-Versionsnummer | Veröffentlichungsdatum | Status |
|----|----------------------|--------------|------------------|
| V9 Release | 9.0.0.0 – 9.1.0.0 | – | Nicht unterstützt – Agent-Version ist am 16. Februar 2021 abgelaufen |
| V8-Release | 8.0.0.0 | Nicht zutreffend | Nicht unterstützt – Agent-Version ist am 12. Januar 2021 abgelaufen |
| V7 Release | 7.0.0.0 – 7.2.0.0 | – | Nicht unterstützt – Agent-Versionen sind am 1. September 2020 abgelaufen. |
| V6-Release | 6.0.0.0–6.3.0.0 | – | Nicht unterstützt – Agent-Versionen sind am 21. April 2020 abgelaufen. |
| Release V5 | 5.0.2.0–5.2.0.0 | – | Nicht unterstützt – Agent-Versionen sind am 18. März 2020 abgelaufen. |
| Release V4 | 4.0.1.0 – 4.3.0.0 | – | Nicht unterstützt – Agent-Versionen sind am 6. November 2019 abgelaufen. |
| Release V3 | 3.1.0.0 – 3.4.0.0 | – | Nicht unterstützt – Agent-Versionen sind am 19. August 2019 abgelaufen. |
| Pre-GA-Agents | 1.1.0.0 – 3.0.13.0 | – | Nicht unterstützt – Agent-Versionen sind am 1. Oktober 2018 abgelaufen. |

### <a name="azure-file-sync-agent-update-policy"></a>Updaterichtlinie für den Azure-Dateisynchronisierungs-Agent
[!INCLUDE [storage-sync-files-agent-update-policy](../../../includes/storage-sync-files-agent-update-policy.md)]

## <a name="agent-version-11200"></a>Agent-Version 11.2.0.0
Die folgenden Versionshinweise gelten für Version 11.2.0.0 des Azure-Dateisynchronisierungs-Agents, die am 2. Februar 2021 veröffentlicht wurde. Diese Hinweise gelten zusätzlich zu den Versionshinweisen, die für die Version 11.1.0.0 angegeben sind.

### <a name="improvements-and-issues-that-are-fixed"></a>Verbesserungen und behobene Probleme 
- Wenn eine Synchronisierungssitzung aufgrund einer hohen Anzahl von Fehlern pro Element abgebrochen wird, kann die Synchronisierung beim Starten einer neuen Sitzung durch einen Abgleich erfolgen, wenn der Azure-Dateisynchronisierungsdienst feststellt, dass eine benutzerdefinierte Synchronisierungssitzung erforderlich ist, um die Fehler pro Element zu korrigieren.
- Beim Registrieren eines Servers mit dem Cmdlet „Register-AzStorageSyncServer“ tritt möglicherweise ein „Ausnahmefehler“ auf.
- Neues PowerShell-Cmdlet (Add-StorageSyncAllowedServerEndpointPath) zum Konfigurieren zulässiger Serverendpunktpfade auf einem Server. Dieses Cmdlet eignet sich für Szenarien, in denen die Bereitstellung der Azure-Dateisynchronisierung von einem Cloud Solution Provider (CSP) oder Dienstanbieter verwaltet wird und der Kunde zulässige Serverendpunktpfade auf einem Server konfigurieren möchte. Wenn beim Erstellen eines Serverendpunkts der angegebene Pfad nicht in der Zulassungsliste enthalten ist, tritt beim Erstellen dieses Serverendpunkts ein Fehler auf. Beachten Sie, dass es sich hierbei um ein optionales Feature handelt und alle unterstützten Pfade beim Erstellen eines Serverendpunkts standardmäßig zulässig sind.  

    
    - Um einen zulässigen Serverendpunktpfad hinzuzufügen, führen Sie die folgenden PowerShell-Befehle auf dem Server aus:

    ```powershell
    Import-Module 'C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll' -verbose
    Add-StorageSyncAllowedServerEndpointPath -Path <path>
    ```  

    - Um die Liste der unterstützten Pfade abzurufen, führen Sie den folgenden PowerShell-Befehl aus:
    
    ```powershell
    Get-StorageSyncAllowedServerEndpointPath
    ```     
    - Zum Entfernen eines Pfads führen Sie den folgenden PowerShell-Befehl aus:
    
    ```powershell
    Remove-StorageSyncAllowedServerEndpointPath -Path <path>
    ```  
## <a name="agent-version-11100"></a>Agent-Version 11.1.0.0
Die folgenden Versionshinweise gelten für Version 11.1.0.0 des Azure-Dateisynchronisierungs-Agents (Veröffentlichung: 4. November 2020).

### <a name="improvements-and-issues-that-are-fixed"></a>Verbesserungen und behobene Probleme
- Neue Cloudtieringmodi zum Steuern des ersten Downloads und des proaktiven Abrufs
    - Ursprünglicher Downloadmodus: Sie können jetzt auswählen, wie Ihre Dateien anfänglich auf den neuen Serverendpunkt heruntergeladen werden sollen. Möchten Sie Ihre Dateien aufteilen oder so viele Dateien wie möglich gemäß dem Zeitstempel der letzten Änderung auf Ihren Server herunterladen? Das ist kein Problem. Sie können kein Cloudtiering verwenden? Nun können Sie dafür sorgen, dass es auf Ihrem System keine verteilten Dateien mehr gibt. Weitere Informationen finden Sie im Abschnitt [Erstellen eines Serverendpunkts](./storage-sync-files-deployment-guide.md?tabs=azure-portal%252cproactive-portal#create-a-server-endpoint) in der Dokumentation zum Bereitstellen der Azure-Dateisynchronisierung.
    - Proaktiver Abrufmodus: Jedes Mal, wenn eine Datei erstellt oder geändert wird, können Sie sie proaktiv auf Servern abrufen, die Sie in derselben Synchronisierungsgruppe angeben. Dadurch wird die Datei auf jedem angegebenen Server für den Verbrauch bereitgestellt. Arbeiten Teams auf der ganzen Welt an denselben Daten? Aktivieren Sie den proaktiven Abruf. Wenn ein Team in einer anderen Zeitzone Dateien aktualisiert, werden diese heruntergeladen und stehen am nächsten Morgen direkt für ein anderes Team bereit. Weitere Informationen finden Sie im Abschnitt [Proaktives Abrufen neuer und geänderter Dateien von einer Azure-Dateifreigabe](./storage-sync-files-deployment-guide.md?tabs=azure-portal%252cproactive-portal#proactively-recall-new-and-changed-files-from-an-azure-file-share) in der Dokumentation zum Bereitstellen der Azure-Dateisynchronisierung.

- Ausschließen von Anwendungen vom Cloudtiering basierend auf der Nachverfolgung der letzten Zugriffszeit. Sie können nun Anwendungen von der Nachverfolgung der letzten Zugriffszeit ausschließen. Wenn eine Anwendung auf eine Datei zugreift, wird die Uhrzeit des letzten Zugriffs auf die Datei in der Cloudtiering-Datenbank aktualisiert. Anwendungen, die das Dateisystem scannen (z. B. Virenschutz), führen dazu, dass der Zeitpunkt des letzten Zugriffs für alle Dateien gleich ist. Dies wirkt sich auf das Aufteilen von Dateien aus.

    Um Anwendungen aus der Nachverfolgung der letzten Zugriffszeit auszuschließen, fügen Sie der Registrierungseinstellung HeatTrackingProcessNameExclusionList unter „HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Azure\StorageSync“ den Prozessnamen hinzu.

    Beispiel: reg ADD "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Azure\StorageSync" /v HeatTrackingProcessNameExclusionList /t REG_MULTI_SZ /d "SampleApp.exe\0AnotherApp.exe" /f

    > [!Note]  
    > Datendeduplizierungs- und FSRM-Prozesse (File Server Resource Manager) werden standardmäßig ausgeschlossen (hartcodiert), und die Prozessausschlussliste wird alle 5 Minuten aktualisiert.

- Sonstige Verbesserungen bei Leistung und Zuverlässigkeit
    - Die Leistung bei der Änderungserkennung zum Erkennen von in der Azure-Dateifreigabe geänderten Dateien wurde verbessert.
    - Die Uploadleistung bei der Synchronisierung wurde verbessert.
    - Der erste Upload wird jetzt von einer VSS-Momentaufnahme ausgeführt. Dadurch werden die Fehler pro Element und Synchronisierungssitzungsfehler verringert.
    - Die Zuverlässigkeit der Synchronisierung für bestimmte E/A-Muster wurde verbessert.
    - Es wurde ein Fehler behoben, der verhinderte, dass die Synchronisierungsdatenbank bei einem Failover in Failoverclustern in der Zeit zurückläuft.
    - Die Rückrufleistung beim Zugriff auf verteilte Dateien wurde verbessert.

### <a name="evaluation-tool"></a>Auswertungstool
Vor der Bereitstellung der Azure-Dateisynchronisierung müssen Sie mit dem Auswertungstool für die Azure-Dateisynchronisierung auswerten, ob Kompatibilität mit Ihrem System gegeben ist. Dieses Tool ist ein Azure PowerShell-Cmdlet, das auf potenzielle Probleme mit Ihrem Dateisystem und Dataset prüft, z.B. nicht unterstützte Zeichen oder eine nicht unterstützte Betriebssystemversion. Anweisungen zur Installation und Verwendung finden Sie im Planungshandbuch im Abschnitt [Auswertungstools](./storage-sync-files-planning.md#evaluation-cmdlet). 

### <a name="agent-installation-and-server-configuration"></a>Agent-Installation und Serverkonfiguration
Weitere Informationen zum Installieren und Konfigurieren des Azure File Sync-Agents mit Windows Server finden Sie unter [Planen einer Bereitstellung der Azure-Dateisynchronisierung (Vorschau)](storage-sync-files-planning.md) sowie unter [Bereitstellen von Azure File Sync (Vorschau)](storage-sync-files-deployment-guide.md).

- Das Agent-Installationspaket muss mit erhöhten Berechtigungen (Administratorberechtigungen) installiert werden.
- Der Agent wird für die Bereitstellungsoption „Nano Server“ nicht unterstützt.
- Der Agent wird nur unter Windows Server 2019, Windows Server 2016 und Windows Server 2012 R2 unterstützt.
- Der Agent benötigt mindestens 2 GiB Arbeitsspeicher. Wenn der Server auf einem virtuellen Computer ausgeführt wird, für den dynamischer Arbeitsspeicher aktiviert ist, muss der virtuelle Computer mit mindestens 2.048 MiB Arbeitsspeicher konfiguriert werden. Weitere Informationen finden Sie unter [Empfohlenen Systemressourcen](./storage-sync-files-planning.md#recommended-system-resources).
- Der Dienst „Storage-Synchronisierungs-Agent“ (FileSyncSvc) unterstützt keine Serverendpunkte, die sich auf einem Volume befinden, für das das Verzeichnis „System Volume Information“ (SVI) komprimiert ist. Diese Konfiguration führt zu unerwarteten Ergebnissen.

### <a name="interoperability"></a>Interoperabilität
- Virenschutz, Sicherung und andere Anwendungen, die auf Tieringdateien zugreifen, können zu unerwünschten Rückrufen führen, wenn sie das Offlineattribut nicht berücksichtigen und das Lesen des Inhalts dieser Dateien nicht überspringen. Weitere Informationen finden Sie unter [Problembehandlung bei der Azure-Dateisynchronisierung (Vorschau)](storage-sync-files-troubleshoot.md).
- FSRM-Dateiprüfungen (File Server Resource Manager, Ressourcen-Manager für Dateiserver) können zu Fehlern aufgrund einer endlosen Synchronisierung führen, wenn Dateien aufgrund der damit verbundenen Vorgänge blockiert werden.
- Die Ausführung von Sysprep auf einem Server, auf dem der Azure-Dateisynchronisierungs-Agent installiert ist, wird nicht unterstützt und kann zu unerwarteten Ergebnissen führen. Der Azure-Dateisynchronisierungs-Agent sollte installiert werden, nachdem das Serverimage bereitgestellt und das Sysprep-Mini-Setup abgeschlossen wurde.

### <a name="sync-limitations"></a>Einschränkungen bei der Synchronisierung
Folgende Elemente werden nicht synchronisiert, aber der restliche Systembetrieb ist nicht beeinträchtigt:
- Dateien mit nicht unterstützten Zeichen. Eine Liste mit den Zeichen, die nicht unterstützt werden, finden Sie im [Leitfaden zur Problembehandlung](storage-sync-files-troubleshoot.md#handling-unsupported-characters).
- Dateien oder Verzeichnisse, die mit einem Punkt enden.
- Pfade, die länger als 2.048 Zeichen sind.
- Der SACL-Teil (System-Zugriffssteuerungsliste) einer Sicherheitsbeschreibung, die für die Überwachung verwendet wird.
- Erweiterte Attribute
- Alternative Datenströme
- Analysepunkte
- Feste Links
- Die Komprimierung (sofern für eine Serverdatei festgelegt) wird nicht beibehalten, wenn Änderungen mit der Datei von anderen Endpunkten synchronisiert werden.
- Mit EFS (oder einer anderen Benutzermodusverschlüsselung) verschlüsselte Dateien, die den Dienst am Lesen der Daten hindern.

    > [!Note]  
    > Bei der Azure-Dateisynchronisierung werden Daten während der Übertragung immer verschlüsselt. Ruhende Daten werden in Azure immer verschlüsselt.
 
### <a name="server-endpoint"></a>Serverendpunkt
- Ein Serverendpunkt kann nur auf einem NTFS-Volume erstellt werden. ReFS, FAT, FAT32 und andere Dateisysteme werden von der Azure-Dateisynchronisierung derzeit nicht unterstützt.
- Das Cloudtiering wird auf dem Systemvolume nicht unterstützt. Um einen Serverendpunkt auf dem Systemvolume zu erstellen, deaktivieren Sie Cloudtiering, wenn Sie den Serverendpunkt erstellen.
- Failoverclustering wird nur mit Clusterdatenträgern, aber nicht mit freigegebenen Clustervolumes (Cluster Shared Volumes, CSVs) unterstützt.
- Ein Serverendpunkt kann nicht geschachtelt werden. Er kann auf demselben Volume parallel zu einem anderen Endpunkt vorhanden sein (Koexistenz).
- Speichern Sie keine Betriebssystem- oder Anwendungsauslagerungsdatei an einem Serverendpunkt-Standort.
- Der Servername im Portal wird bei Umbenennung des Servers nicht aktualisiert.

### <a name="cloud-endpoint"></a>Cloudendpunkt
- Die Azure-Dateisynchronisierung unterstützt direkte Änderungen an der Azure-Dateifreigabe. Allerdings müssen alle Änderungen, die Sie an der Azure-Dateifreigabe vornehmen, zuerst von einem Azure-Dateisynchronisierungsauftrag zum Erkennen von Änderungen erkannt werden. Ein Auftrag zum Erkennen von Änderungen für einen Cloudendpunkt wird einmal alle 24 Stunden gestartet. Um Dateien, die in der Azure-Dateifreigabe geändert wurden, sofort zu synchronisieren, kann das PowerShell-Cmdlet [Invoke-AzStorageSyncChangeDetection](/powershell/module/az.storagesync/invoke-azstoragesyncchangedetection) verwendet werden, um die Erkennung von Änderungen in der Azure-Dateifreigabe manuell auszulösen. Darüber hinaus bewirken Änderungen, die über das REST-Protokoll an einer Azure-Dateifreigabe vorgenommen wurden, keine Aktualisierung der letzten SMB-Änderungszeit, und die Änderungen sind für eine Synchronisierung nicht zu sehen.
- Der Speichersynchronisierungsdienst und/oder das Speicherkonto kann in eine andere Ressourcengruppe, ein anderes Abonnement oder einen anderen Azure AD-Mandanten verschoben werden. Wenn der Speichersynchronisierungsdienst oder das Speicherkonto verschoben wurde, müssen Sie der Anwendung „Microsoft.StorageSync“ Zugriff auf das Speicherkonto gewähren (siehe [Stellen Sie sicher, dass die Azure-Dateisynchronisierung über Zugriff auf das Speicherkonto verfügt.](./storage-sync-files-troubleshoot.md?tabs=portal1%252cportal#troubleshoot-rbac)).

    > [!Note]  
    > Beim Erstellen des Cloudendpunkts müssen sich der Speichersynchronisierungsdienst und das Speicherkonto im selben Azure AD-Mandanten befinden. Nach der Erstellung des Cloudendpunkts können der Speichersynchronisierungsdienst und das Speicherkonto in verschiedene Azure AD Mandanten verschoben werden.

### <a name="cloud-tiering"></a>Cloudtiering
- Wenn eine Tieringdatei mit Robocopy an einen anderen Speicherort kopiert wird, ist die sich ergebende Datei keine Tieringdatei. Das Offlineattribut kann festgelegt werden, da dieses Attribut fälschlicherweise von Robocopy in Kopiervorgänge eingefügt wird.
- Verwenden Sie beim Kopieren der Dateien mit Robocopy die Option „/MIR“, um Dateizeitstempel beizubehalten. So wird sichergestellt, dass das Tiering für ältere Dateien früher als für die Dateien durchgeführt wird, auf die zuletzt zugegriffen wurde.
    > [!Warning]  
    > Der Robocopy /B-Switch wird bei der Azure-Dateisynchronisierung nicht unterstützt. Die Verwendung des Robocopy /B-Switches bei einem Azure-Dateisynchronisierungsserver-Endpunkt als Quelle kann zur Dateibeschädigung führen.

## <a name="agent-version-10100"></a>Agent-Version 10.1.0.0
Die folgenden Versionshinweise gelten für Version 10.1.0.0 des Azure-Dateisynchronisierungs-Agents, die am 5. Juni 2020 veröffentlicht wurde. Sie gelten zusätzlich zu den für die Versionen 10.0.0.0 und 10.0.2.0 angegeben Versionshinweisen.

### <a name="improvements-and-issues-that-are-fixed"></a>Verbesserungen und behobene Probleme

- Unterstützung für private Azure-Endpunkte
    - Synchronisierungsdatenverkehr zum Speichersynchronisierungsdienst kann jetzt an einen privaten Endpunkt gesendet werden. Dies ermöglicht ein Tunneln über eine ExpressRoute- oder VPN-Verbindung. Weitere Informationen finden Sie unter [Konfigurieren von Netzwerkendpunkten für die Azure-Dateisynchronisierung](./storage-sync-files-networking-endpoints.md).
- Die Metrik „Dateien synchronisiert“ zeigt nun bei einer großen Synchronisierung den Fortschritt während der Ausführung an statt am Ende.
- Verschiedene Verbesserungen in Bezug auf die Zuverlässigkeit bei der Agent-Installation, beim Cloudtiering, bei der Synchronisierung und bei der Telemetrie.

## <a name="agent-version-10020"></a>Agent-Version 10.0.2.0
Die folgenden Versionshinweise gelten für Version 10.0.2.0 des Azure-Dateisynchronisierungs-Agents, die am 19. Mai 2020 veröffentlicht wurde. Diese Hinweise gelten zusätzlich zu den Versionshinweisen, die für die Version 10.0.0.0 angegeben sind.

In dieser Version behobenes Problem:  
- Der Storage-Synchronisierungs-Agent (FileSyncSvc) stürzt nach der Installation des Azure-Dateisynchronisierung-Agents, Version 10, häufig ab.

> [!Note]  
>Für diese Version wurde kein Test-Flight für Server durchgeführt, die automatisch aktualisiert werden, wenn eine neue Version verfügbar ist. Verwenden Sie zum Installieren dieses Updates Microsoft Update oder den Microsoft Update-Katalog (Installationsanweisungen finden Sie unter [KB4522412](https://support.microsoft.com/help/4522412)).

## <a name="agent-version-10000"></a>Agent-Version 10.0.0.0
Die folgenden Versionshinweise gelten für Version 10.0.0.0 des Azure-Dateisynchronisierungs-Agents, die am 9. April 2020 veröffentlicht wurde.

### <a name="improvements-and-issues-that-are-fixed"></a>Verbesserungen und behobene Probleme

- Verbesserter Synchronisierungsfortschritt im Portal
    - Bei Verwendung der Agent-Version V10 wird in Kürze der Typ der aktuell ausgeführten Synchronisierungssitzung im Azure-Portal angezeigt. Beispiel: erster Download, regulärer Download, Hintergrundrückruf (Fälle von schnellen Notfallwiederherstellungen) und ähnliches 

- Verbesserte Benutzeroberfläche des Portals für Cloudtiering
    - Für Dateien mit Tiering- oder Abruffehlern werden jetzt die Tieringfehler in den Eigenschaften des Serverendpunkts angezeigt.
    - Für Serverendpunkte stehen jetzt zusätzliche Cloudtieringinformationen bereit:
        - Größe des lokalen Caches
        - Effizienz der Cachenutzung
        - Details zur Cloudtieringrichtlinie: Volumegröße, aktueller freier Speicherplatz oder der Zeitpunkt des letzten Zugriffs auf die älteste Datei im lokalen Cache.
    - Diese Änderungen werden im Azure-Portal unmittelbar nach dem V10-Agent-Erstrelease bereitgestellt.

- Unterstützung für das Verschieben des Speichersynchronisierungsdiensts und/oder Speicherkontos in einen anderen Azure Active Directory-Mandanten
    - Die Azure-Dateisynchronisierung unterstützt jetzt das Verschieben des Speichersynchronisierungsdiensts und/oder Speicherkontos in eine andere Ressourcengruppe, in ein anderes Abonnement oder in einen anderen Azure AD Mandanten.
    
- Sonstige Verbesserungen bei Leistung und Zuverlässigkeit
    - Die Änderungserkennung für die Azure-Dateifreigabe schlägt möglicherweise fehl, wenn ein virtuelles Netzwerk (VNET) und Firewallregeln für das Speicherkonto konfiguriert sind.
    - Geringerer Arbeitsspeicherverbrauch im Zusammenhang mit Rückrufen 
    - Verbesserte Leistung bei Verwendung des [Invoke-AzStorageSyncChangeDetection](/powershell/module/az.storagesync/invoke-azstoragesyncchangedetection)-Cmdlets
    - Verschiedene Verbesserungen im Hinblick auf die Zuverlässigkeit der Synchronisierung 
    
### <a name="evaluation-tool"></a>Auswertungstool
Vor der Bereitstellung der Azure-Dateisynchronisierung müssen Sie mit dem Auswertungstool für die Azure-Dateisynchronisierung auswerten, ob Kompatibilität mit Ihrem System gegeben ist. Dieses Tool ist ein Azure PowerShell-Cmdlet, das auf potenzielle Probleme mit Ihrem Dateisystem und Dataset prüft, z.B. nicht unterstützte Zeichen oder eine nicht unterstützte Betriebssystemversion. Anweisungen zur Installation und Verwendung finden Sie im Planungshandbuch im Abschnitt [Auswertungstools](./storage-sync-files-planning.md#evaluation-cmdlet). 

### <a name="agent-installation-and-server-configuration"></a>Agent-Installation und Serverkonfiguration
Weitere Informationen zum Installieren und Konfigurieren des Azure File Sync-Agents mit Windows Server finden Sie unter [Planen einer Bereitstellung der Azure-Dateisynchronisierung (Vorschau)](storage-sync-files-planning.md) sowie unter [Bereitstellen von Azure File Sync (Vorschau)](storage-sync-files-deployment-guide.md).

- Das Agent-Installationspaket muss mit erhöhten Berechtigungen (Administratorberechtigungen) installiert werden.
- Der Agent wird für die Bereitstellungsoption „Nano Server“ nicht unterstützt.
- Der Agent wird nur unter Windows Server 2019, Windows Server 2016 und Windows Server 2012 R2 unterstützt.
- Der Agent benötigt mindestens 2 GiB Arbeitsspeicher. Wenn der Server auf einem virtuellen Computer ausgeführt wird, für den dynamischer Arbeitsspeicher aktiviert ist, muss der virtuelle Computer mit mindestens 2.048 MiB Arbeitsspeicher konfiguriert werden.
- Der Dienst „Storage-Synchronisierungs-Agent“ (FileSyncSvc) unterstützt keine Serverendpunkte, die sich auf einem Volume befinden, für das das Verzeichnis „System Volume Information“ (SVI) komprimiert ist. Diese Konfiguration führt zu unerwarteten Ergebnissen.

### <a name="interoperability"></a>Interoperabilität
- Virenschutz, Sicherung und andere Anwendungen, die auf Tieringdateien zugreifen, können zu unerwünschten Rückrufen führen, wenn sie das Offlineattribut nicht berücksichtigen und das Lesen des Inhalts dieser Dateien nicht überspringen. Weitere Informationen finden Sie unter [Problembehandlung bei der Azure-Dateisynchronisierung (Vorschau)](storage-sync-files-troubleshoot.md).
- FSRM-Dateiprüfungen (File Server Resource Manager, Ressourcen-Manager für Dateiserver) können zu Fehlern aufgrund einer endlosen Synchronisierung führen, wenn Dateien aufgrund der damit verbundenen Vorgänge blockiert werden.
- Die Ausführung von Sysprep auf einem Server, auf dem der Azure-Dateisynchronisierungs-Agent installiert ist, wird nicht unterstützt und kann zu unerwarteten Ergebnissen führen. Der Azure-Dateisynchronisierungs-Agent sollte installiert werden, nachdem das Serverimage bereitgestellt und das Sysprep-Mini-Setup abgeschlossen wurde.

### <a name="sync-limitations"></a>Einschränkungen bei der Synchronisierung
Folgende Elemente werden nicht synchronisiert, aber der restliche Systembetrieb ist nicht beeinträchtigt:
- Dateien mit nicht unterstützten Zeichen. Eine Liste mit den Zeichen, die nicht unterstützt werden, finden Sie im [Leitfaden zur Problembehandlung](storage-sync-files-troubleshoot.md#handling-unsupported-characters).
- Dateien oder Verzeichnisse, die mit einem Punkt enden.
- Pfade, die länger als 2.048 Zeichen sind.
- Der SACL-Teil (System-Zugriffssteuerungsliste) einer Sicherheitsbeschreibung, die für die Überwachung verwendet wird.
- Erweiterte Attribute
- Alternative Datenströme
- Analysepunkte
- Feste Links
- Die Komprimierung (sofern für eine Serverdatei festgelegt) wird nicht beibehalten, wenn Änderungen mit der Datei von anderen Endpunkten synchronisiert werden.
- Mit EFS (oder einer anderen Benutzermodusverschlüsselung) verschlüsselte Dateien, die den Dienst am Lesen der Daten hindern.

    > [!Note]  
    > Bei der Azure-Dateisynchronisierung werden Daten während der Übertragung immer verschlüsselt. Ruhende Daten werden in Azure immer verschlüsselt.
 
### <a name="server-endpoint"></a>Serverendpunkt
- Ein Serverendpunkt kann nur auf einem NTFS-Volume erstellt werden. ReFS, FAT, FAT32 und andere Dateisysteme werden von der Azure-Dateisynchronisierung derzeit nicht unterstützt.
- Das Cloudtiering wird auf dem Systemvolume nicht unterstützt. Um einen Serverendpunkt auf dem Systemvolume zu erstellen, deaktivieren Sie Cloudtiering, wenn Sie den Serverendpunkt erstellen.
- Failoverclustering wird nur mit Clusterdatenträgern, aber nicht mit freigegebenen Clustervolumes (Cluster Shared Volumes, CSVs) unterstützt.
- Ein Serverendpunkt kann nicht geschachtelt werden. Er kann auf demselben Volume parallel zu einem anderen Endpunkt vorhanden sein (Koexistenz).
- Speichern Sie keine Betriebssystem- oder Anwendungsauslagerungsdatei an einem Serverendpunkt-Standort.
- Der Servername im Portal wird bei Umbenennung des Servers nicht aktualisiert.

### <a name="cloud-endpoint"></a>Cloudendpunkt
- Die Azure-Dateisynchronisierung unterstützt direkte Änderungen an der Azure-Dateifreigabe. Allerdings müssen alle Änderungen, die Sie an der Azure-Dateifreigabe vornehmen, zuerst von einem Azure-Dateisynchronisierungsauftrag zum Erkennen von Änderungen erkannt werden. Ein Auftrag zum Erkennen von Änderungen für einen Cloudendpunkt wird einmal alle 24 Stunden gestartet. Um Dateien, die in der Azure-Dateifreigabe geändert wurden, sofort zu synchronisieren, kann das PowerShell-Cmdlet [Invoke-AzStorageSyncChangeDetection](/powershell/module/az.storagesync/invoke-azstoragesyncchangedetection) verwendet werden, um die Erkennung von Änderungen in der Azure-Dateifreigabe manuell auszulösen. Darüber hinaus bewirken Änderungen, die über das REST-Protokoll an einer Azure-Dateifreigabe vorgenommen wurden, keine Aktualisierung der letzten SMB-Änderungszeit, und die Änderungen sind für eine Synchronisierung nicht zu sehen.
- Der Speichersynchronisierungsdienst und/oder das Speicherkonto kann in eine andere Ressourcengruppe, ein anderes Abonnement oder einen anderen Azure AD-Mandanten verschoben werden. Wenn der Speichersynchronisierungsdienst oder das Speicherkonto verschoben wurde, müssen Sie der Anwendung „Microsoft.StorageSync“ Zugriff auf das Speicherkonto gewähren (siehe [Stellen Sie sicher, dass die Azure-Dateisynchronisierung über Zugriff auf das Speicherkonto verfügt.](./storage-sync-files-troubleshoot.md?tabs=portal1%252cportal#troubleshoot-rbac)).

    > [!Note]  
    > Beim Erstellen des Cloudendpunkts müssen sich der Speichersynchronisierungsdienst und das Speicherkonto im selben Azure AD-Mandanten befinden. Nach der Erstellung des Cloudendpunkts können der Speichersynchronisierungsdienst und das Speicherkonto in verschiedene Azure AD Mandanten verschoben werden.

### <a name="cloud-tiering"></a>Cloudtiering
- Wenn eine Tieringdatei mit Robocopy an einen anderen Speicherort kopiert wird, ist die sich ergebende Datei keine Tieringdatei. Das Offlineattribut kann festgelegt werden, da dieses Attribut fälschlicherweise von Robocopy in Kopiervorgänge eingefügt wird.
- Verwenden Sie beim Kopieren der Dateien mit Robocopy die Option „/MIR“, um Dateizeitstempel beizubehalten. So wird sichergestellt, dass das Tiering für ältere Dateien früher als für die Dateien durchgeführt wird, auf die zuletzt zugegriffen wurde.
