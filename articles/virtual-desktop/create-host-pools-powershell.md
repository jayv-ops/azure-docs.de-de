---
title: 'Erstellen eines Windows Virtual Desktop-Hostpools – PowerShell: Azure'
description: Informationen zum Erstellen eines Hostpools in Windows Virtual Desktop mit PowerShell-Cmdlets.
author: Heidilohr
ms.topic: how-to
ms.date: 10/02/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 9ec900f0537030d3ed0d1c875e8125806159bd51
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "98251453"
---
# <a name="create-a-windows-virtual-desktop-host-pool-with-powershell"></a>Erstellen eines Windows Virtual Desktop-Hostpools mit PowerShell

>[!IMPORTANT]
>Dieser Inhalt gilt für Windows Virtual Desktop mit Windows Virtual Desktop-Objekten für Azure Resource Manager. Wenn Sie Windows Virtual Desktop (klassisch) ohne Azure Resource Manager-Objekte verwenden, finden Sie weitere Informationen in [diesem Artikel](./virtual-desktop-fall-2019/create-host-pools-powershell-2019.md).

Hostpools sind eine Sammlung identischer virtueller Computer innerhalb von Windows Virtual Desktop-Mandantenumgebungen. Jeder Hostpool kann mit mehreren RemoteApp-Gruppen, einer Desktop-App-Gruppe und mehreren Sitzungshosts verknüpft werden.

## <a name="prerequisites"></a>Voraussetzungen

Dieser Artikel setzt voraus, dass Sie die Anweisungen unter [Einrichten des PowerShell-Moduls](powershell-module.md) bereits befolgt haben.

## <a name="use-your-powershell-client-to-create-a-host-pool"></a>Verwenden Ihres PowerShell-Clients zum Erstellen eines Hostpools

Führen Sie das folgende Cmdlet aus, um sich bei der Windows Virtual Desktop-Umgebung anzumelden:

```powershell
New-AzWvdHostPool -ResourceGroupName <resourcegroupname> -Name <hostpoolname> -WorkspaceName <workspacename> -HostPoolType <Pooled|Personal> -LoadBalancerType <BreadthFirst|DepthFirst|Persistent> -Location <region> -DesktopAppGroupName <appgroupname>
```

Mit diesem Cmdlet werden Hostpool, Arbeitsbereich und Desktop-App-Gruppe erstellt. Außerdem wird die Desktop-App-Gruppe beim Arbeitsbereich registriert. Sie können entweder mit diesem Cmdlet einen Arbeitsbereich erstellen oder einen bereits vorhandenen Arbeitsbereich verwenden.

Führen Sie das nächste Cmdlet aus, um ein Registrierungstoken zum Autorisieren eines Sitzungshosts zu erstellen und den Hostpool einzubinden und in einer neuen Datei auf Ihrem lokalen Computer zu speichern. Sie können mit dem Parameter „-ExpirationHours“ angeben, wie lange das Registrierungstoken gültig ist.

>[!NOTE]
>Das Ablaufdatum des Tokens darf nicht weniger als eine Stunde und nicht mehr als einen Monat betragen. Wenn Sie *-ExpirationTime* außerhalb dieser Vorgaben festlegen, erstellt das Cmdlet das Token nicht.

```powershell
New-AzWvdRegistrationInfo -ResourceGroupName <resourcegroupname> -HostPoolName <hostpoolname> -ExpirationTime $((get-date).ToUniversalTime().AddDays(1).ToString('yyyy-MM-ddTHH:mm:ss.fffffffZ'))
```

Wenn Sie beispielsweise ein Token erstellen möchten, das nach zwei Stunden abläuft, führen Sie dieses Cmdlet aus:

```powershell
New-AzWvdRegistrationInfo -ResourceGroupName <resourcegroupname> -HostPoolName <hostpoolname> -ExpirationTime $((get-date).ToUniversalTime().AddHours(2).ToString('yyyy-MM-ddTHH:mm:ss.fffffffZ'))
```

Führen Sie anschließend dieses Cmdlet aus, um Azure Active Directory-Benutzer der Desktop-App-Standardgruppe für den Hostpool hinzuzufügen.

```powershell
New-AzRoleAssignment -SignInName <userupn> -RoleDefinitionName "Desktop Virtualization User" -ResourceName <hostpoolname+"-DAG"> -ResourceGroupName <resourcegroupname> -ResourceType 'Microsoft.DesktopVirtualization/applicationGroups'
```

Führen Sie anschließend dieses Cmdlet aus, um Azure Active Directory-Benutzer der Desktop-App-Standardgruppe für den Hostpool hinzuzufügen:

```powershell
New-AzRoleAssignment -ObjectId <usergroupobjectid> -RoleDefinitionName "Desktop Virtualization User" -ResourceName <hostpoolname+"-DAG"> -ResourceGroupName <resourcegroupname> -ResourceType 'Microsoft.DesktopVirtualization/applicationGroups'
```

Führen Sie das folgende Cmdlet aus, um das Registrierungstoken in eine Variable zu exportieren. Sie verwenden sie später unter [Registrieren der virtuellen Computer unter dem Windows Virtual Desktop-Hostpool (Vorschauversion)](#register-the-virtual-machines-to-the-windows-virtual-desktop-host-pool).

```powershell
$token = Get-AzWvdRegistrationInfo -ResourceGroupName <resourcegroupname> -HostPoolName <hostpoolname>
```

## <a name="create-virtual-machines-for-the-host-pool"></a>Erstellen von virtuellen Computern für den Hostpool

Sie können jetzt einen virtuellen Azure-Computer erstellen, der in Ihren Windows Virtual Desktop-Hostpool eingebunden werden kann.

Sie können einen virtuellen Computer auf unterschiedliche Arten erstellen:

- [Erstellen eines virtuellen Computers aus einem Azure-Katalogimage](../virtual-machines/windows/quick-create-portal.md#create-virtual-machine)
- [Erstellen eines virtuellen Computers aus einem verwalteten Image](../virtual-machines/windows/create-vm-generalized-managed.md)
- [Erstellen eines virtuellen Computers aus einem nicht verwalteten Image](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-user-image-data-disks)

>[!NOTE]
>Wenn Sie einen virtuellen Computer mit Windows 7 als Hostbetriebssystem bereitstellen, ist der Erstellungs- und Bereitstellungsprozess ein wenig anders. Weitere Informationen finden Sie unter [Bereitstellen eines virtuellen Windows 7-Computers in Windows Virtual Desktop](./virtual-desktop-fall-2019/deploy-windows-7-virtual-machine.md).

Nachdem Sie Ihre virtuellen Sitzungshostcomputer erstellt haben, [wenden Sie eine Windows-Lizenz auf einen virtuellen Sitzungshostcomputer an](./apply-windows-license.md#apply-a-windows-license-to-a-session-host-vm), um Ihre virtuellen Windows- oder Windows Server-Computer auszuführen, ohne für eine weitere Lizenz zu bezahlen.

## <a name="prepare-the-virtual-machines-for-windows-virtual-desktop-agent-installations"></a>Vorbereiten der virtuellen Computer für Installationen von Windows Virtual Desktop-Agents

Sie müssen die folgenden Schritte zum Vorbereiten Ihrer virtuellen Computer ausführen, bevor Sie die Windows Virtual Desktop-Agents installieren und die virtuellen Computer für Ihren Windows Virtual Desktop-Hostpool registrieren können:

- Sie müssen für den Computer den Domänenbeitritt durchführen. Auf diese Weise können eingehende Windows Virtual Desktop-Benutzer von ihrem Azure Active Directory-Konto dem Active Directory-Konto zugeordnet werden, und ihnen kann der Zugriff auf den virtuellen Computer erfolgreich gewährt werden.
- Sie müssen die Rolle „Remotedesktop-Sitzungshost (RDSH)“ installieren, wenn auf dem virtuellen Computer ein Windows Server-Betriebssystem ausgeführt wird. Mit der RDSH-Rolle können die Windows Virtual Desktop-Agents richtig installiert werden.

Führen Sie auf jedem virtuellen Computer die folgenden Schritte aus, damit der Domänenbeitritt erfolgreich ist:

1. [Stellen Sie eine Verbindung mit dem virtuellen Computer her](../virtual-machines/windows/quick-create-portal.md#connect-to-virtual-machine), indem Sie die Anmeldeinformationen verwenden, die Sie beim Erstellen des virtuellen Computers angegeben haben.
2. Starten Sie auf dem virtuellen Computer die **Systemsteuerung**, und wählen Sie **System**.
3. Wählen Sie **Computername**, **Einstellungen ändern** und dann **Ändern...** .
4. Wählen Sie **Domäne**, und geben Sie dann die Active Directory-Domäne im virtuellen Netzwerk ein.
5. Authentifizieren Sie sich mit einem Domänenkonto, das über Berechtigungen für in die Domäne eingebundene Computer verfügt.

    >[!NOTE]
    > Wenn Sie Ihre VMs mit einer Azure Active Directory Domain Services (Azure AD DS)-Umgebung verknüpfen, stellen Sie sicher, dass Ihre Domänenbeitrittsbenutzer ebenfalls Mitglied der [Gruppe „AAD DC-Administratoren“](../active-directory-domain-services/tutorial-create-instance-advanced.md#configure-an-administrative-group) ist.

>[!IMPORTANT]
>Es empfiehlt sich, keine Richtlinien oder Konfigurationen zu aktivieren, die eine Deaktivierung des Windows Installers bewirken. Wenn Sie den Windows Installer deaktivieren, kann der Dienst keine Agent-Updates auf Ihren Sitzungshosts installieren, und Ihre Sitzungshosts funktionieren nicht einwandfrei.

## <a name="register-the-virtual-machines-to-the-windows-virtual-desktop-host-pool"></a>Registrieren der virtuellen Computer für den Windows Virtual Desktop-Hostpool

Das Registrieren der virtuellen Computer für einen Windows Virtual Desktop-Hostpool ist so einfach wie das Installieren der Windows Virtual Desktop-Agents.

Führen Sie auf jedem virtuellen Computer die folgenden Schritte aus, um die Windows Virtual Desktop-Agents zu registrieren:

1. [Stellen Sie eine Verbindung mit dem virtuellen Computer her](../virtual-machines/windows/quick-create-portal.md#connect-to-virtual-machine), indem Sie die Anmeldeinformationen verwenden, die Sie beim Erstellen des virtuellen Computers angegeben haben.
2. Führen Sie den Download und die Installation des Windows Virtual Desktop-Agents durch.
   - Laden Sie den [Windows Virtual Desktop-Agent](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RWrmXv) herunter.
   - Führen Sie das Installationsprogramm aus. Wenn Sie vom Installationsprogramm nach dem Registrierungstoken gefragt werden, geben Sie den über das Cmdlet **Get-AzWvdRegistrationInfo** erhaltenen Wert ein.
3. Führen Sie den Download und die Installation des Bootloaders für Windows Virtual Desktop-Agents durch.
   - Führen Sie den Download des [Bootloaders für Windows Virtual Desktop-Agents](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RWrxrH) durch.
   - Führen Sie das Installationsprogramm aus.

>[!IMPORTANT]
>Zum Schutz Ihrer Windows Virtual Desktop-Umgebung in Azure empfiehlt es sich, den eingehenden Port 3389 auf Ihren virtuellen Computern nicht zu öffnen. Für Windows Virtual Desktop muss der eingehende Port 3389 nicht geöffnet sein, damit Benutzer auf die virtuellen Computer des Hostpools zugreifen können. Wenn Sie den Port 3389 zur Problembehandlung öffnen müssen, verwenden Sie am besten den [Just-In-Time-Zugriff auf virtuelle Computer](../security-center/security-center-just-in-time.md). Außerdem wird empfohlen, dass Sie Ihren VMs keine öffentliche IP-Adresse zuweisen.

## <a name="update-the-agent"></a>Aktualisieren des Agents

Sie müssen den Agent in folgenden Situationen aktualisieren:

- Sie möchten einen zuvor registrierten Sitzungshost zu einem neuen Hostpool migrieren.
- Der Sitzungshost wird nach einer Aktualisierung nicht in Ihrem Hostpool angezeigt.

Zum Aktualisieren des Agents führen Sie die folgenden Schritte aus:

1. Melden Sie sich bei der VM als Administrator an.
2. Navigieren Sie zu **Dienste**, und beenden Sie dann die Prozesse **Rdagent** und **Remote Desktop Agent Loader**.
3. Als Nächstes suchen Sie die Agent- und Bootloader-MSIs. Diese befinden sich entweder im Ordner **C:\DeployAgent** oder an dem Speicherort, an dem sie bei der Installation gespeichert wurden.
4. Suchen Sie die folgenden Dateien, und deinstallieren Sie sie:
     
     - Microsoft.RDInfra.RDAgent.Installer-x64-verx.x.x
     - Microsoft.RDInfra.RDAgentBootLoader.Installer-x64

   Zum Deinstallieren dieser Dateien klicken Sie mit der rechten Maustaste auf den jeweiligen Dateinamen, und wählen Sie dann **Deinstallieren** aus.
5. Optional können Sie auch die folgenden Registrierungseinstellungen entfernen:
     
     - Computer\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\RDInfraAgent
     - Computer\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\RDAgentBootLoader

6. Nachdem Sie diese Elemente deinstalliert haben, sollten alle Zuordnungen zum alten Hostpool entfernt sein. Wenn Sie diesen Host erneut für den Dienst registrieren möchten, folgen Sie die Anweisungen unter [Registrieren der virtuellen Computer für den Windows Virtual Desktop-Hostpool](create-host-pools-powershell.md#register-the-virtual-machines-to-the-windows-virtual-desktop-host-pool).


## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie nun einen Hostpool erstellt haben, können Sie ihn mit RemoteApps füllen. Weitere Informationen zum Verwalten von Apps in Windows Virtual Desktop finden Sie im folgenden Tutorial:

> [!div class="nextstepaction"]
> [Manage app groups for Windows Virtual Desktop Preview](./manage-app-groups.md) (Verwalten von App-Gruppen für Windows Virtual Desktop (Vorschauversion))
