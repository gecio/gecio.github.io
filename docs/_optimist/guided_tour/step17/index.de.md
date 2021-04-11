---
title: "17: Das Netzwerk im Heat"
lang: de
permalink: /optimist/guided_tour/step17
nav_order: 1170
parent: Guided Tour
---

Schritt 17: Das Netzwerk im Heat
================================

Vorwort
-------

Im letzten Schritt war ein individueller Parameter das Ziel und in
diesem das komplette Netzwerk.

Das Template
------------

Um nicht bei Null zu starten, dient das Template aus dem vorigen Schritt
als Vorlage. 

Wichtig ist dabei, dass direkt ein neuer Parameter hinzufügt wird,
genauer die ID des öffentlichen Netzwerks:

```yaml
heat_template_version: 2014-10-16
 
parameters:
    key_name:
        type: string
    public_network_id:
        type: string
        default: provider 

resources:
    Instanz:
        type: OS::Nova::Server
        properties:
            key_name: { get_param: key_name }
            image: Ubuntu 16.04 Xenial Xerus - Latest
            flavor: m1.small
```

Netzwerk
--------

Nachdem dieser Parameter eingefügt wurde, ist es Zeit das Netzwerk in
das Template einzufügen.

Hierbei handelt es sich um eine weitere Ressource und wird unter dem
Punkt `resources` eingefügt.

Der zugehörige Typ lautet `OS::Neutron::Net`:

```yaml
heat_template_version: 2014-10-16
 
parameters:
    key_name:
        type: string
    public_network_id:
        type: string
        default: provider 

resources:
    Instanz:
        type: OS::Nova::Server
        properties:
            key_name: { get_param: key_name }
            image: Ubuntu 16.04 Xenial Xerus - Latest
            flavor: m1.small

    Netzwerk:
        type: OS::Neutron::Net
            properties:
            name: BeispielNetzwerk
```

Der Port
--------

Der nächste Schritt ist dann der Port, der Typ lautet dafür
`OS::Neutron::Port`.

Wichtig ist, dass der Port in das bestehende Netzwerk eingegliedert wird
und die Instanz dem Port zuzuordnen ist. 

Um dies zu erreichen, wird erneut ein get Befehl genutzt und statt dem
Parameter eine Ressource eingebunden:

```yaml
heat_template_version: 2014-10-16
 
parameters:
    key_name:
        type: string
    public_network_id:
        type: string
        default: provider 

resources:
    Instanz:
        type: OS::Nova::Server
        properties:
            key_name: { get_param: key_name }
            image: Ubuntu 16.04 Xenial Xerus - Latest
            flavor: m1.small

    Netzwerk:
        type: OS::Neutron::Net
        properties:
            name: BeispielNetzwerk

    Port:
        type: OS::Neutron::Port
        properties:
            network: { get_resource: Netzwerk }
```

Der Router
----------

Nachdem Netzwerk und Port, wird nun ein Router (Typ =
`OS::Neutron::Router`) in das Template eingebunden. 

Bei diesem Typ ist es wichtig, das öffentliche Netzwerk einzubinden:

```yaml
heat_template_version: 2014-10-16
 
parameters:
    key_name:
        type: string
    public_network_id:
        type: string
        default: provider 

resources:
    Instanz:
        type: OS::Nova::Server
        properties:
            key_name: { get_param: key_name }
            image: Ubuntu 16.04 Xenial Xerus - Latest
            flavor: m1.small

    Netzwerk:
        type: OS::Neutron::Net
            properties:
            name: BeispielNetzwerk
    Port:
        type: OS::Neutron::Port
        properties:
            network: { get_resource: Netzwerk }

    Router:
        type: OS::Neutron::Router
        properties:
            external_gateway_info: { "network": { get_param: public_network_id }
            name: BeispielRouter
```

Das Subnet
----------

Der vorletzte Schritt ist das Subnet (Typ = `OS::Neutron::Subnet` )

In selbigem werden eigene Nameserver eintragen, die Informationen des
Netzwerks eingebunden, die IP-Version sowie der IP-Adressraum festgelegt
und die verfügbaren IPs definiert:

```yaml
heat_template_version: 2014-10-16
 
parameters:
    key_name:
        type: string
    public_network_id:
        type: string
        default: provider 

resources:
    Instanz:
        type: OS::Nova::Server
        properties:
            key_name: { get_param: key_name }
            image: Ubuntu 16.04 Xenial Xerus - Latest
            flavor: m1.small

    Netzwerk:
        type: OS::Neutron::Net
            properties:
            name: BeispielNetzwerk
    Port:
        type: OS::Neutron::Port
        properties:
            network: { get_resource: Netzwerk }

    Router:
        type: OS::Neutron::Router
        properties:
            external_gateway_info: { "network": { get_param: public_network_id }
            name: BeispielRouter

    Subnet:
        type: OS::Neutron::Subnet
        properties:
            name: BeispielSubnet
            dns_nameservers:
                - 8.8.8.8
                - 8.8.4.4
            network: { get_resource: Netzwerk }
            ip_version: 4
            cidr: 10.0.0.0/24
            allocation_pools:
                - { start: 10.0.0.10, end: 10.0.0.250 }
```

Subnet Bridge
-------------

Im letzten Schritt wird eine Subnet Bridge (Typ =
`OS::Neutron::RouterInterface`) angelegt, also eine Brücke zwischen Router und
Subnet.

Auch gibt es hier eine weitere neue Komponente, genauer `depends_on`.

Damit können wir Resource erstellen lassen, die nur dann gebaut werden,
wenn es die referenzierte Resource auch gibt.  

In unserem Beispiel wird die Bridge zwischen Subnet und Router nur
gebaut, wenn es auch ein Subnet gibt.

```yaml
heat_template_version: 2014-10-16
 
parameters:
    key_name:
        type: string
    public_network_id:
        type: string
        default: provider 

resources:
    Instanz:
        type: OS::Nova::Server
        properties:
            key_name: { get_param: key_name }
            image: Ubuntu 16.04 Xenial Xerus - Latest
            flavor: m1.small

    Netzwerk:
        type: OS::Neutron::Net
            properties:
            name: BeispielNetzwerk
    Port:
        type: OS::Neutron::Port
        properties:
            network: { get_resource: Netzwerk }

    Router:
        type: OS::Neutron::Router
        properties:
            external_gateway_info: { "network": { get_param: public_network_id } }
            name: BeispielRouter

    Subnet:
        type: OS::Neutron::Subnet
        properties:
            name: BeispielSubnet
            dns_nameservers:
                - 8.8.8.8
                - 8.8.4.4
            network: { get_resource: Netzwerk }
            ip_version: 4
            cidr: 10.0.0.0/24
            allocation_pools:
                - { start: 10.0.0.10, end: 10.0.0.250 }

    Router_Subnet_Bridge:
        type: OS::Neutron::RouterInterface
        depends_on: Subnet
        properties:
            router: { get_resource: Router }
            subnet: { get_resource: Subnet }
```

Abschluss
---------

Nachdem nun das komplette Netzwerk eingerichtet wurde, wird im [nächsten
Schritt](schritt18.md) eine eigene Security Group erstellt und zusätzlich der Instanz
eine öffentliche IP-Adresse zugewiesen.
