---
title: GKS Changelog v2.21
lang: de
permalink: /gks/about/changelog-v2.21/
nav_order: 600
parent: Über GKS
---

## Unterstützte Kubernetes Versionen

Im aktuellen Release werden die folgenden Kubernetes-Versionen unterstützt:

* 1.22.15
* 1.23.12
* 1.24.6

## End of Life Ankündigungen

Wir werden die Unterstützung der Kubernetes Version v1.21 am 18.10.2022 beenden.

Führen Sie bei allen bestehenden Clustern mit Kubernetes Version 1.22 ein Update auf mindestens Version 1.23 bis zu diesem Datum durch.

## Neue Funktionen

### Projektübersichtsseite

Es gibt eine neue Übersichtsseite für Projekte, die all Projektinformationen Übersichtlich anzeigt.

Sollten sie das alte Verhalten bevorzugen, können sie sich das Userprofil umstellen.

### New CNI Versionen

Canal wurde auf 3.23 und Cilium auf 1.12 updated. Bitte updaten sie ihr CNI in Ihren Clustern.

## Änderungen in Kubernetes

### Upgrade-Hinweise für Kubernetes 1.24

Wenn Sie ein Upgrade auf Kubernetes 1.24 planen, lesen Sie bitte im offiziellen Kubernetes v1.24 Changelog den Abschnitt [What's New](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.24.md#whats-new-major-themes) und machen Sie sich mit den bevorstehenden Änderungen vertraut. Es werden diesmal sehr viele beta-Kubernetes APIs entfernt, was potenziell Änderungen im Software-Ausroll-Prozess zur Folge hat.

Eine Übersicht über die Änderungen finden Sie im Changelog in Abschnitt [Changes by Kind](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.24.md#changes-by-kind-2).

* [Wichtige Hinweise zum Upgrade](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.24.md#urgent-upgrade-notes)
* [Deprecations](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.24.md#deprecation)
* [API-Änderungen](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.24.md#api-change-4)
* [Features](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.24.md#feature-7)
