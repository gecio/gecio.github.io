---
title: CNI Choices
lang: en
permalink: /imke/clusterlifecycle/cnichoices/
nav_order: 4250
parent: Cluster Lifecycle
---

# Choosing a CNI

This page is providing a small overview of what CNIs are, why they are used and what features they provide.


## What's a CNI and why do I need it

CNI is the abbreviation for ContainerNetworkInterface. Simple put it's a way for kubernetes to describe what
network functionality is needed to implement pod-to-pod networking inside a kubernetes cluster. There
are many different (network) environments where kubernetes can be run. To focus more on the kubernetes
product the kubernetes developers decided to just specify what is required in terms of network functionaliy
and let other do the implemention. This resulted in many different implementations for Kubernetes networking
(CNIs) with very different features for very different environments.

Our Kubernetes platform only supported one CNI implementation for the longest time and therefore did not
provide any means to change that. Recently we added a second one so you are now able to choose which CNI
suits your use-case best.


## Canal

The CNI solution we used in the past is called *canal*, which is a combination of the two CNIs *flannel*
and *calico*. In these setups flannel is used to implement the pod-to-pod communication while calico is
used enable network policies. This CNI has been around a long time and is thus mature and battle-tested.

Canal supports both *iptables* as well as *ipvs* as proxy modes and runs on almost any OS image.

If you are unsure on what to choose, this CNI is the conservative choice.


## Cilium

Cilium is a fairly new CNI which leverages eBPF instead of traditional ways to manage traffic between
kubernetes pods and nodes. It has advanced observability features like a dedicated looking-glass addon
called *hubble* and improved health checks. Cilium also supports network policies to allow tighter
control of the traffic flow into or out of your cluster as well as inter-cluster traffic.

If you want to utilize Cilium features to the fullest you need to set the proxy mode to *eBPF* after
choosing the CNI (this can only be done if Konnectivity is chosen as well!). Another requirement for
Cilium (and especially the eBFP proxy mode) is that the OS image needs to run a fairly recent kernel
to be able to support all functionality of the CNI (which is the case with our flatcar images!).
A compatibility matrix can be found [here](https://docs.cilium.io/en/stable/operations/system_requirements/).

Cilium is under heavy development so new features as well as bugfixes are released regularly.

If you are interested in a deeper understanding of the network flows inside your kubernetes cluster,
then this CNI is for you.


# None

There is also the possibility to install a custom CNI yourself. In that case you choose *None* as
CNI and the installation will skip the installation step for a CNI.

Please be aware that this is only for the most advanced users of kubernetes and you should really
know what you are doing!


# Installing a CNI

In the cluster creation process the Nth(FIXME) step enables you to choose between the two CNIs described
above.

![choose CNI](choosing_cni.png)

After choosing your CNI you may need to choose a proxy mode as well, depending on the CNI chosen
in the previous step.

![choose proxy](choosing_proxy_mode.png)

Be aware that the choice of the eBPF proxy requires that you choose *Konnectivity* as your control-plane
connector as well (box beside Konnectivity needs to be checked)! More information on the control-plane
connector and Konnectivity can be found on [this](/imke/clusterlifecycle/controlplaneconnector) dedicated
page.


# Installation Hubble addon

If Cilium is chosen as a CNI, you can install the graphical visualization addon hubble. This can be
done after cluster creation has finished successfully via the addons tab at the bottom of the cluster
overview.

![install hubble](installing_hubble_addon.png)


# Final note

The choice of an CNI can only be done once in the creation process of the cluster. While it is technically
possible to switch or change CNIs in a running cluster, we are not supporting this in our platform. So choose
wisely as this choice cannot be changed without deleting and re-creating the cluster!



# Further reading

* [Flannel github page](https://github.com/flannel-io/flannel)
* [Canal github page](https://github.com/projectcalico/canal)
* [Canal installation docs](https://projectcalico.docs.tigera.io/getting-started/kubernetes/flannel/flannel)
* [Cilium Documentation](https://docs.cilium.io/en/stable/)
