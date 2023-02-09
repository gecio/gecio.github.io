---
title: Mehrere Availability-Zones
lang: "de"
permalink: /gks/machinedeployments/multipleavailibilityzones/
nav_order: 6600
parent: Machine Deployments
---
<!-- LTeX:  language=de-DE -->

# Mehrere Availability-Zones

## Einleitung

Ein *machine deployment* ist immer an eine Availability-Zone (im
Weiteren AZ abgekürzt) gebunden, da eine Maschine zu jedem Zeitpunkt
immer nur in einer Availability-Zone existieren kann und *machine
deployments* generell nicht aus Maschinen verschiedener
Availability-Zones bestehen. Dies beschränkt den nutzbaren
persistenten Speicher für Pods auf die jeweilige Availability-Zone in
der der Pod das erste mal ge-schedule'd wurde.


## Setups in einer Availability-Zone

Im Normalfall hat ein Kubernetes Cluster zum Erstellungszeitpunkt nur
ein *machine deployment*, welches in genau einer Availability-Zone
liegt und somit keinerlei Probleme bereitet. Wenn ein Pod auf einer
Maschine dieses *machine deployments* laufen soll und persistenten
Speicher braucht, fordert er einfach Speicher aus dieser Availability-Zone
an. Sollte der Pod von seiner Maschine verdrängt werden, wählt
Kubernetes automatisch eine passende Maschine mit hinreichend
Ressourcen aus und fährt den Pod auf seinem neuen Ziel wieder hoch.
Der dabei an die alte Maschine gebundene persistente Speicher wird
nahtlos an der neuen Maschine zur Verfügung gestellt und der Pod
kann wie erwartet normal hochfahren.


## Setups in mehreren Availability-Zones

### Setups beschränkt auf einer von mehreren Availability-Zones

Man kann mehr als ein *machine deployment* haben. Wenn man sich für
ein Zweites (oder N-tes) entscheidet kann man wählen, in welcher
Availability-Zone dieses neue *machine deployment* liegen soll.
Es ist möglich das neue *machine deployment* in dieselbe Availability-Zone
zu stellen wie das Erste oder es in eine der anderen AZ (dies
ist abhängig vom geplanten Verwendungszweck; beides kann sinnvoll sein).

Existieren zwei *machine deployments* in zwei verschiedenen Availability-Zones
so kann es passieren dass ein Pod von seiner Maschine in AZ A verdrängt
wird und der Kubernetes Scheduler einen neuen Pod in der AZ B einplant
und laufen lassen will. Der Pod wird jedoch im Status *pending* verbleiben
und in der Beschreibung einen Fehler hinterlassen dass es nicht möglich
ist die Voraussetzungen für das Hochfahren dieses Pods zu erfüllen.

Der Grund hierfür liegt im bereits oben beschriebenen Verhalten dass
der persistente Speicher ebenfalls an eine Availability-Zone gebunden
ist und nicht automatisch migriert werden kann.

Eine Lösung ist es Kubernetes beizubringen diese Art von Pod (Applikation)
explizit nur in einer bestimmten Availability-Zone zu platzieren. Dies kann
sowohl mit einem Deployment, StatefulSet oder Daemonset erreicht werden.

Dieses Kubernetes Feature heißt *affinity* und kann Pods sowohl
an (oder gegen) seine eigenen oder andere Pods als auch gegen Nodes
verwendet werden.

Hier ein Beispiel für ein Deployment welches in der AZ ES1 konfiguriert
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
(Die drei aufeinander folgenden Punkte symbolisieren dass hier Zeilen
 aus Gründen der einfacheren Lesbarkeit unterschlagen wurden. Das
 Deployment wird ohne diese unterschlagenen Zeilen nicht funktionieren!)


### Setups verteilt über mehrere Availability-Zones

Um die Verfügbarkeit der eigenen Applikation zu erhöhen ist es gängig,
diese in zwei verschiedenen Availability-Zones zu platzieren, um den
Verlust einer ganzen Availability-Zone mitigieren zu können (natürlich
muss die Applikation dies auch unterstützen aber das setzen wir hier
mal als gegeben voraus).

Um dies zu Erreichen muss man Kubernetes so konfigurieren dass es
zwei Repliken einer Applikation immer in zwei verschiedenen
Availability-Zones hochfährt. Zusätzlich muss die jeweilige Applikation
in derselben AZ bleiben in der der persistente Speicher liegt.

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
(Wieder symbolisieren die drei aufeinander folgenden Punkte dass hier Zeilen
 aus Gründen der einfacheren Lesbarkeit unterschlagen wurden. Das
 StatefulSet wird ohne diese unterschlagenen Zeilen nicht funktionieren!)

Um mehr über die Möglichkeiten des Platzierens von Pods in Kubernetes
zu lernen folgen sie bitte den unten stehenden Links.


## Weiterführende Themen

* [einfache Affinity](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/)
* [Affinity mit mehr Möglichkeiten](https://kubernetes.io/docs/concepts/scheduling-eviction/topology-spread-constraints/)
