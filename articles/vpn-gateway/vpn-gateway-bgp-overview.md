---
title: 'BGP und Azure VPN Gateway: Übersicht'
description: Erfahren Sie mehr über das Border Gateway Protocol (BGP) in Azure-VPN, das Standardinternetprotokoll zum Austauschen von Routing- und Erreichbarkeitsinformationen zwischen Netzwerken.
services: vpn-gateway
author: yushwang
ms.service: vpn-gateway
ms.topic: article
ms.date: 09/02/2020
ms.author: yushwang
ms.openlocfilehash: 464d00cbeddbacd617b1d2c88f9e5f68cc5d996e
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "89400873"
---
# <a name="about-bgp-with-azure-vpn-gateway"></a>Übersicht über BGP mit Azure VPN Gateway
Dieser Artikel enthält eine Übersicht über die Unterstützung von BGP (Border Gateway Protocol) in Azure VPN Gateway.

BGP ist das standardmäßige Routingprotokoll, mit dem im Internet üblicherweise Routing- und Erreichbarkeitsinformationen zwischen mehren Netzwerken ausgetauscht werden. Im Kontext virtueller Azure-Netzwerke können Azure-VPN-Gateways und Ihre lokalen VPN-Geräte (so genannte BGP-Peers oder Nachbarn) über BGP Routen austauschen, die beide Gateways über die Verfügbarkeit und Erreichbarkeit der Präfixe informieren, die die beteiligten Gateways oder Router durchlaufen. BGP ermöglicht auch Transitrouting zwischen mehreren Netzwerken. Hierzu werden von einem BGP-Gateway ermittelte Routen eines BGP-Peers an alle anderen BGP-Peers weitergegeben. 

## <a name="why-use-bgp"></a><a name="why"></a>Argumente für die Verwendung von BGP
BGP ist ein optionales Feature für routenbasierte Azure-VPN-Gateways. Vergewissern Sie sich vor dem Aktivieren des Features, dass BGP von Ihren lokalen VPN-Geräten unterstützt wird. Azure-VPN-Gateways und Ihre lokalen VPN-Geräte können auch weiterhin ohne BGP verwendet werden. Sie haben somit die Wahl zwischen statischen Routen (ohne BGP) *oder* dynamischem Routing mit BGP zwischen Ihren Netzwerken und Azure.

BGP bietet einige Vorteile und neue Möglichkeiten:

### <a name="support-automatic-and-flexible-prefix-updates"></a><a name="prefix"></a>Unterstützung automatischer und flexibler Präfixupdates
Mit BGP müssen Sie lediglich ein Mindestpräfix für einen bestimmten BGP-Peer über den IPsec-S2S-VPN-Tunnel deklarieren. Das Präfix muss mindestens die Größe eines Hostpräfix (/32) der BGP-Peer-IP-Adresse Ihres lokalen VPN-Geräts besitzen. Sie können steuern, welche lokalen Netzwerkpräfixe sie gegenüber Azure ankündigen möchten, um Ihrem virtuellen Azure-Netzwerk den Zugriff darauf zu ermöglichen.

Sie können auch größere Präfixe ankündigen, die auch einige Ihrer VNET-Adresspräfixe umfassen können – etwa einen großen privaten IP-Adressraum (beispielsweise 10.0.0.0/8). Beachten Sie jedoch, dass die Präfixe keinem Ihrer VNET-Präfixe entsprechen dürfen. Mit Ihren VNET-Präfixen identische Routen werden abgelehnt.

### <a name="support-multiple-tunnels-between-a-vnet-and-an-on-premises-site-with-automatic-failover-based-on-bgp"></a><a name="multitunnel"></a>Unterstützung mehrerer Tunnel zwischen einem VNET und einem lokalen Standort mit automatischem Failover auf der Grundlage von BGP
Sie können mehrere Verbindungen zwischen Ihrem Azure-VNET und Ihren lokalen VPN-Geräten am gleichen Standort herstellen. Dies ermöglicht die Einrichtung mehrerer Tunnel (Pfade) zwischen den beiden Netzwerken in einer Aktiv/Aktiv-Konfiguration. Wird die Verbindung mit einem der Tunnel getrennt, werden die entsprechenden Routen per BGP zurückgezogen, und der Datenverkehr wird automatisch auf die verbleibenden Tunnel verlagert.

Das folgende Diagramm zeigt ein einfaches Beispiel für dieses hochverfügbare Setup:

![Mehrere aktive Pfade](./media/vpn-gateway-bgp-overview/multiple-active-tunnels.png)

### <a name="support-transit-routing-between-your-on-premises-networks-and-multiple-azure-vnets"></a><a name="transitrouting"></a>Unterstützung von Transitrouting zwischen Ihren lokalen Netzwerken und mehreren Azure-VNETs
Mit BGP können mehrere Gateways Präfixe aus unterschiedlichen Netzwerken ermitteln und weitergeben (sowohl bei direkter als auch bei indirekter Verbindung). Dies ermöglicht die Verwendung von Transitrouting mit Azure-VPN-Gateways zwischen Ihren lokalen Standorten oder zwischen mehreren virtuellen Azure-Netzwerken.

Das folgende Diagramm veranschaulicht ein Beispiel für eine Mehrfachhop-Topologie mit mehreren Pfaden und Transitrouting von Datenverkehr zwischen den beiden lokalen Netzwerken über Azure-VPN-Gateways innerhalb der Microsoft-Netzwerke:

![Mehrfachhop-Übertragung](./media/vpn-gateway-bgp-overview/full-mesh-transit.png)

## <a name="bgp-faq"></a><a name="faq"></a>BGP – Häufig gestellte Fragen
[!INCLUDE [vpn-gateway-faq-bgp-include](../../includes/vpn-gateway-faq-bgp-include.md)]

## <a name="next-steps"></a>Nächste Schritte
Schritte zum Konfigurieren von BGP für standortübergreifende Verbindungen und VNET-zu-VNET-Verbindungen finden Sie unter [Konfigurieren von BGP auf Azure VPN Gateways mithilfe von Azure Resource Manager und PowerShell](vpn-gateway-bgp-resource-manager-ps.md) .

