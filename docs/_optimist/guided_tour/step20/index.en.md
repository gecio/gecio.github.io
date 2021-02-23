---
title: Step 20 - Build multiple VMs via HEAT
lang: en
permalink: /optimist/guided_tour/step20
nav_order: 4000
parent: Guided Tour
---

Step 20: Build multiple VMs via HEAT
====================================

Start
-----

Previously, we've only created a single VM, now we're going to create
multiple VMs at the same time.

First Steps
-----------

To begin with, we'll spit the template into two parts. We're not doing
this for any reason except to show that it's possible.

It's best practice to break big setups up into multiple files.

First, we'll start with a simple template containing only the network
and the port.

```
heat_template_version: 2014-10-16

description: A simple template which deploys 3 VMs

resources:

    ExampleNet:
        type: OS::Neutron::Net
        properties:
            name: ExampleNet

    ExampleSubnet:
        type: OS::Neutron::Subnet
        properties:
            name: ExampleSubnet
            dns_nameservers:
                - 8.8.8.8
                - 8.8.4.4
            network: {get_resource: ExampleNet}
            ip_version: 4
            cidr: 10.0.0.0/24
            allocation_pools:
            - {start: 10.0.0.10, end: 10.0.0.250}
```

This is the basic structure for our stack, we'll save it as `groups.yaml.`

Now we'll create a template called *exampleserver.yaml,* we'll define
the VM here.Now we will create a new template `exampleserver.yaml` and
we will describe the vm here. 

Make sure that name and network\_id are not defined.

Be sure to use a valid value to fill `image:`. You can use the image name or ID.
You can get either of those by running `openstack image list`.

```
heat_template_version: 2014-10-16

description: a single server description

parameters:
  network_id:
    type: string
  server_name:
    type: string

resources:
  VM:
    type: OS::Nova::Server
    properties:
      user_data_format: RAW
      image: Ubuntu 16.04 Xenial Xerus - Latest
      flavor: m1.small
      name: { get_param: server_name }
      networks:
        - port: { get_resource: ExamplePort }

  ExamplePort:
    type: OS::Neutron::Port
    properties:
      network: { get_param: network_id }
```

We'll now change our *groups.yaml* and add a resource group where we'll add
the VMs with the required arguments.

```
heat_template_version: 2014-10-16

description: A simple template which deploys 3 VMs

resources:
 
    ExampleVM:
        type: OS::Heat::ResourceGroup
        depends_on: ExampleSubnet
        properties:
            count: 3
            resource_def:
                type: exampleserver.yaml
                properties:
                    network_id: { get_resource: ExampleNet}
                    server_name: ExampleVM_%index%

    ExampleNet:
        type: OS::Neutron::Net
        properties:
            name: ExampleNet

    ExampleSubnet:
        type: OS::Neutron::Subnet
        properties:
            name: ExampleSubnet
            dns_nameservers:
                - 8.8.8.8
                - 8.8.4.4
            network: {get_resource: ExampleNet}
            ip_version: 4
            cidr: 10.0.0.0/24
            allocation_pools:
            - {start: 10.0.0.10, end: 10.0.0.250}
```

Now that we've supplied all the data we can create our stack:

```
openstack stack create -t groups.yaml <Name of the stack>
```

Conclusion
----------

Congratulations, we went from creating a single VM via the web interface
all the way to creating full stacks with the OpenStack client!
