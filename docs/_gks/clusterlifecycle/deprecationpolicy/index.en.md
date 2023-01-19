---
title: Deprecation Policy
lang: "en"
permalink: /gks/clusterlifecycle/deprecationpolicy/
nav_order: 4500
parent: Cluster Lifecycle
---

# Deprecation Policy

The upstream Kubernetes project releases approximately four Kubernetes versions a year and deprecates the same number of old versions. Up to v1.18, Kubernetes followed an N-2 support policy, meaning that the 3 most recent minor versions received security and bug fixes.

From v1.19 onwards, Kubernetes follows an N-3 support policy. This means that the support window is extended to one year.

A good visualization of the support period for each release is available below:

[![K8sVersionSupport](../images/k8s_version_support.png)](https://endoflife.date/kubernetes)

GKS aligns loosely to this lifecycle by continuously introducing new versions and deprecating older ones.

After a given Kubernetes version has reached End-of-Life, it will not get any bugfixes or security updates. Hence, we cannot support it anymore either and have to deprecate it.

## Deprecation Process

If we deprecate a Kubernetes version, we inform the customers in advance about the deprecation (End-of-Life announcement) to give them enough time to plan, prepare, and perform the Kubernetes upgrade themselves.

When this grace period is over, we remove the deprecated version from GKS.

You can find the list of supported Kubernetes versions and their planned End-of-Life dates [here](/gks/about/kubernetesversions/). A detailed documentation about how to upgrade clusters is available [here](../upgradingacluster/).

**What Does an End-of-Life Announcement Mean for Me?**

If an End-of-Life announcement for a given Kubernetes version has been made, customers are asked to upgrade their clusters to a newer version (preferably to the latest one).

**What Happens If I Do Not Update Until the EOL Date?**

If a customer cluster will not get updated before the removal of an EOL Kubernetes version, nothing happens at first. One cannot create new clusters with the deprecated version anymore, but existing clusters will continue to run.

**Can I Stay on an EOL Version Forever?**

No, as this would possibly mean serious security issues in the future.

## GKS Force Upgrade Policy

If a Kubernetes version reaches End-of-Life, we have to remove its support from GKS since it will not receive any bugfixes or security updates anymore. From this point onward it is no longer possible to create new clusters with this version.

It is important to emphasize the following technical limitations in Kubernetes:

* A (control plane of a) Kubernetes cluster can be upgraded by one version at a time, e.g. from v1.21 -> v1.22.
* It is not possible to upgrade more than one versions at once.
* It is not possible to downgrade a cluster.

This means that if customers do not update their clusters before the removal of the **next** EOL version, they risk not being able to upgrade after the removal of the **next** deprecated version.

This would imply a serious problem as their only alternative would be to create a new cluster and migrate the workload from the old one as upgrading would not be possible (since it would require upgrading two versions at once).

To overcome this issue, we need to actively force customers to upgrade clusters running on an EOL Kubernetes version, before we remove the next deprecated version.

**What Happens to My Clusters During Force Upgrade?**

We will initiate an automated Kubernetes upgrade for the control plane and the Machine Deployment(s).

While this *should* work, it cannot be guaranteed to work, given the diversity of applications and use cases.

Breaking changes in the Kubernetes API can lead to broken/incompatible applications inside the Kubernetes cluster. **We can not overtake responsibility for such problems**.

That's why we strongly advise every customer to plan and perform the Kubernetes upgrade themselves.
