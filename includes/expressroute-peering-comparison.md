---
author: cherylmc
ms.date: 12/13/2019
ms.service: expressroute
ms.topic: include
ms.author: cherylmc
ms.openlocfilehash: 7aee201b0419b65c7ea8ddcb6f2d42ffaff4711e
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101750581"
---
|  | **Privates Peering** | **Microsoft-Peering** |  **Öffentliches Peering** (für neue Verbindungen veraltet) |
| --- | --- | --- | --- |
| **Max. Anzahl von unterstützten Präfixen pro Peering** |Standardmäßig 4.000, bei ExpressRoute Premium 10.000 |200 |200 |
| **Unterstützte IP-Adressbereiche** |Jede gültige IPv4-Adresse innerhalb des WAN |Öffentliche IP-Adressen in Ihrem Besitz oder im Besitz des Konnektivitätsanbieters |Öffentliche IP-Adressen in Ihrem Besitz oder im Besitz des Konnektivitätsanbieters |
| **Anforderungen für AS-Nummern** |Private und öffentliche AS-Nummern. Wenn Sie eine öffentliche AS-Nummer verwenden möchten, muss diese in Ihrem Besitz sein. |Private und öffentliche AS-Nummern. Sie müssen allerdings den Besitz öffentlicher IP-Adressen nachweisen. |Private und öffentliche AS-Nummern. Sie müssen allerdings den Besitz öffentlicher IP-Adressen nachweisen. |
| **Unterstützte IP-Protokolle**| IPv4, IPv6 (Vorschau) |  IPv4, IPv6 | IPv4 |
| **IP-Adressen der Routingschnittstelle** |RFC1918 und öffentliche IP-Adressen |In Routingregistrierungen für Sie registrierte öffentliche IP-Adressen. |In Routingregistrierungen für Sie registrierte öffentliche IP-Adressen. |
| **MD5-Hash-Unterstützung** |Ja |Ja |Ja |