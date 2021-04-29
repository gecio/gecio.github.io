---
title: "17: The network in Heat"
lang: en
permalink: /optimist/guided_tour/step17
nav_order: 1170
parent: Guided Tour
---

Step 17: The network in Heat
============================

Preface
-------

Now that we have a simple template with a parameter, we'll add the network.

The template
------------

We'll continue using the template we previously created.

First, we'll add a new parameter, the id of the external network and name it
*public\_network\_id,* we'll also define a default *provider*:

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

Network 
--------- 

Next, we'll add the network.

Like the Vm, the network is a `resource`, so we'll add it to that block.

The type for network resources is *`OS::Neutron::Net`*

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

The port
--------

Now that we've defined a network we can add the port, which is a resource with
type *`OS::Neutron::Port`*.

To make sure that sure that this port is used by our VM, we add the *networks*
property to it and define a *port* property that then uses the *get\_resource*
function to link it to the *Port*.

Furthermore, we want to link the port to the network by adding a *network*
property that also uses the *get\_resource* function to link it to the
*Netzwerk*.

By now, our template will look like this:

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
            networks:
                - port: {get_resource: Port }
    
    Netzwerk:
        type: OS::Neutron::Net
        properties:
            name: BeispielNetzwerk
 
    Port:
        type: OS::Neutron::Port
        properties:
            network: { get_resource: Netzwerk }
```

The router 
------------

Our network will need a router, so now we'll add a *Router* resource, with the
type *`OS::Neutron::Router`. *

We will use our parameter to define the external network it will use:

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
            networks:
                - port: {get_resource: Port }
    
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

The subnet
------------

Now we want to define a subnet for our network, this is the *Subnet* resource
with type *`OS::Neutron::Subnet.`*

It's in the subnet that we'll define IP information like nameserver(s), the
IP version, the IP range and other IP related settings:

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
            networks:
                - port: {get_resource: Port }
    
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

Subnet bridge
-------------

Finally, we'll define a subnet bridge with the type
*`OS::Neutron::RouterInterface`*, this will associate the subnet and the router
so that VMs in that subnet will use the router.

We wil also define the *depends\_on* property, which makes sure that the subnet
bridge will only be created if *Subnet* is available:

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
            networks:
                - port: {get_resource: Port }
    
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
 
    Router_Subnet_Bridge:
        type: OS::Neutron::RouterInterface
        depends_on: Subnet
        properties:
            router: { get_resource: Router }
            subnet: { get_resource: Subnet } 
```

Conclusion
----------

We have now defined the full network, if this stack is now created, it
will create a VM and all the components needed to give it connectivity. The next step is to assign a public IP address to the instance.
