---
title: External Cloud Provider Migration (OCCM)
lang: en
permalink: /imke/clusterlifecycle/clustermigrations/externalcloudprovider
nav_order: 4350
parent: Cluster Lifecycle
---

It is now possible to migrate cluster that still uses the in-tree to the external cloud provider.

**This is required to be done before the upgrade to kubernetes v1.22 as the in-tree provider will be removed in that version.

You can verify the if you need to migrate in the cluster details page.

![migration needed](migration-needed.png)
![migration not needed](migration-not-needed.png)

# Migration Process

If you need special assistance, please contact the GEC Support.

## Step 1 start migration

Press the update button and ok.
![migration needed](migration-needed.png)

That will trigger the control plane update and migrate all PV/PVC to the new cinder CSI plugin.

**Your Loadblancer will get a new IP**

While the migration is running, all Neutron loadblancers will be replaced with a new Octavia loadblancer, as [required in our Optimist platform](/optimist/migration_loadbalancer/).

At this state you will have two lbs, the old neutron loadblancer with the old IP and a new octavia loadblancer with a new IP.

## Step 2 fix/update IP/DNS setting

You can either update your DNS settings now or move the old FIP from the neutron to the new octavia loadblancer.

Changing DNS has no downtime and should be prepared with a reduction of the TTL before starting the migration.

Change the FIP will lead to a small downtime, while detatching it from the neutron and reattachin to the octavia loadblancer.

> __NOTE:__
> Do not recreate your nodes before finishing this step! That will lead to a downtime, as the old loadblancer will not be updated

## Step 3 rotate machinedeployment

rotate your machines to finisch the migration.
![worker rotation](rotate-nodes.png)

> __Note:__
> the old neutron loadblancer will stop working at that point

## Step 4 cleanup

Delete the old neutron loadblancer.


# Learn more
* [Neutron deprecation in our Optimist OpenStack Platform](/optimist/migration_loadbalancer/)
* [Neutron LBaaS Deprecation in the OpenStack Wiki](https://wiki.openstack.org/wiki/Neutron/LBaaS/Deprecation)
