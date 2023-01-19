---
title: Shelving Instances
lang: "en"
permalink: /optimist/specs/shelving_instances/
parent: Specifications
nav_order: 9500
last_modified_date: 2022-08-03
---

# Shelving Instances

## Introduction

On the OpenStack platform, you have the ability to shelve an instance. Shelving instances allows you to stop an instance without having it consume resources.
A shelved instance, as well as its assigned resources (such as IP address etc), will be retained as a bootable instance.

This feature may be used as part of an instance life cycle process or to conserve resources.

{: .warning }
This does not apply to l1 (localstorage) flavors.
For more information please see [Storage → Localstorage](/optimist/storage/localstorage/#openstack-features).

## Shelve an Instance

Instances can be shelved as follows:
`$ openstack server shelve <server-id>`

## Unshelve an Instance

Instances can be unshelved with the following command:
`$ openstack server unshelve <server-id>`

## View Event List for Instances

You can view the shelving / unshelving history of any server by viewing the event list:

```bash
$ openstack server event list <server-id>
+------------------------------------------+--------------------------------------+--------+----------------------------+
| Request ID                               | Server ID                            | Action | Start Time                 |
+------------------------------------------+--------------------------------------+--------+----------------------------+
| req-8d593999-a09b-41a7-8916-1d7c28cd4dc0 | 846112be-d107-4c75-db75-a32eb47a78c5 | shelve | 2022-07-17T15:28:08.000000 |
| req-076969ee-15a4-470e-8913-051c6f9d4bd3 | 846112be-d107-4c75-db75-a32eb47a78c5 | create | 2022-07-19T16:15:22.000000 |
+------------------------------------------+--------------------------------------+--------+----------------------------+
```

## Why Use Shelving?

This feature is useful for archiving instances you are not currently using but do not want to delete. Shelving an instance allows you to you retain the instance data and resource allocations, but frees up the instance memory.

When you shelve an instance, the Compute service generates a snapshot image that captures the state of the instance, and uploads it to the Glance image library. If the instance is unshelved, it will be rebuilt using the snapshot.
The snapshot image will be deleted if the instance is later unshelved or deleted.

## Billing for Shelved Instances

From a billing perspective, only the root disk of the shelved instance continues to be billed. Once the instance is shelved, CPU and memory resources from the flavor of the instance cease to be billed, however, billing will automatically resume again after unshelving.

Shelving has no effect on the utilization of quotas in the project. Shelved resources do not release their quota to ensure sufficient resources for unshelving the instance in the project at all times.
