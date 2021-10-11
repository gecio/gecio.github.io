---
title: iMKE Changelog v2.18
lang: en
permalink: /imke/about/changelog-v2.18/
nav_order: 900
parent: About iMKE
---

## Supported Kubernetes versions

With the current release, we support the following Kubernetes versions:

* 1.19.15
* 1.20.11
* 1.21.5

## End of Life announcements

We will end the support of Kubernetes version 1.19 on the 3rd November 2021.

Please make sure to upgrade all existing clusters running 1.19 at least to 1.20 before that date.

## New features

* [Cluster Templates](../../clusterlifecycle/clustertemplates/) are now supported
* Older clusters (created before 1.17) can now be [migrated to the external cloud controller manager](../../clusterlifecycle/clustermigrations/externalcloudprovider)
* `containerd` is now supported as container runtime ([How to migrate your clusters to containerd](../../clusterlifecycle/clustermigrations/containerruntimeengine/))

## Bugfixes

* Updated supported Kubernetes versions to newest patch-levels to mitigate [CVE-2021-25741](https://github.com/kubernetes/kubernetes/issues/104980). Cluster control planes have been automatically updated. Please update your Machine Deployments soon, at a time convenient for you. Updating your Machine Deployment will lead to worker node recreation and rolling restarts of your Deployments and Statefulsets.

## Kubernetes related changes

### Upgrade notes for Kubernetes 1.21

If you are planning to upgrade to Kubernetes 1.21, please have a look at the [What's new](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.21.md#whats-new-major-themes) section of the official Kubernetes v1.21 release notes and make sure you familiarise yourself with the upcoming changes.

For an overview of the changes, please refer to the [Changes by Kind](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.21.md#changes-by-kind-2) section of the Changelog.

* [Urgent Upgrade Notes](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.21.md#urgent-upgrade-notes)
* [Deprecations](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.21.md#deprecation)
* [API changes](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.21.md#api-change-1)
* [Features](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.21.md#feature-2)