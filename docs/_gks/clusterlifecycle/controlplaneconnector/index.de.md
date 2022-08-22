---
title: Controlplane Konnektor
lang: de
permalink: /gks/clusterlifecycle/controlplaneconnector/
nav_order: 4280
parent: Cluster Lebenszyklus
---

# Controlplane Konnektor

Diese Seite bietet eine Übersicht darüber, wie die Controlplane eines GKS Kubernetes Clusters
mit seinen Worker-Nodes kommuniziert.

## Cluster Aufbau

Die GKS Plattform stellt verwaltete Kubernetes Dienste für Endkunden zur Verfügung. Aus Gründen
der Verfügbarkeit, Sicherheit und Stabilität wird die Controlplane getrennt von den
Worker-Nodes betrieben. Der Endkunde hat die vollständige Kontrolle über die Worker-Nodes (bis hinab
auf Betriebssystemebene der Kubernetes Worker), welche in deren eigenem Openstack Tenant laufen.
Die Controlplane hingegen wird vom Service-Erbringer in dessen eigenem dedizierten Openstack-Tenant
betrieben. Trotz dieser Trennung muss weiterhin gewährleistet sein, dass Controlplane und Worker
miteinander kommunizieren können.

### Kommunikation initiiert von den Kubernetes Workern

Diese Richtung der Kommunikation ist einfach realisiert, da die Controlplane ihre Dienste über den
Kubernetes API Server anbietet, der über eine öffentliche IPv4 Adresse erreichbar ist, die den
Kubernetes Workern bekannt ist. Die Netzwerkpakete verlassen zum Initiieren der Verbindung
das private Openstack-Subnetz über den egress-Router des Subnetzes in Richtung Internet. Anschließend betreten sie über einen exponierten
Loadbalancer das private Subnetz des Openstack-Tenants vom Servicebetreiber.

Allerdings ist es manchmal auch notwendig, dass der Kubernetes API Server die Verbindung zu Diensten
auf den Worker-Nodes initiiert (hauptsächlich zu den kubelets), vor allem wenn Befehle
wie `kubectl port-forward`, `kubectl exec` oder `kubectl logs` ausgeführt werden sollen. Die
Worker-Nodes sind **nicht** über einen öffentlichen Endpunkt erreichbar (was aus Sicherheitsperspektive
 sehr sinnvoll ist).

### Kommunikation initiiert von der Controlplane

Der Trick ist, bereits von Anfang an, einen VPN-Tunnel von den Worker-Nodes aufzubauen und darüber
dafür zu sorgen, dass die Controlplane die Worker-Nodes jederzeit erreichen kann. Die Konfiguration
dazu entsteht bereits bei der Erstellung des Clusters.

Die bisherige von uns benutzte Lösung implementierte diesen Tunnel mittels der openVPN Software.
Diese Lösung hatte jedoch einige Nachteile, so dass ein anderes speziell dafür entwickeltes Tool
dessen Aufgabe übernahm, die sogenannte *Konnectivity*. Es ermöglicht einen deutlich stabileren Aufbau mit
einfacheren Mitteln und wird von der Kubernetes Community entwickelt und gepflegt.

## Installation/Migration

Wenn ein neuer Kubernetes Cluster auf der GKS Plattform erstellt wird, enthält er bereits
automatisch Konnectivity.

Bestehende Cluster müssen einmalig im GKS Dashboard unter dem Menüpunkt "Edit cluster" einen
Haken in die Box für Konnectivity setzen und speichern. Danach läuft ein Automatismus los und
führt nahtlos die Migration von openVPN hin zu Konnectivity vor.

## Weiterführende Themen

* [konnectivity](https://kubernetes.io/docs/concepts/architecture/control-plane-node-communication/#konnectivity-service)
* [Github repo für die Specifization](https://github.com/kubernetes-sigs/apiserver-network-proxy)
