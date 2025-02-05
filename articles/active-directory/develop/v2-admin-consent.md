---
title: Protokolle zur Administratoreinwilligung auf Microsoft Identity Platform
description: Beschreibung der Autorisierung mit Bereichen, Berechtigungen und Zustimmung in Microsoft Identity Platform.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: conceptual
ms.date: 12/18/2020
ms.author: ryanwi
ms.reviewer: hirsin
ms.custom: aaddev
ms.openlocfilehash: b6fb5f680dfa5e2c87533083e3df4c2bae1ed12a
ms.sourcegitcommit: 4b7a53cca4197db8166874831b9f93f716e38e30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "102097022"
---
# <a name="admin-consent-on-the-microsoft-identity-platform"></a>Administratoreinwilligung auf Microsoft Identity Platform

Einige Berechtigungen erfordern eine Administratoreneinwilligung, bevor sie innerhalb eines Mandanten erteilt werden können.  Sie können auch den Endpunkt für die Administratoreinwilligung verwenden, um Berechtigungen für einen gesamten Mandanten zu erteilen.

## <a name="recommended-sign-the-user-into-your-app"></a>Empfohlen: Anmelden des Benutzers bei Ihrer App

Beim Erstellen einer Anwendung, die den Endpunkt für die Administratorzustimmung verwendet, benötigt die App in der Regel eine Seite/Ansicht, die dem Administrator das Genehmigen der App-Berechtigungen gestattet. Diese Seite kann Teil des Anmelde-Flows der App, Teil der App-Einstellungen oder ein dedizierter Flow „Verbinden“ sein. In vielen Fällen ist es sinnvoll, wenn die App diese Ansicht „Verbinden“ erst anzeigt, wenn ein Benutzer sich mit einem Geschäfts-, Schul- oder Unikonto von Microsoft angemeldet hat.

Durch das Anmelden des Benutzers bei der App können Sie die Organisation identifizieren, der der Administrator angehört, bevor Sie zur Genehmigung der nötigen Berechtigungen auffordern. Auch wenn es nicht unbedingt erforderlich ist, können Sie für Ihre Organisationsbenutzer auf diese Weise eine intuitivere Benutzeroberfläche erstellen. Gehen Sie gemäß den [Tutorials zum Microsoft Identity Platform-Protokoll](active-directory-v2-protocols.md) vor, um den Benutzer anzumelden.

## <a name="request-the-permissions-from-a-directory-admin"></a>Anfordern der Berechtigungen von einem Verzeichnisadministrator

Wenn Sie dazu bereit sind, vom Administrator der Organisation Berechtigungen anzufordern, können Sie den Benutzer zum *Endpunkt für die Administratorzustimmung* von Microsoft Identity Platform umleiten.

```none
https://login.microsoftonline.com/{tenant}/v2.0/adminconsent
        ?client_id=6731de76-14a6-49ae-97bc-6eba6914391e
        &scope=https://graph.microsoft.com/Calendars.Read https://graph.microsoft.com/Mail.Send
        &redirect_uri=http://localhost/myapp/permissions
        &state=12345
```

| Parameter | Bedingung | BESCHREIBUNG |
| :--- | :--- | :--- |
| `tenant` | Erforderlich | Der Verzeichnismandant, von dem Sie die Berechtigung anfordern möchten. Kann als eindeutiger Bezeichner oder Anzeigename bereitgestellt oder mit `organizations` generisch referenziert werden, wie im Beispiel gezeigt. Verwenden Sie nicht „Allgemein“, weil persönliche Konten die Administratoreinwilligung nur im Kontext eines Mandanten bereitstellen können. Um die bestmögliche Kompatibilität mit persönlichen Konten sicherzustellen, die Mandanten verwalten, sollten Sie nach Möglichkeit die Mandanten-ID verwenden. |
| `client_id` | Erforderlich | Die **Anwendungs-ID (Client-ID)** , die Ihrer App im [Azure-Portal auf der Seite „App-Registrierungen“](https://go.microsoft.com/fwlink/?linkid=2083908) zugewiesen wurde. |
| `redirect_uri` | Erforderlich |Der Umleitungs-URI, an den die Antwort zur Verarbeitung durch die App gesendet werden soll. Er muss genau mit einem der Umleitungs-URIs übereinstimmen, die Sie im Portal registriert haben. |
| `state` | Empfohlen | Ein in der Anforderung enthaltener Wert, der auch in der Antwort zurückgegeben wird. Es kann sich um eine Zeichenfolge mit jedem beliebigen Inhalt handeln. Der Status wird verwendet, um Informationen über den Status des Benutzers in der App zu codieren, bevor die Authentifizierungsanforderung aufgetreten ist, z.B. Informationen zu der Seite oder Ansicht, die der Benutzer besucht hat. |
|`scope` | Erforderlich | Definiert den von der Anwendung angeforderten Satz an Berechtigungen. Dies können statische (mit `/.default`) oder dynamische Bereiche sein. Dies kann auch die OIDC-Bereiche (`openid`, `profile`, `email`) einschließen. |

An diesem Punkt erzwingt Azure AD, dass sich nur ein Mandantenadministrator anmelden kann, um die Anforderung abzuschließen. Der Administrator wird aufgefordert, alle Berechtigungen zu genehmigen, die Sie für den Parameter `scope` angefordert haben.  Wenn Sie einen statischen Wert (`/.default`) verwendet haben, funktioniert er wie der v1.0-Endpunkt für die Administratoreinwilligung und fordert die Zustimmung für alle Bereiche an, die in den erforderlichen Berechtigungen gefunden werden (Benutzer und App). Zum Anfordern von App-Berechtigungen muss der Wert `/.default` verwendet werden. Wenn Administratoren bei Verwendung von `/.default` eine bestimmte Berechtigung nicht jedes Mal auf dem Bildschirm für die Administratoreinwilligung angezeigt werden soll, empfiehlt es sich, die Berechtigung nicht im Abschnitt für erforderliche Berechtigungen zu platzieren. Stattdessen können Sie eine dynamische Einwilligung verwenden, um die gewünschten Berechtigungen für den Einwilligungsbildschirm zur Laufzeit hinzuzufügen, anstatt `/.default` zu verwenden.

### <a name="successful-response"></a>Erfolgreiche Antwort

Wenn der Administrator die Berechtigungen für Ihre Anwendung genehmigt, lautet die erfolgreiche Antwort wie folgt:

```none
http://localhost/myapp/permissions
    ?admin_consent=True
    &tenant=fa00d692-e9c7-4460-a743-29f2956fd429
    &scope=https://graph.microsoft.com/Calendars.Read https://graph.microsoft.com/Mail.Send
    &state=12345
```

| Parameter | BESCHREIBUNG |
| :--- | :--- |
| `tenant`| Der Verzeichnismandant, der Ihrer Anwendung die angeforderten Berechtigungen erteilt hat, im GUID-Format.|
| `state` | Ein in der Anforderung enthaltener Wert, der auch in der Tokenantwort zurückgegeben wird. Es kann sich um eine Zeichenfolge mit jedem beliebigen Inhalt handeln. Der Status wird verwendet, um Informationen über den Status des Benutzers in der App zu codieren, bevor die Authentifizierungsanforderung aufgetreten ist, z.B. Informationen zu der Seite oder Ansicht, die der Benutzer besucht hat.|
| `scope` | Die Berechtigungen, zu denen der App Zugang gewährt wurde.|
| `admin_consent` | Wird auf `True` festgelegt.|

### <a name="error-response"></a>Fehlerantwort

```none
http://localhost/myapp/permissions
        ?admin_consent=True
        &tenant=fa15d692-e9c7-4460-a743-29f2956fd429
        &error=consent_required
        &error_description=AADSTS65004%3a+The+resource+owner+or+authorization+server+denied+the+request.%0d%0aTrace+ID%3a+d320620c-3d56-42bc-bc45-4cdd85c41f00%0d%0aCorrelation+ID%3a+8478d534-5b2c-4325-8c2c-51395c342c89%0d%0aTimestamp%3a+2019-09-24+18%3a34%3a26Z
        &state=12345
```

Wenn Sie zu den Parametern hinzugefügt werden, die in einer erfolgreichen Antwort angezeigt werden, werden Fehlerparameter wie unten dargestellt.

| Parameter | BESCHREIBUNG |
|:-------------------|:-------------------------------------------------------------------------------------------------|
| `error` | Eine Fehlercodezeichenfolge, die verwendet werden kann, um unterschiedliche Arten auftretender Fehler zu klassifizieren und um auf Fehler zu reagieren.|
| `error_description` | Eine spezifische Fehlermeldung, mit der ein Entwickler die Grundursache eines Fehlers identifizieren kann.|
| `tenant`| Der Verzeichnismandant, der Ihrer Anwendung die angeforderten Berechtigungen erteilt hat, im GUID-Format.|
| `state` | Ein in der Anforderung enthaltener Wert, der auch in der Tokenantwort zurückgegeben wird. Es kann sich um eine Zeichenfolge mit jedem beliebigen Inhalt handeln. Der Status wird verwendet, um Informationen über den Status des Benutzers in der App zu codieren, bevor die Authentifizierungsanforderung aufgetreten ist, z.B. Informationen zu der Seite oder Ansicht, die der Benutzer besucht hat.|
| `admin_consent` | Wird auf `True` festgelegt, um anzugeben, dass diese Antwort bei einem Datenfluss zur Administratoreinwilligung aufgetreten ist.|

## <a name="next-steps"></a>Nächste Schritte
- Lesen Sie die Informationen unter [Anmelden von Azure Active Directory-Benutzern mit dem mehrinstanzenfähigen Anwendungsmuster](howto-convert-app-to-be-multi-tenant.md).
- Ausführlichere Informationen zur [OAuth 2.0-Protokollunterstützung für Zustimmung während des Flows zum Gewähren des Autorisierungscodes](v2-oauth2-auth-code-flow.md#request-an-authorization-code).
- Ausführlichere Informationen zur [Verwendung des Zustimmungsframeworks durch mehrinstanzenfähige Anwendungen](./howto-convert-app-to-be-multi-tenant.md) zur Implementierung von Benutzer- und Administratoreinwilligung mit Unterstützung für erweiterte Anwendungsmuster mit mehreren Ebenen.
- Grundlegendes zu [Einwilligungserfahrungen für Azure AD-Anwendungen](application-consent-experience.md)
