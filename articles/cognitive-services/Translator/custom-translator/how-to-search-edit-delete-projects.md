---
title: Suchen, Bearbeiten und Löschen von Projekten – Custom Translator
titleSuffix: Azure Cognitive Services
description: Custom Translator bietet verschiedene Möglichkeiten, Ihre Projekte effizient zu verwalten. Sie können mehrere Projekte erstellen, nach ausgewählten Kriterien suchen und Ihre Projekte bearbeiten. Außerdem können Sie in Custom Translator Projekte löschen.
author: laujan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 08/17/2020
ms.author: lajanuar
ms.topic: conceptual
ms.openlocfilehash: 308ca25d35011c67ded7300177149cd590462952
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "98896442"
---
# <a name="search-edit-and-delete-projects"></a>Suchen, Bearbeiten und Löschen von Projekten

Custom Translator bietet mehrere Möglichkeiten, Ihre Projekte effizient zu verwalten. Sie können viele Projekte erstellen, nach ausgewählten Kriterien suchen und Ihre Projekte bearbeiten. Außerdem können Sie in Custom Translator Projekte löschen.  

## <a name="search-and-filter-projects"></a>Suchen und Filtern von Projekten

Das Filtertool ermöglicht es Ihnen, Projekte nach verschiedenen Filterbedingungen zu suchen. Es filtert z.B. nach Projektname, Status, Ausgangs- und Zielsprache sowie Projektkategorie.

1. Klicken Sie auf die Schaltfläche „Filter“.

    ![Suchen nach Projekt](media/how-to/how-to-search-project.png)

2. Sie können nach den folgenden Feldern filtern: Projektname, Ausgangssprache, Zielsprache, Kategorie und Projektverfügbarkeit.

3. Klicken Sie auf „Übernehmen“.

    ![Filteroptionen für die Suche nach Projekten](media/how-to/how-to-search-project-filters.png)

4. Löschen Sie den Filter durch Tippen auf „Löschen“, damit alle Projekte angezeigt werden.

## <a name="edit-a-project"></a>Bearbeiten eines Projekts

Der benutzerdefinierte Translator bietet Ihnen die Möglichkeit, den Namen und die Beschreibung eines Projekts zu bearbeiten. Andere Projektmetadaten wie Kategorie, Ausgangs- und Zielsprache können nicht bearbeitet werden. Die folgenden Schritte beschreiben, wie Sie ein Projekt bearbeiten können.

1. Klicken Sie auf das Bleistiftsymbol, das erscheint, wenn Sie mit der Maus auf ein Projekt zeigen.

    ![Bearbeiten eines Projekts](media/how-to/how-to-edit-project.png)

2. Im Dialogfeld können Sie den Projektnamen, die Beschreibung des Projekts, die Kategoriebeschreibung und die Projektbezeichnung ändern, wenn kein Modell bereitgestellt wird. Sie können die Kategorie oder das Sprachpaar nicht ändern, nachdem das Projekt erstellt wurde.

    ![Dialogfeld „Projekt bearbeiten“](media/how-to/how-to-edit-project-dialog.png)

3. Klicken Sie auf die Schaltfläche „Speichern“.

## <a name="delete-a-project"></a>Löschen eines Projekts

Sie können ein Projekt löschen, wenn Sie es nicht mehr benötigen. Stellen Sie sicher, dass das Projekt keine Modelle im aktivem Zustand aufweist (z. B. „deployed“ (bereitgestellt), „training submitted“ (Training übermittelt), „data processing“ (Datenverarbeitung), „deploying“ (wird bereitgestellt) usw.), andernfalls schlägt der Löschvorgang fehl. Die folgenden Schritte beschreiben, wie Sie ein Projekt löschen können.

1. Zeigen Sie mit der Maus auf einen beliebigen Projektdatensatz, und klicken Sie auf das Papierkorbsymbol.

   ![Löschen eines Projekts](media/how-to/how-to-delete-project.png)

2. Bestätigen Sie den Löschvorgang. Beim Löschen eines Projekts werden alle Modelle entfernt, die innerhalb dieses Projekts erstellt wurden. Das Löschen von Projekten hat keinen Einfluss auf Ihre Dokumente.

   ![Dialogfeld zum Bestätigen des Löschvorgangs](media/how-to/how-to-delete-project-confirm.png)

## <a name="next-steps"></a>Nächste Schritte

- [Laden Sie Dokumente hoch](how-to-upload-document.md), um Ihr benutzerdefiniertes Übersetzungsmodell zu erstellen.
