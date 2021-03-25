---
title: Node Deployment
lang: en
permalink: /imke/nodedeployments/nodedeployment/
nav_order: 5100
parent: Node Deployments
---
# Add Node Deployment

To add a new node deployment, use the `Add Node Deployment Button` in the right top corner.

![add_node_deployment](add_nodedep.png)

This brings up the `Add Node Deployment`-dialog, which has the same options as at cluster creation time:

![add_dialog](add_dialog.png)

After pressing `Add Node Deployment`:

![add_button](add_button.png)

the new nodes will be created. You can look at the progress in the node deployment details.

Click on the new node deployment:

![node_deployment_overview](node_deployment_overview.png)

and wait until all nodes are green.

![node_deployment_status](node_deployment_status.png)

# Delete Node Deployment

To delete a node deployment use the trash symbol in either the list:

![delete_from_list](delete_from_list.png)

or the details page:

![delete_from_details](delete_from_details.png)

# Rename Node Deployment

Node deployment can't be renamed. So we need to [create](#add-node-deployment) a second one and [delete](#delete-node-deployment) the old one.

But there is a Gotcha! Deleting a node deployment will delete all nodes at the same time. Depending on our replicas and number of nodes, that can lead to a downtime.

To mitigate this, you should reduce the replica of the node deployment step by step until it is 0 and then delete the node deployment.

The Pods will most likely rescheduled to the new host directly, but it is possible that some pod will end up on old nodes. This will lead to many rescheduled. If that is a problem, it is possible to first cordon all old nodes with `kubectl cordon <node name>` and then slowly reduce the replicas afterwards.
