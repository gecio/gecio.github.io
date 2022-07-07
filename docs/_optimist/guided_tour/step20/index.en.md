---
title: "20: Build multiple VMs with HEAT"
lang: en
permalink: /optimist/guided_tour/step20/
nav_order: 1200
parent: Guided Tour
---

# Step 20: Build multiple VMs with HEAT

## Start

Previous steps outlined how to create a single VM. The next step is to create
multiple VMs at the same time.

In this step, two VMs that share a common network will be created.

## First Steps

To begin with, split the template into two parts. It is best practice to break larger setups up into multiple files.

First, start with a simple template which contains only the network and the port.

```yaml
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

This is the basic structure for your stack, the file can be saved under the name `groups.yaml.`

Create a new template `exampleserver.yaml` and define the VM here.

Make sure that `name` and `network_id` are **not** defined.

Use a valid value to fill `image:`. You can use the image name or ID.
Exact image names / IDs can be obtained by running `openstack image list`.
ß

```yaml
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

You can now modify your `groups.yaml` and add a resource group where you add the VMs with the required arguments.
After the file has been updated, it can be saved as `exampleserver.yaml`

The next step is to integrate the second template created as a resource group. The number of instances, the names, etc. can also be specified here:

```yaml
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

Now that you have supplied all data, you can create your stack:

```bash
openstack stack create -t groups.yaml <Name of the stack>
```

## Conclusion

Congratulations, you went from creating a single VM with the web interface to creating full stacks with the OpenStack client. Several instances can now be rolled out at the same time using a template, a good starting point for OpenStack administration.
