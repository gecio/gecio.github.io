---
title: Verwaltung der Zeitzonen
lang: de
permalink: /imke/k8sapplications/timezones
nav_order: 4
parent: Anwendungen in Kubernetes
---

Falls Ihre Anwendung Zeitzonenanpassung benötigt, können Sie diesen Guide befolgen, um diese in Kubernetes zu steuern.

## Voraussetzungen

Um diesen Guide erfolgreich abzuschließen brauchen Sie folgendes:

* `kubectl` [die neueste Version](https://kubernetes.io/de/docs/tasks/tools/install-kubectl/)
* Ein laufender Kubernetes Cluster, von iMKE erstellt mit laufender Node Deployment.
  * Sehen Sie bitte [Einen Cluster anlegen](/imke/clusterlifecycle/creatingacluster/)
* Eine valide Konfigdatei `kubeconfig` für den Cluster.
  * Sehen Sie bitte [Mit einem Cluster verbinden](/imke/accessmanagement/connectingtoacluster/).

## Zeitzonen in einer Kubernetes-Umgebung

### Das Standardverhalten im Kubernetes-Cluster

Kubernetes-Cluster erben die Zeitzonenkonfiguration von der Workernode, so dass die Standardzeitzone im Kubernetes-Cluster die gleiche ist, wie die der Workernode – diese wird also vom Kernel gesteuert.

### Pod & Container

Während es nicht trivial ist, die Zeitzone auf Clusterebene zu ändern, es gibt eine einfache Möglichkeit, dies auf Pod- und Containerebene zu bestimmen.

Um dieses Ziel zu erreichen, sollten Sie das folgende Szenario befolgen:

#### Szenario

##### Starten Sie einen Testcontainer

Starten Sie einen minimalen Container z. B. ein Container mit Busybox, wie folgendes:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: busybox-sleep
spec:
  containers:
  - name: busybox
    image: busybox
    args:
    - sleep
    - "100000"
```

Legen Sie die oben genannte Datei z. B. als `busybox.yaml` an und wenden Sie diese Pod-Deklaration an:
```bash
$ kubectl apply -f busybox.yaml
```

Warten Sie einen Augenblick, bis Ihr Deployment gestartet ist und fragen Sie die Zeit vom Container ab:
```bash
$ kubectl exec busybox-sleep -it -- date
```
Sie sollten einen ähnlichen Output erhalten, in dem z. B. die Zeitzone `UTC` zu sehen ist:
```bash
Wed Feb 26 10:57:53 UTC 2020
```

##### Die Zeitzone auf dem Testcontainer ändern

Folgendermaßen können Sie die Zeitzone auf Pod bzw. Containerebene ändern:

Teilen Sie Ihre gewünschte `zoneinfo` Datei dem Pod bzw. Container mit, indem Sie diese von der Workernode in den Container einbinden. Beispielhaft folgt die Umstellung auf Berliner Zeitzone:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: busybox-sleep-2
spec:
  containers:
  - name: busybox
    image: busybox
    args:
    - sleep
    - "100000"
    volumeMounts:
    - name: timezone-config
      mountPath: /etc/localtime
  volumes:
    - name: timezone-config
      hostPath:
        path: /usr/share/zoneinfo/Europe/Berlin
```

Grundsätzlich ist es die selbe Deklarationsdatei, aber ergänzt um ein Volume namens `timezone-config`, welches die gewünschte Zonendatei dem Container bereitstellt.

Speichern Sie die oben genannte Datei als `busybox-2.yaml` und wenden Sie diese Pod-Deklaration in Kubernetes an:
```bash
$ kubectl apply -f busybox-2.yaml
```

Nachdem Sie gewartet haben, bis das Deployment gestartet ist, fragen Sie die Zeit vom neuen Container ab:
```bash
$ kubectl exec busybox-sleep-2 -it -- date
```
Sie sollten einen Output erhalten, in dem die Berliner Zeitzone zu sehen ist:
```bash
Wed Feb 26 11:01:04 CET 2020
```

## Zusammenfassung

Folgende Schritte wurden erfolgreich durchgeführt und gelernt:

* Wie man die Zeit aus dem Container abfragt.
* Wie man die `zoneinfo` bzw. `localtime` in den Container einbinden kann, so dass die Zone geändert wird.

Herzlichen Glückwunsch! Dies sind alle notwendigen Schritte, um in Kubernetes
die Zeitzone auf Pod bzw. Containerebene zu ändern.
