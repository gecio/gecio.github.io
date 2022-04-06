---
title: CNI Updates
lang: en
permalink: /imke/clusterlifecycle/upgradingcni/
nav_order: 4250
parent: Cluster Lifecycle
---

# Update the CNI

On the detail page of the cluster you can see if an update of the CNI is available.  
If you see a green arrow on the CNI plugin box, you can update. Click the plugin box to start.

![Step 1](cni_update_details.png)

It will show you the available versions, to start the upgrade just klick the `Change CNI Version` button.

![Step 2](cni_update_popup.png)

The update process starts in the background. While updating the worker nodes will experience some drop packages while each node is update.
The process updates one worker at a time, so deployments with more the 1 replica should not experience an outage.

The update normally completes without issues and the cluster is working afterwards.
In cases wehre the there are network problems at the end of the update (all pods in the canal daemonset are recreated), the first to possible solutions would be:

1. Restart the pods one more time after the update. You can do this via the kubectl client: `kubectl rollout restart daemonset --namespace kube-system canal`
2. A rolling recreation of all worker nodes. Can be done in the UI.

CNI update only supports updates to the next minor version. If you see more than one version in the dropdown, you need to update one by one.

![Dropdown](cni_update_dropdown.png)