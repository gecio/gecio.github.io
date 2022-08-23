---
title: Cluster Lebenszyklus
lang: de
permalink: /gks/clusterlifecycle/
nav_order: 4000
has_children: true
---
<!-- LTeX:  language=de-DE -->
# Cluster Lebenszyklus

Das Verwalten von GKS-basierten Kubernetes-Clustern wird weitestgehend von der GKS-Plattform selbst übernommen. Jedoch gibt es einige Aufgaben im Lebenszyklus eines Clusters, die manuell gemacht werden müssen. Neben selbstverständlichen Dingen wie das Erstellen oder Löschen eines nicht mehr benötigten Clusters zählen dazu auch gewisse Cluster-Updates, wie zum Beispiel die Aktualisierung der Kubernetes-Version. Da solche Updates den rollierenden Neustart der Worker-Nodes beinhalten, müssen diese (zum Beispiel in einem Wartungsfenster Ihrer Wahl) manuell vorgenommen bzw. gestartet werden.

## Weiterführende Themen

* [Einen Cluster anlegen](/gks/clusterlifecycle/creatingacluster/)
* [Cluster Templates](/gks/clusterlifecycle/clustertemplates/)
* [Einen Cluster aktualisieren/Kubernetes-Updates](/gks/clusterlifecycle/upgradingacluster/)
* [CNI Auswahl](/gks/clusterlifecycle/cnichoices/)
* [CNI Updates](/gks/clusterlifecycle/upgradingcni/)
* [Anbindung der Controlplane](/gks/clusterlifecycle/controlplaneconnector/)
* [Openstack-Credentials ändern](/gks/clusterlifecycle/openstackcredentials/)
* [Einen Cluster löschen](/gks/clusterlifecycle/deletingacluster/)
* [Deprecation Policy](/gks/clusterlifecycle/deprecationpolicy/)
* [Migration Container Runtime Engine](/gks/clusterlifecycle/containerruntimeengine/)
