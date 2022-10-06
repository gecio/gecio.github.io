---
title: "08: Delete the first VM"
lang: "en"
permalink: /optimist/guided_tour/step08/
nav_order: 1080
parent: Guided Tour
---

# Step 8: Delete the first VM

## Start

In the previous step, you created a VM. In this step, you will learn how to delete it so that you can reuse its resources.

First of all, you need to acquire the name or the ID of the VM.

If you only have few VMs, you can use the name. Since names are not unique, we
strongly recommend using the ID.

Let's get a list of all your VMs:

```bash
$ openstack server list
+--------------------------------------+--------------+--------+---------------------------------------------------+------------------------------------+
| ID                                   | Name         | Status | Networks                                          | Image Name                         |
+--------------------------------------+--------------+--------+---------------------------------------------------+------------------------------------+
| 801b3021-0c00-4566-881e-b50d47152e63 | singleserver | ACTIVE | single_internal_network=10.0.0.12, 185.116.245.39 | Ubuntu 16.04 Xenial Xerus - Latest |
+--------------------------------------+--------------+--------+---------------------------------------------------+------------------------------------+
```

This returns a list of all your VMs. You can find the ID in column *ID*, and the name in column *Name*.

Now that you have this information, you can delete it:

```bash
openstack server delete 801b3021-0c00-4566-881e-b50d47152e63
```

If you re-run the command, it should return nothing at all:

```bash
$ openstack server list

$
```

## Conclusion

You have now learned how to delete instances. Additionally, with the command `openstack server list` you can get an overview of all instances.

In [Step 9](/optimist/guided_tour/step09/), we will focus on security groups.
