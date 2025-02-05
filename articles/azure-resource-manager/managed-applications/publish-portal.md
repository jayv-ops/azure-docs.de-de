---
title: Veröffentlichen verwalteter Apps über das Portal
description: Erfahren Sie, wie Sie mit dem Azure-Portal eine verwaltete Azure-Anwendung erstellen, die für Mitglieder Ihrer Organisation vorgesehen ist.
author: tfitzmac
ms.topic: conceptual
ms.date: 11/02/2017
ms.author: tomfitz
ms.openlocfilehash: 05302d92f2304be35a7b88fac6fabfc17b13c63e
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "75649586"
---
# <a name="publish-a-service-catalog-application-through-azure-portal"></a>Veröffentlichen einer Dienstkataloganwendung über das Azure-Portal

Sie können [verwaltete Anwendungen](overview.md), die für Mitglieder Ihrer Organisation vorgesehen sind, mit dem Azure-Portal veröffentlichen. Die IT-Abteilung kann beispielsweise verwaltete Anwendungen veröffentlichen, die die Konformität mit Ihren Unternehmensstandards gewährleisten. Diese verwalteten Anwendungen stehen über den Dienstkatalog, aber nicht über den Azure Marketplace zur Verfügung.

## <a name="prerequisites"></a>Voraussetzungen

Geben Sie beim Veröffentlichen einer verwalteten Anwendung eine Identität zum Verwalten der Ressourcen an. Es wird empfohlen, eine Azure Active Directory-Benutzergruppe anzugeben. Informationen zum Erstellen einer Azure Active Directory-Benutzergruppe finden Sie unter [Erstellen einer Gruppe und Hinzufügen von Mitgliedern in Azure Active Directory](../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md). 

Die ZIP-Datei, die die Definition der verwalteten Anwendung enthält, muss über einen URI verfügbar sein. Es wird empfohlen, die ZIP-Datei in einen Speicherblob hochzuladen. 

## <a name="create-managed-application-with-portal"></a>Erstellen Sie eine verwaltete Anwendung mit dem Portal

1. Klicken Sie in der oberen linken Ecke auf **+ Neu**.

   ![Neuer Dienst](./media/publish-portal/new.png)

1. Suchen Sie nach **Dienstkatalog**.

1. Scrollen Sie in den Ergebnissen zu **Service Catalog Managed Application Definition (Definition für die verwaltete Dienstkataloganwendung)**. Wählen Sie es aus.

   ![Suchen Sie nach Definitionen für die verwaltete Anwendung](./media/publish-portal/select-managed-apps-definition.png)

1. Wählen Sie **Erstellen** aus, um den Erstellungsprozess der Definition für die verwaltete Anwendung zu starten.

   ![Erstellen der Definition für die verwaltete Anwendung](./media/publish-portal/create-definition.png)

1. Geben Sie Werte für Name, Anzeigename, Beschreibung, Speicherort, Abonnement und Ressourcengruppe ein. Geben Sie für den Paketdatei-URI den Pfad zur ZIP-Datei an, die Sie erstellt haben.

   ![Geben Sie die Werte ein](./media/publish-portal/fill-application-values.png)

1. Wenn Sie zur Authentifizierung und zum Sperrebene-Abschnitt gelangen, wählen Sie **Autorisierung hinzufügen** aus.

   ![Autorisierung hinzufügen](./media/publish-portal/add-authorization.png)

1. Wählen Sie bei Bedarf eine Azure Active Directory-Gruppe aus, um die Ressourcen zu verwalten, und wählen Sie **OK** aus.

   ![Autorisierungsgruppe hinzufügen](./media/publish-portal/add-auth-group.png)

1. Wenn Sie alle Werte eingegeben haben, wählen Sie **Erstellen**.

   ![Verwaltete Anwendung erstellen](./media/publish-portal/create-app.png)

## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung in verwaltete Anwendungen finden Sie in der [Übersicht über verwaltete Anwendungen](overview.md).
* Beispiele zu verwalteten Anwendungen finden Sie unter [Sample projects for Azure managed applications](sample-projects.md) (Beispielprojekte für verwaltete Azure-Anwendungen).
* Informationen zum Erstellen einer UI-Definitionsdatei für eine verwaltete Anwendung finden Sie unter [Erste Schritte mit „CreateUiDefinition“](create-uidefinition-overview.md).