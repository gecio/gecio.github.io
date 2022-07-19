---
title: Mit einem Cluster verbinden
lang: de
permalink: /gks/accessmanagement/connectingtoacluster/
nav_order: 6100
parent: Access Management
---
<!-- LTeX:  language=de-DE -->
# Mit einem Cluster verbinden

Nachdem Sie in GKS einen Cluster angelegt haben, zeigen wir Ihnen, wie Sie sich mit diesem verbinden. Das ist notwendig, um
Applikationen zu deployen und zu managen.

## Den Cluster finden

Um einen Cluster zu finden, müssen Sie in die Detailansicht
des Clusters gehen.
Hierfür klicken Sie auf den Eintrag `first-system`.
![Step 1](connect_1.png)

## Die Zugangsdaten herunterladen

Klicken Sie rechts oben auf den nach unten gerichteten Pfeil.
![Step 2](connect_2.png)

Damit laden Sie eine Datei herunter, die sich im Kubernetes-Umfeld
`kubeconfig` nennt. In dieser Datei stehen alle Endpunkte,
Zertifikate sowie Bereiche des Clusters. Mit dieser Datei kann
`kubectl` sich mit dem Cluster verbinden.

Um diese Datei zu nutzen, müssen Sie diese auf der Konsole
registrieren. Dafür gibt es zwei Möglichkeiten:

1. `kubectl` schaut als Standard in die Datei `.kube/config`
    im Heimat-Verzeichnis des Benutzers
1. Sie können die `kubeconfig` temporär mittels einer Umgebungsvariable
    exportieren

Der Einfachheit halber und um auf Ihrem System die Standards
nicht zu verändern, nehmen Sie hier Variante 2.

Dafür benutzen Sie eine Konsole. In den Screenshots wurde
iTerm2 auf macOS verwendet, es funktioniert jedoch auf Linux und Windows
bash genau so.

Als Erstes müssen Sie die heruntergeladene Datei finden.
Chrome und Firefox laden diese normalerweise in den Downloads-Ordner. Der Dateiname setzt sich aus den zwei folgenden Komponenten zusammen:

* `kubeconfig-admin-`
* Ihre Cluster ID

Um diese dann zu registrieren, nutzen Sie folgendes Kommando:

```bash
cd Downloads
export KUBECONFIG=$(pwd)/kubeconfig-admin-CLUSTERID
```

Nun können Sie mit Ihrem Cluster kommunizieren. Das einfachste Kommando ist
hier: "zeige mir alle Nodes meines Clusters":

```bash
kubectl get nodes

NAME                           STATUS   ROLES    AGE   VERSION
musing-kalam-XXXXXXXXX-ks4xz   Ready    <none>   10m   v1.20.7
musing-kalam-XXXXXXXXX-txc4w   Ready    <none>   10m   v1.20.7
musing-kalam-XXXXXXXXX-vc4g2   Ready    <none>   10m   v1.20.7
```

## Kubernetes Dashboard

In GKS können Sie mit einem Klick auf das Kubernetes Dashboard zugreifen.
Um dies im Browser zu öffnen, klicken Sie oben rechts auf die Schaltfläche `Open Dashboard`.
![Step 3](connect_3.png)

Nun sehen Sie das Kubernetes Dashboard und können
Ihren Cluster grafisch erkunden.
![Step 4](connect_4.png)

## Zusammenfassung

Folgende Schritte wurden erfolgreich durchgeführt:

* Wie komme ich an meine `kubectl` Konfiguration?
* Wie konfiguriere ich `kubectl` auf meinem Computer?
* Wie verbinde ich mich mit dem Kubernetes Dashboard?

Herzlichen Glückwunsch! Dies sind alle notwendigen Schritte, um sich
mit einem Kubernetes Cluster zu verbinden.
