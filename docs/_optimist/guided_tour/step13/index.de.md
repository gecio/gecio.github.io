---
title: "13: Der strukturierte Weg zu einer Instanz (mit Stacks)"
lang: "de"
permalink: /optimist/guided_tour/step13/
nav_order: 1130
parent: Guided Tour
---

# Schritt 13: Der strukturierte Weg zu einer Instanz (mit Stacks)

## Einführung

Nachdem wir die erste Instanz, inklusive einer Security Group und virtuellem
Netzwerk, in einem sehr aufwendigen Prozess per Hand angelegt haben,
zeigen wir in diesem Schritt eine Alternative auf.

Voraussetzung für die folgenden
Schritte ist die Installation des Paketes `python-heatclient` (siehe [Schritt 4](/optimist/guided_tour/step04/)).

## Installation

Anstatt einzelne Instanzen von Hand anzulegen, können beliebige
OpenStack Resourcen (z.B. Instanzen, Netzwerke, Router, Security Groups)
auch in einem definierten Verbund, einem sogenannten **Stack** (oder Heat
Stack) betrieben werden.

Dadurch werden sie logisch zusammengefaßt und können -- je nach Verwendungszweck -- einfach erstellt
und gelöscht werden.

Für die folgenden Schritte verwenden wir Heat-Templates als Grundlage.

Die bisherigen Schritte 9 bis 11, lassen sich einfach in einem Template zusammenfassen.

Hierzu gibt es ein Template unter [BeispielTemplates](https://github.com/innovocloud/openstack_examples/tree/master/heat/templates).

Dieses Template erstellt einen Stack, in dem eine Instanz, zwei Security Groups, ein virtuelles Netzwerk (inkl. Router, Port, Subnet) und eine Floating-IP enthalten sind.

Um den Stack erstellen zu können, müssen wir ins Verzeichnis des Templates wechseln und den folgenden Befehl ausführen:

```bash
$ openstack stack create -t SingleServer.yaml --parameter key_name=Beispiel SingleServer --wait
2017-12-08 13:13:43Z [SingleServer]: CREATE_IN_PROGRESS  Stack CREATE started
2017-12-08 13:13:44Z [SingleServer.router]: CREATE_IN_PROGRESS  state changed
2017-12-08 13:13:45Z [SingleServer.enable_traffic]: CREATE_IN_PROGRESS  state changed
2017-12-08 13:13:45Z [SingleServer.enable_traffic]: CREATE_COMPLETE  state changed
2017-12-08 13:13:46Z [SingleServer.internal_network_id]: CREATE_IN_PROGRESS  state changed
2017-12-08 13:13:46Z [SingleServer.router]: CREATE_COMPLETE  state changed
2017-12-08 13:13:46Z [SingleServer.internal_network_id]: CREATE_COMPLETE  state changed
2017-12-08 13:13:47Z [SingleServer.enable_ssh]: CREATE_IN_PROGRESS  state changed
2017-12-08 13:13:47Z [SingleServer.subnet]: CREATE_IN_PROGRESS  state changed
2017-12-08 13:13:47Z [SingleServer.enable_ssh]: CREATE_COMPLETE  state changed
2017-12-08 13:13:48Z [SingleServer.start-config]: CREATE_IN_PROGRESS  state changed
2017-12-08 13:13:48Z [SingleServer.subnet]: CREATE_COMPLETE  state changed
2017-12-08 13:13:49Z [SingleServer.router_subnet_bridge]: CREATE_IN_PROGRESS  state changed
2017-12-08 13:13:49Z [SingleServer.port]: CREATE_IN_PROGRESS  state changed
2017-12-08 13:13:50Z [SingleServer.start-config]: CREATE_COMPLETE  state changed
2017-12-08 13:13:50Z [SingleServer.port]: CREATE_COMPLETE  state changed
2017-12-08 13:13:50Z [SingleServer.host]: CREATE_IN_PROGRESS  state changed
2017-12-08 13:13:52Z [SingleServer.router_subnet_bridge]: CREATE_COMPLETE  state changed
2017-12-08 13:13:53Z [SingleServer.floating_ip]: CREATE_IN_PROGRESS  state changed
2017-12-08 13:13:55Z [SingleServer.floating_ip]: CREATE_COMPLETE  state changed
2017-12-08 13:14:05Z [SingleServer.host]: CREATE_COMPLETE  state changed
2017-12-08 13:14:06Z [SingleServer]: CREATE_COMPLETE  Stack CREATE completed successfully
+---------------------+-------------------------------------------------+
| Field               | Value                                           |
+---------------------+-------------------------------------------------+
| id                  | 0f5cdf0e-24cc-4292-a0bc-adf2e9f8618a            |
| stack_name          | SingleServer                                    |
| description         | A simple template to deploy your first instance |
| creation_time       | 2017-12-08T13:13:42Z                            |
| updated_time        | None                                            |
| stack_status        | CREATE_COMPLETE                                 |
| stack_status_reason | Stack CREATE completed successfully             |
+---------------------+-------------------------------------------------+
```

Der Befehl `openstack stack create` erstellt dabei den Stack. Mit
`-t SingleServer.yaml` wird festgelegt, dass das angegebene Template
verwendet werden soll.

Mit `--parameter key_name=BEISPIEL` wird  ein SSH-Schlüssel
angegeben und mit `SingleServer` der Name des Stacks festgelegt.

Der letzte Bestandteil `--wait` zeigt alle Zwischenschritte der Erstellung an (siehe das obere Bild).

Nach kurzer Zeit ist der Stack einschließlich der Instanz erstellt und kann mit SSH erreicht werden.

Die notwendige IP findet man mit dem folgenden Befehl heraus, wichtig
ist dabei, die korrekte ID zu nutzen:

```bash
$ openstack stack output show 0f5cdf0e-24cc-4292-a0bc-adf2e9f8618a instance_fip
+--------------+---------------------------------+
| Field        | Value                           |
+--------------+---------------------------------+
| description  | External IP address of instance |
| output_key   | instance_fip                    |
| output_value | 185.116.245.70                  |
+--------------+---------------------------------+
```

Nun stellen wir noch die Verbindung über SSH her:

```bash
$ ssh ubuntu@185.116.245.70
The authenticity of host '185.116.245.70 (185.116.245.70)' can't be established.
ECDSA key fingerprint is SHA256:kbSkm8eJA0748911RkbWK2/pBVQOjJBASD1oOOXalk.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '185.116.245.70' (ECDSA) to the list of known hosts.
Enter passphrase for key '/Users/ubuntu/.ssh/id_rsa':
```

## Zusammenfassung

Wir haben die bisherigen Schritte 9 bis 11 in einem Heat-Template zusammengefaßt.

In den folgenden Schritten gehen wir weiter auf Heat ein und zeigen weitere Beispiele.
