---
title: Managing GKS-Projects
lang: en
permalink: /gks/managingprojects/
nav_order: 3000
has_children: true
---

An GKS-project is an abstraction layer to manage a set of clusters.

All clusters in an GKS-project:

* share the *same users* ("members" of that project), who can be either
  administrators or have read-only rights. A project member can always log in to the
  GKS-Dashboard and will be able to download a `kubeconfig` for accessing all
  clusters in that project directly.
* share the *same GKS service-accounts*, which can access the GKS API
  via a REST API and API token.
* can *have SSH-Keys assigned* to them, which are organized via the project.

Projects are only required for management and have no impact
on how you can use the Clusters within. You can have multiple projects.

**Further reading**
* [How to create an GKS-Project](/gks/managingprojects/creatingaproject)
* [How to manage users in an GKS-Project](/gks/managingprojects/projectusermanagement)
* [How to manage service accounts in an GKS-Project](/gks/managingprojects/projectserviceaccounts/)
