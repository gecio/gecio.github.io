---
title: etcd Backups and Restore
lang: en
permalink: /gks/clusterbackups/etcdbackups/
nav_order: 5200
parent: Cluster Backups
---

# Etcd Backups and Restore

GKS supports a simple etcd backup and restore feature, which is enabled by default for all clusters.
The default backup configuration schedule is created with a performed backup of @every 20m interval and a retention of 20 backups.

However, it’s possible to create additional backup configuration if needed.

> **Note:**
> Before starting keep in mind that this is *only* an etcd backup and restore. The only thing that is restored is the etcd state, not PVC volumes with application data or similar.

## Etcd Backups

### Creating Etcd Backup Schedules

etcd backups and restores are resources bounded to a project. You can manage them in the Project view.
![Etcd Backups](../images/etcdbck01.png)

To create a new backup schedule, you need to click on the `Add Automatic Backup` button. You have a choice of preset daily, weekly, or monthly backups, or you can create a backup with a custom interval and keep time:
![Etcd Backups Configuration](../images/etcdbck02.png)
![Add new Backup](../images/etcdbck03.png)

To see all available backups, click on a backup you are interested in:
![Etcd Backups Details](../images/etcdbck04.png)

You will see the list of the completed backups:
![Etcd Backups Details](../images/etcdbck05.png)

### Creating Etcd Backup Snapshots

You can also create one-time backup snapshots. They are set up similarly to the automatic ones, with the difference that they do not have a schedule or keep count set.
![Etcd Snapshot](../images/etcdbck06.png)
![Etcd Snapshot Details](../images/etcdbck07.png)

## Restoring Etcd Backups

If you want to restore a backup, you need to click on the restore from backup icon in the UI.

### Restoring Etcd Backups from Schedule

![Restore backup from Schedule](../images/etcdbck08.png)

### Restoring Etcd Backups from Snapshot

![Restore backup from Snapshot](../images/etcdbck09.png)
![Restore etcd backup for cluster](../images/etcdbck10.png)

After that the cluster gets paused, etcd gets deleted, and then it is recreated from the backup. When it’s done, the cluster is unpaused again.
In the meantime, an EtcdRestore object is created in the project, and you can observe its progress in the EtcdRestore list.

In the cluster view, you may notice that your cluster is in a Restoring state, and you can not interact with it until it is done.
![Cluster Restoring](../images/etcdbck11.png)

When it’s done, the cluster gets unpaused and unblocked, so you can use it.
The Etcd Restore goes into a Completed state.
![Etcd Restore Completed](../images/etcdbck12.png)
