---
title: iMKE Changelog v2.12.4
lang: de
permalink: /imke/about/changelog-v2.12.4/
nav_order: 1300
parent: Über iMKE
---
<!-- LTeX:  language=de-DE -->

iMKE aktualisiert von v2.11.6 → v2.12.4

## Unterstützte Kubernetes Versionen

Kubernetes-Cluster werden wie folgt aktualisiert, um Bugfixes und Sicherheitskorrekturen bereitzustellen

v1.13.x -> v1.13.12

v1.14.x -> v1.14.8

v1.15.x -> v1.15.5

- Kunden können jetzt Kubernetes-Cluster mit Version 1.16.x erstellen
- Das auslaufende Kubernetes 1.13 wurde entfernt

## Wichtige neue Funktionen

- Unterstützung für Kubernetes 1.16 wurde hinzugefügt
- K8S-Versionen, die von CVE-2019-11253 betroffen sind, wurden entfernt
- Möglichkeit hinzugefügt, Beschriftungen zu Projekten und Clustern hinzuzufügen und diese Beschriftungen an Knotenobjekte zu vererben
- Unterstützung für die Kubernetes-Überwachungsprotokollierung hinzugefügt
- Die Schaltfläche "Verbinden" in den Clusterdetails öffnet jetzt das Kubernetes Dashboard
- Pod-Sicherheitsrichtlinien können jetzt aktiviert werden

## Dashboard

- Das auslaufende Kubernetes 1.13 wurde entfernt
- Neugestaltung des Dialogfelds zur Verwaltung von SSH-Schlüsseln im Cluster
- Redesign-Assistent: Zusammenfassung
- Das Umschalten des Clustertyps im Assistenten ist jetzt ausgeblendet, wenn nur ein Clustertyp aktiv ist
- Deaktivierung der Möglichkeit, neue Knotenbereitstellungen hinzuzufügen bis der Cluster vollständig bereit ist
- Der Clustername kann jetzt über das Dashboard bearbeitet werden
- Hinzugefügte Warnung über Änderungen an der Knotenbereitstellung, welche alle Knoten neu erstellen
- Die Pod-Sicherheitsrichtlinie kann jetzt über den Assistenten aktiviert werden
- Erweiterte Optionen im Assistenten überarbeitet
- Verschiedene Sicherheitsverbesserungen bei der Authentifizierung
