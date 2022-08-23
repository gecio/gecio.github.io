---
title: Cluster Templates
lang: de
permalink: /gks/clusterlifecycle/clustertemplates/
nav_order: 4150
parent: Cluster Lebenszyklus
---
<!-- LTeX:  language=de-DE -->

# Cluster Templates

## Was sind Cluster Templates?

Cluster Templates sind Vorlagen, die eine schnelle und einheitliche Erstellung von Kubernetes Clustern ermöglichen. Mit Cluster Templates können Sie Cluster mit wenigen Klicks anlegen, ohne dass Sie Einstellungen wie z.B. Zugangsdaten, Netzwerkeinstellungen und Verfügbarkeitszonen jedes Mal neu eingeben müssen.

## Erstellung von Cluster Templates

Zur Erstellung eines Cluster Templates wählen Sie in der Sidebar den Menüpunkt `Cluster Templates` und betätigen anschließend die Schaltfläche `Create Cluster`.

![Empty Overview](../images/ClusTempl01.png)

Daraufhin öffnet sich der aus dem Abschnitt [`Einen Cluster anlegen`](/gks/clusterlifecycle/creatingacluster/) bekannte Cluster-Erstellungsprozess. Geben Sie hier alle Daten ein, die für die Cluster-Erstellung benötigt werden. Klicken Sie im letzten Schritt `Summary` jedoch **nicht** auf `Create Cluster`, sondern auf `Save Cluster Template`.

![Save Cluster Template](../images/ClusTempl02.png)

Nun öffnet sich der Dialog `Save Cluster Template`. Hier können Sie den gewünschten Namen und Speicherbereich festlegen.

Templates können in 2 verschiedenen Bereichen gespeichert werden:

* Auf Projekt-Ebene: Alle User des Projekts können das Template nutzen.
* Auf User-Ebene: Das Template kann in allen Projekten genutzt werden, in denen der User Schreibrechte besitzt. Andere User können das Template nicht nutzen.

![Save Cluster Template](../images/ClusTempl03.png)

Die Auswahl wird durch die Schaltfläche `Save Cluster Template` bestätigt und das Cluster Template wurde damit gespeichert.

## Cluster aus Templates erstellen

Mit dem gerade angelegtem Template können Sie nun einfach neue Cluster erstellen.

Wählen Sie in der Sidebar den Menüpunkt `Cluster Templates` aus. Wählen Sie das gewünschte Template aus und betätigen Sie die Schaltfläche `Create Cluster from Template`.

![Create Cluster from Template](../images/ClusTempl04.png)

Daraufhin werden Sie gefragt, wie viele Cluster Sie aus diesem Template erstellen wollen. Geben Sie eine Zahl ein und bestätigen Sie die Auswahl mit `Create Clusters`.

![Create Cluster from Template](../images/ClusTempl05.png)

Der bzw. die Cluster werden nun angelegt. Cluster, die mit einem Cluster Template erstellt wurden, sind an dem `template-instance-id` Label erkennbar.

![Template Create Cluster](../images/ClusTempl06.png)

> **Hinweis:** Einen weiteren Weg, Cluster aus Templates zu erstellen, finden Sie im `Cluster` Menü mit der Schaltfläche `Create Clusters from Template`. Die Funktion unterscheidet sich nicht von der gerade gezeigten und ist nur eine Abkürzung in der Oberfläche.

![Cluster Overview New Cluster](../images/ClusTempl07.png)

## Cluster Templates löschen

Um Cluster Templates wieder zu löschen, wählen Sie in der Sidebar den Menüpunkt `Cluster Templates` und das entsprechende Template aus. Zum Löschen betätigen Sie die Schaltfläche `Delete Cluster Template`.

![Template Delete](../images/ClusTempl08.png)
