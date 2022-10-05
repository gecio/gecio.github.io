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

Beim Localstorage befindet sich der Storage Ihrer Instanzen direkt auf dem Hypervisor (Server). Die Localstorage Funktion ist   über unsere l1 Flavors verfügbar und für Anwendungen vorgesehen, die eine geringe Latenz erfordern.

Datensicherheit und Verfügbarkeit
-----

Da Ihre Daten direkt durch Ihre Instanz auf dem Storage des lokalen Hypervisors gebunden sind, empfiehlt es sich, diese Daten mithilfe eines HA Konzepts über die gegebenen GEC Availability Zonen zu verteilen. Die Hypervisor unterliegen unserem Compliance Patch Zyklus, bei dem  die Hypervisoren nacheinander gebootet werden müssen. Dabei wird innerhalb einer Availability Zone und innerhalb eines festgelegten Wartungsfensters ein Server nach dem anderen gebootet.
Das Storage Array ist gegen den Ausfall von Einzelkomponenten geschützt. Dies bezieht sich jedoch nur innerhalb eines einzelnen Hypervisors. Beim Ersetzen von Einzelkomponenten aufgrund eines Hardwaredefekts, kann es bis zur Wiederherstellung kurzfristig zu einer eingeschränkten Verfügbarkeit und Performance kommen.

**Standard Maintenance**
<!-- TODO: Wartungsfenster definieren -->

| Fenster | Tag | Zeit |
|:---|---|---:|
| 1 | Mittwoch | 10:00 - 15:00 Uhr |
| 2 | Freitag | 5:00 - 9:00 Uhr |

Openstack Features
-----

OpenStack bietet Ihnen viele Funktionen für  Ihre Instanzen, wie z.B. resize, shelving oder snapshot. Wenn Sie für Ihre Instanzen l1 Flavors verwenden möchten, beachten Sie bitte folgendes:

_Resize:_ Die Option Resize wird Ihnen angezeigt, aber technisch ist es nicht möglich, eine auf einem l1 Flavor basierende Instanz zu resizen. Sie können das umgehen, in dem Sie einen Cluster (Applikationsbezogen) mit l1 Flavors aufsetzen, größere l1 Flavors parallel starten und Ihre Daten von den alten l1 Flavors auf die neuen l1 Flavors rollen.

_Shelving/Snapshoting:_ Beide Features sind möglich, aber aufgrund der Disk Size innerhalb der l1 Flavors raten wir wegen der langen Uploadzeiten davon ab. Hier empfiehlt es sich, die für die Applikation vorgesehene externe Backup-Lösung zu nutzen.

Löschen der Instanz
-----

Beim Löschen Ihrer Instanz werden von unserem System wie in unserer Policy beschrieben, Ihre Daten mehrfach überschrieben.

Apendix: Flavor Benchmarks
-----

Als Entscheidungshilfe welcher Flavor für Sie der richtige ist, haben wir ausgiebige Benchmarks erstellen lassen. Dies sind die maximal zu erreichenden (bis zu) Werte. Die Benchmarks wurden auf leeren Instanzen, also ohne laufendem Prozess außer FIO, gemacht.

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
