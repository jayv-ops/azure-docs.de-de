---
title: Verstehen von Ablehnungszuweisungen für Azure-Ressourcen – Azure RBAC
description: Informationen zu Ablehnungszuweisungen bei der rollenbasierten Zugriffssteuerung (Azure RBAC, Role-Based Access Control).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: ''
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/26/2020
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: ''
ms.openlocfilehash: a5f17f009caa9306631debf511f2c890f8f2a450
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "82733770"
---
# <a name="understand-azure-deny-assignments"></a>Verstehen von Azure-Ablehnungszuweisungen

Ähnlich wie eine Rollenzuweisung verknüpft eine *Ablehnungszuweisung* in einem bestimmten Bereich einen Satz von Aktionen mit einem Benutzer, einer Gruppe oder einem Dienstprinzipal, um den Zugriff zu verweigern. Ablehnungszuweisungen hindern Benutzer an der Ausführung bestimmter Aktionen für Azure-Ressourcen, auch wenn ihnen über eine Rollenzuweisung Zugriff erteilt wird.

In diesem Artikel wird die Definition von Ablehnungszuweisungen beschrieben.

## <a name="how-deny-assignments-are-created"></a>Wie werden Ablehnungszuweisungen erstellt?

Ablehnungszuweisungen werden von Azure erstellt und verwaltet, um Ressourcen zu schützen. Azure Blueprints und von Azure verwaltete Apps verwenden Ablehnungszuweisungen, um systemseitig verwaltete Ressourcen zu schützen. Azure Blueprints und von Azure verwaltete Apps stellen die einzige Möglichkeit dar, Ablehnungszuweisungen zu erstellen. Sie können Ihre eigenen Ablehnungszuweisungen nicht direkt erstellen. Weitere Informationen zur Verwendung von Ablehnungszuweisungen zum Sperren von Ressourcen durch Blueprints finden Sie unter [Grundlegendes zur Ressourcensperre in Azure Blueprints](../governance/blueprints/concepts/resource-locking.md).

> [!NOTE]
> Sie können Ihre eigenen Ablehnungszuweisungen nicht direkt erstellen.

## <a name="compare-role-assignments-and-deny-assignments"></a>Vergleich zwischen Rollenzuweisungen und Ablehnungszuweisungen

Ablehnungszuweisungen folgen einem ähnlichen Muster wie Rollenzuweisungen, weisen aber auch einige Unterschiede auf.

| Funktion | Rollenzuweisung | Ablehnungszuweisung |
| --- | --- | --- |
| Gewähren von Zugriff | :heavy_check_mark: |  |
| Zugriff verweigern |  | :heavy_check_mark: |
| Kann direkt erstellt werden | :heavy_check_mark: |  |
| Gelten in einem Bereich | :heavy_check_mark: | :heavy_check_mark: |
| Schließen Prinzipale aus |  | :heavy_check_mark: |
| Verhindern die Vererbung auf untergeordnete Bereiche |  | :heavy_check_mark: |
| Werden auch auf Zuweisungen für [klassische Abonnementadministratoren](rbac-and-directory-admin-roles.md) angewendet. |  | :heavy_check_mark: |

## <a name="deny-assignment-properties"></a>Eigenschaften von Ablehnungszuweisungen

 Eine Ablehnungszuweisungen hat folgende Eigenschaften:

> [!div class="mx-tableFixed"]
> | Eigenschaft | Erforderlich | type | BESCHREIBUNG |
> | --- | --- | --- | --- |
> | `DenyAssignmentName` | Ja | String | Der Anzeigename der Ablehnungszuweisung. Namen müssen für einen bestimmten Bereich eindeutig sein. |
> | `Description` | Nein | String | Die Beschreibung der Ablehnungszuweisung. |
> | `Permissions.Actions` | Mindestens ein Actions- oder ein DataActions-Element | String[] | Ein Array von Zeichenfolgen, welche die Verwaltungsvorgänge angeben, auf die die Ablehnungszuweisung den Zugriff blockiert. |
> | `Permissions.NotActions` | Nein | String[] | Ein Array von Zeichenfolgen, welche die Verwaltungsvorgänge angeben, die von der Ablehnungszuweisung auszuschließen sind. |
> | `Permissions.DataActions` | Mindestens ein Actions- oder ein DataActions-Element | String[] | Ein Array von Zeichenfolgen, welche die Datenvorgänge angeben, auf die die Ablehnungszuweisung den Zugriff blockiert. |
> | `Permissions.NotDataActions` | Nein | String[] | Ein Array von Zeichenfolgen, welche die Datenvorgänge angeben, die von der Ablehnungszuweisung auszuschließen sind. |
> | `Scope` | Nein | String | Eine Zeichenfolge, die den Bereich festlegt, für den die Ablehnungszuweisung gilt. |
> | `DoNotApplyToChildScopes` | Nein | Boolean | Gibt an, ob die Ablehnungszuweisung für untergeordnete Bereiche gilt. Der Standardwert ist „false“. |
> | `Principals[i].Id` | Ja | String[] | Ein Array aus Azure AD-Prinzipalobjekt-IDs (Benutzer, Gruppe, Dienstprinzipal oder verwaltete Identität), für die die Ablehnungszuweisung gilt. Die Festlegung einer leeren GUID `00000000-0000-0000-0000-000000000000` repräsentiert alle Prinzipale. |
> | `Principals[i].Type` | Nein | String[] | Ein Array von Objekttypen, das durch „Principals[i].Id“ dargestellt wird. Eine leere GUID `SystemDefined` repräsentiert alle Prinzipale. |
> | `ExcludePrincipals[i].Id` | Nein | String[] | Ein Array aus Azure AD-Prinzipalobjekt-IDs (Benutzer, Gruppe, Dienstprinzipal oder verwaltete Identität), für die die Ablehnungszuweisung nicht gilt. |
> | `ExcludePrincipals[i].Type` | Nein | String[] | Ein Array von Objekttypen, das durch „ExcludePrincipals[i].Id“ dargestellt wird. |
> | `IsSystemProtected` | Nein | Boolean | Gibt an, ob diese Ablehnungszuweisung von Azure erstellt wurde und nicht bearbeitet oder gelöscht werden kann. Derzeit sind alle Ablehnungszuweisungen vom System geschützt. |

## <a name="the-all-principals-principal"></a>Der Prinzipal „Alle Prinzipale“

Zur Unterstützung von Ablehnungszuweisungen wurde ein systemseitig definierter Prinzipal namens *Alle Prinzipale* eingeführt. Dieser Prinzipal repräsentiert alle Benutzer, Gruppen, Dienstprinzipale und verwalteten Identitäten in einem Azure AD-Verzeichnis. Wenn die Prinzipal-ID eine aus Nullen bestehende GUID `00000000-0000-0000-0000-000000000000` ist und der Prinzipaltyp `SystemDefined` lautet, repräsentiert der Prinzipal alle Prinzipale. In der Azure PowerShell-Ausgabe sieht „Alle Prinzipale“ wie folgt aus:

```azurepowershell
Principals              : {
                          DisplayName:  All Principals
                          ObjectType:   SystemDefined
                          ObjectId:     00000000-0000-0000-0000-000000000000
                          }
```

„Alle Prinzipale“ kann mit `ExcludePrincipals` kombiniert werden, um den Zugriff für alle Prinzipale mit Ausnahme einiger Benutzer zu verweigern. „Alle Prinzipale“ hat die folgenden Einschränkungen:

- Kann nur in `Principals` verwendet werden, aber nicht in `ExcludePrincipals`.
- `Principals[i].Type` muss auf `SystemDefined` festgelegt werden.

## <a name="next-steps"></a>Nächste Schritte

* [Tutorial: Schützen neuer Ressourcen mit Azure Blueprints-Ressourcensperren](../governance/blueprints/tutorials/protect-new-resources.md)
* [Auflisten von Ablehnungszuweisungen über das Azure-Portal](deny-assignments-portal.md)
