---
title: Deprecation Policy
lang: en
permalink: /imke/clusterlifecycle/deprecationpolicy/
nav_order: 4500
parent: Cluster Lifecycle
---

## Deprecation policy in iMKE

The upstream Kubernetes project releases approximately four Kubernetes versions a year and deprecates the same number of old versions. Up to v1.18, Kubernetes followed an N-2 support policy, meaning that the 3 most recent minor versions received security and bug fixes.

From v1.19 onwards, Kubernetes will follow an N-3 support policy. This means that the support window will be extended to one year.

A good visualization of the period for which each release is/was supported is below:

![K8sVersionSupport](k8s_version_support.png)

iMKE aligns loosely to this lifecycle by continuously introducing new versions and deprecating older ones.

After a given Kubernetes version has reached End-of-Life, it will not get any bugfixes or security updates. Hence, we can not support it anymore either and have to deprecate it.

## The deprecation process

If we deprecate a Kubernetes version, we do this by informing the customers in advance about the deprecation (End-of-Life announcement) to give them enough time to plan, prepare and perform the Kubernetes upgrade by themselves.

When this grace period is over, we remove the deprecated version from iMKE.

You can find the list of supported Kubernetes versions and their planned End-of-Life dates [here](/imke/about/kubernetesversions/). A detailed documentation about how to do cluster upgrades can be found [here](../upgradingacluster/).

**What does an End-of-Life announcement mean for me?**

If an End-of-Life announcement for a given Kubernetes version has been made, customers are asked to upgrade their clusters to a newer version (preferably the latest one).

**What happens if I don't update until the EOL date?**

If a customer cluster will not get updated before the removal of an EOL'd Kubernetes version, nothing happens at first. One can not create new clusters with the removed version anymore, but existing clusters will continue to run.

**Can I stay on an EOL version forever?**

No, as this would possibly mean serious security issues in the future.

## iMKE Force Upgrade policy

If a Kubernetes version reaches End-of-Life, we have to remove its support from iMKE as it will not receive any bugfixes or security updates anymore. From this point on it is no longer possible to create new clusters with this version.

It is important to underline the following technical limitations in Kubernetes:

* A (control-plane of a) Kubernetes cluster can be upgraded by one version at a time, e.g. from v1.17 -> v1.18.
* It is not possible to upgrade more than one versions at once.
* It is not possible to downgrade a cluster.

What this means for customers is that if they don't update their clusters before the removal of the **next** EOL'd version, they risk not to be able to upgrade after the removal of the **next** deprecated version.

This would imply a serious problem, since their only alternative would be to create a new cluster and migrate the workload over from the old one, as an upgrade would not be possible (given that this would require upgrading two versions at once).

To overcome this issue, we need to actively force upgrade clusters running on an already EOL'd Kubernetes version, before removing the next deprecated version.

**What happens to my clusters during force upgrade?**

We will initiate an automated Kubernetes upgrade for the control plane and the Machine Deployment(s).

While this *should* work, it can not be guaranteed to work, given the diversity of applications and use cases.

Breaking changes in the Kubernetes API can lead to broken/incompatible applications inside the Kubernetes cluster. **We can not overtake responsibility for such problems**.

That's why we strongly advise every customer to plan and perform the Kubernetes upgrade themselves.
