---
title: Verwalten von Ressourcen, die während des VM-Verschiebevorgangs in Azure Resource Mover erstellt werden
description: Erfahren Sie, wie Sie Ressourcen verwalten, die während des VM-Verschiebevorgangs in Azure Resource Mover erstellt werden.
manager: evansma
author: rayne-wiselman
ms.service: resource-move
ms.topic: how-to
ms.date: 09/10/2020
ms.author: raynew
ms.openlocfilehash: d3c4c4e86e2461ea1d05af284e724a5a2991f040
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101727038"
---
# <a name="manage-resources-created-for-the-vm-move"></a>Verwalten von für die VM-Verschiebung erstellten Ressourcen

In diesem Artikel wird beschrieben, wie Ressourcen verwaltet werden, die explizit durch [Azure Resource Mover](overview.md) erstellt werden, um den Vorgang der VM-Verschiebung zu ermöglichen. 

Nach dem Verschieben von VMs zwischen verschiedenen Regionen gibt es eine Reihe von Ressourcen, die von Resource Mover erstellt wurden und manuell bereinigt werden sollten.

## <a name="delete-resources-created-for-vm-move"></a>Löschen von für die VM-Verschiebung erstellten Ressourcen

Löschen Sie die Sammlung für die VM-Verschiebung und die erstellten Site Recovery-Ressourcen manuell.

1. Überprüfen Sie die Ressourcen in der Ressourcengruppe ```ResourceMoverRG-<sourceregion>-<target-region>-<metadataRegionShortName>```.
2. Überprüfen Sie, ob die VMs und alle anderen Quellressourcen in der Verschiebungssammlung verschoben/gelöscht wurden. Dadurch wird sichergestellt, dass sie nicht von ausstehenden Ressourcen verwendet werden.
2. Löschen Sie diese Ressourcen.

    - Der Name der Sammlung für die Verschiebung ist ```movecollection-<sourceregion>-<target-region>-<metadata-region>```.
    - Der Name des Cachespeichers lautet ```resmovecache<guid>```.
    - Der Tresorname ist ```ResourceMove-<sourceregion>-<target-region>-GUID```.

## <a name="next-steps"></a>Nächste Schritte

Versuchen Sie, eine VM mit Resource Mover in eine andere Region zu [verschieben](tutorial-move-region-virtual-machines.md).
