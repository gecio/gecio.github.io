---
title: "15: Das erste eigene Heat Orchestration Template (HOT)"
lang: "de"
permalink: /optimist/guided_tour/step15/
nav_order: 1150
parent: Guided Tour
---

# Schritt 15: Das erste eigene Heat Orchestration Template (HOT)

## Einführung

Im vorherigen Schritt haben wir die wichtigsten Elemente eines Templates
erläutert. In diesem Schritt bauen wir auf dieses Wissen auf.

# Der Anfang

Dieser ist bei jedem Template gleich und ist immer
`heat_template_version`

Für das Beispiel wird Version `2016-10-14` genutzt:

```yaml
heat_template_version: 2016-10-14
```

Nachdem die `heat_template_version` festgelegt ist, können Sie dem Template eine Beschreibung hinzufügen:

```yaml
heat_template_version: 2016-10-14
 
description: Ein einfaches Template, um eine Instanz zu erstellen
```

Nachdem die Beschreibung in das Template integriert wurde, fügem wir nun eine
Ressource, also die Instanz hinzu.

Dabei sind einige Punkte zu beachten. Wir starten zunächst mit der Ressource.

Wichtig ist dabei, dass wir eine Strukturierung mit Leerzeichen vornehmen.

Dies dient der Übersichtlichkeit, außerdem würden Tabstops zu Fehlern
führen. Nur so kann das Template korrekt ausgeführt werden:

```yaml
heat_template_version: 2016-10-14

description: Ein einfaches Template, um eine Instanz zu erstellen

resources:
    Instanz:
```

Der nächste Schritt ist, den Typ der Ressource zu benennen.

Eine ausführliche Liste aller verfügbaren Typen befindet sich unter
anderem in der [offiziellen OpenStack
Dokumentation](https://docs.openstack.org/developer/heat/template_guide/openstack.html).

Da im Beispiel eine Instanz erstellt werden soll, ist der Typ dann
folgender:

```yaml
heat_template_version: 2016-10-14

description: Ein einfaches Template, um eine Instanz zu erstellen

resources:
    Instanz:
        type: OS::Nova::Server
```

Nun geben wir die Eigenschaften an.

Im Beispiel ist das ein SSH-Key, ein Flavor und ein Image:

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

## Zusammenfassung

Sie haben Ihr erstes eigenes Template fertiggestellt. Nachdem Sie es gespeichert habe, können Sie es mit dem OpenStack Client wie in [Schritt 13](/optimist/guided_tour/step13/) beschrieben, starten.
