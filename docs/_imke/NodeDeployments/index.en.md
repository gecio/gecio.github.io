---
title: Node Deployments
lang: en
permalink: /imke/nodedeployments
nav_order: 5
has_children: true
---

A Kubernetes-Cluster typically consists of a controlplane (running the Kubernetes API, the etcd database and other components) and Worker-Nodes - (virtual) servers, which run the actual workloads (applications etc.).

The controlplane of all iMKE-Clusters is managed by the iMKE-platform itself and is not accessible for customers.

The Worker-Nodes of a cluster need to be configured via a `Node Deployment`. The customer can control:

* *Node-Flavors* are the virtual machine types which are used to create the Worker-Nodes. The types usually differ in their size, i.e. in the available CPU and RAM.
* *Number of Nodes* of course you can choose how many Nodes you want to have in total.
* *SSH-Keys* if enabled for your cluster and if your Nodes have a public IP (Floating IP), you can connect to your Worker-Nodes via SSH, i.e. for extended debugging.
* *Operating System of the Nodes* although the OS of the Worker-Nodes should not matter for the Kubernetes workloads, it might matters for you and your debugging.

