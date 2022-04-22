---
title: "08: Löschen der ersten eigenen Instanz"
lang: de
permalink: /optimist/guided_tour/step08/
nav_order: 1080
parent: Guided Tour
---

Schritt 8: Löschen der ersten eigenen Instanz
=============================================

Vorwort
-------

Nachdem in Schritt 7: Die erste eigene Instanz die Instanz in
der Kommandozeile angelegt wurde, wird in diesem Schritt erklärt, wie man eine
Instanz wieder löscht.

Vorgehen
--------

Damit eine Instanz generell gelöscht werden kann, wird entweder der
Name oder die ID der zu löschenden Instanz benötigt.

Bei wenigen Instanzen in einem Stack, kann der Name für das Löschen
verwendet werden.

Sobald allerdings mehrere Instanzen verwendet werden, wird von uns
empfohlen für das Löschen die ID zu nutzen, da Namen im Gegensatz zu IDs
nicht einzigartig sind.

Der OpenStackClient zeigt einem sonst an, dass es mehrere Instanzen mit
dem entsprechenden Namen gibt.

Um nun eine Liste aller verfügbaren Instanzen zu erhalten, kann
`openstack server list` als Befehl ausgeführt werden:

```bash
$ openstack server list
+--------------------------------------+--------------+--------+---------------------------------------------------+------------------------------------+
| ID                                   | Name         | Status | Networks                                          | Image Name                         |
+--------------------------------------+--------------+--------+---------------------------------------------------+------------------------------------+
| 801b3021-0c00-4566-881e-b50d47152e63 | singleserver | ACTIVE | single_internal_network=10.0.0.12, 185.116.245.39 | Ubuntu 16.04 Xenial Xerus - Latest |
+--------------------------------------+--------------+--------+---------------------------------------------------+------------------------------------+
```

Die angezeigte Liste listet alle verfügbaren Instanzen auf und enthält
neben dem Namen der jeweiligen Instanz, auch die zugehörige ID.

Um die im vorigen Schritt erstelle Instanz zu löschen, wird der Befehl
`openstack server delete ID` verwendet, wobei "ID" durch die korrekte
ID der Instanz ausgetauscht wird.

In unserem Beispiel lautet der Befehl also wie folgt:

```bash
openstack server delete 801b3021-0c00-4566-881e-b50d47152e63
```

Bei einer erneuten Ausgabe von `openstack server list`, sollte kein
Server mehr angezeigt werden:

```bash
$ openstack server list

$
```

Abschluss
---------

Nachdem im vorigen Schritt eine Instanz per Hand erstellt wurde, haben
wir diese Instanz hier gelöscht.

Außerdem konnte mit dem Befehl `openstack server list` eine Übersicht
über alle Instanzen gewonnen werden.

In Schritt 9: Die erste Security-Group wird an den bisherigen Erfahrungen angeknüpft und das gewonnene Wissen um das Thema Security-Groups erweitert.
