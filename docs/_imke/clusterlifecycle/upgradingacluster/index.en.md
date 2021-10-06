---
title: Kubernetes Updates
lang: en
permalink: /imke/clusterlifecycle/upgradingacluster/
nav_order: 4200
parent: Cluster Lifecycle
---

Cluster security is paramount, and new features come with
each release. In order to stay safe and up-to-date, means
installing Kubernetes updates on a regular basis.

In especially critical cases, we will automatically update the
Cluster API to the latest minor version in order to keep our
own infrastructure up-to-date. In this case the following section
*The Cluster* can be skipped. However, nodes must still be updated
manually by you.

Before you upgrade a cluster, please refer to the target version's [Changelog](/imke/about/)
and make sure you familiarise yourself with the upcoming changes.

One tool that can help to prepare the update will be [kubepug](https://github.com/rikatz/kubepug).
It checks all deployed resources against the new Kubernetes version and will warn about removals and deprecations.

## The Cluster

In Kubernetes the infrastructure is divided into master (= Kubernetes control plane) and (worker-)nodes.
The master is managed by iMKE itself.

Since several versions for the master are offered, you have the
possibility to choose the version in iMKE's web interface. An
update of the master can be accomplished with a few mouse clicks.

First, we select the cluster which we'd like to update.

![Step 1](update_1.png)

Then we click on the field `Master Version` and choose a new
version for the master.

![Step 2](update_2a.png)

It is recommended to select `Upgrade Machine Deployments`, as this will upgrade the worker-nodes as well:

![Step 2](update_2b.png)

Now iMKE automatically updates the master and optionally the
worker nodes as well, and we are done with this step.

## The Nodes

If the master has been updated without upgrading the Machine Deployments or if a scheduled maintenance of the iMKE
platform has led to an implicit upgrade of the master (normally this just updates a patchlevel), we must still update the nodes.
The iMKE web interface can help us here as well.

It's worth noting that this update process deletes the old nodes and
replaces them with new ones. This also means that all pods will be
restarted.

The first step is to click on the Machine Deployment.

![Step 3](update_3.png)

Next we click on the pencil icon to open the update view.

![Step 4](update_4.png)

Now, under `kubelet Version` we select the version, for example
`1.21.5`, which matches the cluster's master version. Confirm the
update by clicking `Save Changes`:

![Step 5](update_5.png)

Now iMKE will automatically update the node group to the new version,
amd Kubernetes will take care of deploying your applications to the
new nodes.

## Two Node Cluster

Watch out for clusters with two nodes or fewer. iMKE uses a rolling update
strategy. This means one node after another is swapped. In a cluster
with two or fewer nodes that means that the first updated node must
be fully scheduled before the second one is ready.

As a solution to this is a simple bash script, which per-namespace triggers
the regeneration of all pods.
<https://github.com/truongnh1992/playing-with-istio/blob/master/upgrade-sidecar.sh>

We use this after the cluster has been completely updated in a terminal with `kubectl` configured. To get `kubectl` working with your cluster, look at our chapter [Connecting to a Cluster](/imke/accessmanagement/connectingtoacluster/).

```bash
curl -o upgrade-node.sh https://raw.githubusercontent.com/truongnh1992/playing-with-istio/master/upgrade-sidecar.sh
chmod +x upgrade-node.sh
echo -e "#\!/bin/bash\n$(cat upgrade-node.sh)" > upgrade-node.sh
```

Now we must use this script on all of our namespaces.

```bash
kubectl get namespace
NAME              STATUS   AGE
default           Active   36m
kube-node-lease   Active   36m
kube-public       Active   36m
kube-system       Active   36m

# So for default namespace we would run:
./upgrade-node.sh default
Refreshing pods in all Deployments
```

Now all pods are cleanly distributed across our nodes.
