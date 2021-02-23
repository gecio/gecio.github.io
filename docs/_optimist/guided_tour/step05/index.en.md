---
title: Step 5 - An overview of the most important commands of the OpenStackClient
lang: en
permalink: /optimist/guided_tour/step05
nav_order: 2500
parent: Guided Tour
---

Step 5: An overview of the most important commands of the OpenStackClient
=========================================================================

Start
-----

Now that we've installed the OpenStack client in step 4, we will learn
some of the more important commands for it.

To get more details about a specific subcommand, you can append the
`--help` flag to it.

To list all commands, you can use `--help` without any other
information:

```
openstack --help
```

Server
------

With the command `openstack server` it's possible to create,
administrate or delete a VM.

Here is a list of some common commands:

-   `openstack server add`
    This commands will add parameters
    (Fixed IP, Floating IP, Security group, Volume) to a VM.
-   `openstack server create`
    This command creates a VM.
-   `openstack server delete`
    This command deletes a VM.
-   `openstack sever list`
    This command shows a list of all VMs.
-   `openstack server remove`
    This command will remove parameters (Fixed IP, Floating IP, Security
    group, Volume) from a VM.
-   `openstack server show`
    This command shows all important information about the specified VM.

Stack
-----

With the command `openstack stack` you are able to administrate
complete stacks, like `openstack server` for instances.

Here is a list for some common commands:

-   `openstack stack create`
    This command creates a new stack.
-   `openstack stack list`
    This command lists all stacks.
-   `openstack stack show`
    This command shows all important information about the specified
    stack.
-   `openstack stack delete`
    This command deletes the specified stack.

 

Security Group
--------------

Security Groups are used, to allow or deny incoming and outgoing network
traffic based on ip-adresses and ports for VMs.

You can also manage security groups in the OpenStackClient.

Here are some common commands:

-   `openstack security group create`
    Creates a new security group
-   `openstack security group delete`
    Deletes a security group
-   `openstack security group list`
    List of all security groups
-   `openstack security group show`
    Shows all important information about a security group
-   `openstack security group rule create`
    Adds a rule for a security group
-   `openstack security group rule delete`
    Deletes a rule in a security group

Network
-------

To create VMs, they need a network, here are some common network commands:

-   `openstack network create`
    Creates a new network
-   `openstack nerwork list`
    List of all networks
-   `openstack network show`
    Shows all important information about a network
-   `openstack network delete`
    Deletes a network

Router
------

For the VMs on your network to reach the internet, you need a router,
here are some common router commands.

-   `openstack router create`
    Creates a new router
-   `openstack router delete`
    Deletes a router
-   `openstack router add port`
    Adds a port to a router
-   `openstack router add subnet`
    Adds a subnet to a router

Subnet
------

To use a virtual router correctly, we will need a subnet, which can be
administrated with `openstack subnet` and here are some common commands:

-   `openstack subnet create`
    Creates a new subnet
-   `openstack subnet delete`
    Deletes a subnet
-   `openstack subnet show`
    Shows all infomation about a subnet

Port
----

Ports connect your VMs to your network, here are some common commands:

-   `openstack port create`
    Create a new port
-   `openstack port delete`
    Deletes a port
-   `openstack port show`
    Shows all infomation about a port

Volume
------

Volumes are persistent storage locations, they will show up as a disk on your
VM, here are some common commands:

-   `openstack volume create`
    Creates a new Volume
-   `openstack volume delete`
    Deletes a volume
-   `openstack volume show`
    Shows all infomation about a volume

Conclusion
----------

Now we know some common openstack commands, and have a better overview
of the system.
