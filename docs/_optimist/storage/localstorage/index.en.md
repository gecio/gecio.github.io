---
title: Localstorage
lang: en
permalink: /optimist/storage/localstorage/
nav_order: 3200
parent: storage
has_children: true
---

# Compute localstorage for your instances

## What exactly is Compute Localstorage?

With Localstorage, the storage of your instances is located directly on the hypervisor (server). The localstorage feature (available through our l1 flavors) is intended for use with applications that require low latency.

## Data security and availability

Since your data is directly bound yb your instance on our local hypervisor storage, we recommend that you distribute this data via a HA concept, over the provided GEC Availability Zones. The hypervisors are subject to our compliance patch cycle where we must boot through the hypervisors one by one. This will be performed per Availability Zone, one server at a time, within a defined Maintainace Window.

**Standard Maintainance Windows**

| Window | Day | Time |
|:---|---|---:|
| 1 | Wednesday | 10:00 - 15:00 |
| 2 | Friday | 5:00 - 9:00 a.m. |

## OpenStack Features

OpenStack provides many ways to handle your instances, such as resizing, shelving, and snapshots. If you want to use l1 flavors for your instances please note the following:

_Resize:_ The resize option will be displayed, but it is technically not possible to resize an instance based on an l1 flavor. However, you can address this by doing a cluster setup (application based) with l1 flavors, running larger l1 flavors in parallel and rolling your data from the old l1 to the new l1 flavors.

_Shelving/Snapshoting:_ Both features are possible, but due to the larger disk size within l1 flavors we do not recommend this, as the associated upload will take much longer. In this case we recommend using your external backup solution.

## Deleting instances

When you delete your instance, our system will overwrite your data multiple times as outlined in our policy.

## Apendix: Flavor Benchmarks

To help you decide which flavor is right for you, we have created extensive benchmarks. These are the maximum (up to) values that can be achieved. The benchmarks were created on completely empty instances, i.e. no running process except FIO.

**Sequential Write**

| | l1.micro | l1.small | l1.medium | l1.large | l1.xlarge | l1.2xlarge |
|:---|---|---|---|---|---|---|
| Bandwitdh | ![](attachments/micro_bw_seqwrite.png) | ![](attachments/small_bw_randwrite.png) | ![](attachments/medium_bw_seqwrite.png) | ![](attachments/large_bw_seqwrite.png) | ![](attachments/xlarge_bw_seqwrite.png) | ![](attachments/2xlarge_bw_seqwrite.png) |
| IOPS | ![](attachments/micro_iops_seqwrite.png) | ![](attachments/small_iops_randwrite.png) | ![](attachments/medium_iops_seqwrite.png) | ![](attachments/large_iops_seqwrite.png) | ![](attachments/xlarge_iops_seqwrite.png) | ![](attachments/2xlarge_iops_seqwrite.png) |
| Latency | ![](attachments/micro_lat_seqwrite.png) | ![](attachments/small_lat_randwrite.png) | ![](attachments/medium_lat_seqwrite.png) | ![](attachments/large_lat_seqwrite.png) | ![](attachments/xlarge_lat_seqwrite.png) | ![](attachments/2xlarge_lat_seqwrite.png) |

**Sequencial Read**

| | l1.micro | l1.small | l1.medium | l1.large | l1.xlarge | l1.2xlarge |
|:---|---|---|---|---|---|---|
| Bandwitdh | ![](attachments/micro_bw_seqread.png) | ![](attachments/small_bw_randread.png) | ![](attachments/medium_bw_seqread.png) | ![](attachments/large_bw_seqread.png) | ![](attachments/xlarge_bw_seqread.png) | ![](attachments/2xlarge_bw_seqread.png) |
| IOPS | ![](attachments/micro_iops_seqread.png) | ![](attachments/small_iops_randread.png) | ![](attachments/medium_iops_seqread.png) | ![](attachments/large_iops_seqread.png) | ![](attachments/xlarge_iops_seqread.png) | ![](attachments/2xlarge_iops_seqread.png) |
| Latency | ![](attachments/micro_lat_seqread.png) | ![](attachments/small_lat_randread.png) | ![](attachments/medium_lat_seqread.png) | ![](attachments/large_lat_seqread.png) | ![](attachments/xlarge_lat_seqread.png) | ![](attachments/2xlarge_lat_seqread.png) |

**Random Write**

| | l1.micro | l1.small | l1.medium | l1.large | l1.xlarge | l1.2xlarge |
|:---|---|---|---|---|---|---|
| Bandwitdh | ![](attachments/micro_bw_randwrite.png) | ![](attachments/small_bw_randwrite.png) | ![](attachments/medium_bw_randwrite.png) | ![](attachments/large_bw_randwrite.png) | ![](attachments/xlarge_bw_randwrite.png) | ![](attachments/2xlarge_bw_randwrite.png) |
| IOPS | ![](attachments/micro_iops_randwrite.png) | ![](attachments/small_iops_randwrite.png) | ![](attachments/medium_iops_randwrite.png) | ![](attachments/large_iops_randwrite.png) | ![](attachments/xlarge_iops_randwrite.png) | ![](attachments/2xlarge_iops_randwrite.png) |
| Latency | ![](attachments/micro_lat_randwrite.png) | ![](attachments/small_lat_randwrite.png) | ![](attachments/medium_lat_randwrite.png) | ![](attachments/large_lat_randwrite.png) | ![](attachments/xlarge_lat_randwrite.png) | ![](attachments/2xlarge_lat_randwrite.png) |

**Random Read**

| | l1.micro | l1.small | l1.medium | l1.large | l1.xlarge | l1.2xlarge |
|:---|---|---|---|---|---|---|
| Bandwitdh | ![](attachments/micro_bw_randread.png) | ![](attachments/small_bw_randread.png) | ![](attachments/medium_bw_randread.png) | ![](attachments/large_bw_randread.png) | ![](attachments/xlarge_bw_randread.png) | ![](attachments/2xlarge_bw_randread.png) |
| IOPS | ![](attachments/micro_iops_randread.png) | ![](attachments/small_iops_randread.png) | ![](attachments/medium_iops_randread.png) | ![](attachments/large_iops_randread.png) | ![](attachments/xlarge_iops_randread.png) | ![](attachments/2xlarge_iops_randread.png) |
| Latency | ![](attachments/micro_lat_randread.png) | ![](attachments/small_lat_randread.png) | ![](attachments/medium_lat_randread.png) | ![](attachments/large_lat_randread.png) | ![](attachments/xlarge_lat_randread.png) | ![](attachments/2xlarge_lat_randread.png) |
