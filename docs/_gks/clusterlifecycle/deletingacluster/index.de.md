---
title: Einen Cluster löschen
lang: de
permalink: /gks/clusterlifecycle/deletingacluster/
nav_order: 4400
parent: Cluster Lebenszyklus
---
<!-- LTeX:  language=de-DE -->
# Einen Cluster löschen

Einen Cluster in GKS zu löschen ist sehr schnell machbar.
Die Voraussetzung dafür ist ein existierender
Cluster in einem Projekt.

## Einen Cluster finden

Um einen Cluster zu löschen, müssen Sie in die Detailansicht
des Clusters gehen.
Hierfür klicken Sie auf den Eintrag `first-system`.
![Step 1](delete_1.png)

Für die folgenden Schritte müssen Sie sich den Cluster-Namen merken. Um diesen
in die Zwischenablage zu kopieren, klicken Sie auf den Namen.
![Step 2](delete_2.png)

## Einen Cluster löschen

Klicken Sie dazu auf `Delete`.
![Step 3](delete_3.png)

In dem sich öffnenden Fenster wird als Sicherheitsfrage
der Cluster-Name abgefragt. Da Sie diesen vorher
schon in die Zwischenablage kopiert haben, müssen Sie diesen
nur noch einfügen.
![Step 4](delete_4.png)

Da Sie alles löschen wollen, lassen Sie die beiden Checkboxen
angekreuzt. Damit werden auch die Volumes und der Loadbalancer in
OpenStack gelöscht.

## Zusammenfassung

Herzlichen Glückwunsch! Sie haben folgende Schritte erfolgreich durchgeführt und gelernt:

* Wie lösche ich einen Cluster?
* Wie lösche ich auch in OpenStack alle Ressourcen?
