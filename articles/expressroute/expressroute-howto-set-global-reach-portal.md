---
title: 'Azure ExpressRoute: Konfigurieren von Global Reach mit dem Azure-Portal'
description: Dieser Artikel hilft Ihnen, mithilfe des Azure-Portals ExpressRoute-Leitungen miteinander zu verknüpfen, um ein privates Netzwerk zwischen Ihren lokalen Netzwerken aufzubauen und Global Reach zu aktivieren.
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: how-to
ms.date: 03/05/2021
ms.author: duau
ms.openlocfilehash: 336bd4aaf881b7315921ef374c92a2ac95ff3c8c
ms.sourcegitcommit: ba676927b1a8acd7c30708144e201f63ce89021d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2021
ms.locfileid: "102431314"
---
# <a name="configure-expressroute-global-reach-using-the-azure-portal"></a>Konfigurieren von ExpressRoute Global Reach über das Azure-Portal

Dieser Artikel unterstützt Sie bei der Konfiguration von ExpressRoute Global Reach mit PowerShell. Weitere Informationen finden Sie unter [ExpressRoute Global Reach (Vorschau)](expressroute-global-reach.md).

 ## <a name="before-you-begin"></a>Voraussetzungen

Vergewissern Sie sich, dass folgende Kriterien erfüllt sind, bevor Sie mit der Konfiguration beginnen:

* Sie sind mit den [Workflows](expressroute-workflows.md) zur Bereitstellung von ExpressRoute-Leitungen vertraut.
* Ihre ExpressRoute-Leitungen weisen den Status „Bereitgestellt“ auf.
* Für Ihre ExpressRoute-Leitungen ist privates Azure-Peering konfiguriert.
* Wenn Sie PowerShell lokal ausführen möchten, stellen Sie sicher, dass die neueste Version von Azure PowerShell auf Ihrem Computer installiert ist.

## <a name="identify-circuits"></a>Identifizieren von Leitungen

1. Navigieren Sie in einem Browser zum [Azure-Portal](https://portal.azure.com) , und melden Sie sich mit Ihrem Azure-Konto an.

2. Identifizieren Sie die ExpressRoute-Leitungen, die Sie verwenden möchten. Sie können ExpressRoute Global Reach zwischen dem privaten Peering von zwei beliebigen ExpressRoute-Leitungen aktivieren, solange sich diese in den unterstützten Ländern/Regionen befinden. Die Leitungen müssen an verschiedenen Peeringstandorten erstellt worden sein. 

   * Wenn beide Leitungen Ihrem Abonnement zugewiesen sind, können Sie eine der beiden Leitungen auswählen, um die Konfiguration in den folgenden Abschnitten auszuführen.
   * Wenn sich die beiden Leitungen in unterschiedlichen Azure-Abonnements befinden, benötigen Sie die Autorisierung eines der Azure-Abonnements. Den Autorisierungsschlüssel übergeben Sie, wenn Sie den Konfigurationsbefehl im anderen Azure-Abonnement ausführen.

    :::image type="content" source="./media/expressroute-howto-set-global-reach-portal/expressroute-circuit-global-reach-list.png" alt-text="Screenshot einer Liste von ExpressRoute-Leitungen.":::

## <a name="enable-connectivity"></a>Herstellen von Konnektivität

Aktivieren Sie die Konnektivität zwischen Ihren lokalen Netzwerken. Es gibt getrennte Anweisungen für Leitungen, die sich im gleichen Azure-Abonnement befinden, und für Leitungen in unterschiedlichen Abonnements.

### <a name="expressroute-circuits-in-the-same-azure-subscription"></a>ExpressRoute-Leitungen im gleichen Azure-Abonnement

1. Wählen Sie als Peeringkonfiguration **Azure, privat** aus. 

    :::image type="content" source="./media/expressroute-howto-set-global-reach-portal/expressroute-circuit-private-peering.png" alt-text="Screenshot der Übersichtsseite von ExpressRoute-Leitungen.":::

1. Wählen Sie **Add Global Reach** (Global Reach hinzufügen) aus, um die Konfigurationsseite *Add Global Reach* zu öffnen.

    :::image type="content" source="./media/expressroute-howto-set-global-reach-portal/private-peering-enable-global-reach.png" alt-text="Aktivieren von Global Reach über das private Peering":::

1. Geben Sie auf der Konfigurationsseite *Global Reach hinzufügen* einen Namen für diese Konfiguration an. Wählen Sie die *ExpressRoute-Leitung* aus, mit der Sie diese Leitung verbinden möchten, und geben Sie als *Global Reach-Subnetz* den Wert **/29 IPv4** ein. Wir verwenden IP-Adressen in diesem Subnetz, um eine Verbindung zwischen den beiden ExpressRoute-Leitungen herzustellen. Die Adressen in diesem Subnetz dürfen nicht in Ihren virtuellen Azure-Netzwerken oder in Ihrem lokalen Netzwerk verwendet werden. Wählen Sie **Hinzufügen** aus, um die Leitung der Konfiguration für das private Peering hinzuzufügen.

    :::image type="content" source="./media/expressroute-howto-set-global-reach-portal/add-global-reach-configuration.png" alt-text="Screenshot: Hinzufügen von Global Reach im privaten Peering.":::

1. Wählen Sie **Speichern** aus, um die Global Reach-Konfiguration abzuschließen. Wenn der Vorgang abgeschlossen ist, stellen die beiden ExpressRoute-Leitungen Konnektivität zwischen Ihren beiden lokalen Netzwerken bereit.

    :::image type="content" source="./media/expressroute-howto-set-global-reach-portal/save-private-peering-configuration.png" alt-text="Screenshot: Speichern von Konfigurationen für das private Peering.":::

### <a name="expressroute-circuits-in-different-azure-subscriptions"></a>ExpressRoute-Leitungen in verschiedenen Azure-Abonnements

Wenn sich die beiden Leitungen nicht im gleichen Azure-Abonnement befinden, benötigen Sie eine Autorisierung. In der folgenden Konfiguration wird die Autorisierung aus dem Abonnement von Leitung 2 generiert. Der Autorisierungsschlüssel wird dann an Leitung 1 übergeben.

1. Generieren Sie einen Autorisierungsschlüssel.

   :::image type="content" source="./media/expressroute-howto-set-global-reach-portal/create-authorization-expressroute-circuit.png" alt-text="Screenshot: Generieren eines Autorisierungsschlüssels."::: 

   Notieren Sie sich die Ressourcen-ID von Leitung 2 sowie den Autorisierungsschlüssel.

1. Wählen Sie als Peeringkonfiguration **Azure, privat** aus. 

    :::image type="content" source="./media/expressroute-howto-set-global-reach-portal/expressroute-circuit-private-peering.png" alt-text="Screenshot des privaten Peerings auf der Übersichtsseite.":::

1. Wählen Sie **Add Global Reach** (Global Reach hinzufügen) aus, um die Konfigurationsseite *Add Global Reach* zu öffnen.

    :::image type="content" source="./media/expressroute-howto-set-global-reach-portal/private-peering-enable-global-reach.png" alt-text="Screenshot: Hinzufügen von Global Reach im privaten Peering.":::

1. Geben Sie auf der Konfigurationsseite *Global Reach hinzufügen* einen Namen für diese Konfiguration an. Aktivieren Sie das Kontrollkästchen **Autorisierung einlösen**. Geben Sie den **Autorisierungsschlüssel** und die **ExpressRoute-Leitungs-ID** ein, die in Schritt 1 generiert und abgerufen wurden. Geben Sie als *Global Reach-Subnetz* den Wert **/29 IPv4** an. Wir verwenden IP-Adressen in diesem Subnetz, um eine Verbindung zwischen den beiden ExpressRoute-Leitungen herzustellen. Die Adressen in diesem Subnetz dürfen nicht in Ihren virtuellen Azure-Netzwerken oder in Ihrem lokalen Netzwerk verwendet werden. Wählen Sie **Hinzufügen** aus, um die Leitung der Konfiguration für das private Peering hinzuzufügen.

    :::image type="content" source="./media/expressroute-howto-set-global-reach-portal/add-global-reach-configuration-with-authorization.png" alt-text="Screenshot von „Add Global Reach“ mit Autorisierungsschlüssel.":::

1. Wählen Sie **Speichern** aus, um die Global Reach-Konfiguration abzuschließen. Wenn der Vorgang abgeschlossen ist, stellen die beiden ExpressRoute-Leitungen Konnektivität zwischen Ihren beiden lokalen Netzwerken bereit.

    :::image type="content" source="./media/expressroute-howto-set-global-reach-portal/save-private-peering-configuration.png" alt-text="Screenshot: Speichern der Konfiguration für das private Peering mit Global Reach.":::

## <a name="verify-the-configuration"></a>Überprüfen der Konfiguration

Überprüfen Sie die Global Reach-Konfiguration, indem Sie unter der Konfiguration der ExpressRoute-Leitung *Privates Peering* auswählen. Eine ordnungsgemäße Konfiguration sollte wie folgt aussehen:

:::image type="content" source="./media/expressroute-howto-set-global-reach-portal/verify-global-reach-configuration.png" alt-text="Screenshot von konfiguriertem Global Reach.":::

## <a name="disable-connectivity"></a>Deaktivieren der Konnektivität

Um die Verbindung einer einzelnen Leitung zu trennen, wählen Sie die Schaltfläche zum Löschen (Papierkorb) neben dem *Global Reach-Namen* aus. Wählen Sie anschließend **Speichern** aus, um den Vorgang abzuschließen.

:::image type="content" source="./media/expressroute-howto-set-global-reach-portal/disable-global-reach-configuration.png" alt-text="Screenshot, der zeigt, wie Global Reach deaktiviert wird.":::

Nach Abschluss des Vorgangs besteht zwischen Ihren lokalen Netzwerken keine Verbindung mehr über Ihre ExpressRoute-Leitungen.

## <a name="next-steps"></a>Nächste Schritte
- [Weitere Informationen zu ExpressRoute Global Reach](expressroute-global-reach.md)
- [Überprüfen der ExpressRoute-Konnektivität](expressroute-troubleshooting-expressroute-overview.md)
- [Verknüpfen einer ExpressRoute-Leitung mit einem virtuellen Azure-Netzwerk](expressroute-howto-linkvnet-arm.md)
