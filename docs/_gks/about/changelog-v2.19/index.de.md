---
title: GKS Changelog v2.19
lang: "de"
permalink: /gks/about/changelog-v2.19/
nav_order: 800
parent: Über GKS
---

## Unterstützte Kubernetes Versionen

Im Rahmen des aktuellen Release werden die folgenden Kubernetes-Versionen unterstützt:

* 1.21.8
* 1.22.5

## End of Life Ankündigungen

Wir werden die Unterstützung der Kubernetes Version v1.21 am 28.06.2022 beenden.

Bitte updaten Sie alle bestehenden Cluster mit Kubernetes Version 1.21 auf mindestens 1.22 bis zu diesem Datum.

## Neue Funktionen

* CNI Update Funktionalität: es ist nun möglich die CNI canal [im dashboard upzudaten](/gks/somepath/to/the/new/docs/)

## Bugfixes

* Um eine Reihe von CVEs ([CVE-2021-44716](https://www.cve.org/CVERecord?id=CVE-2021-44716), [CVE-2021-44717](https://www.cve.org/CVERecord?id=CVE-2021-44717), [CVE-2021-3711](https://www.cve.org/CVERecord?id=CVE-2021-3711), [CVE-2021-3712](https://www.cve.org/CVERecord?id=CVE-2021-3712), [CVE-2021-33910](https://www.cve.org/CVERecord?id=CVE-2021-33910)) zu mitigieren wurden die unterstützten Kubernetes-Versionen aktualisiert. Die Controlplanes der Cluster wurden automatisch upgedated. Bitte stellen Sie ein zeitnahes Updates Ihrer Machine Deployments sicher. Bei einem Update der Machine Deployments kann es durch den rollierenden Neustart der Worker-Nodes zu rollierenden Restarts der Deployments und Statefulsets kommen.

## Änderungen in Kubernetes

### Upgrade-Hinweise für Kubernetes 1.22

Wenn Sie ein Upgrade auf Kubernetes 1.22 planen, lesen Sie bitte den Abschnitt [What's New](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.22.md#whats-new-major-themes) des offiziellen Kubernetes v1.22 Changelogs und machen Sie sich mit den bevorstehenden Änderungen vertraut. Es werden diesmal sehr viele beta-Kubernetes APIs entfernt, welches potenziell Änderungen in ihren Software-Ausroll-Prozess zur Folge hat.

Eine Übersicht über die Änderungen finden Sie im Abschnitt [Changes by Kind](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.22.md#changes-by-kind-2) des Changelogs.

* [Wichtige Hinweise zum Upgrade](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.22.md#urgent-upgrade-notes)
* [Deprecations](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.22.md#deprecation)
* [API-Änderungen](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.22.md#api-change-1)
* [Features](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.22.md#feature-2)
