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

To create the cluster, we click on `Add Cluster` in the top right corner:
![Add Cluster](projectview_addcluster.png)

This opens the first page of the Cluster Creation procedure. In the example,
we call our Cluster `first-system` and select Kubernetes version 1.18.6:
![Add Cluster Step 1](add_step1.png)

Then we click `Next` and choose the provider `openstack` and one of the three
availability zones. In this example, we pick `IX1`:
![Add Cluster Step 2.1](add_step2_1.png) ![Add Cluster Step 2.2](add_step2_2.png)

To allow iMKE to request the required resources from OpenStack, we add our
OpenStack credentials now. After that, the contents of `Project` is refreshed
automatically and we can choose the OpenStack project we want to run the cluster
in:
![Add Cluster Step 3.1](add_step3.png)
![Add Cluster Step 3.2](add_step3_2.png)

We can use an existing network to create the cluster. At this point, we must select the network and the subnet.
Those must be attached to a router.
So you have to create a router, this can be done from the Optimist Dashboard or from the OpenStack command line.
We can use our OpenStack documentation to create the router and attach it. (</optimist/guided_tour/step10/>)
![Add Cluster Network](create-cluster-network-exist.png)

In Node Settings we define how many virtual machines will be available as worker nodes
in the cluster. This node deployment needs a name and a size. For the first cluster
the name is not super important, so we use the random name generator. We use the
defaults for the number and size of the machines.
![Add Cluster Step 4](add_step4.png)

We choose Flatcar Linux as the image. Flatcar Linux is designed to run containers
and will automatically be updated by iMKE.
![Add Cluster Step 5](add_step5.png)

For occassional SSH access, we need to deploy an SSH Key and to assign a Floating IP to each Worker-Node.
To add an SSH-Key, we click on `Add SSH Key`, enter an SSH Public Key, and give it a memorable name:
![Add Cluster Step 6](add_step6.png)
![Add Cluster Step 6.2](add_step6_2.png)

To finish, we click on `Next`. After we verified all settings, we click on `Create`.
![Add Cluster Step 7](add_step7.png)

Now the cluster is being created. To access the information, we return to the Cluster
view of the project and click our Cluster's name.
![Add Cluster Step 8](add_step8.png)
![Add Cluster Step 8.2](add_step8_2.png)

## Summary

In this guide, we achieved and learnt the following:

* What is an iMKE Project
* How to create an iMKE Project
* What is an iMKE Cluster
* How to configure an iMKE Cluster
* How to start an iMKE Cluster

Congratulations! Now you know how to create a Kubernetes Cluster with iMKE.
The following pages describe cluster usage examples.
