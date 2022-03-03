---
title: Node-Flavors (Leistung) konfigurieren
lang: de
permalink: /imke/machinedeployments/nodeflavors/
nav_order: 5200
parent: Machine Deployments
---
<!-- LTeX:  language=de-DE -->

Während der Erstellung von Nodes in einem Cluster kann man sich zwischen diversen sogenannten "Flavors" entscheiden.

![Flavor-Select](flavor-select.png?resize=600,65)

Flavors bestimmen die Anzahl an Prozessorkernen ("CPUs") und Speicher ("GB RAM"), sowie die Festplattengröße ("GB Disk") der entsprechenden Node.

![Flavors](flavors.png?resize=600,500)

Hilfreich bei der Auswahl des richtigen Flavors sind die folgenden, wiederholbaren Schritte:

1. Zunächst startet man mit kleinen Flavors.
2. Dann evaluiert man die Performance des geplanten Projektes mit diesem Flavor.
3. Treten Probleme auf, so wechselt man auf einen höheren Flavor.

## Flavors ändern

Der Wechsel auf einen anderen Flavor ist denkbar einfach:

1. Zuerst navigiert man in den gewünschten Cluster.

    ![Clusters](clusters.png?resize=1500,300)

1. Nun klickt man auf das Editieren-Icon desjenigen MachineDeployments, deren Flavor man ändern möchte. Das Icon erscheint dabei erst dann, wenn man mit der Maus über die entsprechende MachineDeployment fährt.

    ![Select_edit_machine_deployment](edit_machine_deployment.png?resize=1500,700)

1. In dem jetzt geöffneten Fenster selektiert man dann den gewünschten Flavor und klickt zuletzt auf `Save Changes`, um den Prozess abzuschließen.

    ![Edit-Flavor](edit-flavor.png?resize=600,700)

1. Kurz danach erscheint eine Bestätigungsnachricht auf Ihrem Bildschirm und der Flavor wurde erfolgreich geändert.

    ![Success-Message](success-message.png?resize=600,700)
