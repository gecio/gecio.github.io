---
title: Control-Plane Konnektor
lang: de
permalink: /imke/clusterlifecycle/controlplaneconnector/
nav_order: 4250
parent: Cluster Lebenszyklus
---

# Control-Plane Konnektor

Diese Seite bietet eine Übersicht darüber wie die Control-Plane eines iMKE Kubernete Clusters
mit seinen worker nodes kommuniziert.


# Cluster Aufbau

Die iMKE Plattform stellt gemanage'te Kubernetes Dienste für Endkunden zur Verfügung. Aus Gründen
der Verfügbarkeit, Sicherheit und Stabilität wird die Control-Plane dabei getrennt von den
Worker-Knoten betrieben. Der Endkunde hat vollständige Kontrolle über die Worker-Knoten (bis hinab
auf Betriebssystemebene der Kubernetes Worker) welche in dessen eigenem Openstack Tenant laufen.
Die Control-Plane wird indes vom Service-Erbringer in dessen eigenem dedizierten Openstack-Tenant
betrieben. Trotz dieser Trennung muss weiterhin gewährleistet sein dass Contol-Plane und Worker
miteinander kommunizieren können.


## Kommunikation initiiert von den Kubernetes Workern

Diese Richtung der Kommunikation ist einfach realisiert da die Control-Plane ihre Dienste über den
Kubernetes api-server anbietet, der über eine öffentliche IPv4 Adresse erreichbar ist, die den
Kubernetes Workern wohlbekannt ist. Die Netzwerkpakete verlassen zum initiieren der Verbindung
also das private Openstack-Subnetz über den egress-Router des Subnetzes in Richtung Internet und
betreten dann das private Subnetz des Openstack-Tenant des Servicebetriebers über einen exponierten
Loadbalancer.

Allerdings ist es manchmal auch notwendig dass der Kubernetes API-server die Verbindung zu Diensten
auf den Worker-Knoten initiieren muss (hauptsächlich zu den kubelet's), vor allem wenn Befehle
wie "kubectl port-forward", "kubectl exec" oder "kubectl logs" ausgeführt werden sollen. Die
Worker-Knoten sind **nicht** über einen öffentlichen Endpunkt erreichbar (was aus Sicherheitsperspektive
eine sehr sinnvolle Sache ist!).


## Kommunikation initiiert von der Control-Plane

Der Trick ist bereits von Anfang an einen VPN-Tunnel von den Worker-Knoten aufzubauen und darüber
dafür zu sorgen dass die Control-Plane die Worker-Knoten jederzeit erreichen kann. Die Konfiguration
dazu entsteht bereits bei der Erstellung des Clusters.

Die bisherige von uns benutzte Lösung implementierte diesen Tunnel mittels der openVPN software.
Diese Lösung hatte jedoch einige Nachteile so dass ein anderes speziell dafür entwickeltes Tool
dessen Aufgabe übernahm *Konnectivity*. Es ermöglicht einen deutlich stabileren Aufbau mit
deutlich einfacheren Mitteln und wird von der Kubernetes Community entwickelt und gepflegt.


# Installation/Migration

Wenn ein neuer Kubernetes Cluster auf der iMKE Plattform erstellt wird, dann enthält er bereits
automatisch Konnectivity.

Bestehende Cluster müssen einmalig im iMKE-Dashboard unter dem Menüpunkt "Edit cluster" einen
Haken in die Box für Konnectivity setzen und speichern. Danach läuft ein Automatismus los und
führt nahtlos die Migration von openVPN hin zu Konnectivity vor.


# Weitere Quellen

* [konnectivity](https://kubernetes.io/docs/concepts/architecture/control-plane-node-communication/#konnectivity-service)
* [Github repo für die Specifization](https://github.com/kubernetes-sigs/apiserver-network-proxy)
