---
title: iMKE Changelog v2.13.10
lang: de
permalink: /imke/about/changelog-v2.13.10/
nav_order: 1200
parent: Über iMKE
---

iMKE aktualisiert von v2.12.4 → v2.13.10

## Unterstützte Kubernetes Versionen

Kubernetes-Cluster werden wie folgt aktualisiert, um Bugfixes und Sicherheitskorrekturen bereitzustellen

v1.15.x -> v1.15.10

v1.16.x -> v1.16.13

v1.17.x -> v1.17.9

- Kunden können jetzt Kubernetes-Cluster mit Version 1.17.x erstellen
- Kubernetes v1.14 wurde eingestellt.

## Wichtige neue Funktionen

- Die `authorized_keys` Dateien auf den Nodes werden geupdatet, falls Änderungen in den SSH-Keys im Cluster auftreten.
- Unterstützung von benutzerdefinierten Zertifizierungsstellen (Custom CA) für OpenID-Provider in der API.
- Benutzereinstellungen sind jetzt im Benutzerbereich verfügbar.
- Cluster-Addon wurde in der graphischen Oberfläche hinzugefügt.
- MachineDeployments können jetzt konfiguriert werden, damit [Dynamic Kubelet Configuration](https://kubernetes.io/blog/2018/07/11/dynamic-kubelet-configuration/) aktiviert wird.
- RBAC Verwaltungsfunktion wurde in der Oberfläche hinzugefügt.

## Updates im Cloud-Provider

- OpenStack: Ein Bug hat dazu geführt, dass die Wiederherstellung eines Clusters fehlschlägt – ein Absturz an der falschen Stelle war die Ursache. Status: Behoben.
- OpenStack: Neue Kubernetes-Cluster verwenden externe [Cloud Controller Manager](https://kubernetes.io/docs/tasks/administer-cluster/running-cloud-controller/) und [CSI](https://kubernetes.io/blog/2019/01/15/container-storage-interface-ga/)
- OpenStack: die verteilten Routers werden nun in der Router-Suche inbegriffen

## Bugfixes

- Der Fehler des Master-Controllers bei der Erstellung von Projekt-Label-Synchronizer-Controllern wurde behoben.
- Der defekte NodePort-Proxy für Nutzercluster mit LoadBalancer Expose-Strategy wurde gefixt.
- Es wurde behoben, dass Cluster-Namespaces nach dem Entfernen im Terminating-Status hängen bleiben.
- Der Seed-Validierung-Webhook hat bestimmte neue Seeds in bestimmten Situationen nicht aktziptiert. Status: Behoben.
- Security-Fix für den Fall, welcher auf den Clustern Credentials- und CredentialsSecret-Mängel auftreten könnte.
- Ein Bug hat in manchen Fällen zu folgendem Fehler im UI geführt: `Error: no matches for kind- "MachineDeployment" in version "cluster.k8s.io/v1alpha1"` Status: Behoben
- Es wurde ein Memory Leak in der Port-Forwarding des Kubernetes Dashboards gefixt
- Es wurde gefixt, dass ein Bug die funktionierenden Cluster im Seed nicht im Dashboard dargestellt hat.
- Es wurde gefixt, dass die Entfernung der System-Label im Cluster nach einer Änderung blockiert wurde.
- Das Entfernen der ausgesuchten Addons von den Clustern wurde gefixt.
- Nodename-Validierung während des Aufsetzens der Cluster und Node-Deployment wurde gefixt.
- Swagger und API-Client für das Erstellen von SSH keys wurden gefixt
- Ein Bug wurde gefixt, welcher die Bearbeitung von bestehenden Cluster-Credentials verhindert hat
- Die Einstellung `componentsOverride` eines Clusters beeinflusst nun keine anderen Clusters
