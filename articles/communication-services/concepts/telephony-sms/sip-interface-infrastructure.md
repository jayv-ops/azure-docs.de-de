---
title: 'Infrastrukturanforderungen für die SIP-Schnittstelle: Azure Communication Services'
description: Machen Sie sich mit den Infrastrukturanforderungen für die Konfiguration der SIP-Schnittstelle von Azure Communication Services vertraut.
author: boris-bazilevskiy
manager: nmurav
services: azure-communication-services
ms.author: bobazile
ms.date: 03/10/2021
ms.topic: overview
ms.service: azure-communication-services
ms.openlocfilehash: ede650ae072ef53ed40a9372a292ab69fe8cc1af
ms.sourcegitcommit: 4bda786435578ec7d6d94c72ca8642ce47ac628a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/16/2021
ms.locfileid: "103492726"
---
# <a name="sip-interface-infrastructure-requirements"></a>Infrastrukturanforderungen für die SIP-Schnittstelle 

[!INCLUDE [Private Preview Notice](../../includes/private-preview-include.md)]

 
Dieser Artikel enthält ausführliche Informationen zur Infrastruktur, Lizenzierung und SBC-Konnektivität (Session Border Controller), die bei der Planung Ihrer SIP-Schnittstellenbereitstellung berücksichtigt werden sollten.


## <a name="infrastructure-requirements"></a>Infrastrukturanforderungen
In der folgenden Tabelle sind die Infrastrukturanforderungen für die unterstützten SBCs und Domänen sowie andere Netzwerkkonnektivitätsanforderungen für die Bereitstellung der SIP-Schnittstelle aufgeführt:  

|Infrastrukturanforderung|Voraussetzung|
|:--- |:--- |
|Session Border Controller (SBC)|Unterstützter SBC. Weitere Informationen finden Sie unter [Unterstützte Session Border Controller (SBC)](#supported-session-border-controllers-sbcs).|
|Mit dem SBC verbundene Telefonie-Trunks|Mindestens ein mit dem SBC verbundener Telefonie-Trunk. Auf einer Seite ist der SBC über die SIP-Schnittstelle mit der Azure Communication Services-Instanz verbunden. Der SBC kann auch Verbindungen mit Drittanbieter-Telefonieentitäten wie Nebenstellenanlagen, analogen Telefonieadaptern und Ähnlichem herstellen. Jede mit dem SBC verbundene Festnetz-Konnektivitätsoption funktioniert. (Informationen zur Konfiguration der Festnetz-Trunks für den SBC erhalten Sie vom jeweiligen SBC- oder Trunk-Anbieter.)|
|Azure-Abonnement|Ein Azure-Abonnement zum Erstellen der ACS-Ressource sowie die Konfiguration und die Verbindung mit dem SBC.|
|Communication Services-Zugriffstoken|Um Anrufe tätigen zu können, benötigen Sie ein gültiges Zugriffstoken mit dem Bereich `voip`. Weitere Informationen finden Sie unter [Zugriffstoken](../identity-model.md#access-tokens).|
|Öffentliche IP-Adresse für den SBC|Eine öffentliche IP-Adresse, die verwendet werden kann, um eine Verbindung mit dem SBC herzustellen. Basierend auf dem SBC-Typ kann der SBC NAT verwenden.|
|Vollqualifizierter Domänenname (Fully Qualified Domain Name, FQDN) für den SBC|Ein FQDN für den SBC, bei dem der Domänenteil des FQDN keiner registrierten Domäne in Ihrer Microsoft 365- oder Office 365-Organisation entspricht. Weitere Informationen finden Sie unter [SBC-Domänennamen](#sbc-domain-names).|
|Öffentlicher DNS-Eintrag für den SBC |Ein öffentlicher DNS-Eintrag, der den SBC-FQDN der öffentlichen IP-Adresse zuordnet. |
|Öffentliches vertrauenswürdiges Zertifikat für den SBC |Ein Zertifikat für den SBC, das für die gesamte Kommunikation mit der SIP-Schnittstelle verwendet wird. Weitere Informationen finden Sie unter [Öffentliches vertrauenswürdiges Zertifikat für den SBC](#public-trusted-certificate-for-the-sbc).|
|Firewall-IP-Adressen und -Ports für SIP-Signalisierung und Medien |Der SBC kommuniziert mit folgenden Diensten in der Cloud:<br/><br/>SIP-Proxy (zur Behandlung der Signalisierung)<br/>Medienprozessor (zur Behandlung von Medien)<br/><br/>Die beiden Dienste verfügen in Microsoft Cloud über unterschiedliche IP-Adressen. Weitere Informationen finden Sie weiter unten in diesem Dokument.


## <a name="sbc-domain-names"></a>SBC-Domänennamen

Kunden ohne Office 365 können einen beliebigen Domänennamen verwenden, für den Sie ein öffentliches Zertifikat beziehen können.

Die folgende Tabelle enthält Beispiele für DNS-Namen, die für den Mandanten registriert sind. Außerdem ist angegeben, ob der Name als vollqualifizierter Domänenname (Fully Qualified Domain Name, FQDN) für den SBC verwendet werden kann, und es werden Beispiele für gültige FQDNs bereitgestellt:

|DNS-Name|Geeignet als SBC-FQDN?|Beispiele für FQDNs|
|:--- |:--- |:--- |
contoso.com|Ja|**Gültige Namen:**<br/>sbc1.contoso.com<br/>ssbcs15.contoso.com<br/>europe.contoso.com|
|contoso.onmicrosoft.com|Nein|Domänen vom Typ „*.onmicrosoft.com“ werden für SBC-Namen nicht unterstützt.

Bei Office 365-Kunden darf der SBC-Domänenname keiner im Office 365-Mandanten registrierten Domäne entsprechen. Das folgende Beispiel zeigt die Koexistenz von Office 365 und Azure Communication Services:

|In Office 365 registrierte Domäne|SBC-FQDN-Beispiele in Teams|SBC-FQDN-Beispiele in ACS|
|:--- |:--- |:--- |
**contoso.com** (Domäne der zweiten Ebene)|**sbc.contoso.com** (Name in der Domäne der zweiten Ebene)|**sbc.acs.contoso.com** (Name in der Domäne der dritten Ebene)<br/>**sbc.fabrikam.com** (beliebiger Name in einer anderen Domäne)|
|**o365.contoso.com** (Domäne der dritten Ebene)|**sbc.o365.contoso.com** (Name in der Domäne der dritten Ebene)|**sbc.contoso.com** (Name in der Domäne der zweiten Ebene)<br/>**sbc.acs.o365.contoso.com** (Name in der Domäne der vierten Ebene)<br/>**sbc.fabrikam.com** (beliebiger Name in einer anderen Domäne)

Die SBC-Kopplung erfolgt auf der Communication Services-Ressourcenebene. Dadurch können viele SBCs zu einer einzelnen Communication Services-Ressource zusammengefasst werden. Ein einzelner SBC kann jedoch nicht mit mehreren Communication Services-Ressourcen gekoppelt werden. Für die Kopplung mit unterschiedlichen Ressourcen sind eindeutige SBC-FQDNs erforderlich.

## <a name="public-trusted-certificate-for-the-sbc"></a>Öffentliches vertrauenswürdiges Zertifikat für den SBC

Microsoft empfiehlt, das Zertifikat für den SBC durch Generieren einer Zertifikatsignieranforderung (Certificate Signing Request, CSR) anzufordern. Eine spezielle Anleitung zum Erstellen einer CSR für einen SBC finden Sie in der Verbindungsanleitung oder in der Dokumentation Ihres SBC-Anbieters. 

  > [!NOTE]
  > Bei den meisten Zertifizierungsstellen (Certificate Authorities, CA) muss die Größe des privaten Schlüssels mindestens 2.048 betragen. Berücksichtigen Sie dies beim Generieren der CSR.

Für das Zertifikat muss der SBC-FQDN als allgemeiner Name (Common Name, CN) oder als alternativer Antragstellername (Subject Alternative Name, SAN) verwendet werden. Das Zertifikat muss direkt von einer Zertifizierungsstelle ausgestellt werden, nicht von einem Zwischenanbieter.

Alternativ unterstützt die SIP-Schnittstelle von Communication Services auch einen Platzhalter in CN und/oder SAN. Der Platzhalter muss dem Standard [RFC HTTP Over TLS](https://tools.ietf.org/html/rfc2818#section-3.1) entsprechen. 

Ein Beispiel wäre die Verwendung von `\*.contoso.com`, was dem SBC-FQDN `sbc.contoso.com`, aber nicht `sbc.test.contoso.com` entsprechen würde.

Das Zertifikat muss von einer der folgenden Stammzertifizierungsstellen generiert werden:

- AffirmTrust
- AddTrust External CA Root
- Baltimore CyberTrust Root*
- Buypass
- Cybertrust
- Class 3 Public Primary Certification Authority
- Comodo Secure Root CA
- Deutsche Telekom 
- DigiCert Global Root CA
- DigiCert High Assurance EV Root CA
- Entrust
- GlobalSign
- Go Daddy
- GeoTrust
- Verisign, Inc. 
- SSL.com
- Starfield
- Symantec Enterprise Mobile Root for Microsoft 
- SwissSign
- Thawte Timestamping CA
- Trustwave
- TeliaSonera 
- T-Systems International GmbH (Deutsche Telekom)
- QuoVadis

Microsoft arbeitet daran, weitere, von Kunden gewünschte Zertifizierungsstellen hinzuzufügen. 

## <a name="sip-signaling-fqdns"></a>SIP-Signalisierung: FQDNs 

Die Verbindungspunkte für die SIP-Schnittstelle von Communication Services sind die drei folgenden FQDNs:

- **sip.pstnhub.microsoft.com**: Globaler FQDN. Muss zuerst verwendet werden. Wenn der SBC eine Anforderung zum Auflösen dieses Namens sendet, wird von den Microsoft Azure DNS-Servern eine IP-Adresse zurückgegeben, die auf das primäre Azure-Rechenzentrum verweist, das dem SBC zugewiesen ist. Die Zuweisung basiert auf Leistungsmetriken der Rechenzentren und der geografischen Nähe zum SBC. Die zurückgegebene IP-Adresse entspricht dem primären FQDN.
- **sip2.pstnhub.microsoft.com**: Sekundärer FQDN. Geografisch der zweiten Prioritätsregion zugeordnet.
- **sip3.pstnhub.microsoft.com**: Tertiärer FQDN. Geografisch der dritten Prioritätsregion zugeordnet.

Die Reihenfolge dieser drei FQDNs ist für Folgendes erforderlich:

- Optimale Erfahrung (weniger Auslastung und am nächsten zum zugewiesenen SBC-Rechenzentrum durch Abfragen des ersten FQDN)
- Failover, wenn von einem SBC eine Verbindung mit einem Rechenzentrum hergestellt wird, bei dem ein vorübergehendes Problem auftritt. Weitere Informationen finden Sie weiter unten unter [Failovermechanismus für die SIP-Signalisierung](#failover-mechanism-for-sip-signaling).  

Die FQDNs („sip.pstnhub.microsoft.com“, „sip2.pstnhub.microsoft.com“ und „sip3.pstnhub.microsoft.com“) werden zu einer der folgenden IP-Adressen aufgelöst:

- `52.114.148.0`
- `52.114.132.46`
- `52.114.75.24` 
- `52.114.76.76` 
- `52.114.7.24` 
- `52.114.14.70`
- `52.114.16.74`
- `52.114.20.29`

Öffnen Sie Firewallports für diese IP-Adressen, um ein- und ausgehenden Datenverkehr der Adressen für die Signalisierung zuzulassen. Wenn Ihre Firewall DNS-Namen unterstützt, wird der FQDN `sip-all.pstnhub.microsoft.com` zu allen diesen IP-Adressen aufgelöst. 

## <a name="sip-signaling-ports"></a>SIP-Signalisierung: Ports

Verwenden Sie die folgenden Ports für die SIP-Schnittstelle von Communication Services:

|Verkehr|From|Beschreibung|Quellport|Zielport|
|:--- |:--- |:--- |:--- |:--- |
|SIP/TLS|SIP-Proxy|SBC|1024–65535|Auf dem SBC definiert. (Für Office 365 GCC High/DoD darf nur der Port 5061 verwendet werden.)|
SIP/TLS|SBC|SIP-Proxy|Auf dem SBC definiert|5061|

### <a name="failover-mechanism-for-sip-signaling"></a>Failovermechanismus für die SIP-Signalisierung

Vom SBC wird eine DNS-Abfrage zum Auflösen von „sip.pstnhub.microsoft.com“ gesendet. Das primäre Rechenzentrum wird basierend auf dem SBC-Standort und den Leistungsmetriken des Rechenzentrums ausgewählt. Liegt im primären Rechenzentrum ein Problem vor, wird „sip2.pstnhub.microsoft.com“ verwendet. Dieser FQDN wird zum zweiten zugewiesenen Rechenzentrum aufgelöst. Sollten Rechenzentren in zwei Regionen nicht verfügbar sein (was äußerst selten vorkommt), wird der letzte FQDN (sip3.pstnhub.microsoft.com) verwendet, der die IP-Adresse des tertiären Rechenzentrums ergibt.

## <a name="media-traffic-ip-and-port-ranges"></a>Mediendatenverkehr: IP- und Portbereiche

Der Mediendatenverkehr fließt zu und aus einem separaten Dienst, der als Medienprozessor bezeichnet wird. Zum Zeitpunkt der Veröffentlichung kann der Medienprozessor für ACS eine beliebige Azure-IP-Adresse verwenden. Die vollständige Liste der Adressen können Sie [hier](https://www.microsoft.com/download/details.aspx?id=56519) herunterladen.

### <a name="port-range"></a>Portbereich
Die folgende Tabelle enthält den Portbereich der Medienprozessoren: 

|Verkehr|From|Beschreibung|Quellport|Zielport|
|:--- |:--- |:--- |:--- |:--- |
|UDP/SRTP|Medienprozessor|SBC|3478–3481 und 49152–53247|Auf dem SBC definiert|
|UDP/SRTP|SBC|Medienprozessor|Auf dem SBC definiert|3478–3481 und 49152–53247|

  > [!NOTE]
  > Microsoft empfiehlt mindestens zwei Ports pro gleichzeitigem Aufruf auf dem SBC.


## <a name="media-traffic-media-processors-geography"></a>Mediendatenverkehr: Geografie von Medienprozessoren

Der Mediendatenverkehr wird über sogenannte Medienprozessoren geleitet. Medienprozessoren werden in den gleichen Rechenzentren platziert wie SIP-Proxys. Außerdem gibt es zusätzliche Medienprozessoren zur Optimierung des Medienflusses. Beispielsweise gibt es derzeit keine SIP-Proxykomponente in Australien (SIP-Datenverkehr wird über Singapur oder Hongkong SAR abgewickelt), der Medienprozessor ist jedoch lokal in Australien vorhanden. Der Bedarf für lokale Medienprozessoren geht auf die Wartezeit zurück, die auftritt, wenn Datenverkehr über weite Strecken gesendet wird (beispielsweise von Australien nach Singapur oder Hongkong SAR). Im Beispiel von Datenverkehr, der von Australien nach Hongkong SAR oder Singapur gesendet wird, lässt sich für SIP-Datenverkehr trotz Wartezeit eine gute Gesprächsqualität erreichen. Für Echtzeit-Mediendatenverkehr ist sie allerdings nicht akzeptabel.

Standorte, an denen sowohl SIP-Proxy- als auch Medienprozessorkomponenten bereitgestellt werden:
- USA (zwei in den Rechenzentren der Regionen „USA, Westen“ und „USA, Osten“)
- Europa (Rechenzentren in Amsterdam und Dublin)
- Asien (Rechenzentren in Singapur und Hongkong SAR)
- Australien (Rechenzentren in den Regionen „Australien, Osten“ und „Australien, Südosten“)

Standorte, an denen nur Medienprozessoren bereitgestellt werden (SIP-Flows über das nächstgelegene Rechenzentrum):
- Japan (Rechenzentren in den Regionen „Japan, Osten“ und „Japan, Westen“)


## <a name="media-traffic-codecs"></a>Mediendatenverkehr: Codecs

### <a name="leg-between-sbc-and-cloud-media-processor-or-microsoft-teams-client"></a>Abschnitt zwischen SBC und Cloudmedienprozessor oder Microsoft Teams-Client.
Gilt sowohl für Medienumgehung als auch für Fälle ohne Medienumgehung.

Von der Schnittstelle für direktes Routing für den Abschnitt zwischen dem Session Border Controller und dem Cloudmedienprozessor können folgende Codecs verwendet werden:

- SILK, G.711, G.722, G.729

Sie können die Verwendung des spezifischen Codecs für den Session Border Controller erzwingen, indem Sie unerwünschte Codecs aus dem Angebot ausschließen.

### <a name="leg-between-acs-sdk-app-and-cloud-media-processor"></a>Abschnitt zwischen der ACS SDK-App und dem Cloudmedienprozessor

Für den Abschnitt zwischen dem Cloudmedienprozessor und der ACS SDK-App wird entweder SILK oder G.722 verwendet. Die Codec-Wahl für diesen Abschnitt basiert auf Microsoft-Algorithmen, von denen mehrere Parameter berücksichtigt werden. 

## <a name="supported-session-border-controllers-sbcs"></a>Unterstützte Session Border Controller (SBCs)

Die Zertifizierung ist in Arbeit. In der Zwischenzeit können Kunden [für Teams zertifizierte Session Border Controller](/MicrosoftTeams/direct-routing-border-controllers) verwenden. 

## <a name="next-steps"></a>Nächste Schritte

### <a name="conceptual-documentation"></a>Dokumentation

- [Telefoniekonzepte](./telephony-concept.md)
- [Telefonnummerntypen in Azure Communication Services](./plan-solution.md)
- [Preise](../pricing.md)

### <a name="quickstarts"></a>Schnellstarts

- [Schnellstart: Auf Telefon anrufen](../../quickstarts/voice-video-calling/pstn-call.md)