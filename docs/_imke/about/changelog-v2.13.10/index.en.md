---
title: iMKE Changelog v2.13.10
lang: en
permalink: /imke/about/changelog-v2.13.10/
nav_order: 2
parent: About iMKE
---

iMKE upgraded from v2.12.4 → v2.13.10

## Supported Kubernetes versions

Kubernetes clusters will be upgraded as follows in order to deploy bugfixes and security fixes.

v1.15.x -> v1.15.10

v1.16.x -> v1.16.13

v1.17.x -> v1.17.9

- Customers can now create Kubernetes clusters with version v1.17.x.
- End-of-Life Kubernetes v1.14 is no longer supported.

## Major new features

- The `authorized_keys` files on nodes are now updated whenever the SSH keys for a cluster are changed.
- Added support for custom CA for OpenID provider in the API.
- Added user settings panel.
- Added cluster addon UI
- MachineDeployments can now be configured to enable [Dynamic Kubelet Configuration](https://kubernetes.io/blog/2018/07/11/dynamic-kubelet-configuration/)
- Added RBAC management functionality to UI

## Cloud providers

- Openstack: A bug that caused cluster reconciliation to fail if the controller crashed at the wrong time was fixed
- Openstack: New Kubernetes 1.16+ clusters use the external [Cloud Controller Manager](https://kubernetes.io/docs/tasks/administer-cluster/running-cloud-controller/) and [CSI](https://kubernetes.io/blog/2019/01/15/container-storage-interface-ga/) by default
- Openstack: include distributed routers in existing router search

## Bugfixes

- Fixed master-controller failing to create project-label-synchronizer controllers.
- Fixed broken NodePort-Proxy for user clusters with LoadBalancer expose strategy.
- Fixed cluster namespaces being stuck in Terminating state when deleting a cluster.
- Fixed Seed Validation Webhook rejecting new Seeds in certain situations
- A panic that could occur on clusters that lack both credentials and a credentialsSecret was fixed.
- A bug that occasionally resulted in a `Error: no matches for kind- "MachineDeployment" in version "cluster.k8s.io/v1alpha1"` visible in the UI was fixed.
- A memory leak in the port-forwarding of the Kubernetes dashboard endpoint was fixed
- Fixed a bug that prevented clusters in working seeds from being listed in the- dashboard if any other seed was unreachable.
- Prevented removing system labels during cluster edit
- Fixed deleting user-selectable addons from clusters.
- Fixed node name validation while creating clusters and node deployments
- Fixed swagger and API client for ssh key creation.
- Fixed a bug preventing editing of existing cluster credential secrets
- Fix `componentsOverride` of a cluster affecting other clusters

## Kubernetes related changes

### What's new in Kubernetes 1.17

- Cloud Provider Labels reached GA
- Volume Snapshot moved to Beta
- CSI Migration is in Beta

### Upgrade notes for Kubernetes 1.17

If you are planning to upgrade, please have a look at the [Upgrade Notes](https://v1-17.docs.kubernetes.io/docs/setup/release/notes/#urgent-upgrade-notes) section of the official Kubernetes v1.18 release notes and make sure you familiarise yourself with the upcoming changes.

For an overview of the changes, please refer to the [Changes](https://v1-17.docs.kubernetes.io/docs/setup/release/notes/#changes) section of the Changelog.

* [Deprecations](https://v1-17.docs.kubernetes.io/docs/setup/release/notes/#deprecations-and-removals)
* [API changes](https://v1-17.docs.kubernetes.io/docs/setup/release/notes/#api-changes)
* [Metrics changes](https://v1-17.docs.kubernetes.io/docs/setup/release/notes/#metrics-changes)
* [Features](https://v1-17.docs.kubernetes.io/docs/setup/release/notes/#notable-features)
* [Other changes/Bugfixes](https://v1-17.docs.kubernetes.io/docs/setup/release/notes/#other-notable-changes)
