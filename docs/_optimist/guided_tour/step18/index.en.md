---
title: "18: Our VM will be reachable via IPv4"
lang: en
permalink: /optimist/guided_tour/step18/
nav_order: 1180
parent: Guided Tour
---

Step 18: Our VM will be reachable via IPv4
==========================================

Start
-----

Now that our template defines the full network and can reach the internet,
we'll have to make it possible to reach the VM from the internet.

Floating-IP
-----------

We'll define a floating public IPv4 address, which is a resource with type
`OS::Neutron::FloatingIP`.

Please note that it's important to define the external network that this IP
will be assigned from and the port that this IP will lead to:

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
 
    Floating_IP:
        type: OS::Neutron::FloatingIP
        properties:
            floating_network: { get_param: public_network_id }
            port_id: { get_resource: Port }
```

Security Groups
---------------

If we would create a stack as defined above, the VM would start but it
wouldn't be reachable. As we've mentioned before, VMs will not receive
traffic without a security group explicitly allowing it.

So, of course, the logical next step is to create a resource with type
`OS::Neutron::SecurityGroup`.

We'll have to define the security group to use on the *Port* and in the
resource itself, we'll specify the rules themselves. These rules will consist
of the direction, port range, remote IP prefix and protool that these rules
want to allow.

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
            security_groups: { get_resource: Sec_SSH }
 
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
                - { direction: ingress, remote_ip_prefix: 0.0.0.0/0, port_range_min: 22, port_range_max: 22, protocol:tcp }
                - { direction: ingress, remote_ip_prefix: 0.0.0.0/0, protocol: icmp }
```

Conclusion
----------

We can now create a stack that contains a single reachable instance. 

In the next step, we will customize the instance using CloudConfig.
