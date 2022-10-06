---
title: Localstorage
lang: "de"
permalink: /optimist/storage/localstorage/
nav_order: 3200
parent: Storage
has_children: false
---

# Compute Localstorage für Ihre Instanzen

## Was genau ist Compute Localstorage?

Beim Localstorage befindet sich der Storage Ihrer Instanzen direkt auf dem Hypervisor (Server). Die Localstorage Funktion ist über unsere l1 Flavors verfügbar und für Anwendungen vorgesehen, die eine geringe Latenz erfordern.

## Datensicherheit und Verfügbarkeit

Da Ihre Daten direkt durch Ihre Instanz auf dem Storage des lokalen Hypervisors gebunden sind, empfiehlt es sich, diese Daten mithilfe eines HA Konzepts über die gegebenen GEC Availability Zonen zu verteilen. Die Hypervisor unterliegen unserem Compliance Patch Zyklus, bei dem  die Hypervisoren nacheinander gebootet werden müssen. Dabei wird innerhalb einer Availability Zone und innerhalb eines festgelegten Wartungsfensters ein Server nach dem anderen gebootet.
Das Storage Backend der Localstorage Instanzen ist gegen einen Ausfall einzelner Speichermedien des Arrays geschützt, die dadurch hergestellte Redundanz besteht jedoch gegenüber der Ceph basierten Instanzen nur innerhalb des Hypervisor Nodes welcher die Instanz bereitstellt. Beim Ersetzen von Einzelkomponenten auf Grund eines Hardwaredefekts, kann es bis zur Wiederherstellung kurzfristig zu einer eingeschränkten Verfügbarkeit und Performance kommen.

## Standard Wartungsfenster

| Fenster | Availability Zone | Monat | Wöchentlich | Tag | Zeit |
|:---|---|---|---|---|---:|
| 1 | ES1 | Januar, April, Juli, Oktober | 25% der Hypervisors | Mittwoch | 10:00 Uhr - 15:00 Uhr |
| 2 | IX1 | Februar, Mai, August, November | 25% der Hypervisors | Mittwoch | 10:00 Uhr - 15:00 Uhr |
| 3 | IX2 | März, Juni, September, Dezember | 25% der Hypervisors | Mittwoch | 10:00 Uhr - 15:00 Uhr |

## Openstack Features

OpenStack bietet Ihnen viele Funktionen für Ihre Instanzen, wie z.B. resize, shelving oder snapshot. Wenn Sie für Ihre Instanzen l1 Flavors verwenden möchten, beachten Sie bitte folgendes:

_Resize:_ Die Option Resize wird Ihnen angezeigt, aber technisch ist es nicht möglich, eine auf einem l1 Flavor basierende Instanz zu resizen. Sie können das umgehen, in dem Sie einen Cluster (Applikationsbezogen) mit l1 Flavors aufsetzen, größere l1 Flavors parallel starten und Ihre Daten von den alten l1 Flavors auf die neuen l1 Flavors rollen. Dies gilt auch bei einem wechsel von einem andren Flavor zu l1 Flavors.

_Shelving/Snapshotting:_ Beide Features sind möglich, aber aufgrund der Disk Size innerhalb der l1 Flavors raten wir wegen der langen Uploadzeiten davon ab. Hier empfiehlt es sich, die für die Applikation vorgesehene externe Backup-Lösung zu nutzen.

## Löschen der Instanz

Beim Löschen Ihrer Instanz werden von unserem System wie in unserer Policy beschrieben, Ihre Daten mehrfach überschrieben.
