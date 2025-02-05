---
title: Konfigurieren der Front-End-IP-Adresse für Azure Application Gateway
description: In diesem Artikel wird das Konfigurieren der Front-End-IP-Adresse für Azure Application Gateway beschrieben.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: conceptual
ms.date: 09/09/2020
ms.author: surmb
ms.openlocfilehash: eff63510f70dd7b4cdd522cc5a2a68096cda7166
ms.sourcegitcommit: 7edadd4bf8f354abca0b253b3af98836212edd93
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2021
ms.locfileid: "102548715"
---
# <a name="application-gateway-front-end-ip-address-configuration"></a>Konfigurieren der Front-End-IP-Adresse für Application Gateway

Sie können das Application Gateway mit einer öffentlichen IP-Adresse, mit einer privaten IP-Adresse oder mit beidem konfigurieren. Eine öffentliche IP-Adresse ist erforderlich, wenn Sie ein Back-End hosten, auf das Clients über eine für den Internetzugriff verfügbare virtuelle IP-Adresse (VIP) über das Internet zugreifen müssen.

## <a name="public-and-private-ip-address-support"></a>Unterstützung für öffentliche und private IP-Adressen

Application Gateway v2 unterstützt derzeit nicht den rein privaten IP-Modus. Es unterstützt die folgenden Kombinationen:

* Private IP-Adresse und öffentliche IP-Adresse
* Nur öffentliche IP-Adresse

Weitere Informationen finden Sie in den [häufig gestellten Fragen zu Application Gateway](application-gateway-faq.yml#how-do-i-use-application-gateway-v2-with-only-private-frontend-ip-address).


Eine öffentliche IP-Adresse ist nicht für einen internen Endpunkt erforderlich, auf den kein Zugriff über das Internet erfolgt. Dieser wird als *Interner Lastenausgleich*-Endpunkt (Internal Load-Balancer, ILB) oder private Front-End-IP bezeichnet. Ein ILB-Application Gateway ist für interne Branchenanwendungen nützlich, die nicht für das Internet verfügbar gemacht werden. Es ist auch hilfreich für Dienste und Ebenen in einer Anwendung mit mehreren Ebenen innerhalb einer Sicherheitsgrenze, die nicht für das Internet verfügbar gemacht werden, aber eine Roundrobin-Lastverteilung, Sitzungspersistenz oder TLS-Beendigung erfordern.

Es werden nur jeweils eine öffentliche und eine private IP-Adresse unterstützt. Sie wählen die Front-End-IP beim Erstellen des Application Gateways aus.

- Für eine öffentliche IP-Adresse können Sie eine neue öffentliche IP-Adresse erstellen oder eine vorhandene öffentliche IP-Adresse am gleichen Standort wie das Anwendungsgateway verwenden. Weitere Informationen finden Sie unter [Statische und dynamische öffentliche IP-Adresse im Vergleich](./application-gateway-components.md#static-versus-dynamic-public-ip-address).

- Für eine private IP-Adresse können Sie eine private IP-Adresse aus dem Subnetz angeben, in dem das Anwendungsgateway erstellt wird. Wenn Sie keine angeben, wird automatisch eine beliebige IP-Adresse aus dem Subnetz ausgewählt. Der von Ihnen ausgewählte Typ der IP-Adresse (statisch oder dynamisch) kann später nicht mehr geändert werden. Weitere Informationen finden Sie unter [Erstellen eines Application Gateways mit einem internen Lastenausgleich (ILB)](./application-gateway-ilb-arm.md).

Eine Front-End-IP-Adresse ist einem *Listener* zugeordnet, der auf über die Front-End-IP eingehende Anforderungen prüft.

## <a name="next-steps"></a>Nächste Schritte

- [Erfahren Sie mehr über die Listenerkonfiguration.](configuration-listeners.md)
