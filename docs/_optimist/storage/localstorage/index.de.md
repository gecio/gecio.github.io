---
title: Localstorage
lang: de
permalink: /optimist/storage/localstorage/
nav_order: 3200
parent: Storage
has_children: true
---

Compute Localstorage für Ihre Instanzen
=================================================

Was genau ist Compute Localstorage?
-----

Mit Localstorage ist hier gemeint das sich der Storage Ihrer Instanzen direkt auf dem Hypervisor (Server) befindet. Das Localstorage feature, nutzbar über unsere l1 Flavors, sind für Anwendungen gedacht die geringe Latenz erfordern.

Datensicherheit und Verfügbarkeit
-----

Da Ihre Daten direkt durch Ihre Instanz auf unser Storage des lokalen Hypervisors gebunden ist, sollten Sie darauf achten diese Daten über ein HA Konzept, über die gegebenen GEC Availability Zones zu verteilen. Die Hypervisoren unterliegen unseren Compliance Patch Cycle wo wir die Hypervisoren nacheinander durchbooten müssen. Dies wird innerhalb einer Availability Zone, ein Server nach dem andrern, innernhalb eines festgelegten Maintainace Window geschehen.

**Standard Maintainance**
<!-- TODO: Wartungsfenster definieren -->

| Fenster | Tag | Zeit |
|:---|---|---:|
| 1 | Mittwoch | 10:00 - 15:00 Uhr |
| 2 | Freitag | 5:00 - 9:00 Uhr |

Openstack Features
-----

Openstack bietet Ihnen viele Features im Umgang mit Ihren Instanzen, wie z.B. resize, shelving, snapshot. Wenn Sie für Ihre Instanzen l1 Flavors verwenden möchten bitte beachten Sie dabei folgendes

_Resize:_ Die Option Resize wird Ihnen angezeit, aber es ist technisch nicht möglich eine Instanz basierend auf einem l1 Flavor zu resizen. Sie können jedoch dem jedoch entgegengehen in dem Sie ein Cluster Setup (Applicationsbezogen) mit l1 Flavors machen, größere l1 Flavors parallel starten und Ihre Daten von den alten l1 auf die neuen l1 Flavors rollen.

_Shelving/Snapshoting:_ Beide features sind möglich, aber aufgrund der disk size innerhalb der l1 Flavors raten wir davon ab, weil der zugehörige Upload lange dauern wird. Hier Empfieht sich das nutzen Ihrer externen Backup Lösung Applicationsbezogen.

Löschung der Instanz
-----

Bei der Löschung Ihrer Instanz werden von unserem System Ihre Daten mehrfach überschrieben wie es in unserer Policy beschrieben ist.

Apendix: Flavor Benchmarks
-----

Als Entscheidungshilfe welcher Flavor für Sie der richtige ist haben wir ausgibige Benchmarks erstellen lassen. Dies sind die maximal zu erreichende (bis zu) Werte. Die Benchmarks wurden auf komplett leeren Instanzen gemacht, d.h. kein laufender Prozess außer FIO.

**Sequencial Write**

| | l1.micro | l1.small | l1.medium | l1.large | l1.xlarge | l1.2xlarge |
|:---|---|---|---|---|---|---|
| Bandwitdh | ![](attachments/micro_bw_seqwrite.png) | ![](attachments/small_bw_randwrite.png) | ![](attachments/medium_bw_seqwrite.png) | ![](attachments/large_bw_seqwrite.png) | ![](attachments/xlarge_bw_seqwrite.png) | ![](attachments/2xlarge_bw_seqwrite.png) |
| IOPS | ![](attachments/micro_iops_seqwrite.png) | ![](attachments/small_iops_randwrite.png) | ![](attachments/medium_iops_seqwrite.png) | ![](attachments/large_iops_seqwrite.png) | ![](attachments/xlarge_iops_seqwrite.png) | ![](attachments/2xlarge_iops_seqwrite.png) |
| Latency | ![](attachments/micro_lat_seqwrite.png) | ![](attachments/small_lat_randwrite.png) | ![](attachments/medium_lat_seqwrite.png) | ![](attachments/large_lat_seqwrite.png) | ![](attachments/xlarge_lat_seqwrite.png) | ![](attachments/2xlarge_lat_seqwrite.png) |

**Sequencial Read**

| | l1.micro | l1.small | l1.medium | l1.large | l1.xlarge | l1.2xlarge |
|:---|---|---|---|---|---|---|
| Bandwitdh | ![](attachments/micro_bw_seqread.png) | ![](attachments/small_bw_randread.png) | ![](attachments/medium_bw_seqread.png) | ![](attachments/large_bw_seqread.png) | ![](attachments/xlarge_bw_seqread.png) | ![](attachments/2xlarge_bw_seqread.png) |
| IOPS | ![](attachments/micro_iops_seqread.png) | ![](attachments/small_iops_randread.png) | ![](attachments/medium_iops_seqread.png) | ![](attachments/large_iops_seqread.png) | ![](attachments/xlarge_iops_seqread.png) | ![](attachments/2xlarge_iops_seqread.png) |
| Latency | ![](attachments/micro_lat_seqread.png) | ![](attachments/small_lat_randread.png) | ![](attachments/medium_lat_seqread.png) | ![](attachments/large_lat_seqread.png) | ![](attachments/xlarge_lat_seqread.png) | ![](attachments/2xlarge_lat_seqread.png) |

**Random Write**

| | l1.micro | l1.small | l1.medium | l1.large | l1.xlarge | l1.2xlarge |
|:---|---|---|---|---|---|---|
| Bandwitdh | ![](attachments/micro_bw_randwrite.png) | ![](attachments/small_bw_randwrite.png) | ![](attachments/medium_bw_randwrite.png) | ![](attachments/large_bw_randwrite.png) | ![](attachments/xlarge_bw_randwrite.png) | ![](attachments/2xlarge_bw_randwrite.png) |
| IOPS | ![](attachments/micro_iops_randwrite.png) | ![](attachments/small_iops_randwrite.png) | ![](attachments/medium_iops_randwrite.png) | ![](attachments/large_iops_randwrite.png) | ![](attachments/xlarge_iops_randwrite.png) | ![](attachments/2xlarge_iops_randwrite.png) |
| Latency | ![](attachments/micro_lat_randwrite.png) | ![](attachments/small_lat_randwrite.png) | ![](attachments/medium_lat_randwrite.png) | ![](attachments/large_lat_randwrite.png) | ![](attachments/xlarge_lat_randwrite.png) | ![](attachments/2xlarge_lat_randwrite.png) |

**Random Read**

| | l1.micro | l1.small | l1.medium | l1.large | l1.xlarge | l1.2xlarge |
|:---|---|---|---|---|---|---|
| Bandwitdh | ![](attachments/micro_bw_randread.png) | ![](attachments/small_bw_randread.png) | ![](attachments/medium_bw_randread.png) | ![](attachments/large_bw_randread.png) | ![](attachments/xlarge_bw_randread.png) | ![](attachments/2xlarge_bw_randread.png) |
| IOPS | ![](attachments/micro_iops_randread.png) | ![](attachments/small_iops_randread.png) | ![](attachments/medium_iops_randread.png) | ![](attachments/large_iops_randread.png) | ![](attachments/xlarge_iops_randread.png) | ![](attachments/2xlarge_iops_randread.png) |
| Latency | ![](attachments/micro_lat_randread.png) | ![](attachments/small_lat_randread.png) | ![](attachments/medium_lat_randread.png) | ![](attachments/large_lat_randread.png) | ![](attachments/xlarge_lat_randread.png) | ![](attachments/2xlarge_lat_randread.png) |
