---
title: Creating a Cluster
lang: en
permalink: /gks/clusterlifecycle/creatingacluster/
nav_order: 4100
parent: Cluster Lifecycle
---
# Creating a Cluster

You can create a cluster in GKS with a couple of clicks.
Before you can do that you need a project.

If you do not have a project yet, [create a project](/gks/managingprojects/creatingaproject) first.

To create the cluster, click on `Create Cluster` in the top right corner:
![Create Cluster](projectview_addcluster.png)

The first page of the cluster creation procedure opens.
Choose the provider `openstack` and one of the three
datacenters. In this example, we pick `IX2`:
![Add Cluster Step 1](add_step1.png)

In the next step, you have to configure the cluster details. In the example,
we call our cluster `first-system` and select the desired Kubernetes version:
![Add Cluster Step 2](add_step2.png)

For occasional SSH access to worker nodes, you can optionally deploy an SSH Key.
To add an SSH Key, click on `Add SSH key`:
![Add Cluster Step 2.2](add_step2_2.png)

After that add the SSH Public Key and give it a memorable name:
![Add Cluster Step 2.3](add_step2_3.png)

To allow GKS to request the required resources from OpenStack, add your
OpenStack credentials. After that the content of `Project` is refreshed
automatically, and you can choose the OpenStack project where you want to run the cluster:
![Add Cluster Step 3.1](add_step3.png)
![Add Cluster Step 3.2](add_step3_2.png)

By adding the credentials and selecting the OpenStack project, you could proceed to the next
step. If you do so, a new and dedicated network, subnet, and security group will be automatically created for the cluster.

It is also possible to use an **existing** network to create the cluster. For this, you have to select
the network and the subnet from the dropdown menu, and attach them to a router.
You can create a router from the Optimist dashboard, or from the OpenStack command line.
For details on how to create and attach the router, refer to our [OpenStack documentation](/optimist/guided_tour/step10/).

![Add Cluster Network](create-cluster-network-exist.png)

In the next step, you define the number and the kind of virtual machines that will be initially available as worker nodes
in the cluster.

First, this so-called `Machine Deployment` needs a name. For your test cluster you use the random name generator:
![Add Cluster Step 4](add_step4.png)

Next, specify the `Replicas` (number of worker nodes in your Kubernetes cluster) and the `Flavor` (machine type), which
defines the amount of CPU and RAM for each worker node:
![Add Cluster Step 4.2](add_step4_2.png)

To finish, click on `Next`. After you verified all settings, click on `Create Cluster`:
![Add Cluster Step 5](add_step5.png)

Now the cluster is being created. To access the information, return to the cluster
view of the project and click your cluster's name:
![Add Cluster Step 6](add_step6.png)

This opens a page with all cluster details:
![Add Cluster Step 6.2](add_step6_2.png)

## Summary

Congratulations! You learned and achieved the following:

* What is a GKS cluster
* How to create an GKS cluster

The following sections describe cluster usage examples.
