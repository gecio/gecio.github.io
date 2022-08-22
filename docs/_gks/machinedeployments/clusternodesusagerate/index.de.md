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
Wir sehen, dass ein Node komplett ausgelastet ist, während der andere nur wenig Last zeigt.
Dies passiert oft, wenn bei einem Cluster mit weniger als drei Nodes ein Upgrade auf den Nodes durchgeführt wird.
Hier erklären wir, wie es dazu kommt.

## Node Auslastung

Als Erstes schauen wir, wie viele Nodes im Cluster sind, und wie ihre Auslastung ist.

![Step 1](get_top_node_1.png)

Der Befehl `kubectl top node` zeigt die aktuelle Node-Auslastung. Im Beispiel haben wie zwei laufende (running) Nodes.

Erst wird der Node "cordoned" - also deaktiviert, damit keine neuen Pods auf dem Node gestartet werden.

![Step 2](get_node_2.png)

Dann wird der Node "drained" - also komplett leer gemacht und die bisher auf dem gedrainten Node laufenden Pods auf alle anderen Nodes des Clusters verteilt.
Bei nur zwei Nodes wird also alles immer auf den anderen Node platziert.

![Step 3](top_node_3.png)

Wenn der zweite Node nach dem Update wieder läuft, werden die Pods _nicht_ automatisch auf beide Nodes verteilt. Dadurch kommt es zu dem eingangs beobachteten Ungleichgewicht.

## Ein paar Tipps

* Wir empfehlen mindestens drei Nodes zu verwenden, da so auch bei Upgrades die Last besser verteilt werden kann.
* Das Tool `popeye` analysiert Cluster und macht Verbesserungsvorschläge auf Grundlage von Best Practices.
* Führen Sie auf den Nodes ein Upgrade auf die neueste Version durch und beachten Sie den letzten Schritt in [Kubernetes Updates](/gks/clusterlifecycle/upgradingacluster/).
