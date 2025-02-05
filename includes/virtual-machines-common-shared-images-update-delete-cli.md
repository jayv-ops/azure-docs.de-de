---
title: include file
description: include file
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 04/25/2019
ms.author: cynthn
ms.custom: include file, devx-track-azurecli
ms.openlocfilehash: 4392e7f146f13e581f722b94f13038ad8abff0ba
ms.sourcegitcommit: f7eda3db606407f94c6dc6c3316e0651ee5ca37c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2021
ms.locfileid: "102245067"
---
## <a name="update-resources"></a>Aktualisieren von Ressourcen

Es gibt Einschränkungen, welche Elemente aktualisiert werden können. Die folgenden Elemente können aktualisiert werden: 

Katalog mit geteilten Images:
- BESCHREIBUNG

Imagedefinition:
- Empfohlene vCPUs
- Empfohlener Arbeitsspeicher
- BESCHREIBUNG
- Datum für Ende des Lebenszyklus

Imageversion:
- Anzahl regionaler Replikate
- Zielregionen
- Ausschluss aus neuester Version
- Datum für Ende des Lebenszyklus

Wenn Sie Replikatregionen hinzufügen möchten, dürfen Sie das verwaltete Quellimage nicht löschen. Das verwaltete Quellimage ist erforderlich, um die Imageversion in weiteren Regionen zu replizieren. 

Aktualisieren Sie die Beschreibung eines Katalogs mit ([az sig update](/cli/azure/sig#az-sig-update). 

```azurecli-interactive
az sig update \
   --gallery-name myGallery \
   --resource-group myGalleryRG \
   --set description="My updated description."
```


Aktualisieren Sie die Beschreibung einer Imagedefinition mit [az sig image-definition update](/cli/azure/sig/image-definition#az-sig-image-definition-update).

```azurecli-interactive
az sig image-definition update \
   --gallery-name myGallery\
   --resource-group myGalleryRG \
   --gallery-image-definition myImageDefinition \
   --set description="My updated description."
```

Aktualisieren Sie eine Imageversion zum Hinzufügen einer Region als Replikationsziel mit [az sig image-version update](/cli/azure/sig/image-definition#az-sig-image-definition-update). Diese Änderung wird einige Zeit in Anspruch nehmen, da das Image in eine neue Region repliziert wird.

```azurecli-interactive
az sig image-version update \
   --resource-group myGalleryRG \
   --gallery-name myGallery \
   --gallery-image-definition myImageDefinition \
   --gallery-image-version 1.0.0 \
   --add publishingProfile.targetRegions  name=eastus
```

In diesem Beispiel wird gezeigt, wie Sie mit [az sig image-version update](/cli/azure/sig/image-definition#az-sig-image-definition-update) dafür sorgen können, dass diese Imageversion nicht als das *neueste* Image verwendet wird.

```azurecli-interactive
az sig image-version update \
   --resource-group myGalleryRG \
   --gallery-name myGallery \
   --gallery-image-definition myImageDefinition \
   --gallery-image-version 1.0.0 \
   --set publishingProfile.excludeFromLatest=true
```

Dieses Beispiel zeigt, wie [az sig image-version update](/cli/azure/sig/image-definition#az-sig-image-definition-update) verwendet wird, um diese Imageversion in die Berücksichtigung als *neuestes* Image einzubeziehen.

```azurecli-interactive
az sig image-version update \
   --resource-group myGalleryRG \
   --gallery-name myGallery \
   --gallery-image-definition myImageDefinition \
   --gallery-image-version 1.0.0 \
   --set publishingProfile.excludeFromLatest=false
```

## <a name="delete-resources"></a>Löschen von Ressourcen

Ressourcen müssen in umgekehrter Reihenfolge gelöscht werden, beginnend mit der Imageversion. Nachdem alle Imageversionen gelöscht sind, können Sie die Imagedefinition löschen. Nachdem alle Imagedefinitionen gelöscht sind, können Sie den Katalog löschen. 

Löschen Sie eine Imageversion mit [az sig image-version delete](/cli/azure/sig/image-version#az-sig-image-version-delete).

```azurecli-interactive
az sig image-version delete \
   --resource-group myGalleryRG \
   --gallery-name myGallery \
   --gallery-image-definition myImageDefinition \
   --gallery-image-version 1.0.0 
```

Löschen Sie eine Imagedefinition mit [az sig image-definition delete](/cli/azure/sig/image-definition#az-sig-image-definition-delete).

```azurecli-interactive
az sig image-definition delete \
   --resource-group myGalleryRG \
   --gallery-name myGallery \
   --gallery-image-definition myImageDefinition
```


Löschen Sie einen Imagekatalog mit [az sig delete](/cli/azure/sig#az-sig-delete).

```azurecli-interactive
az sig delete \
   --resource-group myGalleryRG \
   --gallery-name myGallery
```