---
title: Cluster Templates
lang: de
permalink: /gks/clusterlifecycle/clustertemplates/
nav_order: 4150
parent: Cluster Lebenszyklus
---
<!-- LTeX:  language=de-DE -->

## Was sind Cluster Templates?

Cluster Templates sind Vorlagen die eine schnelle und einheitliche Erstellung von Kubernetes Clustern ermöglichen. Mit Cluster Templates können Cluster mit wenigen Klicks angelegt werden, ohne das Einstellungen wie z.B. Zugangsdaten, Netzwerkeinstellungen und Verfügbarkeitszonen jedes Mal erneut eingegeben werden müssen.

## Erstellung von Cluster Templates

Zur Erstellung eines Cluster Templates in der Sidebar den Menüpunkt `Cluster Templates` wählen und anschließend die Schaltfläche `Create Cluster` betätigen.

![Empty Overview](template_overview_empty.png)

Daraufhin öffnet sich der aus dem Abschnitt [`Einen Cluster anlegen`](/gks/clusterlifecycle/creatingacluster/) bekannte Clustererstellungsprozess. Geben Sie hier alle Daten ein, die für die Clustererstellung benötigt werden. Klicken Sie im letzten Schritt `Summary` jedoch **nicht** auf `Create Cluster`, sondern auf `Save Cluster Template`.

![Save Cluster Template Button](template_save.png)

Nun öffnet sich der Dialog `Save Cluster Template`. Hier kann der gewünschte Name und Speicherbereich festgelegt werden.

Templates können in 2 verschiedenen Bereichen gespeichert werden:

* Auf Projektebene: Alle User des Projekts können das Template nutzen
* Auf Userebene: Das Template kann in allen Projekten genutzt werden, in dem der User Schreibrechte besitzt. Andere User können das Template nicht nutzen.

![Dialog Save Cluster Template](template_dialog_save.png)

Die Auswahl wird durch die Schaltfläche `Save Cluster Template` bestätigt und das Cluster Template wurde damit gespeichert.

## Cluster aus Template erstellen

Aus dem gerade angelegtem Template können nun einfach neue Cluster erstellt werden.

Wählen Sie in der Sidebar den Menüpunkt `Cluster Templates` aus. Wählen Sie nun das gewünschte Template aus und betätigen Sie die Schaltfläche `Create Cluster from Template`.

![Template Overview Create](template_overview_create.png)

Daraufhin werden Sie gefragt wie viele Cluster Sie aus diesem Template erstellen wollen. Geben Sie eine Zahl ein und bestätigen Sie die Auswahl mit `Create Clusters`.

![Template Dialog Create Cluster](template_dialog_create_cluster.png)

Darauf hin wird das Cluster bzw. die Cluster angelegt. Aus Cluster Template erstellte Cluster sind an dem `template-instance-id` Label erkennbar.

![Cluster Overview New Cluster](cluster_overview_new_cluster.png)

**Hinweis:** Ein weiterer Weg Cluster aus Templates zu erstellen findet sich im `Cluster` Menü mit der Schaltfläche `Create Clusters from Template`. Die Funktion unterscheidet sich nicht von der gerade gezeigten und ist nur eine Abkürzung in der Oberfläche.

![cluster_overview_create_alternative](cluster_overview_create_alternative.png)

## Cluster Template löschen

Um Cluster Templates wieder zu löschen, wählen Sie in der Sidebar den Menüpunkt `Cluster Templates` und das entsprechende Template aus. Zum Löschen betätigen Sie die Schaltfläche `Delete Cluster Template`.

![Template Overview Delete](template_overview_delete.png)
