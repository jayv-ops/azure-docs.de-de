---
title: Problembehandlung für Azure Spring Cloud in virtuellen Netzwerken
description: Leitfaden zur Problembehandlung für Azure Spring Cloud in virtuellen Netzwerken.
author: mikedodaro
ms.service: spring-cloud
ms.topic: how-to
ms.date: 09/19/2020
ms.author: brendm
ms.custom: devx-track-java
ms.openlocfilehash: b2369a6380c7b74302d32366d0604fca616fc3ed
ms.sourcegitcommit: 42e4f986ccd4090581a059969b74c461b70bcac0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/23/2021
ms.locfileid: "104877608"
---
# <a name="troubleshooting-azure-spring-cloud-in-virtual-networks"></a>Problembehandlung für Azure Spring Cloud in virtuellen Netzwerken

Dieses Dokument hilft Ihnen bei der Behandlung verschiedener Probleme, die bei der Verwendung von Azure Spring Cloud in virtuellen Netzwerken auftreten können.

## <a name="i-encountered-a-problem-with-creating-an-azure-spring-cloud-service-instance"></a>Bei mir ist ein Problem beim Erstellen einer Azure Spring Cloud-Dienstinstanz aufgetreten

Zum Erstellen einer Instanz von Azure Spring Cloud müssen Sie über ausreichende Berechtigungen zum Bereitstellen der Instanz im virtuellen Netzwerk verfügen.  Die Spring Cloud-Dienstinstanz muss sich selbst die [Azure Spring Cloud-Dienstberechtigung für das virtuelle Netzwerk erteilen](spring-cloud-tutorial-deploy-in-azure-virtual-network.md#grant-service-permission-to-the-virtual-network).

Wenn Sie das Azure-Portal zum Einrichten der Azure Spring Cloud-Dienstinstanz verwenden, werden die Berechtigungen vom Azure-Portal überprüft.

Überprüfen Sie Folgendes, um die Azure Spring Cloud-Dienstinstanz mithilfe der [Azure CLI](/cli/azure/get-started-with-azure-cli) einzurichten:

- Das Abonnement ist aktiv.
- der Standort von Azure Spring Cloud unterstützt wird.
- Die Ressourcengruppe für die Instanz wurde bereits erstellt.
- Der Ressourcenname entspricht der Benennungsregel. die Instanz nur Kleinbuchstaben, Zahlen und Bindestriche enthält. Das erste Zeichen muss ein Buchstabe sein. Das letzte Zeichen muss ein Buchstabe oder eine Zahl sein. Der Wert muss zwischen 2 und 32 Zeichen lang sein.

Wenn Sie die Azure Spring Cloud-Dienstinstanz mithilfe der Resource Manager-Vorlage einrichten möchten, lesen Sie zunächst den Artikel [Verstehen der Struktur und Syntax von Azure Resource Manager-Vorlagen](../azure-resource-manager/templates/template-syntax.md).

### <a name="common-creation-issues"></a>Häufig auftretende Probleme bei der Erstellung

| Fehlermeldung | So behebt man den Fehler |
|------|------|
| Von Azure Spring Cloud erstellte Ressourcen wurden von der Richtlinie nicht zugelassen. | Netzwerkressourcen werden erstellt, wenn Sie Azure Spring Cloud in Ihrem eigenen virtuellen Netzwerk bereitstellen. Überprüfen Sie, ob Sie in [Azure Policy](../governance/policy/overview.md) definiert haben, diese Erstellung zu blockieren. Eine Fehlermeldung teilt mit, dass Ressourcen nicht erstellt werden konnten. |
| Angegebene Subnetze sind Routingtabellen zugeordnet, bitte heben Sie die Zuordnung auf. | Derzeit wird die Bereitstellung von Azure Spring Cloud in Subnetzen, die vorhandenen Routingtabellen zugeordnet sind, nicht unterstützt. Heben Sie die Zuordnung auf, und versuchen Sie es erneut. |
| Der erforderliche Datenverkehr ist nicht in der Positivliste enthalten. | Weitere Informationen dazu, wie Sie sicherstellen, dass der gewünschte Datenverkehr in der Positivliste enthalten ist, finden Sie unter [Kundenzuständigkeiten für die Ausführung von Azure Spring Cloud im VNet](spring-cloud-vnet-customer-responsibilities.md). |

## <a name="my-application-cant-be-registered"></a>Meine Anwendung kann nicht registriert werden

Dieses Problem tritt auf, wenn Ihr virtuelles Netzwerk mit benutzerdefinierten DNS-Einstellungen konfiguriert ist. In diesem Fall ist die von Azure Spring Cloud verwendete private DNS-Zone nicht wirksam. Fügen Sie die Azure DNS-IP-Adresse 168.63.129.16 als Upstream-DNS-Server im benutzerdefinierten DNS-Server hinzu.

## <a name="other-issues"></a>Andere Probleme

[Behandlung von allgemeinen Problemen mit Azure Spring Cloud](./spring-cloud-troubleshoot.md).