---
title: Cluster Lebenszyklus
lang: de
permalink: /gks/clusterlifecycle/
nav_order: 4000
has_children: true
---
<!-- LTeX:  language=de-DE -->

Das Management von gks-basierten Kubernetes-Clustern wird weitergehend von der gks-Plattform selbst übernommen. Jedoch gibt es einige Aufgaben im Lebenszyklus eines Clusters, die manuelle Aufmerksamkeit benötigen: Neben selbstverständlichen Dingen wie dem Erstellen oder auch Löschen eines nicht mehr benötigten Clusters zählen dazu auch gewisse Cluster-Updates, wie bspw. die Aktualisierung der Kubernetes-Version. Da solche Updates den rollierenden Restart der Worker-Nodes beinhalten, müssen diese (bspw. in einem Wartungsfenster Ihrer Wahl) manuell vorgenommen bzw. gestartet werden.

**Weiterführende Themen**
* [Einen Cluster anlegen](/gks/clusterlifecycle/creatingacluster/)
* [Einen Cluster aktualisieren / Kubernetes-Updates](/gks/clusterlifecycle/upgradingacluster/)
* [Einen Cluster aktualisieren / CNI-Updates](/gks/clusterlifecycle/upgradingcsi/)
* [Einen Cluster aktualisieren / Openstack-Credentials ändern](/gks/clusterlifecycle/openstackcredentials/)
* [Einen Cluster löschen](/gks/clusterlifecycle/deletingacluster/)
* [Deprecation Policy in GKS](/gks/clusterlifecycle/deprecationpolicy/)
* [Eine CNI wählen](/gks/clusterlifecycle/cnichoices/)
* [Informationen über die Anbindung der Control-Plane](/gks/clusterlifecycle/controlplaneconnector/)
