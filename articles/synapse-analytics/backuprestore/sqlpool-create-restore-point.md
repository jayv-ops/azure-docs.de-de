---
title: Erstellen eines benutzerdefinierten Wiederherstellungspunkts für einen dedizierten SQL-Pool
description: Erfahren Sie, wie Sie über das Azure-Portal einen benutzerdefinierten Wiederherstellungspunkt für einen dedizierten SQL-Pool in Azure Synapse Analytics erstellen.
services: synapse-analytics
author: joannapea
manager: igorstan
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: sql
ms.date: 10/29/2020
ms.author: joanpo
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: 21fd20100095040fda9f72b00e17147ff560fbca
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "94579537"
---
# <a name="user-defined-restore-points"></a>Benutzerdefinierte Wiederherstellungspunkte

In diesem Artikel erfahren Sie, wie Sie über das Azure-Portal einen neuen benutzerdefinierten Wiederherstellungspunkt für einen dedizierten SQL-Pool in Azure Synapse Analytics erstellen.

## <a name="create-user-defined-restore-points-through-the-azure-portal"></a>Erstellen benutzerdefinierter Wiederherstellungspunkte mit dem Azure-Portal

Benutzerdefinierte Wiederherstellungspunkte können auch mit dem Azure-Portal erstellt werden.

1. Melden Sie sich bei Ihrem [Azure-Portal](https://portal.azure.com/)-Konto an.

2. Navigieren Sie zum dedizierten SQL-Pool, für den Sie einen Wiederherstellungspunkt erstellen möchten.

3. Wählen Sie im linken Bereich **Übersicht** und dann **+ Neuer Wiederherstellungspunkt** aus. Wenn die Schaltfläche „Neuer Wiederherstellungspunkt“ nicht aktiviert wird, stellen Sie sicher, dass der dedizierte SQL-Pool nicht angehalten wird.

    ![Neuer Wiederherstellungspunkt](../media/sql-pools/create-sqlpool-restore-point-01.png)

4. Geben Sie einen Namen für den benutzerdefinierten Wiederherstellungspunkt ein, und klicken Sie auf **Übernehmen**. Für benutzerdefinierte Wiederherstellungspunkte gilt eine standardmäßige Aufbewahrungsdauer von sieben Tagen.

    ![Name für den Wiederherstellungspunkt](../media/sql-pools/create-sqlpool-restore-point-02.png)

## <a name="next-steps"></a>Nächste Schritte

- [Wiederherstellen eines vorhandenen dedizierten SQL-Pools](restore-sql-pool.md)

