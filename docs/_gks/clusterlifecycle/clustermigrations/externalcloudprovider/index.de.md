---
title: External Cloud Provider Migration (OCCM)
lang: de
permalink: /gks/clusterlifecycle/clustermigrations/externalcloudprovider/
nav_order: 4350
parent: Cluster Lebenszyklus
---
<!-- LTeX:  language=de-DE -->
# External Cloud Provider Migration (OCCM)

Es ist jetzt möglich, alle Cluster die noch den In-Tree Cloud Provider verwenden, zum External Cloud Provider zu migrieren.

**Das muss bis spätestens vor dem Kubernetes 1.21 Update geschehen**, da in dieser Version der In-Tree Provider entfernt wird.

Auf der Clusterdetailseite ist der Providerstatus zu sehen.

![migration needed](migration-needed.png)
![migration not needed](migration-not-needed.png)

## Migrationsprozess

Für Hilfe oder weitere Fragen, kontaktieren Sie den GEC Support.

### Schritt 1: Start der Migration

Klicken Sie die Update-Schaltfläche und dann Bestätigen.
![migration needed](migration-needed.png)

Als ersten werden die Prozesse auf der Control Plane upgedatet und alle PV/PVC auf das neue Cinder CSI Plugin migriert.

**Der Loadblancer bekommt eine neue IP**

Während der Migration werden alle alten Neutron Loadbalancer mit Octavia Loadbalancer ersetzt, wie es auch in unserer [Optimist Platform beschrieben ist](/optimist/migration_loadbalancer/).

Zu diesem Zeitpunkt haben sie 2 Loadbalancer, den alten Neutron und den neuen Octavia Loadbalancer mit einer neuen IP.

### Schritt 2: Fix/Update IP/DNS Einstellungen

Jetzt sollten Sie entweder ihre DNS Einträge mit der neuen IP updaten, oder die alte IP vom Neutron zum Octavia Loadbalancer umziehen.

Das Ändern des DNS Eintrags sollte ohne Unterbrechung vonstattengehen. Um lange Wartezeiten zu vermeiden, sollte als Vorbereitung die TTL angepasst sein.

Das Umziehen der alten Floating-IP(FIP) ist mit einer kurzen Downtime verbunden, während man sie vom alten Neutron löst wird und an den neuen Octavia anhängt.

> **Wichtig:**
> Bitte starten Sie keine Worker-Node Rotation bevor sie diesen Schritt beendet haben. Der alte Loadbalancer wird nicht mehr upgedatet und die Anwendungen werden nicht mehr erreichbar sein.

### Schritt 3: Machine Deployment rotieren

Zum Abschließen der Migration muss das Machine Deployment einmalig rotiert werden.
![worker rotation](rotate-nodes.png)

> **Wichtig:**
> Der alte Neutron Loadbalancer leitet ab hier den Traffic nicht mehr richtig weiter.**

### Schritt 4: Aufräumen

Der alte Neutron Loadbalancer muss noch von Hand gelöscht werden.

## Weiterführende Themen

* [Neutron Deprecation in unserer Optimist OpenStack Umgebung](/optimist/migration_loadbalancer/)
* [Neutron LBaaS Deprecation im OpenStack Wiki](https://wiki.openstack.org/wiki/Neutron/LBaaS/Deprecation)
