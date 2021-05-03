---
title: Getting Started
lang: en
permalink: /imke/gettingstarted/
nav_order: 2000
has_children: false
---


This Guide describes how to create your first iMKE project with a first Kubernetes
cluster, how to connect to that cluster and how to clean up all resources
afterwards.

## Create your first project

After logging into iMKE for the very first time, we will see the following window.
As a project is required to create our first Kubernetes cluster, we
need to click on `Add Project` as a very first step:
![Add Project](addproject.png)

This opens a window, where we can name the project. In the
example, we use `Team Kubernetes`.
To finish, we click on `Save`.

![Add Project Modal](addproject_modal.png?resize=600)

Now iMKE creates our Project and adds it to the Overview. With a click on
the entry `Team Kubernetes` we enter the project:
![Project list](projectlist.png)

This opens a view showing us the project. We find a list of all existing
clusters and their users, as well as some other controls.
![Project View](projectview.png)

At the moment, this list is empty until we create our first managed Kubernetes cluster.

## Create your first cluster

To create the cluster, we click on `Create Cluster` in the top right corner:
![Add Cluster](projectview_addcluster.png)

This opens the first page of the cluster creation procedure.
We choose the provider `openstack` and one of the three
datacenters. In this example, we pick `IX2`:
![Add Cluster Step 1](add_step1.png)

In the next step, we have to configure the cluster details. In the example,
we call our Cluster `first-system` and select the desired Kubernetes version:
![Add Cluster Step 2](add_step2.png)

For occassional SSH access to worker nodes, we need can optionally deploy an SSH Key.
To add an SSH-Key, we click on `Add SSH key`:
![Add Cluster Step 2.2](add_step2_2.png)

After that we can add the SSH Public Key and give it a memorable name:
![Add Cluster Step 2.3](add_step2_3.png)

To allow iMKE to request the required resources from OpenStack, we add our
OpenStack credentials now. After that, the contents of `Project` is refreshed
automatically and we can choose the OpenStack project we want to run the cluster
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

We choose `Ubuntu` as the operating system for the worker nodes:
![Add Cluster Step 4.3](add_step4_3.png)

To finish, we click on `Next`. After we verified all settings, we click on `Create`.
![Add Cluster Step 5](add_step5.png)

Now the cluster is being created. To access the information, we return to the Cluster
view of the project and click our Cluster's name:
![Add Cluster Step 6](add_step6.png)

This will open a page with all cluster details:
![Add Cluster Step 6.2](add_step6_2.png)

## Access your first cluster

To access the cluster, we need to click on the downwards
facing arrow at the top right corner.

![Step 2](connect_2.png)

This way we download a file which is called `kubeconfig` in
kubernetes jargon. This file contains all end points, certificates
and other information about the cluster. The `kubectl`command uses
this file to connect to the cluster.

To use the `kubeconfig`, we need to register it on the console.
There are two ways to do this:

1. `kubectl` by default tries to use the file `.kube/config`
   in your home directory.
2. We can temporarily use the `kubeconfig` by exporting it to
   an environment variable.

To keep things straightforward and to avoid changing standards
on our system, we choose the second method in the example.

For this we need to open a Terminal. In the screenshots we use
iTerm2 on macOS, but the examples work the same way when using
bash on Linux or Windows.

First, we need to find the downloaded `kubeconfig` file. Browsers
like Chrome or Firefox usually store it in the Downloads folder.
The name is constructed from two parts:

* `kubeconfig-admin-`
* the cluster id.

 To register the kubeconfig, we use the following command:

```bash
cd Downloads
export KUBECONFIG=$(pwd)/kubeconfig-admin-CLUSTERID
```

Now we can interact with the cluster. The simplest command is: "show
all the nodes that comprise my cluster":

```bash
kubectl get nodes

NAME                           STATUS   ROLES    AGE   VERSION
musing-kalam-XXXXXXXXX-ks4xz   Ready    <none>   10m   v1.15.0
musing-kalam-XXXXXXXXX-txc4w   Ready    <none>   10m   v1.15.0
musing-kalam-XXXXXXXXX-vc4g2   Ready    <none>   10m   v1.15.0
```

## Cleanup

To cleanup the cluster we created, we need to click `Delete` in the iMKE-Dashboard:
![Step 3](delete_3.png)

This opens a window where we need to enter the cluster name
to avoid sudden and unwanted deletions:
![Step 4](delete_4.png)

Since we also want to free up the resources, we leave both check
boxes marked. That way, volumes and load balancers provided by
OpenStack will be removed as well.
