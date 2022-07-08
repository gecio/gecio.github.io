---
title: GKS Changelog v2.19
lang: en
permalink: /gks/about/changelog-v2.19/
nav_order: 800
parent: About GKS
---

## Supported Kubernetes versions

With the current release, we support the following Kubernetes versions:

* 1.21.8
* 1.22.5

## End of Life announcements

We will end the support of Kubernetes version 1.21 on the 28th June 2022.

Please make sure to upgrade all existing clusters running 1.21 at least to 1.22 before that date.

## New features

* CNI update functionality: the CNI canal can now be [updated via the dashboard](/gks/somepath/to/the/new/docs/)

## Bugfixes

* Updated supported Kubernetes versions to newest patch-levels to mitigate several CVEs ([CVE-2021-44716](https://www.cve.org/CVERecord?id=CVE-2021-44716), [CVE-2021-44717](https://www.cve.org/CVERecord?id=CVE-2021-44717), [CVE-2021-3711](https://www.cve.org/CVERecord?id=CVE-2021-3711), [CVE-2021-3712](https://www.cve.org/CVERecord?id=CVE-2021-3712), [CVE-2021-33910](https://www.cve.org/CVERecord?id=CVE-2021-33910)). Cluster control planes have been automatically updated. Please update your Machine Deployments soon, at a time convenient for you. Updating your Machine Deployment will lead to worker node recreation and rolling restarts of your Deployments and Statefulsets.

## Kubernetes related changes

### Upgrade notes for Kubernetes 1.22

If you are planning to upgrade to Kubernetes 1.22, please have a look at the [What's new](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.22.md#whats-new-major-themes) section of the official Kubernetes v1.22 release notes and make sure you familiarise yourself with the upcoming changes.

For an overview of the changes, please refer to the [Changes by Kind](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.22.md#changes-by-kind-2) section of the Changelog.

* [Urgent Upgrade Notes](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.22.md#urgent-upgrade-notes)
* [Deprecations](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.22.md#deprecation)
* [API changes](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.22.md#api-change-1)
* [Features](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.22.md#feature-2)
