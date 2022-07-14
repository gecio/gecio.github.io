---
title: Machine Deployments
lang: en
permalink: /gks/machinedeployments/
nav_order: 5000
has_children: true
---
# Machine Deployments

A Kubernetes-Cluster typically consists of a controlplane (running the Kubernetes API, the etcd database, and other components) and worker nodes - (virtual) servers, which run the actual workloads (applications etc.).

The controlplane of all GKS-Clusters is managed by the GKS platform itself and is not accessible for customers.

The worker nodes of a cluster need to be configured with a `Machine Deployment`. The customer can control:

* *Node Flavors* are the virtual machine types which are used to create the worker nodes. The types usually differ in their size, i.e. in the available CPU and RAM.
* *Number of Nodes* you can choose how many worker nodes you want to have in total.
* *SSH-Keys* if enabled for your cluster and if your nodes have a public IP (Floating IP), you can connect to your worker nodes with SSH, i.e. for extended debugging.
* *Operating System of the Nodes* although the OS of the worker-nodes should not matter for the Kubernetes workloads, it might matter for you and your debugging.

## Learn More

* [Sizing: Choosing Node Flavors](/gks/machinedeployments/nodeflavors/)
* [Debugging: Cluster Nodes Usage Rate](/gks/machinedeployments/clusternodesusagerate/)
* [Debugging: Adding SSH Keys to Worker Nodes](/gks/machinedeployments/add_ssh_key/)
* [Upgrading the OS on Worker Nodes](/gks/machinedeployments/updatingnodeos/)
