---
title: "17: Das Netzwerk in Heat"
lang: "de"
permalink: /optimist/guided_tour/step17/
nav_order: 1170
parent: Guided Tour
---

# Schritt 17: Das Netzwerk in Heat

## Einführung

Im letzten Schritt haben wir ein einfaches Template mit einem individuellen Parameter erstellt. In diesem Schritt fügen wir das Netzwerk hinzu.

## Das Template

Wir verwenden wieder das Template aus dem vorigen Schritt
als Vorlage und fügen einen neuen Parameter hinzu.
In unserem Fall ist das die ID des öffentlichen Netzwerks. Wir nennen sie `public_network_id`, und definieren als default `provider`:

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

## Netzwerk

Nachdem dieser Parameter eingefügt wurde, fügen wir das Netzwerk in
das Template hinzu.

Hierbei handelt es sich um eine weitere Ressource. Diese wird unter `resources` eingefügt.

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

## Der Port

Nun fügen wir den Port hinzu. Der Typ dafür lautet:
`OS::Neutron::Port`.

Wichtig ist, dass der Port in das bestehende Netzwerk eingegliedert wird und die Instanz dem Port zugeordnet wird.

Dazu nutzen wir erneut einen `get` Befehl und binden anstelle des
Parameters eine Ressource ein:

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

## Der Router

Nachdem wir Netzwerk und Port hinzugefügt haben, binden wir einen Router (Typ =
`OS::Neutron::Router`) in das Template ein.

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

## Das Subnet

Im vorletzten Schritt definieren wir das Subnet (Typ = `OS::Neutron::Subnet` ).

In diesem tragen wir eigene Nameserver ein, binden die Netzwerk-Informationen ein, legen die IP-Version und den IP-Adressraum fest und definieren die verfügbaren IPs:

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

## Subnet Bridge

Im letzten Schritt legen wir eine Subnet Bridge (Typ =
`OS::Neutron::RouterInterface`) an, also eine Brücke zwischen Router und
Subnet.

Auch gibt es hier eine weitere neue Komponente, `depends_on`.

Damit können wir Resourcen erstellen lassen, die nur dann gebaut werden,
wenn es die referenzierte Resource auch gibt.  

In unserem Beispiel wird die Bridge zwischen Subnet und Router nur
gebaut, wenn es ein Subnet gibt.

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

## Zusammenfassung

Wir haben nun das komplette Netzwerk eingerichtet. Im nächsten
Schritt erstellen wir eine eigene Security Group und weisen der Instanz
eine öffentliche IP-Adresse zu.
