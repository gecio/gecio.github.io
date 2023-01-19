---
title: GKS Changelog v2.17
lang: "en"
permalink: /gks/about/changelog-v2.17/
nav_order: 1000
parent: About GKS
---

## Supported Kubernetes versions

With the current release, we support the following Kubernetes versions:

* 1.18.18
* 1.19.10
* 1.19.11
* 1.20.7
* 1.21.2

## End of Life announcements

We will end the support of Kubernetes version 1.18 on the 15th September 2021.

Please make sure to upgrade all existing clusters running 1.18 at least to 1.19 before that date.

## New features

* Added support for Kubernetes 1.20
* Added support for Kubernetes 1.21

## Bugfixes

* the recurring cluster-event "failed to ensure CronJob kube-system/etcd-backup-$CLUSTERID: failed waiting for the cache to contain our latest changes: timed out waiting for the condition" has been fixed.

## Kubernetes related changes

### Upgrade notes for Kubernetes 1.20

If you are planning to upgrade to Kubernetes 1.20, please have a look at the [Upgrade Notes](https://v1-20.docs.kubernetes.io/docs/setup/release/notes/#urgent-upgrade-notes) section of the official Kubernetes v1.20 release notes and make sure you familiarise yourself with the upcoming changes.

For an overview of the changes, please refer to the [Changes by Kind](https://v1-20.docs.kubernetes.io/docs/setup/release/notes/#changes-by-kind) section of the Changelog.

* [Urgent Upgrade Notes](https://v1-20.docs.kubernetes.io/docs/setup/release/notes/#urgent-upgrade-notes)
* [Deprecations](https://v1-20.docs.kubernetes.io/docs/setup/release/notes/#deprecation)
* [API changes](https://v1-20.docs.kubernetes.io/docs/setup/release/notes/#api-change)
* [Features](https://v1-20.docs.kubernetes.io/docs/setup/release/notes/#feature)

### Upgrade notes for Kubernetes 1.21

If you are planning to upgrade to Kubernetes 1.21, please have a look at the [What's new](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.21.md#whats-new-major-themes) section of the official Kubernetes v1.21 release notes and make sure you familiarise yourself with the upcoming changes.

For an overview of the changes, please refer to the [Changes by Kind](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.21.md#changes-by-kind-2) section of the Changelog.

* [Urgent Upgrade Notes](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.21.md#urgent-upgrade-notes)
* [Deprecations](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.21.md#deprecation)
* [API changes](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.21.md#api-change-1)
* [Features](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.21.md#feature-2)
