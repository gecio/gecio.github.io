---
title: "08: Löschen der ersten eigenen Instanz"
lang: "de"
permalink: /optimist/guided_tour/step08/
nav_order: 1080
parent: Guided Tour
---

# Schritt 8: Löschen der ersten eigenen Instanz

## Einführung

Nachdem Sie in [Schritt 7](/optimist/guided_tour/step07/), die erste eigene Instanz in der Kommandozeile angelegt haben, erklären wir in diesem Schritt, wie Sie eine Instanz wieder löschen.

## Vorgehensweise

Damit Sie eine Instanz löschen können, benötigen Sie entweder den Namen oder die ID der zu löschenden Instanz.

Existieren nur wenige Instanzen in einem Stack, können Sie den Namen für das Löschen verwenden.

Werden allerdings mehrere Instanzen verwendet, empfehlen wir Ihnen, für das Löschen die ID zu nutzen, da Namen im Gegensatz zu IDs nicht einzigartig sind.

Der OpenStackClient zeigt Ihnen ansonsten an, dass es mehrere Instanzen mit dem entsprechenden Namen gibt.

Sie können sich eine Liste aller verfügbaren Instanzen mit dem Befehl
`openstack server list` anzeigen lassen:

```bash
$ openstack server list
+--------------------------------------+--------------+--------+---------------------------------------------------+------------------------------------+
| ID                                   | Name         | Status | Networks                                          | Image Name                         |
+--------------------------------------+--------------+--------+---------------------------------------------------+------------------------------------+
| 801b3021-0c00-4566-881e-b50d47152e63 | singleserver | ACTIVE | single_internal_network=10.0.0.12, 185.116.245.39 | Ubuntu 16.04 Xenial Xerus - Latest |
+--------------------------------------+--------------+--------+---------------------------------------------------+------------------------------------+
```

Die angezeigte Liste enthält alle verfügbaren Instanzen und
neben dem Namen der jeweiligen Instanz auch die zugehörige ID.

Um die im vorigen Schritt erstellte Instanz zu löschen, verwenden Sie den Befehl
`openstack server delete ID`, wobei Sie "ID" durch die korrekte
ID der Instanz ersetzen müssen.

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

## Zusammenfassung

Nachdem Sie in [Schritt 7](/optimist/guided_tour/step07/) eine Instanz per Hand erstellt haben, haben Sie in diesem Schritt diese Instanz  wieder gelöscht.

Außerdem haben Sie gelernt, wie Sie sich mit dem Befehl `openstack server list` eine Liste aller Instanzen anzeigen lassen können.

In [Schritt 9](/optimist/guided_tour/step09/) beschäftigen wir uns mit dem Thema Security-Groups.
