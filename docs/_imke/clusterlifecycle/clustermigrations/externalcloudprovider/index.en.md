---
title: External Cloud Provider Migration (OCCM)
lang: en
permalink: /imke/clusterlifecycle/clustermigrations/externalcloudprovider
nav_order: 4350
parent: Cluster Lifecycle
---

It is now possible to migrate cluster that still uses the in-tree to the external cloud provider.

**This need to be done latest before the upgrade to kubernetes 1.22.** as the in-tree provider will be removed in that version.

You can verify the if you need to migrate in the cluster details page.

[image]

# Migration Process

If you need special assistance, please contact the GEC Support.

## Step 1 start migration

Press the update button and ok.
[image]

That will trigger the control plane update and migrate al pv to the new cinder csi plugin.

**Loadblancer will get a new IP**

While migrating all old netron Loadblancer will be replaced with a new Octavia Loadblancer.

At this state you will have two lbs, the old neutron Loadblancer with the old IP and a new Octavia Loadblancer with a new IP.

## Step 2 fix/update IP/DNS setting

You can either update your DNS settings now or move the old FIP from the Neutrol to the new Octavia Loadblancer.

Changing DNS has no Downtime and should be prepared with a reduction of the TTL before starting the migration.

Change the FIP will lead to a small Downtime, while detatching it from the Neutrol and reattachin to the Octavia Loadblancer.

**NOTE: don't recreate your node before finisch this step! That will lead to a downtime, as the old Loadblancer will not be updated**

## Step 3 rotate machinedeployment

rotate your machines to finisch the migration.
[image]

**Note: the old neutron Loadblancer will stop working at that point**

## Step 4 cleanup

Delete the old neutron Loadblancer.
