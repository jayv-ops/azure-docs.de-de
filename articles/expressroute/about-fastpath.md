---
title: Informationen zu Azure ExpressRoute FastPath
description: Hier erfahren Sie, wie Sie mit Azure ExpressRoute FastPath Netzwerkdatenverkehr senden, indem Sie das Gateway umgehen.
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: conceptual
ms.date: 03/25/2020
ms.author: duau
ms.openlocfilehash: ba23319c35aed1d09da652e6f84b60e5f8e9495e
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101740884"
---
# <a name="about-expressroute-fastpath"></a>Informationen zu ExpressRoute FastPath

Das virtuelle Netzwerkgateway ExpressRoute wurde entwickelt, um Netzwerkrouten auszutauschen und den Netzwerkdatenverkehr zu steuern. FastPath wurde entwickelt, um die Datenpfadleistung zwischen Ihrem lokalen und dem virtuellen Netzwerk zu verbessern. Wenn aktiviert, sendet FastPath den Netzwerkdatenverkehr direkt an virtuelle Computer im virtuellen Netzwerk und umgeht dabei das Gateway.

## <a name="requirements"></a>Anforderungen

### <a name="circuits"></a>Leitungen

FastPath ist unter allen ExpressRoute-Verbindungen verfügbar.

### <a name="gateways"></a>Gateways

FastPath erfordert weiterhin die Erstellung eines virtuellen Netzwerkgateways für den Austausch von Routen zwischen virtuellem und lokalem Netzwerk. Weitere Informationen zu Gateways für virtuelle Netzwerk und ExpressRoute, einschließlich Leistungsinformationen und Gateway-SKUs, finden Sie unter [Informationen zu ExpressRoute-Gateways für virtuelle Netzwerke](expressroute-about-virtual-network-gateways.md).

Zum Konfigurieren von FastPath muss das Gateway für virtuelle Netzwerke eines der folgenden Typen sein:

* Höchstleistung
* ErGw3AZ

> [!IMPORTANT]
> Wenn Sie FastPath mit dem IPv6-basierten privaten Peering über ExpressRoute verwenden möchten, wählen Sie unbedingt ErGw3AZ für die **SKU** aus.
> 
>

## <a name="limitations"></a>Einschränkungen

FastPath unterstützt zwar die meisten Konfigurationen, aber nicht die folgenden Features:

* UDR auf dem Gatewaysubnetz: Wenn Sie ein UDR auf das Gatewaysubnetz Ihres virtuellen Netzwerks anwenden, wird der Netzwerkdatenverkehr aus Ihrem lokalen Netzwerk weiterhin an das virtuelle Netzwerkgateway gesendet.

* VNet-Peering: Wenn andere virtuelle Netzwerke per Peering mit dem Netzwerk verbunden sind, das mit ExpressRoute verbunden ist, wird der Netzwerkdatenverkehr von Ihrem lokalen Netzwerk zu den anderen virtuellen Netzwerken (d. h. den sogenannten virtuellen „Spoke“-Netzwerken) weiterhin an das virtuelle Netzwerkgateway gesendet. Die Problemumgehung besteht darin, alle virtuellen Netzwerke direkt mit der ExpressRoute-Verbindung zu verbinden.

* Basic-Load Balancer: Wenn Sie einen internen Basic-Load Balancer in Ihrem virtuellen Netzwerk bereitstellen oder der Azure-PaaS-Dienst, den Sie in Ihrem virtuellen Netzwerk bereitstellen, einen internen Basic-Load Balancer verwendet, wird der Netzwerkdatenverkehr von Ihrem lokalen Netzwerk zu den virtuellen IP-Adressen, die auf dem Basic-Load Balancer gehostet werden, an das Gateway des virtuellen Netzwerks gesendet. Die Lösung besteht darin, den Basic-Load Balancer auf einen [Standard-Load Balancer](../load-balancer/load-balancer-overview.md) zu aktualisieren.

* Private Link: Wenn Sie von Ihrem lokalen Netzwerk eine Verbindung mit einem [privaten Endpunkt](../private-link/private-link-overview.md) in Ihrem virtuellen Netzwerk herstellen, erfolgt die Verbindung über das Gateway für virtuelle Netzwerke.
 
## <a name="next-steps"></a>Nächste Schritte

Informationen zum Aktivieren von FastPath finden Sie unter [Verbinden eines virtuellen Netzwerks mit einer ExpressRoute-Verbindung](expressroute-howto-linkvnet-arm.md#configure-expressroute-fastpath).
