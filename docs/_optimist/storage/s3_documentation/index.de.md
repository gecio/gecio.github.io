---
title: S3 Kompatiblen Objekt Storage
lang: de
permalink: /optimist/storage/s3_documentation/
nav_order: 3100
parent: Storage
has_children: true
---

Einführung in den S3 Kompatiblen Objekt Storage
=================================================

- [S3 Kennung erstellen und einlesen](./CreateanduseS3credentialsDE.md)
- [Einen Bucket erstellen und wieder löschen](./CreateAndDeleteBucketDE.md)
- [Versionierung aktivieren/deaktivieren und wie eine versioniertes Objekt gelöscht wird](./VersioningDE.md)
- [Ein Objekt hochladen und löschen](./UploadAndDeleteObjectDE.md)

Was genau ist Object Storage?
-----

Object Storage ist eine alternative zum klassischen Block Storage. Dabei werden die einzelnen Daten nicht einzelnen Blöcken auf einem Gerät zugeordnet, sondern als binäre Objekte innerhalb eines Storage Clusters gespeichert. Die notwendigen Metadaten werden in einer separaten DB gespeichert. Der Storage Cluster besteht dabei aus mehreren einzelnen Servern (Nodes), die wiederum mehrere Storage Devices verbaut haben. Bei den Storage Devices haben wir einen mix aus klassischen HDDs, SSDs und modernen NVMe Lösungen. Auf welchem Gerät letztendlich ein Object landet, wird durch den CRUSH Algorithmus entschieden, der beim Object Storage serverseitig implementiert ist.

Wie kann ich darauf zugreifen?
-----

Der Zugriff auf diese Art von Storage erfolgt ausschließlich über HTTPS Zugriffe. Dafür stellen wir einen hochverfügbaren Endpunkt zur Verfügung, über den die einzelnen Operationen ausgeführt werden können.
Dabei unterstützen wir zwei unabhängige Protokolle:

- S3
- Swift

S3 ist ein von Amazon ins leben gerufene Protokoll, um mit dieser Art von Daten arbeiten zu können. Swift ist das Protokoll, welches vom gleichnamigen OpenStack Service bereitgestellt wird. Unabhängig davon, welches Protokoll man nutzt, hat man immer Zugriff auf seine gesamten Daten. Man kann also beide Protokolle in Kombination nutzen. Es gibt für alle gängigen Plattformen Tools, um mit den Daten im Object Storage arbeiten zu können:

- Windows: s3Browser, Cyberduck
- MacOS: s3cmd, Cyberduck
- Linux: s3cmd

Darüberhinaus gibt es Integrationen in allen gängigen Programmiersprachen.

Wie sicher sind meine Daten?
-----

Basis für den Object Storage ist unsere zugrunde liegende Openstack Cloud Plattform mit dem verteilten Ceph Storage Cluster. Die Objekte werden serverseitig über mehrere Storage Devices hinweg verteilt und repliziert.
Dabei sorgt Ceph für die Sicherstellung von Replikation und Integrität der Datensätze. Fällt ein Server oder eine Festplatte aus, werden die betroffenen Datensätze auf verfügbare Server repliziert und das gewünschte Replikationslevel automatisch wiederhergestellt.

Darüberhinaus werden die Daten in ein weiteres RZ auf einen weiteren dedizierten Storage Cluster gespiegelt und können von dort im K-Fall weitergenutzt werden.

Vorteile im Überblick
-----

-  Bereitstellung per API: Die HTTPS-Schnittstelle ist sowohl kompatibel zur Amazon S3 API, also auch zur OpenStack Swift API.
-  unterstützt alle gängigen Betriebssysteme und Programmiersprachen.
-  Volle Skalierbarkeit - Der Storage kann dynamisch genutzt werden.
-  Höchste Ausfallsicherheit dank integrierter Replikation und Spiegelung über 2 unabhängige Rechenzentren.
-  Zugriff ist nahezu von jedem internetfähigen Gerät aus möglich - Somit eine super alternative zu NFS und Co.
-  PAYG Abrechnung nach genutztem Monatsdurchschnitt.
-  Transparente Abrechnung und somit gute Planbarkeit - Keine extra Traffic Kosten oder Kosten für Zugriffe auf die Daten.
-  Automatisierte Verwaltung der Objekte in Buckets mit s3 Lifecycle Policies möglich.
