---
title: Leitfaden zur Azure Disk Encryption-Problembehandlung
description: Dieser Artikel bietet Tipps zur Problembehandlung für Microsoft Azure Disk Encryption für virtuelle Windows-Computer.
author: msmbaldwin
ms.service: virtual-machines
ms.subservice: disks
ms.collection: windows
ms.topic: troubleshooting
ms.author: mbaldwin
ms.date: 08/06/2019
ms.custom: seodec18
ms.openlocfilehash: 057aba3e49d32694563f412101be499342f2aad0
ms.sourcegitcommit: 7edadd4bf8f354abca0b253b3af98836212edd93
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2021
ms.locfileid: "102550602"
---
# <a name="azure-disk-encryption-troubleshooting-guide"></a>Leitfaden zur Azure Disk Encryption-Problembehandlung

Dieser Leitfaden ist für IT-Experten, Informationssicherheitsanalysten und Cloudadministratoren bestimmt, in deren Organisationen Azure Disk Encryption verwendet wird. Dieser Artikel hilft beim Beheben von Problemen mit der Verschlüsselung von Datenträgern.

Bevor Sie einen der folgenden Schritte ausführen, vergewissern Sie sich zunächst, dass die zu verschlüsselnden VMs zu den[unterstützten VM-Größen und -Betriebssystemen](disk-encryption-overview.md#supported-vms-and-operating-systems) gehören und dass alle Voraussetzungen erfüllt sind:

- [Netzwerkanforderungen](disk-encryption-overview.md#networking-requirements)
- [Gruppenrichtlinienanforderungen](disk-encryption-overview.md#group-policy-requirements)
- [Speicheranforderungen für Verschlüsselungsschlüssel](disk-encryption-overview.md#encryption-key-storage-requirements)

## <a name="troubleshooting-azure-disk-encryption-behind-a-firewall"></a>Azure Disk Encryption-Problembehandlung hinter einer Firewall

Wenn die Konnektivität durch eine Firewall, eine Proxyanforderung oder Einstellungen für Netzwerksicherheitsgruppen (NSGs) eingeschränkt ist, kann die Fähigkeit der Erweiterung zur Durchführung von erforderlichen Aufgaben beeinträchtigt sein. Dies kann zu Statusmeldungen der Art „Erweiterungsstatus auf der VM nicht verfügbar“. In erwarteten Szenarien kann die Verschlüsselung nicht abgeschlossen werden. In den folgenden Abschnitten werden einige häufig auftretende Firewallprobleme beschrieben, die Sie ggf. untersuchen müssen.

### <a name="network-security-groups"></a>Netzwerksicherheitsgruppen
Für alle angewendeten Einstellungen von Netzwerksicherheitsgruppen muss es ermöglicht werden, dass der Endpunkt die dokumentierten [Voraussetzungen](disk-encryption-overview.md#networking-requirements) für die Netzwerkkonfiguration in Bezug auf die Datenträgerverschlüsselung erfüllt.

### <a name="azure-key-vault-behind-a-firewall"></a>Azure Key Vault hinter einer Firewall

Wenn die Verschlüsselung mit [Azure AD-Anmeldeinformationen](disk-encryption-windows-aad.md#) aktiviert wird, muss der virtuelle Zielcomputer die Konnektivität sowohl mit Azure Active Directory-Endpunkten als auch mit Schlüsseltresor-Endpunkten zulassen. Aktuelle Azure Active Directory-Authentifizierungsendpunkte werden in den Abschnitten 56 und 59 der Dokumentation zu [URLs und IP-Adressbereichen in Microsoft 365](/microsoft-365/enterprise/urls-and-ip-address-ranges) verwaltet. Anweisungen zu Schlüsseltresoren werden in der Dokumentation [Zugreifen auf Azure Key Vault hinter einer Firewall](../../key-vault/general/access-behind-firewall.md) bereitgestellt.

### <a name="azure-instance-metadata-service"></a>Azure-Instanzmetadatendienst 
Der virtuelle Computer muss Zugriff auf den [Azure-Instanzmetadatendienst](../windows/instance-metadata-service.md)-Endpunkt haben, der eine bekannte nicht routingfähige IP-Adresse (`169.254.169.254`) verwendet, auf die nur von innerhalb des virtuellen Computers aus zugegriffen werden kann.  Proxykonfigurationen, die den lokalen HTTP-Datenverkehr an diese Adresse ändern (z. B. durch Hinzufügen eines X-Forwarded-For-Headers), werden nicht unterstützt.

## <a name="troubleshooting-windows-server-2016-server-core"></a>Problembehandlung bei Windows Server 2016 Server Core

Unter Windows Server 2016 Server Core ist die Komponente bdehdcfg nicht standardmäßig verfügbar. Diese Komponente ist für Azure Disk Encryption erforderlich. Mit ihr wird das Systemvolume vom Betriebssystemvolume getrennt. Diese Teilung erfolgt nur einmal während der Lebensdauer des virtuellen Computers. Diese Binärdateien sind während der späteren Verschlüsselungsvorgänge nicht erforderlich.

Um dieses Problem zu umgehen, kopieren Sie die folgenden vier Dateien von einer Windows Server 2016 Data Center-VM an denselben Speicherort unter Server Core:

   ```
   \windows\system32\bdehdcfg.exe
   \windows\system32\bdehdcfglib.dll
   \windows\system32\en-US\bdehdcfglib.dll.mui
   \windows\system32\en-US\bdehdcfg.exe.mui
   ```

1. Geben Sie den folgenden Befehl ein:

   ```
   bdehdcfg.exe -target default
   ```

1. Dieser Befehl erstellt eine Systempartition der Größe 550 MB. Starten Sie das System neu.

1. Verwenden Sie DiskPart zum Überprüfen der Volumes, und fahren Sie dann fort.  

Beispiel:

```
DISKPART> list vol

  Volume ###  Ltr  Label        Fs     Type        Size     Status     Info
  ----------  ---  -----------  -----  ----------  -------  ---------  --------
  Volume 0     C                NTFS   Partition    126 GB  Healthy    Boot
  Volume 1                      NTFS   Partition    550 MB  Healthy    System
  Volume 2     D   Temporary S  NTFS   Partition     13 GB  Healthy    Pagefile
```

## <a name="troubleshooting-encryption-status"></a>Behandeln von Problemen mit dem Verschlüsselungsstatus 

Im Portal kann ein Datenträger als verschlüsselt angezeigt werden, auch nachdem er bereits auf dem virtuellen Computer entschlüsselt wurde.  Dies kann auftreten, wenn Befehle auf niedriger Ebene verwendet werden, um den Datenträger auf dem virtuellen Computer zu entschlüsseln, anstatt die Verwaltungsbefehle auf höherer Ebene von Azure Disk Encryption zu verwenden.  Die Befehle auf höherer Ebene entschlüsseln den Datenträger nicht nur auf dem virtuellen Computer, sondern sie aktualisieren auch außerhalb des virtuellen Computers wichtige Verschlüsselungs- und Erweiterungseinstellungen auf Plattformebene, die dem virtuellen Computer zugeordnet sind.  Wenn diese nicht einheitlich gehalten werden, kann die Plattform den Verschlüsselungsstatus nicht melden und den virtuellen Computer nicht ordnungsgemäß bereitstellen.

Verwenden Sie zum Deaktivieren von Azure Disk Encryption mit PowerShell [Disable-AzVMDiskEncryption](/powershell/module/az.compute/disable-azvmdiskencryption), gefolgt von [Remove-AzVMDiskEncryptionExtension](/powershell/module/az.compute/remove-azvmdiskencryptionextension). Wenn Sie „Remove-AzVMDiskEncryptionExtension“ ausführen, bevor die Verschlüsselung deaktiviert wurde, tritt ein Fehler auf.

Verwenden Sie zum Deaktivieren von Azure Disk Encryption mit der CLI [az vm encryption disable](/cli/azure/vm/encryption). 

## 



## <a name="next-steps"></a>Nächste Schritte

In diesem Dokument haben Sie weitere Informationen zu einigen häufigen Problemen in Azure Disk Encryption und deren Behandlung erhalten. Weitere Informationen zu diesem Dienst und seinen Funktionen finden Sie in den folgenden Artikeln:

- [Anwenden der Datenträgerverschlüsselung in Azure Security Center](../../security-center/asset-inventory.md)
- [Datenverschlüsselung ruhender Azure-Daten](../../security/fundamentals/encryption-atrest.md)