---
title: FAQ
lang: "de"
permalink: /edge/faq/
has_children: false
nav_order: 8000
---

# FAQ

## Openstack

### Wie werden User hinzugefügt?

Aktuell nur auf Anfrage per Jira Helpdesk Ticket da jeder Openstack User auch Admin ist.

Wie kann man VM's löschen von anderen Usern/Projekten?
Im Horizon dashboard kann man über den Adminbereich auf alle VMs zugreifen und diese löschen.

Sonst ist der Zugriff auch über die Openstack CLI möglich

### Wie kann die Plattengröße einer bestehenden VM erweitert werden?

Dies ist in der aktuellen Openstack Version nicht möglich.

### Welche Subnetze gibt es

Die folgenden Subnetze sind immer vorhanden:

* public
* shared
* private/internal

Auf Anfrage kann ein viertes Netzwerk, Labor, für eine direkte Verbindung ohne die Barracuda Firewall erstellt werden.

### Wie erfolgt die Trennung der Subnetze:

Subnetze werden über OpenVSwitch virtualisiert zur garantierten Trennung können Securityregeln angelegt werden.
Diese Regeln sind durch IPFilter Regeln in OpenStack Neutron dar.
Die Einhaltung von Zonen wird im Operations Center über Sicherheitsregeln und Sicherheitsgruppen verwaltet.

## Operations Center

### Wie werden User hinzugefügt im Operations Center?

Der Zugriff auf das Operations Center wird über SSO realisiert. Nachdem Benutzer sich angemeldet haben können sie durch Projekteigentümer zu einem Projekt hinzugefügt werden.

Jeder Nutzer kann eigene Projekte anlegen.

Zusätzlich können im Operations Center externe Benutzer angelegt werden. Siehe Rolle Externe Benutzer

### Gruppen vs Rollen Unterschied von Rollen und Gruppen

Das Operations Center hat aktuell 4 Benutzergruppen:

* Externe Benutzer: Externe Benutzer werden im Operations Center verwaltet. Sie können Zugriff per VPN auf VMs erhalten. Zusätzlich kann der öffentliche SSH Schlüssel zu VMS hinzugefügt werden.
* Benutzer (SSO): Benutzer können Projekte anlegen und Verwalten. Auf Projektebene gibt es drei Rollen
* * Owner: Vollzugriff auf das Projekt.
* * Member: Vollen Zugriff auf die Projektresourcen ohne Benutzerverwaltung.
* * Viewer: Nur lesender Zugriff. Der Gast kann keine Passwörter für VMs einsehen.
* Admin (SSO)*können externe Benutzer verwalten und haben Zugriff auf die Ressourcenübersicht und Globale Nachrichten Funktion.
* Global Admin (SSO)*: Haben zusätzlichen Zugriff auf alle Projekte und können das VPNaaS für die Projekte aktivieren VPN können die normalen Admins auch anlegen

*Anfrage an GEC notwendig nachdem der Benutzer sich das erste mal am Operations Center angemeldet hat.

### Admin Rechte nur auf einem Projekt? Wie?

Benutzer haben automatisch vollen Zugriff auf Projekte, die sie anlegen. Andere Projektbesitzer können das Recht weitergeben.

### Wie kann ich den Besitzer von einem Projekt ändern?

Jeder Projektbesitzer und Admin mit Zugriff auf das Projekt kann die Berechtigungen ändern.

### Was passiert wenn ein User das Unternehmen verlässt?

Durch die Integration von SSO verliert der Benutzer automatisch den Zugriff. Es ist ggfls. notwendig seine Projekte neu zuzuweisen. Siehe vorheriger Punkt. VPN Zugänge müssen manuell gelöscht werden.

### Können Vorgaben gemacht werden wie ein Projekt heißen soll?

Beim Anlegen. Eine Änderung ist nicht vorgesehen.

### Wie wird ein VPN angelegt?

Globale Administratoren können die Funktion für einzelne Projekte aktivieren. Projektbesitzer können weiter Benutzer die dem Projekt hinzugefügt sind freischalten.

Benutzer brauchen Zugriff auf das Projekt um ihre eigene Konfiguration herunterzuladen.

#### Externe Benutzer

Externe Benutzer können auch für VPN freigeschaltet werden.

Die VPN Konfiguration für externe Benutzer muss durch einen Projektbesitzer Benutzer heruntergeladen und zur Verfügung gestellt werden.

### Wie werden Projektefreigaben angelegt bzw. erstellt?

Auf der Startseite jedes Projektes über die Mitgliederliste.

### Wie werden die Flavor verwaltet?

Flavor werden in Openstack verwaltet. Jedes "public" Flavor steht im OC zur Konfiguration bereit.

Flavor können nicht auf einzelne Benutzer beschränkt werden.

### Dürfen Flavor gelöscht werden

Ja, aber nur wenn sichergestellt ist, dass keine VM das Flavor nutzt. Das Löschen erfolgt über Horizon.

Sollte ein Flavor, welches noch in Nutzung ist, gelöscht werden, kann jedes Projekt mit einer betroffenen VM nicht mehr über das Operations Center verwaltet werden.

### Wo kann man Logs sehen, wenn eine VM angelegt wurde aber sie nicht erscheint?

Über Horizon ist es möglich den Startvorgang der VM zu beobachten und zu wiederholen.

Wird die VM in Horizon nicht angelegt, ist aktuell ein Ticket im Helpdesk anzulegen.

### Wie kann man VMs löschen von anderen Usern/Projekten?

Nur Benutzer und Globale Administratoren mit Zugriff auf Projekte können VMs löschen.

### Projektliste Operations Center vs Horizon

Die Projekte vom Operations Center sind auch in Horizon sichtbar. Projekte, die in Horizon erzeugt werden sind nicht im Operations Center sichtbar.

### Wie können VMs gestartet werden?

Der Start von VMs ist über die Schaltfläche "Starten" im Operations Center möglich.

### Wie kann die Plattengröße einer bestehenden VM erweitert werden im Operations Center?

Es ist nicht möglich.

### Wie können externe/interne User gelöscht werden?

Interne User werden über das SSO authentifiziert. Werden sie dort deaktiviert, werden sie automatisch gesperrt.

Externe Benutzer können in der Übersicht der Externen Benutzer im Operations Center von Administratoren gelöscht werden.

### Wie kann ich den Router bearbeiten und Konfigurieren?

Das Operations Center konfiguriert die Router entsprechend der Projektvorgaben automatisch.

### Wie können Routen bearbeitet werden im Operations Center?

Es können im Operations Center keine zusätzlichen Routen eingerichtet werden.

### Wie kann man einen SSH key einer vorhandenen VM hinzufügen?

Es ist nicht möglich SSH keys nach der Erstellung einer VM über das Operations Center hinzuzufügen. Dies müssen direkt in der VM hinzugefügt werden.

### Volumegrößen

Das Operations Center erlaubt das Anlegen von Volumes in unterschiedlichen Größen. Die Liste kann auf Anfrage erweitert werden.

### Kann das Operations Center per API konfiguriert werden

Nein, das ist nicht vorgesehen.

### Wie kann ich mich bei einer Windows VM anmelden? Der User operation funktioniert nicht mit dem eingegebenen Passwort.

Anstelle des Benutzers "operation" muss für Windows VMs der Benutzer des ausgewählten images verwendet werden. Oft Administrator.

## General

### Zonen

![Zonen Konzept](./zonen_konzept.png)
Sicherheitszonen sind in hierarchisch absteigender Reihenfolge strukturiert, beginnend mit der Private / Internal Zone, gefolgt von der Shared Zone und Public Zone. Auf dieser Grundlage können virtuelle Maschinen (VMs) durch die Zuweisung von Floating IPs hierarchisch absteigend kommunizieren. Wenn VMs in sich in der Private / Internal Zonen befinden ist eine Kommunikation mit den Floating IPs in der Shared und Public Zone möglich. Allerdings ist der umgekehrte Weg, also von der Shared Zone zurPrivate / Internal Zonen, nicht gestattet. Analog ist die Kommunikation von der Public Zone in die Shared Zone zur Private / Internal Zonen nicht gestattet.

Die Kommunikation folgt dabei stets dem Prinzip von sicher zu unsicher. Diese hierarchische Struktur ermöglicht eine geordnete und sicherheitsbewusste Kommunikation zwischen den verschiedenen Zonen.
Des weiteren sind Floating IPs aus der Private / Internal Zonen nur aus dem Instituts Netz erreichbar, Services mit einer Floating IP aus der Public IP können durch die Instituts IT ins Internet bereitgestellt werden.

Als spezial Zone kann auf Anfrage eine Lab Zone eingerichtet werden mit direkt Verbindung an den Switchen.

Die Zonen werden durch das Operations Center verwaltet. Bei Projekten, die in Openstack angelegt werden, ist der Nutzer selbst verantwortlich.

### Wie können die Hardware Server runter/hoch gefahren werden? z.B. bei einer kurzfristigen Stromabschaltung.

Aktuell nur per Anfrage im Jira Helpdesk

### Starten die Server nach Strom automatisch?

Die Server Starten automatisch aber die Edge ist nicht automatisch nutzbar, die bei einem geregelten Herunterfahren die CEPH Replikation deaktiviert wird.
Diese muss bei einem Neustart wieder aktiviert werden.

### Wenn das beigefügte Windows Image ausgewählt ist, ist da bereits eine Lizenz mit dabei. Um was für eine Lizenz handelt es sich?

Es handelt sich um eine Trial Lizenz von Microsoft. Bei Windows VM's, ist der Kunde für den Erwerb und das Einfügen einer Lizenz verantwortlich.

### NVLink und NVSwitches

NV Link und NVSwitches verbinden NVidia Grafikkarten zum direkten Austausch von Daten zwischen den Grafikkarten.

Aktuell verbauen wir Supermicro GPU Server diese haben keine NVLink oder NVSwitch zwischen den einzelnen GPUs.

Zum aktuellen Zeitpunkt werden NVLink und NVSwitches nur in NVidia eigenen Servern verbaut der Marke DGX oder HGX.
