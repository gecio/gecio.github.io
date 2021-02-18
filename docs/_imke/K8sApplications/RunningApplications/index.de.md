---
title: Eine Anwendung in Kubernetes starten
lang: de
permalink: /imke/k8sapplications/runningapplications
nav_order: 1
parent: Anwendungen in Kubernetes
---

Der Cluster läuft und wir wollen eine Applikation
betreiben. Als Beispiel verwenden wir einen nginx, der
per Load Balancer vor dem Cluster veröffentlicht wird.

## Voraussetzungen

Um diesen Guide erfolgreich abzuschließen brauchen Sie folgendes:

* `kubectl` [die neueste Version](https://kubernetes.io/de/docs/tasks/tools/install-kubectl/)
* Ein laufender Kubernetes Cluster, von iMKE erstellt mit laufender Node Deployment.
  * Sehen Sie bitte [Einen Cluster anlegen](/imke/clusterlifecycle/creatingacluster)
* Eine valide Konfigdatei `kubeconfig` für den Cluster.
  * Sehen Sie bitte [Mit einem Cluster verbinden](/imke/accessmanagement/connectingtoacluster/).

## Datentypen

Kubernetes ist im Endeffekt eine große Datenbank. Alle Dinge
die es betreibt, speichert der api-server und dementsprechend
lassen sich Applikationen in Kubernetes auch betreiben.

### Deployment

Der Datentyp `Deployment` kümmert sich darum, dass Applikationen
in Kubernetes laufen und nach einigen Methoden wie zum Beispiel _Rolling_
aktualisiert werden können.

Ein `Deployment` müssen wir auch in unserem nginx Beispiel anlegen, damit
Kubernetes uns das Docker Image nginx im Cluster platziert.

### Service

Ein Service in Kubernetes ist eine Zusammenfassung diverser
Container die im Cluster laufen. Das Matching geschieht
auf Basis von Labels, die wir an die Deployments hängen.

Ein Service kann mehrere Typen haben. In unserem Beispiel
wählen wir `LoadBalancer`, damit unser Service von extern
über eine öffentliche IP Adresse erreichbar ist.

## Manifeste

Um nginx auf Kubernetes laufen zu lassen, brauchen wir
zunächst ein Objekt vom Typ `Deployment`. Dies lässt
sich mittels `kubectl` leicht generieren.

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

Das sieht schon sehr gut aus. Dies speichern wir nun in eine Datei
`deployment.yaml`.

```bash
kubectl create deployment --dry-run -o yaml --image nginx nginx > deployment.yaml
```

Als nächstes benötigen wir einen Service, der die Applikation von
der Öffentlichkeit aus zugänglich macht. Als Typ wählen wir
`LoadBalancer`, dies erstellt in OpenStack direkt einen fertig
konfigurierten LoadBalancer als Einstieg in den Cluster.

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

Auch dies speichern wir nun wieder in eine Datei, dieses Mal `service.yaml`.

```bash
kubectl create service loadbalancer --dry-run --tcp=80 -o yaml nginx > service.yaml
```

Diese beiden Dateien sind die Grundlage für ein öffentliches nginx in Kubernetes.
Zusammen gehören diese zwei Manifeste nicht. Die einzige Verbindung ist das Label
`app: nginx`, welches das Deployment in den Metadaten und der Service als Selektor
definiert hat.

## Erstellen der Applikation

Nun müssen wir die zwei Dateien an die Kubernetes API schicken.

```bash
kubectl apply -f deployment.yaml
deployment.apps/nginx created

kubectl apply -f service.yaml
service/nginx created
```

Nun können wir uns die zwei erstellten Objekte anschauen.

```bash
kubectl get deployment,service
NAME                          READY   UP-TO-DATE   AVAILABLE   AGE
deployment.extensions/nginx   1/1     1            1           55s

NAME                 TYPE           CLUSTER-IP    EXTERNAL-IP   PORT(S)         AGE
service/kubernetes   NodePort       10.10.10.1    <none>        443:31630/TCP   2d23h
service/nginx        LoadBalancer   10.10.10.86   <pending>     80:31762/TCP    46s
```

Wie wir in der Ausgabe sehen, wurde das Deployment angelegt und ist im Zustand READY.
Der Service nginx wurde auch angelegt, die EXTERNAL-IP ist jedoch noch
`pending`. Hier müssen wir ein bisschen warten bis der LoadBalancer
provisioniert wurde.

Nach etwa 1-2 Minuten kann man das Kommando erneut ausführen und bekommt
nun eine IP angezeigt:

```bash
kubectl get deployment,svc
NAME                          READY   UP-TO-DATE   AVAILABLE   AGE
deployment.extensions/nginx   1/1     1            1           2m8s

NAME                 TYPE           CLUSTER-IP    EXTERNAL-IP       PORT(S)         AGE
service/kubernetes   NodePort       10.10.10.1    <none>            443:31630/TCP   2d23h
service/nginx        LoadBalancer   10.10.10.86   185.116.245.169   80:31762/TCP    119s
```

Die External IP `185.116.245.169`aus unserem Beispiel ist nun öffentlich
erreichbar und zeigt unsere Instanz von nginx an.

## Aufräumen

Das ganze kann sehr einfach nun auch wieder gelöscht werden.

```bash
kubectl delete -f service.yaml
service "nginx" deleted

kubectl delete -f deployment.yaml
deployment.apps "nginx" deleted

kubectl get deployment,svc

NAME                 TYPE       CLUSTER-IP   EXTERNAL-IP   PORT(S)         AGE
service/kubernetes   NodePort   10.10.10.1   <none>        443:31630/TCP   2d23h
```

Wie man sieht ist alles wieder weg und wenn man die IP-Adresse im Browser noch einmal aufruft, wird ein Fehler angezeigt: Die Applikation läuft nicht mehr.

## Zusammenfassung

Folgende Schritte wurden erfolgreich durchgeführt und gelernt:

* Wie spreche ich mit Kubernetes
* Was ist ein Deployment
* Wie lege ich ein Deployment an
* Was ist ein Service
* Wie lege ich einen Service an
* Wie lösche ich die Applikation wieder

Herzlichen Glückwunsch! Dies sind alle notwendigen Schritte, um in Kubernetes
Applikationen zu starten.
