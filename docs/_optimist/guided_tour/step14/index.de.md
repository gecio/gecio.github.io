---
title: "14: Unsere ersten Schritte mit Heat"
lang: de
permalink: /optimist/guided_tour/step14
nav_order: 1140
parent: Guided Tour
---

Schritt 14: Unsere ersten Schritte mit Heat
===========================================

Vorwort
-------

Nachdem im letzten Schritt die erste Berührung mit einem Template
erfolgte, ist der nächste Schritt, zu verstehen, wie Templates mit Heat
aufgebaut sind und funktionieren.

Dieser Schritt erklärt nur die einzelnen Punkte eines Templates und soll
diese näher bringen.

Das Template
------------

Jedes Heat-Template folgt der gleichen Struktur und diese ist wie folgt
aufgebaut:

```yaml
heat_template_version: 2016-10-14 
 
description: 
# Die Beschreibung des Templates (optional) 
 
parameter_groups: 
# Die Definition der Eingabeparameter Gruppen und deren Reihenfolge 
 
parameters: 
# Die Definition der Eingabeparameter 
 
resources: 
# Die Definition der Ressourcen des Templates 
 
outputs: 
# Die Definition der Ausgangsparameter 
 
conditions: 
# Die Definition der Bedingungen
```

Heat Template Version
---------------------

Die Template Version kann tatsächlich nicht willkürlich gewählt werden,
sondern hat feste Vorgaben.

Diese unterscheiden sich in den möglichen Befehlen und aktuell sind
folgende Daten möglich:

-   2013-05-23
-   2014-10-16
-   2015-04-30
-   2015-10-15
-   2016-04-08
-   2016-10-14
-   2017-02-24

Description
-----------

Die Description oder auch Beschreibung ist ein komplett optionales
Feld. 

Das bedeutet, dass das Feld nicht genutzt werden muss.

Es bietet sich allerdings an, denn damit kann das Template in seinen
Grundzügen beschrieben und direkt auf mögliche Besonderheiten
hingewiesen werden.

Auch besteht die Möglichkeit jederzeit eine Zeile mit dem Zeichen `#`
auszukommentieren und dadurch das Template nach den jeweiligen
Bedürfnissen zu verändern oder mehr Kommentare zu verfassen. 

Parameter Groups
----------------

In diesem Bereich ist es möglich zu spezifizieren, wie Parameter
gruppiert werden sollen und die Reihenfolge für die Gruppierung
festzulegen.

Die Gruppen sind in eine Liste aufgegliedert, welche wiederum die
einzelnen Parameter enthält.

Jeder Parameter sollte nur einer Gruppe zugeordnet sein, damit es später
zu keinen Problemen führt.

Jede Parameter Group ist dabei wie folgt aufgebaut:

```yaml
parameter_groups: 
- label: <Name der Gruppe> 
  description: <Beschreibung der Gruppe> 
  parameters: 
  - <Name des Parameters> 
  - <Name des Parameters>
```

-   `label`: Name der Gruppe.
-   `description`: Dieses Attribut gibt  die Möglichkeit die Parameter
    Gruppe zu beschreiben und so für jeden verständlich zu machen, wofür
    diese genutzt wird.
-   `parameter`: Eine Auflistung aller Parameter die für diese Parameter
    Gruppe gelten.
-   `Name des Parameters`: Der in der Parameter Sektion definiert wurde.

Parameter
---------

Diese Sektion erlaubt es die eingegebenen Parameter zu spezifizieren,
welche für die Ausführung benötigt werden.

Die Parameter werden typischerweise dafür genutzt, jedes deployment zu
individualisieren.

Dabei wird jeder Parameter in einem separaten Block definiert, wobei am
Anfang immer der Parameter genannt wird und dann weitere Attribute
diesem zugeordnet werden. 

```yaml
 parameters:
  <Parameter Name>:
    type: <string | number | json | comma_delimited_list | boolean>
    label: <Name des Parameters>
    description: <Beschreibung des Parameters>
    default: <Standardwert des Parameters>
    hidden: <true | false>
    constraints:
      <Vorgaben für den Parameter>
    immutable: <true | false>
```

-   `Parameter Name`: Der Name des Parameters.
-   `type`: Der Typ des Parameters. Unterstützte Typen: string, number,
    json, comma\_delimited\_list, boolean.
-   `label`: Name des Parameters. (optional)
-   `description`: Dieses Attribut gibt die Möglichkeit den Parameter zu
    beschreiben und so für jeden verständlich zu machen, wofür dieser
    genutzt wird. (optional)
-   `default`: Der vorgegebene Wert des Parameters. Dieser Wert wird
    genutzt, wenn durch den User kein spezifischer Wert festgelegt
    werden soll. (optional)
-   `hidden`: Gibt an ob der Parameter bei einer Abfrage nach der
    Erstellung angezeigt wird oder versteckt ist. (optional und per
    Default auf *false* gesetzt)
-   `constraints`: Hier kann eine Liste von Vorgaben definiert werden.
    Sollten diese beim deployment nicht erfüllt werden, schlägt die
    Erstellung des Stacks fehl.
-   `immutable`: Definiert ob der Parameter aktualisiert werden kann.
    Für den Fall, dass der Parameter auf *true* gesetzt ist und sich der
    Wert bei einem stack update ändert, schlägt das Update fehl.

Resources
---------

Dieser Punkt definiert die aktuell genutzten Resourcen, die bei der Erstellung
des Stacks genutzt werden. 

Dabei wird jede Ressource in einem eigenen Block definiert:

```yaml
resources:
  <ID der Ressource>:
    type: <Ressourcen Typ>
    properties:
      <Name der Eigenschaft>: <Wert der Eigenschaft>
    metadata:
      <Ressourcen spezifische Metadaten>
    depends_on: <Ressourcen ID oder eine Liste der IDs>
    update_policy: <Update Regel>
    deletion_policy: <Regel für das Löschen>
    external_id: <Externe Ressourcen ID>
    condition: <Name der Kondition oder Ausdruck oder boolean>
```

-   `ID der Ressource`: Diese muss einzigartig im Bereich der Ressourcen
    sein. Es darf durchaus ein sprechender Name gewählt werden z. B.
    (`web_network`).
-   `type`: Der Typ der Ressource, wie zum
    Beispiel `OS::NEUTRON::SecurityGroup` (für eine Security Group).
    (benötigt)
-   `properties`: Eine Liste der Ressourcen spezifischen Eigenschaften.
    (optional) 
-   `metadata`: Hier können für die jeweilige Ressource spezifische
    Metadaten hinterlegt werden. (optional)
-   `depends_on`: Hier können verschiedene Abhängigkeiten zu anderen
    Ressourcen hinterlegt werden. (optional)
-   `update_policy`: Hier können Update Regeln festgelegt werden. Die
    Voraussetzung dafür ist, dass die entsprechende Resource dies auch
    unterstützt. (optional)
-   `deletion_policy`: Hier werden die Regeln für das Löschen
    festgelegt. Erlaubt sind Delete, Retain und Snapshot. Mit der
    heat_template_version 2016-10-14 ist nun auch die Kleinschreibung
    der Werte erlaubt

-   `external_id`: Falls notwendig, können externe Ressourcen IDs
    verwendet werden
-   `condition`: Anhand der Kondition wird entschieden, ob die Ressource
    erstellt wird oder nicht. (optional)

Output
------

Die Output-Sektion definiert die ausgegeben Parameter die für den User
oder auch für andere Templates nach dem Erstellen des Stacks verfügbar
sind.

Das können zum Beispiel Parameter wie die IP-Adresse der erstellten
Instanz oder auch die URL der erstellten Web-App sein.

Auch wird jeder Parameter in einem eigenen Block definiert:

```yaml
outputs:
  <Name des Parameters>:
    description: <Beschreibung>
    value: <Wert des Parameters>
    condition: <Name der Kondition oder Ausdruck oder boolean>
```

-   `Name des Parameters`: Dieser muss wieder einzigartig sein.
-   `description`: Es kann eine Beschreibung für den Parameter
    hinterlegt werden. (optional)
-   `value`: Hier wird der Wert des Parameters vermerkt. (benötigt)
-   `condition`: Hier kann eine Kondition für den Parameter festlegt
    werden. (optional)

Condition
---------

Dieser Bereich legt die verschiedenen Konditionen fest und das auf Basis
der eingegebenen Parameter des Users, beim Erstellen oder updaten des
Stacks.

Die Konditionen können mit Resources, Resource properties und
Outputs verbunden werden.

Auch dieser Bereich folgt wieder einem Muster:

```yaml
conditions:
  <Name der Condition1>: {Bezeichnung1}
  <Name der Condition2>: {Bezeichnung2}
```

-   `Name der Condition`: Dieser muss wieder einzigartig im Bereich der
    Condition sein.
-   `Bezeichnung`: Bei der Bezeichnung wird erwartet, dass sie ein
    *true* oder *false* zurückgibt.

Abschluss
---------

In diesem Schritt wurden wichtige Bestandteile eines Heat-Templates
vorgestellt. 

Mit diesem Wissen wird im [nächsten Schritt](schritt15.md) das erste eigene
Heat-Template erstellt.
