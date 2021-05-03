---
title: Machine Deployments
lang: de
permalink: /imke/machinedeployments/
nav_order: 5000
has_children: true
redirect_from:
  - /imke/nodedeployments/
---

Ein Kubernetes-Cluster besteht vereinfacht gesagt aus der Controlplane (dort läuft u.a. die Kubernetes-API, die etcd Datenbank und andere Steuerungskomponenten) und sogenannten Worker-Nodes - (virtuellen) Servern, auf denen die eigentlichen Kubernetes-Applikationen gestartet werden.

Die Controlplane eines iMKE-Clusters wird dabei von der Plattform selbst verwaltet, Kunden können nicht direkt auf diese zugreifen.

Dagegen können die Worker-Nodes sehr wohl auf verschiedenste Arten konfiguriert werden. Der Kunde kann dabei u.a. das Folgende konfigurieren:

* *Node-Flavors* sind die Maschinentypen, die als Worker-Nodes benutzt werden. Diese Typen unterscheiden sich typischerweise in ihrer Ausstattung bzgl. CPU und RAM.
* Über die *Anzahl der Worker-Nodes* kann konfiguriert werden, wieviele Server der o.g. Größe das Cluster bilden sollen.
* *SSH-Keys* können für das Cluster aktiviert werden, so dass Sie sich auf den Worker-Nodes einloggen können. Dafür ist es weiterhin notwendig, dass die Nodes eine Public IP (Floating IP) besitzen, damit Sie diese erreichen können. Ein Login per SSH kann beispielsweise im Rahmen eines intensiven Debuggings der Kubernetes-Applikationen hilfreich sein.
* *Betriebssystem der Worker-Nodes*: Auch wenn die Wahl des Betriebssystem irrelevant für die auf Kubernetes laufenden Applikationen ist, könnte es für Sie im Kontext des Debuggings eventuell eine Rolle spielen.

**Weiterführende Themen**
* [Sizing: Node Flavors konfigurieren](/imke/machinedeployments/nodeflavors/)
* [Debugging: Auslastung der Cluster-Nodes](/imke/machinedeployments/clusternodesusagerate/)
* [Debugging: SSH-Keys hinzufügen](/imke/machinedeployments/add_ssh_key/)
* [Aktualisierung des Betriebssystems auf Worker-Nodes](/imke/machinedeployments/upgradingnodeos/)
