---
title: Geteilte Netzwerke
lang: de
permalink: /optimist/networking/shared_networks/
nav_order: 2200
parent: Netzwerke
---

# Geteilte Netzwerke

## Motivation

Es kommt oft die Frage auf, ob es möglich sei, ein Netzwerk zwischen zwei Projekten in OpenStack zu teilen.

## Netzwerk teilen

### Zugriff auf beide Projekte ist vorhanden:

Damit das Netzwerk geteilt werden kann, wird folgendes benötigt:

* Der [OpenStackClient](https://docs.openstack.org/python-openstackclient/latest/)
* Die Projekt-ID, in welches das Netzwerk geteilt werden soll
* Die Netzwerk-ID des zu teilenden Netzwerks

Die Projekt-ID befindet sich in der Ausgabe unter "id", wenn der folgende Befehl benutzt wird:

```bash
openstack project show <Name des Projekts> -f value -c id
```

Als nächstes wird noch die Netzwerk-ID des zu teilenden Netzwerks benötigt. Diese befindet sich in der Ausgabe unter "id", wenn der folgende Befehl genutzt wird:

```bash
openstack network show <Name des Netzwerks> -f value -c id
```

Mit den erhaltenen IDs kann nun das Netzwerk in das entsprechende Projekt geteilt werden, dafür wird die rollenbasierte Zugriffskontrolle (RBAC) benutzt:

```bash
openstack network rbac create --type network --action access_as_shared --target-project <ID des Projekts> <ID des zu teilenden Netzwerks>
```

### Zugriff auf beide Projekte ist nicht vorhanden:

In diesem Fall kann das Netzwerk nur vom Support, nach voriger Freigabe des anderen Projekt Inhabers, geteilt werden.

Um ein Netzwerk mit einem Projekt zu teilen, schreiben Sie eine E-Mail an [support@gec.io](mailto:support@gec.io) mit folgenden Angaben:

* Name und ID des Netzwerks, welches geteilt werden soll
* Name und ID des Projekts, in welchem das Netzwerk sichtbar sein soll

## Wichtige Informationen zum geteilten Netzwerk

Wenn man auf ein geteiltes Netzwerk zugreift, gibt es Einschränkungen, die beachtet werden müssen.
Eine Einschränkung ist, dass keine Remote Security-Groups benutzt werden können.
Auch gibt es keinen Einblick in Ports und IP Adressen vom anderen Projekt.
Daher können auch keine konkrete IP Adressen für neue Ports in einem Subnetz (im geteilten Netzwerk) angegeben werden, da es sonst möglich wäre, IPs zu finden, die bereits genutzt werden.

Damit das geteilte Netzwerk sinnvoll genutzt werden kann, gibt es die Möglichkeit, einen neuen Port erstellen.
Dieser erhält dann eine beliebige IP-Adresse und kann weiter genutzt werden, um zum Beispiel einen Router über diesen Port hinzuzufügen.
Im Horizon Dashboard ist dies nicht möglich und der OpenStackClient wird benötigt.

Bitte achten Sie darauf, bei den Bezeichnungen keine Leer- und/oder Sonderzeichen zu nutzen, da die Verwendung zu Problemen führen kann.
Zuerst erstellen wir den Port und geben dort das geteilte Netzwerk an:

```bash
openstack port create --network <ID des geteilten Netzwerks> <Name des Ports>
```

Jetzt kann zum Beispiel ein Router erstellt und dem neu erstellten Port zugeordnet werden:

```bash
##Erstellung des Routers
$ openstack router create <Name des Routers>

##Port dem Router zuordnen
$ openstack router add port <Name des Routers> <Name des Ports>
```

## Netzwerk Topology Projekt 1

![](attachments/SharedNetwork1.png)

Das Netzwerk "shared" aus dem Projekt 1 wird mit Projekt 2 geteilt. In diesem Netzwerk steht der Service "Example" zur Verfügung, der dort auf einer Instanz läuft.

## Netzwerk Topology Projekt 2

![](attachments/SharedNetwork2.png)

Das Netzwerk "shared" ist auch in Projekt 2 sichtbar und wurde dort an den Router "router2" angehangen.
Zusätzlich existiert dort das Netzwerk "network", aus dem auf die Services in dem Netzwerk "shared" zugegriffen wird.

Dabei muss berücksichtigt werden, dass im Subnet des "shared" Networks in Projekt 1 die entsprechende Route unter dem Eintrag "Host Routes" gesetzt wird, um einen korrekten Rücktransport der Pakete zu ermöglichen.

Im unserem Beispiel ist die folgende Route notwendig: `10.0.1.0/24,10.0.0.1`
