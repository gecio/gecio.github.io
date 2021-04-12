---
title: Shared Networks
lang: en
permalink: /optimist/networking/shared_networks/
nav_order: 2200
parent: Networking
---

Shared Networks
===============

Motivation
----------

The question often arises of whether it is possible to share a network between two OpenStack projects. In this document, we will explain what is needed and how this can be implemented.

Share Network
-------------

To share a network with a project, please send us an e-mail to support@innovo-cloud.de with the following information:

- Name and ID of the network to be shared
- Name and ID of the project in which the network should be visible

Important information about shared networks
-------------------------------------------

When accessing a shared network, there are limitations that must be considered. One limitation is that no remote security groups can be used.
Additionally, there is no insight into ports and IP addresses from the other project.
Therefore, one can also specify any specific IP addresses for new ports in a subnet (in the shared network), as it would be possible to find IPs that are already in use.

In order to make use of the shared network, there is the option to create a new port. This then receives a random IP address to use, for example, to add a router through this port.
This is not possible in the Horizon dashboard; we need to use the [OpenStackClient](https://docs.openstack.org/python-openstackclient/latest/).

Please ensure that no spaces or special characters are used in names, as using these can lead to problems.

First, we create the port and specify the shared network there:

```
$ openstack port create --network <ID of shared Networks> <Name of Ports>
```

Now, for example, a router can be created and then mapped to the newly created port:

```
##Creation of the router
$ openstack router create <Name of Router>

##Assigning a port to the router
$ openstack router add port <Name of Router> <Name of Port>
```


Network Topology Project 1
--------------------------

![](attachments/SharedNetwork1.png)

The network "shared" is shared from project 1 to project 2. The service "Example" is available in this network and runs on an instance there.

Network Topology Project 2
--------------------------

![](attachments/SharedNetwork2.png)

The network "shared" is also visible in project 2 and was attached to the router "router2". In addition, we have the network "network" from which the services in the network "shared" should be accessed.

You have to make sure that the corresponding route is set in the subnet "host route" configuration of the "shared" network in project 1 in order to enable the correct return transport of the packets.

In our example the following route is required: `10.0.1.0/24,10.0.0.1`
