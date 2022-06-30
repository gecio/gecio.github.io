---
title: "18: Unsere Instanz wird von außen über IPv4 erreichbar"
lang: "de"
permalink: /optimist/guided_tour/step18/
nav_order: 1180
parent: Guided Tour
---

# Schritt 18: Unsere Instanz wird von außen über IPv4 erreichbar

## Einführung

Im letzten Schritt haben wir das komplette Netzwerk eingerichtet. In diesem Schritt zeigen wir, wie
die Instanz von außen erreicht werden kann (u.a. über ICMP
und SSH Zugriff).

## Floating-IP

Zunächst definieren wir die öffentliche IP-Adresse, welche auch als Ressource
hinzugefügt wird. Der zugehörige Typ lautet `OS::Neutron::FloatingIP`.

Wichtig ist, dass wir der Floating IP den entsprechenden Port und das öffentliche Netz, das genutzt wird, mitteilen:

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

    Floating_IP:
        type: OS::Neutron::FloatingIP
        properties:
            floating_network: { get_param: public_network_id }
            port_id: { get_resource: Port }
```

## Security Groups

Wenn das oben geschriebene Template gestartet wird, würde die Instanz
zwar erstellt werden, kann aber aufgrund der voreingestellten Security Group nicht erreicht werden.

Um dies zu ändern, legen wir eine Security Group an mit dem Typ
= `OS::Neutron::SecurityGroup`.

Auch gibt es einige Besonderheiten zu beachten. So wird mit
Regeln (`rules`) gearbeitet, die dem Port zugewiesen werden müssen.

Dann kann die `direction` (Richtung des Traffics) in `ingress` (eingehend)
oder `egress` (ausgehend) eingeteilt werden. Außerdem können der entsprechende Port oder auch
die Range der Ports definiert und das Protokoll festgelegt werden.

Außerdem wird mit `remote_ip_prefix` festgelegt, wer die Instanz erreicht (falls nötig).

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

    Floating_IP:
        type: OS::Neutron::FloatingIP
        properties:
            floating_network: { get_param: public_network_id }
            port_id: { get_resource: Port }

    Sec_SSH:
        type: OS::Neutron::SecurityGroup
        properties:
            description: Diese Security Group erlaubt den eingehenden SSH-Traffic über Port22 und ICMP
            name: Ermöglicht SSH (Port22) und ICMP
            rules:
                - { direction: ingress, remote_ip_prefix: 0.0.0.0/0, port_range_min: 22, port_range_max: 22, protocol: tcp }
                - { direction: ingress, remote_ip_prefix: 0.0.0.0/0, protocol: icmp }
```

## Zusammenfassung

Die erstellte Instanz ist nun von außen erreichbar einschließlich.

Im nächsten Schritt passen wir die Instanz mit CloudConfig an.
