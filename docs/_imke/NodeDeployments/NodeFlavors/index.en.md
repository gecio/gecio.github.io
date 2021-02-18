---
title: Choosing Node Flavors / Sizes
lang: en
permalink: /imke/nodedeployments/nodeflavors
nav_order: 1
parent: Node Deployments
---

During the setup of new nodes in a cluster you can select a respective "flavor" for that node.

![Flavor-Select](flavor-select.png?resize=600,65)

Flavors determine the amount of processing cores ("CPUs"), Memory ("GB RAM") and disk space ("GB Disk") of the respective node.

![Flavors](flavors.png?resize=600,500)

A good practice for choosing the right flavor for your project is cycling through the following steps:

1. Start by selecting a small flavor.
2. Evaluate the performance of the desired use case.
3. Pick a larger flavor in case of any issues.

## Change Flavors

Changing the flavor of a node at any point in time is as easy as following these steps:

1. Navigate to the desired cluster.

    ![Clusters](clusters.png?resize=1500,300)

1. Click the Edit-Icon on the node for which you would like to select another flavor. The Icon becomes visible when hovering over the node with your mouse.

    ![Node-Selection](node-selection.png?resize=1500,700)

1. Select the desired flavor and click the "Edit Node Deployment"-button when you're finished.

    ![Edit-Node](edit-node.png?resize=600,700)

1. Soon after performing these steps, a confirmation message pops up and your node flavor has been changed successfully.

    ![Success-Message](success-message.png?resize=600,700)
