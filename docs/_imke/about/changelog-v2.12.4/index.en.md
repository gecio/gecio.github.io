---
title: iMKE Changelog v2.12.4
lang: en
permalink: /imke/about/changelog-v2.12.4/
nav_order: 1300
parent: About iMKE
---

iMKE upgraded from v2.11.6 → v2.12.4

## Supported Kubernetes versions

Kubernetes clusters will be upgraded as follows in order to deploy bugfixes and security fixes.

v1.13.x -> v1.13.12

v1.14.x -> v1.14.8

v1.15.x -> v1.15.5

- Customers can now create Kubernetes clusters with version v1.16.x.
- Kubernetes 1.13 which is end-of-life has been removed.

## Major new features

- Kubernetes 1.16 support was added
- Removed K8S releases affected by CVE-2019-11253
- Added possibility to add labels to projects and clusters and have these labels inherited by node objects.
- Added support for Kubernetes audit logging
- "Connect" button on cluster details will now open Kubernetes Dashboard
- Pod Security Policies can now be enabled

## Dashboard

- Kubernetes 1.13 which is end-of-life has been removed.
- Redesign dialog to manage SSH keys on cluster
- Redesign Wizard: Summary
- Cluster type toggle in wizard is now hidden if only one cluster type is active
- Disabled the possibility of adding new node deployments until the cluster is fully ready.
- The cluster name is now editable from the dashboard
- Added warning about node deployment changes that will recreate all nodes.
- Pod Security Policy can now be activated from the wizard
- Redesigned extended options in wizard
- Various security improvements in authentication
