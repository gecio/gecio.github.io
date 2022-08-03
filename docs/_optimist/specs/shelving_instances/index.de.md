---
title: Shelving-Instanzen
lang: de
permalink: /optimist/specs/shelving_instances/
parent: Spezifikationen
nav_order: 9500
last_modified_date: 2022-02-02
---

# Shelving-Instanzen

## Einführung

Auf der OpenStack-Plattform haben Sie die Möglichkeit, eine Instanz zurückzustellen. Shelving-Instanzen ermöglichen es Ihnen, eine Instanz zu stoppen, ohne dass sie Ressourcen verbraucht.
Eine zurückgestellte Instanz sowie die ihr zugewiesenen Ressourcen (z.B. IP-Adresse, usw.) werden als bootfähige Instanz beibehalten.

Diese Funktion kann als Teil eines Lifecycle-Prozesses einer Instanz oder zum Einsparen von Ressourcen verwendet werden.

## Shelving einer Instanz

Instanzen auf Openstack können wie folgt abgelegt werden:
`$ openstack server shelve <server-id>`

## Unshelving einer Instanz

Instanzen können mit dem folgenden Befehl Unshelved werden:
`$ openstack server shelve <server-id>`

## Eventliste für eine Instanz anzeigen

Sie können den Shelving/Unshelving-Verlauf jedes Servers anzeigen, indem Sie die Ereignisliste anzeigen:

```bash
$ openstack server event list <server-id>
+------------------------------------------+--------------------------------------+--------+----------------------------+
| Request ID                               | Server ID                            | Action | Start Time                 |
+------------------------------------------+--------------------------------------+--------+----------------------------+
| req-8d593999-a09b-41a7-8916-1d7c28cd4dc0 | 846112be-d107-4c75-db75-a32eb47a78c5 | shelve | 2022-07-17T15:28:08.000000 |
| req-076969ee-15a4-470e-8913-051c6f9d4bd3 | 846112be-d107-4c75-db75-a32eb47a78c5 | create | 2022-07-19T16:15:22.000000 |
+------------------------------------------+--------------------------------------+--------+----------------------------+
```

## Warum Shelving verwenden?

Diese Funktion ist nützlich, um Instanzen zu archivieren, die Sie derzeit nicht verwenden, aber nicht löschen möchten. Das Shelving einer Instanz ermöglicht es Ihnen, die Instanzdaten und Ressourcenzuordnungen beizubehalten, aber gibt das Instanz-Memory frei.

Wenn Sie eine Instanz zurückstellen, generiert der Compute-Dienst ein Snapshot-Image, das den Status der Instanz erfasst, und lädt es in die Glance-Library hoch. Wenn die Instanz unshelved wird, wird sie mithilfe des Snapshots neu erstellt.
Das Snapshot-Image wird gelöscht, wenn die Instanz unshelved oder gelöscht wird.

## Wie funktioniert das abrechnungstechnisch?

Aus Abrechnungssicht wird beim S3-Preismodell nur die Root Disk abgerechnet. Die CPU und der Arbeitsspeicher, die für die jeweilige Vorratsinstanz reserviert wurden, werden nicht in Rechnung gestellt.
Die Anzahl der genutzten Instanzen verringert sich nicht vom Quota, es bleibt gleich ob eine Instanz shelved wurde oder nicht.
