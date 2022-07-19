---
title: GKS-Projekte verwalten
lang: de
permalink: /gks/managingprojects/
nav_order: 3000
has_children: true
---
<!-- LTeX:  language=de-DE -->
# GKS-Projekte verwalten

Ein GKS-Projekt ist eine Abstraktionsschicht, um mehrere Kubernetes
Cluster gemeinsam verwalten zu können.

Alle Kubernetes Cluster in einem GKS-Projekt:

* Haben die *gleichen Benutzer* ("Member" des Projekts). Die für das Projekt freigeschalteten
  Benutzer können sich über das GKS-Frontend anmelden und alle im Projekt vorhandenen Cluster
  (je nach Zugriffslevel) sehen bzw. administrieren. Sie können sich auch eine `kubeconfig`-Datei für den direkten Zugriff auf den Cluster herunterladen.
* Haben die *gleichen Service Accounts*, die auf die GKS REST API mit einem API-Token
  zugreifen können.
* Können im Projekt hinterlegte *SSH-Keys* zugewiesen haben.

Projekte sind im o.g. Sinn nur für die Verwaltung wichtig und haben technisch weiter keine
Auswirkungen auf die enthaltenen Kubernetes Cluster.

## Weiterführende Themen

* [Ein GKS-Projekt anlegen](/gks/managingprojects/creatingaproject/)
* [Benutzer-Management in GKS-Projekten](/gks/managingprojects/projectusermanagement/)
* [Service-Accounts in GKS-Projekten](/gks/managingprojects/projectserviceaccounts/)
