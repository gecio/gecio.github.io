---
title: Openstack
lang: "en"
permalink: /edge/openstack/
has_children: true
nav_order: 3000
---

# Openstack

OpenStack is the software used for the Infrastructure as a Service layer.
All VMs are created in OpenStack, which is directly connected to CEPH for providing storage.

## Openstack - Horizon

The Dashboard for Openstack is called Horizon.
![Openstack Overview](./openstack_overview.png)

### Horizon Login

All Admins gain login credentials to the Openstack Dashboard after Handover
![Openstack Login](./openstack_login.png)

### Horizon Instances

In the instance overview, you can view the VMs running in the selected project.

![Openstack Instances](./openstack_instances.png)

### Horizon Network

In the "Network" tab, you can see the networks in the current project, including the provider networks. The provider networks are the networks that connect the VMs to the customer network.
![Openstack Network](./openstack_network.png)

Furthermore, you can view the network topology, showing how the VMs are connected and how the networks are interconnected.

![Openstack Toplogy without VM](./openstack_network_topology_1.png)

![Openstack Toplogy with VM](./openstack_network_topology_2.png)

### Horizon New Network

To create a new network, click on "Create Network" in the "Network" tab.
Then, you can choose the name.

![Openstack New Network](./openstack_new_network.png)

And you need to create a subnet.
![Openstack New Network define subnet](./openstack_new_subnet.png)

To work with the network or connect the network externally, a router must be created.

![Openstack New Router](./openstack_new_router.png)

The router must then be connected to a network.
This is done by creating an interface on the router, which then connects the router to a network.

![Openstack add Interface to Router](./openstack_new_router_-_add_interface.png)

To connect two networks, the router needs an interface in both networks.

![Openstack Router with two interfaces](./openstack_new_router_-_interfaces.png)
