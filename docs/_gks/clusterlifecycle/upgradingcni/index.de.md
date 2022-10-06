---
title: CNI Updates
lang: "de"
permalink: /gks/clusterlifecycle/upgradingcni/
nav_order: 4260
parent: Cluster Lebenszyklus
---


# CNI Updates

Auf der Clusterdetailseite kann man sehen, ob ein Update für das CNI verfügbar ist.
Wenn ein grüner Pfeil in der CNI Plugin Box angezeigt wird, steht ein Update zur Verfügung. Um das Update zu beginnen, müssen Sie auf die Box klicken.

![Step 1](../images/CNIUpd01.png)

In einem Popup werden die verfügbaren Versionen angezeigt. Mit `Change CNI Version` wird der Updateprozess im Cluster begonnen.

![Step 2](../images/CNIUpd02.png)

Das Update läuft im Hintergrund. Während die Netzwerk-Pods im Cluster neu starten, kann es kurz zu Paketverlusten auf den einzelnen Workern kommen.
Es wird immer nur ein Worker gleichzeitig aktualisiert, daher sollte es nicht zu Ausfällen in Deployments mit mehr als einem Replica kommen.

Nach dem Update, wenn alle Netzwerk-Pods im Daemonset restarted wurden, ist der Cluster wieder voll einsatzfähig.
Sollte es wider Erwarten zu Netzwerkproblemen kommen, können die folgenden Schritte Abhilfe schaffen:

1. Ein erneuter Rolling Restart der Canal Pods: `kubectl rollout restart daemonset --namespace kube-system canal`
2. Ein Rolling Recreate der Worker-Nodes kann auch helfen, dies kann im Web Interface gestartet werden.

CNI Updates werden immer nur auf die nächste Minorversion unterstützt. Sollte mehr als eine neue Version verfügbar sein, muss eine nach der anderen aktualisiert werden.

![Step 3](../images/CNIUpd03.png)
