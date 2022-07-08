---
title: GKS-Projekte verwalten
lang: de
permalink: /gks/managingprojects/
nav_order: 3000
has_children: true
---
<!-- LTeX:  language=de-DE -->

Ein GKS-Projekt ist eine Abstraktionsschicht, um mehrere Kubernetes
Cluster gemeinsam verwalten zu können.

Alle Kubernetes-Cluster in einem GKS-Projekt:

* haben die *gleichen Benutzer* ("Member" des Projektes). Die für das Projekt freigeschalteten
  Benutzer können sich über das GKS-Frontend anmelden, alle im Projekt vorhandenen Cluster
  (je nach Zugriffslevel) sehen bzw. administrieren und sich auch eine `kubeconfig` für den direkten Zugriff auf das Cluster herunterladen.
* haben die *gleichen Service Accounts*, die auf die GKS REST API mit einem API-Token
  zugreifen können.
* können im Projekt hinterlegte *SSH-Keys* zugewiesen haben.

Projekte sind im o.g. Sinn nur für die Verwaltung wichtig und haben technisch weiter keine
Auswirkungen auf die enthaltenen Kubernetes Cluster.

**Weiterführende Themen**
* [Ein GKS-Projekt anlegen](/gks/managingprojects/creatingaproject/)
* [Benutzer-Management in GKS-Projekten](/gks/managingprojects/projectusermanagement/)
* [Service-Accounts in GKS-Projekten](/gks/managingprojects/projectserviceaccounts/)
