---
title: Node-Flavors konfigurieren
lang: "de"
permalink: /gks/machinedeployments/nodeflavors/
nav_order: 6200
parent: Machine Deployments
---
<!-- LTeX:  language=de-DE -->

# Node-Flavors konfigurieren

Während der Erstellung von Nodes in einem Cluster können Sie zwischen diversen sogenannten "Flavors" wählen.

![Flavor-Select](../images/NodeFlav01.png)
Flavors bestimmen die Anzahl der Prozessorkerne ("CPUs"), den Speicher ("GB RAM") und die Festplattengröße ("GB Disk") der entsprechenden Node.

![Flavors](../images/NodeFlav02.png)

Hilfreich bei der Auswahl des richtigen Flavors sind die folgenden, wiederholbaren Schritte:

1. Zunächst starten Sie mit kleinen Flavors.
2. Dann evaluieren Sie die Performance des geplanten Projektes mit diesem Flavor.
3. Treten Probleme auf, wechseln Sie zu einem größeren Flavor.

## Flavors ändern

Der Wechsel auf einen anderen Flavor ist denkbar einfach:

1. Zuerst navigieren Sie zu dem gewünschten Cluster.

     ![Clusters](../images/NodeFlav03.png)

1. Nun klicken Sie auf das Editieren-Icon des Machine Deployments, dessen Flavor Sie ändern möchten. Das Icon erscheint erst, wenn Sie mit der Maus über die entsprechende Machine Deployment fahren.

    ![edit-machine-deployment](../images/NodeFlav04.png)

1. In dem jetzt geöffneten Fenster selektieren Sie den gewünschten Flavor und klicken zuletzt auf `Save Changes`, um den Prozess abzuschließen.

    ![Edit-Flavor](../images/NodeFlav05.png)

1. Kurz danach erscheint eine Bestätigungsnachricht auf Ihrem Bildschirm, dass der Flavor erfolgreich geändert wurde.

    ![Success-Message](../images/NodeFlav06.png)
