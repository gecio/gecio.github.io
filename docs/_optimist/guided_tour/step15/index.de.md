---
title: "15: Das erste eigene Heat Orchestration Template (HOT)"
lang: de
permalink: /optimist/guided_tour/step15/
nav_order: 1150
parent: Guided Tour
---

Schritt 15: Das erste eigene Heat Orchestration Template (HOT)
==============================================================

Vorwort
-------

Im folgenden Schritt sind die wichtigsten Elemente eines Templates
erläutert worden und auf dieses Wissen, wird in diesem Schritt
aufgebaut.

Der Anfang
------------------------

Dieser ist bei jedem Template gleich und ist immer
`heat_template_version`   

Für das Beispiel wird Version `2016-10-14` genutzt und somit sieht das
Template erst einmal so aus:

```yaml
heat_template_version: 2016-10-14
```

Nachdem die `heat_template_version` festgelegt ist, wird dem Template
nun eine Beschreibung hinzugefügt:

```yaml
heat_template_version: 2016-10-14
 
description: Ein einfaches Template, um eine Instanz zu erstellen
```

Nachdem die Beschreibung in das Template integriert wurde, wird nun eine
Ressource, also die Instanz hinzugefügt. 

Dabei sind einige Punkte zu beachten, starten wir zunächst mit der
Ressource.

Wichtig ist dabei, dass eine Strukturierung mit Leerzeichen genutzt
wird.

Dies dient der Übersichtlichkeit, außerdem würden Tabstops zu Fehlern
führen und nur so kann das Template korrekt ausgeführt werden:

```yaml
heat_template_version: 2016-10-14

description: Ein einfaches Template, um eine Instanz zu erstellen

resources:
    Instanz:
```

Der nächste Schritt ist dann den Typ der Ressource zu benennen.

Eine ausführliche Liste aller verfügbaren Typen befindet sich unter
anderem in der [offiziellen OpenStack
Dokumentation](https://docs.openstack.org/developer/heat/template_guide/openstack.html)

Da im Beispiel eine Instanz erstellt werden soll, ist der Typ dann
folgender: 

```yaml
heat_template_version: 2016-10-14

description: Ein einfaches Template, um eine Instanz zu erstellen

resources:
    Instanz:
        type: OS::Nova::Server
```

Nach dem Typ sind dann die Eigenschaften der nächste Punkt.

Im Beispiel soll dies ein SSH-Key, ein Flavor und ein Image sein:

```yaml
heat_template_version: 2016-10-14

description: Ein einfaches Template, um eine Instanz zu erstellen

resources:
    Instanz:
        type: OS::Nova::Server
        properties:
            key_name: BeispielKey
            image: Ubuntu 16.04 Xenial Xerus - Latest
            flavor: m1.small
```

Abschluss
----------

Damit ist das Erste eigenes Template fertiggestellt und kann, wenn es
gespeichert wird, einfach mit dem OpenStackClienten wie in [Schritt 13: "Der strukturierte Weg zu einer Instanz (mit Stacks)"](/optimist/guided_tour/step13) beschrieben, gestartet werden.
