---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 02/10/2020
ms.author: cynthn
ms.openlocfilehash: 397dd3d16fa994df29a08ff9095b4c7c6c4af815
ms.sourcegitcommit: 15d27661c1c03bf84d3974a675c7bd11a0e086e6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2021
ms.locfileid: "102510691"
---
| Resource | Begrenzung |
| --- | --- |
| VMs pro [Abonnement](https://azure.microsoft.com/pricing/) |25.000<sup>1</sup> pro Region |
| Gesamte Kerne pro [Abonnement](https://azure.microsoft.com/pricing/) |20<sup>1</sup> pro Region Wenden Sie sich an den Support, um diesen Grenzwert heraufzusetzen. |
| Azure Spot-VM: Kerne gesamt pro [Abonnement](https://azure.microsoft.com/pricing/) |20<sup>1</sup> pro Region Wenden Sie sich an den Support, um diesen Grenzwert heraufzusetzen. |
| VM-Kerne pro Serie (z. B. Dv2 und F) pro [Abonnement](https://azure.microsoft.com/pricing/) |20<sup>1</sup> pro Region Wenden Sie sich an den Support, um diesen Grenzwert heraufzusetzen. |
| [Verfügbarkeitsgruppen](../articles/virtual-machines/availability-set-overview.md) pro Abonnement |2\.500 pro Region |
| Virtuelle Computer pro Verfügbarkeitsgruppe | 200 |
| [Näherungsplatzierungsgruppen](../articles/virtual-machines/windows/proximity-placement-groups-portal.md) pro [Ressourcengruppe](../articles/azure-resource-manager/management/overview.md#resource-groups) | 800 | 
| Zertifikate pro Verfügbarkeitsgruppe | 199<sup>2</sup> |
| Zertifikate pro Abonnement |Unbegrenzt<sup>3</sup> |

<sup>1</sup> Standardgrenzwerte variieren nach angebotenem Kategorietyp, z. B. kostenlose Testversion oder nutzungsbasierte Bezahlung, und nach Serie, z. B. Dv2, F und G. Der Standardgrenzwert für Enterprise Agreement-Abonnements ist beispielsweise 350.

<sup>2</sup> Eigenschaften wie öffentliche SSH-Schlüssel werden ebenfalls als Zertifikate gepusht und zur Ermittlung des Grenzwerts gezählt. Verwenden Sie zum Umgehen dieses Grenzwerts die [Azure Key Vault-Erweiterung für Windows](../articles/virtual-machines/extensions/key-vault-windows.md) oder die [Azure Key Vault-Erweiterung für Linux](../articles/virtual-machines/extensions/key-vault-linux.md), um Zertifikate zu installieren.

<sup>3</sup> Beim Azure Resource Manager werden Zertifikate unter Azure Key Vault gespeichert. Die Anzahl der Zertifikate für ein Abonnement ist unbegrenzt. Es besteht ein Grenzwert von 1 MB an Zertifikaten pro Bereitstellung, die entweder aus einer einzelnen VM oder einer Verfügbarkeitsgruppe besteht.



> [!NOTE]
> Für VM-Kerne gilt ein regionaler Gesamtgrenzwert. Außerdem gilt ein Grenzwert für die regionaler Serien pro Größe wie Dv2 und F. Diese Grenzwerte werden separat erzwungen. Angenommen, Sie verwenden ein Abonnement mit einem Kerngesamtgrenzwert von 30 für VMs in der Region „USA, Osten“, einem Kerngrenzwert von 30 für die A-Serie und einem Kerngrenzwert von 30 für die D-Serie. Für dieses Abonnement dürfen dann 30 virtuelle A1-Computer bzw. 30 virtuelle D1-Computer bereitgestellt werden – oder eine Kombination daraus, bei der die Gesamtanzahl von 30 Kernen nicht überschritten wird. Ein Beispiel wäre eine Kombination aus 10 A1-VMs und 20 D1-VMs.  
> <!-- -->
>