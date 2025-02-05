---
title: Anpassen eine Synchronisierungsregel – Azure AD Connect | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie den Synchronisierungsregel-Editor verwenden, um eine Synchronisierungsregel zu bearbeiten oder eine neue zu erstellen.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: curtand
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 01/31/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: e2bb86988454141dc692b4a9967997c4ff7574a2
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "90530487"
---
# <a name="how-to-customize-a-synchronization-rule"></a>Anpassen einer Synchronisierungsregel

## <a name="recommended-steps"></a>**Empfohlene Schritte**

Sie können den Synchronisierungsregel-Editor verwenden, um eine Synchronisierungsregel zu bearbeiten oder eine neue Regel zu erstellen. Um Änderungen an Synchronisierungsregeln vornehmen zu können, müssen Sie ein fortgeschrittener Benutzer sein. Falsche Änderungen können zur Löschung von Objekten in Ihrem Zielverzeichnis führen. Lesen Sie noch einmal [Empfohlene Dokumente](#recommended-documents), um sich das für Synchronisierungsregeln notwendige Fachwissen anzueignen. Führen Sie die folgenden Schritte aus, um eine Synchronisierungsregel zu ändern:

* Starten Sie den Synchronisierungsregel-Editor im Anwendungsmenü auf dem Desktop, wie unten dargestellt:

    ![Menü „Synchronisierungsregel-Editor“](media/how-to-connect-create-custom-sync-rule/how-to-connect-create-custom-sync-rule/syncruleeditormenu.png)

* Um eine Standardsynchronisierungsregel anzupassen, klonen Sie die vorhandene Regel, indem Sie im Synchronisierungsregel-Editor auf die Schaltfläche „Bearbeiten“ klicken. Dadurch wird eine Kopie der Standardregel erstellt und diese deaktiviert. Speichern Sie die geklonte Regel mit einer Priorität unter 100.  Die Priorität bestimmt im Fall eines Attributflusskonflikts, welche Regel bei der Konfliktlösung den Vorrang hat (d.h. welche Regel den niedrigeren numerischen Wert hat).

    ![Synchronisierungsregel-Editor](media/how-to-connect-create-custom-sync-rule/how-to-connect-create-custom-sync-rule/clonerule.png)

* Wenn Sie ein bestimmtes Attribut ändern, sollten Sie im Idealfall nur das geänderte Attribut in der geklonten Regel beibehalten.  Aktivieren Sie dann die Standardregel so, dass das geänderte Attribut aus der geklonten Regel und andere Attribute aus der Standardregel bezogen werden. 

* Dabei ist Folgendes zu beachten: Wenn der berechnete Wert des geänderten Attributs in der geklonten Regel NULL und in der Standardregel NICHT NULL beträgt, hat der NICHT-NULL-Wert Vorrang und ersetzt den NULL-Wert. Wenn ein NULL-Wert nicht durch einen NICHT-NULL-Wert ersetzt werden soll, weisen Sie dem Wert in der geklonten Regel „AuthoritativeNull“ zu.

* Wenn Sie eine **ausgehende** Regel ändern möchten, ändern Sie den Filter im Synchronisierungsregel-Editor.

## <a name="recommended-documents"></a>**Empfohlene Dokumente**
* [Azure AD Connect-Synchronisierung: Technische Konzepte](./how-to-connect-sync-technical-concepts.md)
* [Azure AD Connect-Synchronisierung: Grundlagen der Architektur](./concept-azure-ad-connect-sync-architecture.md)
* [Azure AD Connect-Synchronisierung: Grundlegendes zur deklarativen Bereitstellung](./concept-azure-ad-connect-sync-declarative-provisioning.md)
* [Azure AD Connect-Synchronisierung: Grundlegendes zu Ausdrücken für die deklarative Bereitstellung](./concept-azure-ad-connect-sync-declarative-provisioning-expressions.md)
* [Azure AD Connect-Synchronisierung: Grundlegendes zur Standardkonfiguration](./concept-azure-ad-connect-sync-default-configuration.md)
* [Azure AD Connect-Synchronisierung: Grundlegendes zu Benutzern, Gruppen und Kontakten](./concept-azure-ad-connect-sync-user-and-contacts.md)
* [Azure AD Connect-Synchronisierung: Schattenattribute](./how-to-connect-syncservice-shadow-attributes.md)

## <a name="next-steps"></a>Nächste Schritte
- [Azure AD Connect-Synchronisierung](how-to-connect-sync-whatis.md)
- [Was ist eine Hybrididentität?](whatis-hybrid-identity.md)