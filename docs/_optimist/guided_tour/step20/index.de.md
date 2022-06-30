---
title: "20: Mehrere Instanzen gleichzeitig erstellen"
lang: "de"
permalink: /optimist/guided_tour/step20/
nav_order: 1200
parent: Guided Tour
---

# Schritt 20: Mehrere Instanzen gleichzeitig erstellen

## Einführung

Nachdem Sie in [Schritt 15](/optimist/guided_tour/step15/) eine Instanz inklusive aller wichtigen
Einstellungen angelegt haben, lernen Sie in diesem Schritt, wie Sie mehr als eine Instanz pro Template starten können.

Dazu erstellen wir zwei Instanzen, die ein gemeinsames Netzwerk nutzen.

## Start

Neben den beiden Instanzen teilen wir auch das Template in zwei Dateien auf.

Wir starten mit einem einfachen Template,
welches nur ein Netzwerk und ein Subnet enthält:

```yaml
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

Dieses Template stellt das Grundgerüst des Stacks dar und wird vorerst als `Gruppen.yaml` gespeichert.

Die Instanzen selbst beschreiben wir in einer zweiten Datei `BeispielServer.yaml`. Sie haben den gleichen Aufbau wie in den vorigen Schritten.
Um `image:` zu füllen, kann wahlweise den Image Name oder die Image-ID benutzt werden.
Eine korrekte Image-ID bzw. einen korrekten Namen erhält man mit `openstack image list`.

Es ist wichtig, dass kein Server-Namen definiert wird und `network_id` keinen Eintrag hat:

```yaml
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

Nachdem wir die Datei fertiggestellt haben, wird diese wie oben beschrieben als `BeispielServer.yaml` gespeichert.

Als nächstes setzen wir die Arbeit am ursprünglichen Template (`Gruppen.yaml`) fort und binden das zweite erstellte Template als `RessourceGroup` ein.

Dadurch können wir direkt die Anzahl der Instanzen, die Namen, usw. angeben:

```yaml
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

Nachdem wir die Arbeit an `Gruppen.yaml` abgeschlossen haben, können wir den so
erstellten Stack direkt mit dem OpenStackClient starten:

```bash
openstack stack create -t Gruppen.yaml <Name des Stacks>
```

## Zusammenfassung

Nachdem Sie am Anfang der Guided Tour noch Instanzen per Hand erstellt
haben, können Sie nun bereits mehrere Instanzen gleichzeitig über ein Template
ausrollen. Dies stellt einen guten Ausgangspunkt für die Administration von OpenStack dar.
