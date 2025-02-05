---
title: 'Tutorial: Azure Active Directory-Integration in Cisco Umbrella | Microsoft-Dokumentation'
description: Erfahren Sie, wie Sie einmaliges Anmelden (Single Sign-On, SSO) zwischen Azure Active Directory und Cisco Umbrella konfigurieren.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 02/09/2021
ms.author: jeedes
ms.openlocfilehash: cfb8ee9972d19ce07e2d681533d30c4794ddca28
ms.sourcegitcommit: b4647f06c0953435af3cb24baaf6d15a5a761a9c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/02/2021
ms.locfileid: "101649065"
---
# <a name="tutorial-azure-active-directory-integration-with-cisco-umbrella"></a>Tutorial: Azure Active Directory-Integration in Cisco Umbrella

In diesem Tutorial erfahren Sie, wie Sie Cisco Umbrella in Azure Active Directory (Azure AD) integrieren. Die Integration von Cisco Umbrella in Azure AD ermöglicht Folgendes:

* Steuern Sie in Azure AD, wer Zugriff auf Cisco Umbrella hat.
* Ermöglichen Sie es Ihren Benutzern, sich mit ihren Azure AD-Konten automatisch bei Cisco Umbrella anzumelden.
* Verwalten Sie Ihre Konten zentral im Azure-Portal.

## <a name="prerequisites"></a>Voraussetzungen

Für die ersten Schritte benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Falls Sie über kein Abonnement verfügen, können Sie ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/) verwenden.
* Ein Cisco Umbrella-Abonnement, für das einmaliges Anmelden (Single Sign-On, SSO) aktiviert ist

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung.

* Cisco Umbrella unterstützt **SP- und IDP-initiiertes** einmaliges Anmelden.

## <a name="add-cisco-umbrella-from-the-gallery"></a>Hinzufügen von Cisco Umbrella aus dem Katalog

Zum Konfigurieren der Integration von Cisco Umbrella in Azure AD müssen Sie Cisco Umbrella aus dem Katalog zu Ihrer Liste verwalteter SaaS-Apps hinzufügen.

1. Melden Sie sich mit einem Geschäfts-, Schul- oder Unikonto oder mit einem persönlichen Microsoft-Konto beim Azure-Portal an.
1. Wählen Sie im linken Navigationsbereich den Dienst **Azure Active Directory** aus.
1. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie dann **Alle Anwendungen** aus.
1. Wählen Sie zum Hinzufügen einer neuen Anwendung **Neue Anwendung** aus.
1. Geben Sie im Abschnitt **Aus Katalog hinzufügen** den Suchbegriff **Cisco Umbrella** in das Suchfeld ein.
1. Wählen Sie im Ergebnisbereich **Cisco Umbrella** aus, und fügen Sie dann die App hinzu. Warten Sie einige Sekunden, während die App Ihrem Mandanten hinzugefügt wird.

## <a name="configure-and-test-azure-ad-sso-for-cisco-umbrella"></a>Konfigurieren und Testen des einmaligen Anmeldens von Azure AD für Cisco Umbrella

Konfigurieren und testen Sie das einmalige Anmelden von Azure AD mit Cisco Umbrella mithilfe eines Testbenutzers mit dem Namen **B. Simon**. Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Cisco Umbrella eingerichtet werden.

Führen Sie zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD mit Cisco Umbrella die folgenden Schritte aus:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-sso)** , um Ihren Benutzern die Verwendung dieses Features zu ermöglichen.
    1. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)** , um das einmalige Anmelden von Azure AD mit dem Testbenutzer B. Simon zu testen.
    1. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)** , um B. Simon die Verwendung des einmaligen Anmeldens von Azure AD zu ermöglichen.
1. **[Konfigurieren des einmaligen Anmeldens für Cisco Umbrella](#configure-cisco-umbrella-sso)** , um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren
    1. **[Erstellen eines Cisco Umbrella-Testbenutzers](#create-cisco-umbrella-test-user)** , um eine Entsprechung von B. Simon in Cisco Umbrella zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist
1. **[Testen des einmaligen Anmeldens](#test-sso)** , um zu überprüfen, ob die Konfiguration funktioniert

## <a name="configure-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens (Single Sign-On, SSO) von Azure AD

Gehen Sie wie folgt vor, um das einmalige Anmelden von Azure AD im Azure-Portal zu aktivieren.

1. Navigieren Sie im Azure-Portal auf der Anwendungsintegrationsseite für **Cisco Umbrella** zum Abschnitt **Verwalten**, und wählen Sie **Einmaliges Anmelden** aus.
1. Wählen Sie auf der Seite **SSO-Methode auswählen** die Methode **SAML** aus.
1. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Stiftsymbol für **Grundlegende SAML-Konfiguration**, um die Einstellungen zu bearbeiten.

   ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

4. Im Abschnitt **Grundlegende SAML-Konfiguration** muss der Benutzer keine Schritte ausführen, weil die App bereits in Azure integriert ist.

    a. Wenn Sie die Anwendung im **SP-initiierten Modus** konfigurieren möchten, führen Sie die folgenden Schritte durch:

    b. Klicken Sie auf **Zusätzliche URLs festlegen**.

    c. Geben Sie im Textfeld **Anmelde-URL** diese URL ein: `https://login.umbrella.com/sso`

5. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** auf **Herunterladen**, um das Ihrer Anforderung entsprechende **Metadaten-XML** aus den verfügbaren Optionen herunterzuladen und auf Ihrem Computer zu speichern.

    ![Downloadlink für das Zertifikat](common/metadataxml.png)

6. Kopieren Sie im Abschnitt **Cisco Umbrella einrichten** die entsprechenden URLs gemäß Ihren Anforderungen.

    ![Kopieren der Konfiguration-URLs](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

In diesem Abschnitt erstellen Sie im Azure-Portal einen Testbenutzer mit dem Namen B. Simon.

1. Wählen Sie im linken Bereich des Microsoft Azure-Portals **Azure Active Directory** > **Benutzer** > **Alle Benutzer** aus.
1. Wählen Sie oben im Bildschirm die Option **Neuer Benutzer** aus.
1. Führen Sie unter den Eigenschaften für **Benutzer** die folgenden Schritte aus:
   1. Geben Sie im Feld **Name** die Zeichenfolge `B.Simon` ein.  
   1. Geben Sie im Feld **Benutzername** die Zeichenfolge username@companydomain.extension ein. Beispiel: `B.Simon@contoso.com`.
   1. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert aus dem Feld **Kennwort**.
   1. Klicken Sie auf **Erstellen**.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie B. Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Cisco Umbrella gewähren.

1. Wählen Sie im Azure-Portal **Unternehmensanwendungen** > **Alle Anwendungen** aus.
1. Wählen Sie in der Anwendungsliste **Cisco Umbrella** aus.
1. Navigieren Sie auf der Übersichtsseite der App zum Abschnitt **Verwalten**, und wählen Sie **Benutzer und Gruppen** aus.
1. Wählen Sie **Benutzer hinzufügen** und anschließend im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.
1. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **B. Simon** aus, und klicken Sie dann unten auf dem Bildschirm auf die Schaltfläche **Auswählen**.
1. Wenn den Benutzern eine Rolle zugewiesen werden soll, können Sie sie im Dropdownmenü **Rolle auswählen** auswählen. Wurde für diese App keine Rolle eingerichtet, ist die Rolle „Standardzugriff“ ausgewählt.
1. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.

## <a name="configure-cisco-umbrella-sso"></a>Konfigurieren des einmaligen Anmeldens für Cisco Umbrella

1. Melden Sie sich in einem anderen Browserfenster bei der Cisco Umbrella-Unternehmenswebsite als Administrator an.

2. Klicken Sie auf der linken Seite des Menüs auf **Admin** (Administrator), navigieren Sie zu **Authentication** (Authentifizierung), und klicken Sie dann auf **SAML**.

    ![Administrator](./media/cisco-umbrella-tutorial/admin.png)

3. Wählen Sie **Other** (Andere) aus, und klicken Sie dann auf **Next** (Weiter).

    ![Option „Other“ (Andere)](./media/cisco-umbrella-tutorial/other.png)

4. Klicken Sie auf der Seite **Cisco Umbrella Metadata** (Cisco Umbrella-Metadaten) auf **Next** (Weiter).

    ![Metadaten](./media/cisco-umbrella-tutorial/metadata.png)

5. Wenn Sie SAML vorab konfiguriert haben, wählen Sie auf der Registerkarte **Upload Metadata** (Metadaten hochladen) die Option **Click here to change them** (Zum Ändern hier klicken) aus, und führen Sie die folgenden Schritte aus.

    ![Schaltfläche „Next“ (Weiter)](./media/cisco-umbrella-tutorial/next.png)

6. Laden Sie unter **Option A: Upload XML file** (Option A: XML-Datei hochladen) die **Verbundmetadaten-XML**-Datei hoch, die Sie aus dem Azure-Portal heruntergeladen haben. Nach dem Hochladen der Metadaten werden die folgenden Werte automatisch aufgefüllt. Klicken Sie anschließend auf **Next** (Weiter).

    ![Dateiauswahl](./media/cisco-umbrella-tutorial/choose-file.png)

7. Klicken Sie im Abschnitt **Validate SAML Configuration** (SAML-Konfiguration überprüfen) auf **TEST YOUR SAML CONFIGURATION** (SAML-KONFIGURATION TESTEN).

    ![Test](./media/cisco-umbrella-tutorial/test.png)

8. Klicken Sie auf **SPEICHERN**.

### <a name="create-cisco-umbrella-test-user"></a>Erstellen des Cisco Umbrella-Testbenutzers

Damit sich Azure AD-Benutzer bei Cisco Umbrella anmelden können, müssen sie in Cisco Umbrella bereitgestellt werden.  
Im Fall von Cisco Umbrella ist die Bereitstellung eine manuelle Aufgabe.

**Führen Sie zum Bereitstellen eines Benutzerkontos die folgenden Schritte aus:**

1. Melden Sie sich in einem anderen Browserfenster bei der Cisco Umbrella-Unternehmenswebsite als Administrator an.

2. Klicken Sie auf der linken Seite des Menüs auf **Admin** (Administrator), und navigieren Sie zu **Accounts** (Konten).

    ![Konto](./media/cisco-umbrella-tutorial/account.png)

3. Klicken Sie oben rechts auf der Seite **Accounts** (Konten) auf **Add** (Hinzufügen), und führen Sie die folgenden Schritte aus.

    ![Der Benutzer](./media/cisco-umbrella-tutorial/create-user.png)

    a. Geben Sie im Feld **First Name** (Vorname) den Vornamen ein (in diesem Beispiel **Britta**).

    b. Geben Sie im Feld **Last Name** (Nachname) den Nachnamen ein (in diesem Beispiel **Simon**).

    c. Wählen Sie unter **Choose Delegated Admin Role** (Delegierte Administratorrolle auswählen) Ihre Rolle aus.

    d. Geben Sie im Feld **Email address** (E-Mail-Adresse) die E-Mail-Adresse des Benutzers ein, z. B. **brittasimon\@contoso.com**.

    e. Geben Sie Ihr Kennwort in das Feld **Password** (Kennwort) ein.

    f. Wiederholen Sie die Eingabe des Kennworts im Feld **Confirm Password** (Kennwort bestätigen).

    g. Klicken Sie auf **ERSTELLEN**.

## <a name="test-sso"></a>Testen des einmaligen Anmeldens

In diesem Abschnitt testen Sie die Azure AD-Konfiguration für einmaliges Anmelden mit den folgenden Optionen: 

#### <a name="sp-initiated"></a>SP-initiiert:

* Klicken Sie im Azure-Portal auf **Diese Anwendung testen**. Dadurch werden Sie zur Anmelde-URL für Cisco Umbrella weitergeleitet, wo Sie den Anmeldeflow initiieren können.  

* Navigieren Sie direkt zur Anmelde-URL für Cisco Umbrella, und initiieren Sie den Anmeldeflow.

#### <a name="idp-initiated"></a>IDP-initiiert:

* Klicken Sie im Azure-Portal auf **Diese Anwendung testen**. Dadurch sollten Sie automatisch bei der Cisco Umbrella-Instanz angemeldet werden, für die Sie einmaliges Anmelden eingerichtet haben. 

Sie können auch den Microsoft-Bereich „Meine Apps“ verwenden, um die Anwendung in einem beliebigen Modus zu testen. Beim Klicken auf die Kachel „Cisco Umbrella“ in „Meine Apps“ geschieht Folgendes: Wenn Sie den SP-Modus konfiguriert haben, werden Sie zum Initiieren des Anmeldeflows zur Anmeldeseite der Anwendung weitergeleitet. Wenn Sie den IDP-Modus konfiguriert haben, sollten Sie automatisch bei der Cisco Umbrella-Instanz angemeldet werden, für die Sie einmaliges Anmelden eingerichtet haben. Weitere Informationen zu „Meine Apps“ finden Sie in [dieser Einführung](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Nächste Schritte

Nach dem Konfigurieren von Cisco Umbrella können Sie die Sitzungssteuerung erzwingen, die in Echtzeit vor der Exfiltration und Infiltration vertraulicher Unternehmensdaten schützt. Die Sitzungssteuerung basiert auf bedingtem Zugriff. [Hier](/cloud-app-security/proxy-deployment-any-app) erfahren Sie, wie Sie die Sitzungssteuerung mit Microsoft Cloud App Security erzwingen.