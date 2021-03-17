---
title: Geteilte Netzwerke
lang: de
permalink: /optimist/shared_networks/
nav_order: 30
---

Geteilte Netzwerke
==================

Motivation
----------

Es kommt oft die Frage auf, ob es möglich ist ein Netzwerk zwischen zwei Projekten im OpenStack zu teilen.

Netzwerk teilen
---------------

Um ein Netzwerk mit einem Projekt zu teilen, schreiben Sie uns bitte eine E-Mail an support@innovo-cloud.de mit folgenden Angaben:

- Name und ID des Netzwerks, welches geteilt werden soll
- Name und ID des Projekts, in welchem das Netzwerk sichtbar sein soll

Wichtige Informationen zum geteilten Netzwerk
---------------------------------------------

Wenn man auf ein geteiltes Netzwerk zugreift, gibt es Einschränkungen, die beachtet werden müssen.
Eine Einschränkung ist, dass keine Remote Security-Groups benutzt werden können.
Auch gibt es keinen Einblick in Ports und IP Adressen vom anderen Projekt gibt.
Daher kann man auch keine konkrete IP Adressen für neue Ports in einem Subnetz (im geteilten Netzwerk) angeben, da es so möglich wäre, IPs zu finden die bereits genutzt werden.

Damit man das geteilte Netzwerk sinnvoll nutzen kann, gibt es die Möglichkeit einen neuen Port zu erstellen.
Dieser erhält dann eine beliebige IP-Adresse und kann weiter genutzt werden, um zum Beispiel einen Router über diesen Port hinzuzufügen.
Im Dashboard (Horizon) ist dies nicht möglich und der OpenStackClient wird benötigt.

Bitte achten Sie darauf bei den Bezeichnungen keine Leer- und/oder Sonderzeichen zu nutzen, da die Nutzung selbiger zu Problem führen kann.
Zuerst erstellen wir den Port und geben dort das geteilte Netzwerk an:

```
$ openstack port create --network <ID des geteilten Netzwerks> <Name des Ports>
```

Jetzt kann zum Beispiel ein Router erstellt und dann dem neu erstellten Port zugeordnet werden:

```
##Erstellung des Routers
$ openstack router create <Name des Routers>

##Port dem Router zuordnen
$ openstack router add port <Name des Routers> <Name des Ports>
```


Netzwerk Topology Projekt 1
--------------------------
![](attachments/SharedNetwork1.png)

Das Netzwerk "shared" aus dem Projekt 1 wird mit Projekt 2 geteilt. In diesem Netzwerk steht der Service "Example" zur Verfügung der dort auf einer Instanz läuft.

Netzwerk Topology Projekt 2
--------------------------

![](attachments/SharedNetwork2.png)

Das Netzwerk "shared" ist auch in Projekt 2 sichtbar und wurde dort an den Router "router2" angehangen.
Zusätzlich existiert dort das Netzwerk "network", aus dem auf die Services in dem Netzwerk "shared" zugegriffen wird.

Dabei muss berücksichtigt werden, das im Subnet des "shared" Networks in Projekt 1 die entsprechende Route unter dem Eintrag "Host Routes" gesetzt wird, um einen korrekten Rücktransport der Pakete zu ermöglichen.

Im unserem Beispiel ist die folgende Route notwendig: `10.0.1.0/24,10.0.0.1`

