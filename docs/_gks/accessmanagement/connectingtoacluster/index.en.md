---
title: Connecting to a Cluster
lang: en
permalink: /gks/accessmanagement/connectingtoacluster/
nav_order: 7100
parent: Access Management
---
# Connecting to a Cluster

After you created a cluster in GKS, it is time to connect to
it. This is necessary to deploy and manage your applications.

## Finding Your Cluster

To find a cluster, you need to go to the detail view of the cluster.
For that, click on the entry `first-system`:

![Step 1](../images/ConnClus01.png)

## Getting Access

Click on  `Get Kubeconfic` in the top right corner:

![Step 2](../images/ConnClus02.png)

This way you download a file which is called `kubeconfig` in
Kubernetes jargon. This file contains all end points, certificates
and other information about the cluster. The `kubectl` command uses
this file to connect to the cluster.

To use `kubeconfig`, you need to register it on the console.
There are two ways to do this:

1. `kubectl` by default tries to use the file `.kube/config`
   in your home directory.
1. You can temporarily use the `kubeconfig` by exporting it to
   an environment variable.

To keep things straightforward and to avoid changing standards
on your system, choose the second method in the example.

For this you need to open a Terminal. In the screenshots we use
iTerm2 on macOS, but the examples work the same way when using
bash on Linux or Windows.

First, you need to find the downloaded `kubeconfig` file. Browsers
like Chrome or Firefox usually store it in the Downloads folder.
The name is composed of two parts:

* `kubeconfig-admin-`
* The cluster id

To register the `kubeconfig`, use the following command:

```bash
cd Downloads
export KUBECONFIG=$(pwd)/kubeconfig-admin-CLUSTERID
```

Now you can interact with the cluster. The simplest command is: "show
all the nodes that comprise my cluster":

```bash
kubectl get nodes

NAME                           STATUS   ROLES    AGE   VERSION
musing-kalam-XXXXXXXXX-ks4xz   Ready    <none>   10m   v1.20.7
musing-kalam-XXXXXXXXX-txc4w   Ready    <none>   10m   v1.20.7
musing-kalam-XXXXXXXXX-vc4g2   Ready    <none>   10m   v1.20.7
```

## Kubernetes Dashboard

In GKS you can access the Kubernetes dashboard with one click.
You only need to click on the `Open Dashboard` button on the top right of the cluster view:

![Step 4](../images/ConnClus03.png)

Now you see the Kubernetes dashboard and can explore your cluster
graphically:

![Step 5](../images/ConnClus04.png)

## Summary

You've learned the following:

* How to get the `kubectl` configuration file
* How to configure `kubectl` to use the `kubeconfig`file
* How to access the Kubernetes dashboard for a cluster

Congratulations! Now you know all what's needed to connect to a
Kubernetes cluster.
