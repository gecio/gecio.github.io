---
title: Create VM
lang: "en"
permalink: /edge/openstack/create_vm
has_children: false
nav_order: 3100
parent: Openstack
---

# Creation of a VM

To create a new Virtual Machine in OpenStack, you need to log in to the OpenStack Dashboard. This access is only available to administrators.

After logging in, new VMs can be created in the "Instances" tab.

![Launch Instance - Detail](./openstack_instances.png)

Click on "Launch Instance" to be guided through the creation of the VM using a wizard.

## Details

Here, the name for the VM can be chosen. Additionally, the number of VMs can be changed; the default is one.
![Launch Instance - Detail](./openstack_instances_2.png)

## Source

Here, you can choose whether the instance should be created from an existing volume.
![Launch Instance - Source](./openstack_instances_3.png)
Or whether the VM should be created from an image.
![Launch Instance - Source with Images](./openstack_instances_4.png)
Furthermore, you can set whether a new volume should be created for the VM and whether this volume should be deleted along with the VM.

## Flavor

Here, you can specify the size of the VM and whether it should be equipped with a graphics card or TSN card if necessary.
![Launch Instance - Flavour](./openstack_instances_5.png)

## Network

Here, you can select the network to which the VM will be connected during creation.
![Launch Instance - Networks](./openstack_instances_6.png)

## Key Pair

Here, the SSH key can be selected for connecting via SSH. Additionally, new SSH keys can be imported here.
![Launch Instance - Key Pair](./openstack_instances_7.png)
