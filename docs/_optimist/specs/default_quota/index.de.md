---
title: Default Quotas
lang: de
permalink: /optimist/specs/default_quota/
parent: Spezifikationen
nav_order: 9200
last_modified_date: 2021-04-19
---

OpenStack Default Quotas
========================

Im Optimist haben wir Standardwerte für den OpenStack Compute-Dienst, den OpenStack Block Storage-Dienst und den OpenStack Networking-Dienst definiert. Wir haben auch separate Quotas für den Octavia Loadbalancer-Dienst und die zugehörigen Komponenten. Diese Standardwerte sind unten aufgeführt.

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

Octavia Loadbalancers
----------------

|**Ressource**             |**Wert**             |
|:-------------------------|:--------------------|
| Load Balancers           | 100                 |
| Listeners                | 100                 |
| Pools                    | 100                 |
| Health Monitors          | 100                 |
| Members                  | 100                 |
