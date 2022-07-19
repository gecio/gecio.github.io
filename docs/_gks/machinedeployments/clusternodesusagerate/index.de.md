---
title: Nutzungsrate der Cluster Nodes
lang: de
permalink: /gks/machinedeployments/clusternodesusagerate/
nav_order: 5300
parent: Machine Deployments
---
<!-- LTeX:  language=de-DE -->
# Nutzungsrate der Cluster Nodes

Bei der Überprüfung des Clusters ist uns eine ungewöhnlich hohe Speicher-Auslastung aufgefallen.
Wir sehen, dass ein Knoten (node) komplett ausgelastet ist, während der andere nur wenig Last zeigt.
Dies passiert oft, wenn bei einem Cluster mit weniger als drei Knoten ein Upgrade auf den Knoten durchgeführt wird.
Hier erklären wir, wie es dazu kommt.

## Node Auslastung

Als Erstes schauen wir, wie viele Knoten im Cluster sind, und wie ihre Auslastung ist.

![Step 1](get_top_node_1.png)

Der Befehl `kubectl top node` zeigt die aktuelle Knoten Auslastung. Im Beispiel haben wie zwei laufende (running) Knoten.

Erst wird der Knoten "cordoned" - also deaktiviert, damit keine neuen Pods auf dem Knoten gestartet werden.

![Step 2](get_node_2.png)

Dann wird der Knoten "drained" - also komplett leer gemacht und die bisher auf dem gedrainten Knoten laufenden Pods auf alle anderen Knoten des Clusters verteilt.
Bei nur zwei Knoten wird also alles immer auf den anderen Knoten platziert.

![Step 3](top_node_3.png)

Wenn der zweite Knoten nach dem Update wieder läuft, werden die Pods _nicht_ automatisch auf beide Knoten verteilt. Dadurch kommt es zu dem eingangs beobachteten Ungleichgewicht.

## Ein paar Tipps

* Wir empfehlen mindestens drei Knoten zu verwenden, da so auch bei Upgrades die Last besser verteilt werden kann.
* Das Tool `popeye` analysiert Cluster und macht Verbesserungsvorschläge auf Grundlage von Best Practices.
* Führen Sie auf den Knoten ein Upgrade auf die neueste Version durch und beachten Sie den letzten Schritt in [Kubernetes Updates](/gks/clusterlifecycle/upgradingacluster/).
