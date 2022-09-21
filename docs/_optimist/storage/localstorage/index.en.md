---
title: Localstorage
lang: de
permalink: /optimist/storage/localstorage/
nav_order: 3200
parent: Storage
has_children: true
---

Compute Localstorage für Ihre Instanzen
=================================================

Was genau ist Compute Localstorage?
-----

Mit Localstorage ist hier gemeint das sich der Storage Ihrer Instanzen direkt auf dem Hypervisor (Server) befindet. Das Localstorage feature, nutzbar über unsere l1 Flavors, sind für Anwendungen gedacht die geringe Latenz erfordern.

Datensicherheit und Verfügbarkeit
-----

liegt direkt auf dem server, unterliegt wartungsfenster, während wartungsfenster down, HA über AZs vom Kunden zu realisieren

Openstack Features
-----

Resize nicht möglich
snapshot möglich aber nicht empfohlen
shelving möglich aber nicht empfohlen

Löschung der Instanz
-----

Shred / zeroing (laut iso müssen wir glaube shred)


Apendix: Flavor Benchmarks
-----

"Bunte Bilder"
