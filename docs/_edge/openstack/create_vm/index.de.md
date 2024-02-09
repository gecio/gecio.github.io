---
title: VM Erstellung
lang: "de"
permalink: /edge/openstack/create_vm
has_children: false
nav_order: 3100
parent: Openstack
---

# Erstellung einer VM

Zum erstellen einer neue Virtuele Machine im Openstack muss man sich im Openstack Dashboard einloggen.
Diesen Zugriff haben nur Administratoren.

Nach dem einloggen, können neue VMs im Reiter Instances erstellt werden.

![Launch Instance - Detail](./openstack_instances.png)
Man klickt auf "Launch Instance" um mit einen Wizard durch die Erstellung der VM durch geführt zu werden.

## Details

Hier kann der Name ausgesucht werden, welche die VM erhält.
Desweiteren kann die Anzahl der VMs geändert werden, der standard ist eine.
![Launch Instance - Detail](./openstack_instances_2.png)

## Source

Hier kann ausgewählt werden ob die Instance aus einem existierenden Volume erstellt werden soll.

![Launch Instance - Source](./openstack_instances_3.png)

Oder ob die VM aus einem Image erstellt werden soll.

![Launch Instance - Source mit Images](./openstack_instances_4.png)

Desweiteren kann eingestellt werden ob für die VM ein neues Volume erstellt werden soll und ob dieses Volume nach der Löschung der VM mit gelöscht werden soll.

## Flavour

Hier kann eingestellt werden welche größe die VM haben soll und ob diese gegebenfalls mit einer Grafikkarte oder TSN Karte ausgestattet werden soll.

![Launch Instance - Flavour](./openstack_instances_5.png)

## Network

Hier kann das Netzwerk ausgewählt werden mit welcher die VM bei der Erstellung verbunden ist.

![Launch Instance - Networks](./openstack_instances_6.png)

## Key Pair

Hier kann der SSH Key ausgewählt werden, mit welchen man sich über SSH verbinden kann.
Desweiteren können hier neue SSH keys importiert werden.

![Launch Instance - Key Pair](./openstack_instances_7.png)
