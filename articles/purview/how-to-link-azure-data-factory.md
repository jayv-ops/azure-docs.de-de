---
title: Herstellen einer Verbindung mit Azure Data Factory
description: In diesem Artikel wird beschrieben, wie Sie Azure Data Factory und Azure Purview verbinden, um die Datenherkunft nachzuverfolgen.
author: chanuengg
ms.author: csugunan
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: how-to
ms.date: 03/08/2021
ms.openlocfilehash: 8812806e535e8e34ca07fdb13e6223bfa0c91d6b
ms.sourcegitcommit: f6193c2c6ce3b4db379c3f474fdbb40c6585553b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2021
ms.locfileid: "102449610"
---
# <a name="how-to-connect-azure-data-factory-and-azure-purview"></a>Verbinden von Azure Data Factory und Azure Purview

In diesem Dokument werden die Schritte erläutert, die zum Verbinden eines Azure Data Factory-Kontos mit einem Azure Purview-Konto erforderlich sind, um die Datenherkunft nachzuverfolgen. Das Dokument enthält zudem Details zum Abdeckungsbereich und zu den unterstützten Herkunftsmustern.

## <a name="view-existing-data-factory-connections"></a>Anzeigen vorhandener Data Factory-Verbindungen

Mehrere Azure Data Factorys können eine Verbindung mit einem einzelnen Azure Purview Data Catalog herstellen, um Herkunftsinformationen per Push zu übertragen. Gemäß den aktuellen Grenzwerten können Sie vom Purview-Verwaltungscenter eine Verbindung mit bis zu zehn Data Factory-Konten gleichzeitig herstellen. Um die Liste der mit Ihrem Purview Data Catalog verbundenen Data Factory-Konten anzuzeigen, gehen Sie folgendermaßen vor:

1. Wählen Sie im linken Navigationsbereich **Verwaltungscenter** aus.
2. Wählen Sie unter **Externe Verbindungen** die Option **Data Factory-Verbindung** aus.
3. Die Liste mit Data Factory-Verbindungen wird angezeigt.

    :::image type="content" source="./media/how-to-link-azure-data-factory/data-factory-connection.png" alt-text="Screenshot einer Data Factory-Verbindungsliste" lightbox="./media/how-to-link-azure-data-factory/data-factory-connection.png":::

4. Beachten Sie die verschiedenen Werte für den **Status** einer Verbindung:

    - **Verbunden**: Die Data Factory ist mit dem Data Catalog verbunden.
    - **Getrennt**: Die Data Factory hat Zugriff auf den Katalog, ist aber mit einem anderen Katalog verbunden. Daher wird die Datenherkunft nicht automatisch an den Katalog gemeldet.
    - **CannotAccess**: Der aktuelle Benutzer hat keine Zugriff auf die Data Factory, daher ist der Verbindungsstatus unbekannt.
 >[!Note]
 >Um die Data Factory-Verbindungen anzeigen zu können, muss Ihnen eine der Purview-Rollen zugewiesen sein:
 >- Mitwirkender
 >- Besitzer
 >- Leser
 >- Benutzerzugriffsadministrator

## <a name="create-new-data-factory-connection"></a>Erstellen einer neuen Data Factory-Verbindung

>[!Note]
>Um die Data Factory-Verbindungen hinzuzufügen oder entfernen zu können, muss Ihnen eine der Purview-Rollen zugewiesen sein:
>- Besitzer
>- Benutzerzugriffsadministrator
>
> Außerdem müssen die Benutzer „Besitzer“ oder „Mitwirkender“ der Data Factory sein. 

Führen Sie die folgenden Schritte aus, um vorhandene Data Factory-Konten mit Ihrem Purview Data Catalog zu verbinden.

1. Wählen Sie im linken Navigationsbereich **Verwaltungscenter** aus.
2. Wählen Sie unter **Externe Verbindungen** die Option **Data Factory-Verbindung** aus.
3. Wählen Sie auf der Seite **Data Factory-Verbindung** die Option **neu** aus.

4. Wählen Sie Ihr Data Factory-Konto aus der Liste aus, und klicken Sie auf **OK**. Sie können die Liste auch nach Abonnementnamen filtern, um sie einzuschränken.

    :::image type="content" source="./media/how-to-link-azure-data-factory/connect-data-factory.png" alt-text="Screenshot: Verbinden von Azure Data Factory" lightbox="./media/how-to-link-azure-data-factory/connect-data-factory.png":::

    Einige Data Factory-Instanzen sind möglicherweise deaktiviert, wenn die Data Factory bereits mit dem aktuellen Purview-Konto verbunden ist oder nicht über eine verwaltete Identität verfügt.

    Es wird eine Warnmeldung angezeigt, wenn eine der ausgewählten Data Factorys bereits mit einem anderen Purview-Konto verbunden ist. Durch Klicken auf „OK“ wird die Data Factory-Verbindung mit dem anderen Purview-Konto getrennt. Es sind keine weiteren Bestätigungen erforderlich.


    :::image type="content" source="./media/how-to-link-azure-data-factory/warning-for-disconnect-factory.png" alt-text="Screenshot: Warnung vor dem Trennen einer Azure Data Factory-Verbindung" lightbox="./media/how-to-link-azure-data-factory/warning-for-disconnect-factory.png":::

>[!Note]
>Zurzeit wird das gleichzeitige Hinzufügen von nicht mehr als 10 Data Factorys unterstützt. Wenn Sie mehr als 10 Data Factorys gleichzeitig hinzufügen möchten, erstellen Sie ein Supportticket.

### <a name="how-does-the-authentication-work"></a>Wie funktioniert die Authentifizierung?

Wenn ein Purview-Benutzer eine Data Factory registriert, auf die er Zugriff hat, erfolgt im Back-End Folgendes:

1. Die **verwaltete Data Factory-Identität** wird zur RBAC-Rolle für Purview hinzugefügt: **Datenkurator für Purview**.

    :::image type="content" source="./media/how-to-link-azure-data-factory/adf-msi.png" alt-text="Screenshot, der die Azure Data Factory-MSI zeigt." lightbox="./media/how-to-link-azure-data-factory/adf-msi.png":::
     
2. Die Data Factory-Pipeline muss erneut ausgeführt werden, sodass die Herkunftsmetadaten per Push zurück an Purview übertragen werden können.
3. Nach der Ausführung werden die Data Factory-Metadaten per Push an Purview übertragen.

### <a name="remove-data-factory-connections"></a>Entfernen von Data Factory-Verbindungen
Um eine Data Factory-Verbindung zu entfernen, gehen Sie folgendermaßen vor:

1. Klicken Sie auf der Seite **Data Factory-Verbindung** auf die Schaltfläche **Entfernen** neben mindestens einer Data Factory-Verbindung.
2. Klicken Sie in der angezeigten Popupmeldung auf **Bestätigen**, um die ausgewählten Data Factory-Verbindungen zu löschen.

    :::image type="content" source="./media/how-to-link-azure-data-factory/remove-data-factory-connection.png" alt-text="Screenshot: Auswählen der Data Factorys, deren Verbindung entfernt werden soll" lightbox="./media/how-to-link-azure-data-factory/remove-data-factory-connection.png":::

## <a name="configure-a-self-hosted-integration-runtime-to-collect-lineage"></a>Konfigurieren einer selbstgehosteten Integration Runtime zum Erfassen der Herkunft

Die Erfassung der Herkunft für die Data Factory-Kopieraktivität ist für lokale Datenspeicher wie SQL-Datenbanken verfügbar. Wenn Sie eine selbstgehostete Integration Runtime (IR) für die Datenverschiebung mit Azure Data Factory ausführen und die Herkunft in Azure Purview erfassen möchten, stellen Sie sicher, dass Sie mindestens Version 5.0 verwenden. Weitere Informationen zur selbstgehosteten Integration Runtime finden Sie unter [Erstellen und Konfigurieren einer selbstgehosteten Integration Runtime](../data-factory/create-self-hosted-integration-runtime.md).

## <a name="supported-azure-data-factory-activities"></a>Unterstützte Azure Data Factory-Aktivitäten

Azure Purview erfasst die Runtimeherkunft aus den folgenden Azure Data Factory-Aktivitäten:

- [Daten kopieren](../data-factory/copy-activity-overview.md)
- [Datenfluss](../data-factory/concepts-data-flow-overview.md)
- [Ausführen des SSIS-Pakets](../data-factory/how-to-invoke-ssis-package-ssis-activity.md)

> [!IMPORTANT]
> Azure Purview verwirft die Herkunft, wenn die Quelle oder das Ziel ein nicht unterstütztes Datenspeichersystem verwendet.

Die Integration zwischen Data Factory und Purview unterstützt nur eine Teilmenge der Datensysteme, die von Data Factory unterstützt werden. Dies wird in den folgenden Abschnitten beschrieben.

### <a name="data-factory-copy-activity-support"></a>Unterstützung der Data Factory-Kopieraktivität

| Datenspeicher | Unterstützt | 
| ------------------- | ------------------- | 
| Azure Blob Storage | Ja |
| Azure Cognitive Search | Ja | 
| Azure Cosmos DB (SQL-API) \* | Ja | 
| Azure Cosmos DB-API für MongoDB \* | Ja |
| Azure Data Explorer \* | Ja | 
| Azure Data Lake Storage Gen1 | Ja | 
| Azure Data Lake Storage Gen2 | Ja | 
| Azure Database for MariaDB \* | Ja | 
| Azure Database for MySQL \* | Ja | 
| Azure Database for PostgreSQL \* | Ja |
| Azure File Storage | Ja | 
| Azure SQL-Datenbank \* | Ja | 
| Azure SQL Managed Instance \* | Ja | 
| Azure Synapse Analytics \* | Ja | 
| Azure Table Storage | Ja |
| Amazon S3 | Ja | 
| Hive \* | Ja | 
| SAP ECC \* | Ja |
| SAP-Tabelle | Ja |
| SQL Server \* | Ja | 
| Teradata \* | Ja |

*\* Azure Purview unterstützt derzeit keine Abfrage oder gespeicherte Prozedur für die Erfassung der Herkunft oder Überprüfung. Die Herkunft ist auf Tabellen- und Ansichtsquellen beschränkt.*

> [!Note]
> Das Herkunftsfeature verursacht einen gewissen Leistungsoverhead in der Data Factory-Kopieraktivität. Wenn Sie Data Factory-Verbindungen in Purview einrichten, werden Sie möglicherweise feststellen, dass bestimmte Kopieraufträge länger dauern. Die Auswirkungen sind in den meisten Fällen zu vernachlässigen. Wenn Ihre Kopieraufträge deutlich länger dauern als üblich, wenden Sie sich mit einem Zeitvergleich an den Support.

#### <a name="known-limitations-on-copy-activity-lineage"></a>Bekannte Einschränkungen der Herkunftserfassung für die Kopieraktivität

Wenn Sie die folgenden Funktionen der Kopieraktivität verwenden, wird die Erfassung der Herkunft derzeit noch nicht unterstützt:

- Kopieren von Daten in Azure Data Lake Storage Gen1 mithilfe des Binärformats
- Kopieren von Daten in Azure Synapse Analytics mithilfe von PolyBase oder der COPY-Anweisung
- Komprimierungseinstellung für Binärdateien, durch Trennzeichen getrennte Textdateien, Excel-, JSON- und XML-Dateien
- Quellpartitionsoptionen für Azure SQL-Datenbank, Azure SQL Managed Instance, Azure Synapse Analytics, SQL Server und SAP-Tabelle
- Option zur Quellpartitionsermittlung für dateibasierte Speicher
- Kopieren von Daten in eine dateibasierte Senke mit Einstellung für maximale Zeilenanzahl pro Datei
- Hinzufügen zusätzlicher Spalten während des Kopiervorgangs

### <a name="data-factory-data-flow-support"></a>Data Factory – Unterstützung für Datenflüsse

| Datenspeicher | Unterstützt |
| ------------------- | ------------------- | 
| Azure Blob Storage | Ja |
| Azure Data Lake Storage Gen1 | Ja |
| Azure Data Lake Storage Gen2 | Ja |
| Azure SQL-Datenbank \* | Ja |
| Azure Synapse Analytics \* | Ja |

*\* Azure Purview unterstützt derzeit keine Abfrage oder gespeicherte Prozedur für die Erfassung der Herkunft oder Überprüfung. Die Herkunft ist auf Tabellen- und Ansichtsquellen beschränkt.*

### <a name="data-factory-execute-ssis-package-support"></a>Unterstützung für Data Factory-Aktivität zum Ausführen eines SSIS-Pakets

| Datenspeicher | Unterstützt |
| ------------------- | ------------------- |
| Azure Blob Storage | Ja |
| Azure Data Lake Storage Gen1 | Ja |
| Azure Data Lake Storage Gen2 | Ja |
| Azure File Storage | Ja |
| Azure SQL-Datenbank \* | Ja |
| Azure SQL Managed Instance \*| Ja |
| Azure Synapse Analytics \* | Ja |
| SQL Server \* | Ja |

*\* Azure Purview unterstützt derzeit keine Abfrage oder gespeicherte Prozedur für die Erfassung der Herkunft oder Überprüfung. Die Herkunft ist auf Tabellen- und Ansichtsquellen beschränkt.*

> [!Note]
> Azure Data Lake Storage Gen2 ist jetzt allgemein verfügbar. Es wird empfohlen, ab sofort diese SKU zu verwenden. Weitere Informationen hierzu finden Sie auf der [Produktseite](https://azure.microsoft.com/en-us/services/storage/data-lake-storage/).

## <a name="supported-lineage-patterns"></a>Unterstützte Herkunftsmuster

Azure Purview unterstützt verschiedene Herkunftsmuster. Die generierten Herkunftsdaten basieren auf der Art der Quelle und der Senke, die in den Data Factory-Aktivitäten verwendet wird. Data Factory bietet zwar Unterstützung für über 80 Quellen und Senken, allerdings wird nur ein Teil davon auch in Azure Purview unterstützt. Eine Liste finden Sie unter [Unterstützte Azure Data Factory-Aktivitäten](#supported-azure-data-factory-activities).

Informationen dazu, wie Sie Data Factory für das Senden von Herkunftsdaten konfigurieren, finden Sie unter [Erste Schritte mit der Datenherkunft](catalog-lineage-user-guide.md#get-started-with-lineage).

Es gibt noch weitere Möglichkeiten, Informationen in der Herkunftsansicht zu suchen. Hierzu gehören u. a. die folgenden:

- Zeigen Sie auf der Registerkarte **Herkunft** auf Formen, um in der QuickInfo eine Vorschau zusätzlicher Informationen zur Ressource anzuzeigen.
- Wählen Sie den Knoten oder Edge aus, um den zugehörigen Ressourcentyp anzuzeigen oder Ressourcen zu wechseln.
- Auf der linken Seite der Registerkarte **Herkunft** werden die Spalten eines Datasets angezeigt. Weitere Informationen zur Herkunft auf Spaltenebene finden Sie unter [Spaltenherkunft für Dataset](catalog-lineage-user-guide.md#dataset-column-lineage).

### <a name="data-lineage-for-11-operations"></a>Datenherkunft für 1:1-Vorgänge

Das gängigste Muster für die Erfassung der Datenherkunft ist das Verschieben von Daten von einem einzelnen Eingabedataset zu einem einzelnen Ausgabedataset mit einem Prozess dazwischen.

Im Folgenden sehen Sie ein Beispiel für dieses Muster:

- 1 Quelle/Eingabe: *Customer* (SQL-Tabelle)
- 1 Senke/Ausgabe: *Customer1.csv* (Azure-Blob)
- 1 Prozess: *CopyCustomerInfo1\#Customer1.csv* (Data Factory-Kopieraktivität)

:::image type="content" source="./media/how-to-link-azure-data-factory/adf-copy-lineage.png" alt-text="Screenshot: Herkunft für einen Eins-zu-eins-Kopiervorgang von Data Factory" lightbox="./media/how-to-link-azure-data-factory/adf-copy-lineage.png":::

### <a name="data-movement-with-11-lineage-and-wildcard-support"></a>Datenverschiebung mit 1:1-Herkunft und Unterstützung für Platzhalter

Ein weiteres gängiges Szenario für die Erfassung der Herkunft ist die Verwendung eines Platzhalters zum Kopieren von Dateien aus einem einzelnen Eingabedataset in ein einzelnes Ausgabedataset. Mit dem Platzhalter kann die Kopieraktivität anhand eines gemeinsamen Dateinamensteils mehrere Dateien für den Kopiervorgang abgleichen. Azure Purview erfasst die Herkunft auf Dateiebene für jede einzelne Datei, die von der entsprechenden Kopieraktivität kopiert wurde.

Im Folgenden sehen Sie ein Beispiel für dieses Muster:

* Quelle/Eingabe: *CustomerCall\*.csv* (ADLS Gen2-Pfad)
* Senke/Ausgabe: *CustomerCall\*.csv* (Azure-Blobdatei)
* 1 Prozess: *CopyGen2ToBlob\#CustomerCall.csv* (Data Factory-Kopieraktivität)  

:::image type="content" source="./media/how-to-link-azure-data-factory/adf-copy-lineage-wildcard.png" alt-text="Screenshot: Herkunft für einen Eins-zu-eins-Kopiervorgang mit Unterstützung für Platzhalter" lightbox="./media/how-to-link-azure-data-factory/adf-copy-lineage-wildcard.png":::

### <a name="data-movement-with-n1-lineage"></a>Datenverschiebung mit n:1-Herkunft

Sie können Datenflussaktivitäten verwenden, um Datenvorgänge wie Merges, Joins usw. auszuführen. Um ein Zieldataset zu erzeugen, kann mehr als ein Quelldataset verwendet werden. In diesem Beispiel erfasst Azure Purview die Herkunft auf Dateiebene für einzelne Eingabedateien einer SQL-Tabelle, die zu einer Datenflussaktivität gehört.

Im Folgenden sehen Sie ein Beispiel für dieses Muster:

* 2 Quellen/Eingaben: *Customer.csv*, *Sales.parquet* (ADLS Gen2-Pfad)
* 1 Senke/Ausgabe: *companydata* (Azure SQL-Tabelle)
* 1 Prozess: *DataFlowBlobsToSQL* (Data Factory-Datenflussaktivität)

:::image type="content" source="./media/how-to-link-azure-data-factory/adf-data-flow-lineage.png" alt-text="Screenshot: Herkunft eines N-zu-eins-ADF-Datenflussvorgangs" lightbox="./media/how-to-link-azure-data-factory/adf-data-flow-lineage.png":::

### <a name="lineage-for-resource-sets"></a>Herkunft für Ressourcensätze

Ein Ressourcensatz ist ein logisches Objekt im Katalog, das viele Partitionsdateien im zugrunde liegenden Speicher repräsentiert. Weitere Informationen finden Sie unter [Grundlegendes zu Ressourcensätzen](concept-resource-sets.md). Wenn Azure Purview die Herkunft aus Azure Data Factory erfasst, werden die Regeln zur Normalisierung der einzelnen Partitionsdateien angewendet, und es wird ein einzelnes logisches Objekt erstellt.

Im folgenden Beispiel wird aus einem Azure-Blob ein Azure Data Lake Gen2-Ressourcensatz erzeugt:

* 1 Quelle/Eingabe: *Employee\_management.csv* (Azure-Blob)
* 1 Senke/Ausgabe: *Employee\_management.csv* (Azure Data Lake Gen2)
* 1 Prozess: *CopyBlobToAdlsGen2\_RS* (Data Factory-Kopieraktivität)

:::image type="content" source="./media/how-to-link-azure-data-factory/adf-resource-set-lineage.png" alt-text="Screenshot: Herkunft für einen Ressourcensatz" lightbox="./media/how-to-link-azure-data-factory/adf-resource-set-lineage.png":::

## <a name="next-steps"></a>Nächste Schritte

- [Benutzerhandbuch zur Katalogherkunft](catalog-lineage-user-guide.md)
- [Azure Data Share-Datenherkunft](how-to-link-azure-data-share.md)
