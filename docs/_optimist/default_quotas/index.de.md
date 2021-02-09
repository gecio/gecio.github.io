---
title: OpenStack Default Quotas
lang: de
permalink: /optimist/default_quotas/
nav_order: 10
---

OpenStack Default Quotas
========================

Im Optimist haben wir Standardwerte für den OpenStack Compute-Dienst, den OpenStack Block Storage-Dienst und den OpenStack Networking-Dienst definiert. Diese Standardwerte sind unten aufgeführt.

Compute Standardwerte
---------------------

|**Ressource**             |**Wert**             |
|:-------------------------|:--------------------|
| Cores                    |        256          |
| Fixed IPs                |        Unlimited    |
| Floating IPs             |        15           |
| Injected File Size       |        10240 (MB)   |
| Injected Files           |        100          |
| Instances                |        100          |
| Key Pairs                |        100          |
| Properties               |        128          |
| Ram                      |        524288 (MB)  |
| Server Groups            |        10           |
| Server Group Members     |        10           |

Block Storage Standardwerte
---------------------------

|**Ressource**             |**Wert**             |
|:-------------------------|:--------------------|
| Backups                  |        100          |
| Backup Gigabytes         |        10000 (MB)   |
| Gigabytes                |        10000 (MB)   |
| Per-volume-gigabytes     |        Unlimited    |
| Snapshots                |        100          |
| Volumes                  |        100          |

Network Standardwerte
---------------------

|**Ressource**             |**Wert**             |
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