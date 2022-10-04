---
title: GKS Changelog v2.21
lang: en
permalink: /gks/about/changelog-v2.21/
nav_order: 600
parent: About GKS
---

## Supported Kubernetes Versions

With the current release, we support the following Kubernetes versions:

* 1.22.15
* 1.23.12
* 1.24.6

## End of Life Announcements

We will end the support of Kubernetes version 1.22 on October 10, 2022.

Make sure to upgrade all existing clusters running on version 1.22 at least to version 1.23 before that date.

## New Features

### Project overview site

We added a project overview page that gives an overview of the content of the project.

It is possible to change back to the old page in the user setting.

### New CNI Versionen

Canal was updated to v3.23 and Cilium to v1.12. Please update your CNI in the cluster.

## Kubernetes Related Changes

### Upgrade Notes for Kubernetes 1.24

If you are planning to upgrade to Kubernetes 1.24, refer to the [What's new](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.24.md#whats-new-major-themes) section of the official Kubernetes v1.24 release notes and make sure that you familiarise yourself with the upcoming changes.

For an overview of the changes, refer to the [Changes by Kind](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.24.md#changes-by-kind) section of the Changelog.

* [Urgent Upgrade Notes](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.24.md#urgent-upgrade-notes)
* [Deprecations](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.24.md#deprecation)
* [API changes](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.24.md#api-change-4)
* [Features](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.24.md#feature-7)
