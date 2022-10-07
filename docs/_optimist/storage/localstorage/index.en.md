---
title: Localstorage
lang: "en"
permalink: /optimist/storage/localstorage/
nav_order: 3200
parent: Storage
has_children: false
---

# Compute localstorage for your instances

## What exactly is Compute Localstorage?

With Localstorage, the storage of your instances is located directly on the hypervisor (server). The localstorage feature (available through our l1 flavors) is intended for use with applications that require low latency.

## Data security and availability

Since your data is directly bound by your instance on our local hypervisor storage, we recommend that you distribute this data via a HA concept, over the provided Availability Zones.
The storage backend of the local storage instances is protected against the failure of individual storage media in the array, however, the resulting redundancy compared to the Ceph-based instances only exists within the hypervisor node providing the instance.
When replacing individual components following a hardware issue, there may be limited availability and performance for a short time until recovery.
The hypervisors are subject to our defined patch cycle where we must boot through the hypervisors one by one.
As the instances are using local storage, the maintenance work cannot be carried out uninterrupted, unlike flavors based on Ceph storage. Consequently, there is a regular maintenance window for l1 Flavors. Within each Availability Zone, one server after the other is updated and rebooted during the defined maintenance window. Within the maintenance window, running instances are shut down by our system and stopped after 10 minutes.

## Standard Maintainance Windows

| Interval | day | time |
|:---|---|---:|
| weekly | Wednesday | 10:00 a.m. - 4:00 p.m. |

## OpenStack Features

OpenStack provides many ways to handle your instances, such as resizing, shelving, and snapshots. If you want to use l1 flavors for your instances please note the following:

_Resize:_ The resize option will be displayed, but it is technically not possible to resize an instance based on an l1 flavor. However, you can address this by doing a cluster setup (application based) with l1 flavors, running larger l1 flavors in parallel and rolling your data from the old l1 to the new l1 flavors.

_Shelving/Snapshotting:_ Both features are possible, but due to the larger disk size within l1 flavors we do not recommend this, as the associated upload will take much longer. In this case we recommend using your external backup solution.

## Deleting instances

Deleting instances based on l1 flavors can take a long time due to the background process for deleting the data.
