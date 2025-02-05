---
title: Festlegen der Größe für SAP HANA in Azure (große Instanzen) | Microsoft-Dokumentation
description: Festlegen der Größe für SAP HANA in Azure (große Instanzen)
services: virtual-machines-linux
documentationcenter: ''
author: msjuergent
manager: bburns
editor: ''
ms.service: virtual-machines-sap
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/04/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d49191abbba9c189672be4cd8bad4346e9689bf6
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2021
ms.locfileid: "101675424"
---
# <a name="sizing"></a>Festlegen der Größe

Die Größenanpassung für HANA (große Instanz) unterscheidet sich nicht von der Größenanpassung für HANA im Allgemeinen. Für vorhandene und bereitgestellte Systeme, die Sie von anderen RDBMS auf HANA umstellen möchten, bietet SAP eine Reihe von Berichten, die auf Ihren vorhandenen SAP-Systemen ausgeführt werden können. Wenn die Datenbank zu HANA migriert wird, überprüfen diese Berichte die Daten und berechnen Arbeitsspeicheranforderungen für die HANA-Instanz. Weitere Informationen zur Ausführung dieser Berichte und zum Abrufen der aktuellen Patches oder Versionen finden Sie in den folgenden SAP-Hinweisen:

- [SAP-Hinweis 1793345 – Größenanpassung für die SAP-Suite für HANA](https://launchpad.support.sap.com/#/notes/1793345)
- [SAP-Hinweis 1872170 – Bericht zur Größenanpassung für die Suite für HANA und S/4 HANA](https://launchpad.support.sap.com/#/notes/1872170)
- [SAP-Hinweis 2121330 – Häufig gestellte Fragen: Bericht zur Größenanpassung für SAP BW unter HANA](https://launchpad.support.sap.com/#/notes/2121330)
- [SAP-Hinweis 1736976 – Bericht zur Größenanpassung für BW unter HANA](https://launchpad.support.sap.com/#/notes/1736976)
- [SAP-Hinweis 2296290 – Neuer Bericht zur Größenanpassung für BW unter HANA](https://launchpad.support.sap.com/#/notes/2296290)

Für Greenfield-Implementierungen steht der SAP Quick Sizer zur Verfügung, um Arbeitsspeicheranforderungen für die Implementierung von SAP-Software zusätzlich zu HANA zu berechnen.

Der Arbeitsspeicherbedarf für HANA nimmt mit zunehmender Datenmenge zu. Beobachten Sie Ihre aktuelle Arbeitsspeichernutzung, um den zukünftigen Bedarf zu prognostizieren. Basierend auf den Arbeitsspeicheranforderungen können Sie Ihren Bedarf dann einer der SKUs von HANA (große Instanz) zuordnen.

**Nächste Schritte**
- Lesen Sie [Onboardinganforderungen](hana-onboarding-requirements.md).