---
title: Erste Schritte
lang: de
permalink: /imke/gettingstarted/
nav_order: 2
has_children: false
---

Diese Anleitung beschreibt, wie Sie ihr erstes iMKE-Projekt inkl. einem ersten
Kubernetes-Cluster erzeugen, wie Sie auf das Cluster zugreifen und anschließend
die angelegten Ressourcen wieder vollständig löschen.

This Guide describes how to create your first iMKE project with a first Kubernetes
cluster, how to connect to that cluster and how to cleanup all resources
afterwards.

## Das erste Projekt anlegen

Nach dem Login in iMKE erscheint folgendes Fenster, in dem wir auf
`Add Project` klicken müssen.
![Add Project](addproject.png)

Danach öffnet sich ein Fenster, in dem wir dem Projekt einen Namen geben.
Als Beispiel verwenden wir hier `Team Kubernetes`.
Im zweiten Schritt muss dann auf `Save` geklickt werden.

![Add Project Modal](addproject_modal.png?resize=600)

Im Anschluss legt iMKE das Projekt an und stellt es in der Übersicht dar.
Mit einem Klick auf den Eintrag `Team Kubernetes` sind wir
im Projekt-Umfeld und können das Cluster anlegen.
![Project list](projectlist.png)

Die folgende Seite zeigt das Projekt. Hier sind alle bereits
bestehenden Cluster sowie zugehörige User und weitere Kontroll-Mechanismen
sichtbar.
![Project View](projectview.png)

Im Augenblick ist diese Liste noch leer, bis wir unser erstes Kubernetes
Cluster erstellt haben.

## Das erste Cluster erstellen

Um einen Cluster anzulegen, klicken wir oben rechts auf `Add Cluster`.
![Add Cluster](projectview_addcluster.png)

Jetzt öffnet sich die erste Seite für den Prozess, einen Cluster anzulegen.
Im Beispiel nennen wir unseren Cluster `first-system` und wählen die Kubernetes
Version 1.18.6 aus:
![Add Cluster Step 1](add_step1.png)

Wir klicken dann auf `Next` und im nächsten Schritt wählen wir den Provider
`openstack` und eine der drei Verfügbarkeitszonen aus, in diesem Beispiel
nehmen wir `IX1`:
![Add Cluster Step 2.1](add_step2_1.png) ![Add Cluster Step 2.2](add_step2_2.png)

Damit iMKE in der OpenStack Plattform die notwendigen Ressourcen erzeugen kann,
geben wir hier erneut unsere Zugangsdaten ein:
![Add Cluster Step 3.1](add_step3.png)

Danach wird der Inhalt in `Project` automatisch aktualisiert und wir können
unser gewünschtes OpenStack Projekt auswählen:
![Add Cluster Step 3.2](add_step3_2.png)

In den Node Settings konfigurieren wir, wie viele Server später im Kubernetes Cluster
als Worker-Nodes verwendet werden können. Diese Gruppe von Servern braucht einen Namen und
eine Größe. Für den ersten Cluster sind genaue Namen nicht so wichtig, deswegen benutzen
wir den Namensgenerator. Die Anzahl und Größe der Server belassen wir auf den
Standardwerten.
![Add Cluster Step 4](add_step4.png)

Als Image entscheiden wir uns für Flatcar Linux. Flatcar Linux ist
speziell für den Betrieb von Containern gedacht und wird durch iMKE
automatisch aktualisiert.
![Add Cluster Step 5](add_step5.png)

Zum Schluss klicken wir wieder auf `Next` und nachdem wir alle Informationen
noch einmal geprüft haben, auf `Create`.
![Add Cluster Step 7](add_step7.png)

Nun wird das Cluster erstellt. Um auf die Informationen zugreifen zu können müssen
wir nun wieder auf die Cluster-Übersicht des Projektes und dort auf unser Cluster
klicken.
![Add Cluster Step 8](add_step8.png)
![Add Cluster Step 8.2](add_step8_2.png)

## Auf das Cluster zugreifen

Um auf das Cluster zuzugreifen, klicken wir oben rechts
auf den nach unten gerichteten Pfeil:
![Step 2](connect_2.png)

Damit laden wir eine Datei herunter, die sich im Kubernetes-Jargon
`kubeconfig` nennt. In dieser Datei stehen alle Endpunkte,
Zertifikate sowie Bereiche des Clusters. Mit dieser Datei ist
`kubectl`  in der Lage, sich mit dem Cluster zu verbinden.

Um diese Datei zu nutzen, müssen wir sie auf der Konsole
registrieren. Dafür gibt es zwei Möglichkeiten:

1. `kubectl` schaut als Standard in die Datei `.kube/config`
    im Heimat-Verzeichnis des Benutzers.
2. Wir können die `kubeconfig` temporär mittels einer Umgebungsvariable
    exportieren.

Der Einfachheit halber und um auf unserem System die Standards
nicht zu verändern, gehen wir hier mit Variante 2.

Dafür benutzen wir eine Konsole. In den Screenshots verwenden
ich iTerm2 auf macOS, es funktioniert jedoch auf Linux und Windows
bash genau so.

Als erstes müssen wir die herunter geladene Datei finden.
Chrome und Firefox laden diese beide normalerweise in den Downloads
Ordner. Der Dateiname setzt sich jetzt aus zwei Komponenten zusammen:

* `kubeconfig-admin-`
* unser Cluster ID

Um diese dann zu registrieren nutzen wir folgendes Kommando:

```bash
cd Downloads
export KUBECONFIG=$(pwd)/kubeconfig-admin-CLUSTERID
```

Nun können wir mit unserem Cluster reden. Das einfachste Kommando ist
hier: "zeige mir alle Nodes meines Clusters":

```bash
kubectl get nodes

NAME                           STATUS   ROLES    AGE   VERSION
musing-kalam-XXXXXXXXX-ks4xz   Ready    <none>   10m   v1.15.0
musing-kalam-XXXXXXXXX-txc4w   Ready    <none>   10m   v1.15.0
musing-kalam-XXXXXXXXX-vc4g2   Ready    <none>   10m   v1.15.0
```

## Aufräumen

Um nach diesem ersten Test das Cluster wieder zu löschen, klicken wir auf `Delete`:
![Step 3](delete_3.png)

In dem sich öffnenden Fenster wird als Sicherheitsfrage
der Cluster-Name angefragt:
![Step 4](delete_4.png)

Da wir alles löschen wollen, lassen wir die beiden Checkboxen
angehakt. Damit werden auch Volumes und LoadBalancer in
OpenStack gelöscht.
