---
title: Machine Deployments
lang: en
permalink: /imke/machinedeployments/
nav_order: 5000
has_children: true
---

A Kubernetes-Cluster typically consists of a controlplane (running the Kubernetes API, the etcd database and other components) and worker-nodes - (virtual) servers, which run the actual workloads (applications etc.).

The controlplane of all iMKE-Clusters is managed by the iMKE-platform itself and is not accessible for customers.

The worker-nodes of a cluster need to be configured via a `Machine Deployment`. The customer can control:

* *Node Flavors* are the virtual machine types which are used to create the worker-nodes. The types usually differ in their size, i.e. in the available CPU and RAM.
* *Number of Nodes* of course you can choose how many worker-nodes you want to have in total.
* *SSH-Keys* if enabled for your cluster and if your Nodes have a public IP (Floating IP), you can connect to your worker-nodes via SSH, i.e. for extended debugging.
* *Operating System of the Nodes* although the OS of the worker-nodes should not matter for the Kubernetes workloads, it might matter for you and your debugging.

**Further reading**
* [Sizing: Choosing Node Flavors](/imke/machinedeployments/nodeflavors/)
* [Debugging: Cluster Nodes Usage Rate](/imke/machinedeployments/clusternodesusagerate/)
* [Debugging: Adding SSH-Keys to worker nodes](/imke/machinedeployments/add_ssh_key/)
* [Upgrading the OS on Worker-Nodes](/imke/machinedeployments/updatingnodeos/)
