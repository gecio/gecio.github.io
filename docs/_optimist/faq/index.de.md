---
title: FAQ
lang: de
permalink: /optimist/faq/
nav_order: 8000
last_modified_date: 2021-03-02
---

# FAQ

## Der Befehl `openstack --help` zeigt bei vielen Punkten "Could not load EntryPoint.parse" an.

In diesem Fall ist eine der mit dem OpenStack Client installierten Komponenten nicht aktuell. Um zu sehen, welche der Komponenten
aktualisiert werden muss, rufen wir folgenden Befehl auf:

```bash
$ openstack --debug --help
```

Hier wird nun vor jedem Punkt angezeigt, was genau der Fehler ist und man kann einfach die jeweilige Komponente mit dem folgenden Befehl
aktualisieren (`<PROJECT>` muss durch die richtige Komponente ersetzt werden):

```bash
$ pip install python-<PROJECT>client -U
```

## Wie kann ich [VRRP](https://de.wikipedia.org/wiki/Virtual_Router_Redundancy_Protocol) nutzen?

Um VRRP nutzen zu können, muss dies in einer Security-Group aktiviert und dann den jeweiligen Instanzen zugeordnet werden. Aktuell ist dies
nur mit dem Openstack Client möglich. Zum Beispiel:

```bash
$ openstack security group rule create --remote-ip 10.0.0.0/24 --protocol vrrp --ethertype IPv4 --ingress  default
```

## Kann ich ED25519 SSH Keys verwenden?

Aktuell ist die Nutzung von ED25519 SSH-Keys nicht möglich.

Der Grund dafür liegt an der fehlenden Implementierung in OpenSSL, welche für die Bereitstellung von TLS und HTTP Services in OpenStack
genutzt wird. Der Fortschritt der Entwicklung ist einsehbar unter:

- [OpenSSL Issue #487](https://github.com/openssl/openssl/issues/487)
- [OpenStack Bugtracker #1555521](https://bugs.launchpad.net/nova/+bug/1555521)

## Warum werden mir Floating IPs berechnet, die ich gar nicht benutze?

Der Grund dafür ist mit hoher Wahrscheinlichkeit, dass Floating IPs erstellt wurden, aber nach der Benutzung nicht korrekt gelöscht wurden.
Um eine Übersicht über die aktuell verwendeten Floating IPs zu erhalten, kann man zum einen das
[Horizon Dashboard](https://dashboard.optimist.innovo.cloud/) nutzen.

Dort befindet sich der entsprechende Punkt unter _Project_ → _Network_ → _Floating-IPs_.

Alternativ der Weg per OpenStack Client:

```bash
$ openstack floating ip list
+--------------------------------------+---------------------+------------------+--------------------------------------+--------------------------------------+----------------------------------+
| ID                                   | Floating IP Address | Fixed IP Address | Port                                 | Floating Network                     | Project                          |
+--------------------------------------+---------------------+------------------+--------------------------------------+--------------------------------------+----------------------------------+
| 84eca713-9ac1-42c3-baf6-860ba920a23c | 185.116.245.222     | 192.0.2.7        | a3097883-21cc-49fa-a060-bccc1678ece7 | 54258498-a513-47da-9369-1a644e4be692 | b15cde70d85749689e6568f973bb002  |
+--------------------------------------+---------------------+------------------+--------------------------------------+--------------------------------------+----------------------------------+
```

## Wie kann ich den Flavor einer Instanz ändern (instance resize)?

### Resizing über die Command Line

Geben Sie den Namen oder die UUID des Servers an, dessen Größe Sie ändern möchten, und ändern Sie die Größe mit dem Befehl
`openstack server resize`.

Geben Sie das gewünschte neue Flavor und dann den Instanznamen oder die UUID an:

```bash
$ openstack server resize --flavor FLAVOR SERVER
```

Die Größenänderung kann einige Zeit in Anspruch nehmen. Während dieser Zeit wird der Instanzstatus als RESIZE angezeigt.

Wenn die Resizing abgeschlossen ist, wird der Instanzstatus VERIFY_RESIZE angezeigt. Sie können nun die Größenänderung bestätigen, um den
Status auf ACTIVE zu ändern:

```bash
$ openstack server resize --confirm SERVER
```

### Resizing über das Optimist-Dashboard

Navigieren Sie auf [Optimist Dashboard → Instances](https://dashboard.optimist.innovo.cloud/project/instances/) zu der Instanz, deren Größe
geändert werden soll, und wählen Sie dann _Actions_ → _Resize Flavor_.

Der aktuelle Flavor wird angezeigt, verwenden Sie die Dropdown-Liste "Select a new flavor", um den neuen Flavor auszuwählen und bestätigen
Sie mit "Resize".

## Warum ist das Logfile der Compute Instanz im Optimist Dashboard leer?

Bedingt durch Wartungsarbeiten oder einem Umverteilen der Last im OpenStack wurde die Instanz verschoben. In diesem Fall wird das Logfile
neu angelegt und neue Meldungen werden wie gewohnt protokolliert.

## Warum erhalte ich den Fehler "Conflict (HTTP 409)" beim Erstellen eines Swift Containers?

Swift verwendet einzigartige Namen über die gesamte OpenStack Umgebung hinweg. Die Fehlermeldung besagt, dass der gewählte Name bereits in
Verwendung ist.

## Anbringen von Cinder-Volumes an Instanzen per UUID

Wenn Sie mehrere Cinder-Volumes an eine Instanz anhängen, werden die Mount-Punkte möglicherweise bei jedem Neustart neu gemischt. Durch das
Mounten der Volumes per UUID wird sichergestellt, dass die richtigen Volumes wieder an die richtigen Mount-Punkte angehängt werden, falls
für die Instanz ein Aus- und Wiedereinschalten erforderlich ist.

Nachdem Sie die UUID des Volumes mit `blkid` in der Instanz abgerufen haben ändern Sie den Mountpunkt in `/etc/fstab` wie folgt, um die
UUID zu verwenden. Zum Beispiel:

```bash
# /boot war auf /dev/sda2 während der Installation
/dev/disk/by-uuid/f6a0d6f3-b66c-bbe3-47ba-d264464cb5a2 /boot ext4    defaults        0       2
```

## Ist die Nutzung von Cinder multi-attached Volumes möglich?

Wir unterstützen keine multi-attached Volumes in der Optimist Platform, da für die Nutzung von multi-attached Volumes clusterfähige Dateisysteme erforderlich sind, um den gleichzeitigen Zugriff auf das Dateisystem zu koordinieren.
Versuche, mutli-attached Volumes ohne clusterfähige Dateisysteme zu verwenden, bergen ein hohes Risiko der Datenkorruption, daher ist diese Funktion auf der Optimist Plattform nicht aktiviert.
