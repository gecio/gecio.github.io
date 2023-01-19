---
title: Einen Cluster löschen
lang: "de"
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
![Cluster Details](../images/DelClus01.png)

Für die folgenden Schritte müssen Sie sich den Cluster-Namen merken. Um diesen
in die Zwischenablage zu kopieren, klicken Sie auf den Namen.
![Cluster Details](../images/DelClus02.png)

## Einen Cluster löschen

Klicken Sie dazu auf `Delete`.
![Add Cluster Delete](../images/DelClus03.png)

In dem sich öffnenden Fenster wird als Sicherheitsfrage
der Cluster-Name abgefragt. Da Sie diesen vorher
schon in die Zwischenablage kopiert haben, müssen Sie diesen
nur noch einfügen.
![Add Cluster Delete](../../gettingstarted/images/GS20_DelClus.png)

Da Sie alles löschen wollen, lassen Sie die beiden Checkboxen
angekreuzt. Damit werden auch die Volumes und der Loadbalancer in
OpenStack gelöscht.

## Zusammenfassung

Herzlichen Glückwunsch! Sie haben folgende Schritte erfolgreich durchgeführt und gelernt:

* Wie lösche ich einen Cluster?
* Wie lösche ich auch in OpenStack alle Ressourcen?
