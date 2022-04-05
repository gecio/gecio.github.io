---
title: OpenStack Images
lang: de
permalink: /optimist/specs/images/custom_images/
parent: Images
nav_order: 9420
---

# Hochladen von benutzerdefinierten Images auf OpenStack

Benutzer können nicht nur aus einer Reihe eigener Images auswählen, sondern auch ihre eigenen Images hochladen, entweder über das Optimist-Dashboard oder über die CLI.

## Image über Dashboard hochladen

Ein Image kann in das Dashboard hochgeladen werden, indem Sie zum Abschnitt Images des Dashboards unter Project > Images > Create Image 

Klicken Sie auf "Image erstellen", um mit der Erstellung eines neuen benutzerdefinierten Imagees zu beginnen. Hier sehen Sie die folgenden Optionen:
![](attachments/CreateImageDashboard.png)

Die einzelnen Abschnitte im Überblick:
- `Name`: Geben Sie einen Namen für das Image ein.
- `Description`: Geben Sie eine kurze Beschreibung des Imagees ein.
- `Image Source (File)`: Hier können wir die Imagedatei hochladen, die wir verwenden möchten.
- `Format`: Das hängt von unserer Imageformaterweiterung ab, in diesem Fall verwenden wir qcow2.
- `Image Sharing`: Protected (Yes/No) – Wenn Sie "No" auswählen, kann das Image mit der Benutzerbasis geteilt werden.

Die anderen Felder sind optional: Kernel, Ramdisk, Minimum Disk (GB) und Minimum RAM (GB) können bei Bedarf leer gelassen werden.

Klicken Sie auf „Image erstellen“, um mit der Erstellung des Imagees zu beginnen. Das Image wird in Kürze auf dem Dashboard verfügbar sein.

## Image über CLI hochladen

Wir können dieselbe Datei auch über die CLI hochladen. Abhängig vom Speicherort der Datei kann es hilfreich sein, den touch-Befehl auszuführen, um sicherzustellen, dass sie erreichbar ist:

`$ touch debian-11-genericcloud-amd64.qcow2`

Mit dem folgenden Befehl können wir nun direkt aus der CLI heraus ein Image erstellen:
```bash
$ openstack image create --disk-format qcow2 --container-format bare --private --file ./debian-11-genericcloud-amd64.qcow2 CustomDebianBullseyeImage
+------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Field            | Value                                                                                                                                                                         |
+------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| container_format | bare                                                                                                                                                                          |
| created_at       | 2022-03-31T06:47:19Z                                                                                                                                                          |
| disk_format      | qcow2                                                                                                                                                                         |
| file             | /v2/images/532e26209e22-4048-5747-9b36-28e0d279/file                                                                                                                          |
| id               | 532e26209e22-4048-5747-9b36-28e0d279                                                                                                                                          |
| min_disk         | 0                                                                                                                                                                             |
| min_ram          | 0                                                                                                                                                                             |
| name             | CustomDebianBullseyeImage                                                                                                                                                     |
| owner            | ac5a32b34bc906ac5a32b34b54a                                                                                                                                                   |
| properties       | locations='[]', os_hidden='False', owner_specified.openstack.md5='', owner_specified.openstack.object='images/CustomDebianBullseyeImage', owner_specified.openstack.sha256='' |
| protected        | False                                                                                                                                                                         |
| schema           | /v2/schemas/image                                                                                                                                                             |
| status           | queued                                                                                                                                                                        |
| tags             |                                                                                                                                                                               |
| updated_at       | 2022-03-31T06:47:19Z                                                                                                                                                          |
| visibility       | private                                                                                                                                                                       |
+------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
```

Der Befehl erfordert mindestens diese Felder:
- `--disk-format`: qcow2 - in diesem Fall. Dies hängt von der Imagedateierweiterung ab.
- `--container-format`: leer
- `--private`: Geben Sie an, ob unser Image --private oder --public sein soll
- `--file` Die spezifische Imagedatei, die wir verwenden möchten, zum Beispiel: ./debian-11-genericcloud-amd64.qcow2
- Name des Imagees: In diesem Fall "CustomDebianBullseyeImage"

Es gelten die gleichen Felder wie beim Hochladen über das Dashboard, dazu die optionalen Felder mit Minimum Disk / RAM etc.
