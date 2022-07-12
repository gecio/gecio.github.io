---
title: FAQ
lang: en
permalink: /optimist/faq/
nav_order: 8000
last_modified_date: 2021-03-02
---

# FAQ

## The command `openstack --help` shows the error "Could not load EntryPoint.parse"

In this case some components of the OpenstackClient are outdated. You can use the command below, to get an overview of which components need
to be updated:

```bash
openstack --debug --help
```

This will show you which components need to be updated, which you can do with the command below. (Replace `<PROJECT>` with the correct
project):

```bash
pip install python-<PROJECT>client -U
```

## How can I use [VRRP](https://en.wikipedia.org/wiki/Virtual_Router_Redundancy_Protocol)?

In order to use VRRP, you will need to add a rule to a security group that's associated with an actual VM. You can only add this via the
OpenStack client! An example of adding VRRP to a security group would be:

```bash
openstack security group rule create --remote-ip 10.0.0.0/24 --protocol vrrp --ethertype IPv4 --ingress  default
```

## Why am I charged for Floating IPs I am not using?

We have to charge for reserved Floating IPs, and most often the case is that you did not remove the Floating IP after deleting the VM that
used it.

To get an overview of your Floating IPs, you can use the Horizon Dashboard, where you can find the list a
_Project_ → _Network_ → _Floating-IPs_.

You can accomplish the same using the OpenStack client:

```bash
$ openstack floating ip list
+--------------------------------------+---------------------+------------------+--------------------------------------+--------------------------------------+----------------------------------+
| ID                                   | Floating IP Address | Fixed IP Address | Port                                 | Floating Network                     | Project                          |
+--------------------------------------+---------------------+------------------+--------------------------------------+--------------------------------------+----------------------------------+
| 84eca713-9ac1-42c3-baf6-860ba920a23c | 185.116.245.222     | 192.0.2.7        | a3097883-21cc-49fa-a060-bccc1678ece7 | 54258498-a513-47da-9369-1a644e4be692 | b15cde70d85749689e6568f973bb002  |
+--------------------------------------+---------------------+------------------+--------------------------------------+--------------------------------------+----------------------------------+
```

## How can I change the flavor of a virtual machine (instance resize)?

In the Optimist Stack you change the flavor of an instance -- the available RAM
and vCPUs -- using the following steps:

### Resizing via command line

With the name or UUID of the server you wish to resize, resize it using the openstack server resize command.

Specifying the desired new flavor and then the instance name or UUID:

```bash
openstack server resize --flavor FLAVOR SERVER
```

Resizing can take some time. During this time, the instance status will show as RESIZE.

When the resize completes, the instance status will be VERIFY_RESIZE. You can now confirm the resize to change the status to ACTIVE:

```bash
openstack server resize --confirm SERVER
```

### Resizing via the Optimist dashboard

On [Optimist Dashboard → Instances](https://dashboard.optimist.innovo.cloud/project/instances/) navigate to the instance to resize, then
choose: _Actions_ → \_Resize Flavor:.

The current flavor will be shown, use the "Select a new flavor" dropdown list to choose the new flavor and confirm with "Resize"

## Why are the logs of the compute instance in the optimist dashboard empty?

Due to maintenance work or load balancing of the OpenStack, the instance might have been migrated. In this case the log file gets cleared.
New messages will be logged to the file as they occur.

## Why do I get the error "Conflict (HTTP 409)" when creating a swift container?

Swift uses unique names across the entire OpenStack environment. The error message states that the selected name is already in use.

## How-To Mount Cinder Volumes to Instances by UUID

When attaching multiple Cinder Volumes to an instance, it is possible that the mount points will be shuffled on every reboot. Mounting the
volumes by UUID ensures the proper volumes are reattached to the correct mount points in the event the instance requires a power cycle.

Change the mountpoint in `/etc/fstab` to use the UUID after fetching the infos with `blkid` on eg:

```bash
# /boot was on /dev/sda2 during installation
/dev/disk/by-uuid/f6a0d6f3-b66c-bbe3-47ba-d264464cb5a2 /boot ext4    defaults        0       2
```

## Is it possible to have multiattached volumes on Cinder?

We do not support multiattached volumes on our instances as cluster-capable file systems are required for multi attach volumes in order to handle concurrent file system access.
Attempts to use multi-attached volumes without cluster-capable filesystems carries a high risk of data corruption, therefore this feature is not enabled on the Optimist platform.

## Why am I unable to create a snapshot of a running instance?

In order to provide consistent snapshots, the Optimist platform utilises the property [os_require_quiesce=yes](https://opendev.org/openstack/nova/commit/926e58a179ef373646164bea40dc46b1ebef4748).
This property allows `fsfreeze` to suspend and resume access on running instances and ensures that a stable image is maintained on disk.

Users must therefore choose one of the following approaches in order to create snapshots on running instances:

The first option is to take a snapshot of the running instance by installing and running the `qemu-guest-agent`. It can be installed and run as follows:

```bash
apt install qemu-guest-agent
systemctl start qemu-guest-agent
```

Once the qemu-guest-agent is running, the snapshot can be created.

Additionally, when [uploading your own images](https://docs.gec.io/optimist/specs/images/#uploading-your-own-images), we recommend that you include `--property hw_qemu_guest_agent=True` to install this upon creation of the new image.

The second option is to stop the running instance, create the snapshot, then finally restart the instance again. This can be done via the Horizon Dashboard or on the CLI as follows:

```bash
openstack server stop ExampleInstance
openstack server image create --name ExampleInstanceSnapshot ExampleInstance
openstack server start ExampleInstance
```
