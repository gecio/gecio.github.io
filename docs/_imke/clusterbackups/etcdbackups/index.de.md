---
title: Etcd Sicherung und Wiederherstellung
lang: de
permalink: /imke/clusterbackups/etcdbackups/
nav_order: 5200
parent: Cluster-Backups
---
<!-- LTeX:  language=de-DE -->

iMKE führt eine neue Sicherungs- und Wiederherstellungsfunktion ein, die standardmäßig für alle Cluster aktiviert ist.
Die Standard-Sicherungskonfigurationsressource wird mit einem Standardintervall von @alle 20m und einer Sicherungsaufbewahrung von 20 erstellt.

Es ist jedoch möglich, bei Bedarf zusätzliche Sicherungskonfigurationen zu erstellen.

> Bevor Sie beginnen, bedenken Sie bitte, dass es sich um eine etcd-Sicherung und -Wiederherstellung handelt. Das einzige, was wiederhergestellt wird, ist der etcd-Status, nicht die Anwendungsdaten oder ähnliches.

## Etcd-Backups anlegen

Etcd Sicherungen und Wiederherstellungen sind an ein Projekt gebundene Ressourcen, die Sie in der Projektansicht verwalten können.
![Projekt Etcd-Sicherungen](backup_1.png)

Um eine neue Sicherung zu erstellen, müssen Sie auf die Schaltfläche Automatische Sicherung hinzufügen klicken. Sie haben die Wahl zwischen voreingestellten täglichen, wöchentlichen oder monatlichen Backups, oder Sie können ein Backup mit einem benutzerdefinierten Intervall und einer bestimmten Zeit erstellen.
![Etcd Backups Konfiguration](backup_2.png)
![Neue Sicherung hinzufügen](backup_3.png)
Um alle verfügbaren Backups zu sehen, klicken Sie auf ein Backup, das Sie interessiert,
![Etcd-Sicherungen Details](backup_4.png)
und Sie sehen dann die Liste der abgeschlossenen Sicherungen.
![Etcd Backups Details](backup_5.png)

## Etcd-Backups wiederherstellen

Wenn Sie ein Backup wiederherstellen möchten, müssen Sie auf das Symbol Wiederherstellen aus Backup in der Benutzeroberfläche klicken.
![Schaltfläche Backup wiederherstellen](backup_6.png)
![etcd-Sicherung für Cluster wiederherstellen](backup_7.png)
Danach wird der Cluster angehalten, etcd wird gelöscht und dann aus dem Backup neu erstellt. Danach wird die Pause des Clusters wieder aufgehoben.
In der Zwischenzeit wird ein Etcd-Wiederherstellungsobjekt im Projekt erstellt, dessen Fortschritt Sie in der Etcd-Wiederherstellungsliste beobachten können.
![EtcdRestore-Liste](backup_8.png)
In der Cluster-Ansicht werden Sie feststellen, dass sich Ihr Cluster in einem Wiederherstellungszustand befindet und Sie nicht mit ihm interagieren können, bis der Vorgang abgeschlossen ist.
![Cluster-Wiederherstellung](backup_9.png)
Wenn die Wiederherstellung abgeschlossen ist, wird der Cluster wieder freigegeben, so dass Sie ihn verwenden können.
Die Etcd-Wiederherstellung geht in den Zustand Completed über.
![Etcd-Wiederherstellung abgeschlossen](backup_10.png)