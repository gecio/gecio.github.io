---
title: Openstack
lang: "de"
permalink: /edge/openstack
has_children: true
nav_order: 3000
---

# Openstack

Openstack ist die Software die genutzt wird für den Infrastructure as a Service Layer.
Alle VMs werden im Openstack erstellt, Openstack ist direkt mit CEPH verbunden worüber der Storage angeboten wird.

## Openstack - Horizon

Das Dashboard für Openstack ist Horizon.
![Openstack Overview](./openstack_overview.png)

### Horizon Login

Alle Administratoren erhalten bei der Übergabe einen Login.
![Openstack Login](./openstack_login.png)

### Horizon Instances

In der Instance Übersicht kann man die VMs ansehen die im ausgewählten Projekt laufen.

![Openstack Instances](./openstack_instances.png)

### Horizon Network

Im reiter Network kann man die Netzweke im aktuell Projekt sehen und die Provider Netzwerke.
Die Provider Netzwerke sind die Netzwerke welche die VMs zum Kunden Netzwerk verbinden.

![Openstack Network](./openstack_network.png)

Desweiteren kann man sich die Netzwerk Topologie ansehen, wie die VMs verbunden sind und wie die Netzwerke verbunden sind.

![Openstack Toplogy without VM](./openstack_network_topology_1.png)

![Openstack Toplogy with VM](./openstack_network_topology_2.png)

### Horizon New Network

Um ein neues Netzwerk anzulegen muss im Reiter "Network" auf "Create Network" geklickt werden.
Dann kann man den Namen auswählen.

![Openstack New Network](./openstack_new_network.png)
Und man muss ein Subnet erstellen.
![Openstack New Network define subnet](./openstack_new_subnet.png)

Um mit dem Netzwerk zu arbeiten oder das Netzwerk nach aussen zu verbinden muss ein Router erstellt werden.

![Openstack New Router](./openstack_new_router.png)

Der Router muss dann mit einem Netzwerk verbunden werden.
Das passiert durch die Erstellung eines Interfaces im Router, welches den Router dann mit einem Netzwerk verbindet.

![Openstack add Interface to Router](./openstack_new_router_-_add_interface.png)

Um zwei Netzwerke zu verbinden braucht der Router ein Interface in beiden Netzwerken

![Openstack Router with two interfaces](./openstack_new_router_-_interfaces.png)
