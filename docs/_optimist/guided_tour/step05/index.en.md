---
title: "05: An overview of the most important commands of the OpenStackClient"
lang: en
permalink: /optimist/guided_tour/step05/
nav_order: 1050
parent: Guided Tour
---

# Step 5: An overview of the most important commands of the OpenStackClient

## Start

Now that you have installed the OpenStack client in [Step 4](/optimist/guided_tour/step4/), you will learn
some important commands.

To get more information about a specific subcommand, append the
`--help` flag to it.

To list all commands, you can use `--help` without any other
information:

```bash
openstack --help
```

## Server

With the command `openstack server` you can create,
administrate, or delete a VM.

Here is a list of some common commands:

- `openstack server add`
    Adds parameters
    (Fixed IP, Floating IP, Security group, Volume) to a VM
- `openstack server create`
    Creates a VM
- `openstack server delete`
    Deletes a VM
- `openstack sever list`
    Shows a list of all VMs
- `openstack server remove`
    Removes parameters (Fixed IP, Floating IP, Security
    group, Volume) from a VM
- `openstack server show`
    Shows all important information about the specified VM

## Stack

With the command `openstack stack` you are able to administrate
complete stacks, like `openstack server` for instances.

Here is a list for some common commands:

- `openstack stack create`
    Creates a new stack
- `openstack stack list`
    Shows a list of all stacks
- `openstack stack show`
    Shows all important information about the specified
    stack
- `openstack stack delete`
    Deletes the specified stack

## Security Group

Security Groups are used to allow or deny incoming and outgoing network
traffic based on IP adresses and ports for VMs.

You can also manage security groups in the OpenStackClient.

Here are some common commands:

- `openstack security group create`
    Creates a new security group.
- `openstack security group delete`
    Deletes a security group
- `openstack security group list`
    Shows a list of all security groups
- `openstack security group show`
    Shows all important information about a security group
- `openstack security group rule create`
    Adds a rule for a security group
- `openstack security group rule delete`
    Deletes a rule in a security group

## Network

To create VMs a network is required. Here are some common network commands:

- `openstack network create`
    Creates a new network
- `openstack nerwork list`
    Shows a list of all networks
- `openstack network show`
    Shows all important information about a network
- `openstack network delete`
    Deletes a network

## Router

For the VMs on your network to reach the internet, you need a router. Here are some common router commands.

- `openstack router create`
    Creates a new router
- `openstack router delete`
    Deletes a router
- `openstack router add port`
    Adds a port to a router
- `openstack router add subnet`
    Adds a subnet to a router

## Subnet

To use a virtual router correctly, you need a subnet that can be
administrated with `openstack subnet`. Here are some common commands:

- `openstack subnet create`
    Creates a new subnet
- `openstack subnet delete`
    Deletes a subnet
- `openstack subnet show`
    Shows all infomation about a subnet

## Port

Ports connect your VMs to your network. Here are some common commands:

- `openstack port create`
    Create a new port
- `openstack port delete`
    Deletes a port
- `openstack port show`
    Shows all infomation about a port

## Volume

Volumes are persistent storage locations, and will show up as a disk on your
VM. Here are some common commands:

- `openstack volume create`
    Creates a new Volume
- `openstack volume delete`
    Deletes a volume
- `openstack volume show`
    Shows all infomation about a volume

## Conclusion

Now you know some common OpenStack commands, and have a better overview
of the system.

The mentioned commands are required in the next steps and form the basis for the rest of the guided tour.

In [Step 6](/optimist/guided_tour/step6/), you will create and use your own SSH key pairs.
