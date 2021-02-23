---
title: Schritt 16 - Wir lernen Heat besser kennen
lang: de
permalink: /optimist/guided_tour/step16
nav_order: 3600
parent: Guided Tour
---

Schritt 16: Wir lernen Heat besser kennen
=========================================

Vorwort
-------

Bisher konnte der Eindruck entstehen, das Heat und das manuelle
Erstellen per Kommandozeile genau so viel Zeit in Anspruch nimmt, was
beim einmaligen Erstellen auch stimmt.

Dadurch das nun ein Template existiert, können wir diese Grundlage immer
wieder nutzen und im Zweifel weiter entwickeln.

Damit dies auch möglich ist, wird in diesem Schritt weiter auf Heat
eingegangen.

Parameter
---------

Da das Ganze aufgebaut werden soll, ist es zunächst sinnvoll, bekannte
oder individuelle Parameter zu definieren. 

In diesem Kontext wird der vorgegebene SSH-Key ersetzt und statt einem
festen Wert, wird er als individueller Parameter definiert, der beim
Start angegeben werden kann:

```yaml
heat_template_version: 2014-10-16
 
parameters:
  key_name:
    type: string
```

Wie bisher gelernt, beginnt das Template mit der Version und wird dann
mit `parameters` fortgeführt.

Nach dem Parameter wird der Name, welcher individuell benannt werden
kann, vergeben.

Auch ist es notwendig den Typ anzugeben, in diesem Fall ist es `string`.

Nachdem der Parameter festgelegt ist, nutzen wir als Vorlage das vorige
Template und ergänzen es. 

Damit sieht das Template dann so aus: 

```yaml
heat_template_version: 2014-10-16

parameters:
  key_name:
    type: string


resources:
  Instanz:
    type: OS::Nova::Server
    properties:
      key_name: BeispielKey
      image: Ubuntu 16.04 Xenial Xerus - Latest
      flavor: m1.small
```

Um das Template zu komplettieren, wird `key_name` durch den vorher
definierten Parameter ersetzt.

Der Befehl dafür lautet `get_param`.

Dieser sagt aus, dass er einen definierten Parameter nutzen soll und
damit das Template weiß, welchen Parameter er nutzen soll, ergänzen wir
den Befehl `get_param` um `key_name`:

```yaml
heat_template_version: 2014-10-16

parameters:
  key_name:
    type: string

resources:
  Instanz:
    type: OS::Nova::Server
    properties:
      key_name: { get_param: key_name }
      image: Ubuntu 16.04 Xenial Xerus - Latest
      flavor: m1.small
```

Abschluss
---------

Das Template wurde jetzt bereits über einen frei definierbaren Parameter
erweitert und im [nächsten Schritt](schritt17.md) wird das Netzwerk
hinzugefügt.
