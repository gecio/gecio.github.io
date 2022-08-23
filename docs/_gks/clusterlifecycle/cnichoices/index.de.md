---
title: CNI Auswahl
lang: de
permalink: /gks/clusterlifecycle/cnichoices/
nav_order: 4250
parent: Cluster Lebenszyklus
---

# CNI Auswahl

Diese Seite gibt einen Überblick darüber, was eine CNI ist, nach welchen Kriterien man eine Auswahl trifft
und wie man sie im Clustererstellungsprozess auswählt.

## Was ist eine CNI und wofür wird sie gebraucht?

CNI ist eine Abkürzung für Container Network Interface. Vereinfacht erklärt, ist es eine Beschreibung der
in Kubernetes benötigten Netzwerkfunktionalität, um die Kommunikation zwischen Pods und Nodes zu realisieren.
Da die Netzwerkumgebungen, in denen Kubernetes läuft, sehr verschieden sind, haben sich die Entwickler
von Kubernetes darauf geeinigt, sich auf eine standardisierte Schnittstellenbeschreibung der benötigten
Netzwerkfunktionalität zu beschränken und anderen die Implementierung zu überlassen. Dieser Schritt
resultierte in der Entstehung vieler verschiedener CNIs mit verschiedenen Funktionen und für verschiedene
Einsatzzwecke.

Unsere Kubernetes Plattform unterstützte lange Zeit nur eine CNI und hat diese fest als Standard definiert,
ohne eine Auswahlmöglichkeit zu bieten. Dies hat sich kürzlich geändert. Wir unterstützen nun zwei
verschiedene CNIs, aus denen der Benutzer eine wählen kann, die am Besten zu seinen Bedürfnissen passt.

## Canal

Unser langjähriger Standard in Sachen CNI war bislang *Canal*. Es handelt sich dabei um die Kombination
der beiden CNIs *Flannel* und *Calico*. Flannel realisiert dabei die Netzwerkfunktionalität zwischen den
Pods, während calico sich um die *network policies* kümmert.

Die CNI Canal gibt es schon sehr lange und besitzt daher auch die nötige Produktionsreife.
Canal unterstützt sowohl den proxy-Modus *iptables* als auch *ipvs*.

Falls Sie sich unsicher sind, welches CNI sie wählen sollen, dann wählen sie *Canal*.

## Cilium

Cilium ist relativ neu auf dem CNI Markt. Es setzt, im Gegensatz zu Canal, für die Konfiguration der
Kommunikation zwischen den Pods intern auf neuere Technologien wie beispielsweise eBPF. Cilium bietet neben
besseren health-checks außerdem mit *Hubble* ein Add-on zur genaueren Analyse von Netzwerkverkehr innerhalb
des Kubernetes Clusters an. Zudem bietet es ebenfalls Unterstützung für feingranulares Filtern des eingehenden
und ausgehenden Netzwerkverkehrs sowie des Verkehrs innerhalb des Clusters.

Um alle fortgeschrittenen Funktionen von Cilium nutzen zu können ist es notwendig den proxy Modus
*eBPF* auszuwählen. Dieser erscheint als Wahlmöglichkeit sobald Cilium als CNI gewählt wurde.
Außerdem muss das Betriebssystemimage des Kubernetes Workers eine relativ aktuelle Kernelversion anbieten,
welches auf unseren Flatcar images immer gegeben ist.

An dem Cilium Projekt wird im Moment rege weiterentwickelt so dass regelmäßig neue Features herausgebracht
und Fehler behoben werden.

Wenn Sie an einem tieferen Verständnis über die Netzwerkflüsse in ihrem Kubernetes Cluster interessiert sind,
dann wählen Sie Cilium als CNI.

## Installation der CNI

Beim Erstellungsprozess Ihres Kubernetes Clusters in Schritt zwei können Sie unter dem Punkt "Network
Configuration" Ihre CNI auswählen.

![choose CNI](../images/CNIChoice01.png)

Dort sind auch weitere Einstellungen wie proxy-Modus und weiter unten auch der Controlplane Konnektor
*Konnectivity* möglich.

![choose proxy](../images/CNIChoice02.png)

Bitte beachten Sie, dass die Wahl von *eBPF* als proxy-Modus erst zur Verfügung steht, wenn Cilium als
CNI ausgewählt wurde. Weitere Informationen zum Controlplane Konnektor *Konnectivity* finden sie auf
einer eigenen Seite [hier](/gks/clusterlifecycle/controlplaneconnector).

## Installation des Hubble Add-ons

Wenn Cilium als CNI verwendet wurde, dann hat man die Möglichkeit sich den Netzwerkverkehr des
Kubernetes Cluster visualisieren zu lassen. Dazu muss die Komponente *Hubble* über den Addons-Reiter
installiert werden, nachdem die Clustererstellung erfolgreich beendet wurde.

![install hubble](../images/CNIChoice03.png)

## Warnung

Die Auswahl der CNI kann nur zum Zeitpunkt der Erstellung des Kubernetes Clusters erfolgen. Ein nachträgliches
Ändern der CNI ist zwar technisch möglich, wird aber von unserer Plattform nicht unterstützt.

Im Falle eines manuellen Änderns des CNI im Nachhinein ist die Wahrscheinlichkeit sehr hoch, dass es bei Updates
der Kubernetes Version zu Komplikationen kommt. Wir raten daher dringend von der nachträglichen manuellen
Einflussnahme ab. Falls Sie generell vorhaben, Änderungen an der CNI vorzunehmen, erstellen Sie den Cluster
 **ohne** CNI und installieren und warten sie diese danach selber.

## Weiterführende Themen

* [Flannel github page](https://github.com/flannel-io/flannel)
* [Canal github page](https://github.com/projectcalico/canal)
* [Canal installation docs](https://projectcalico.docs.tigera.io/getting-started/kubernetes/flannel/flannel)
* [Cilium Documentation](https://docs.cilium.io/de/stable/)
