---
title: "05: Die wichtigsten Befehle des OpenStackClients"
lang: "de"
permalink: /optimist/guided_tour/step05/
nav_order: 1050
parent: Guided Tour
---

# Schritt 5: Die wichtigsten Befehle des OpenStackClients

## Einführung

Nachdem in [Schritt 4](/optimist/guided_tour/step4/) der OpenStack Client installiert wurde, lernen Sie in diesem Schritt alle wichtigen OpenStack fehle kennen.

Die Übersicht der spezifischen Subbefehle kann in der Kommandozeile mit
einem `--help` hinter dem eigentlichen Befehl separat angezeigt werden.

Um alle Befehle aufzulisten, kann der Schalter `--help` auch ohne
weitere Angaben von einem Bestandteil verwendet
werden:

```bash
openstack --help
```

## Server

Mit dem Kommando `openstack server` können eigene Instanz
erstellt, verwaltet, gelöscht und
andere administrative Aufgaben durchgeführt werden.

Hier ist eine Liste der wichtigsten Befehle:

- `openstack server add`
    Weist einer bestehenden Instanz verschiedene Bestandteile zu (Fixed IP, Floating IP, Security
    Group, Volume)
- `openstack server create`
    Erstellt eine neue Instanz
- `openstack server delete`
    Löscht die im Befehl angegebene Instanz
- `openstack server list`
    Listet alle bestehenden Instanzen auf
- `openstack server remove`
    Entfernt verschiedene Bestandteile (Fixed IP, Floating IP, Security
    Group, Volume)
- `openstack server show`
    Zeigt alle verfügbaren Informationen zu der im Befehl genannten
    Instanz an

## Stack (Heat)

Genauso wie mit `openstack server ...` Befehlen einzelne Instanzen
administriert werden, können mit `openstack stack ...` ganze
Stacks verwaltet werden.

Hier ist eine Liste der wichtigsten Befehle:

- `openstack stack create`
    Erstellt einen neuen Stack
- `openstack stack list`
    Listet alle bestehenden Stacks auf
- `openstack stack show`
    Zeigt alle Informationen zu dem im Befehl angegebenen Stack
- `openstack stack delete`
    Löscht den im Befehl angegebenen Stack

## Security Group

Security Groups werden verwendet, um Instanzen eingehende und ausgehende
Netzwerk-Verbindungen basierend auf IP-Adressen und Ports zu erlauben
oder zu verbieten.

Security Groups können ebenfalls mit dem OpenStackClient verwaltet werden.

Hier ist eine Liste wichtiger Befehle:

- `openstack security group create`
    Erstellt eine neue Security Group
- `openstack security group delete`
    Löscht die im Befehl angegebene Security Group
- `openstack security group list`
    Listet alle bestehenden Gruppen auf
- `openstack security group show`
    Zeigt alle verfügbaren Informationen zu der im Befehl angegebenen
    Security Group
- `openstack security group rule create`
    Fügt eine Regel zu einer Security Group hinzu
- `openstack security group rule delete`
    Löscht die im Befehl angegeben Regel

## Network

Um später auch Instanzen sinnvoll nutzen zu können, benötigen diese ein
Netzwerk. Hier ist eine Liste der wichtigsten Befehle, um ein
Netzwerk zu erstellen:

- `openstack network create`
    Erstellt ein neues Netzwerk
- `openstack nerwork list`
    Listet alle bestehenden Netzwerke auf
- `openstack network show`
    Zeigt alle Informationen zu dem im Befehl angegebenen Netzwerk
- `openstack network delete`
    Löscht das im Befehl angegebene Netzwerk

## Router

Damit eine Instanz mit einem Netzwerk verbunden werden kann, ist ein virtueller Router notwendig.

Hier ist eine Liste wichtiger Befehle:

- `openstack router create`
    Erstellt einen neuen Router
- `openstack router delete`
    Löscht einen bestehenden Router
- `openstack router add port`
    Weist dem angegebenen Router den angegebenen Port zu
- `openstack router add subnet`
    Weist dem angegeben Router das angegebene Subnet zu

## Subnet

Um den virtuellen Router korrekt betreiben zu können, wird auch ein Subnet benötigt, welches mit dem Kommando `openstack subnet` administriert werden kann.

Hier ist eine Liste wichtiger Befehle:

- `openstack subnet create`
    Erstellt ein neues Subnet
- `openstack subnet delete`
    Löscht ein bestehendes Subnet
- `openstack subnet show`
    Zeigt alle verfügbaren Informationen zu einem Subnet an

## Port

Nachdem nun bereits virtuelle Router und Subnets bekannt sind, darf der
Port nicht fehlen. Ports verbinden die VMs mit dem Netzwerk.

Hier ist eine Liste wichtiger Befehle

- `openstack port create`
    Erstellt einen neuen Port
- `openstack port delete`
    Löscht einen bestehenden Port
- `openstack port show`
    Zeigt alle verfügbaren Informationen zu einem Port an

## Volume

Volumes sind persistente Speicherorte, die über die Existenz von
einzelnen Instanzen hinaus erhalten bleiben.

Hier ist eine Liste wichtiger Befehle:

- `openstack volume create`
    Erstellt ein neues Volume
- `openstack volume delete`
    Löscht ein bestehendes Volume
- `openstack volume show`
    Zeigt alle verfügbaren Informationen zu einem Volume an

## Zusammenfassung

In diesem Schritt sind die wichtigsten OpenStack Befehle aufgelistet und
erklärt.
Die Befehle werden für die nächsten Schritten der Guided Tour benötigt.

In [Schritt 6](/optimist/guided_tour/step6/) zeigen wir, wie Sie ein SSH Key Pair mit der Kommandokonsole erzeugen und benutzen können.
