---
title: Updating the OS of Worker Nodes
lang: en
permalink: /gks/machinedeployments/updatingnodeos/
nav_order: 5500
parent: Machine Deployments
---

## Flatcar

### Automatic Updates of Flatcar Worker Nodes

GKS provides the functionality to keep the operating system of Flatcar-based worker nodes up-to-date.
This feature automatically installs any updates released by the upstream vendor (Kinvolk) for Flatcar
on the worker nodes.

The auto-update feature uses [FLUO](https://github.com/kinvolk/flatcar-linux-update-operator), the Flatcar Linux Update Operator, in the background.
When a reboot is needed after updating the system, it will drain the node before rebooting it. It coordinates the reboots of multiple nodes in the cluster,
ensuring that only one node is rebooting at once.

Using the auto-update functionality is enabled by default. The following screenshot shows the creation of a machine deployment with auto-updater enabled:

![Create Machine Deployment with autoupdater](autoupdate_flatcar.png)

If you would like to take care of OS updates (and the reboots) yourself, you can disable the automatic updates of the worker nodes by selecting the `Disable auto-update` checkbox:

![Create Machine Deployment without autoupdater](autoupdate_flatcar_disable.png)

> We highly encourage our users to use the auto-update feature to keep your infrastructure safe.

### Checking the State of the Auto-Updater

To check if your nodes receive automatic OS-updates, click on the machine deployment:

![Open Machine Deployment](autoupdate_open_md.png)

Check if the `Disable auto-update` option has a green checkmark in front of it (auto-updater is off):

![Autoupdater off](autoupdate_disabled.png)

Or check if it's greyed out (auto-updater is on):

![Autoupdater on](autoupdate_enabled.png)

### Enabling/Disabling Auto-Updater on an Existing Machine Deployment

To change the status of the auto-updater, click on the "edit" button of the machine deployment.

![Edit Machine Deployment](autoupdate_edit_md.png)

(De)-select the checkbox accordingly.

![Modify MD Autoupdater](autoupdate_flatcar_modify.png)

After clicking on `Save Changes`, all worker nodes will perform a rolling update and reboot.

### Updating Flatcar Worker Nodes Manually

To update a Flatcar worker node manually, you require [access with SSH](/gks/machinedeployments/add_ssh_key/).

You can find out the actual installed OS version from `/etc/os-release`.

```bash
$ grep VERSION_ID /etc/os-release
VERSION_ID=2765.2.2
```

In the next step you'll unmask and start the systemd unit `update-engine`.

```bash
$ sudo systemctl unmask update-engine.service
Removed /etc/systemd/system/update-engine.service.
$ sudo systemctl start update-engine.service
```

Now you can check for available updates and install them.

```bash
$ sudo update_engine_client -check_for_update
$ sudo update_engine_client -status
```

The Update-Engine client now downloads the latest available release of Flatcar and adjusts
the boot order automatically so that the new release will be activated at the next boot.

![Update Engine](fc_update_engine.gif)

As soon as the status has changed from `UPDATE_STATUS_UPDATE_AVAILABLE` to `UPDATE_STATUS_DOWNLOADING`,
and then to `UPDATE_STATUS_UPDATED_NEED_REBOOT`, you can reboot the worker node and repeat the steps
for all nodes in the machine deployment.

````bash
$ sudo systemctl reboot
````

> We highly recommend using the auto-updater feature, in order to ensure the security and integrity of your infrastructure.

## Ubuntu

Ubuntu support in GKS was removed in July 2021. Read here how to update to supported Flatcar OS for existing machine deployments.

### Update to Flatcar

To update from Ubuntu to Flatcar, click on the edit button of the machine deployment.

![Edit Machine Deployment](update_to_flatcar_edit.png)

Then click on the flatcar logo.

![Change to Flatcar](update_to_flatcar.png)

It changed the Image and enabled the auto-update feature.

![Save Flatcar](update_to_flatcar_save.png)

The node is recreated, and your cluster is up-to-date.

## Summary

In this section you've learned the following:

* What the auto-update feature is
* How to enable and disable the auto-update feature for a Flatcar machine deployment
* How to apply the updates on a Flatcar worker node manually
* How to enable the automatic reboots of Ubuntu-based worker nodes

## Learn More

* [Auto-Updating Flatcar Container Linux](https://kinvolk.io/docs/lokomotive/git-main/how-to-guides/auto-update-flatcar/)
* [FLUO on Github](https://github.com/kinvolk/flatcar-linux-update-operator)
* [The Flatcar partitioning scheme](https://kinvolk.io/docs/flatcar-container-linux/latest/reference/developer-guides/sdk-disk-partitions/)
