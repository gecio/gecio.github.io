---
title: "12: Eine nutzbare Instanz"
lang: de
permalink: /optimist/guided_tour/step12
nav_order: 1120
parent: Guided Tour
---

Schritt 12: Eine nutzbare Instanz
=================================

Vorwort
-------

In [Schritt 7: Die erste eigene Instanz](schritt07.md) wurde bereits eine
Instanz erstellt, diese konnte jedoch nur genutzt werden, wenn man ein paar
Schritte übersprungen hat und das entsprechende Netzwerk mit erstellt.

Es gab nur so die Möglichkeit eine Verbindung zu dieser herzustellen.

Daher werden wir in diesem Schritt eine Instanz erstellen, die diese
Problematik nicht mehr hat.

Installation
------------

Damit die Instanz all die fehlenden Einstellungen enthält, wird der
Befehl aus [Schritt 7](schritt07.md) modifiziert:

`openstack server create BeispielInstanz --flavor m1.small --key-name Beispiel --image "Ubuntu 16.04 Xenial Xerus - Latest" --security-group allow-ssh-from-anywhere --network=BeispielNetzwerk`

```bash
$ openstack server create BeispielInstanz --flavor m1.small --key-name Beispiel --image "Ubuntu 16.04 Xenial Xerus - Latest" --security-group allow-ssh-from-anywhere --network=BeispielNetzwerk
+-----------------------------+---------------------------------------------------------------------------+
| Field                       | Value                                                                     |
+-----------------------------+---------------------------------------------------------------------------+
| OS-DCF:diskConfig           | MANUAL                                                                    |
| OS-EXT-AZ:availability_zone | es1                                                                       |
| OS-EXT-STS:power_state      | NOSTATE                                                                   |
| OS-EXT-STS:task_state       | scheduling                                                                |
| OS-EXT-STS:vm_state         | building                                                                  |
| OS-SRV-USG:launched_at      | None                                                                      |
| OS-SRV-USG:terminated_at    | None                                                                      |
| accessIPv4                  |                                                                           |
| accessIPv6                  |                                                                           |
| addresses                   |                                                                           |
| adminPass                   | jkSdvP3A9yo6                                                              |
| config_drive                |                                                                           |
| created                     | 2017-12-08T12:52:37Z                                                      |
| flavor                      | m1.small (b7c4fa0b-7960-4311-a86b-507dbf58e8ac)                           |
| hostId                      |                                                                           |
| id                          | 1de98aa4-7d2b-4427-a8a5-d369ea8bdaf5                                      |
| image                       | Ubuntu 16.04 Xenial Xerus - Latest (82242d21-d990-4fc2-92a5-c7bd7820e790) |
| key_name                    | Beispiel                                                                  |
| name                        | BeispielInstanz                                                           |
| progress                    | 0                                                                         |
| project_id                  | b15cde70d85749689e08106f973bb002                                          |
| properties                  |                                                                           |
| security_groups             | name='allow-ssh-from-anywhere'                                            |
| status                      | BUILD                                                                     |
| updated                     | 2017-12-08T12:52:37Z                                                      |
| user_id                     | 9bf501f4c3d14b7eb0f1443efe80f656                                          |
| volumes_attached            |                                                                           |
+-----------------------------+---------------------------------------------------------------------------+
```

Genutzt wurden die folgenden Parameter:

-   `--flavor` = Gibt den Flavor (Größe) der Instanz an. Eine Übersicht
    aller verfügbaren Flavors kann mit  `openstack flavor list`
    aufgerufen werden
-   `--key-name` = Der Name des zu verwendenden SSH-Keys
-   `--image` = Gibt an welches Image für die Instanz genutzt wird. Auch
    ist es möglich, sich im Vorfeld eine Liste aller verfügbaren Images
    anzusehen \"openstack image list\"
-   `--security-group` = Gibt an, welche Security-Groups genutzt wird
-   `--network` = Mit diesem Parameter kann unter anderem das gewünscht
    Netzwerk angeben werden (in alten Versionen des
    clients `--nic net-id=<network>`)

Damit die erstellte Instanz über das Internet erreichbar ist, wird noch
eine IP benötigt, welche zuerst angelegt wird. 

```bash
$ openstack floating ip create provider
+---------------------+--------------------------------------+
| Field               | Value                                |
+---------------------+--------------------------------------+
| created_at          | 2017-12-08T12:53:37Z                 |
| description         |                                      |
| fixed_ip_address    | None                                 |
| floating_ip_address | 185.116.245.65                       |
| floating_network_id | 54258498-a513-47da-9369-1a644e4be692 |
| id                  | 84eca140-9ac1-42c3-baf6-860ba920a23c |
| name                | None                                 |
| port_id             | None                                 |
| project_id          | b15cde70d85749689e08106f973bb002     |
| revision_number     | 0                                    |
| router_id           | None                                 |
| status              | DOWN                                 |
| updated_at          | 2017-12-08T12:53:37Z                 |
+---------------------+--------------------------------------+
```

Die gerade erstellte IP wird im nächsten Schritt mit der vorher
erstellten Instanz verbunden.

```bash
$ openstack server add floating ip BeispielInstanz 185.116.245.145
```

Nutzung
-------

Die erstellte Instanz ist nun erreichbar.

Um zu testen, ob alle Schritte funktionieren, stellen wir nun eine
Verbindung per SSH her.

Wichtig ist hierbei, dass eine Verbindung nur funktioniert, wenn der
weiter oben genutzte SSH Key auch existiert und verwendet wird
(Siehe Schritt 6: Einen eigenen SSH-Key per Konsole erstellen und
nutzen):

```bash
$ ssh ubuntu@185.116.245.145
The authenticity of host '185.116.245.145 (185.116.245.145)' can't be established.
ECDSA key fingerprint is SHA256:kbSkm8eJA0748911RkbWK2/pBVQOjJBASD1oOOXalk.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '185.116.245.145' (ECDSA) to the list of known hosts.
Enter passphrase for key '/Users/ubuntu/.ssh/id_rsa':
```

Clean-Up
--------

Für den Fall, dass die in den vorigen Schritten erstellten Bestandteile
wieder gelöscht werden sollen, muss das in folgender Reihenfolge, mit
dem entsprechenden Befehl geschehen.

Sollte man dies nicht befolgen, kann es dazu führen, dass Bestandteile
sich nicht löschen lassen.

-   Instanz
    -   `openstack server delete BeispielInstanz`
-   Floating-IP
    -   `openstack floating ip delete 185.116.245.145`
-   Port
    -   `openstack port delete BeispielPort`
-   Router
    -   `openstack router delete BeispielRouter`
-   Subnet
    -   `openstack subnet delete BeispielSubnet`
-   Netzwerk
    -   `openstack network delete BeispielNetzwerk`

Abschluss
---------

In den Schritten 7 bis 11 wurde eine Instanz Schritt für Schritt erstellt und
jeder Schritt hat einen Teilbereich hinzugefügt (inklusive Netzwerk und einer
eigenen Security-Group).

Im [nächsten Schritt](schritt13.md) lösen wir uns von einzelnen Instanzen und
erstellen einen Stack.
