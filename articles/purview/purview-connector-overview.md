---
title: Purview-Connector – Übersicht
description: In diesem Artikel sind die verschiedenen in Purview unterstützten Datenspeicher und Funktionen skizziert.
author: chandrakavya
ms.author: kchandra
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: conceptual
ms.date: 11/13/2020
ms.openlocfilehash: 08b22af8743082bab1d547205e51917cb9d92a11
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101695769"
---
# <a name="supported-data-stores"></a>Unterstützte Datenspeicher

Purview unterstützt die folgenden Datenspeicher. Klicken Sie auf die einzelnen Datenspeicher, um mehr über die unterstützten Funktionen und die entsprechende Konfiguration zu erfahren.

## <a name="purview-data-sources"></a>Purview-Datenquellen

|**Kategorie**|  **Datenspeicher**  |**Metadatenextrahierung**|**Vollständige Überprüfung**|**Inkrementelle Überprüfung**|**Bereichsbezogene Überprüfung**|**Klassifizierung**|**Herkunft**|
|---|---|---|---|---|---|---|---|
| Azure | [Azure Blob Storage](register-scan-azure-blob-storage-source.md)| Ja| Ja| Ja| Ja| Ja| Ja|
||[Azure Cosmos DB](register-scan-azure-cosmos-database.md)|Ja| Ja| Ja| Ja| Ja| Ja|
||[Azure Data Explorer](register-scan-azure-data-explorer.md)|Ja| Ja| Ja| Ja| Ja| Ja|
||[Azure Data Lake Storage Gen1](register-scan-adls-gen1.md)|Ja| Ja| Ja| Ja| Ja| Ja|
||[Azure Data Lake Storage Gen2](register-scan-adls-gen2.md)|Ja| Ja| Ja| Ja| Ja| Ja|
||[Azure SQL-Datenbank](register-scan-azure-sql-database.md)|Ja| Ja| Nein| Ja| Ja| Ja|
||[Verwaltete Azure SQL-Datenbank-Instanz](register-scan-azure-sql-database-managed-instance.md)|Ja| Ja| Nein| Ja| Ja| Ja|
||[Azure Synapse Analytics (ehemals SQL DW)](register-scan-azure-synapse-analytics.md)|Ja| Ja| Nein| Ja| Ja| Ja|
|Datenbank|[Oracle Database](register-scan-oracle-source.md)|Ja| Ja| Nein| Nein| Nein| Ja|
||[SQL Server](register-scan-on-premises-sql-server.md)|Ja| Ja| Nein| Ja| Ja| Ja|
||[Teradata](register-scan-teradata-source.md)|Ja| Ja| Nein| Nein| Nein| Ja|
|Power BI|[Power BI](register-scan-power-bi-tenant.md)|Ja| Ja| Nein| Nein| Nein| Ja|
|Dienste und Apps|[SAP ECC](register-scan-sapecc-source.md)|Ja| Ja| Nein| Ja| Ja| Ja|
||[SAP S4HANA](register-scan-saps4hana-source.md)|Ja| Ja| Nein| Ja| Ja| Ja|

## <a name="next-steps"></a>Nächste Schritte

- [Registrieren und Überprüfen von Azure Blob Storage](register-scan-azure-blob-storage-source.md)