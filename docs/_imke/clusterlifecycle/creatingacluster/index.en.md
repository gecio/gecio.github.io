---
title: Creating a Cluster
lang: en
permalink: /imke/clusterlifecycle/creatingacluster/
nav_order: 4100
parent: Cluster Lifecycle
---

We can create a cluster in iMKE with a couple of clicks.
Before we can do that, there needs to be a project.

So if you don't have a project yet, [create a project first](/imke/managingprojects/creatingaproject).

To create the cluster, we click on `Create Cluster` in the top right corner:
![Create Cluster](projectview_addcluster.png)

This opens the first page of the cluster creation procedure.
We choose the provider `openstack` and one of the three
datacenters. In this example, we pick `IX2`:
![Add Cluster Step 1](add_step1.png)

In the next step, we have to configure the cluster details. In the example,
we call our Cluster `first-system` and select the desired Kubernetes version:
![Add Cluster Step 2](add_step2.png)

For occasional SSH access to worker nodes, we need can optionally deploy an SSH Key.
To add an SSH-Key, we click on `Add SSH key`:
![Add Cluster Step 2.2](add_step2_2.png)

After that we can add the SSH Public Key and give it a memorable name:
![Add Cluster Step 2.3](add_step2_3.png)

To allow iMKE to request the required resources from OpenStack, we add our
OpenStack credentials now. After that, the contents of `Project` is refreshed
automatically, and we can choose the OpenStack project we want to run the cluster
in:
![Add Cluster Step 3.1](add_step3.png)
![Add Cluster Step 3.2](add_step3_2.png)

By adding the credentials and selecting the OpenStack project, we could proceed to the next
step. If we do so, a new and dedicated network, subnet and security group will be created for the cluster
automatically.

It is also possible to use an **existing** network to create the cluster. For this to work, we have to select
the network and the subnet from the dropdown menu. Those must be attached to a router.
Creating a router can be done from the Optimist Dashboard or from the OpenStack command line.
Please refer to our [OpenStack documentation](/optimist/guided_tour/step10/) for details how to create the router and attach it.
![Add Cluster Network](create-cluster-network-exist.png)

In the next step, we define how many and what kind of virtual machines will be initially available as worker nodes
in the cluster.

First, this so called `Machine Deployment` needs a name. For our test cluster we use the random name generator:
![Add Cluster Step 4](add_step4.png)

Next, we should specify the `Replicas` (number of worker nodes in your Kubernetes cluster) and the `Flavor` (machine type), which 
defines the amount of CPU and RAM for each worker node:
![Add Cluster Step 4.2](add_step4_2.png)

We choose `Flatcar` as the operating system for the worker nodes:
![Add Cluster Step 4.3](add_step4_3.png)

To finish, we click on `Next`. After we verified all settings, we click on `Create Cluster`:
![Add Cluster Step 5](add_step5.png)

Now the cluster is being created. To access the information, we return to the Cluster
view of the project and click our Cluster's name:
![Add Cluster Step 6](add_step6.png)

This will open a page with all cluster details:
![Add Cluster Step 6.2](add_step6_2.png)

## Summary

In this guide, we achieved and learnt the following:

* What is an iMKE Cluster
* How to create an iMKE Cluster

Congratulations! Now you know how to create a Kubernetes Cluster with iMKE.
The following pages describe cluster usage examples.
