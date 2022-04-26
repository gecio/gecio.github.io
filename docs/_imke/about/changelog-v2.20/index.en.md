---
title: iMKE Changelog v2.20
lang: en
permalink: /imke/about/changelog-v2.20/
nav_order: 700
parent: About iMKE
---

## Supported Kubernetes versions

With the current release, we support the following Kubernetes versions:

* 1.21.8
* 1.22.5

## End of Life announcements

We will end the support of Kubernetes version 1.21 on the 28th June 2022.

Please make sure to upgrade all existing clusters running 1.21 at least to 1.22 before that date.

## New features

This is a technical release without any new features.

## Bugfixes

* For user clusters that use etcd 3.5 (Kubernetes 1.22 clusters), etcd corruption checks are turned on to detect etcd data consistency issues. Checks run at etcd startup and every 4 hours ([#13766](https://groups.google.com/a/kubernetes.io/g/dev/c/B7gJs88XtQc/m/rSgNOzV2BwAJ))

## Kubernetes related changes

### Upgrade notes for Kubernetes 1.22

If you are planning to upgrade to Kubernetes 1.22, please have a look at the [What's new](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.22.md#whats-new-major-themes) section of the official Kubernetes v1.22 release notes and make sure you familiarise yourself with the upcoming changes.

For an overview of the changes, please refer to the [Changes by Kind](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.22.md#changes-by-kind-2) section of the Changelog.

* [Urgent Upgrade Notes](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.22.md#urgent-upgrade-notes)
* [Deprecations](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.22.md#deprecation)
* [API changes](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.22.md#api-change-1)
* [Features](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.22.md#feature-2)
