---
title: iMKE-Projekte verwalten
lang: de
permalink: /imke/managingprojects/
nav_order: 3000
has_children: true
---

Ein iMKE-Projekt ist eine Abstraktionsschicht, um mehrere Kubernetes
Cluster gemeinsam verwalten zu können.

Alle Kubernetes-Cluster in einem iMKE-Projekt:

* haben die *gleichen Benutzer* ("Member" des Projektes). Die für das Projekt freigeschalteten
  Benutzer können sich über das iMKE-Frontend anmelden, alle im Projekt vorhandenen Cluster
  (je nach Zugriffslevel) sehen bzw. administrieren und sich auch eine `kubeconfig` für den direkten Zugriff auf das Cluster herunterladen.
* haben die *gleichen Service Accounts*, die auf die iMKE REST API mit einem API-Token
  zugreifen können.
* können im Projekt hinterlegte *SSH-Keys* zugewiesen haben.

Projekte sind im o.g. Sinn  nur für die Verwaltung wichtig und haben technisch weiter keine
Auswirkungen auf die enthaltenen Kubernetes Cluster.

**Weiterführende Themen**
* [Ein iMKE-Projekt anlegen](/imke/managingprojects/creatingaproject/)
* [Benutzer-Management in iMKE-Projekten](/imke/managingprojects/projectusermanagement/)
* [Service-Accounts in iMKE-Projekten](/imke/managingprojects/projectserviceaccounts/)
