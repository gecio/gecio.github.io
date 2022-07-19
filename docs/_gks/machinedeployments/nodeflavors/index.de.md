---
title: Node-Flavors (Leistung) konfigurieren
lang: de
permalink: /gks/machinedeployments/nodeflavors/
nav_order: 5200
parent: Machine Deployments
---
<!-- LTeX:  language=de-DE -->

Während der Erstellung von Nodes in einem Cluster können Sie zwischen diversen sogenannten "Flavors" wählen.

![Flavor-Select](flavor-select.png?resize=600,65)

Flavors bestimmen die Anzahl der Prozessorkerne ("CPUs"), den Speicher ("GB RAM") und die Festplattengröße ("GB Disk") der entsprechenden Node.

![Flavors](flavors.png?resize=600,500)

Hilfreich bei der Auswahl des richtigen Flavors sind die folgenden, wiederholbaren Schritte:

1. Zunächst starten Sie mit kleinen Flavors.
2. Dann evaluieren Sie die Performance des geplanten Projektes mit diesem Flavor.
3. Treten Probleme auf, wechseln Sie zu einem größeren Flavor.

## Flavors ändern

Der Wechsel auf einen anderen Flavor ist denkbar einfach:

1. Zuerst navigieren Sie zu dem gewünschten Cluster.

    ![Clusters](clusters.png?resize=1500,300)

1. Nun klicken Sie auf das Editieren-Icon des Machine Deployments, dessen Flavor Sie ändern möchten. Das Icon erscheint erst, wenn Sie mit der Maus über die entsprechende Machine Deployment fahren.

    ![Select_edit_machine_deployment](edit_machine_deployment.png?resize=1500,700)

1. In dem jetzt geöffneten Fenster selektieren Sie den gewünschten Flavor und klicken zuletzt auf `Save Changes`, um den Prozess abzuschließen.

    ![Edit-Flavor](edit-flavor.png?resize=600,700)

1. Kurz danach erscheint eine Bestätigungsnachricht auf Ihrem Bildschirm, dass der Flavor erfolgreich geändert wurde.

    ![Success-Message](success-message.png?resize=600,700)
