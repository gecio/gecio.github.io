---
title: S3 Kompatibler Object Storage
lang: de
permalink: /optimist/storage/s3_documentation/
nav_order: 3100
parent: Storage
has_children: true
---

# S3 Kompatibler Objekt Storage

## Was ist ein Object Storage?

Der Object Storage ist eine Alternative zum klassischen Block Storage. Dabei werden die einzelnen Daten nicht einzelnen Blöcken auf einem Gerät zugeordnet, sondern als binäre Objekte innerhalb eines Storage Clusters gespeichert. Die notwendigen Metadaten werden in einer separaten DB gespeichert. Der Storage Cluster besteht aus mehreren einzelnen Servern (Nodes), die wiederum mehrere Storage Devices verbaut haben. Bei den Storage Devices haben wir einen Mix aus klassischen HDDs, SSDs und modernen NVMe Lösungen. Auf welchem Gerät letztendlich ein Object gespeichert wird, wird durch den CRUSH Algorithmus entschieden, der beim Object Storage serverseitig implementiert ist.

## Wie kann ich darauf zugreifen?

Der Zugriff auf diese Art von Storage erfolgt ausschließlich über HTTPS. Dafür stellen wir einen hochverfügbaren Endpunkt zur Verfügung, über den die einzelnen Operationen ausgeführt werden können.
Dabei unterstützen wir zwei unabhängige Protokolle:

- S3
- Swift

S3 ist ein Protokoll von Amazon, das entwickelt wurde, um mit dieser Art von Daten arbeiten zu können. Swift ist das Protokoll, das vom gleichnamigen OpenStack Service bereitgestellt wird. Unabhängig davon, welches Protokoll genutzt wird: Die Benutzer haben immer Zugriff auf ihre gesamten Daten und können beide Protokolle in Kombination nutzen. Es gibt für alle gängigen Plattformen Tools, um mit den Daten im Object Storage arbeiten zu können:

- Windows: s3Browser, Cyberduck
- MacOS: s3cmd, Cyberduck
- Linux: s3cmd

Darüberhinaus gibt es Integrationen in allen gängigen Programmiersprachen.

## Wie sicher sind meine Daten?

Basis für den Object Storage ist unsere zugrunde liegende Openstack Cloud Plattform mit dem verteilten Ceph Storage Cluster. Die Objekte werden serverseitig über mehrere Storage Devices hinweg verteilt und repliziert.
Dabei sorgt Ceph für die Sicherstellung von Replikation und Integrität der Datensätze. Fällt ein Server oder eine Festplatte aus, werden die betroffenen Datensätze auf verfügbare Server repliziert und das gewünschte Replikationslevel wird automatisch wiederhergestellt.

Darüberhinaus werden die Daten in ein weiteres Rechenzentrum auf einen weiteren dedizierten Storage Cluster gespiegelt und können von dort im K-Fall weitergenutzt werden.

## Vorteile

- Bereitstellung per API - die HTTPS-Schnittstelle ist sowohl kompatibel zur Amazon S3 API als auch zur OpenStack Swift API
- Unterstützt alle gängigen Betriebssysteme und Programmiersprachen
- Volle Skalierbarkeit - der Storage kann dynamisch genutzt werden
- Höchste Ausfallsicherheit dank integrierter Replikation und Spiegelung über 2 unabhängige Rechenzentren
- Zugriff ist nahezu von jedem internetfähigen Gerät aus möglich  und ist somit eine sehr gute Alternative zu NFS und Co
- PAYG Abrechnung nach genutztem Monatsdurchschnitt
- Transparente Abrechnung und somit gute Planbarkeit - keine extra Traffic Kosten oder Kosten für Datenzugriffe
- Automatisierte Verwaltung der Objekte in Buckets mit S3 Lifecycle Policies möglich
