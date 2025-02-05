---
title: Verwenden eines benutzerdefinierten NGINX-Eingangscontrollers und Konfigurieren von HTTPS
services: azure-dev-spaces
ms.date: 12/10/2019
ms.topic: conceptual
description: Erfahren Sie, wie Sie Azure Dev Spaces für die Verwendung eines benutzerdefinierten NGINX-Eingangscontrollers und HTTPS mithilfe dieses Eingangscontrollers konfigurieren.
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, Container, Helm, Service Mesh, Service Mesh-Routing, kubectl, k8s
ms.custom: devx-track-js, devx-track-azurecli
ms.openlocfilehash: c5ef14074c6e601bcd8a23ce62921d67b01ba3bb
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102198131"
---
# <a name="use-a-custom-nginx-ingress-controller-and-configure-https"></a>Verwenden eines benutzerdefinierten NGINX-Eingangscontrollers und Konfigurieren von HTTPS

[!INCLUDE [Azure Dev Spaces deprecation](../../../includes/dev-spaces-deprecation.md)]

In diesem Artikel wird erläutert, wie Sie Azure Dev Spaces für die Verwendung eines benutzerdefinierten NGINX-Eingangscontrollers konfigurieren. Außerdem wird in diesem Artikel beschrieben, wie Sie den benutzerdefinierten Eingangscontroller für die Verwendung von HTTPS konfigurieren.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Azure-Abonnement. Falls Sie über keins verfügen, können Sie ein [kostenloses Konto][azure-account-create] erstellen.
* Die [Azure CLI][az-cli] muss installiert sein.
* Azure Kubernetes Service-Cluster (AKS) mit aktiviertem Azure Dev Spaces-Dienst
* [kubectl][kubectl] muss installiert sein.
* [Helm 3 muss installiert sein.][helm-installed]
* [Eine benutzerdefinierte Domäne][custom-domain] mit einer [DNS-Zone][dns-zone].  In diesem Artikel wird davon ausgegangen, dass sich die benutzerdefinierte Domäne und DNS-Zone in derselben Ressourcengruppe wie Ihr AKS-Cluster befinden. Es ist jedoch möglich, eine benutzerdefinierte Domäne und DNS-Zone in einer anderen Ressourcengruppe zu verwenden.

## <a name="configure-a-custom-nginx-ingress-controller"></a>Konfigurieren eines benutzerdefinierten NGINX-Eingangscontrollers

Stellen Sie unter Verwendung des Kubernetes-Befehlszeilenclients ([kubectl][kubectl]) eine Verbindung mit Ihrem Cluster her. Mit dem Befehl [az aks get-credentials][az-aks-get-credentials] können Sie `kubectl` für die Verbindungsherstellung mit Ihrem Kubernetes-Cluster konfigurieren. Mit diesem Befehl werden die Anmeldeinformationen heruntergeladen, und die Kubernetes-Befehlszeilenschnittstelle wird für deren Verwendung konfiguriert.

```azurecli
az aks get-credentials --resource-group myResourceGroup --name myAKS
```

Überprüfen Sie die Verbindung mit Ihrem Cluster mithilfe des Befehls [kubectl get][kubectl-get], um eine Liste der Clusterknoten zurückzugeben.

```console
kubectl get nodes
NAME                                STATUS   ROLES   AGE    VERSION
aks-nodepool1-12345678-vmssfedcba   Ready    agent   13m    v1.14.1
```

Fügen Sie das [offizielle stabile Helm-Repository][helm-stable-repo] hinzu, das das Helm-Chart des NGINX-Eingangscontrollers enthält.

```console
helm repo add stable https://kubernetes-charts.storage.googleapis.com/
```

Erstellen Sie einen Kubernetes-Namespace für den NGINX-Eingangscontroller, und installieren Sie ihn mithilfe von `helm`.

```console
kubectl create ns nginx
helm install nginx stable/nginx-ingress --namespace nginx --version 1.27.0
```

> [!NOTE]
> Im obigen Beispiel wird ein öffentlicher Endpunkt für den Eingangscontroller erstellt. Wenn Sie stattdessen einen privaten Endpunkt für Ihren Eingangscontroller verwenden müssen, fügen Sie den Parameter *--set controller.service.annotations."service\\.beta\\.kubernetes\\.io/azure-load-balancer-internal"=true* für den Befehl *helm install* hinzu. Beispiel:
> ```console
> helm install nginx stable/nginx-ingress --namespace nginx --set controller.service.annotations."service\.beta\.kubernetes\.io/azure-load-balancer-internal"=true --version 1.27.0
> ```
> Dieser private Endpunkt wird innerhalb des virtuellen Netzwerks verfügbar gemacht, in dem Ihr AKS-Cluster bereitgestellt wird.

Rufen Sie die IP-Adresse des NGINX-Eingangscontrollerdiensts mithilfe von [kubectl get][kubectl-get] ab.

```console
kubectl get svc -n nginx --watch
```

Die Beispielausgabe enthält die IP-Adressen für alle Dienste im *nginx*-Namespace.

```console
NAME                                  TYPE           CLUSTER-IP     EXTERNAL-IP      PORT(S)                      AGE
nginx-nginx-ingress-controller        LoadBalancer   10.0.19.39     <pending>        80:31314/TCP,443:30521/TCP   10s
nginx-nginx-ingress-default-backend   ClusterIP      10.0.210.231   <none>           80/TCP                       10s
...
nginx-nginx-ingress-controller        LoadBalancer   10.0.19.39     MY_EXTERNAL_IP   80:31314/TCP,443:30521/TCP   26s
```

Fügen Sie Ihrer DNS-Zone mithilfe von [az network dns record-set a add-record][az-network-dns-record-set-a-add-record] einen *A*-Datensatz mit der externen IP-Adresse des NGINX-Diensts hinzu.

```azurecli
az network dns record-set a add-record \
    --resource-group myResourceGroup \
    --zone-name MY_CUSTOM_DOMAIN \
    --record-set-name *.nginx \
    --ipv4-address MY_EXTERNAL_IP
```

Im obigen Beispiel wird der DNS-Zone *MY_CUSTOM_DOMAIN* ein *A*-Eintrag hinzugefügt.

In diesem Artikel verwenden Sie die [Azure Dev Spaces-Beispielanwendung „Bike Sharing“](https://github.com/Azure/dev-spaces/tree/master/samples/BikeSharingApp), um die Nutzung von Azure Dev Spaces zu veranschaulichen. Klonen Sie die Anwendung von GitHub, und navigieren Sie zum Verzeichnis der Anwendung:

```cmd
git clone https://github.com/Azure/dev-spaces
cd dev-spaces/samples/BikeSharingApp/charts
```

Öffnen Sie [values.yaml][values-yaml], und führen Sie die folgenden Updates durch:
* Ersetzen Sie alle Vorkommen von *<REPLACE_ME_WITH_HOST_SUFFIX>* durch *nginx.MY_CUSTOM_DOMAIN*, wobei Sie für *MY_CUSTOM_DOMAIN* Ihre Domäne verwenden. 
* Ersetzen Sie *kubernetes.io/ingress.class: traefik-azds  # Dev Spaces-specific* durch *kubernetes.io/ingress.class: nginx  # Custom Ingress*. 

Nachstehend finden Sie ein Beispiel für eine aktualisierte `values.yaml`-Datei:

```yaml
# This is a YAML-formatted file.
# Declare variables to be passed into your templates.

bikesharingweb:
  ingress:
    annotations:
      kubernetes.io/ingress.class: nginx  # Custom Ingress
    hosts:
      - dev.bikesharingweb.nginx.MY_CUSTOM_DOMAIN  # Assumes deployment to the 'dev' space

gateway:
  ingress:
    annotations:
      kubernetes.io/ingress.class: nginx  # Custom Ingress
    hosts:
      - dev.gateway.nginx.MY_CUSTOM_DOMAIN  # Assumes deployment to the 'dev' space
```

Speichern Sie Ihre Änderungen, und schließen Sie die Datei.

Erstellen Sie mithilfe von `azds space select` den *dev*-Raum mit Ihrer Beispielanwendung.

```console
azds space select -n dev -y
```

Stellen Sie die Beispielanwendung mithilfe von `helm install` bereit.

```console
helm install bikesharingsampleapp . --dependency-update --namespace dev --atomic
```

Im obigen Beispiel wird die Beispielanwendung für den *dev*-Namespace bereitgestellt.

Zeigen Sie die URLs für den Zugriff auf die Beispielanwendung mit `azds list-uris` an.

```console
azds list-uris
```

Die folgende Ausgabe zeigt die Beispiel-URLs aus `azds list-uris`.

```console
Uri                                                  Status
---------------------------------------------------  ---------
http://dev.bikesharingweb.nginx.MY_CUSTOM_DOMAIN/  Available
http://dev.gateway.nginx.MY_CUSTOM_DOMAIN/         Available
```

Navigieren Sie zum Dienst *bikesharingweb*, indem Sie die öffentliche URL über den Befehl `azds list-uris` öffnen. Im obigen Beispiel lautet die öffentliche URL für den Dienst *bikesharingweb*`http://dev.bikesharingweb.nginx.MY_CUSTOM_DOMAIN/`.

> [!NOTE]
> Wenn anstelle des Diensts *bikesharingweb* eine Fehlerseite angezeigt wird, überprüfen Sie, ob Sie **sowohl** die Anmerkung *kubernetes.io/ingress.class* als auch den Host in der Datei *values.yaml* aktualisiert haben.

Verwenden Sie den Befehl `azds space select`, um unter *dev* einen untergeordneten Bereich zu erstellen, und listen Sie die URLs für den Zugriff auf den untergeordneten Entwicklungsbereich auf.

```console
azds space select -n dev/azureuser1 -y
azds list-uris
```

Die folgende Ausgabe zeigt die Beispiel-URLs aus `azds list-uris` für den Zugriff auf die Beispielanwendung im untergeordneten Entwicklungsbereich *azureuser1*.

```console
Uri                                                  Status
---------------------------------------------------  ---------
http://azureuser1.s.dev.bikesharingweb.nginx.MY_CUSTOM_DOMAIN/  Available
http://azureuser1.s.dev.gateway.nginx.MY_CUSTOM_DOMAIN/         Available
```

Navigieren Sie zum Dienst *bikesharingweb* im untergeordneten Entwicklungsbereich *azureuser1*, indem Sie die öffentliche URL über den Befehl `azds list-uris` öffnen. Im obigen Beispiel wird für den Dienst *bikesharingweb* im untergeordneten Entwicklungsbereich *azureuser1* die öffentliche URL `http://azureuser1.s.dev.bikesharingweb.nginx.MY_CUSTOM_DOMAIN/` verwendet.

## <a name="configure-the-nginx-ingress-controller-to-use-https"></a>Konfigurieren des NGINX-Eingangscontrollers für die Verwendung von HTTPS

Verwenden Sie [cert-manager][cert-manager], um die Verwaltung des TLS-Zertifikats zu automatisieren, wenn Sie den NGINX-Eingangscontroller für die Verwendung von HTTPS konfigurieren. Verwenden Sie `helm`, um das Chart *certmanager* zu installieren.

```console
kubectl apply --validate=false -f https://raw.githubusercontent.com/jetstack/cert-manager/release-0.12/deploy/manifests/00-crds.yaml --namespace nginx
kubectl label namespace nginx certmanager.k8s.io/disable-validation=true
helm repo add jetstack https://charts.jetstack.io
helm repo update
helm install cert-manager --namespace nginx --version v0.12.0 jetstack/cert-manager --set ingressShim.defaultIssuerName=letsencrypt --set ingressShim.defaultIssuerKind=ClusterIssuer
```

Erstellen Sie die Datei `letsencrypt-clusterissuer.yaml`, und aktualisieren Sie das Feld für die E-Mail mit Ihrer E-Mail-Adresse.

```yaml
apiVersion: cert-manager.io/v1alpha2
kind: ClusterIssuer
metadata:
  name: letsencrypt
spec:
  acme:
    server: https://acme-v02.api.letsencrypt.org/directory
    email: MY_EMAIL_ADDRESS
    privateKeySecretRef:
      name: letsencrypt
    solvers:
      - http01:
          ingress:
            class: nginx
```

> [!NOTE]
> Zu Testzwecken gibt es auch einen [Stagingserver][letsencrypt-staging-issuer], den Sie für Ihren *ClusterIssuer* verwenden können.

Verwenden Sie `kubectl`, um `letsencrypt-clusterissuer.yaml` anzuwenden.

```console
kubectl apply -f letsencrypt-clusterissuer.yaml --namespace nginx
```

Aktualisieren Sie [values.yaml][values-yaml], sodass die Details für die Verwendung von *cert-manager* und HTTPS eingeschlossen sind. Nachstehend finden Sie ein Beispiel für eine aktualisierte `values.yaml`-Datei:

```yaml
# This is a YAML-formatted file.
# Declare variables to be passed into your templates.

bikesharingweb:
  ingress:
    annotations:
      kubernetes.io/ingress.class: nginx  # Custom Ingress
      cert-manager.io/cluster-issuer: letsencrypt
    hosts:
      - dev.bikesharingweb.nginx.MY_CUSTOM_DOMAIN  # Assumes deployment to the 'dev' space
    tls:
    - hosts:
      - dev.bikesharingweb.nginx.MY_CUSTOM_DOMAIN
      secretName: dev-bikesharingweb-secret

gateway:
  ingress:
    annotations:
      kubernetes.io/ingress.class: nginx  # Custom Ingress
      cert-manager.io/cluster-issuer: letsencrypt
    hosts:
      - dev.gateway.nginx.MY_CUSTOM_DOMAIN  # Assumes deployment to the 'dev' space
    tls:
    - hosts:
      - dev.gateway.nginx.MY_CUSTOM_DOMAIN
      secretName: dev-gateway-secret
```

Führen Sie für die Beispielanwendung mithilfe von `helm` ein Upgrade durch:

```console
helm upgrade bikesharingsampleapp . --namespace dev --atomic
```

Navigieren Sie zur Beispielanwendung im untergeordneten Bereich *dev/azureuser1*, und beachten Sie, dass Sie zur Verwendung von HTTPS umgeleitet werden. Beachten Sie auch, dass die Seite geladen wird, im Browser jedoch einige Fehler angezeigt werden. Wenn Sie die Browserkonsole öffnen, wird angezeigt, dass sich der Fehler auf eine HTTPS-Seite bezieht, auf der HTTP-Ressourcen geladen werden. Beispiel:

```console
Mixed Content: The page at 'https://azureuser1.s.dev.bikesharingweb.nginx.MY_CUSTOM_DOMAIN/devsignin' was loaded over HTTPS, but requested an insecure resource 'http://azureuser1.s.dev.gateway.nginx.MY_CUSTOM_DOMAIN/api/user/allUsers'. This request has been blocked; the content must be served over HTTPS.
```

Um diesen Fehler zu beheben, aktualisieren Sie [BikeSharingWeb/azds.yaml][azds-yaml] ähnliche der folgenden Darstellung:

```yaml
...
    ingress:
      annotations:
        kubernetes.io/ingress.class: nginx
        cert-manager.io/cluster-issuer: letsencrypt
      hosts:
      # This expands to [space.s.][rootSpace.]bikesharingweb.<random suffix>.<region>.azds.io
      - $(spacePrefix)$(rootSpacePrefix)bikesharingweb.nginx.MY_CUSTOM_DOMAIN
      tls:
      - hosts:
        - $(spacePrefix)$(rootSpacePrefix)bikesharingweb.nginx.MY_CUSTOM_DOMAIN
        secretName: dev-bikesharingweb-secret
...
```

Aktualisieren Sie [BikeSharingWeb/package.json][package-json] mit einer Abhängigkeit für das *URL*-Paket.

```json
{
...
    "react-responsive": "^6.0.1",
    "universal-cookie": "^3.0.7",
    "url": "0.11.0"
  },
...
```

Aktualisieren Sie die *getApiHostAsync*-Methode in [BikeSharingWeb/lib/helpers.js][helpers-js] für die Verwendung von HTTPS:

```javascript
...
    getApiHostAsync: async function() {
        const apiRequest = await fetch('/api/host');
        const data = await apiRequest.json();
        
        var urlapi = require('url');
        var url = urlapi.parse(data.apiHost);

        console.log('apiHost: ' + "https://"+url.host);
        return "https://"+url.host;
    },
...
```

Navigieren Sie zum Verzeichnis `BikeSharingWeb`, und verwenden Sie `azds up`, um den aktualisierten Dienst *BikeSharingWeb* auszuführen.

```console
cd ../BikeSharingWeb/
azds up
```

Navigieren Sie zur Beispielanwendung im untergeordneten Bereich *dev/azureuser1*, und beachten Sie, dass Sie ohne Fehler zur Verwendung von HTTPS umgeleitet werden.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Funktionsweise von Azure Dev Spaces:

> [!div class="nextstepaction"]
> [Funktionsweise von Azure Dev Spaces](../how-dev-spaces-works.md)


[az-cli]: /cli/azure/install-azure-cli
[az-aks-get-credentials]: /cli/azure/aks#az-aks-get-credentials
[az-network-dns-record-set-a-add-record]: /cli/azure/network/dns/record-set/a#az-network-dns-record-set-a-add-record
[custom-domain]: ../../app-service/manage-custom-dns-buy-domain.md#buy-an-app-service-domain
[dns-zone]: ../../dns/dns-getstarted-cli.md
[azds-yaml]: https://github.com/Azure/dev-spaces/blob/master/samples/BikeSharingApp/BikeSharingWeb/azds.yaml
[azure-account-create]: https://azure.microsoft.com/free
[cert-manager]: https://cert-manager.io/
[helm-installed]: https://helm.sh/docs/intro/install/
[helm-stable-repo]: https://helm.sh/docs/intro/quickstart/#initialize-a-helm-chart-repository
[helpers-js]: https://github.com/Azure/dev-spaces/blob/master/samples/BikeSharingApp/BikeSharingWeb/lib/helpers.js#L7
[kubectl]: https://kubernetes.io/docs/user-guide/kubectl/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[letsencrypt-staging-issuer]: https://cert-manager.io/docs/configuration/acme/#creating-a-basic-acme-issuer
[package-json]: https://github.com/Azure/dev-spaces/blob/master/samples/BikeSharingApp/BikeSharingWeb/package.json
[values-yaml]: https://github.com/Azure/dev-spaces/blob/master/samples/BikeSharingApp/charts/values.yaml
