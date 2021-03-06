---
title: "08: Delete the first VM"
lang: en
permalink: /optimist/guided_tour/step08/
nav_order: 1080
parent: Guided Tour
---

Step 8: Delete the first VM
===========================

Preface
-------

Previously, in Step 7: The first VM, we created a VM, in this step, we will delete it so that we can reuse its resources.

Start
-----

First of all, we need to acquire the name or the ID of the VM.

If we only have few VMs, we can use the name but as names aren't unique, it's
strongly recommended that we use the ID.

Let's get a list of all our VMs:

```bash
$ openstack server list
+--------------------------------------+--------------+--------+---------------------------------------------------+------------------------------------+
| ID                                   | Name         | Status | Networks                                          | Image Name                         |
+--------------------------------------+--------------+--------+---------------------------------------------------+------------------------------------+
| 801b3021-0c00-4566-881e-b50d47152e63 | singleserver | ACTIVE | single_internal_network=10.0.0.12, 185.116.245.39 | Ubuntu 16.04 Xenial Xerus - Latest |
+--------------------------------------+--------------+--------+---------------------------------------------------+------------------------------------+
```

This returns a list of all our VMs, the ID is in the column "ID" and the name is in the column "Name".

Now that we have this information, let's delete it:

```bash
$ openstack server delete 801b3021-0c00-4566-881e-b50d47152e63
```

If we ask for a new list of our VMs, it should return nothing at all:

```bash
$ openstack server list

$
```

Conclusion
----------

We've now learned how to delete Instances. Additionally, with the command `openstack server list` an overview of all instances could be obtained.

In Step 9: The first security group, we build on our previous experience and focus on the topic of security groups. 
