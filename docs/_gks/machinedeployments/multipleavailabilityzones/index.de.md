---
title: Mehrere Verfügbarkeitszonen
lang: "de"
permalink: /gks/machinedeployments/multipleavailibilityzones/
nav_order: 6600
parent: Machine Deployments
---
<!-- LTeX:  language=de-DE -->

# Mehrere Verfügbarkeitszonen

## Einleitung

Ein *machine deployment* ist immer an einer Verfügbarkeitszone (im
Weiteren VZ abgekürzt) gebunden, eine Maschine zu jedem Zeitpunkt
immer nur in einer Verfügbarkeitszone existieren kann und *machine
deployments* generell nicht aus Maschinen verschiedener
Verfügbarkeitszonen bestehen. Dies beschränkt den nutzbaren
persistenten Speicher für Pods auf die jeweilige Verfügbarkeitszone in
der der Pod das erste mal ge-schedule'd wurde.


## Setups in einer Verfügbarkeitszone

Im Normalfall hat ein Kubernetes Cluster zum Erstellungszeitpunkt nur
ein *machine deployment*, welches in genau einer Verfügbarkeitszone
liegt und somit keinerlei Probleme bereitet. Wenn ein Pod auf einer
Maschine dieses *machine deployments* laufen soll und persistenten
Speicher braucht, fordert er einfach Speicher aus dieser Verfügbarkeitszone
an. Sollte der Pod von seiner Maschine verdrängt werden, wählt
Kubernetes automatisch eine passende Maschine mit hinreichend
Ressourcen aus und fährt den Pod auf seinem neuen Ziel wieder hoch.
Der dabei an die alte Maschine gebundene persistente Speicher wird
nahtlos an der neuen Maschine zur Verfügung gestellt und der Pod
kann wie erwartet normal hochfahren.


## Setups in mehreren Verfügbarkeitszonen

### Setups beschränkt auf einer von mehreren Verfügbarkeitszonen

Man kann mehr als ein *machine deployment* haben. Wenn man sich für
ein zweites (oder n-tes) entscheidet kann man wählen, in welcher
Verfügbarkeitszone dieses neue *machine deployment* liegen soll.
Es ist möglich das neue *machine deployment* in dieselbe Verfügbarkeitszone
zu stellen wie das Erste oder es in eine der anderen VZn (dies
ist abhängig vom geplanten Verwendungszweck; beides kann sinnvoll sein).

Existieren zwei *machine deployments* in zwei verschiedenen Verfügbarkeitszonen
so kann es passieren dass ein Pod von seiner Maschine in VZ A verdrängt
wird und der Kubernetes Scheduler einen neuen Pod in der VZ B einplant
und laufen lassen will. Der Pod wird jedoch im Status *pending* verbleiben
und in der Beschreibung einen Fehler hinterlassen dass es nicht möglich
ist die Voraussetzungen für das Hochfahren dieses Pods zu erfüllen.

Der Grund hierfür liegt im bereits oben beschriebenen Verhalten dass
der persistente Speicher ebenfalls an eine Verfügbarkeitszone gebunden
ist und nicht automatisch migriert werden kann.

Es gibt zwei Lösungsmöglichkeiten. Entweder man investiert in einen
persistenten Speicher der VZ-übergreifend angeboten wird (sehr teuer
und nicht in allen Lokationen verfügbar). Eine Alternative ist es
Kubernetes beizubringen diese Art von Pod (Applikation) explizit nur
in einer bestimmten Verfügbarkeitszone zu platzieren. Dies kann
sowohl mit einem Deployment, StatefulSet oder Daemonset erreicht werden.

Dieses Kubernetes Feature heißt *affinity* und kann Pods sowohl
an (oder gegen) seine eigenen oder andere Pods als auch gegen Nodes
verwendet werden.

Hier ein Beispiel für ein Deployment welches in der VZ ES1 konfiguriert
wird:


```bash
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: myapp
  name: myapp
  namespace: default
spec:
  replicas: 1
  ...
  template:    
    ...
    spec:
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: topology.kubernetes.io/zone:
                operator: In
                values:
                - es1
      ...
```
(Die drei aufeinander folgenden Punkt symbolisieren dass hier Zeilen
 aus Gründen der einfacheren Lesbarkeit unterschlagen wurden. Das
 Deployment wird ohne diese unterschlagenen Zeilen nicht funktionieren!)


### Setups verteilt über mehrere Verfügbarkeitszonen

Um die Verfügbarkeit der eigenen Applikation zu erhöhen ist es gängig,
diese in zwei verschiedenen Verfügbarkeitszonen zu platzieren, um den
Verlust einer ganzen Verfügbarkeitszone mitigieren zu können (natürlich
muss die Applikation dies auch unterstützen aber das setzen wir hier
mal als gegeben voraus).

Um dies zu Erreichen muss man Kubernetes so konfigurieren dass es
zwei Repliken einer Applikation immer in zwei verschiedene
Verfügbarkeitszonen hochfährt. Zusätzlich muss die jeweilige Applikation
in derselben VZ bleiben in der der persistente Speicher liegt.

Dies kann mit der *podAntiAffinity* aus dem folgenden Beispiel
erreicht werden. In diesem Fall wählen wir ein StatefulSet
welches uns die Zuordnung von den Pods zu den jeweils richtigen
persistenten Speicherobjekten erleichtert:

```bash
apiVersion: apps/v1
kind: StatefulSet
metadata:
  labels:
    app: myapp
  name: myapp
  namespace: default
spec:
  replicas: 2
  ...
  template:    
    ...
    spec:
      affinity:
        podAntiAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
          - labelSelector:
              matchExpressions:
              - key: app
                operator: In
                values:
                - myapp
            topologyKey: topology.kubernetes.io/zone
      ...
```
(Wieder symbolisieren die drei aufeinander folgenden Punkt dass hier Zeilen
 aus Gründen der einfacheren Lesbarkeit unterschlagen wurden. Das
 StatefulSet wird ohne diese unterschlagenen Zeilen nicht funktionieren!)

Um mehr über die Möglichkeiten des Platzierens von Pods in Kubernetes
zu lernen folgen sie bitte den unten stehenden Links.


## Weiterführende Themen

* [einfache Affinity](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/)
* [Affinity mit mehr Möglichkeiten](https://kubernetes.io/docs/concepts/scheduling-eviction/topology-spread-constraints/)
