---
title: Managing iMKE-Projects
lang: en
permalink: /imke/managingprojects/
nav_order: 3000
has_children: true
---

An iMKE-project is an abstraction layer to manage a set of clusters.

All clusters in an iMKE-project:

* share the *same users* ("members" of that project), who can be either
  administrators or have read-only rights. A project member can always log in to the
  iMKE-Dashboard and will be able to download a `kubeconfig` for accessing all
  clusters in that project directly.
* share the *same iMKE service-accounts*, which can access the iMKE API
  via a REST API and API token.
* can *have SSH-Keys assigned* to them, which are organized via the project.

Projects are only required for management and have no impact
on how you can use the Clusters within. You can have multiple projects.

**Further reading**
* [How to create an iMKE-Project](/imke/managingprojects/creatingaproject))
* [How to manage users in an iMKE-Project](/imke/managingprojects/projectusermanagement))
* [How to manage service accounts in an iMKE-Project](/imke/managingprojects/projectserviceaccounts/)
