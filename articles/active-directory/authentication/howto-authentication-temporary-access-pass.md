---
title: Konfigurieren eines befristeten Zugriffspasses in Azure AD für die Registrierung von kennwortlosen Authentifizierungsmethoden
description: Erfahren Sie, wie Sie einen befristeten Zugriffspass (Temporary Access Pass, TAP) konfigurieren und Benutzern die Registrierung von kennwortlosen Authentifizierungsmethoden mithilfe eines befristeten Zugriffspasses ermöglichen.
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 03/03/2021
ms.author: justinha
author: inbarckms
manager: daveba
ms.reviewer: inbarckms
ms.collection: M365-identity-device-management
ms.openlocfilehash: 101e3ee9279d3560c0b561f0ea7ea695387bee15
ms.sourcegitcommit: 4b7a53cca4197db8166874831b9f93f716e38e30
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/04/2021
ms.locfileid: "102096461"
---
# <a name="configure-temporary-access-pass-in-azure-ad-to-register-passwordless-authentication-methods-preview"></a>Konfigurieren eines befristeten Zugriffspasses in Azure AD für die Registrierung von kennwortlosen Authentifizierungsmethoden (Vorschau)

Kennwortlose Authentifizierungsmethoden (wie z. B. FIDO2 und die kennwortlose Anmeldung per Telefon über die Microsoft Authenticator-App) ermöglichen Benutzern die sichere Anmeldung ohne Kennwort. Für das Bootstrapping kennwortloser Methoden stehen Benutzern zwei Möglichkeiten zur Verfügung:

- Verwenden vorhandener Azure AD Multi-Factor Authentication-Methoden 
- Verwenden eines befristeten Zugriffspasses 

Ein befristeter Zugriffspass ist ein von einem Administrator ausgegebener zeitlich begrenzter Passcode, der strenge Authentifizierungsanforderungen erfüllt und zum Integrieren anderer Authentifizierungsmethoden einschließlich kennwortloser Methoden verwendet werden kann. Ein befristeter Zugriffspass erleichtert auch die Wiederherstellung, wenn ein Benutzer seinen Faktor für die strenge Authentifizierung (z. B. FIDO2-Sicherheitsschlüssel oder Microsoft Authenticator-App) verloren oder vergessen hat, sich jedoch anmelden muss, um neue strenge Authentifizierungsmethoden zu registrieren.


In diesem Artikel wird beschrieben, wie Sie in Azure AD mit dem Azure-Portal einen befristeten Zugriffspass aktivieren und verwenden. Sie können diese Aktionen auch mithilfe der REST-APIs ausführen. 

>[!NOTE]
>Der befristete Zugriffspass befindet sich derzeit in der öffentlichen Vorschauphase. Einige Features werden möglicherweise nicht unterstützt oder sind nur eingeschränkt verwendbar. 

## <a name="enable-the-temporary-access-pass-policy"></a>Aktivieren der TAP-Richtlinie

In einer TAP-Richtlinie werden Einstellungen definiert, wie z. B. die Gültigkeitsdauer von im Mandanten erstellten Pässen oder die Benutzer und Gruppen, die einen befristeten Zugriffspass für die Anmeldung verwenden können. Bevor sich Benutzer mit einem befristeten Zugriffspass anmelden können, müssen Sie die Richtlinie für die Authentifizierungsmethode aktivieren und auswählen, welche Benutzer und Gruppen sich mit einem befristeten Zugriffspass anmelden können.
Sie können zwar einen befristeten Zugriffspass für jeden Benutzer erstellen, es können sich aber nur die in der TAP-Richtlinie enthaltenen Benutzer damit anmelden.

Globale Administratoren und Authentifizierungsmethodenrichtlinien-Administratoren können die TAP-Authentifizierungsmethodenrichtlinie aktualisieren.
So konfigurieren Sie die Authentifizierungsmethodenrichtlinie für den befristeten Zugriffspass

1. Melden Sie sich als globaler Administrator beim Azure-Portal an, und klicken Sie auf **Azure Active Directory** > **Sicherheit** > **Authentifizierungsmethoden** > **Befristeter Zugriffspass**.
1. Klicken Sie auf **Ja**, um die Richtlinie zu aktivieren, wählen Sie aus, auf welche Benutzer die Richtlinie angewendet werden soll, und wählen Sie unter **Allgemein** die gewünschten Einstellungen aus.

   ![Screenshot: Aktivieren der TAP-Authentifizierungsmethodenrichtlinie](./media/how-to-authentication-temporary-access-pass/policy.png)

   Die Standardwerte und der Bereich der zulässigen Werte werden in der folgenden Tabelle beschrieben.


   | Einstellung          | Standardwerte | Zulässige Werte               | Kommentare                                                                                                                                                                                                                                                                 |   |
   |------------------|----------------|------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---|
    Mindestlebensdauer | 1 Stunde         | 10 – 43200 Minuten (30 Tage) | Die Mindestanzahl von Minuten, die der befristete Zugriffspass gültig ist.                                                                                                                                                                                                                         |   |
   | Maximale Lebensdauer | 24 Stunden       | 10 – 43200 Minuten (30 Tage) | Die Höchstzahl von Minuten, die der befristete Zugriffspass gültig ist.                                                                                                                                                                                                                         |   |
   | Standardlebensdauer | 1 Stunde         | 10 – 43200 Minuten (30 Tage) | Die Standardwerte können von den einzelnen Pässen innerhalb der in der Richtlinie konfigurierten minimalen und maximalen Lebensdauer überschrieben werden.                                                                                                                                                |   |
   | Einmalige Verwendung     | False          | True/False                 | Wenn die Richtlinie auf „False“ festgelegt ist, können Pässe im Mandanten entweder einmal oder mehrmals während der jeweiligen Gültigkeitsdauer (maximalen Lebensdauer) verwendet werden. Durch Erzwingen der einmaligen Verwendung in der TAP-Richtlinie werden alle im Mandanten erstellten befristeten Zugriffspässe zur einmaligen Verwendung erstellt. |   |
   | Länge           | 8              | 8 – 48 Zeichen              | Definiert die Länge des Passcodes.                                                                                                                                                                                                                                      |   |

## <a name="create-a-temporary-access-pass-in-the-azure-ad-portal"></a>Erstellen eines befristeten Zugriffspasses im Azure AD-Portal

Nachdem Sie eine TAP-Richtlinie aktiviert haben, können Sie in Azure AD einen befristeten Zugriffspass für einen Benutzer erstellen. Folgende Rollen können in Bezug auf den befristeten Zugriffspass die folgenden Aktionen ausführen:

- Globale Administratoren können einen befristeten Zugriffspass für jeden Benutzer (außer für sich selbst) erstellen, löschen und anzeigen.
- Privilegierte Authentifizierungsadministratoren können einen befristeten Zugriffspass für Administratoren und Mitglieder (außer für sich selbst) erstellen, löschen und anzeigen.
- Authentifizierungsadministratoren können einen befristeten Zugriffspass für Mitglieder (außer für sich selbst) erstellen, löschen und anzeigen.
- Globale Administratoren können die Details des befristeten Zugriffspasses für den Benutzer anzeigen (ohne den Code selbst zu lesen).

So erstellen Sie einen befristeten Zugriffspass

1. Melden Sie sich als globaler Administrator, als privilegierter Authentifizierungsadministrator oder als Authentifizierungsadministrator beim Portal an. 
1. Klicken Sie auf **Azure Active Directory**, navigieren Sie zu „Benutzer“, wählen Sie einen Benutzer (z. B. *Chris Green*) aus, und wählen Sie dann **Authentifizierungsmethoden** aus.
1. Wählen Sie ggf. die Option zum **Testen der neuen Oberfläche für Benutzerauthentifizierungsmethoden** aus.
1. Wählen Sie die Option **Authentifizierungsmethoden hinzufügen** aus.
1. Klicken Sie unter **Methode auswählen** auf **Befristeter Zugriffspass (Vorschau)** .
1. Definieren Sie eine benutzerdefinierte Aktivierungszeit oder -dauer, und klicken Sie auf **Hinzufügen**.

   ![Screenshot: Erstellen eines befristeten Zugriffspasses](./media/how-to-authentication-temporary-access-pass/create.png)

1. Nach dem Hinzufügen werden die Details des befristeten Zugriffspasses angezeigt. Notieren Sie sich den tatsächlichen Wert für den befristeten Zugriffspass. Diesen Wert stellen Sie für den Benutzer bereit. Nachdem Sie auf **OK** geklickt haben, können Sie diesen Wert nicht mehr anzeigen.
   
   ![Screenshot: Details zum befristeten Zugriffspass](./media/how-to-authentication-temporary-access-pass/details.png)

## <a name="use-a-temporary-access-pass"></a>Verwenden eines befristeten Zugriffspasses

Benutzer verwenden am häufigsten einen befristeten Zugriffspass, um bei der ersten Anmeldung Authentifizierungsdaten zu registrieren, ohne zusätzliche Sicherheitsabfragen durchlaufen zu müssen. Authentifizierungsmethoden werden unter [https://aka.ms/mysecurityinfo](https://aka.ms/mysecurityinfo) registriert. Dort können Benutzer auch vorhandene Authentifizierungsmethoden aktualisieren.

1. Öffnen Sie einen Webbrowser, und navigieren Sie zu [https://aka.ms/mysecurityinfo](https://aka.ms/mysecurityinfo).
1. Geben Sie den UPN des Kontos ein, für das Sie den befristeten Zugriffspass erstellt haben, beispielsweise *tapuser@contoso.com*.
1. Wenn der Benutzer in der TAP-Richtlinie enthalten ist, wird ein Bildschirm zum Eingeben des befristeten Zugriffspasses angezeigt.
1. Geben Sie den befristeten Zugriffspass ein, der im Azure-Portal angezeigt wurde.

   ![Screenshot: Eingabe eines befristeten Zugriffspasses](./media/how-to-authentication-temporary-access-pass/enter.png)

>[!NOTE]
>Bei Verbunddomänen wird ein befristeter Zugriffspass gegenüber dem Verbund bevorzugt. Ein Benutzer mit einem befristeten Zugriffspass führt die Authentifizierung in Azure AD durch und wird nicht an den Verbundidentitätsanbieter (Identity Provider, IdP) umgeleitet.

Der Benutzer ist jetzt angemeldet und kann eine Methode (z. B. den FIDO2-Sicherheitsschlüssel) aktualisieren oder registrieren. Benutzer, die ihre Authentifizierungsmethoden aktualisieren, weil Sie ihre Anmeldeinformationen oder ihr Gerät verloren haben, sollten sicherstellen, dass sie die alten Authentifizierungsmethoden entfernen.

Benutzer können ihren befristeten Zugriffspass auch verwenden, um sich direkt über die Authenticator-App für die kennwortlose Anmeldung per Telefon zu registrieren. Weitere Informationen finden Sie unter [Hinzufügen Ihres Geschäfts-, Schul- oder Unikontos in der Microsoft Authenticator-App](../user-help/user-help-auth-app-add-work-school-account.md).

![Screenshot: Eingeben eines befristeten Zugriffspasses mit einem Geschäfts-, Schul- oder Unikonto](./media/how-to-authentication-temporary-access-pass/enter-work-school.png)

## <a name="delete-a-temporary-access-pass"></a>Löschen eines befristeten Zugriffspasses

Ein abgelaufener befristeter Zugriffspass kann nicht mehr verwendet werden. Unter den **Authentifizierungsmethoden** für einen Benutzer wird in der Spalte **Details** angezeigt, wann der befristete Zugriffspass abgelaufen ist. Sie können einen abgelaufenen befristeten Zugriffspass löschen, indem Sie die folgenden Schritte ausführen:

1. Navigieren Sie im Azure AD-Portal zu **Benutzer**, wählen Sie einen Benutzer (z. B. *TAP-Benutzer*) aus, und wählen Sie dann **Authentifizierungsmethoden** aus.
1. Wählen Sie auf der rechten Seite der in der Liste angezeigten Authentifizierungsmethode **Befristeter Zugriffspass (Vorschau)** die Option **Löschen** aus.

## <a name="replace-a-temporary-access-pass"></a>Ersetzen eines befristeten Zugriffspasses 

- Ein Benutzer kann nur über einen befristeten Zugriffspass verfügen. Der Passcode kann innerhalb der Start- und Endzeit des befristeten Zugriffspasses verwendet werden.
- Wenn der Benutzer einen neuen befristeten Zugriffspass benötigt, passiert Folgendes:
  - Wenn der vorhandene befristete Zugriffspass gültig ist, muss der Administrator den vorhandenen befristeten Zugriffspass löschen und einen neuen Pass für den Benutzer erstellen. Wenn Sie einen gültigen befristeten Zugriffspass löschen, werden die Sitzungen des Benutzers widerrufen. 
  - Wenn der vorhandene befristete Zugriffspass abgelaufen ist, wird der vorhandene befristete Zugriffspass durch einen neuen befristeten Zugriffspass überschrieben, und die Sitzungen des Benutzers werden nicht widerrufen.

Weitere Informationen zu NIST-Standards für das Onboarding und die Wiederherstellung finden Sie unter [NIST SP 800-63A](https://pages.nist.gov/800-63-3/sp800-63a.html#sec4).

## <a name="limitations"></a>Einschränkungen

Beachten Sie die folgenden Einschränkungen:

- Bei Verwendung eines einmaligen befristeten Zugriffspasses zum Registrieren einer kennwortlosen Methode wie FIDO2 oder die Anmeldung per Telefon muss der Benutzer die Registrierung innerhalb von 10 Minuten nach der Anmeldung mit dem einmaligen befristeten Zugriffspass abschließen. Diese Einschränkung gilt nicht für einen befristeten Zugriffspass, der mehrmals verwendet werden kann.
- Gastbenutzer können sich nicht mit einem befristeten Zugriffspass anmelden.
- Benutzer im Geltungsbereich der Richtlinie für die Self-Service-Kennwortzurücksetzung (Self Service Password Reset, SSPR) müssen eine der SSPR-Methoden registrieren, nachdem sie sich mit einem befristeten Zugriffspass angemeldet haben. Wenn der Benutzer nur FIDO2-Schlüssel verwendet, schließen Sie ihn von der SSPR-Richtlinie aus, oder deaktivieren Sie die SSPR-Registrierungsrichtlinie. 
- Ein befristeter Zugriffspass kann nicht mit der NPS-Erweiterung (Network Policy Server) und dem AD FS-Adapter (Active Directory Federation Services) verwendet werden.
- Wenn nahtloses einmaliges Anmelden für den Mandanten aktiviert ist, werden die Benutzer zur Eingabe eines Kennworts aufgefordert. Der Link **Verwenden Sie stattdessen Ihren befristeten Zugriffspass** ist für den Benutzer verfügbar, damit er sich mit einem befristeten Zugriffspass anmelden kann.

![Screenshot des Links „Verwenden Sie stattdessen Ihren befristeten Zugriffspass“](./media/how-to-authentication-temporary-access-pass/alternative.png)

## <a name="troubleshooting"></a>Problembehandlung    

- Wenn einem Benutzer die Verwendung eines befristeten Zugriffspass bei der Anmeldung nicht angeboten wird, überprüfen Sie Folgendes:
  - Der Benutzer befindet sich im Geltungsbereich der TAP-Authentifizierungsmethodenrichtlinie.
  - Der Benutzer verfügt über einen gültigen befristeten Zugriffspass, und wenn es sich um einen einmaligen befristeten Zugriffspass handelt, wurde er noch nicht verwendet.
- Wenn bei der Anmeldung mit einem befristeten Zugriffspass die Meldung angezeigt wird, dass die **Anmeldung mit befristetem Zugriffspass aufgrund der Richtlinie für Benutzeranmeldeinformationen blockiert wurde**, überprüfen Sie Folgendes:
  - Der Benutzer hat einen befristeten Zugriffspass für die Mehrfachverwendung, während die Authentifizierungsmethodenrichtlinie einen einmaligen befristeten Zugriffspass erfordert.
  - Ein einmaliger befristeter Zugriffspass wurde bereits verwendet.

## <a name="next-steps"></a>Nächste Schritte

- [Planen einer Bereitstellung mit kennwortloser Authentifizierung in Azure Active Directory](howto-authentication-passwordless-deployment.md)

