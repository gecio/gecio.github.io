---
title: Eine Anwendung in Kubernetes starten
lang: "de"
permalink: /gks/k8sapplications/runningapplications/
nav_order: 8100
parent: Anwendungen in Kubernetes
---
<!-- LTeX:  language=de-DE -->
# Eine Anwendung in Kubernetes starten

Der Cluster läuft und Sie wollen eine Applikation
betreiben. Als Beispiel verwenden wir einen NGINX, der mittels eines Load Balancers auch außerhalb des Clusters zugreifbar gemacht wird.

## Voraussetzungen

Damit Sie die folgenden Schritte erfolgreich abschließen können, brauchen Sie Folgendes:

* `kubectl` [die neueste Version](https://kubernetes.io/de/docs/tasks/tools/install-kubectl/)
* Einen laufenden Kubernetes Cluster, von GKS erstellt mit laufender Machine Deployment.
  * Siehe hierzu auch: [Einen Cluster anlegen](/gks/clusterlifecycle/creatingacluster)
* Eine valide Konfigdatei `kubeconfig` für den Cluster.
  * Siehe hierzu auch: [Mit einem Cluster verbinden](/gks/accessmanagement/connectingtoacluster/).

## Datentypen

Kubernetes ist im Prinzip eine große Datenbank. Alle Dinge, die
es betreibt, speichert der API-Server und dementsprechend
lassen sich Applikationen in Kubernetes auch betreiben.

### Deployment

Der Datentyp `Deployment` kümmert sich darum, dass Applikationen
in Kubernetes laufen und nach einigen Methoden wie zum Beispiel _Rolling_
aktualisiert werden können.

Ein `Deployment` müssen Sie auch in Ihrem NGINX Beispiel anlegen, damit
Kubernetes Ihnen das Docker Image NGINX im Cluster platziert.

### Service

Ein Service in Kubernetes ist eine Zusammenfassung diverser
Container, die im Cluster laufen. Das Matching geschieht
auf Basis von Labels, die Sie an die Deployments hängen.

Ein Service kann mehrere Typen haben. In diesem Beispiel
wählen Sie `LoadBalancer`, damit unser Service extern
über eine öffentliche IP-Adresse erreichbar ist.

## Manifeste

Um NGINX auf Kubernetes laufen zu lassen, brauchen Sie
zunächst ein Objekt vom Typ `Deployment`. Dies lässt
sich mit `kubectl` leicht generieren.

```bash
kubectl create deployment --dry-run -o yaml --image nginx nginx

apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: nginx
  name: nginx
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: nginx
    spec:
      containers:
      - image: nginx
        name: nginx
        resources: {}
status: {}
```

Dies speichern Sie nun in eine Datei
`deployment.yaml`.

```bash
kubectl create deployment --dry-run -o yaml --image nginx nginx > deployment.yaml
```

Als Nächstes benötigen Sie einen Service, der die Applikation von
der Öffentlichkeit aus zugänglich macht. Als Typ wählen Sie
`LoadBalancer` womit in OpenStack direkt ein fertig
konfigurierter Loadbalancer als Einstieg in den Cluster erstellt wird.

```bash
kubectl create service loadbalancer --dry-run --tcp=80 -o yaml nginx

apiVersion: v1
kind: Service
metadata:
  creationTimestamp: null
  labels:
    app: nginx
  name: nginx
spec:
  ports:
  - name: "80"
    port: 80
    protocol: TCP
    targetPort: 80
  selector:
    app: nginx
  type: LoadBalancer
status:
  loadBalancer: {}
```

Speichern Sie dies nun wieder in eine Datei, dieses Mal in `service.yaml`.

```bash
kubectl create service loadbalancer --dry-run --tcp=80 -o yaml nginx > service.yaml
```

Diese beiden Dateien sind die Grundlage für ein öffentliches NGINX in Kubernetes.
Zusammen gehören diese zwei Manifeste nicht. Die einzige Verbindung ist das Label
`app: nginx`, welches das Deployment in den Metadaten und der Service als Selektor
definiert hat.

## Erstellen der Applikation

Nun müssen Sie die zwei Dateien an die Kubernetes API schicken.

```bash
kubectl apply -f deployment.yaml
deployment.apps/nginx created

kubectl apply -f service.yaml
service/nginx created
```

Nun können Sie sich die zwei erstellten Objekte anschauen.

```bash
kubectl get deployment,service
NAME                          READY   UP-TO-DATE   AVAILABLE   AGE
deployment.extensions/nginx   1/1     1            1           55s

NAME                 TYPE           CLUSTER-IP    EXTERNAL-IP   PORT(S)         AGE
service/kubernetes   NodePort       10.10.10.1    <none>        443:31630/TCP   2d23h
service/nginx        LoadBalancer   10.10.10.86   <pending>     80:31762/TCP    46s
```

Wie Sie in der Ausgabe sehen, wurde das Deployment angelegt und ist im Zustand READY.
Der Service NGINX wurde auch angelegt, die EXTERNAL-IP ist jedoch noch
`pending`. Hier müssen Sie ein bisschen warten, bis der Loadbalancer
provisioniert wurde.

Nach etwa 1–2 Minuten können Sie das Kommando erneut ausführen und bekommen
 eine IP angezeigt:

```bash
kubectl get deployment,svc
NAME                          READY   UP-TO-DATE   AVAILABLE   AGE
deployment.extensions/nginx   1/1     1            1           2m8s

NAME                 TYPE           CLUSTER-IP    EXTERNAL-IP       PORT(S)         AGE
service/kubernetes   NodePort       10.10.10.1    <none>            443:31630/TCP   2d23h
service/nginx        LoadBalancer   10.10.10.86   185.116.245.169   80:31762/TCP    119s
```

Die External IP `185.116.245.169` aus unserem Beispiel ist nun öffentlich
erreichbar und zeigt Ihre NGINX-Instanz an.

## Aufräumen

Sie können das ganze sehr einfach nun auch wieder löschen.

```bash
kubectl delete -f service.yaml
service "nginx" deleted

kubectl delete -f deployment.yaml
deployment.apps "nginx" deleted

kubectl get deployment,svc

NAME                 TYPE       CLUSTER-IP   EXTERNAL-IP   PORT(S)         AGE
service/kubernetes   NodePort   10.10.10.1   <none>        443:31630/TCP   2d23h
```

Wie Sie sehen, wurde alles entfernt. Wenn Sie die IP-Adresse im Browser noch einmal aufrufen, wird ein Fehler angezeigt: Die Applikation läuft nicht mehr.

## Zusammenfassung

Herzlichen Glückwunsch! Sie haben folgende Schritte erfolgreich durchgeführt und gelernt:

* Wie spreche ich mit Kubernetes?
* Was ist ein Deployment?
* Wie lege ich ein Deployment an?
* Was ist ein Service?
* Wie lege ich einen Service an
* Wie lösche ich die Applikation wieder?

Dies sind alle notwendigen Schritte, um in Kubernetes
Applikationen zu starten.
