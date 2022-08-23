---
title: Control Plane Connector
lang: en
permalink: /gks/clusterlifecycle/controlplaneconnector/
nav_order: 4280
parent: Cluster Lifecycle
---

# Control Plane Connector

Here you find an overview of how the worker nodes communicate with the control plane.

## Cluster Setup

The GKS platform provides a managed Kubernetes services to end users. For more reliability the
control plane of these Kubernetes clusters is isolated from the workers. The end user has complete
control of the worker nodes (right down to the operating system of the VM the worker runs on) in the
end user's own Openstack tenant. The control plane however is managed by the service provider and runs
on isolated clusters apart from the worker nodes in different Openstack tenants. Still the worker nodes
must communicate with the control plane on a regular bases.

### Communication Initiated from the Worker Nodes to the Control Plane

This direction is easy as the communication with the control plane is done by the Kubernetes API server
exposing it's service via an publicly accessible IPv4 address. That way components running on the worker
nodes create a TCP connection from inside their openstack tenants private subnet, exiting the cluster
via the router's egress to the internet and entering the control planes' dedicated openstack tenants
subnet where the respective Kubernetes API server is exposed publicy and then forwarded inside it's
private openstack subnet again.

But sometimes the Kubernetes API server needs to initiate the connection to the worker nodes (mostly
their kubelet's) either when doing "kubectl port-forward", or "kubectl exec", or "kubectl logs".
The worker nodes do not expose an endpoint to the internet for easy reachability (which is a good
thing from a security perspective).

### Communication Initiated from the Control Plane to the Worker Nodes

The trick is to create a VPN from the worker nodes to the control plane in advance and let the
control plane use this tunnel for communication. The configuration for this is done at cluster
creation time.

The way we have implemented this until recently was by managing an openVPN tunnel. This had some
drawbacks and has now been replaced by a tool from the Kubernetes community dedicated for this
special purpose: enter *Konnectivity*. This tool offers more reliability by being able to create
multiple tunnels (instead of only one with the previous solution) much ore easily.

## Install/Migration

When creating a new cluster with the GKS platform, setup of Konnectivity is the default. Existing
clusters can be migrated easily by editing the cluster in the GKS dashboard and checking the box
right next to *Konnectivity*. This will initiate an automatic and seamless removal of openVPN and
creation of Konnectivity agent and -server pairs on the worker- and control plane nodes respectively.

## Learn More

* [Konnectivity](https://kubernetes.io/docs/concepts/architecture/control-plane-node-communication/#konnectivity-service)
* [GitHub repo for specification](https://github.com/kubernetes-sigs/apiserver-network-proxy)
