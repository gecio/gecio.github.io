---
title: CNI Updates
lang: de
permalink: /imke/clusterlifecycle/upgradingcni/
nav_order: 4250
parent: Cluster Lebenszyklus
---


# Das CNI updaten

Auf der Clusterdetailseite kann man sehen, ob ein Update verfügbar ist.  
Wenn ein grüner Pfeil in der CNI Plugin Box ist, steht ein Update zur verfügung. Klickt die Box, um das Update zu beginnen.

![Step 1](cni_update_details.png)

In einem Popup werden die verfügbaren Versionen angezeigt. Mit `Change CNI Version` wird er Updateprozess im Cluster begonnen.

![Step 2](cni_update_popup.png)

Das Update läuft im Hintergrund. Wärnend die Netzwerk Pods neustarten kann es zu Paketverlusten auf den einzelnen Workern kommen.  
Es wird immer nur ein Worker gleichzeitig aktualisiert, so sollte es nicht zu Ausfällen in Deployments mit mehr als einem Replica kommen.

Nach dem Update, wenn alle Canal Pods im Daemonset restarted wurden, ist der Cluster wieder voll Einsatzfähig.  
Sollte es wiedererwarten zu Netzwerkproblemen kommen könnte:

1. ein weiteres rolling restart der Canal Pods helfen. Das ist mit dem kubectl clinet machbar `kubeclt rollout restart daemonset --namespace kube-system canal`
2. ein rolling recreate der Workernodes kan auch helfen, er kann im Webinterface gestartet werden.


CNI Updates sind nur auf die nächste Minorversion unterstützt. Sollten mehr als eine neue Version verfügbar sein, muss eine nach der anderen aktualisiert werden.

![Dropdown](cni_update_dropdown.png)
