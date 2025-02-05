---
title: Automatisches Anwenden von Vertraulichkeitsbezeichnungen auf Ihre Daten
description: Erfahren Sie, wie Sie Vertraulichkeitsbezeichnungen erstellen und sie während eines Scans automatisch auf Ihre Daten anwenden.
author: batamig
ms.author: bagol
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: how-to
ms.date: 03/09/2021
ms.openlocfilehash: 37ac292acc76c681ea38b2ae881ff8cd2ae5ec3c
ms.sourcegitcommit: 15d27661c1c03bf84d3974a675c7bd11a0e086e6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2021
ms.locfileid: "102502445"
---
# <a name="automatically-label-your-data-in-azure-purview"></a>Automatisches Bezeichnen Ihrer Daten in Azure Purview

In diesem Artikel wird beschrieben, wie Sie MIP-Vertraulichkeitsbezeichnungen (Microsoft Information Protection) erstellen und automatisch auf Ihre Azure-Ressourcen in Azure Purview anwenden.

## <a name="what-are-sensitivity-labels"></a>Was sind Vertraulichkeitsbezeichnungen? 

Um die Arbeit zu erledigen, arbeiten Personen in Ihrer Organisation mit anderen Personen innerhalb und außerhalb der Organisation zusammen. Daten bleiben nicht immer in Ihrer Cloud und wandern häufig über Geräte, Apps und Dienste hierhin und dorthin. 

Wenn Ihre Daten wandern, sollen sie dabei so sicher und geschützt sein, wie es den Geschäfts- und Compliancerichtlinien Ihrer Organisation entspricht.

Mit Vertraulichkeitsbezeichnungen können Sie angeben, wie vertraulich bestimmte Daten in Ihrer Organisation sind. Beispielsweise kann ein bestimmter Projektname in Ihrer Organisation streng vertraulich sein, während derselbe Begriff in anderen Organisationen nicht vertraulich ist. 

### <a name="sensitivity-labels-in-azure-purview"></a>Vertraulichkeitsbezeichnungen in Azure Purview

In Purview ähneln Klassifizierungen Betrefftags und werden zum Kennzeichnen und Identifizieren von Daten eines bestimmten Typs verwendet, die während der Überprüfung in Ihrer Datenumgebung gefunden werden.

Purview verwendet die gleichen Klassifizierungen, auch als vertrauliche Informationstypen bezeichnet, wie Microsoft 365.  Die MIP-Vertraulichkeitsbezeichnungen werden im Microsoft 365 Security and Compliance Center (SCC) erstellt. Dies ermöglicht Ihnen, Ihre vorhandenen Vertraulichkeitsbezeichnungen auf Ihre Azure Purview-Ressourcen auszuweiten.

**Klassifizierungen** werden direkt abgeglichen, z. B. eine Sozialversicherungsnummer, die über die Klassifizierung **Sozialversicherungsnummer** verfügt. 

Im Gegensatz dazu werden **Vertraulichkeitsbezeichnungen** angewendet, wenn eine oder mehrere Klassifizierungen und Bedingungen gleichzeitig gefunden werden. In diesem Kontext beziehen sich [Bedingungen](/microsoft-365/compliance/apply-sensitivity-label-automatically) auf alle Parameter, die Sie für unstrukturierte Daten definieren können, z. B. *Nähe zu einer anderen Klassifizierung* und *% Konfidenz*. 

Vertraulichkeitsbezeichnungen in Azure Purview können zum automatischen Anwenden von Bezeichnungen auf Dateien und Datenbankspalten verwendet werden.

Weitere Informationen finden Sie unter

- [Informationen zu Vertraulichkeitsbezeichnungen](/microsoft-365/compliance/sensitivity-labels) in der Microsoft 365-Dokumentation
- [Was sind Regeln für die automatische Bezeichnung?](#what-are-auto-labeling-rules)
- [Unterstützte Datentypen für Vertraulichkeitsbezeichnungen in Azure Purview](#supported-data-types-for-sensitivity-labels-in-azure-purview)
- [Bezeichnungen für Spalten von SQL-Datenbanken](#labeling-for-sql-database-columns)

#### <a name="what-are-auto-labeling-rules"></a>Was sind Regeln für die automatische Bezeichnung?

Die Menge Ihrer Daten nimmt ständig zu und sie werden ständig geändert. Das Nachverfolgen der zurzeit nicht bezeichneten Daten und manuelle Bezeichnen ist nicht nur mühsam, sondern sorgt auch für unnötiges Kopfzerbrechen. 

Die Regeln für die automatische Bezeichnung sind Bedingungen, mit denen Sie angeben, wann eine bestimmte Bezeichnung angewandt werden soll. Wenn diese Bedingungen erfüllt sind, wird die Bezeichnung automatisch den Daten zugewiesen, sodass konsistente Vertraulichkeitsbezeichnungen für Ihre Daten im großen Stil beibehalten werden.

Stellen Sie beim Erstellen der Bezeichnungen sicher, dass Sie sowohl für [Dateien](#define-auto-labeling-rules-for-files) als auch [Datenbankspalten](#define-auto-labeling-rules-for-database-columns) Regeln für die automatische Bezeichnung definieren, um die Bezeichnungen automatisch mit jedem Datenscan anzuwenden. 

Nachdem Sie Ihre Daten in Purview gescannt haben, können Sie die automatisch angewendeten Bezeichnungen im Purview Catalog und in den Erkenntnisse-Berichten anzeigen.
#### <a name="supported-data-types-for-sensitivity-labels-in-azure-purview"></a>Unterstützte Datentypen für Vertraulichkeitsbezeichnungen in Azure Purview

Vertraulichkeitsbezeichnungen werden in Azure Purview für die folgenden Datentypen unterstützt:

|Datentyp  |Quellen  |
|---------|---------|
|Automatische Bezeichnung für Dateien     |      : Azure Blob Storage  </br>– Azure Data Lake Storage Gen 1 und Gen 2  |
|Automatische Bezeichnung für Datenbankspalten     |  – SQL Server </br>– Azure SQL-Datenbank </br>– Verwaltete Azure SQL-Datenbank-Instanz   <br> – Azure Synapse  <br>: Azure Cosmos DB <br><br>Weitere Informationen finden Sie im Anschluss unter [Bezeichnungen für Spalten von SQL-Datenbanken](#labeling-for-sql-database-columns).  |
| | |

#### <a name="labeling-for-sql-database-columns"></a>Bezeichnungen für Spalten von SQL-Datenbanken

Zusätzlich zu Purview-Bezeichnungen für Datenbankspalten unterstützt Microsoft auch Bezeichnungen für die Spalten von SQL-Datenbanken, indem die SQL-Datenklassifizierung in [SQL Server Management Studio (SSMS)](/sql/ssms/sql-server-management-studio-ssms) verwendet wird. Während in Purview die globalen [MIP-Vertraulichkeitsbezeichnungen](/microsoft-365/compliance/sensitivity-labels) verwendet werden, werden in SSMS nur lokal definierte Bezeichnungen genutzt.

Bei den Bezeichnungen in Purview und in SSMS handelt es sich um separate Prozesse, die derzeit nicht miteinander interagieren. Aus diesem Grund werden in SSMS angewendete Bezeichnungen in Purview nicht angezeigt (und umgekehrt). Für Bezeichnungen in SQL-Datenbanken empfehlen wir Azure Purview, da hierbei globale MIP-Bezeichnungen genutzt werden, die übergreifend für mehrere Plattformen angewendet werden können.

Weitere Informationen finden Sie unter [SQL-Datenermittlung und -klassifizierung](/sql/relational-databases/security/sql-data-discovery-and-classification).

## <a name="how-to-create-sensitivity-labels-in-microsoft-365"></a>Erstellen von Vertraulichkeitsbezeichnungen in Microsoft 365

Wenn Sie nicht bereits über Vertraulichkeitsbezeichnungen verfügen, müssen Sie sie erstellen und für Azure Purview verfügbar machen. Vorhandene Vertraulichkeitsbezeichnungen können auch geändert werden, um sie für Azure Purview verfügbar zu machen.

Weitere Informationen finden Sie unter

- [Lizenzanforderungen](#licensing-requirements)
- [Erweitern von Vertraulichkeitsbezeichnungen auf Azure Purview](#extending-sensitivity-labels-to-azure-purview)
- [Erstellen neuer Vertraulichkeitsbezeichnungen oder Ändern vorhandener Bezeichnungen](#creating-new-sensitivity-labels-or-modifying-existing-labels)
### <a name="licensing-requirements"></a>Lizenzanforderungen

Die MIP-Vertraulichkeitsbezeichnungen werden im Microsoft 365 Security and Compliance Center erstellt und verwaltet. Zum Erstellen von Vertraulichkeitsbezeichnungen für die Verwendung in Azure Purview müssen Sie über eine aktive Microsoft 365 E5-Lizenz verfügen.

Wenn Sie nicht bereits über die erforderliche Lizenz verfügen, können Sie sich für eine Testversion von [Microsoft 365 E5](https://www.microsoft.com/microsoft-365/business/compliance-solutions#midpagectaregion) registrieren.

### <a name="extending-sensitivity-labels-to-azure-purview"></a>Erweitern von Vertraulichkeitsbezeichnungen auf Azure Purview

Standardmäßig sind MIP-Vertraulichkeitsbezeichnungen nur für Ressourcen in Microsoft 365 verfügbar, wo Sie sie auf Dateien und E-Mails anwenden können.

Zum Anwenden von MIP-Vertraulichkeitsbezeichnungen auf Azure-Ressourcen in Azure Purview müssen Sie der Erweiterung der Bezeichnungen ausdrücklich zustimmen und die spezifischen Bezeichnungen auswählen, die in Purview verfügbar sein sollen.

Durch die Erweiterung der MIP-Vertraulichkeitsbezeichnungen mit Azure Purview können Organisationen die Vertraulichkeit nun für eine breitere Palette von Datenquellen erkennen und klassifizieren und Erkenntnisse dazu erhalten. Dies minimiert das Risiko in Bezug auf die Compliance.

> [!NOTE]
> Da Microsoft 365 und Azure Purview separate Dienste sind, werden sie möglicherweise in verschiedenen Regionen bereitgestellt. Bezeichnungsnamen und Namen benutzerdefinierter Vertraulichkeitsinformationstypen werden als Kundendaten betrachtet und standardmäßig am gleichen geografischen Standort aufbewahrt, um die Vertraulichkeit Ihrer Daten zu schützen und die Vorschriften der DSGVO einzuhalten.
>
> Aus diesem Grund werden Bezeichnungen und benutzerdefinierte vertrauliche Informationstypen nicht standardmäßig für Azure Purview freigegeben, sondern erfordern zur Verwendung in Azure Purview Ihre Zustimmung.

**So erweitern Sie Vertraulichkeitsbezeichnungen auf Purview:**

Mit den folgenden Schritten können Sie Ihre Vertraulichkeitsbezeichnungen zur Verwendung in Azure Purview zur Verfügung stellen, wo Sie Ihre Vertraulichkeitsbezeichnungen auf Objekte wie SQL-Spalten, Dateien in Azure Blob Storage und vieles mehr anwenden können.

1. Navigieren Sie in Microsoft 365 zur Seite **Information Protection**. 
1. Wählen Sie in **Bezeichnung auf Ressourcen in Azure Purview erweitern** die Schaltfläche **Aktivieren** und im dann angezeigten Bestätigungsdialogfeld **Ja** aus.

Beispiel:

:::image type="content" source="media/create-sensitivity-label/extend-sensitivity-labels-to-purview-small.png" alt-text="Wählen Sie **Aktivieren** aus, um Vertraulichkeitsbezeichnungen auf Purview zu erweitern." lightbox="media/create-sensitivity-label/extend-sensitivity-labels-to-purview.png":::
 
Nachdem Sie die Bezeichnung auf Ressourcen in Azure Purview erweitert haben, können Sie die Bezeichnungen auswählen, die Sie in Purview zur Verfügung stellen möchten. Weitere Informationen finden Sie unter [Erstellen neuer Vertraulichkeitsbezeichnungen oder Ändern vorhandener Bezeichnungen](#creating-new-sensitivity-labels-or-modifying-existing-labels).
### <a name="creating-new-sensitivity-labels-or-modifying-existing-labels"></a>Erstellen neuer Vertraulichkeitsbezeichnungen oder Ändern vorhandener Bezeichnungen

Wenn Sie Vertraulichkeitsbezeichnungen für Office-Apps unter Windows, macOS, iOS und Android verwenden, werden Benutzern neue Bezeichnungen innerhalb von vier Stunden und für Office im Web innerhalb einer Stunde angezeigt. Es kann jedoch bis zu 24 Stunden dauern, bis Änderungen in allen Apps und Diensten repliziert sind.

> [!IMPORTANT]
> Löschen Sie keine Bezeichnung, es sei denn, Sie kennen die Auswirkung für Ihre Benutzer. Weitere Informationen finden Sie unter [Entfernen und Löschen von Bezeichnungen](/microsoft-365/compliance/create-sensitivity-labels#removing-and-deleting-labels) in der Microsoft 365-Dokumentation.
>

**Zum Erstellen neuer Vertraulichkeitsbezeichnungen oder Ändern vorhandener Bezeichnungen gehen Sie folgendermaßen vor**:

1. Öffnen Sie das [Microsoft 365 Security and Compliance Center](https://protection.office.com/homepage). 

1. Wählen Sie unter **Lösungen** die Option **Information Protection** und dann **Bezeichnung erstellen** aus. 

    :::image type="content" source="media/create-sensitivity-label/create-sensitivity-label-full-small.png" alt-text="Erstellen von Vertraulichkeitsbezeichnungen im Microsoft 365 Security and Compliance Center" lightbox="media/create-sensitivity-label/create-sensitivity-label-full.png":::

1. Benennen Sie die Bezeichnung. Unter **Definieren des Bereichs für diese Bezeichnung** gehen Sie folgendermaßen vor:

    - Wählen Sie in allen Fällen **Azure Purview-Ressourcen** aus.
    - Zum Bezeichnen von Dateien wählen Sie außerdem **Dateien und E-Mails** aus. Diese Option ist beim ausschließlichen Bezeichnen von Datenbankressourcen nicht erforderlich. 
    
    :::image type="content" source="media/create-sensitivity-label/create-label-scope-small.png" alt-text="Erstellen Ihrer Bezeichnung im Microsoft 365 Security and Compliance Center" lightbox="media/create-sensitivity-label/create-label-scope.png":::

1. Befolgen Sie die übrigen Eingabeaufforderungen im Assistenten für Ihre Bezeichnungseinstellungen. 

    Definieren Sie insbesondere die Regeln für die automatische Bezeichnung von Dateien und Datenbankspalten:

    - [Definieren von Regeln für die automatische Bezeichnung von Dateien](#define-auto-labeling-rules-for-files)
    - [Definieren von Regeln für die automatische Bezeichnung von Datenbankspalten](#define-auto-labeling-rules-for-database-columns)

    Weitere Informationen zu den Optionen des Assistenten finden Sie unter [Wirkung von Vertraulichkeitsbezeichnungen](/microsoft-365/compliance/sensitivity-labels#what-sensitivity-labels-can-do) in der Microsoft 365-Dokumentation.

1. Wiederholen Sie die oben aufgeführten Schritte, um weitere Bezeichnungen zu erstellen. 

    Um eine untergeordnete Bezeichnung zu erstellen, wählen Sie die übergeordnete Bezeichnung und dann **...**  > **Weitere Aktionen** > **Untergeordnete Bezeichnung hinzufügen**.

1. Zum Ändern vorhandener Bezeichnungen navigieren Sie zu **Information Protection** > **Bezeichnungen** und wählen Ihre Bezeichnung aus. 

    Wählen Sie dann **Bezeichnung bearbeiten** aus, um den Assistenten **Vertraulichkeitsbezeichnung bearbeiten** erneut mit allen Einstellungen zu öffnen, die Sie beim Erstellen der Bezeichnung festgelegt haben.

    :::image type="content" source="media/create-sensitivity-label/edit-sensitivity-label-full-small.png" alt-text="Bearbeiten einer vorhandenen Vertraulichkeitsbezeichnung" lightbox="media/create-sensitivity-label/edit-sensitivity-label-full.png":::

1. Wenn Sie alle Bezeichnungen erstellt haben, zeigen Sie unbedingt die Reihenfolge der Bezeichnungen an, und ordnen Sie sie bei Bedarf neu an. 

    Wenn Sie die Reihenfolge einer Bezeichnung ändern möchten, wählen Sie **...** **> Weitere Aktionen** > **Nach oben** oder **Nach unten** aus. 

    Weitere Informationen finden Sie unter [Priorität der Bezeichnungen (Reihenfolge wesentlich)](/microsoft-365/compliance/sensitivity-labels#label-priority-order-matters) in der Microsoft 365-Dokumentation.


Fahren Sie fort, indem Sie [Ihre Daten scannen, um Bezeichnungen automatisch anzuwenden](#scan-your-data-to-apply-labels-automatically), und dann Folgendes ausführen:

- [Anzeigen von Bezeichnungen für Ressourcen](#view-labels-on-assets)
- [Anzeigen von Erkenntnisse-Berichten für die Klassifizierungen und Vertraulichkeitsbezeichnungen](#view-insight-reports-for-the-classifications-and-sensitivity-labels)

#### <a name="define-auto-labeling-rules-for-files"></a>Definieren von Regeln für die automatische Bezeichnung von Dateien

Definieren Sie die Regeln für die automatische Bezeichnung von Dateien im Assistenten, wenn Sie die Bezeichnung erstellen oder bearbeiten. 

Aktivieren Sie auf der Seite **Automatische Bezeichnung für Office-Apps** die Option **Automatische Bezeichnung für Office-Apps**, und definieren Sie dann die Bedingungen für die automatische Anwendung Ihrer Bezeichnung auf die Daten.

Beispiel:

:::image type="content" source="media/create-sensitivity-label/create-auto-labeling-rules-files-small.png" alt-text="Definieren von Regeln für die automatische Bezeichnung von Dateien im Microsoft 365 Security and Compliance Center" lightbox="media/create-sensitivity-label/create-auto-labeling-rules-files.png":::
 
Weitere Informationen finden Sie unter [Konfigurieren der automatischen Zuweisung von Bezeichnungen für Office-Apps](/microsoft-365/compliance/apply-sensitivity-label-automatically#how-to-configure-auto-labeling-for-office-apps) in der Microsoft 365-Dokumentation. 

#### <a name="define-auto-labeling-rules-for-database-columns"></a>Definieren von Regeln für die automatische Bezeichnung von Datenbankspalten

Definieren Sie die Regeln für die automatische Bezeichnung von Datenbankspalten im Assistenten, wenn Sie die Bezeichnung erstellen oder bearbeiten. 

Führen Sie unter der Option **Azure Purview-Ressourcen (Vorschau)** Folgendes aus:

1. Wählen Sie den Schieberegler **Automatische Bezeichnung für Datenbankspalten** aus.

1. Wählen Sie **Vertrauliche Informationstypen überprüfen** aus, um die vertraulichen Informationstypen auszuwählen, die Sie auf Ihre Bezeichnung anwenden möchten.

Beispiel:
        
:::image type="content" source="media/create-sensitivity-label/create-auto-labeling-rules-db-columns-small.png" alt-text="Definieren von Regeln für die automatische Bezeichnung von SQL-Spalten im Microsoft 365 Security and Compliance Center" lightbox="media/create-sensitivity-label/create-auto-labeling-rules-db-columns.png":::

## <a name="scan-your-data-to-apply-labels-automatically"></a>Scannen Ihrer Daten, um Bezeichnungen automatisch anzuwenden

Überprüfen Sie Ihre Daten in Azure Purview, um die erstellten Bezeichnungen automatisch auf Grundlage der definierten Regeln für die automatische Bezeichnung anzuwenden. 

Weitere Informationen zum Einrichten von Scans für verschiedene Ressourcen in Azure Purview finden Sie unter:

|`Source`  |Referenz  |
|---------|---------|
|**Azure Blob Storage**     |[Registrieren und Überprüfen von Azure Blob Storage](register-scan-azure-blob-storage-source.md)         |
|**Azure Data Lake-Speicher**     |[Registrieren und Überprüfen von Azure Data Lake Storage Gen1](register-scan-adls-gen1.md) </br>[Registrieren und Überprüfen von Azure Data Lake Storage Gen2](register-scan-adls-gen2.md)         |
|**Azure SQL-Datenbank-Instanzen**|[Registrieren und Überprüfen von Azure SQL-Datenbank](register-scan-azure-sql-database.md) </br>[Registrieren und Überprüfen einer verwalteten Azure SQL-Datenbank-Instanz](register-scan-azure-sql-database-managed-instance.md)|
| | |

## <a name="view-labels-on-assets"></a>Anzeigen von Bezeichnungen für Ressourcen

Nachdem Sie die Regeln für die automatische Bezeichnung in Microsoft 365 definiert und Ihre Daten in Azure Purview gescannt haben, werden automatisch Bezeichnungen auf Ihre Ressourcen angewandt. 

**So zeigen Sie die Bezeichnungen an, die im Azure Purview Catalog auf Ihre Ressourcen angewendet werden:**

Verwenden Sie im Azure Purview Catalog die Optionen zum Filtern der **Bezeichnung**, um nur Dateien mit bestimmten Bezeichnungen anzuzeigen. Beispiel: 

:::image type="content" source="media/create-sensitivity-label/filter-search-results-small.png" alt-text="Nach Bezeichnung nach Ressourcen suchen" lightbox="media/create-sensitivity-label/filter-search-results.png":::

Beispiel:

:::image type="content" source="media/create-sensitivity-label/view-labeled-files-blob-storage-small.png" alt-text="Vertraulichkeitsbezeichnung einer Datei in Ihrer Azure Blob Storage-Instanz anzeigen" lightbox="media/create-sensitivity-label/view-labeled-files-blob-storage.png":::

## <a name="view-insight-reports-for-the-classifications-and-sensitivity-labels"></a>Anzeigen von Erkenntnisse-Berichten für die Klassifizierungen und Vertraulichkeitsbezeichnungen

Finden Sie mit den Berichten **Klassifizierung** und **Vertraulichkeitsbezeichnung** Erkenntnisse über Ihre klassifizierten und bezeichneten Daten in Azure Purview.

> [!div class="nextstepaction"]
> [Klassifizierungs-Erkenntnisse](./classification-insights.md)

> [!div class="nextstepaction"]
> [Vertraulichkeitsbezeichnungs-Erkenntnisse](sensitivity-insights.md)
