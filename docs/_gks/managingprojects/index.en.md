---
title: Managing GKS Projects
lang: en
permalink: /gks/managingprojects/
nav_order: 3000
has_children: true
---
# Managing GKS Projects

A GKS project is an abstraction layer to manage a set of clusters.

All clusters in an GKS project:

* Share the *same users* ("members" of that project), who can be either
  administrators or have read-only rights. A project member can always log in to the
  GKS dashboard and will be able to download a `kubeconfig` for accessing all
  clusters in that project directly.
* Share the *same GKS service accounts*, which can access the GKS API
  with a REST API and API token.
* Can *have SSH Keys assigned* to them, which are organized via the project.

Projects are only required for management and have no impact
on how you can use the clusters within. You can have multiple projects.

## Learn More

* [How to create a GKS project](/gks/managingprojects/creatingaproject)
* [How to manage users in a GKS project](/gks/managingprojects/projectusermanagement)
* [How to manage service accounts in a GKS project](/gks/managingprojects/projectserviceaccounts/)
