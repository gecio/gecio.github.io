---
title: CNI Auswahl
lang: en
permalink: /imke/clusterlifecycle/cnichoices/
nav_order: 4250
parent: Cluster Lebenszyklus
---

# Die Wahl einer CNI

Diese Seite gibt einen Überblick darüber was eine CNI ist, nach welchen Kriterien man eine Auswahl trifft
und wie man sie im Clustererstellungsprozess auswählt.


## Was ist eine CNI und warum brauche ich sie?

CNI ist eine Abkürzung for Container Network Interface. Vereinfacht erklärt ist es eine Beschreibung der
in Kubernetes benötigten Netzwerkfunktionalität um die Kommunkation zwischen Pods und Knoten zu realisieren.
Da die Netzwerkumgebungen in denen Kubernetes laufen kann sehr verschieden sind haben sich die Entwickler
von Kubernetes darauf geeinigt sich auf eine standardisierte Schnittstellenbeschreibung der benötigten
Netzwerjfunktionalität zu beschränken und anderen die Implementierung zu überlassen. Dieser Schritt
resultierte in der Entstehung vieler verschiedener CNIs mit verschiednen Funktionen und für verschiedene
Einsatzzwecke.

Unsere Kubernetes Plattform unterstütze lange Zeit nur eine CNI und hat diese fest als Standard definiert
ohne eine Auswahlmöglichkeit zu bieten. Dies hat sich kürzlich geändert. Wir unterstützen nun zwei
verschiedene CNIs aus denen der Benutzer eine wählen kann die am Besten zu seinen Bedürfnissen passt.


## Canal

Unser langjähriger Standard in Sachen CNI war bislang *canal*. Es handelt sich dabei um die Kombination
der beiden CNIs *flannel* und *calicao*. Flannel realisiert dabei die Netzwerkfunktionalität zwischen den
pods während calico sich um die *network policies* kümmert.

Die CNI canal gibt es schon sehr lange und besitzt daher auch die nötige Produktionsreife.
Canal unterstützt sowohl den proxy-Modus *iptables* als auch *ipvs*.

Falls sie sich unsicher sind welches CNI sie wählen sollten, dann wählen sie Canal!


## Cilium

Cilium ist relativ neu auf dem CNI Markt. Es setzt, im Gegensatz zu Canal, für die Kommunikation zwischen
den Pods intern auf neuere Technologien wie beispielsweise eBPF. Cilium bietet neben besseren health-checks
ausserdem mit *hubble* ein Addon zur genaueren Analyse von Netzwerkverkehr innerhalb des Kubernetes
Clusters an. Zudem bietet es ebenfalls Unterstüzung für feingranulares Filtern des eingehenden und ausgehenden
Netzwerkverkehrs sowie des Verkehrs innerhalb des Clusters.

Um alle fortgeschrittenen Funktionen von Cilium nutzen zu können ist es notwendig den proxy Modus
*eBPF* auszuwählen. Dieser erscheint als Wahlmöglichkeit sobald Cilium als CNI gewählt wurde (und der
Haken bei *Konnectivity* weiter unten gemacht wurde!). Ausserdem muss das Betriebssystemimage des
kubernetes Workers eine relativ aktuelle Kernelversion anbieten (was auf unseren Flatcar images
gegeben ist!) Eine Liste der einzuhaltenden Abhängigkeiten für Cilium finden sich hinter
[diesem Link](https://docs.cilium.io/en/stable/operations/system_requirements/)

An dem Cilium Projekt wird im Moment rege weiterentwickelt so dass regelmäßig neue Features herausgebracht
und Fehler behoben werden.

Wenn sie ein tieferes Verständnis über die Netzwerkflüsse in ihrem Kubernetes Cluster interessiert sind,
dann wählen sie Cilium als ihre CNI!


## Keine

Es ist durchaus möglich komplett andere CNIs in unseren kubernetes Clustern zu benutzen. Dies wird jedoch
von unserer Plattform nicht angeboten. Wenn sie dennoch eine andere CNI wünschen haben sie die Möglchkeit
den Kubernetes Cluster ohne CNI zu erstellen und ihre eigene CNI im Nachhinein auf dem Cluster zu installieren.
Dies bedingt jedoch ein fortgschrittenes Wissen im Bereich Kubernetes-Netzwerk und sollte nur von Experten
in diesem Gebiet gewählt werden!


# Installation der CNI

Beim Erstellungsprozess ihres Kubernetes Clusters in Schritt zwei haben sie unter dem Punkt "Network
Configuration" die Möglichkeit ihre CNI auszuwählen. 

![choose CNI](choosing_cni.png)

Dort sind auch weitere Einstellungen wie proxy-Modus und weiter unten auch der Control-Plane Konnektor
*Konnectivity* möglich.

![choose proxy](choosing_proxy_mode.png)

Bitte beachten sie dass die Wahl von *eBPF* als proxy-Modus erst zur Verfügung steht wenn Cilium als
CNI ausgewählt wurde (und der Haken bei Konnectivity ebenfalls gesetzt ist). Weitere Informationen
zum Control-Plane Konnektor *Kopnnectivity* finden sie auf einer eigenen Seite
[hier](/imke/clusterlifecycle/control-plane-connector).


# Warning

Die Auswahl der CNI kann nur zum Zeitpunkt der Erstellung des Kubernetes Clusters erfolgen. Ein nachträgliches
Ändern der CNI ist zwar technisch möglich, wird von unserer Plattform aber nicht unterstützt.

Im Falle eines manuellen Änderns des CNI im Nachhinein ist die Wahrscheinlichkeit sehr hoch dass es bei Updates
der Kubernetes Version zu Komplikationen kommt. Wir raten daher dringend von der nachträglichen manuellen
Einflussnahme ab. Falls sie generell vorhaben Änderungen an der CNI vorzunehmen, erstellen sie den Cluster
bitte **ohne** CNI und installieren und warten sie diese danach selber!


# Weitere Quellen

* [Flannel github page](https://github.com/flannel-io/flannel)
* [Canal github page](https://github.com/projectcalico/canal)
* [Canal installation docs](https://projectcalico.docs.tigera.io/getting-started/kubernetes/flannel/flannel)
* [Cilium Documentation](https://docs.cilium.io/de/stable/)
