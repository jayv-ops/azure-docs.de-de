---
title: Bereitstellen von Azure-Spot-VMs mithilfe von PowerShell
description: Erfahren Sie, wie Sie Azure-Spot-VMs mit Azure PowerShell bereitstellen, um Kosten zu sparen.
author: cynthn
ms.service: virtual-machines
ms.subservice: spot
ms.workload: infrastructure-services
ms.topic: how-to
ms.date: 06/26/2020
ms.author: cynthn
ms.reviewer: jagaveer
ms.openlocfilehash: 33172004ac4361de51b92389fbf56bd699f7124f
ms.sourcegitcommit: 4b7a53cca4197db8166874831b9f93f716e38e30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "102096444"
---
# <a name="deploy-azure-spot-virtual-machines-using-azure-powershell"></a>Bereitstellen von Azure-Spot-VMs mithilfe von Azure PowerShell


Wenn Sie [Azure Spot-VMs](../spot-vms.md) verwenden, profitieren Sie von unserer ungenutzten Kapazität und erzielen erhebliche Kosteneinsparungen. Wenn die Kapazität von Azure wieder benötigt wird, werden die Azure-Spot-VMs durch die Azure-Infrastruktur entfernt. Aus diesem Grund eignen sich Azure-Spot-VMs hervorragend für Workloads, die Unterbrechungen tolerieren, z. B. Batchverarbeitungsaufträge, Dev/Test-Umgebungen, umfangreiche Computeworkloads und mehr.

Die Preise für Azure-Spot-VMs variieren je nach Region und SKU. Weitere Informationen finden Sie unter den VM-Preisen für [Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) und [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/). Weitere Informationen zum Festlegen des maximalen Preises finden Sie unter [Azure-Spot-VMs – Preise](../spot-vms.md#pricing).

Sie haben die Möglichkeit, einen maximalen Preis festzulegen, den Sie pro Stunde für die VM bezahlen möchten. Der maximale Preis für eine Azure-Spot-VM kann in US-Dollar (USD) mit bis zu fünf Dezimalstellen festgelegt werden. Der Wert `0.98765` würde beispielsweise einem maximalen Preis von 0,98765 US-Dollar pro Stunde entsprechen. Wenn Sie den maximalen Preis auf `-1` festlegen, wird die VM nicht basierend auf dem Preis entfernt. Der Preis für die VM entspricht dem aktuellen Preis für Spot-VMs oder dem Preis für eine Standard-VM, je nachdem, welcher Preis niedriger ist, solange Kapazitäten und Kontingente verfügbar sind.


## <a name="create-the-vm"></a>Erstellen des virtuellen Computers

Erstellen Sie eine Spot-VM mithilfe von [New-AzVmConfig](/powershell/module/az.compute/new-azvmconfig), um die Konfiguration zu erstellen. Schließen Sie außerdem `-Priority Spot` ein, und legen Sie `-MaxPrice` auf eine der folgenden Optionen fest:
- `-1`, damit der virtuelle Computer nicht basierend auf dem Preis entfernt wird
- einen Dollarbetrag mit bis zu 5 Stellen. Beispielsweise bedeutet `-MaxPrice .98765`, dass die Zuordnung des virtuellen Computers aufgehoben wird, sobald der Preis für eine Spot-VM ca. 0,98765 USD pro Stunde übersteigt.


In diesem Beispiel wird eine Spot-VM erstellt, deren Zuordnung nicht basierend auf den Preisen aufgehoben wird. (Dies erfolgt nur, wenn Azure die Kapazität wieder benötigt.) Die Entfernungsrichtlinie wird so festgelegt, dass die Zuordnung der VM aufgehoben wird. Dadurch kann sie zu einem späteren Zeitpunkt neu gestartet werden. Wenn Sie die VM und den zugrunde liegenden Datenträger beim Entfernen löschen möchten, legen Sie `-EvictionPolicy` in `New-AzVMConfig` auf `Delete` fest.


```azurepowershell-interactive
$resourceGroup = "mySpotRG"
$location = "eastus"
$vmName = "mySpotVM"
$cred = Get-Credential -Message "Enter a username and password for the virtual machine."
New-AzResourceGroup -Name $resourceGroup -Location $location
$subnetConfig = New-AzVirtualNetworkSubnetConfig `
   -Name mySubnet -AddressPrefix 192.168.1.0/24
$vnet = New-AzVirtualNetwork -ResourceGroupName $resourceGroup `
   -Location $location -Name MYvNET -AddressPrefix 192.168.0.0/16 `
   -Subnet $subnetConfig
$pip = New-AzPublicIpAddress -ResourceGroupName $resourceGroup -Location $location `
  -Name "mypublicdns$(Get-Random)" -AllocationMethod Static -IdleTimeoutInMinutes 4
$nsgRuleRDP = New-AzNetworkSecurityRuleConfig -Name myNetworkSecurityGroupRuleRDP  -Protocol Tcp `
  -Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
  -DestinationPortRange 3389 -Access Allow
$nsg = New-AzNetworkSecurityGroup -ResourceGroupName $resourceGroup -Location $location `
  -Name myNetworkSecurityGroup -SecurityRules $nsgRuleRDP
$nic = New-AzNetworkInterface -Name myNic -ResourceGroupName $resourceGroup -Location $location `
  -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -NetworkSecurityGroupId $nsg.Id

# Create a virtual machine configuration and set this to be an Azure Spot Virtual Machine

$vmConfig = New-AzVMConfig -VMName $vmName -VMSize Standard_D1 -Priority "Spot" -MaxPrice -1 -EvictionPolicy Deallocate | `
Set-AzVMOperatingSystem -Windows -ComputerName $vmName -Credential $cred | `
Set-AzVMSourceImage -PublisherName MicrosoftWindowsServer -Offer WindowsServer -Skus 2016-Datacenter -Version latest | `
Add-AzVMNetworkInterface -Id $nic.Id

New-AzVM -ResourceGroupName $resourceGroup -Location $location -VM $vmConfig
```

Nachdem die VM erstellt wurde, können Sie den maximalen Preis für alle VMs in der Ressourcengruppe abfragen.

```azurepowershell-interactive
Get-AzVM -ResourceGroupName $resourceGroup | `
   Select-Object Name,@{Name="maxPrice"; Expression={$_.BillingProfile.MaxPrice}}
```

## <a name="simulate-an-eviction"></a>Simulieren einer Entfernung

Sie können die Entfernung einer Azure-Spot-VM [simulieren](/rest/api/compute/virtualmachines/simulateeviction), um zu testen, wie gut die Anwendung auf einen plötzlichen Entfernungsvorgang reagiert. 

Ersetzen Sie Folgendes durch Ihre Informationen: 

- `subscriptionId`
- `resourceGroupName`
- `vmName`


```rest
POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{vmName}/simulateEviction?api-version=2020-06-01
```

`Response Code: 204` bedeutet, dass der simulierte Entfernungs Vorgang erfolgreich war. 

## <a name="next-steps"></a>Nächste Schritte

Sie können eine Azure-Spot-VM auch über die [Azure-Befehlszeilenschnittstelle](../linux/spot-cli.md), das [Portal](../spot-portal.md) oder eine [Vorlage](../linux/spot-template.md) erstellen.

Fragen Sie Preisinformationen ab, indem Sie die [Azure-Einzelhandelspreis-API](/rest/api/cost-management/retail-prices/azure-retail-prices) verwenden, um Informationen zu Preisen von Azure-Spot-VMs zu erhalten. `meterName` und `skuName` enthalten beide `Spot`.

Informationen zu eventuell auftretenden Fehlern finden Sie unter [Fehlercodes](../error-codes-spot.md).
