---
title: iMKE Changelog v2.14.8
lang: de
permalink: /imke/about/changelog-v2.14.8/
nav_order: 1100
parent: Über iMKE
---

iMKE aktualisiert von v2.13.10 → v2.14.9

## Unterstützte Kubernetes Versionen

Kubernetes-Cluster werden wie folgt aktualisiert, um Bugfixes und Sicherheitskorrekturen bereitzustellen

v1.15.x -> v1.15.11

v1.16.x -> v1.16.13

v1.17.x -> v1.17.9

v1.18.x -> v1.18.6

- Kunden können jetzt Kubernetes-Cluster mit Version 1.18.x erstellen
- End-of-Life CoreOS wird nicht länger unterstützt (wurde ersetzt durch Flatcar)

## Neue Funktionen

- Support für Kubernetes 1.18 hinzugefügt
- Flatcar Linux als Betriebssystem für Worker-Nodes hinzugefügt
- RBAC Bindings können jetzt auch für Gruppen (groups) erstellt werden

## Updates im Cloud-Provider

- Openstack: Bereits existierenden Subnets die mit verteilten Routern verbunden sind können jetzt benutzt werden
- Openstack: Bugfix für das Ändern von Benutzernamen und Passwort für OpenStack cluster credentials

## Bugfixes

- Fehlerhafte Apiserver-Deployments behoben, wenn keine Dex-Zertifizierungsstelle konfiguriert wurde.
- Es wurde behoben, dass Secrets der Cluster-Anmeldeinformationen nicht ordnungsgemäß abgeglichen wurden.
- Swagger und API-Client für die Erstellung von SSH-Schlüsseln korrigiert
- Problem behoben, bei dem der Seed-Proxy-Controller nicht gestartet wurde
- Fehlenden Flatcar Linux-Support in der API hinzugefügt
- Behoben, dass Worker-Nodes manchmal nicht das Betriebssystem-Label hatten.
- Fehlende Prometheus-Metriken wurden ergänzt.
- Nodeport Proxy-Label bei Bereitstellung mit dem Operator korrigiert.
- RBAC-Rolle erstellt, damit kubeadm "Node"-Ressourcen abrufen kann. Diese Korrektur führt dazu, dass Worker-Nodes mit Kubeadm-Clustern verbunden werden können, auf denen Kubernetes 1.18+ ausgeführt wird.

## Änderungen in Kubernetes

### Was ist neu in Kubernetes 1.18

- Kubernetes Topology Manager wurde in die Beta Phase verschoben 
- Serverside Apply hat den Stand Beta 2 erreicht
- Ingress wurde um ein Feld `pathType` erweitert
- Die Ressource `IngressClass` wurde eingeführt
- Die neue Funktion `kubectl debug` wurde eingeführt
- Node Local DNSCache wurde auf GA abgestuft

### Upgrade-Hinweise für Kubernetes 1.18

Wenn Sie ein Upgrade auf Kubernetes 1.18 planen, lesen Sie bitte den Abschnitt [Upgrade Notes](https://v1-18.docs.kubernetes.io/docs/setup/release/notes/#urgent-upgrade-notes) des offiziellen Kubernetes v1.18 Changelogs und machen Sie sich mit den bevorstehenden Änderungen vertraut.

Eine Übersicht über die Änderungen finden Sie im Abschnitt [Changes by Kind](https://v1-18.docs.kubernetes.io/docs/setup/release/notes/#changes-by-kind) des Changelogs.

* [Deprecations](https://v1-18.docs.kubernetes.io/docs/setup/release/notes/#deprecation)
* [API-Änderungen](https://v1-18.docs.kubernetes.io/docs/setup/release/notes/#api-change)
* [Änderungen an den Konfigurationsdateien](https://v1-18.docs.kubernetes.io/docs/setup/release/notes/#configuration-file-changes)
* [Funktionen](https://v1-18.docs.kubernetes.io/docs/setup/release/notes/#feature)
* [Weitere Änderungen/Bugfixes](https://v1-18.docs.kubernetes.io/docs/setup/release/notes/#other-bug-cleanup-or-flake)
