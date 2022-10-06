---
title: Port Forwarding auf Floating IPs
lang: "de"
permalink: /optimist/networking/port_forwarding/
parent: Netzwerke
nav_order: 2100
---

Port Forwarding auf Floating IPs
==================================

Floating IP Port Forwarding erlaubt die Weiterleitung eines beliebigen TCP/UDP/anderen Protokoll-Ports einer Floating IP-Adresse an einen TCP/UDP/anderen Protokoll-Port, der mit einer festen IP-Adresse eines Neutron-Ports verbunden ist.

Port Forwarding auf einer Floating IP erstellen
---------------

Um ein Port forwarding auf eine Floating IP anzuwenden, sind die folgenden Informationen erforderlich:

* Die zu verwendende interne IP-Adresse
* Die UUID des Ports, der mit der Floating IP assoziiert werden soll
* Die Portnummer des Netzwerkports der festen IPv4-Adresse
* Die externe Portnummer der Floating IP-Adresse
* Das spezifische Protokoll, das bei der Port-Weiterleitung zu verwenden ist (in diesem Beispiel TCP)
* Die Floating IP, auf der dieser Port freigeschalten werden soll. (in diesem Beispiel 185.116.244.141)

Das folgende Beispiel zeigt die Erstellung eines Port Forwarding auf einer Floating IP unter Verwendung der erforderlichen Optionen:

```bash
$ openstack floating ip port forwarding create \
    --internal-ip-address 10.0.0.14 \
    --port 12c29300-0f8a-4c54-a9dc-bee4c12c6ad2 \
    --internal-protocol-port 80 \
    --external-protocol-port 8080 \
    --protocol tcp 185.116.244.141
```

Anzeigen der Port Forwarding Einstellungen bestimmter Floating IPs
---------------

Innerhalb eines Projekts kann eine Liste der Port Forwarding-Regeln, die für eine bestimmte Floating IP gelten, mit dem folgenden Befehl abgerufen werden:

`$ openstack floating ip port forwarding list 185.116.244.141`

Der obige Befehl kann weiter gefiltert werden, indem vor der Floating IP die Flags `--sort-column`, `--port`, `--external-protcol-port` und/oder `--protocol` verwendet werden.

Anzeigen der Details einer port forwarding-Regel
---------------

Um Details zu einer bestimmten Port Forwarding-Regel anzuzeigen, kann der folgende Befehl verwendet werden:

`$ openstack floating ip port forwarding show <floating-ip> <port-forwarding-id>`

Ändern von Floating IP Port Forwarding-Eigenschaften
---------------

Wenn eine Port Forwarding-Konfiguration auf einer Floating IP bereits mit `$ openstack floating ip port forwarding create` erstellt wurde, können Änderungen an der bestehenden Konfiguration mit `$ openstack floating ip port forwarding set ...` vorgenommen werden.

Die folgenden Parameter eines Port Forwardings können geändert werden:

* `--port`: Die UUID des Ports
* `--internal-ip-address`: Die zum Zielport der Forwarding-Regel gehoerende feste interne IPv4-Adresse
* `--internal-protocol-port`: Die interne TCP/UDP/etc. Portnummer auf die die Floating IPs Port Forwarding-Regel weiterleitet
* `--external-protocol-port`: Die TCP/UDP/etc. Portnummer der Floating-IP-Adresse des Port Forwardings
* `--protocol`: Das IP-Protokoll, das in der Floating IP Port Forwarding-Regel verwendet wird (TCP/UDP/andere)
* `--description`: Text zur Beschreibung der Verwendung der Port Forwarding-Konfiguration

Die Konfiguration jeder der oben genannten Parameter kann mit einer Variation des folgenden Befehls geändert werden:

```bash
$ openstack floating ip port forwarding set \
    --port <port> \
    --internal-ip-address <internal-ip-address> \
    --internal-protocol-port <port-number> \
    --extern-protocol-port <port-number> \
    --protocol <protocol> \
    --description <description> \
    <Floating-ip> <port-forwarding-id>
```

Löschen der Port Forwarding-Konfiguration zu einer Floating IP
---------------

Um eine Port Forwarding-Regel von einer Floating IP zu entfernen, benötigen wir die folgenden Informationen:

* Die Floating IP dessen Port Forwarding-Regel entfernt werden soll
* Die Port Forwarding ID (Diese ID wird bei der Erstellung erzeugt und kann mit dem Befehl `$ openstack Floating ip port forwarding list ...` angezeigt werden)

Mit dem folgenden Befehl lässt sich die Konfiguration für ein Floating IP Port Forwarding löschen:

`$ openstack floating ip port forwarding delete <Floating-ip> <port-forwarding-id>`

test
