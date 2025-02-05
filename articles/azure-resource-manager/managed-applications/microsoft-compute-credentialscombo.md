---
title: Benutzeroberflächenelement „CredentialsCombo“
description: Hier wird das Benutzeroberflächenelement Microsoft.Compute.CredentialsCombo für das Azure-Portal beschrieben.
author: tfitzmac
ms.topic: conceptual
ms.date: 09/29/2018
ms.author: tomfitz
ms.openlocfilehash: 47c88e08e5d2eac09fbcd5b60a8ccd73b46c9616
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/19/2021
ms.locfileid: "87063773"
---
# <a name="microsoftcomputecredentialscombo-ui-element"></a>Benutzeroberflächenelement „Microsoft.Compute.CredentialsCombo“

Eine Gruppe von Steuerelementen mit integrierter Überprüfung für Windows- und Linux-Kennwörter und öffentliche SSH-Schlüssel.

## <a name="ui-sample"></a>Benutzeroberflächenbeispiel

Windows-Benutzern wird Folgendes angezeigt:

![Microsoft.Compute.CredentialsCombo Windows](./media/managed-application-elements/microsoft-compute-credentialscombo-windows.png)

Bei Linux mit ausgewähltem Kennwort wird Benutzern Folgendes angezeigt:

![Linux-Kennwort für „Microsoft.Compute.CredentialsCombo“](./media/managed-application-elements/microsoft-compute-credentialscombo-linux-password.png)

Bei Linux mit ausgewähltem öffentlichem SSH-Schlüssel wird Benutzern Folgendes angezeigt:

![Linux-Schlüssel für „Microsoft.Compute.CredentialsCombo“](./media/managed-application-elements/microsoft-compute-credentialscombo-linux-key.png)

## <a name="schema"></a>Schema

Verwenden Sie unter Windows folgendes Schema:

```json
{
  "name": "element1",
  "type": "Microsoft.Compute.CredentialsCombo",
  "label": {
    "password": "Password",
    "confirmPassword": "Confirm password"
  },
  "toolTip": {
    "password": ""
  },
  "constraints": {
    "required": true,
    "customPasswordRegex": "^(?=.*[A-Za-z])(?=.*\\d)[A-Za-z\\d]{12,}$",
    "customValidationMessage": "The password must be alphanumeric, contain at least 12 characters, and have at least 1 letter and 1 number."
  },
  "options": {
    "hideConfirmation": false
  },
  "osPlatform": "Windows",
  "visible": true
}
```

Verwenden Sie unter **Linux** folgendes Schema:

```json
{
  "name": "element1",
  "type": "Microsoft.Compute.CredentialsCombo",
  "label": {
    "authenticationType": "Authentication type",
    "password": "Password",
    "confirmPassword": "Confirm password",
    "sshPublicKey": "SSH public key"
  },
  "toolTip": {
    "authenticationType": "",
    "password": "",
    "sshPublicKey": ""
  },
  "constraints": {
    "required": true,
    "customPasswordRegex": "^(?=.*[A-Za-z])(?=.*\\d)[A-Za-z\\d]{12,}$",
    "customValidationMessage": "The password must be alphanumeric, contain at least 12 characters, and have at least 1 letter and 1 number."
  },
  "options": {
    "hideConfirmation": false,
    "hidePassword": false
  },
  "osPlatform": "Linux",
  "visible": true
}
```

## <a name="sample-output"></a>Beispielausgabe

Wenn für `osPlatform` das Betriebssystem **Windows** oder für `osPlatform` das Betriebssystem **Linux** angegeben ist und der Benutzer ein Kennwort anstelle eines öffentlichen SSH-Schlüssels eingegeben hat, gibt das Steuerelement folgende Ausgabe zurück:

```json
{
  "authenticationType": "password",
  "password": "p4ssw0rddem0",
}
```

Wenn für `osPlatform` das Betriebssystem **Linux** angegeben ist und der Benutzer einen öffentlichen SSH-Schlüssel eingegeben hat, gibt das Steuerelement folgende Ausgabe zurück:

```json
{
  "authenticationType": "sshPublicKey",
  "sshPublicKey": "AAAAB3NzaC1yc2EAAAABIwAAAIEA1on8gxCGJJWSRT4uOrR13mUaUk0hRf4RzxSZ1zRbYYFw8pfGesIFoEuVth4HKyF8k1y4mRUnYHP1XNMNMJl1JcEArC2asV8sHf6zSPVffozZ5TT4SfsUu/iKy9lUcCfXzwre4WWZSXXcPff+EHtWshahu3WzBdnGxm5Xoi89zcE=",
}
```

## <a name="remarks"></a>Hinweise

- `osPlatform` muss angegeben werden. Mögliche Optionen: **Windows** oder **Linux**.
- Wenn `constraints.required` auf **TRUE** festgelegt ist, muss das Textfeld für das Kennwort oder den öffentlichen SSH-Schlüssel Werte enthalten, damit die Überprüfung erfolgreich ist. Der Standardwert lautet **true**.
- Wenn `options.hideConfirmation` auf **true** festgelegt ist, wird das zweite Textfeld zum Bestätigen des Benutzerkennworts ausgeblendet. Der Standardwert ist **false**.
- Wenn `options.hidePassword` auf **true** festgelegt ist, wird die Option zum Verwenden der Kennwortauthentifizierung ausgeblendet. Sie kann nur verwendet werden, wenn für `osPlatform` das Betriebssystem **Linux** angegeben ist. Der Standardwert ist **false**.
- Zusätzliche Einschränkungen für die zulässigen Kennwörter können mithilfe der `customPasswordRegex`-Eigenschaft implementiert werden. Die Zeichenfolge in `customValidationMessage` wird angezeigt, wenn bei der benutzerdefinierten Überprüfung des Kennworts ein Fehler auftritt. Der Standardwert für beide Eigenschaften ist **null**.

## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung zum Erstellen von Benutzeroberflächendefinitionen finden Sie unter [Erste Schritte mit „CreateUiDefinition“](create-uidefinition-overview.md).
* Eine Beschreibung der allgemeinen Eigenschaften in Benutzeroberflächenelementen finden Sie unter [CreateUiDefinition-Elemente](create-uidefinition-elements.md).
