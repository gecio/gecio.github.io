---
title: Machine Deployment
lang: en
permalink: /imke/machinedeployments/machinedeployment/
nav_order: 5100
parent: Machine Deployments
---
# Add Machine Deployment

To add a new Machine Deployment, use the `Add Machine Deployment` Button in the right top corner.

![add_node_deployment](add_nodedep.png)

This brings up the `Add Machine Deployment`-dialog, which has the same options as at cluster creation time:

![add_dialog](add_dialog.png)

After pressing `Add Machine Deployment`:

![add_button](add_button.png)

the new nodes will be created. You can look at the progress in the Machine Deployment details.

Click on the new Machine Deployment:

![node_deployment_overview](node_deployment_overview.png)

and wait until all nodes are green.

![node_deployment_status](node_deployment_status.png)

# Delete Machine Deployment

To delete a Machine Deployment use the trash symbol in either the list:

![delete_from_list](delete_from_list.png)

or the details page:

![delete_from_details](delete_from_details.png)

# Rename Machine Deployment

Machine Deployment can't be renamed. So we need to [create](#add-node-deployment) a second one and [delete](#delete-node-deployment) the old one.

But there is a Gotcha! Deleting a Machine Deployment will delete all nodes at the same time. Depending on our replicas and number of nodes, that can lead to a downtime.

To mitigate this, you should reduce the replica of the Machine Deployment step by step until it is 0 and then delete the Machine Deployment.

The Pods will most likely rescheduled to the new host directly, but it is possible that some pod will end up on old nodes. This will lead to many rescheduled. If that is a problem, it is possible to first cordon all old nodes with `kubectl cordon <node name>` and then slowly reduce the replicas afterwards.
