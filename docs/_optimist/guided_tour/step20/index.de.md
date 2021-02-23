---
title: Schritt 20 - Mehrere Instanzen gleichzeitig erstellen
lang: de
permalink: /optimist/guided_tour/step20
nav_order: 4000
parent: Guided Tour
---

Schritt 20: Mehrere Instanzen gleichzeitig erstellen
====================================================

Vorwort
-------

Nachdem im Schritt 15 eine Instanz inklusive aller wichtigen
Einstellungen angelegt wurde, ist der nächste Schritt, mehr als eine
Instanz per Template zu starten.

In diesem Schritt, werden zwei Instanzen erstellt, die ein gemeinsames
Netzwerk nutzen.

Start
-----

Neben den beiden Instanzen wird auch das Template aufgeteilt und in zwei
Dateien erstellt.

Dies hat verschiedene Teile und den Start macht ein simples Template,
welches nur ein Net und ein Subnet enthält:

```
heat_template_version: 2014-10-16

description: Ein simples Template welches 2 Instanzen erstellt

resources:

    BeispielNet:
        type: OS::Neutron::Net
        properties:
            name: BeispielNet

    BeispielSubnet:
        type: OS::Neutron::Subnet
        properties:
            name: BeispielSubnet
            dns_nameservers:
                - 8.8.8.8
                - 8.8.4.4
            network: {get_resource: BeispielNet}
            ip_version: 4
            cidr: 10.0.0.0/24
            allocation_pools:
                - {start: 10.0.0.10, end: 10.0.0.250}
```

Dies stellt das Grundgerüst des Stacks dar und wird vorerst als
`Gruppen.yaml` gespeichert.

Die Instanzen selber werden in einer zweiten Datei `BeispielServer.yaml`
beschrieben, welche dem gleichen Aufbau wie in den vorigen Schritten
folgt.

Um `image:` zu füllen kann wahlweise der Image Name oder die Image-ID benutzt werden. 
Eine korrekte Image-ID bzw. einen korrekten Namen erhält man mit `openstack image list`.

Es ist wichtig, dass kein Server-Namen definiert wird und
`network_id` auch keinen Eintrag erfährt:

```
heat_template_version: 2014-10-16

description: Ein einzelner Server der durch eine Ressourcen Gruppe verwendet wird

parameters:
    network_id:
        type: string
    server_name:
        type: string

resources:

    Instanz:
        type: OS::Nova::Server
        properties:
            user_data_format: RAW
            image: Ubuntu 16.04 Xenial Xerus - Latest
            flavor: m1.small
            name: { get_param: server_name }
            networks:
                - port: { get_resource: BeispielPort }

    BeispielPort:
        type: OS::Neutron::Port
        properties:
            network: { get_param: network_id }
```

Nachdem die Datei fertiggestellt wurde, wird diese wie oben beschrieben
als `BeispielServer.yaml` gespeichert.

Um weiter fortzufahren, wird die Arbeit am ursprünglichen Template
(`Gruppen.yaml`) fortgesetzt.

Hier gilt es nun, das zweite erstellte Template als RessourceGroup
einzubinden.

Auch ist so direkt die Möglichkeit gegeben, die Anzahl der Instanzen,
die Namen etc. anzugeben:

```
heat_template_version: 2014-10-16

description: Ein simples Template welches 2 Instanzen erstellt

resources:
 
    BeispielInstanzen:
        type: OS::Heat::ResourceGroup
        depends_on: BeispielSubnet
        properties:
            count: 2
            resource_def:
                type: BeispielServer.yaml
                properties:
                    network_id: { get_resource: BeispielNet }
                    server_name: BeispielInstanz_%index%

    BeispielNet:
        type: OS::Neutron::Net
        properties:
            name: BeispielNet

    BeispielSubnet:
        type: OS::Neutron::Subnet
        properties:
            name: BeispielSubnet
            dns_nameservers:
                - 8.8.8.8
                - 8.8.4.4
            network: {get_resource: BeispielNet}
            ip_version: 4
            cidr: 10.0.0.0/24
            allocation_pools:
                - {start: 10.0.0.10, end: 10.0.0.250}
```

Nachdem die Arbeit an `Gruppen.yaml` abgeschlossen wurde, kann der so
erstellte Stack direkt mit dem OpenStackClient gestartet werden:

```
openstack stack create -t Gruppen.yaml <Name des Stacks>
```

Abschluss
---------

Nachdem am Anfang der Guided Tour noch Instanzen per Hand erstellt
wurden, können nun bereits mehrere Instanzen gleichzeitig per Template 
ausgerollt werden und stellen einen guten Startpunkt für die Administration 
von OpenStack dar.
