---
title: External Cloud Provider Migration (OCCM)
lang: de
permalink: /imke/clusterlifecycle/clustermigrations/externalcloudprovider
nav_order: 4350
parent: Cluster Lifecycle
---

E ist jetzt möglich alle Cluster die noch den In-tree Cloud Provider verwenden, zum External Cloud Provider zu migrieren.

**Das muss bis spätestens for dem kubernetes 1.22 update geschehen**, in dieser Version wird der In-tree Provider entfehrnt.

Auf der Clusterdetailseite gibt is der Providerstatus zu sehen.

[image]

# Migrationsprozess

Für Hilfe oder weiter Frage, Kontaktieren sie bitte den GEC Support.

## Schritt 1 Start der Migration

Klicken sie bitte den Update Button und dann Bestätigen.
[image]

Als ersten werden die Prozesse auf der Control Plane upgedeted und alle PV/PVC auf das neue Cinder SCI Plugin migriert.

**Der Loadblancer bekommt eine neue IP**

Wärend der Migratin werden alle alten Neutron Loadblancer mit Octavia Loadblancern ersetzt.

Zu dieses Zeitpunkt haben sie 2 Loadblancer, den alten Neutron und den neuen Octavia Loadblancer mit einer neuen IP.

## Schritt 2 Fix/Update IP/DNS Einstellungen

Jetzt sollten sie entweder ihre DNS Einträge mit der neuen IP updaten, oder die alte IP vom Neutron zum Octavia Loadblancer umziehen.

Das Ändern des DNS Eintrags sollte ohne Unterbrechnung von statten gehen. Um lange Wartezeiten zu vermeiden, sollte als Vorbereitung die TTL angepasst sein.

Das umziehen der alten IP(FIP) ist mit einer kurzen Downtume verbunden, während man sie vom alten Neutron löst wird und an den neuen Octavia anhängt.

**Wichtig: Bitte starten sie keine Workernode Rotation bevor sie dieses Schritt beendet haben. Der alte Loadblancer wird nicht mehr upgedated und die Anwendungen werden nicht mehr erreichbar sein.**

## Schritt 3 Rotate Machinedeployment

Zum Abschließen der Migratin muss das Machinedeployment einmalig rotiert werden.
[image]

**Wichtig: der alte Neutron Loadblancer leitet ab hier den Traffic nicht mer richtig weiter.**

## Schritt 4 Aufräumen

Der alte Neutron Loadblancer muss noch von Hand gelöscht werden.
