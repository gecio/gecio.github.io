---
title: "14: Die ersten Schritte mit Heat"
lang: de
permalink: /optimist/guided_tour/step14/
nav_order: 1140
parent: Guided Tour
---

# Schritt 14: Die ersten Schritte mit Heat

## Einführung

Nachdem Sie im letzten Schritt die erste Berührung mit einem Template
hatten, lernen Sie in diesem Schritt wie Heat-Templates aufgebaut sind und funktionieren.

## Das Template

Jedes Heat-Template folgt der gleichen Struktur:

```yaml
heat_template_version: 2016-10-14
 
description:
# Beschreibung des Templates (optional)
 
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

## Version des Heat Templates

Die Template-Version kann nicht willkürlich gewählt werden, sondern hat feste Vorgaben.

Diese unterscheiden sich in den möglichen Befehlen. Aktuell sind folgende Daten möglich:

- 2013-05-23
- 2014-10-16
- 2015-04-30
- 2015-10-15
- 2016-04-08
- 2016-10-14
- 2017-02-24

## Description

Die Description (Beschreibung) ist ein optionales Feld und muss nicht genutzt werden.

Es bietet sich allerdings an, denn damit können Sie das Template in seinen
Grundzügen beschreiben und auf mögliche Besonderheiten
hinweisen.

Sie haben auch die Möglichkeit, jederzeit eine Zeile mit dem Zeichen `#`
auszukommentieren und dadurch das Template nach den jeweiligen
Bedürfnissen zu verändern oder mehr Kommentare zu verfassen.

## Parameter Groups

In diesem Bereich können Sie spezifizieren, wie Parameter gruppiert werden sollen und die Reihenfolge für die Gruppierung festlegen.

Die Gruppen sind in eine Liste aufgegliedert, welche wiederum die
einzelnen Parameter enthält.

Jeder Parameter sollte nur einer Gruppe zugeordnet sein, damit es später zu keinen Problemen kommt.

Jede Parameter-Gruppe ist wie folgt aufgebaut:

```yaml
parameter_groups:
- label: <Name der Gruppe>
  description: <Beschreibung der Gruppe>
  parameters:
  - <Name des Parameters>
  - <Name des Parameters>
```

- `label`: Name der Gruppe.
- `description`: Mit diesem Attribut wird die Parameter-Gruppe beschrieben und für jeden verständlich gemacht, wofür
    sie genutzt wird.
- `parameter`: Eine Auflistung aller Parameter, die für diese Parameter-Gruppe gelten.
- `Name des Parameters`: Name, der in dem Parameter-Bereich definiert wurde.

## Parameter

Mit diesem Bereich können die eingegebenen Parameter spezifiziert werden, die für die Ausführung benötigt werden.

Die Parameter werden typischerweise dafür genutzt, jedes Deployment zu individualisieren.

Dabei wird jeder Parameter in einem separaten Block definiert, beginnend mit dem Namen des Parameters, und den weiteren Attributen.

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

- `Parameter Name`: Der Name des Parameters.
- `type`: Der Typ des Parameters. Unterstützte Typen: string, number,
    json, comma\_delimited\_list, boolean.
- `label`: Name des Parameters (optional).
- `description`: Mit diesem Attribut wird der Parameter beschrieben und für jeden verständlich gemacht, wofür dieser genutzt wird (optional).
- `default`: Der vorgegebene Wert des Parameters. Dieser Wert wird
    genutzt, wenn durch den User kein spezifischer Wert festgelegt
    werden soll (optional).
- `hidden`: Gibt an, ob der Parameter bei einer Abfrage nach der Erstellung angezeigt wird oder versteckt ist (optional und standardmäßig auf *false* gesetzt).
- `constraints`: Hier kann eine Liste von Vorgaben definiert werden. Sollten diese beim deployment nicht erfüllt werden, schlägt die Erstellung des Stacks fehl.
- `immutable`: Definiert, ob der Parameter aktualisiert werden kann. Für den Fall, dass der Parameter auf *true* gesetzt ist und sich der Wert bei einem stack update ändert, schlägt das Update fehl.

## Resources

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
    condition: <Name der Bedingung oder Ausdruck oder boolean>
```

- `ID der Ressource`: Diese muss einzigartig im Bereich der Ressourcen sein. Es darf durchaus ein sprechender Name gewählt werden z.B.
    (`web_network`).
- `type`: Typ der Ressource, zum
    Beispiel `OS::NEUTRON::SecurityGroup` (für eine Security Group) (benötigt).
- `properties`: Eine Liste der Ressourcen spezifischen Eigenschaften (optional).
- `metadata`: Hier können für die jeweilige Ressource spezifische Metadaten hinterlegt werden (optional).
- `depends_on`: Hier können verschiedene Abhängigkeiten zu anderen Ressourcen hinterlegt werden (optional).
- `update_policy`: Hier können Update-Regeln festgelegt werden. Die Voraussetzung dafür ist, dass die entsprechende Ressource dies auch unterstützt (optional).
- `deletion_policy`: Hier werden die Regeln für das Löschen festgelegt. Erlaubt sind Delete, Retain und Snapshot. Mit der heat_template_version 2016-10-14 ist nun auch die Kleinschreibung der Werte erlaubt.

- `external_id`: Falls notwendig, können externe Ressourcen IDs verwendet werden.
- `condition`: Anhand der Bedingung wird entschieden, ob die Ressource erstellt wird oder nicht (optional).

## Output

Der Output-Bereich definiert die ausgegeben Parameter, die für den User oder auch für andere Templates nach dem Erstellen des Stacks verfügbar
sind.

Das können zum Beispiel Parameter wie die IP-Adresse der erstellten Instanz oder auch die URL der erstellten Web-App sein.

Auch wird jeder Parameter in einem eigenen Block definiert:

```yaml
outputs:
  <Name des Parameters>:
    description: <Beschreibung>
    value: <Wert des Parameters>
    condition: <Name der Kondition oder Ausdruck oder boolean>
```

- `Name des Parameters`: Dieser muss wieder einzigartig sein.
- `description`: Es kann eine Beschreibung für den Parameter hinterlegt werden (optional).
- `value`: Hier wird der Wert des Parameters vermerkt (benötigt).
- `condition`: Hier kann eine Bedingung für den Parameter festgelegt werden (optional).

## Condition

Dieser Bereich legt die verschiedenen Bedingungen beim Erstellen oder Update des Stacks fest auf Basis der eingegebenen User-Parameter.

Die Bedingungen können mit Resources, Resource Properties und Outputs verbunden werden.

Auch dieser Bereich folgt wieder einem Muster:

```yaml
conditions:
  <Name der Condition1>: {Bezeichnung1}
  <Name der Condition2>: {Bezeichnung2}
```

- `Name der Condition`: Dieser muss wieder einzigartig im Bereich der Bedingung sein.
- `Bezeichnung`: Bei der Bezeichnung wird erwartet, dass sie ein *true* oder *false* zurückgibt.

## Zusammenfassung

In diesem Schritt haben Sie wichtige Bestandteile eines Heat-Templates kennengelernt.

Mit diesem Wissen können Sie im nächsten Schritt das erste eigene Heat-Template erstellen.
