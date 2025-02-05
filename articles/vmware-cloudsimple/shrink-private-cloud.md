---
title: Verkleinern einer privaten Cloud für Azure VMware Solution by CloudSimple
description: Hier erfahren Sie, wie Sie eine private Cloud in CloudSimple dynamisch verkleinern, indem Sie einen Knoten aus einem vorhandenen vSphere-Cluster oder einen gesamten Cluster entfernen.
author: Ajayan1008
ms.author: v-hborys
ms.date: 07/01/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: a99b9b56f17b78a98f37d47dcefab26dd9c859de
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "97899132"
---
# <a name="shrink-a-cloudsimple-private-cloud"></a>Verkleinern einer privaten Cloud von CloudSimple

CloudSimple bietet die Flexibilität, eine private Cloud dynamisch zu verkleinern.  Eine private Cloud besteht aus einem oder mehreren vSphere-Clustern. Jeder Cluster kann 3 bis 16 Knoten enthalten. Wenn Sie eine private Cloud verkleinern, entfernen Sie einen Knoten aus dem bestehenden Cluster oder löschen einen ganzen Cluster. 

## <a name="before-you-begin"></a>Voraussetzungen

Folgende Bedingungen müssen für das Verkleinern einer privaten Cloud erfüllt sein.  Verwaltungscluster (erster Cluster), die beim Erstellen einer privaten Cloud angelegt wurden, können nicht gelöscht werden.

* Ein vSphere-Cluster muss aus drei Knoten bestehen.  Ein Cluster mit nur drei Knoten kann nicht verkleinert werden.
* Der gesamte Speicherverbrauch darf die Gesamtkapazität nach dem Verkleinern des Clusters nicht überschreiten.
* Überprüfen Sie, ob die DRS-Regeln (Distributed Resource Scheduler) die vMotion-Ausführung eines virtuellen Computers verhindern.  Deaktivieren oder löschen Sie die Regeln, wenn Regeln vorhanden sind.  DRS-Regeln schließen virtuelle Computer zum Hosten von Affinitätsregeln ein.

## <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter [https://portal.azure.com](https://portal.azure.com) beim Azure-Portal an.

## <a name="shrink-a-private-cloud"></a>Verkleinern einer privaten Cloud

1. [Rufen Sie das CloudSimple-Portal auf](access-cloudsimple-portal.md).

2. Öffnen Sie die Seite **Ressourcen**.

3. Klicken Sie auf die private Cloud, die Sie verkleinern möchten.

4. Klicken Sie auf der Übersichtsseite auf **Verkleinern**.

    ![Verkleinern einer privaten Cloud](media/shrink-private-cloud.png)

5. Wählen Sie den Cluster aus, den Sie verkleinern oder löschen möchten. 

    ![Private Cloud verkleinern – Cluster auswählen](media/shrink-private-cloud-select-cluster.png)

6. Wählen Sie **Einen Knoten entfernen** oder **Gesamten Cluster löschen** aus. 

7. Überprüfen der Clusterkapazität

8. Klicken Sie auf **Senden**, um die private Cloud zu verkleinern.

Die Verkleinerung der privaten Cloud wird gestartet.  Sie können den Status unter „Aufgaben“ verfolgen.  Der Verkleinerungsprozess kann je nach den Daten, die mit vSAN erneut synchronisiert werden müssen, einige Stunden dauern.

> [!NOTE]
> 1. Wenn Sie eine private Cloud verkleinern, indem Sie den letzten bzw. den einzigen Cluster im Rechenzentrum löschen, wird das Rechenzentrum nicht gelöscht.
> 2. Wenn eine DRS-Regelverletzung auftritt, wird der Knoten nicht aus dem Cluster entfernt und in der Aufgabenbeschreibung wird angezeigt, dass durch das Entfernen eines Knotens die DRS-Regeln im Cluster verletzt werden.    


## <a name="next-steps"></a>Nächste Schritte

* [Nutzen von virtuellen VMware-Computern in Azure](quickstart-create-vmware-virtual-machine.md)
* Weitere Informationen zu [privaten Clouds](cloudsimple-private-cloud.md)
