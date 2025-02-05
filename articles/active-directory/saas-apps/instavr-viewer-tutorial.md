---
title: 'Tutorial: Azure Active Directory-Integration mit InstaVR Viewer | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und InstaVR Viewer konfigurieren.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 01/17/2019
ms.author: jeedes
ms.openlocfilehash: e60e8c73c9f1da617851cc67fb2dbab7171f1cb0
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "92460001"
---
# <a name="tutorial-azure-active-directory-integration-with-instavr-viewer"></a>Tutorial: Azure Active Directory-Integration mit InstaVR Viewer

In diesem Tutorial erfahren Sie, wie Sie InstaVR Viewer in Azure Active Directory (Azure AD) integrieren.
Die Integration von InstaVR Viewer in Azure AD bietet die folgenden Vorteile:

* Sie können in Azure AD steuern, wer auf InstaVR Viewer Zugriff hat.
* Sie können Ihren Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei InstaVR Viewer anzumelden (einmaliges Anmelden; Single Sign-On, SSO).
* Sie können Ihre Konten über das Azure-Portal an einem zentralen Ort verwalten.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).
Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="prerequisites"></a>Voraussetzungen

Um die Azure AD-Integration mit InstaVR Viewer konfigurieren zu können, benötigen Sie Folgendes:

* Ein Azure AD-Abonnement Wenn Sie keine Azure AD-Umgebung besitzen, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/) eine einmonatige Testversion anfordern.
* Ein InstaVR Viewer-Abonnement, für das einmaliges Anmelden aktiviert ist

## <a name="scenario-description"></a>Beschreibung des Szenarios

In diesem Tutorial konfigurieren und testen Sie das einmalige Anmelden von Azure AD in einer Testumgebung.

* InstaVR Viewer unterstützt die **SP-initiierte** SSO.
* InstaVR Viewer unterstützt die **Just-in-Time**-Benutzerbereitstellung.

## <a name="adding-instavr-viewer-from-the-gallery"></a>Hinzufügen von InstaVR Viewer aus dem Katalog

Zum Konfigurieren der Integration von InstaVR Viewer in Azure AD müssen Sie InstaVR Viewer aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um InstaVR Viewer aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **[Azure-Portals](https://portal.azure.com)** auf das Symbol für **Azure Active Directory**.

    ![Schaltfläche „Azure Active Directory“](common/select-azuread.png)

2. Navigieren Sie zu **Unternehmensanwendungen**, und wählen Sie die Option **Alle Anwendungen** aus.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

3. Klicken Sie oben im Dialogfeld auf die Schaltfläche **Neue Anwendung**, um eine neue Anwendung hinzuzufügen.

    ![Schaltfläche „Neue Anwendung“](common/add-new-app.png)

4. Geben Sie im Suchfeld **InstaVR Viewer** ein, wählen Sie im Ergebnisbereich **InstaVR Viewer** aus, und klicken Sie dann auf die Schaltfläche **Hinzufügen**, um die Anwendung hinzuzufügen.

     ![InstaVR Viewer in der Ergebnisliste](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Konfigurieren und Testen des einmaligen Anmeldens in Azure AD

In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei InstaVR Viewer mithilfe eines Testbenutzers namens **Britta Simon**.
Damit einmaliges Anmelden funktioniert, muss eine Linkbeziehung zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in InstaVR Viewer eingerichtet werden.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei InstaVR Viewer müssen die folgenden Schritte ausgeführt werden:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configure-azure-ad-single-sign-on)** , um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Konfigurieren des einmaligen Anmeldens für InstaVR Viewer](#configure-instavr-viewer-single-sign-on)** , um die Einstellungen für einmaliges Anmelden auf der Anwendungsseite zu konfigurieren
3. **[Erstellen eines Azure AD-Testbenutzers](#create-an-azure-ad-test-user)** , um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
4. **[Zuweisen des Azure AD-Testbenutzers](#assign-the-azure-ad-test-user)** , um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Erstellen eines InstaVR Viewer-Testbenutzers](#create-instavr-viewer-test-user)** , um eine Entsprechung von Britta Simon in InstaVR Viewer zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist
6. **[Testen der einmaligen Anmeldung](#test-single-sign-on)** , um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens in Azure AD

In diesem Abschnitt aktivieren Sie das einmalige Anmelden von Azure AD im Azure-Portal.

Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei InstaVR Viewer die folgenden Schritte aus:

1. Wählen Sie im [Azure-Portal](https://portal.azure.com/) auf der Anwendungsintegrationsseite für **InstaVR Viewer** die Option **Einmaliges Anmelden** aus.

    ![Konfigurieren des Links für einmaliges Anmelden](common/select-sso.png)

2. Wählen Sie im Dialogfeld **SSO-Methode auswählen** den Modus **SAML/WS-Fed** aus, um einmaliges Anmelden zu aktivieren.

    ![Auswahlmodus für einmaliges Anmelden](common/select-saml-option.png)

3. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** auf das Symbol **Bearbeiten**, um das Dialogfeld **Grundlegende SAML-Konfiguration** zu öffnen.

    ![Bearbeiten der SAML-Basiskonfiguration](common/edit-urls.png)

4. Führen Sie im Abschnitt **Grundlegende SAML-Konfiguration** die folgenden Schritte aus:

    ![SSO-Informationen zur Domäne und zu den URLs für InstaVR Viewer](common/sp-identifier.png)

    a. Geben Sie im Textfeld **Anmelde-URL** eine URL im folgenden Format ein: `https://console.instavr.co/auth/saml/login/<WEBPackagedURL>`.

    > [!NOTE]
    > Es gibt kein festes Muster für die Anmelde-URL. Sie wird bei der Webpaketerstellung durch den InstaVR Viewer-Kunden generiert. Sie ist für jeden Kunden und jedes Paket eindeutig. Um die exakte Anmelde-URL zu erhalten, müssen Sie sich bei Ihrer InstaVR Viewer-Instanz anmelden und die Webpaketerstellung durchführen.

    b. Geben Sie im Textfeld **Bezeichner (Entitäts-ID)** eine URL im folgenden Format ein: `https://console.instavr.co/auth/saml/sp/<WEBPackagedURL>`.

    > [!NOTE]
    > Der ID-Wert ist nicht der tatsächliche Wert. Ersetzen Sie den Wert durch den tatsächlichen Bezeichner. Dies wird später in diesem Tutorial beschrieben.

5. Klicken Sie auf der Seite **Einmaliges Anmelden (SSO) mit SAML einrichten** im Abschnitt **SAML-Signaturzertifikat** auf **Herunterladen**, um das Ihrer Anforderung entsprechende **Zertifikat (Base64)** und die **Verbundmetadatendatei** aus den angegebenen Optionen herunterzuladen und auf Ihrem Computer zu speichern.

    ![Downloadlink für das Zertifikat](common/metadata-certificatebase64.png)

6. Kopieren Sie im Abschnitt **InstaVR Viewer einrichten** die entsprechenden URLs gemäß Ihren Anforderungen.

    ![Kopieren der Konfiguration-URLs](common/copy-configuration-urls.png)

    a. Anmelde-URL

    b. Azure AD-Bezeichner

    c. Abmelde-URL

### <a name="configure-instavr-viewer-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens für InstaVR Viewer

1. Melden Sie sich in einem neuen Webbrowserfenster bei der InstaVR Viewer-Unternehmenswebsite als Administrator an.

2. Klicken Sie auf das **Benutzersymbol**, und wählen Sie **Account** (Konto) aus.

    ![Screenshot: InstaVR Viewer-Website mit Auswahl eines Benutzers](media/instavr-viewer-tutorial/tutorial-instavr-viewer-account.png)

3. Scrollen Sie nach unten bis zu **SAML Auth** (SAML-Authentifizierung), und führen Sie die folgenden Schritte aus:

    ![Screenshot: Seite „SAML Auth“ (SAML-Authentifizierung), auf der Sie die in diesem Schritt beschriebenen Werte eingeben können](media/instavr-viewer-tutorial/tutorial-instavr-viewer-configure.png)

    a. Fügen Sie im Textfeld **SSO URL** (SSO-URL) den Wert der **Anmelde-URL** ein, den Sie aus dem Azure-Portal kopiert haben.

    b. Fügen Sie im Textfeld **Logout URL** (Abmelde-URL) den Wert der **Abmelde-URL** ein, den Sie aus dem Azure-Portal kopiert haben.

    c. Fügen Sie in das Textfeld **Entity ID** (Entitäts-ID) den Wert für den **Azure AD-Bezeichner** ein, den Sie aus dem Azure-Portal kopiert haben.

    d. Klicken Sie auf **Update** (Aktualisieren), um die heruntergeladene Zertifikatdatei hochzuladen.

    e. Klicken Sie auf **Update** (Aktualisieren), um die heruntergeladene Verbundmetadatendatei hochzuladen.

    f. Kopieren Sie den Wert im Feld **Entity ID** (Entitäts-ID), und fügen Sie ihn im Azure-Portal im Abschnitt **Grundlegende SAML-Konfiguration** ins Textfeld **Bezeichner (Entitäts-ID)** ein.

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im Azure-Portal.

1. Wählen Sie im Azure-Portal im linken Bereich die Option **Azure Active Directory**, **Benutzer** und dann **Alle Benutzer** aus.

    ![Links „Benutzer und Gruppen“ und „Alle Benutzer“](common/users.png)

2. Wählen Sie oben im Bildschirm die Option **Neuer Benutzer** aus.

    ![Schaltfläche „Neuer Benutzer“](common/new-user.png)

3. Führen Sie in den Benutzereigenschaften die folgenden Schritte aus.

    ![Dialogfeld „Benutzer“](common/user-properties.png)

    a. Geben Sie im Feld **Name** den Namen **BrittaSimon** ein.
  
    b. Geben Sie im Feld **Benutzername** Folgendes ein: **brittasimon\@ihreunternehmensdomäne.erweiterung**.  
    Zum Beispiel, BrittaSimon@contoso.com

    c. Aktivieren Sie das Kontrollkästchen **Kennwort anzeigen**, und notieren Sie sich den Wert, der im Feld „Kennwort“ angezeigt wird.

    d. Klicken Sie auf **Erstellen**.

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf InstaVR Viewer gewähren.

1. Wählen Sie im Azure-Portal die Option **Unternehmensanwendungen** aus, wählen Sie **Alle Anwendungen** aus, und wählen Sie dann **InstaVR Viewer** aus.

    ![Blatt „Unternehmensanwendungen“](common/enterprise-applications.png)

2. Geben Sie in der Anwendungsliste **InstaVR Viewer** ein, und wählen Sie den Eintrag aus.

    ![InstaVR Viewer-Link in der Anwendungsliste](common/all-applications.png)

3. Wählen Sie im Menü auf der linken Seite **Benutzer und Gruppen** aus.

    ![Link „Benutzer und Gruppen“](common/users-groups-blade.png)

4. Klicken Sie auf die Schaltfläche **Benutzer hinzufügen**, und wählen Sie dann im Dialogfeld **Zuweisung hinzufügen** die Option **Benutzer und Gruppen** aus.

    ![Bereich „Zuweisung hinzufügen“](common/add-assign-user.png)

5. Wählen Sie im Dialogfeld **Benutzer und Gruppen** in der Liste „Benutzer“ den Eintrag **Britta Simon** aus, und klicken Sie dann unten im Bildschirm auf die Schaltfläche **Auswählen**.

6. Wenn Sie einen beliebigen Rollenwert in der SAML-Assertion erwarten, wählen Sie im Dialogfeld **Rolle auswählen** in der Liste die entsprechende Rolle für den Benutzer aus, und klicken Sie dann unten auf dem Bildschirm auf **Auswählen**.

7. Klicken Sie im Dialogfeld **Zuweisung hinzufügen** auf die Schaltfläche **Zuweisen**.

### <a name="create-instavr-viewer-test-user"></a>Erstellen eines InstaVR Viewer-Testbenutzers

In diesem Abschnitt wird in InstaVR Viewer ein Benutzer namens Britta Simon erstellt. InstaVR Viewer unterstützt die Just-in-Time-Benutzerbereitstellung, die standardmäßig aktiviert ist. Für Sie steht in diesem Abschnitt kein Aktionselement zur Verfügung. Ist ein Benutzer noch nicht in InstaVR Viewer vorhanden, wird nach der Authentifizierung ein neuer Benutzer erstellt. Falls Probleme auftreten, wenden Sie sich an das [InstaVR Viewer-Supportteam](mailto:contact@instavr.co).

### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens

1. Melden Sie sich in einem neuen Webbrowserfenster bei der InstaVR Viewer-Unternehmenswebsite als Administrator an.

2. Wählen Sie im linken Navigationsbereich **Package** (Paket) und dann **Make package for Web** (Paket für das Web erstellen) aus.

    ![Screenshot: InstaVR Viewer-Unternehmenswebsite mit Auswahl von „Package“ (Paket) und „Make package for Web“ (Paket für das Web erstellen)](media/instavr-viewer-tutorial/tutorial-instavr-viewer-testing1.png)

3. Wählen Sie **Herunterladen** aus.

    ![Screenshot: Auswahl des Downloadsymbols](media/instavr-viewer-tutorial/tutorial-instavr-viewer-testing2.png)

4. Wählen Sie **Open Hosted Page** (Gehostete Seite öffnen). Anschließend werden Sie zur Anmeldung an Azure AD weitergeleitet.

    ![Screenshot: Auswahl von „Open Hosted Page“ (Gehostete Seite öffnen)](media/instavr-viewer-tutorial/tutorial-instavr-viewer-testing3.png)

5. Geben Sie Ihre Azure AD-Anmeldeinformationen ein, um sich per SSO bei Azure AD anzumelden.

## <a name="additional-resources"></a>Weitere Ressourcen

- [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](./tutorial-list.md)

- [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

- [Was ist bedingter Zugriff?](../conditional-access/overview.md)