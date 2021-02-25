---
title: iMKE Changelog v2.14.8
lang: en
permalink: /imke/about/changelog-v2.14.8/
nav_order: 1100
parent: About iMKE
---

iMKE upgraded from v2.13.10 → v2.14.9

## Supported Kubernetes versions

Kubernetes clusters will be upgraded as follows in order to deploy bugfixes and security fixes.

v1.15.x -> v1.15.11

v1.16.x -> v1.16.13

v1.17.x -> v1.17.9

v1.18.x -> v1.18.6

- Customers can now create Kubernetes clusters with version v1.18.x.
- End-of-Life CoreOS no longer supported (replaced by Flatcar)

## New features

- Added support for Kubernetes 1.18
- Added Flatcar Linux as an Operating System option
- Added support for creating RBAC bindings to group subjects

## Cloud providers

- Openstack: Fixed a bug preventing the usage of pre-existing subnets connected to distributed routers
- Openstack: Fix changing user/password for OpenStack cluster credentials.

## Bugfixes

- Fix bad apiserver Deployments when no Dex CA was configured.
- Fixed cluster credential Secrets not being reconciled properly.
- Fixed swagger and API client for ssh key creation.
- Fixed seed-proxy controller not being triggered.
- Added missing Flatcar Linux handling in API
- Fixed nodes sometimes not having the correct distribution label applied.
- Fixed missing Prometheus metrics.
- Fix label for nodeport-proxy when deployed with the operator.
- Create an RBAC role to allow kubeadm to get nodes. This fixes nodes failing to join kubeadm clusters running Kubernetes 1.18+.

## Kubernetes related changes

### What's new in Kubernetes 1.18

- Kubernetes Topology Manager has been moved to Beta
- Serverside Apply has reached Beta 2
- Ingress has been extended with a `pathType` field
- The `IngressClass` resource has been introduced
- Addition of the `kubectl debug` feature
- Node Local DNSCache graduated to GA

### Upgrade notes for Kubernetes 1.18

If you are planning to upgrade to Kubernetes 1.18, please have a look at the [Upgrade Notes](https://v1-18.docs.kubernetes.io/docs/setup/release/notes/#urgent-upgrade-notes) section of the official Kubernetes v1.18 release notes and make sure you familiarise yourself with the upcoming changes. 

For an overview of the changes, please refer to the [Changes by Kind](https://v1-18.docs.kubernetes.io/docs/setup/release/notes/#changes-by-kind) section of the Changelog.

* [Deprecations](https://v1-18.docs.kubernetes.io/docs/setup/release/notes/#deprecation)
* [API changes](https://v1-18.docs.kubernetes.io/docs/setup/release/notes/#api-change)
* [Configuration file changes](https://v1-18.docs.kubernetes.io/docs/setup/release/notes/#configuration-file-changes)
* [Features](https://v1-18.docs.kubernetes.io/docs/setup/release/notes/#feature)
* [Other changes/Bugfixes](https://v1-18.docs.kubernetes.io/docs/setup/release/notes/#other-bug-cleanup-or-flake)
