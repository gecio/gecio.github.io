---
title: Machine Deployments
lang: "de"
permalink: /gks/machinedeployments/
nav_order: 6000
has_children: true
---
<!-- LTeX:  language=de-DE -->
# Machine Deployments

Ein Kubernetes-Cluster besteht vereinfacht gesagt aus der Controlplane (dort läuft u. a. die Kubernetes-API, die etcd Datenbank und andere Steuerungskomponenten) und sogenannten Worker-Nodes - (virtuellen) Servern, auf denen die eigentliche Applikations-Lasten gestartet werden.

Die Controlplane eines GKS Clusters wird dabei von der Plattform selbst verwaltet, Kunden können nicht direkt auf diese zugreifen.

Dagegen können die Worker-Nodes sehr wohl auf verschiedenste Arten konfiguriert werden. Der Kunde kann dabei u.a. das Folgende konfigurieren:

* *Node-Flavors*: Die Maschinentypen, die als Worker-Nodes benutzt werden. Diese Typen unterscheiden sich typischerweise in ihrer Ausstattung bzgl. CPU und RAM.
* *Anzahl der Worker-Nodes*: Hier kann konfiguriert werden, wie viele Server der o.g. Größe den Cluster bilden sollen.
* *SSH-Keys*: Diese können für den Cluster aktiviert werden, so dass Sie sich auf den Worker-Nodes einloggen können. Dafür ist es weiterhin notwendig, dass die Nodes eine Public IP (Floating IP) besitzen, damit Sie diese erreichen können. Ein Login per SSH kann beispielsweise im Rahmen eines intensiven Debuggings der Applikationen hilfreich sein.
* *Betriebssystem der Worker-Nodes*: Auch wenn die Wahl des Betriebssystem irrelevant für die auf Kubernetes laufenden Applikationen ist, könnte es im Kontext des Debuggings eine Rolle spielen.

## Weiterführende Themen

* [Sizing: Node Flavors konfigurieren](/gks/machinedeployments/nodeflavors/)
* [Debugging: Auslastung der Cluster Nodes](/gks/machinedeployments/clusternodesusagerate/)
* [Debugging: SSH-Keys hinzufügen](/gks/machinedeployments/add_ssh_key/)
* [Aktualisierung des Betriebssystems auf Worker-Nodes](/gks/machinedeployments/updatingnodeos/)
