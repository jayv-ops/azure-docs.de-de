---
title: Zugreifen auf Azure SQL-Datenbank oder Azure Synapse Analytics mit verwalteten Identitäten – Azure Stream Analytics
description: In diesem Artikel wird die Verwendung von verwalteten Identitäten zum Authentifizieren von Azure Stream Analytics-Aufträgen für die Azure SQL-Datenbank- oder Azure Synapse Analytics-Ausgabe beschrieben.
author: enkrumah
ms.author: ebnkruma
ms.service: stream-analytics
ms.topic: how-to
ms.date: 11/30/2020
ms.openlocfilehash: e491c421f4af256b2e74fa61eb442d269bdb9e34
ms.sourcegitcommit: 8d1b97c3777684bd98f2cfbc9d440b1299a02e8f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2021
ms.locfileid: "102487915"
---
# <a name="use-managed-identities-to-access-azure-sql-database-or-azure-synapse-analytics-from-an-azure-stream-analytics-job-preview"></a>Zugreifen auf Azure SQL-Datenbank oder Azure Synapse Analytics mit verwalteten Identitäten aus einem Azure Stream Analytics-Auftrag (Vorschau)

Azure Stream Analytics unterstützt die [Authentifizierung über verwaltete Identitäten](../active-directory/managed-identities-azure-resources/overview.md) für Ausgabesenken von Azure SQL-Datenbank und Azure Synapse Analytics. Verwaltete Identitäten heben die Einschränkungen benutzerbasierter Authentifizierungsmethoden auf, wie etwa die Notwendigkeit einer erneuten Authentifizierung aufgrund von Kennwortänderungen oder Benutzertoken, die alle 90 Tage ablaufen. Wenn Sie die Notwendigkeit einer manuellen Authentifizierung aufheben, können Ihre Stream Analytics-Bereitstellungen vollständig automatisiert werden.

Bei einer verwalteten Identität handelt es sich um eine in Azure Active Directory registrierte verwaltete Anwendung, die einen bestimmten Stream Analytics-Auftrag repräsentiert. Die verwaltete Anwendung wird zur Authentifizierung bei einer Zielressource verwendet. In diesem Artikel wird gezeigt, wie Sie verwaltete Identitäten für Azure SQL-Datenbank- oder Azure Synapse Analytics-Ausgaben eines Stream Analytics-Auftrags über das Azure-Portal aktivieren.

## <a name="overview"></a>Übersicht

In diesem Artikel werden die erforderlichen Schritte zum Verbinden Ihres Stream Analytics-Auftrags mit Ihrer Azure SQL-Datenbank oder dem Azure Synapse Analytics SQL-Pool mithilfe des Authentifizierungsmodus „Verwaltete Identität“ beschrieben. 

- Sie erstellen zunächst eine dem System zugewiesene verwaltete Identität für Ihren Stream Analytics-Auftrag. Dies ist die Identität Ihres Auftrags in Azure Active Directory.  

- Fügen Sie Ihrem SQL-Server oder Synapse-Arbeitsbereich einen Active Directory-Administrator hinzu, der die Azure AD-Authentifizierung (verwaltete Identität) für diese Ressource aktiviert.

- Als nächstes erstellen Sie einen enthaltenen Benutzer, der die Identität des Stream Analytics-Auftrags in der Datenbank darstellt. Wann immer der Stream Analytics-Auftrag mit Ihrer SQL-Datenbank oder Synapse SQL DB-Ressource interagiert, ist dies die Identität, auf die er sich bezieht, um zu prüfen, über welche Berechtigungen Ihr Stream Analytics-Auftrag verfügt.

- Erteilen Sie Ihrem Stream Analytics-Auftrag die Berechtigung, auf Ihre SQL-Datenbank oder Synapse SQL-Pools zuzugreifen.

- Fügen Sie schließlich Ihre Azure SQL-Datenbank bzw. Azure Synapse Analytics als Ausgabe im Stream Analytics-Auftrag hinzu.

## <a name="prerequisites"></a>Voraussetzungen

#### <a name="azure-sql-database"></a>[Azure SQL-Datenbank](#tab/azure-sql)

Folgendes ist für die Verwendung dieses Features erforderlich:

- Ein Azure Stream Analytics-Auftrag

- Eine Azure SQL-Datenbank-Ressource

#### <a name="azure-synapse-analytics"></a>[Azure Synapse Analytics](#tab/azure-synapse)

Folgendes ist für die Verwendung dieses Features erforderlich:

- Ein Azure Stream Analytics-Auftrag

- Ein Azure Synapse Analytics-SQL-Pool

- Ein Azure Storage-Konto, das [für Ihren Stream Analytics-Auftrag konfiguriert](azure-synapse-analytics-output.md) ist

- Hinweis: Mit Synapse SQL-MSI integrierter Stream Analytics-Kontospeicher-MSI ist derzeit nicht verfügbar.

---

## <a name="create-a-managed-identity"></a>Erstellen einer verwalteten Identität

Erstellen Sie zuerst eine verwaltete Identität für den Azure Stream Analytics-Auftrag.

1. Öffnen Sie im [Azure-Portal](https://portal.azure.com) den Azure Stream Analytics-Auftrag.

1. Wählen Sie im linken Navigationsmenü unter **Konfigurieren** die Option **Verwaltete Identität** aus. Aktivieren Sie dann das Kontrollkästchen neben **Systemseitig zugewiesene verwaltete Identität verwenden**, und wählen Sie **Speichern** aus.

   ![Systemseitig zugewiesene verwaltete Identität](./media/sql-db-output-managed-identity/system-assigned-managed-identity.png)

   Ein Dienstprinzipal für die Identität des Stream Analytics-Auftrags wird in Azure Active Directory erstellt. Der Lebenszyklus der neu erstellten Identität wird von Azure verwaltet. Wenn der Stream Analytics-Auftrag gelöscht wird, wird die zugeordnete Identität (also der Dienstprinzipal) von Azure automatisch ebenfalls gelöscht.

1. Wenn Sie die Konfiguration speichern, wird die Objekt-ID (OID) des Dienstprinzipals als Prinzipal-ID aufgeführt, wie hier gezeigt: 

   ![Als Prinzipal-ID angezeigte Objekt-ID](./media/sql-db-output-managed-identity/principal-id.png)

   Der Dienstprinzipal weist den gleichen Namen auf wie der Stream Analytics-Auftrag. Wenn der Name des Auftrags z. B. *MyASAJob* lautet, erhält auch der Dienstprinzipal den Namen *MyASAJob*.

## <a name="select-an-active-directory-admin"></a>Auswählen eines Active Directory-Administrators

Nachdem Sie eine verwaltete Identität erstellt haben, wählen Sie einen Active Directory-Administrator aus.

1. Navigieren Sie zu Ihrer Azure SQL-Datenbank oder Azure Synapse Analytics SQL-Poolressource, und wählen Sie die SQL Server-Instanz bzw. den Synapse-Arbeitsbereich aus, unter dem sich die Ressource befindet. Den Link zu diesen finden Sie auf der Ressourcenübersichtsseite neben *Servername* oder *Arbeitsplatzname*.

1. Wählen Sie **Active Directory-Administrator** oder **SQL Active Directory-Administrator** entsprechend für SQL Server und Synapse-Arbeitsbereich unter **Einstellungen** aus. Wählen Sie dann **Administrator festlegen** aus.

   ![Seite „Active Directory-Administrator“](./media/sql-db-output-managed-identity/active-directory-admin-page.png)

1. Suchen Sie auf der Seite „Active Directory-Administrator“ nach einem Benutzer oder einer Gruppe, den bzw. die Sie als Administrator für die SQL Server-Instanz festlegen möchten, und klicken Sie auf **Auswählen**. Dies wird der Benutzer sein, der im nächsten Abschnitt den **Benutzer für eigenständige Datenbank** erstellen kann.

   ![Hinzufügen eines Active Directory-Administrators](./media/sql-db-output-managed-identity/add-admin.png)

   Auf der Seite „Active Directory-Administrator“ werden alle Mitglieder und Gruppen in Ihrem Active Directory-Verzeichnis angezeigt. Abgeblendete Benutzer oder Gruppen können nicht ausgewählt werden, da sie nicht als Azure Active Directory-Administratoren unterstützt werden. Siehe die Liste von unterstützten Administratoren im Abschnitt  **Funktionen und Einschränkungen von Azure Active Directory** von  [Verwenden der Azure Active Directory-Authentifizierung für die Authentifizierung mit SQL-Datenbank oder Azure Synapse](../azure-sql/database/authentication-aad-overview.md#azure-ad-features-and-limitations).

1. Wählen Sie auf der Seite **Active Directory-Administrator** die Option **Speichern** aus. Der Prozess zum Ändern des Administrators dauert einige Minuten.

## <a name="create-a-contained-database-user"></a>Erstellen eines Benutzers einer eigenständigen Datenbank

Als Nächstes erstellen Sie einen Benutzer einer eigenständigen Datenbank in Ihrer Datenbank in Azure SQL-Datenbank oder Azure Synapse, die der Azure Active Directory-Identität zugeordnet ist. Der Benutzer einer eigenständigen Datenbank verfügt über keine Anmeldung für die primäre Datenbank, ist aber einer Identität im Verzeichnis zugeordnet, die wiederum mit der Datenbank verknüpft ist. Bei der Azure Active Directory-Identität kann es sich um ein einzelnes Benutzerkonto oder eine Gruppe handeln. In diesem Fall möchten Sie einen Benutzer einer eigenständigen Datenbank für den Stream Analytics-Auftrag erstellen. 

Weitere Informationen finden Sie im folgenden Artikel zu Hintergrundinformationen der Azure AD-Integration: [Universelle Authentifizierung bei SQL-Datenbank und Azure Synapse Analytics (SSMS-Unterstützung für MFA)](../azure-sql/database/authentication-mfa-ssms-overview.md).

1. Stellen Sie über SQL Server Management Studio eine Verbindung mit Ihrer Datenbank in Azure SQL-Datenbank oder Azure Synapse her. Der **Benutzername** ist ein Azure Active Directory-Benutzer mit der Berechtigung **ALTER ANY USER**. Der Administrator, den Sie auf dem SQL Server festlegen, ist ein Beispiel dafür. Verwenden Sie die Authentifizierung **Azure Active Directory: universell mit MFA**. 

   ![Verbindung mit SQL Server herstellen](./media/sql-db-output-managed-identity/connect-sql-server.png)

   Der Servername `<SQL Server name>.database.windows.net` kann in verschiedenen Regionen unterschiedlich sein. Beispielsweise sollte in der Region China `<SQL Server name>.database.chinacloudapi.cn` verwendet werden.
 
   Sie können eine bestimmte Datenbank in Azure SQL-Datenbank oder Azure Synapse angeben. Dazu navigieren Sie zu **Optionen > Verbindungseigenschaften > Mit Datenbank verbinden**.  

   ![SQL Server-Verbindungseigenschaften](./media/sql-db-output-managed-identity/sql-server-connection-properties.png)

1. Wenn Sie zum ersten Mal eine Verbindung herstellen, wird möglicherweise das folgende Fenster angezeigt:

   ![Fenster „Neue Firewallregel“](./media/sql-db-output-managed-identity/new-firewall-rule.png)

   1. Wenn dies der Fall ist, wechseln Sie zu Ihrer SQL Server/Synapse Workspace-Ressource im Azure-Portal. Öffnen Sie unter dem Abschnitt **Sicherheit** die Seite **Firewalls und virtuelles Netzwerk/Firewalls**. 
   1. Fügen Sie eine neue Regel mit einem beliebigen Regelnamen hinzu.
   1. Verwenden Sie die *Von*-IP-Adresse im Fenster **Neue Firewallregel** als *Start-IP*.
   1. Verwenden Sie die *An*-IP-Adresse im Fenster **Neue Firewallregel** als *End-IP*. 
   1. Wählen Sie **Speichern** aus, und versuchen Sie erneut, eine Verbindung von SQL Server Management Studio herzustellen. 

1. Nachdem eine Verbindung hergestellt wurde, erstellen Sie den Benutzer einer eigenständigen Datenbank. Mit dem folgenden SQL-Befehl wird ein Benutzer einer eigenständigen Datenbank erstellt, der denselben Namen wie der Stream Analytics-Auftrag hat. Stellen Sie sicher, dass Sie *ASA_JOB_NAME* in eckige Klammern einschließen. Verwenden Sie die folgende T-SQL-Syntax, und führen Sie die Abfrage aus. 

   ```sql
   CREATE USER [ASA_JOB_NAME] FROM EXTERNAL PROVIDER; 
   ```
   
    Um zu überprüfen, ob Sie den enthaltenen Datenbankbenutzer ordnungsgemäß hinzugefügt haben, führen Sie den folgenden Befehl in SSMS unter der zugehörigen Datenbank aus und überprüfen Sie, ob sich Ihr *ASA_JOB_NAME* unter der Spalte „Name“ befindet.

   ```sql
   SELECT * FROM <SQL_DB_NAME>.sys.database_principals 
   WHERE type_desc = 'EXTERNAL_USER' 
   ```

1. Damit Microsoft Azure Active Directory überprüfen kann, ob der Stream Analytics-Auftrag Zugriff auf die SQL-Datenbank hat, müssen wir Azure Active Directory die Erlaubnis zur Kommunikation mit der Datenbank erteilen. Navigieren Sie dazu im Azure-Portal wieder auf die Seite „Firewalls und virtuelles Netzwerk/Firewalls“, und aktivieren Sie „Azure-Diensten und -Ressourcen den Zugriff auf diesen Server/Arbeitsbereich erlauben“.

   ![Firewall und virtuelles Netzwerk](./media/sql-db-output-managed-identity/allow-access.png)

## <a name="grant-stream-analytics-job-permissions"></a>Erteilen von Berechtigungen für einen Stream Analytics-Auftrag

#### <a name="azure-sql-database"></a>[Azure SQL-Datenbank](#tab/azure-sql)

Nachdem Sie einen Benutzer einer eigenständigen Datenbank erstellt und Zugriff auf Azure-Dienste erteilt haben, wie im vorherigen Abschnitt beschrieben, hat Ihr Stream Analytics-Auftrag von der verwalteten Identität die Berechtigung **CONNECT**. Damit kann er mithilfe der verwalteten Identität eine Verbindung mit Ihrer Azure SQL-Datenbank-Ressource herstellen. Es empfiehlt sich, dem Stream Analytics-Auftrag die Berechtigungen **SELECT** und **INSERT** zu erteilen, da diese später im Stream Analytics-Workflow benötigt werden. Mithilfe der **SELECT**-Berechtigung kann der Auftrag seine Verbindung mit der Tabelle in Azure SQL-Datenbank testen. Die **INSERT**-Berechtigung ermöglicht das Testen von End-to-End-Abfragen in Stream Analytics, nachdem Sie eine Eingabe und die Azure SQL-Datenbank-Ausgabe konfiguriert haben.

#### <a name="azure-synapse-analytics"></a>[Azure Synapse Analytics](#tab/azure-synapse)

Nachdem Sie einen Benutzer einer eigenständigen Datenbank erstellt und Zugriff auf Azure-Dienste erteilt haben, wie im vorherigen Abschnitt beschrieben, hat Ihr Stream Analytics-Auftrag von der verwalteten Identität die Berechtigung **CONNECT**. Damit kann er mithilfe der verwalteten Identität eine Verbindung mit Ihrer Azure Synapse-Datenbankressource herstellen. Es empfiehlt sich, dem Stream Analytics-Auftrag außerdem die Berechtigungen **SELECT**, **INSERT** und **ADMINISTER DATABASE BULK OPERATIONS** zu erteilen, da diese später im Stream Analytics-Workflow benötigt werden. Mithilfe der **SELECT**-Berechtigung kann der Auftrag seine Verbindung mit der Tabelle in der Azure Synapse-Datenbank testen. Die Berechtigungen **INSERT** und **ADMINISTER DATABASE BULK OPERATIONS** ermöglichen das Testen von End-to-End-Abfragen in Stream Analytics, nachdem Sie eine Eingabe und die Azure Synapse-Datenbankausgabe konfiguriert haben.

Zum Erteilen der Berechtigung **ADMINISTER DATABASE BULK OPERATIONS** müssen Sie dem Stream Analytics-Auftrag alle Berechtigungen gewähren, die unter [Impliziert durch Datenbankberechtigung](/sql/t-sql/statements/grant-database-permissions-transact-sql?view=azure-sqldw-latest&preserve-view=true#remarks) mit **CONTROL** bezeichnet sind. Sie benötigen diese Berechtigung, da der Stream Analytics-Auftrag die **COPY**-Anweisung ausführt, für die [ADMINISTER DATABASE BULK OPERATIONS und INSERT](/sql/t-sql/statements/copy-into-transact-sql) erforderlich sind.

---

Sie können dem Stream Analytics-Auftrag diese Berechtigungen mithilfe von SQL Server Management Studio erteilen. Weitere Informationen finden Sie in der Referenz zu GRANT (Transact-SQL).

Um nur Berechtigung für eine bestimmte Tabelle oder ein bestimmtes Objekt in der Datenbank zu erteilen, verwenden Sie die folgende T-SQL-Syntax, und führen Sie die Abfrage aus. 

#### <a name="azure-sql-database"></a>[Azure SQL-Datenbank](#tab/azure-sql)

```sql
GRANT CONNECT, SELECT, INSERT ON OBJECT::TABLE_NAME TO ASA_JOB_NAME;
```

#### <a name="azure-synapse-analytics"></a>[Azure Synapse Analytics](#tab/azure-synapse)

```sql
GRANT CONNECT, SELECT, INSERT, CONTROL, ADMINISTER DATABASE BULK OPERATIONS OBJECT::TABLE_NAME TO ASA_JOB_NAME;
```

---

Alternativ können Sie in SQL Server Management Studio mit der rechten Maustaste auf Ihre Datenbank in Azure SQL-Datenbank oder Azure Synapse klicken und **Eigenschaften > Berechtigungen** auswählen. Im Berechtigungsmenü wird der zuvor hinzugefügte Stream Analytics-Auftrag angezeigt, und Sie können nach Belieben manuell Berechtigungen erteilen oder verweigern.

Um alle Berechtigungen anzuzeigen, die Sie Ihrem Benutzer *ASA_JOB_NAME* hinzugefügt haben, führen Sie den folgenden Befehl in SSMS unter der zugehörigen Datenbank aus: 

```sql
SELECT dbprin.name, dbprin.type_desc, dbperm.permission_name, dbperm.state_desc, dbperm.class_desc, object_name(dbperm.major_id) 
FROM sys.database_principals dbprin 
LEFT JOIN sys.database_permissions dbperm 
ON dbperm.grantee_principal_id = dbprin.principal_id 
WHERE dbprin.name = '<ASA_JOB_NAME>' 
```

## <a name="create-an-azure-sql-database-or-azure-synapse-output"></a>Erstellen einer Azure SQL-Datenbank- oder Azure Synapse-Ausgabe

#### <a name="azure-sql-database"></a>[Azure SQL-Datenbank](#tab/azure-sql)

Nachdem die verwaltete Identität konfiguriert wurde, können Sie dem Stream Analytics-Auftrag eine Azure SQL-Datenbank- oder Azure Synapse-Ausgabe hinzufügen.

Vergewissern Sie sich, dass Sie in Ihrer SQL-Datenbank eine Tabelle mit dem passenden Ausgabeschema erstellt haben. Der Name dieser Tabelle ist eine der erforderlichen Eigenschaften, die eingesetzt werden müssen, wenn Sie dem Stream Analytics-Auftrag die SQL-Datenbank-Ausgabe hinzufügen. Achten Sie außerdem darauf, dass der Auftrag die Berechtigungen **SELECT** und **INSERT** besitzt, um die Verbindung zu testen und Stream Analytics-Abfragen auszuführen. Lesen Sie den Abschnitt [Erteilen von Berechtigungen für einen Stream Analytics-Auftrag](#grant-stream-analytics-job-permissions), wenn Sie dies noch nicht getan haben. 

1. Kehren Sie zum Stream Analytics-Auftrag zurück, und navigieren Sie unter **Auftragstopologie** zur Seite **Ausgaben**. 

1. Wählen Sie **Hinzufügen > SQL-Datenbank** aus. Wählen Sie im Fenster mit den Ausgabeeigenschaften der SQL-Datenbank-Ausgabesenke in der Dropdownliste mit Authentifizierungsmodi die Option **Verwaltete Identität** aus.

1. Geben Sie die restlichen Eigenschaften an. Weitere Informationen zum Erstellen einer SQL-Datenbank-Ausgabe finden Sie unter [Erstellen einer SQL-Datenbank-Ausgabe mit Stream Analytics](sql-database-output.md). Wenn Sie fertig sind, wählen Sie **Speichern** aus.

1. Nachdem Sie auf **Speichern** geklickt haben, sollte automatisch ein Verbindungstest zu Ihrer Ressource ausgelöst werden. Sobald dies erfolgreich abgeschlossen ist, haben Sie Ihren Stream Analytics-Auftrag erfolgreich für die Verbindung mit Ihrer Azure SQL-Datenbank oder Synapse SQL-Datenbank unter Verwendung des Authentifizierungsmodus „Verwaltete Identität“ konfiguriert. 

#### <a name="azure-synapse-analytics"></a>[Azure Synapse Analytics](#tab/azure-synapse)

Nachdem die verwaltete Identität und das Speicherkonto konfiguriert wurden, können Sie dem Stream Analytics-Auftrag eine Azure SQL-Datenbank- oder Azure Synapse-Ausgabe hinzufügen.

Vergewissern Sie sich, dass Sie in Ihrer Azure Synapse-Datenbank eine Tabelle mit dem passenden Ausgabeschema erstellt haben. Der Name dieser Tabelle ist eine der erforderlichen Eigenschaften, die angegeben werden müssen, wenn Sie dem Stream Analytics-Auftrag die Azure Synapse-Ausgabe hinzufügen. Achten Sie außerdem darauf, dass der Auftrag die Berechtigungen **SELECT** und **INSERT** besitzt, um die Verbindung zu testen und Stream Analytics-Abfragen auszuführen. Lesen Sie den Abschnitt [Erteilen von Berechtigungen für einen Stream Analytics-Auftrag](#grant-stream-analytics-job-permissions), wenn Sie dies noch nicht getan haben.

1. Kehren Sie zum Stream Analytics-Auftrag zurück, und navigieren Sie unter **Auftragstopologie** zur Seite **Ausgaben**.

1. Wählen Sie **Hinzufügen > Azure Synapse Analytics** aus. Wählen Sie im Fenster mit den Ausgabeeigenschaften der SQL-Datenbank-Ausgabesenke in der Dropdownliste mit Authentifizierungsmodi die Option **Verwaltete Identität** aus.

1. Geben Sie die restlichen Eigenschaften an. Weitere Informationen zum Erstellen einer Azure-Synapse-Ausgabe finden Sie unter [Azure Synapse Analytics-Ausgabe für Azure Stream Analytics](azure-synapse-analytics-output.md). Wenn Sie fertig sind, wählen Sie **Speichern** aus.

1. Nachdem Sie auf **Speichern** geklickt haben, sollte automatisch ein Verbindungstest zu Ihrer Ressource ausgelöst werden. Wenn dies erfolgreich abgeschlossen ist, können Sie jetzt mit der Verwendung der verwalteten Identität für Ihre Azure Synapse Analytics-Ressource mit Stream Analytics fortfahren. 

---

## <a name="remove-managed-identity"></a>Entfernen der verwalteten Identität

Die für einen Stream Analytics-Auftrag erstellte verwaltete Identität wird nur gelöscht, wenn der Auftrag gelöscht wird. Es gibt keine Möglichkeit, die verwaltete Identität zu löschen, ohne den Auftrag zu löschen. Wenn Sie die verwaltete Identität nicht mehr verwenden möchten, können Sie die Authentifizierungsmethode für die Ausgabe ändern. Die verwaltete Identität bleibt weiterhin bestehen, bis der Auftrag gelöscht wird, und wird verwendet, wenn Sie die Authentifizierung mit der verwalteten Identität erneut verwenden.

## <a name="next-steps"></a>Nächste Schritte

* [Grundlegendes zu den Ausgaben von Azure Stream Analytics](stream-analytics-define-outputs.md)
* [Azure Stream Analytics-Ausgabe an Azure SQL-Datenbank](stream-analytics-sql-output-perf.md)
* [Azure Synapse Analytics-Ausgabe für Azure Stream Analytics](azure-synapse-analytics-output.md)
