---
title: Beheben von Verbindungsproblemen – Azure Database for PostgreSQL – Einzelserver
description: Erfahren Sie, wie Sie Verbindungsprobleme mit einem Azure Database for PostgreSQL-Einzelserver beheben.
keywords: PostgreSQL-Verbindung, Verbindungszeichenfolge, Verbindungsprobleme, vorübergehender Fehler, Verbindungsfehler
author: niklarin
ms.author: nlarin
ms.service: postgresql
ms.topic: how-to
ms.date: 5/6/2019
ms.openlocfilehash: bff930153dc8941fbfe561edf963d5b1c1e7811f
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "96014617"
---
# <a name="troubleshoot-connection-issues-to-azure-database-for-postgresql---single-server"></a>Beheben von Verbindungsproblemen mit Azure Database for PostgreSQL – Einzelserver

Verbindungsprobleme können verschiedene Ursachen haben:

* Firewalleinstellungen
* Verbindungstimeout
* Falsche Anmeldeinformationen
* Maximales Limit für einige Azure Database for PostgreSQL-Ressourcen erreicht
* Probleme mit der Infrastruktur des Diensts
* Derzeit ausgeführte Wartung des Diensts
* Computezuteilung des Servers wird durch das Skalieren der Anzahl von virtuellen Kernen oder den Wechsel in eine andere Dienstebene geändert.

Im Allgemeinen können Verbindungsprobleme mit Azure Database for PostgreSQL wie folgt klassifiziert werden:

* Vorübergehende Fehler (kurzlebige oder zeitweilige Fehler)
* Dauerhafte oder nicht vorübergehende Fehler (Fehler, die sich regelmäßig wiederholen)

## <a name="troubleshoot-transient-errors"></a>Problembehandlung bei vorübergehenden Fehlern

Vorübergehende Fehler treten auf, wenn eine Wartung durchgeführt wird, wenn das System einen Hardware- oder Softwarefehler findet, wenn Sie die virtuellen Kerne oder die Dienstebene des Servers ändern. Der Azure Database for PostgreSQL-Dienst verfügt über integrierte Hochverfügbarkeit und ist dafür ausgelegt, derartige Probleme automatisch zu umgehen. Allerdings wird für eine kurze Zeit die Verbindung Ihrer Anwendung mit dem Server getrennt – in der Regel für weniger als 60 Sekunden. Das Beenden dieser Ereignisse kann manchmal länger dauern, z.B. wenn eine große Transaktion eine lang andauernde Wiederherstellung auslöst.

### <a name="steps-to-resolve-transient-connectivity-issues"></a>Schritte zum Beheben vorübergehender Verbindungsprobleme

1. Überprüfen Sie das [Microsoft Azure-Dienstdashboard](https://azure.microsoft.com/status) auf alle bekannten Ausfälle, die während der Fehlermeldung durch die Anwendung aufgetreten sind.
2. Für Anwendungen, die Verbindungen mit einem Clouddienst wie Azure Database for PostgreSQL herstellen, sollten Sie vorübergehende Fehler erwarten. Daher sollten Sie eine Wiederholungslogik zur Fehlerbehandlung implementieren, anstatt Benutzern diese Anwendungsfehler anzuzeigen. Bewährte Methoden und Entwurfsrichtlinien für das Behandeln vorübergehender Fehler finden Sie unter [Behandeln vorübergehender Konnektivitätsfehler für Azure Database for PostgreSQL](concepts-connectivity.md).
3. Wenn sich ein Server seinen Ressourcenlimits nähert, kann dies wie ein vorübergehendes Verbindungsproblem aussehen. Weitere Informationen finden Sie unter [Einschränkungen in Azure Database for PostgreSQL](concepts-limits.md).
4. Wenn Verbindungsprobleme weiterhin bestehen, die Fehlerdauer 60 Sekunden überschreitet oder der Fehler an einem Tag mehrfach auftritt, schicken Sie eine Azure-Supportanfrage, indem Sie auf der **Azure-Support** -Website [Support erhalten](https://azure.microsoft.com/support/options) auswählen.

## <a name="troubleshoot-persistent-errors"></a>Problembehandlung bei dauerhaften Fehlern

Wenn die Anwendung dauerhaft keine Verbindung mit Azure Database for PostgreSQL herstellen kann, weist dies in der Regel auf Probleme der folgenden Art hin:

* Konfiguration der Serverfirewall: Stellen Sie sicher, dass die Firewall auf dem Azure Database for PostgreSQL-Server so konfiguriert ist, dass Verbindungen von Ihrem Client zulässig sind, einschließlich Proxyserver und Gateways.
* Konfiguration der Clientfirewall: Die Firewall auf dem Client muss Verbindungen mit Ihrem Datenbankserver zulassen. IP-Adressen und Ports des Servers, mit dem Sie eine Verbindung herstellen, sowie Anwendungsnamen wie PostgreSQL müssen in einigen Firewalls zugelassen sein.
* Benutzerfehler: Sie haben möglicherweise Verbindungsparameter wie den Servernamen in der Verbindungszeichenfolge falsch geschrieben, oder es fehlt das Suffix *\@Servername* im Benutzernamen.
* Sollte ein Fehler mit dem Hinweis angezeigt werden, dass der _Server nicht für das Zulassen von IPv6-Verbindungen konfiguriert ist_, beachten Sie, dass im Basic-Tarif keine VNET-Dienstendpunkte unterstützt werden. Entfernen Sie den Endpunkt „Microsoft.Sql“ aus dem Subnetz, das mit dem Basic-Server verbunden werden soll.
* Wenn der Verbindungsfehler _sslmode value „***“ invalid when SSL support is not compiled in_ (sslmode-Wert „***“ ungültig, wenn keine SSL-Unterstützung mitkompiliert wurde) angezeigt wird, bedeutet das, dass der PostgreSQL-Client SSL nicht unterstützt. Höchstwahrscheinlich wurde die clientseitige libpq nicht mit dem Flag „--with-openssl“ kompiliert. Versuchen Sie, eine Verbindung mit einem PostgreSQL-Client herzustellen, der SSL unterstützt. 

### <a name="steps-to-resolve-persistent-connectivity-issues"></a>Schritte zum Beheben dauerhafter Verbindungsprobleme

1. Richten Sie [Firewallregeln](howto-manage-firewall-using-portal.md) so ein, dass die IP-Adresse des Clients zugelassen wird. Richten Sie nur zu Testzwecken vorübergehend eine Firewallregel mit 0.0.0.0 als erster IP-Adresse und 255.255.255.255 als letzter IP-Adresse fest. Dadurch wird der Server für alle IP-Adressen geöffnet. Wird Ihr Konnektivitätsproblem dadurch behoben, entfernen Sie die Regel, und erstellen Sie eine Firewallregel für eine entsprechend eingeschränkte IP-Adresse bzw. einen entsprechend eingeschränkten IP-Adressbereich.
2. Stellen Sie für alle Firewalls zwischen Client und Internet sicher, dass Port 5432 für ausgehende Verbindungen geöffnet ist.
3. Überprüfen Sie die Verbindungszeichenfolge und andere Verbindungseinstellungen.
4. Überprüfen Sie im Dashboard den Dienststatus. Wenn Sie denken, dass es sich um einen regionalen Ausfall handelt, finden Sie unter [Übersicht über die Geschäftskontinuität mit Azure Database for PostgreSQL](concepts-business-continuity.md) Schritte zur Wiederherstellung in einer neuen Region.

## <a name="next-steps"></a>Nächste Schritte

* [Behandeln vorübergehender Konnektivitätsfehler für Azure Database for PostgreSQL](concepts-connectivity.md)
