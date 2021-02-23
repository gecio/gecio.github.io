---
title: Cluster Lebenszyklus
lang: de
permalink: /imke/managingprojects/clusterlifecycle/
nav_order: 4000
has_children: true
---

Das Management von iMKE-basierten Kubernetes-Clustern wird weitergehend von der iMKE-Plattform selbst übernommen. Jedoch gibt es einige Aufgaben im Lebenszyklus eines Clusters, die manuelle Aufmerksamkeit benötigen: Neben selbstverständlichen Dingen wie dem Erstellen oder auch Löschen eines nicht mehr benötigten Clusters zählen dazu auch gewisse Cluster-Updates, wie bspw. die Aktualisierung der Kubernetes-Version. Da solche Updates den rollierenden Restart der Worker-Nodes beinhalten, müssen diese (bspw. in einem Wartungsfenster Ihrer Wahl) manuell vorgenommen bzw. gestartet werden.

