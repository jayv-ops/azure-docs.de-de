---
title: Informationen zur Azure Route Server-Unterstützung (Vorschau) für ExpressRoute und Azure-VPN
description: Hier erfahren Sie, wie Azure Route Server mit ExpressRoute- und Azure-VPN-Gateways interagiert.
services: route-server
author: duongau
ms.service: route-server
ms.topic: conceptual
ms.date: 03/02/2021
ms.author: duau
ms.openlocfilehash: 6e588c7c0381c6825bcf75cbbe28a1dd6b865940
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2021
ms.locfileid: "101678811"
---
# <a name="about-azure-route-server-preview-support-for-expressroute-and-azure-vpn"></a>Informationen zur Azure Route Server-Unterstützung (Vorschau) für ExpressRoute und Azure-VPN

Azure Route Server unterstützt nicht nur virtuelle Netzwerkgeräte (NVAs) von Drittanbietern, die unter Azure ausgeführt werden, sondern kann auch nahtlos in ExpressRoute- und Azure-VPN-Gateways integriert werden. Sie müssen das BGP-Peering zwischen dem Gateway und Azure Route Server weder konfigurieren noch verwalten. Sie können den Routenaustausch zwischen dem Gateway und Azure Route Server mit einer einfachen [Konfigurationsänderung](quickstart-configure-route-server-powershell.md#route-exchange) aktivieren.

> [!IMPORTANT]
> Azure Route Server (Vorschau) befindet sich derzeit in der öffentlichen Vorschauphase.
> Diese Vorschauversion wird ohne Vereinbarung zum Servicelevel bereitgestellt und ist nicht für Produktionsworkloads vorgesehen. Manche Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar.
> Weitere Informationen finden Sie unter [Zusätzliche Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="how-does-it-work"></a>Wie funktioniert dies?

Wenn Sie Azure Route Server zusammen mit einem ExpressRoute-Gateway und einem NVA in einem virtuellen Netzwerk bereitstellen, werden die von Azure Route Server über das NVA und das ExpressRoute-Gateway empfangenen Routen standardmäßig nicht untereinander weitergegeben. Sobald Sie den Routenaustausch aktivieren, erlernen ExpressRoute und das NVA die Routen des jeweils anderen.

Ein Beispiel sehen Sie im folgenden Diagramm:

* Das SDWAN-Gerät empfängt von Azure Route Server die Route von „Lokal 2“, das mit ExpressRoute verbunden ist, zusammen mit der Route des virtuellen Netzwerks.

* Das ExpressRoute-Gateway empfängt die Route von „Lokal 1“, das mit dem SDWAN-Gerät verbunden ist, zusammen mit der Route des virtuellen Netzwerks von Azure Route Server.

    ![Diagramm der mit Route Server konfigurierten ExpressRoute](./media/expressroute-vpn-support/expressroute-with-route-server.png)

Sie können das SDWAN-Gerät auch durch das Azure-VPN-Gateway ersetzen. Da das Azure-VPN-Gateway und ExpressRoute vollständig verwaltet werden, müssen Sie nur den Routenaustausch für die beiden lokalen Netzwerke aktivieren, damit diese miteinander kommunizieren.

> [!IMPORTANT] 
> Das Azure-VPN-Gateway muss im [**Aktiv/Aktiv**](../vpn-gateway/vpn-gateway-activeactive-rm-powershell.md)-Modus konfiguriert sein.
>

![Diagramm der mit Route Server konfigurierten ExpressRoute und des VPN-Gateways](./media/expressroute-vpn-support/expressroute-and-vpn-with-route-server.png)

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr über [Azure Route Server](route-server-faq.md).
- Informieren Sie sich, wie Sie [Azure Route Server konfigurieren](quickstart-configure-route-server-powershell.md).
- Erfahren Sie mehr über die [Koexistenz von Azure ExpressRoute und Azure-VPN](../expressroute/expressroute-howto-coexist-resource-manager.md).
