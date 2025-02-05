---
ms.openlocfilehash: e8d8300d2be7a2a48cb48a51b0f81c96f68c45aa
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2021
ms.locfileid: "101750008"
---
1. Klonen Sie das Repository unter diesem Speicherort: https://github.com/Azure-Samples/live-video-analytics-iot-edge-csharp.
1. Öffnen Sie in Visual Studio Code den Ordner, in den das Repository heruntergeladen wurde.
1. Navigieren Sie in Visual Studio Code zum Ordner *src/cloud-to-device-console-app*. Erstellen Sie darin eine Datei, und geben Sie ihr den Namen *appsettings.json*. Diese Datei enthält die Einstellungen, die zum Ausführen des Programms erforderlich sind.
1. Kopieren Sie den Inhalt aus der Datei *~/clouddrive/lva-sample/appsettings.json*, die Sie weiter oben in dieser Schnellstartanleitung generiert haben.

    Der Text sollte wie in der folgenden Ausgabe aussehen.

    ```
    {  
        "IoThubConnectionString" : "HostName=xxx.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey=XXX",  
        "deviceId" : "lva-sample-device",  
        "moduleId" : "lvaEdge"  
    }
    ```
1. Navigieren Sie zum Ordner *src/edge*, und erstellen Sie eine Datei mit dem Namen *.env*.
1. Kopieren Sie den Inhalt der Datei */clouddrive/lva-sample/edge-deployment/.env*. Der Text sollte wie der folgende Code aussehen.

    ```
    SUBSCRIPTION_ID="<Subscription ID>"  
    RESOURCE_GROUP="<Resource Group>"  
    AMS_ACCOUNT="<AMS Account ID>"  
    IOTHUB_CONNECTION_STRING="HostName=xxx.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey=xxx"  
    AAD_TENANT_ID="<AAD Tenant ID>"  
    AAD_SERVICE_PRINCIPAL_ID="<AAD SERVICE_PRINCIPAL ID>"  
    AAD_SERVICE_PRINCIPAL_SECRET="<AAD SERVICE_PRINCIPAL ID>"  
    VIDEO_INPUT_FOLDER_ON_DEVICE="/home/lvaedgeuser/samples/input"  
    VIDEO_OUTPUT_FOLDER_ON_DEVICE="/var/media"
    APPDATA_FOLDER_ON_DEVICE="/var/local/mediaservices"
    CONTAINER_REGISTRY_USERNAME_myacr="<your container registry username>"  
    CONTAINER_REGISTRY_PASSWORD_myacr="<your container registry password>"      
    ```
    