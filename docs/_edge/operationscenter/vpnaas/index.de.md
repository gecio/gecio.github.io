---
title: VPN as a Service
lang: "de"
permalink: /edge/operationscenter/vpnaas
has_children: false
nav_order: 2100
parent: Operations Center
---

# VPN as a Service

## Übersicht

Die Kunden-OpenStack-Projekte und das Operations Center über das OpenStack Project VPN Gateway kommunizieren.
Das OpenStack Project VPN Gateway hat eine öffentliche IP-Adresse. Es authentifiziert und leitet den Datenverkehr über die verschiedenen Ports zum jeweiligen VPN-Server weiter.
In der Regel hat jedes Projekt einen VPN-Server.
In dieser Konfiguration hat jedes VPN-Gateway eine öffentliche IP-Adresse, die für alle VPN-Server gültig ist.

![VPNaaS Übersicht](./vpn_overview.png)

## Zweck

GEC bietet eine VPNaaS-Lösung, die es dem Kunden ermöglicht, seine Anwendungen und Systeme mit den Systemen der Projektpartner zu integrieren. Ein externer Partner ist entweder eine Person, die mit ihrem Computer auf das Projekt zugreift, oder ein Kommunikationssystem.

## Anforderungen

Die VPNaaS-Lösung hat folgende Anforderungen:

- Das Gateway benötigt eine öffentlich erreichbare IP welche mit der Floating-IP verbunden ist.

## Einschränkungen

Die VPNaaS-Lösung hat folgende Einschränkungen:

- Die Anzahl der VPN-Server ist durch den vordefinierten Portbereich im Gateway begrenzt.
- Die Anzahl der Benutzer in einem Netzwerk ist auf 250 begrenzt.
- Die Gateway- und Serverkonfiguration kann nach der Erstellung nicht geändert werden und muss neu erstellt werden.
- Nach der Neuerstellung sind alle OVPN-Konfigurationen ungültig.

## Komponenten und Kommunikationsfluss

Hier sehen Sie den Kommunikationsfluss zwischen dem Operations Center und den Kunden-VPN-Servern. Der Kunde sendet eine Anfrage, und das VPN-Gateway authentifiziert und verteilt die Anfragen an den jeweiligen VPN-Server.

![VPN-Komponenten und Kommunikationsfluss](./communicationflow.png)

### VPN-Gateway

Das VPN-Gateway ist der zentrale Verwaltungshub dec GEC.
Es wird verwendet, um VPN-Server zu verwalten und externe Verbindungen zuzulassen.
Da nur eine öffentliche IP-Adresse verfügbar ist, werden IPtable-Regeln verwendet, um sicherzustellen, dass die Verbindungen beim richtigen VPN-Server ankommen.
Hierfür ist ein Wide Area Network (WAN)-Netzwerk und ein mit dem VPN-Gateway verbundenes VPN Transfer Network erforderlich.
Das VPN-Gateway verwaltet nur VPN-Server und Proxyverbindungen zu und von den VPN-Servern, sodass der Datenverkehr nur dort endet, wo er benötigt wird.

### VPN-Server

Der VPN-Server verwendet OpenVPN für die VPN-Verbindung zum Kundenetzwerk.
Diese Netzwerkverbindung befindet sich in einem anderen Virtual Routing and Forwarding (VRF), um zusätzliche Isolation zu bieten, ohne andere Ports zu öffnen.
Externe Verbindungen werden über das VPN-Gateway geroutet.
Der API-Server für den VPN-Server verwaltet den auf der Maschine laufenden OpenVPN-Server.

## Sicherheit

### WAN

- Nur die für die vorhandenen VPN-Server erforderlichen Ports sind geöffnet.
- SSH wird nicht verwendet.
- Virtual Routing and Forwarding (VRF)-Trennung ist verfügbar.
- Sicherheitsgruppen erlauben nur die VPN-Server-Portbereiche.
- Firewall-Regeln sind vorhanden und werden bei Bedarf angepasst.

### VPNaaS

- Separates VRF zum WAN.
- Firewall-Regeln sind vorhanden und werden bei Bedarf angepasst.
- SSH und API sind verfügbar.
- SSH-Schlüssel werden verwendet.

## Server- und Benutzerverwaltung

### Serverstatus

1. Klicken Sie auf die VPN-Ressource.
2. Überprüfen Sie unter Eigenschaften den Status. Um die Konfiguration bearbeiten zu können, muss der Status aktiv sein.
![Benutzerressource](./vpnaas_active-resource.png)

### Benutzer hinzufügen

*Hinweis*: Der Projektinhaber wird automatisch für den Remotezugriff hinzugefügt und kann nicht entfernt werden.

*Voraussetzungen*: Sie können nur Benutzer auswählen, die zuvor dem Projekt hinzugefügt wurden.

1. Gehen Sie zur VPN-Konfiguration.
2. Wählen Sie den Benutzer aus, den Sie als VPN-Benutzer hinzufügen möchten.
![Benutzererstellung](./vpnaas_select-new-user.png)

3. Klicken Sie auf Speichern. Der Benutzer wird sofort hinzugefügt.
![Bestätigen Sie die Benutzererstellung](./vpnaas_save-new-user.png)

### OVPN-Konfiguration und Passphrase erhalten

*Hinweis*: Sie können die OVPN-Konfiguration nur für sich selbst oder als Projektinhaber für externe Benutzer herunterladen und eine Passphrase anzeigen. Sie können die OVPN-Konfiguration oder Passphrasen für andere Benutzer nicht herunterladen.

1. Gehen Sie zur VPN-Konfiguration.
2. Klicken Sie auf den Welt-Icon-Link und wählen Sie zwischen Download und Passphrase.
![VPN-Konfiguration erhalten](./vpnaas_open-user-configuration.png)
![VPN-Konfiguration speichern](./vpnaas_download-config-and-get-passphrase.png)

### Benutzer löschen

1. Klicken Sie auf das Löschsymbol neben dem Benutzer, den Sie löschen möchten.
![Benutzerlöschung](./vpnaas_delete-user.png)

1. Klicken Sie auf Speichern.
![Bestätigen Sie die Benutzerlöschung](./vpnaas_save-user-deletion.png)
