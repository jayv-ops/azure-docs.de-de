---
title: Verschlüsselungsbereiche für Blobspeicher (Vorschau)
description: Verschlüsselungsbereiche ermöglichen Ihnen die Verwaltung der Verschlüsselung auf der Ebene des Containers oder eines einzelnen Blobs. Sie können Verschlüsselungsbereiche verwenden, um sichere Grenzen zwischen Daten zu erstellen, die sich im selben Speicherkonto befinden, aber zu unterschiedlichen Kunden gehören.
services: storage
author: tamram
ms.service: storage
ms.date: 03/05/2021
ms.topic: conceptual
ms.author: tamram
ms.reviewer: ozgun
ms.subservice: common
ms.openlocfilehash: 35395a30f7d58b9edb3aa7622a35e8c4a62dc76f
ms.sourcegitcommit: f7eda3db606407f94c6dc6c3316e0651ee5ca37c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2021
ms.locfileid: "102211361"
---
# <a name="encryption-scopes-for-blob-storage-preview"></a>Verschlüsselungsbereiche für Blobspeicher (Vorschau)

Verschlüsselungsbereiche ermöglichen Ihnen die Verwaltung der Verschlüsselung auf der Ebene des Containers oder eines einzelnen Blobs. Sie können Verschlüsselungsbereiche verwenden, um sichere Grenzen zwischen Daten zu erstellen, die sich im selben Speicherkonto befinden, aber zu unterschiedlichen Kunden gehören.

Standardmäßig wird ein Speicherkonto mit einem Schlüssel verschlüsselt, der für das gesamte Speicherkonto gilt. Mit einem Verschlüsselungsbereich können Sie angeben, dass ein oder mehrere Container mit einem Schlüssel verschlüsselt werden, der nur für diese Container gilt.

Sie können entweder von Microsoft verwaltete Schlüssel oder kundenseitig verwaltete Schlüssel verwenden, die in Azure Key Vault gespeichert sind, um den Schlüssel, der Ihre Daten verschlüsselt, zu schützen und Zugriff darauf zu steuern. Unterschiedliche Verschlüsselungsbereiche im gleichen Speicherkonto können entweder von Microsoft verwaltete oder kundenseitig verwaltete Schlüssel verwenden.

Nachdem Sie einen Verschlüsselungsbereich erstellt haben, können Sie diesen für eine Anforderung zum Erstellen eines Containers oder Blobs angeben. Weitere Informationen zum Erstellen eines Verschlüsselungsbereichs finden Sie unter [Erstellen und Verwalten von Verschlüsselungsbereichen (Vorschau)](encryption-scope-manage.md).

> [!IMPORTANT]
> Verschlüsselungsbereiche befinden sich zurzeit in der **VORSCHAU**. Die [zusätzlichen Nutzungsbestimmungen für Microsoft Azure-Vorschauen](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) enthalten rechtliche Bedingungen. Sie gelten für diejenigen Azure-Features, die sich in der Beta- oder Vorschauversion befinden oder aber anderweitig noch nicht zur allgemeinen Verfügbarkeit freigegeben sind.
>
> Achten Sie darauf, alle derzeit nicht benötigten Verschlüsselungsbereiche zu deaktivieren, um unerwartete Kosten zu vermeiden.
>
> Verschlüsselungsbereiche werden bei Konten mit Lesezugriff auf georedundanten Speicher (RA-GRS) oder mit Lesezugriff auf geozonenredundanten Speicher (RA-GZRS) während der Vorschau nicht unterstützt.

[!INCLUDE [storage-data-lake-gen2-support](../../../includes/storage-data-lake-gen2-support.md)]

## <a name="create-a-container-or-blob-with-an-encryption-scope"></a>Erstellen eines Containers oder Blobs mit einem Verschlüsselungsbereich

Blobs, die unter einem Verschlüsselungsbereich erstellt werden, werden mit dem für diesen Bereich angegebenen Schlüssel verschlüsselt. Sie können einen Verschlüsselungsbereich für ein einzelnes Blob angeben, wenn Sie es erstellen, oder Sie können einen Standardverschlüsselungsbereich angeben, wenn Sie einen Container erstellen. Wenn ein Standardverschlüsselungsbereich auf der Ebene eines Containers angegeben wird, werden alle Blobs in diesem Container mit dem Schlüssel verschlüsselt, der dem Standardbereich zugeordnet ist.

Wenn Sie ein Blob in einem Container erstellen, der über einen Standardverschlüsselungsbereich verfügt, können Sie einen Verschlüsselungsbereich angeben, der den Standardverschlüsselungsbereich außer Kraft setzt, wenn der Container so konfiguriert ist, dass Außerkraftsetzungen des Standardverschlüsselungsbereichs zulässig sind. Um Außerkraftsetzungen des Standardverschlüsselungsbereichs zu verhindern, konfigurieren Sie den Container so, dass Außerkraftsetzungen für ein einzelnes Blob verweigert werden.

Lesevorgänge für ein Blob, das zu einem Verschlüsselungsbereich gehört, erfolgen transparent, sofern der Verschlüsselungsbereich nicht deaktiviert ist.

## <a name="disable-an-encryption-scope"></a>Deaktivieren eines Verschlüsselungsbereichs

Wenn Sie einen Verschlüsselungsbereich deaktivieren, schlagen alle nachfolgenden Lese- oder Schreibvorgänge, die mit dem Verschlüsselungsbereich vorgenommen werden, mit dem HTTP-Fehlercode 403 (unzulässig) fehl. Wenn Sie den Verschlüsselungsbereich erneut aktivieren, werden Lese- und Schreibvorgänge in der Regel wieder normal ausgeführt.

Wenn ein Verschlüsselungsbereich deaktiviert ist, wird Ihnen dieser nicht mehr in Rechnung gestellt. Deaktivieren Sie alle Verschlüsselungsbereiche, die nicht benötigt werden, um unnötige Gebühren zu vermeiden.

Wenn Ihr Verschlüsselungsbereich durch kundenseitig verwaltete Schlüssel geschützt ist, können Sie auch den zugehörigen Schlüssel im Schlüsseltresor löschen, um den Verschlüsselungsbereich zu deaktivieren. Beachten Sie, dass kundenseitig verwaltete Schlüssel gegen vorläufiges und endgültiges Löschen im Schlüsseltresor geschützt sind und dass sich ein gelöschter Schlüssel entsprechend den dafür definierten Eigenschaften verhält. Weitere Information finden Sie in den folgenden Themen in der Azure Key Vault-Dokumentation:

- [Verwenden des vorläufigen Löschens mit PowerShell](../../key-vault/general/key-vault-recovery.md)
- [Verwenden des vorläufigen Löschens mit der CLI](../../key-vault/general/key-vault-recovery.md)

> [!NOTE]
> Das Löschen eines Verschlüsselungsbereichs ist nicht möglich.

## <a name="next-steps"></a>Nächste Schritte

- [Azure Storage encryption for data at rest (Azure Storage-Verschlüsselung für ruhende Daten)](../common/storage-service-encryption.md)
- [Erstellen und Verwalten von Verschlüsselungsbereichen (Vorschauversion)](encryption-scope-manage.md)
- [Kundenseitig verwaltete Schlüssel für die Azure Storage-Verschlüsselung](../common/customer-managed-keys-overview.md)
- [Was ist der Azure-Schlüsseltresor?](../../key-vault/general/overview.md)