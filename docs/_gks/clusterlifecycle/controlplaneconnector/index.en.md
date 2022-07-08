---
title: Control Plane Connector
lang: en
permalink: /gks/clusterlifecycle/controlplaneconnector/
nav_order: 4250
parent: Control Plane Connector
---

# Control Plane Connector

This page is providing an overview of how the worker nodes are communicating with the control-plane.


# Cluster Setup

The GKS platform is providing managed kubernetes services to end-users. For more reliability the
control-plane of these kubernetes clusters is isolated from the workers. The end-user has complete
control of the worker nodes (right down to the operating system of the VM the worker runs on) in the
end-users own openstack tenant. The control-plane however is managed by the service provider and runs
on isolated clusters apart from the worker-nodes in different openstack tenant. Still the worker nodes
must communicate with the control-plane on a regular bases.


## Communication initiated from the worker nodes to the control-plane

This direction is easy as the communication with the control-plane is done by the kubernetes api-server
exposing it's service via an publicly accessible IPv4 address. That way components running on the worker
nodes create a TCP connection from inside their openstack tenants private subnet, exiting the cluster
via the router's egress to the internet and entering the control-planes' dedicated openstack tenants
subnet where the respective kubernetes api-server is exposed publicy and then forwarded inside it's
private openstack subnet again.

But sometimes the kubernetes api-server needs to initiate the connection to the worker-nodes (mostly
their kubelet's) either when doing "kubectl port-forward"'s or "kubectl exec"'s or "kubectl logs"'s.
The worker nodes do not expose an endpoint to the internet for easy reachability (which is a good
thing from a security perspective!).


## Communication initiated from the control-plane to the worker nodes

The trick is to create a VPN from the worker nodes to the control-plane in advance and let the
control-plane use this tunnel for communication. The configuration for this is done at cluster
creation time.

The way we have implemented this until recently was by managing an openVPN tunnel. This had some
drawbacks and has now been replaced by a tool from the kubernetes community dedicated for this
special purpose: enter *Konnectivity*. This tool offers more reliability by being able to create
multiple tunnels (instead of only one with the previous solution) much ore easily.


# Install/Migration

When creating a new cluster with the GKS platform, setup of Konnectivity is the default. Existing
clusters can be migrated easily by editing the cluster in the GKS-dashboard and checking the box
right next to *konnectivity*. This will initiate an automatic and seamless removal of openVPN and
creation of konnectivity-agent and -server pairs on the worker- and control-plane nodes respectively.


# Further reading

* [konnectivity](https://kubernetes.io/docs/concepts/architecture/control-plane-node-communication/#konnectivity-service)
* [Github repo for specification](https://github.com/kubernetes-sigs/apiserver-network-proxy)
