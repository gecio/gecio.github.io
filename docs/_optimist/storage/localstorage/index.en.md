---
title: Localstorage
lang: en
permalink: /optimist/storage/localstorage/
nav_order: 3200
parent: Storage
has_children: false
---

# Compute localstorage for your instances

## What exactly is Compute Localstorage?

With Localstorage, the storage of your instances is located directly on the hypervisor (server). The localstorage feature (available through our l1 flavors) is intended for use with applications that require low latency.

## Data security and availability

Since your data is directly bound yb your instance on our local hypervisor storage, we recommend that you distribute this data via a HA concept, over the provided GEC Availability Zones. The hypervisors are subject to our compliance patch cycle where we must boot through the hypervisors one by one. This will be performed per Availability Zone, one server at a time, within a defined Maintainace Window.

## Standard Maintainance Windows

| Window | Availability Zone | Months | Weekly | Day | Time |
|:---|---|---|---|---|---:|
| 1 | ES1 | January, April, July, October | 25% of the hypervisors | Wednesday | 10 a.m - 3 p.m |
| 2 | IX1 | February, May, August, November | 25% of the hypervisors | Wednesday | 10 a.m - 3 p.m |
| 3 | IX2 | March, June, September, December | 25% of the hypervisors | Wednesday | 10 a.m - 3 p.m |

## OpenStack Features

OpenStack provides many ways to handle your instances, such as resizing, shelving, and snapshots. If you want to use l1 flavors for your instances please note the following:

_Resize:_ The resize option will be displayed, but it is technically not possible to resize an instance based on an l1 flavor. However, you can address this by doing a cluster setup (application based) with l1 flavors, running larger l1 flavors in parallel and rolling your data from the old l1 to the new l1 flavors.

_Shelving/Snapshotting:_ Both features are possible, but due to the larger disk size within l1 flavors we do not recommend this, as the associated upload will take much longer. In this case we recommend using your external backup solution.

## Deleting instances

When you delete your instance, our system will overwrite your data multiple times as outlined in our policy.
