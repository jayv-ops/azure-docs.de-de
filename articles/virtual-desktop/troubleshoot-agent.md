---
title: 'Problembehandlung für den Windows Virtual Desktop-Agent: Azure'
description: Hier finden Sie Informationen dazu, wie Sie häufige Agent- und Konnektivitätsprobleme beheben.
author: Sefriend
ms.topic: troubleshooting
ms.date: 12/16/2020
ms.author: sefriend
manager: clarkn
ms.openlocfilehash: 1500a635d5177ed8899cdc3f1364e57a8525892c
ms.sourcegitcommit: 24f30b1e8bb797e1609b1c8300871d2391a59ac2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2021
ms.locfileid: "100099947"
---
# <a name="troubleshoot-common-windows-virtual-desktop-agent-issues"></a>Behandeln häufiger Probleme beim Windows Virtual Desktop-Agent

Der Windows Virtual Desktop-Agent kann aufgrund verschiedener Faktoren Verbindungsprobleme verursachen:
   - Ein Fehler im Broker, der dazu führt, dass der Agent den Dienst beendet.
   - Probleme bei Updates.
   - Probleme während der Agent-Installation, wodurch die Verbindung mit dem Sitzungshost unterbrochen wird.

In diesem Artikel werden Lösungen für diese häufigen Szenarien vorgestellt und erläutert, wie Sie Verbindungsprobleme beheben können.

## <a name="error-the-rdagentbootloader-andor-remote-desktop-agent-loader-has-stopped-running"></a>Error: Die Ausführung von RDAgentBootLoader und/oder Remote Desktop Agent Loader wurde beendet

Wenn Sie eins der folgenden Probleme feststellen, bedeutet dies, dass der Bootloader, der den Agent lädt, den Agent nicht ordnungsgemäß installieren konnte und der Agent-Dienst nicht ausgeführt wird:
- **RDAgentBootLoader** wurde beendet oder wird nicht ausgeführt.
- Für **Remote Desktop Agent Loader** ist kein Status vorhanden.

Zum Beheben dieses Problems starten Sie den RDAgent-Bootloader:

1. Klicken Sie im Fenster „Dienste“ mit der rechten Maustaste auf **Remote Desktop Agent Loader**.
2. Wählen Sie **Starten** aus. Wenn die Option abgeblendet ist, verfügen Sie nicht über Administratorberechtigungen. Diese benötigen Sie, um den Dienst zu starten.
3. Warten Sie 10 Sekunden, und klicken Sie mit der rechten Maustaste auf **Remote Desktop Agent Loader**.
4. Klicken Sie auf **Aktualisieren**.
5. Wenn der Dienst beendet wird, nachdem Sie ihn gestartet und aktualisiert haben, liegt möglicherweise ein Registrierungsfehler vor. Weitere Informationen finden Sie unter [INVALID_REGISTRATION_TOKEN](#error-invalid_registration_token).

## <a name="error-invalid_registration_token"></a>Error: INVALID_REGISTRATION_TOKEN

Wechseln Sie zu **Ereignisanzeige** > **Windows-Protokolle** > **Anwendung**. Wenn ein Ereignis mit der ID 3277 und der Beschreibung **INVALID_REGISTRATION_TOKEN** angezeigt wird, wird Ihr Registrierungstoken nicht als gültig erkannt.

Um dieses Problem zu lösen, erstellen Sie ein gültiges Registrierungstoken:

1. Führen Sie zum Erstellen eines neuen Registrierungstokens die Schritte im Abschnitt [Generieren eines neuen Registrierungsschlüssels für die VM](#step-3-generate-a-new-registration-key-for-the-vm) aus.
2. Öffnen Sie den Registrierungs-Editor. 
3. Wechseln Sie zu **HKEY_LOCAL_MACHINE** > **SOFTWARE** > **Microsoft** > **RDInfraAgent**.
4. Wählen Sie **IsRegistered** aus. 
5. Geben Sie im Feld **Wertdaten:** den Wert **0** ein, und klicken Sie dann auf **OK**. 
6. Wählen Sie **RegistrationToken** aus. 
7. Fügen Sie im Feld **Wertdaten** das Registrierungstoken aus Schritt 1 ein. 

   > [!div class="mx-imgBorder"]
   > ![Screenshot von IsRegistered mit dem Wert 0](media/isregistered-token.png)

8. Öffnen Sie eine Eingabeaufforderung als Administrator.
9. Geben Sie **net stop RDAgentBootLoader** ein. 
10. Geben Sie **net start RDAgentBootLoader** ein. 
11. Öffnen Sie den Registrierungs-Editor.
12. Wechseln Sie zu **HKEY_LOCAL_MACHINE** > **SOFTWARE** > **Microsoft** > **RDInfraAgent**.
13. Überprüfen Sie, ob **IsRegistered** auf 1 festgelegt ist und die Datenspalte für **RegistrationToken** keine Werte enthält. 

   > [!div class="mx-imgBorder"]
   > ![Screenshot von IsRegistered mit dem Wert 1](media/isregistered-registry.png)

## <a name="error-agent-cannot-connect-to-broker-with-invalid_form-or-not_found-url"></a>Error: Agent kann keine Verbindung mit Broker herstellen: INVALID_FORM- oder NOT_FOUND- URL

Wechseln Sie zu **Ereignisanzeige** > **Windows-Protokolle** > **Anwendung**. Wenn Sie ein Ereignis mit der ID 3277 und der Beschreibung **INVALID_FORM** oder **NOT_FOUND- URL** feststellen, ist in der Kommunikation zwischen Agent und Broker etwas schiefgegangen. Der Agent kann keine Verbindung mit dem Broker herstellen und eine bestimmte URL nicht erreichen. Dies kann an den Firewall- oder DNS-Einstellungen liegen.

Um dieses Problem zu lösen, überprüfen Sie, ob BrokerURI und BrokerURIGlobal erreichbar sind:
1. Öffnen Sie den Registrierungs-Editor. 
2. Wechseln Sie zu **HKEY_LOCAL_MACHINE** > **SOFTWARE** > **Microsoft** > **RDInfraAgent**. 
3. Notieren Sie sich die Werte für **BrokerURI** und **BrokerURIGlobal**.

   > [!div class="mx-imgBorder"]
   > ![Screenshot von BrokerURI und BrokerURIGlobal](media/broker-uri.png)

 
4. Öffnen Sie einen Browser, und navigieren Sie zu *\<BrokerURI\>api/health*. 
   - Stellen Sie sicher, dass Sie im **BrokerURI** den Wert aus Schritt 3 verwenden. Im Beispiel dieses Abschnitts wäre es <https://rdbroker-g-us-r0.wvd.microsoft.com/api/health>.
5. Öffnen Sie eine weitere Registerkarte im Browser, und navigieren Sie zu *\<BrokerURIGlobal\>api/health*. 
   - Stellen Sie sicher, dass Sie im Link **BrokerURIGlobal** den Wert aus Schritt 3 verwenden. Im Beispiel dieses Abschnitts wäre es <https://rdbroker.wvd.microsoft.com/api/health>.
6. Wenn das Netzwerk die Brokerverbindung nicht blockiert, werden beide Seiten erfolgreich geladen. In der Meldung **"RD Broker is Healthy"** wird mitgeteilt, dass der RD-Broker fehlerfrei ist, wie in den folgenden Screenshots gezeigt. 

   > [!div class="mx-imgBorder"]
   > ![Screenshot: erfolgreicher Zugriff auf den geladenen Broker-URI](media/broker-uri-web.png)

   > [!div class="mx-imgBorder"]
   > ![Screenshot: erfolgreicher Zugriff auf den geladenen globalen Broker-URI](media/broker-global.png)
 

7. Wenn das Netzwerk eine Brokerverbindung blockiert, werden die Seiten nicht geladen, wie im folgenden Screenshot gezeigt. 

   > [!div class="mx-imgBorder"]
   > ![Screenshot: nicht erfolgreicher Zugriff auf den geladenen Broker-URI](media/unsuccessful-broker-uri.png)

   > [!div class="mx-imgBorder"]
   > ![Screenshot: nicht erfolgreicher Zugriff auf den geladenen globalen Broker-URI](media/unsuccessful-broker-global.png)

8. Wenn das Netzwerk diese URLs blockiert, müssen Sie diese Blockierung für die erforderlichen URLs aufheben. Weitere Informationen finden Sie unter [Liste der erforderlichen URLs](safe-url-list.md).
9. Wenn Ihr Problem damit nicht gelöst wird, stellen Sie sicher, dass keine Gruppenrichtlinien mit Verschlüsselungsverfahren vorhanden sind, die die Verbindung zwischen Agent und Broker blockieren. Windows Virtual Desktop verwendet dasselbe TLS 1.2-Verschlüsselungsverfahren wie [Azure Front Door](../frontdoor/front-door-faq.MD#what-are-the-current-cipher-suites-supported-by-azure-front-door). Weitere Informationen finden Sie unter [Verbindungssicherheit](network-connectivity.md#connection-security).

## <a name="error-3703-or-3019"></a>Error: 3703 oder 3019

Wechseln Sie zu **Ereignisanzeige** > **Windows-Protokolle** > **Anwendung**. Wenn Sie ein Ereignis mit der ID 3703 und der Beschreibung **RD-Gateway-URL nicht zugänglich** oder ein Ereignis mit der ID 3019 in der Beschreibung feststellen, kann der Agent die Gateway-URLs oder die Websockettransport-URLs nicht erreichen. Um erfolgreich eine Verbindung mit Ihrem Sitzungshost herzustellen und zuzulassen, dass Netzwerkdatenverkehr zu diesen Endpunkten Einschränkungen umgeht, müssen Sie die Blockierung der URLs in der [Liste der erforderlichen URLs](safe-url-list.md) aufheben. Stellen Sie auch sicher, dass Ihre Firewall- oder Proxyeinstellungen diese URLs nicht blockieren. Damit Windows Virtual Desktop verwendet werden kann, muss die Blockierung dieser URLs aufgehoben werden.

Um dieses Problem zu lösen, vergewissern Sie sich, dass Ihre Firewall- und/oder DNS-Einstellungen diese URLs nicht blockieren:
1. [Verwenden Sie Azure Firewall zum Schutz von Windows Virtual Desktop-Bereitstellungen](../firewall/protect-windows-virtual-desktop.md).
2. Konfigurieren Sie die [DNS-Einstellungen für Azure Firewall](../firewall/dns-settings.md).

## <a name="error-installmsiexception"></a>Error: InstallMsiException

Wechseln Sie zu **Ereignisanzeige** > **Windows-Protokolle** > **Anwendung**. Wenn Sie ein Ereignis mit der ID 3277 und der Beschreibung **InstallMsiException** feststellen, wird das Installationsprogramm bereits für eine andere Anwendung ausgeführt, während Sie versuchen, den Agent zu installieren. Es ist auch möglich, dass eine Richtlinie die Ausführung des Programms „msiexec.exe“ blockiert.

Um dieses Problem zu lösen, deaktivieren Sie die folgende Richtlinie:
   - Windows Installer deaktivieren  
      - Kategoriepfad: Computerkonfiguration\Administrative Vorlagen\Windows-Komponenten\Windows Installer
   
>[!NOTE]
>Dies ist keine vollständige Liste aller Richtlinien. Hier werden nur diejenigen aufgeführt, die uns derzeit bekannt sind.

So deaktivieren Sie eine Richtlinie:
1. Öffnen Sie eine Eingabeaufforderung als Administrator.
2. Geben Sie **rsop.msc** ein, und führen Sie das Programm aus.
3. Wechseln Sie im Fenster **Richtlinienergebnissatz** zum hier angegebenen Kategoriepfad.
4. Wählen Sie die Richtlinie aus.
5. Wählen Sie **Deaktiviert** aus.
6. Wählen Sie **Übernehmen**.   

   > [!div class="mx-imgBorder"]
   > ![Screenshot der Windows Installer-Richtlinie im Richtlinienergebnissatz](media/gpo-policy.png)

## <a name="error-win32exception"></a>Error: Win32Exception

Wechseln Sie zu **Ereignisanzeige** > **Windows-Protokolle** > **Anwendung**. Wenn Sie ein Ereignis mit der ID 3277 und der Beschreibung **InstallMsiException** feststellen, blockiert eine Richtlinie den Start von „cmd.exe“. Wenn dieses Programm blockiert ist, können Sie das Konsolenfenster nicht ausführen, das Sie benötigen, um den Dienst nach einem Agent-Update neu zu starten.

Um dieses Problem zu lösen, deaktivieren Sie die folgende Richtlinie:
   - Zugriff auf Eingabeaufforderung verhindern   
      - Kategoriepfad: Benutzerkonfiguration\Administrative Vorlagen\System
    
>[!NOTE]
>Dies ist keine vollständige Liste aller Richtlinien. Hier werden nur diejenigen aufgeführt, die uns derzeit bekannt sind.

So deaktivieren Sie eine Richtlinie:
1. Öffnen Sie eine Eingabeaufforderung als Administrator.
2. Geben Sie **rsop.msc** ein, und führen Sie das Programm aus.
3. Wechseln Sie im Fenster **Richtlinienergebnissatz** zum hier angegebenen Kategoriepfad.
4. Wählen Sie die Richtlinie aus.
5. Wählen Sie **Deaktiviert** aus.
6. Wählen Sie **Übernehmen**.   

## <a name="error-stack-listener-isnt-working-on-windows-10-2004-vm"></a>Error: Der Stapellistener funktioniert auf Windows 10-2004-VMs nicht

Führen Sie **qwinsta** an der Eingabeaufforderung aus, und notieren Sie sich die Versionsnummer, die neben **rdp-sxs** angezeigt wird. Wenn neben den Komponenten **rdp-tcp** und **rdp-sxs** nicht **Lauschen** angezeigt wird oder die beiden Komponenten nach der Ausführung von **qwinsta** überhaupt nicht angezeigt werden, liegt ein Stapelfehler vor. Stapelupdates werden zusammen mit Agent-Updates installiert, und wenn bei dieser Installation etwas schiefgeht, funktioniert der Windows Virtual Desktop-Listener nicht.

So lösen Sie das Problem:
1. Öffnen Sie den Registrierungs-Editor.
2. Wechseln Sie zu **HKEY_LOCAL_MACHINE** > **SYSTEM** > **CurrentControlSet** > **Control** > **Terminal Server** > **WinStations**.
3. Unter **WinStations** werden möglicherweise mehrere Ordner für verschiedene Stapelversionen angezeigt. Wählen Sie den Ordner aus, der mit den Versionsinformationen übereinstimmt, die nach der Ausführung von **qwinsta** an der Eingabeaufforderung angezeigt wurden.
4. Suchen Sie **fReverseConnectMode**, und stellen Sie sicher, dass der Datenwert **1** lautet. Stellen Sie auch sicher, dass **fEnableWinStation** auf **1** festgelegt ist.

   > [!div class="mx-imgBorder"]
   > ![Screenshot von fReverseConnectMode](media/fenable-2.png)

5. Wenn **fReverseConnectMode** nicht auf **1** festgelegt ist, wählen Sie **fReverseConnectMode** aus und geben **1** in das Wertfeld ein. 
6. Wenn **fEnableWinStation** nicht auf **1** festgelegt ist, wählen Sie **fEnableWinStation** aus und geben **1** in das Wertfeld ein.
7. Starten Sie den virtuellen Computer neu. 

>[!NOTE]
>Um **fReverseConnectMode** oder **fEnableWinStation** für mehrere VMs gleichzeitig zu ändern, können Sie einen der folgenden Schritte ausführen:
>
>- Exportieren Sie den Registrierungsschlüssel von einem Computer, der bereits funktioniert, und importieren Sie den Schlüssel auf allen Computern, für die diese Änderung erforderlich ist.
>- Erstellen Sie ein Gruppenrichtlinienobjekt (Group Policy Object, GPO), das den Wert des Registrierungsschlüssels für die Computer festlegt, für die diese Änderung erforderlich ist.

7. Wechseln Sie zu **HKEY_LOCAL_MACHINE** > **SYSTEM** > **CurrentControlSet** > **Control** > **Terminal Server** > **ClusterSettings**.
8. Suchen Sie unter **ClusterSettings** nach **SessionDirectoryListener**, und stellen Sie sicher, dass der Datenwert **rdp-sxs...** lautet.
9. Wenn **SessionDirectoryListener** nicht auf **rdp-sxs...** festgelegt ist, müssen Sie die Schritte im Abschnitt [Deinstallieren sämtlicher Komponenten für Agent, Bootloader und Stapel](#step-1-uninstall-all-agent-boot-loader-and-stack-component-programs) ausführen. Damit werden zunächst der Agent, der Bootloader und die Stapelkomponenten deinstalliert. [Installieren Sie Agent und Bootloader danach neu](#step-4-reinstall-the-agent-and-boot-loader). Damit wird der parallele Stapel neu installiert.

## <a name="error-users-keep-getting-disconnected-from-session-hosts"></a>Error: Die Verbindung von Benutzern mit dem Sitzungshosts wird immer wieder getrennt

Wechseln Sie zu **Ereignisanzeige** > **Windows-Protokolle** > **Anwendung**. Wenn Sie ein Ereignis mit der ID 0 und der Beschreibung **CheckSessionHostDomainIsReachableAsync** feststellen und/oder Benutzer immer wieder von ihren Sitzungshosts getrennt werden, erfasst Ihr Server keinen Heartbeat vom Windows Virtual Desktop-Dienst.

Um dieses Problem zu lösen, ändern Sie den Schwellenwert für den Heartbeat:
1. Öffnen Sie eine Eingabeaufforderung als Administrator.
2. Geben Sie den Befehl **qwinsta** ein, und führen Sie ihn aus.
3. Es sollten zwei Stapelkomponenten angezeigt werden: **rdp-tcp** und **rdp-sxs**. 
   - Je nach verwendeter Betriebssystemversion folgt nach **rdp-sxs** möglicherweise eine Buildnummer. Wenn dies der Fall ist, notieren Sie sich diese Nummer für später.
4. Öffnen Sie den Registrierungs-Editor.
5. Wechseln Sie zu **HKEY_LOCAL_MACHINE** > **SYSTEM** > **CurrentControlSet** > **Control** > **Terminal Server** > **WinStations**.
6. Unter **WinStations** werden möglicherweise mehrere Ordner für verschiedene Stapelversionen angezeigt. Wählen Sie den Ordner aus, der der Versionsnummer aus Schritt 3 entspricht.
7. Erstellen Sie einen neuen DWORD-Wert in der Registrierung, indem Sie mit der rechten Maustaste in den Registrierungs-Editor klicken und **Neu** > **DWORD-Wert (32-Bit)** auswählen. Geben Sie zum Erstellen des DWORD-Werts die folgenden Werte ein:
   - HeartbeatInterval: 10000
   - HeartbeatWarnCount: 30 
   - HeartbeatDropCount: 60 
8. Starten Sie den virtuellen Computer neu.

## <a name="error-downloadmsiexception"></a>Error: DownloadMsiException

Wechseln Sie zu **Ereignisanzeige** > **Windows-Protokolle** > **Anwendung**. Wenn Sie ein Ereignis mit der ID 3277 und der Beschreibung **DownloadMsiException** feststellen, ist auf dem Datenträger nicht genügend Speicherplatz für den RDAgent vorhanden.

Um dieses Problem zu beheben, geben Sie durch eine der folgenden Aktionen Speicherplatz auf dem Datenträger frei:
   - Löschen von nicht mehr benötigten Dateien
   - Erhöhen der Speicherkapazität der VM

## <a name="error-vms-are-stuck-in-unavailable-or-upgrading-state"></a>Error: VMs verbleiben im Zustand „Nicht verfügbar“ oder „Upgrade wird durchgeführt“

Öffnen Sie ein PowerShell-Fenster als Administrator, und führen Sie das folgende Cmdlet aus:

```powershell
Get-AzWvdSessionHost -ResourceGroupName <resourcegroupname> -HostPoolName <hostpoolname> | Select-Object *
```

Wenn der Status für den Sitzungshost oder die Hosts im Hostpool immer **Nicht verfügbar** oder **Upgrade wird durchgeführt** lautet, liegt möglicherweise ein Fehler in der Agent- oder Stapelinstallation vor.

Um dieses Problem zu beheben, installieren Sie den parallelen Stapel neu:
1. Öffnen Sie eine Eingabeaufforderung als Administrator.
2. Geben Sie **net stop RDAgentBootLoader** ein. 
3. Wechseln Sie zu **Systemsteuerung** > **Programme** > **Programme und Funktionen**.
4. Deinstallieren Sie die aktuelle Version von **Remote Desktop Services SxS Network Stack** oder die in **HKEY_LOCAL_MACHINE** > **SYSTEM** > **CurrentControlSet** > **Control** > **Terminal Server** > **WinStations** unter **ReverseConnectListener** aufgelistete Version.
5. Öffnen Sie ein Konsolenfenster als Administrator, und wechseln Sie zu **Programme** > **Microsoft RDInfra**.
6. Wählen Sie die Komponente **SxSStack**, oder führen Sie den Befehl **msiexec /i SxsStack-<version>.msi** aus, um die MSI-Datei zu installieren.
8. Starten Sie den virtuellen Computer neu.
9. Kehren Sie zur Eingabeaufforderung zurück, und führen Sie den Befehl **qwinsta** aus.
10. Überprüfen Sie, ob neben der in Schritt 6 installierten Stapelkomponente **Lauschen** angezeigt wird.
   - Wenn dies der Fall ist, geben Sie **net start RDAgentBootLoader** an der Eingabeaufforderung ein, und starten Sie die VM neu.
   - Andernfalls müssen Sie [Ihre VM erneut registrieren und die Agent-Komponente neu installieren](#your-issue-isnt-listed-here-or-wasnt-resolved).

## <a name="error-connection-not-found-rdagent-does-not-have-an-active-connection-to-the-broker"></a>Error: Verbindung nicht gefunden: Der RDAgent verfügt nicht über eine aktive Verbindung mit dem Broker

Möglicherweise ist das Verbindungslimit Ihrer VMs erreicht, sodass die VM keine neuen Verbindungen annehmen kann.

So lösen Sie das Problem:
   - Verringern Sie das Limit für die maximale Anzahl von Sitzungen. So wird sichergestellt, dass die Ressourcen gleichmäßiger auf die Sitzungshosts verteilt werden, und Ressourcenknappheit wird verhindert.
   - Erhöhen Sie die Ressourcenkapazität der VMs.

## <a name="error-operating-a-pro-vm-or-other-unsupported-os"></a>Error: Betreiben einer Pro VM oder eines nicht unterstützten Betriebssystems

Der parallele Stapel wird nur von Windows Enterprise- oder Windows Server-SKUs unterstützt, unter Betriebssystemen wie Pro VM nicht. Wenn Sie nicht über eine Enterprise- oder Server-SKU verfügen, wird der Stapel auf der VM installiert, aber nicht aktiviert. Daher wird er nicht angezeigt, wenn Sie **qwinsta** in der Befehlszeile ausführen.

Um dieses Problem zu lösen, erstellen Sie eine VM mit Windows Enterprise oder Windows Server.
1. Wechseln Sie zu [Details zum virtuellen Computer](create-host-pools-azure-marketplace.md#virtual-machine-details), und führen Sie die Schritte 1–12 aus, um eins der folgenden empfohlenen Images einzurichten:
   - Windows 10 Enterprise, Mehrfachsitzung, Version 1909
   - Windows 10 Enterprise, Mehrfachsitzung, Version 1909 + Microsoft 365-Apps
   - Windows Server 2019 Datacenter
   - Windows 10 Enterprise, Mehrfachsitzung, Version 2004
   - Windows 10 Enterprise, Mehrfachsitzung, Version 2004 + Microsoft 365-Apps
2. Klicken Sie auf **Überprüfen und erstellen**.

## <a name="error-name_already_registered"></a>Error: NAME_ALREADY_REGISTERED

Der Name Ihrer VM wurde bereits registriert und ist wahrscheinlich ein Duplikat.

So lösen Sie das Problem:
1. Führen Sie die Schritte im Abschnitt [Entfernen des Sitzungshosts aus dem Hostpool](#step-2-remove-the-session-host-from-the-host-pool) aus.
2. [Erstellen Sie eine andere VM](expand-existing-host-pool.md#add-virtual-machines-with-the-azure-portal). Stellen Sie sicher, dass Sie einen eindeutigen Namen für diese VM auswählen.
3. Wechseln Sie zum Azure-Portal (https://portal.azure.com), und öffnen Sie die Seite **Übersicht** für den Hostpool, in dem sich Ihre VM befand. 
4. Öffnen Sie die Registerkarte **Sitzungshosts**, und überprüfen Sie, ob sich alle Sitzungshosts in diesem Hostpool befinden.
5. Warten Sie 5–10 Minuten, bis der Status des Sitzungshosts **Verfügbar** lautet.

   > [!div class="mx-imgBorder"]
   > ![Screenshot des verfügbaren Sitzungshosts](media/hostpool-portal.png)

## <a name="your-issue-isnt-listed-here-or-wasnt-resolved"></a>Ihr Problem wird hier nicht aufgeführt oder konnte nicht gelöst werden

Wenn Ihr Problem in diesem Artikel nicht erläutert wurde oder die Anweisungen Ihr Problem nicht lösen konnten, empfiehlt es sich, den Windows Virtual Desktop-Agent zu deinstallieren, erneut zu installieren und erneut zu registrieren. In diesem Abschnitt wird erläutert, wie Sie Ihre VM erneut beim Windows Virtual Desktop-Dienst registrieren, indem Sie den sämtliche Komponenten für Agent, Bootloader und Stapel deinstallieren, den Sitzungshost aus dem Hostpool entfernen, einen neuen Registrierungsschlüssel für die VM generieren und Agent und Bootloader erneut installieren. Wenn eines (oder mehrere) der folgenden Szenarien auf Sie zutrifft, führen Sie die folgenden Schritte aus:
- Ihre VM verbleibt im Zustand **Nicht verfügbar** oder **Upgrade wird durchgeführt**.
- Der Stapellistener funktioniert nicht, und Sie führen Windows 10 in der Version 1809, 1903 oder 1904 aus.
- Sie erhalten den Fehler **EXPIRED_REGISTRATION_TOKEN**.
- Ihre VMs werden in der Liste der Sitzungshosts nicht angezeigt.
- **Remote Desktop Agent Loader** wird im Fenster „Dienste“ nicht angezeigt.
- Die Komponente **RdAgentBootLoader** wird im Task-Manager nicht angezeigt.
- Mit den Anweisungen in diesem Artikel lässt sich Ihr Problem nicht lösen.

### <a name="step-1-uninstall-all-agent-boot-loader-and-stack-component-programs"></a>Schritt 1: Deinstallieren sämtlicher Komponentenprogramme für Agent, Bootloader und Stapel

Bevor Sie den Agent, den Bootloader und den Stapel neu installieren, müssen Sie alle vorhandenen Komponentenprogramme auf Ihrer VM deinstallieren. So deinstallieren Sie sämtliche Komponentenprogramme für Agent, Bootloader und Stapel:
1. Melden Sie sich bei der VM als Administrator an. 
2. Wechseln Sie zu **Systemsteuerung** > **Programme** > **Programme und Funktionen**.
3. Entfernen Sie die folgenden Programme:
   - Remote Desktop Agent Boot Loader
   - Remote Desktop Services Infrastructure Agent
   - Remote Desktop Services Infrastructure Geneva Agent
   - Remote Desktop Services SxS Network Stack
   
>[!NOTE]
>Möglicherweise werden mehrere Instanzen dieser Programme angezeigt. Stellen Sie sicher, dass alle Instanzen entfernt werden.

   > [!div class="mx-imgBorder"]
   > ![Screenshot: Deinstallieren der Programme](media/uninstall-program.png)

### <a name="step-2-remove-the-session-host-from-the-host-pool"></a>Schritt 2: Entfernen des Sitzungshosts aus dem Hostpool

Wenn Sie den Sitzungshost aus dem Hostpool entfernen, ist dieser Host nicht mehr für diesen Pool registriert. Diese Vorgehensweise fungiert als Zurücksetzung der Sitzungshostregistrierung. So entfernen Sie den Sitzungshost aus dem Hostpool:
1. Wechseln Sie im [Azure-Portal](https://portal.azure.com) zur Seite **Übersicht** für den Hostpool, in dem sich Ihre VM befindet. 
2. Wechseln Sie zur Registerkarte **Sitzungshosts**, um eine Liste aller Sitzungshosts in diesem Hostpool anzuzeigen.
3. Sehen Sie sich die Liste der Sitzungshosts an, und wählen Sie die VM aus, die Sie entfernen möchten.
4. Wählen Sie **Entfernen**.  

   > [!div class="mx-imgBorder"]
   > ![Screenshot: Entfernen der VM aus dem Hostpool](media/remove-sh.png)

### <a name="step-3-generate-a-new-registration-key-for-the-vm"></a>Schritt 3: Generieren eines neuen Registrierungsschlüssels für die VM

Sie müssen einen neuen Registrierungsschlüssel generieren, mit dem Ihre VM beim Hostpool und beim Dienst erneut registriert wird. So generieren Sie einen neuen Registrierungsschlüssel für die VM:
1. Öffnen Sie das [Azure-Portal](https://portal.azure.com), und wechseln Sie zur Seite **Übersicht** für den Hostpool der VM, die Sie bearbeiten möchten.
2. Wählen Sie **Registrierungsschlüssel** aus.

   > [!div class="mx-imgBorder"]
   > ![Screenshot des Registrierungsschlüssels im Portal](media/reg-key.png)

3. Öffnen Sie die Registerkarte **Registrierungsschlüssel**, und wählen Sie **Neuen Schlüssel generieren** aus.
4. Geben Sie das Ablaufdatum ein, und klicken Sie auf **OK**.  

>[!NOTE]
>Das Ablaufdatum darf nicht weniger als eine Stunde und nicht mehr als 27 Tage nach dem Datum und der Uhrzeit der Generierung liegen. Es wird dringend empfohlen, das Ablaufdatum auf den Höchstwert von 27 Tagen festzulegen.

5. Kopieren Sie den neu generierten Schlüssel in die Zwischenablage. Sie benötigen diesen Schlüssel später.

### <a name="step-4-reinstall-the-agent-and-boot-loader"></a>Schritt 4: Neuinstallieren von Agent und Bootloader

Wenn Sie die neueste Version von Agent und Bootloader installieren, werden der parallele Stapel und der Geneva-Überwachungs-Agent automatisch ebenfalls installiert. So installieren Sie Agent und Bootloader neu:
1. Melden Sie sich bei Ihrer VM als Administrator an, und befolgen Sie die Anweisungen unter [Registrieren der virtuellen Computer](create-host-pools-powershell.md#register-the-virtual-machines-to-the-windows-virtual-desktop-host-pool), um den **Windows Virtual Desktop-Agent** und den **Bootloader für Windows Virtual Desktop-Agents** herunterzuladen.

   > [!div class="mx-imgBorder"]
   > ![Screenshot der Downloadseite für Agent und Bootloader](media/download-agent.png)

2. Klicken Sie mit der rechten Maustaste auf die Installationsprogramme für den Agent und den Bootloader, die Sie gerade heruntergeladen haben.
3. Wählen Sie **Eigenschaften** aus.
4. Klicken Sie auf **Unblock** (Entsperren).
5. Klicken Sie auf **OK**.
6. Führen Sie das Installationsprogramm des Agents aus.
7. Wenn das Installationsprogramm Sie zur Eingabe des Registrierungstokens auffordert, fügen Sie den Registrierungsschlüssel aus der Zwischenablage ein. 

   > [!div class="mx-imgBorder"]
   > ![Screenshot des eingefügten Registrierungstokens](media/pasted-agent-token.png)

8. Führen Sie das Installationsprogramm für den Bootloader aus.
9. Starten Sie den virtuellen Computer neu. 
10. Wechseln Sie zum [Azure-Portal](https://portal.azure.com), und öffnen Sie die Seite **Übersicht** für den Hostpool, zu dem Ihre VM gehört.
11. Wechseln Sie zur Registerkarte **Sitzungshosts**, um eine Liste aller Sitzungshosts in diesem Hostpool anzuzeigen.
12. Der im Hostpool registrierte Sitzungshost sollte jetzt mit dem Status **Verfügbar** angezeigt werden. 

   > [!div class="mx-imgBorder"]
   > ![Screenshot des verfügbaren Sitzungshosts](media/hostpool-portal.png)

## <a name="next-steps"></a>Nächste Schritte

Falls Ihr Problem weiterhin besteht, erstellen Sie eine Supportanfrage, und geben Sie detaillierte Informationen zum Problem sowie zu den Aktionen an, die Sie ausgeführt haben, um das Problem zu lösen. Die folgende Liste enthält weitere Ressourcen, die Sie zur Behandlung von Problemen in Ihrer Windows Virtual Desktop-Bereitstellung verwenden können.

- Eine Übersicht über die Problembehandlung von Windows Virtual Desktop und die Eskalationspfade finden Sie unter [Überblick über Problembehandlung, Feedback und Support](troubleshoot-set-up-overview.md).
- Informationen zur Problembehandlung beim Erstellen eines Hostpools in einer Windows Virtual Desktop-Umgebung finden Sie unter [Mandanten- und Hostpoolerstellung](troubleshoot-set-up-issues.md).
- Informationen zur Problembehandlung bei der Konfiguration eines virtuellen Computers (VM) in Windows Virtual Desktop finden Sie unter [Konfiguration des virtuellen Sitzungshostcomputers](troubleshoot-vm-configuration.md).
- Informationen zur Behebung von Problemen bei Windows Virtual Desktop-Clientverbindungen finden Sie unter [Windows Virtual Desktop – Clientverbindungen](troubleshoot-service-connection.md).
- Informationen zur Behebung von Problemen bei Remotedesktopclients finden Sie unter [Troubleshooting für den Remotedesktopclient](troubleshoot-client.md).
- Informationen zur Problembehandlung bei der Verwendung von PowerShell mit Windows Virtual Desktop finden Sie unter [Windows Virtual Desktop – PowerShell](troubleshoot-powershell.md).
- Weitere Informationen zum Dienst finden Sie unter [Windows Virtual Desktop-Umgebung](environment-setup.md).
- Ein Tutorial zur Problembehandlung finden Sie unter [Tutorial: Problembehandlung von Bereitstellungen der Resource Manager-Vorlage](../azure-resource-manager/templates/template-tutorial-troubleshoot.md).
- Informationen zur Überwachung von Aktionen finden Sie unter [Überwachen von Vorgängen mit Resource Manager](../azure-resource-manager/management/view-activity-logs.md).
- Weitere Informationen zu Aktionen zum Bestimmen von Fehlern während der Bereitstellung finden Sie unter [Anzeigen von Bereitstellungsvorgängen mit dem Azure-Portal](../azure-resource-manager/templates/deployment-history.md).
