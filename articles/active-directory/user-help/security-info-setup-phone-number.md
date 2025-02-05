---
title: Einrichten Ihrer Telefonnummer als Überprüfungsmethode – Azure AD
description: Erfahren Sie, wie Sie Ihre Seite mit den Sicherheitsinformationen (Preview) einrichten, um Ihre Identität mithilfe Ihrer Telefonnummer und eines mobilen Geräts als Überprüfungsmethode zu verifizieren.
services: active-directory
author: curtand
manager: daveba
ms.reviewer: sahenry
ms.service: active-directory
ms.workload: identity
ms.subservice: user-help
ms.topic: end-user-help
ms.date: 02/13/2019
ms.author: curtand
ms.openlocfilehash: 7500b6b146a627dcdebacd9d2e2c7c61bc43c105
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "83744446"
---
# <a name="set-up-a-phone-number-as-your-verification-method"></a>Einrichten einer Telefonnummer als Überprüfungsmethode

Mit diesen Schritten können Sie Ihre Methoden für die zweistufige Überprüfung und die Kennwortzurücksetzung hinzufügen. Nachdem Sie die Ersteinrichtung abgeschlossen haben, können Sie zur Seite **Sicherheitsinformation** zurückkehren, um Sicherheitsinformationen hinzuzufügen, zu aktualisieren oder zu löschen.

Wenn Sie direkt nach der Anmeldung mit Ihrem Geschäfts-, Schul- oder Unikonto zur Einrichtung der Sicherheitsinformation aufgefordert werden, finden Sie weitere Informationen im Artikel [Einrichten Ihrer Sicherheitsinformation über die Aufforderung auf der Anmeldeseite](security-info-setup-signin.md).

[!INCLUDE [preview-notice](../../../includes/active-directory-end-user-preview-notice-security-info.md)]

> [!Note]
> Für die Sicherheitsinformation werden Nebenstellen nicht unterstützt. Selbst bei der Angabe im richtigen Format (+1 4255551234X12345) werden Nebenstellen vor dem Anruf entfernt.
>
> Wird keine Telefonoption angezeigt, lässt Ihre Organisation unter Umständen die Nutzung dieser Option für die Überprüfung nicht zu. In diesem Fall müssen Sie eine andere Methode auswählen oder sich an den Helpdesk Ihrer Organisation wenden, um weitere Unterstützung zu erhalten.

## <a name="security-verification-versus-password-reset-authentication"></a>Authentifizierung mit Sicherheitsüberprüfung und Kennwortzurücksetzung im Vergleich

Sicherheitsinformationsmethoden werden sowohl für die zweistufige Sicherheitsüberprüfung als auch die Kennwortzurücksetzung verwendet. Es können jedoch nicht alle Methoden für beide Verfahren verwendet werden.

| Methode | Syntaxelemente |
| ------ | -------- |
| Authenticator-App | Authentifizierung mittels zweistufiger Überprüfung und Kennwortzurücksetzung. |
| Textnachrichten | Authentifizierung mittels zweistufiger Überprüfung und Kennwortzurücksetzung. |
| Telefonanrufe | Authentifizierung mittels zweistufiger Überprüfung und Kennwortzurücksetzung. |
| Sicherheitsschlüssel | Authentifizierung mittels zweistufiger Überprüfung und Kennwortzurücksetzung. |
| E-Mail-Konto | Nur Authentifizierung mittels Kennwortzurücksetzung. Sie müssen eine andere Methode für die zweistufige Überprüfung auswählen. |
| Sicherheitsfragen | Nur Authentifizierung mittels Kennwortzurücksetzung. Sie müssen eine andere Methode für die zweistufige Überprüfung auswählen. |

## <a name="set-up-phone-calls-from-the-security-info-page"></a>Einrichten von Telefonanrufen auf der Seite „Sicherheitsinformation“

Je nach den Einstellungen Ihrer Organisation können Sie Telefonanrufe als einer Ihrer Methoden für Sicherheitsinformationen verwenden.

>[!Note]
>Wenn Sie statt eines Anrufs eine SMS erhalten möchten, führen Sie die Schritte im Artikel [Einrichten der Sicherheitsinformation zur Verwendung von SMS](security-info-setup-text-msg.md) aus.

### <a name="to-set-up-phone-calls"></a>Einrichten von Telefonanrufen

1. Melden Sie sich bei Ihrem Geschäfts-, Schul- oder Unikonto an, und rufen Sie die Seite https://myaccount.microsoft.com/ auf.

    ![Seite „Mein Profil“ mit hervorgehobenen Links zu Sicherheitsinformationen](media/security-info/securityinfo-myprofile-page.png)

2. Wählen Sie im linken Navigationsbereich den Eintrag **Sicherheitsinformation** oder den entsprechenden Link im Block **Sicherheitsinformation** aus, und klicken Sie dann auf der Seite **Sicherheitsinformation** auf **Methode hinzufügen**.

    ![Seite „Sicherheitsinformation“ mit hervorgehobener Option „Methode hinzufügen“](media/security-info/securityinfo-myprofile-addmethod-page.png)

3. Wählen Sie auf der Seite **Methode hinzufügen** in der Dropdownliste die Option **Telefon** aus, und klicken Sie dann auf **Hinzufügen**.

    ![Feld „Methode hinzufügen“ mit ausgewählter Option „Telefon“](media/security-info/securityinfo-myprofile-addphonetext.png)

4. Geben Sie auf der Seite **Telefon** die Telefonnummer für Ihr Mobilgerät ein, wählen Sie **Anrufen** aus, und wählen Sie dann **Weiter** aus.

    ![Telefonnummer hinzufügen und Telefonanrufe wählen](media/security-info/securityinfo-myprofile-phonecall-addnumber.png)

5. Nehmen Sie den Überprüfungsanruf an die von Ihnen eingegebene Telefonnummer an, und folgen Sie den Anweisungen.

    Daraufhin wird auf der Seite die erfolgreiche Ausführung angezeigt.

    ![Erfolgsmeldung, Verbindung der Telefonnummer, Auswahl des Empfangs von Telefonanrufen und Ihr Konto](media/security-info/securityinfo-myprofile-phonetext-success.png)

    Ihre Sicherheitsinformation wird aktualisiert, und Sie können bei der zweistufigen Überprüfung oder der Kennwortzurücksetzung Telefonanrufe zur Bestätigung Ihrer Identität verwenden. Wenn Sie Telefonanrufe als Standardmethode einrichten möchten, lesen Sie den Abschnitt [Ändern der Standardmethode für Sicherheitsinformationen](#change-your-default-security-info-method) in diesem Artikel.

## <a name="delete-phone-calls-from-your-security-info-methods"></a>Löschen von Telefonanrufen aus Ihren Methoden für Sicherheitsinformationen

Wenn Sie Telefonanrufe nicht mehr als Methode für Sicherheitsinformationen verwenden möchten, können Sie sie von der Seite **Sicherheitsinformation** entfernen.

>[!Important]
>Wenn Sie Telefonanrufe versehentlich löschen, gibt es keine Möglichkeit, diesen Vorgang rückgängig zu machen. In diesem Fall müssen Sie die Methode mit den Schritten im Abschnitt [Einrichten von Telefonanrufen](#set-up-phone-calls-from-the-security-info-page) dieses Artikels erneut hinzufügen.

### <a name="to-delete-phone-calls"></a>Löschen von Telefonanrufen

1. Klicken Sie auf der Seite **Sicherheitsinformation** auf den Link **Löschen** neben der Option **Telefon**.

    ![Link zum Löschen der E-Mail-Methode aus den Sicherheitsinformationen](media/security-info/securityinfo-myprofile-phonetext-delete.png)

2. Klicken Sie im Bestätigungsfeld auf **Ja**, um die **Telefonnummer** zu löschen. Nachdem die Telefonnummer gelöscht wurde, wird sie aus den Sicherheitsinformationen entfernt und auf der Seite **Sicherheitsinformation** nicht mehr angezeigt. Wenn **Telefon** Ihre Standardmethode war, wird der Standardwert in eine andere verfügbare Methode geändert.

## <a name="change-your-default-security-info-method"></a>Ändern der Standardmethode für Sicherheitsinformationen

Wenn Sie Telefonanrufe als Standardmethode für das Anmelden bei Ihrem Geschäfts-, Schul- oder Unikonto mit zweistufiger Überprüfung oder für das Anfordern von Kennwortzurücksetzungen verwenden möchten, können Sie diese Option auf der Seite **Sicherheitsinformation** einrichten.

### <a name="to-change-your-default-security-info-method"></a>So ändern Sie die Standardmethode für Sicherheitsinformationen

1. Wählen Sie auf der Seite **Sicherheitsinformation** den Link **Ändern** neben **Standardmäßige Anmeldemethode** aus.

    ![Link zum Ändern der Standardanmeldemethode](media/security-info/securityinfo-myprofile-phonetext-defaultchange.png)

2. Wählen Sie aus der Dropdownliste der verfügbaren Methoden die Option **Telefon – (*_Ihre_Telefonnummer_*) anrufen** aus, und klicken Sie dann auf **Bestätigen**.

    ![Standardmäßige Anmeldemethode auswählen](media/security-info/securityinfo-myprofile-phonecall-changeddefault.png)

    Die Standardmethode für Anmeldungen ändert sich zu **Telefon – (*_Ihre_Telefonnummer_*) anrufen**.

## <a name="additional-security-info-methods"></a>Weitere Methoden für Sicherheitsinformationen

Basierend auf der Aktion, die Sie ausführen möchten, stehen zusätzliche Möglichkeiten zur Verfügung, wie Ihre Organisation sich mit Ihnen in Verbindung setzen kann, um Ihre Identität zu überprüfen. Die Optionen lauten:

- **Authentifikator-App:** Sie können eine Authentifikator-App herunterladen und verwenden, um entweder eine Genehmigungsbenachrichtigung oder einen nach dem Zufallsprinzip generierten Genehmigungscode für die zweistufige Überprüfung oder die Kennwortzurücksetzung zu erhalten. Ausführliche Anweisungen zum Einrichten und Verwenden der Microsoft Authenticator-App finden Sie unter [Einrichten der Sicherheitsinformation zur Verwendung einer Authentifikator-App](security-info-setup-auth-app.md).

- **SMS an ein mobiles Gerät:** Geben Sie die Nummer Ihres mobilen Geräts an, und Sie erhalten eine SMS mit dem Code, den Sie für die zweistufige Überprüfung oder die Kennwortzurücksetzung verwenden. Ausführliche Anweisungen dazu, wie Sie Ihre Identität mit einer Textnachricht (SMS) bestätigen, finden Sie unter [Einrichten der Sicherheitsinformation zur Verwendung von SMS](security-info-setup-text-msg.md).

- **Sicherheitsschlüssel:** Registrieren Sie Ihren Microsoft-kompatiblen Sicherheitsschlüssel, und verwenden Sie ihn zusammen mit einer PIN für die zweistufige Überprüfung oder die Kennwortzurücksetzung. Unter [Einrichten der Sicherheitsinformationen zur Verwendung eines Sicherheitsschlüssels (Vorschau)](security-info-setup-security-key.md) erfahren Sie Schritt für Schritt, wie Sie Ihre Identität mithilfe eines Sicherheitsschlüssels bestätigen.

- **E-Mail-Adresse:** Geben Sie Ihre Geschäfts-, Schul- oder Uni-E-Mail-Adresse an, um eine E-Mail für die Kennwortzurücksetzung zu erhalten. Diese Option ist für die zweistufige Überprüfung nicht verfügbar. Ausführliche Anweisungen zum Einrichten Ihrer E-Mail-Adresse finden Sie unter [Einrichten der Sicherheitsinformation zur Verwendung einer E-Mail-Adresse](security-info-setup-email.md).

- **Sicherheitsfragen:** Beantworten Sie einige Sicherheitsfragen, die von Ihrem Administrator für Ihre Organisation erstellt wurden. Diese Option ist nur für die Kennwortzurücksetzung verfügbar, nicht für die zweistufige Überprüfung. Ausführliche Anweisungen zum Einrichten der Sicherheitsfragen finden Sie im Artikel [Einrichten der Sicherheitsinformation zur Verwendung von Sicherheitsfragen](security-info-setup-questions.md).

    >[!Note]
    >Wenn einige dieser Optionen fehlen, sind die entsprechenden Methoden in Ihrer Organisation wahrscheinlich nicht zugelassen. In diesem Fall müssen Sie eine verfügbare Methode auswählen oder sich an Ihren Administrator wenden, um weitere Unterstützung zu erhalten.

## <a name="next-steps"></a>Nächste Schritte

- Setzen Sie Ihr Kennwort zurück, wenn Sie es verloren oder vergessen haben. Verwenden Sie dazu das [Portal für die Kennwortzurücksetzung](https://passwordreset.microsoftonline.com/), oder führen Sie die Schritte im Artikel [Reset your work or school password](active-directory-passwords-update-your-own-password.md) (Zurücksetzen des Kennworts eines Geschäfts-, Schul- oder Unikontos) aus.

- Der Artikel [Wenn Sie sich nicht bei Ihrem Microsoft-Konto anmelden können](https://support.microsoft.com/help/12429/microsoft-account-sign-in-cant) enthält Tipps zur Problembehandlung bei Anmeldeproblemen.
