---
title: OpenStack Default Quotas
lang: en
permalink: /optimist/default_quotas/
nav_order: 10
---

OpenStack Default Quotas
========================

In Optimist we have defined default quotas for the OpenStack Compute service, the OpenStack Block Storage service, and the OpenStack Networking service. These default values are listed below.

Compute Settings
----------------

|**Field**                 |**Value**            |
|:-------------------------|:--------------------|
| Cores                    |        256          |
| Fixed IPs                |        Unlimited    |
| Floating IPs             |        15           |
| Injected File Size       |        10240        |
| Injected Files           |        100          |
| Instances                |        100          |
| Key Pairs                |        100          |
| Properties               |        128          |
| Ram                      |        524288       |
| Server Groups            |        10           |
| Server Group Members     |        10           |

Block Storage settings
----------------------

|**Field**                 |**Value**            |
|:-------------------------|:--------------------|
| Backups                  |        100          |
| Backup Gigabytes         |        10000        |
| Gigabytes                |        10000        |
| Per-volume-gigabytes     |        Unlimited    |
| Snapshots                |        100          |
| Volumes                  |        100          |

Network settings
----------------

|**Field**                 |**Value**            |
|:-------------------------|:--------------------|
| Floating IPs             |        15           |
| Secgroup Rules           |        1000         |
| Secgroups                |        100          |
| Networks                 |        100          |
| Subnets                  |        200          |
| Ports                    |        500          |
| Routers                  |        50           |
| RBAC Policies            |        100          |
| Subnetpools              |        Unlimited    |
| Health Monitors          |        Unlimited    |