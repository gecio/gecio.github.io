---
title: CNI Updates
lang: de
permalink: /imke/clusterlifecycle/upgradingcni/
nav_order: 4250
parent: Cluster Lebenszyklus
---


# Das CNI aktualisieren

Auf der Clusterdetailseite kann man sehen, ob ein Update für das CNI verfügbar ist.  
Wenn ein grüner Pfeil in der CNI Plugin Box angezeigt wird, steht ein Update zur Verfügung. Um das Update zu beginnen, muss auf die Box geklickt werden.

![Step 1](cni_update_details.png)

In einem Popup werden die verfügbaren Versionen angezeigt. Mit `Change CNI Version` wird der Updateprozess im Cluster begonnen.

![Step 2](cni_update_popup.png)

Das Update läuft im Hintergrund. Während die Netzwerk-Pods im Cluster neustarten, kann es kurz Paketverlusten auf den einzelnen Workern kommen.  
Es wird immer nur ein Worker gleichzeitig aktualisiert, daher sollte es nicht zu Ausfällen in Deployments mit mehr als einem Replica kommen.

Nach dem Update, wenn alle Netzwerk-Pods im Daemonset restarted wurden, ist der Cluster wieder voll Einsatzfähig.  
Sollte es wider Erwarten zu Netzwerkproblemen kommen, können die folgenden Schritte Abhilfe schaffen:

1. Ein erneuter Rolling Restart der Canal Pods: `kubectl rollout restart daemonset --namespace kube-system canal`
2. Ein Rolling Recreate der Workernodes kann auch helfen, dies kann im Webinterface gestartet werden.

CNI Updates werden immer nur auf die nächste Minorversion unterstützt. Sollten mehr als eine neue Version verfügbar sein, muss eine nach der anderen aktualisiert werden.

![Dropdown](cni_update_dropdown.png)
