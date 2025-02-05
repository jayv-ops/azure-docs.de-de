---
title: Überwachen der Anwendungsanmeldeintegrität für mehr Resilienz in Azure Active Directory
description: Erstellen Sie Abfragen und Benachrichtigungen, um die Integrität der Anmeldungen bei Ihren Anwendungen zu überwachen.
services: active-directory
author: BarbaraSelden
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 01/10/2021
ms.author: baselden
ms.reviewer: ajburnle
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: ad99c8d319a22f8b5388838b9d537de2f610478a
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2021
ms.locfileid: "101650990"
---
# <a name="monitoring-application-sign-in-health-for-resilience"></a>Überwachen der Anwendungsanmeldeintegrität für mehr Resilienz

Richten Sie zum Erhöhen der Widerstandsfähigkeit bzw. Resilienz der Infrastruktur die Überwachung der Anwendungsanmeldeintegrität für Ihre kritischen Anwendungen ein, damit Sie beim Auftreten eines beeinträchtigenden Vorfalls eine Warnung erhalten. Als Unterstützung bei dieser Arbeit können Sie auf Basis der Arbeitsmappe „Anwendungsanmeldeintegrität“ Warnungen konfigurieren. 

Mit dieser Arbeitsmappe können Administratoren Authentifizierungsanforderungen für Anwendungen in ihrem Mandanten überwachen. Folgende Hauptfunktionen sind verfügbar:

* Konfigurieren der Arbeitsmappe, um alle oder einzelne Apps in nahezu Echtzeit zu überwachen

* Konfigurieren von Warnungen bei Änderungen des Authentifizierungsmusters, um den Vorgang zu untersuchen und entsprechende Maßnahmen einzuleiten

* Vergleichen von Trends über einen bestimmten Zeitraum, z. B. wochenweise (dies ist auch die Standardeinstellung der Arbeitsmappe)

> [!NOTE]
> Informationen zu allen verfügbaren Arbeitsmappen und den Voraussetzungen für deren Verwendung finden Sie unter [Verwenden von Azure Monitor-Arbeitsmappen für Berichte](../reports-monitoring/howto-use-azure-monitor-workbooks.md).

Bei einem beeinträchtigenden Ereignis können zwei Dinge passieren:

* Die Anzahl der Anmeldungen für eine Anwendung fällt steil ab, da Benutzer sich nicht anmelden können.

* Die Anzahl der Anmeldefehler nimmt zu. 

In diesem Artikel wird schrittweise das Einrichten der Arbeitsmappe „Anwendungsanmeldeintegrität“ zum Überwachen von möglichen Unterbrechungen bei Benutzeranmeldungen erläutert.

## <a name="prerequisites"></a>Voraussetzungen 

* Ein Azure AD-Mandant.

* Ein Benutzer mit der Rolle „globaler Administrator“ oder „Sicherheitsadministrator“ für den Azure AD-Mandanten.

* Ein Log Analytics-Arbeitsbereich in Ihrem Azure-Abonnement, um Protokolle an Azure Monitor-Protokolle zu senden. 

   * Lesen Sie hierzu [Erstellen eines Log Analytics-Arbeitsbereichs](../../azure-monitor/logs/quick-create-workspace.md)

* In Azure Monitor-Protokolle integrierte Azure AD-Protokolle.

   * Lesen Sie hierzu [Integrieren von Azure AD-Anmeldeprotokollen in den Azure Monitor-Stream.](../reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md)

 

## <a name="configure-the-app-sign-in-health-workbook"></a>Konfigurieren der Arbeitsmappe „Anwendungsanmeldeintegrität“ 

Öffnen Sie das **Azure-Portal**, wählen Sie **Azure Active Directory** aus, und wählen Sie dann **Arbeitsmappen** aus, um auf die Arbeitsmappen zuzugreifen.

Die jeweiligen Arbeitsmappen werden unter „Nutzung“, „Bedingter Zugriff“ und „Problembehandlung“ angezeigt. Die Arbeitsmappe „Anwendungsanmeldeintegrität“ wird im Abschnitt „Nutzung“ angezeigt.

Sobald Sie eine-Arbeitsmappe verwendet haben, wird sie wahrscheinlich unter „Zuletzt geänderte Arbeitsmappen“ angezeigt.

![Screenshot: Arbeitsmappenkatalog im Azure-Portal](./media/monitor-sign-in-health-for-resilience/sign-in-health-workbook.png)


Mithilfe der Arbeitsmappe „Anwendungsanmeldeintegrität“ können Sie visualisieren, was mit Anmeldungen passiert. 

Standardmäßig enthält die Arbeitsmappe zwei Diagramme. Diese Diagramme vergleichen das aktuelle Geschehen bei Ihrer Anwendung (oder mehreren Anwendungen) mit dem gleichen Zeitraum vor einer Woche. Die blauen Linien stellen das aktuelle Geschehen, die orangefarbenen Linien das Geschehen in der vorigen Woche dar.

![Screenshot: Diagramme zur Anmeldeintegrität](./media/monitor-sign-in-health-for-resilience/sign-in-health-graphs.png)

**Das erste Diagramm stellt die Nutzung pro Stunde (Anzahl erfolgreicher Benutzer) dar**. Das Vergleichen der aktuellen Anzahl erfolgreicher Benutzer mit einem typischen Nutzungszeitraum hilft Ihnen, einen Nutzungsabfall zu erkennen, der möglicherweise untersucht werden muss. Ein Abfall der Nutzungsrate mit erfolgreichen Anmeldungen kann beim Erkennen von Leistungs- und Auslastungsprobleme helfen, was mit der Fehlerrate nicht möglich ist. Beispiel: Wenn Benutzer Ihre Anwendung nicht erreichen können, um einen Anmeldeversuch auszuführen, tritt kein Fehler auf, sondern nur ein Abfall der Nutzungsrate. Eine Beispielabfrage für diese Daten finden Sie im folgenden Abschnitt.

Das zweite Diagramm bildet die Fehlerrate pro Stunde ab. Ein Spitzenwert bei der Fehlerrate kann auf ein Problem mit Ihren Authentifizierungsmechanismen hinweisen. Die Fehlerrate kann nur gemessen werden, wenn Benutzer die Möglichkeit haben, einen Authentifizierungsversuch zu starten. Wenn Benutzer keinen Zugriff erhalten, um einen Authentifizierungsversuch zu starten, werden keine Fehler angezeigt.

Sie können eine Warnung konfigurieren, um eine bestimmte Gruppe zu benachrichtigen, wenn die Nutzungs- oder Fehlerrate einen angegebenen Schwellenwert überschreitet. Eine Beispielabfrage für diese Daten finden Sie im folgenden Abschnitt.

 ## <a name="configure-the-query-and-alerts"></a>Konfigurieren von Abfrage und Warnungen

Sie können Warnungsregeln in Azure Monitor erstellen und in regelmäßigen Abständen automatisch gespeicherte Abfragen oder benutzerdefinierte Protokollsuchen ausführen.

Erstellen Sie anhand der folgenden Anweisungen E-Mail-Warnungen, die auf den in den Diagrammen dargestellten Abfragen basieren. Mit den unten aufgeführten Beispielskripts werden unter folgenden Bedingungen E-Mail-Benachrichtigungen gesendet:

* Die Nutzungsrate erfolgreicher Anmeldungen ist gegenüber der gleichen Stunde vor zwei Tagen um 90 % gefallen (siehe Diagramm der Nutzung pro Stunde im vorherigen Abschnitt). 

* Die Fehlerrate ist gegenüber der gleichen Stunde vor zwei Tagen um 90 % gestiegen (siehe Diagramm der Fehlerrate pro Stunde im vorherigen Abschnitt). 

 Führen Sie die folgenden Schritte aus, um die zugrunde liegende Abfrage zu konfigurieren und Warnungen festzulegen. Die Beispielabfrage wird als Grundlage für Ihre Konfiguration verwendet. Eine Erläuterung der Abfragestruktur finden Sie am Ende dieses Abschnitts.

Weitere Informationen zum Erstellen, Anzeigen und Verwalten von Protokollwarnungen mit Azure Monitor finden Sie unter [Verwalten von Protokollwarnungen](../../azure-monitor/alerts/alerts-log.md).

 
1. Wählen Sie in der Arbeitsmappe **Bearbeiten** aus, und wählen Sie dann das **Abfragesymbol** direkt über der rechten Seite des Diagramms aus.   

   [![Screenshot: Bearbeiten der Arbeitsmappe](./media/monitor-sign-in-health-for-resilience/edit-workbook.png)](./media/monitor-sign-in-health-for-resilience/edit-workbook.png)

   Das Abfrageprotokoll wird geöffnet.

  [![Screenshot: Abfrageprotokoll](./media/monitor-sign-in-health-for-resilience/query-log.png)](/media/monitor-sign-in-health-for-resilience/query-log.png)
‎

2. Kopieren Sie eines der folgenden Beispielskripts für eine neue Kusto-Abfrage.

**Kusto-Abfrage für Nutzungsabfall**

```Kusto

let thisWeek = SigninLogs

| where TimeGenerated > ago(1h)

| project TimeGenerated, AppDisplayName, UserPrincipalName

//| where AppDisplayName contains "Office 365 Exchange Online"

| summarize users = dcount(UserPrincipalName) by bin(TimeGenerated, 1hr)

| sort by TimeGenerated desc

| serialize rn = row_number();

let lastWeek = SigninLogs

| where TimeGenerated between((ago(1h) - totimespan(2d))..(now() - totimespan(2d)))

| project TimeGenerated, AppDisplayName, UserPrincipalName

//| where AppDisplayName contains "Office 365 Exchange Online"

| summarize usersPriorWeek = dcount(UserPrincipalName) by bin(TimeGenerated, 1hr)

| sort by TimeGenerated desc

| serialize rn = row_number();

thisWeek

| join

(

 lastWeek

)

on rn

| project TimeGenerated, users, usersPriorWeek, difference = abs(users - usersPriorWeek), max = max_of(users, usersPriorWeek)

| where (difference * 2.0) / max > 0.9

```

 

**Kusto-Abfrage für Anstieg der Fehlerrate**


```kusto

let thisWeek = SigninLogs

| where TimeGenerated > ago(1 h)

| project TimeGenerated, UserPrincipalName, AppDisplayName, status = case(Status.errorCode == "0", "success", "failure")

| where AppDisplayName == **APP NAME**

| summarize success = countif(status == "success"), failure = countif(status == "failure") by bin(TimeGenerated, 1h)

| project TimeGenerated, failureRate = (failure * 1.0) / ((failure + success) * 1.0)

| sort by TimeGenerated desc

| serialize rn = row_number();

let lastWeek = SigninLogs

| where TimeGenerated between((ago(1 h) - totimespan(2d))..(ago(1h) - totimespan(2d)))

| project TimeGenerated, UserPrincipalName, AppDisplayName, status = case(Status.errorCode == "0", "success", "failure")

| where AppDisplayName == **APP NAME**

| summarize success = countif(status == "success"), failure = countif(status == "failure") by bin(TimeGenerated, 1h)

| project TimeGenerated, failureRatePriorWeek = (failure * 1.0) / ((failure + success) * 1.0)

| sort by TimeGenerated desc

| serialize rn = row_number();

thisWeek

| join (lastWeek) on rn

| project TimeGenerated, failureRate, failureRatePriorWeek

| where abs(failureRate – failureRatePriorWeek) > **THRESHOLD VALUE**

```

3. Fügen Sie die Abfrage im Fenster ein, und wählen Sie dann **Ausführen** aus. Stellen Sie sicher, dass die in der nachstehenden Abbildung dargestellte Meldung „Abgeschlossen“ angezeigt wird und unterhalb dieser Meldung die Ergebnisse angegeben werden.

   [![Screenshot: Ergebnisse der ausgeführten Abfrage](./media/monitor-sign-in-health-for-resilience/run-query.png)](./media/monitor-sign-in-health-for-resilience/run-query.png)

4. Markieren Sie die Abfrage, und wählen Sie **+ Neue Warnungsregel** aus. 
 
   [![Screenshot: Bildschirm für neue Warnungsregel](./media/monitor-sign-in-health-for-resilience/new-alert-rule.png)](./media/monitor-sign-in-health-for-resilience/new-alert-rule.png)


5. Konfigurieren Sie die Warnungsbedingungen. Wählen Sie im Abschnitt „Bedingung“ den Link **Wenn der Mittelwert der benutzerdefinierten Protokollsuche größer ist als <in der Logik definierte Anzahl>** aus. Scrollen Sie im Bereich „Signallogik konfigurieren“ zu „Warnungslogik“.

   [![Screenshot: Bildschirm zum Konfigurieren von Warnungen](./media/monitor-sign-in-health-for-resilience/configure-alerts.png)](./media/monitor-sign-in-health-for-resilience/configure-alerts.png)
 
   * **Schwellenwert**: 0. Mit diesem Wert erfolgt eine Warnung bei allen Ergebnissen.

   * **Auswertungszeitraum (in Minuten)** : 60. Mit diesem Wert wird eine Zeitstunde betrachtet.

   * **Häufigkeit (in Minuten)** : 60. Mit diesem Wert wird der Auswertungszeitraum auf einmal pro Stunde für die vorherige Stunde festgelegt.

   * Wählen Sie **Fertig** aus.

6. Konfigurieren Sie im Abschnitt **Aktionen** die folgenden Einstellungen:  

    [![Screenshot: Seite „Warnungsregel erstellen“](./media/monitor-sign-in-health-for-resilience/create-alert-rule.png)](./media/monitor-sign-in-health-for-resilience/create-alert-rule.png)

   * Wählen Sie unter **Aktionen** die Option **Aktionsgruppe auswählen** aus, und fügen Sie die Gruppe hinzu, die bei Warnungen benachrichtigt werden soll.

   * Wählen Sie unter **Aktionen anpassen** die Option **E-Mail-Warnungen** aus.

   * Fügen Sie eine **Betreffzeile** hinzu.

7. Konfigurieren Sie unter **Warnungsregeldetails** die folgenden Einstellungen:

   * Fügen Sie einen aussagekräftigen Namen und eine Beschreibung hinzu.

   * Wählen Sie die **Ressourcengruppe** aus, der die Warnung hinzugefügt werden soll.

   * Wählen Sie für die Warnung den standardmäßigen **Schweregrad** aus.

   * Wählen Sie **Warnungsregel bei Erstellung aktivieren** aus, wenn sie sofort aktiviert werden soll. Wählen Sie andernfalls **Warnungen unterdrücken** aus.

8. Wählen Sie **Warnungsregel erstellen** aus.

9. Wählen Sie **Speichern** aus, geben Sie einen Namen für die Abfrage ein, und wählen Sie dann unter „Speichern unter“**Abfrage und unter „Kategorie“ Warnung** aus. Wählen Sie dann erneut **Speichern** aus.  

   [![Screenshot: Schaltfläche zum Speichern der Abfrage](./media/monitor-sign-in-health-for-resilience/save-query.png)](./media/monitor-sign-in-health-for-resilience/save-query.png)



### <a name="refine-your-queries-and-alerts"></a>Optimieren von Abfragen und Warnungen
Modifizieren Sie Ihre Abfragen und Warnungen, um maximale Effektivität zu erzielen.

* Denken Sie unbedingt daran, Ihre Warnungen zu testen.

* Ändern Sie Empfindlichkeit und Häufigkeit der Warnung, damit Sie wichtige Benachrichtigungen erhalten. Administratoren können gegenüber Warnungen abstumpfen, wenn sie zu viele Warnungen erhalten, und dabei Wichtiges übersehen. 

* Stellen Sie sicher, dass die E-Mail-Adresse, von der Warnungen bei den E-Mail-Clients des Administrators eingehen, der Liste der zulässigen Absender hinzugefügt wird. Andernfalls können Ihnen Benachrichtigungen aufgrund eines Spamfilters auf dem E-Mail-Client entgehen. 

* Die Warnungsabfrage in Azure Monitor kann nur Ergebnisse der letzten 48 Stunden enthalten. [Dies ist eine aktuelle systembedingte Einschränkung](https://github.com/MicrosoftDocs/azure-docs/issues/22637).

## <a name="create-processes-to-manage-alerts"></a>Erstellen von Prozessen zum Verwalten von Warnungen

Nachdem Sie Abfrage und Warnungen eingerichtet haben, erstellen Sie Geschäftsprozesse, um die Warnungen zu verwalten.

* Wer überwacht wann die Arbeitsmappe?
* Wer führt die Untersuchung durch, wenn eine Warnung generiert wird?

* Welche Anforderungen gelten für die Kommunikation? Wer ist für die Kommunikation verantwortlich und an wen ist sie gerichtet?

* Welche Geschäftsprozesse müssen bei einem eventuellen Ausfall ausgelöst werden?

## <a name="next-steps"></a>Nächste Schritte

[Weitere Informationen zu Arbeitsmappen](../reports-monitoring/howto-use-azure-monitor-workbooks.md)

 

 

