---
title: "05: Die wichtigsten Befehle des OpenStackClients"
lang: de
permalink: /optimist/guided_tour/step05
nav_order: 1050
parent: Guided Tour
---

Schritt 5: Die wichtigsten Befehle des OpenStackClients
=======================================================

Vorwort
-------

Nachdem in Schritt 4 der OpenStack Client installiert wurde,
werden wir in diesem Schritt alle wichtigen Befehle einmal auflisten.

Die Übersicht der spezifischen Subbefehle kann in der Kommandozeile mit
einem `--help` hinter dem eigentlichen Befehl separat angezeigt werden.

Um alle Befehle aufzulisten, kann der Schalter `--help` auch ohne
weitere Angaben von einem Bestandteil  verwendet
werden:

```bash
$ openstack --help
```

Server
------

Mit dem Kommando `openstack server` ist es mögliche eigene Instanz
zu erstellen, diese zu verwalten, zu löschen und
andere} administrative Aufgaben durchzuführen.

Hier eine Liste der wichtigsten Kommandos:}

-   `openstack server add`
    Einer bestehenden Instanz können verschiedene Bestandteile (Fixed IP, Floating IP, Security
    Group, Volume) zugewiesen werden
-   `openstack server create`
    Mit diesem Befehl kann eine neue Instanz erstellt werden
-   `openstack server delete`
    Löscht die im Befehl angegebene Instanz
-   `openstack server list`
    Listet alle bestehenden Instanzen auf
-   `openstack server remove`
    Kann verschiedene Bestandteile (Fixed IP, Floating IP, Security
    Group, Volume) wieder entfernen
-   `openstack server show`
    Zeigt alle verfügbaren Informationen zu der im Befehl genannten
    Instanz an


Stack (Heat)
------------

Genauso wie mit `openstack server ...` Befehlen einzelne Instanzen
administriert werden, kann man mit `openstack stack ...` ganze
Stacks verwalten. 

Auch hier eine kurze Auflistung der wichtigsten Befehle:

-   `openstack stack create`
    Kann einen neuen Stack erstellen
-   `openstack stack list`
    Listet alle bestehenden Stacks auf
-   `openstack stack show`
    Zeigt alle Informationen zu dem im Befehl angegebenen Stack
-   `openstack stack delete`
    Löscht den im Befehl angegebenen Stack

Security Group
--------------

Security Groups werden verwendet, um Instanzen eingehende und ausgehende
Netzwerk-Verbindungen basierend auf IP-Adressen und Ports zu erlauben
oder zu verbieten.

Auch Security Groups kann man mit dem OpenStackClienten verwalten. Hier
eine beispielhafte Liste üblicher Aufrufe:

-   `openstack security group create`
    Erstellt eine neue Security Group 
-   `openstack security group delete`
    Löscht die im Befehl angegebene Security Group
-   `openstack security group list`
    Listet alle bestehenden Gruppen auf
-   `openstack security group show`
    Zeigt alle verfügbaren Informationen zu der im Befehl angegebenen
    Security Group
-   `openstack security group rule create`
    Fügt eine Regel zu einer Security Group hinzu
-   `openstack security group rule delete`
    Löscht die im Befehl angegeben Regel

Network
-------

Um später auch Instanzen sinnvoll nutzen zu können, benötigen diesen ein
Netzwerk, hier eine kurze Auflistung der wichtigsten Befehle um ein
Netzwerk zu erstellen:

-   `openstack network create`
    Erstellt ein neues Netzwerk
-   `openstack nerwork list`
    Listet alle bestehenden Netzwerke auf
-   `openstack network show `
    Zeigt alle Informationen zu dem im Befehl angegebenen Netzwerk
-   `openstack network delete `
    Löscht das im Befehl angegebene Netzwerk

Router
------

Damit eine Instanz mit einem Netzwerk verbunden werden kann, ist ein virtueller
Router notwendig und lässt sich mit diesen Kommando administrieren.

Hier eine kurze Liste der möglichen Befehle:

-   `openstack router create`
    Erstellt einen neuen Router
-   `openstack router delete`
    Löscht einen bestehenden Router
-   `openstack router add port`
    Weist dem angegebenen Router, den angegebenen Port zu
-   `openstack router add subnet`
    Weist dem angegeben Router, das angegebene Subnet zu

 

Subnet
------

Um den virtuellen Router korrekt zu betreiben, wird auch ein Subnet
benötigt, welches mit dem Kommando `openstack subnet` administriert
werden kann.

Möglich Befehle sind:

-   `openstack subnet create`
    Erstellt ein neues Subnet
-   `openstack subnet delete`
    Löscht ein bestehendes Subnet
-   `openstack subnet show`
    Zeigt alle verfügbaren Informationen zu einem Subnet an

Port
----

Nachdem nun bereits virtuelle Router und Subnets bekannt sind, darf der
Port nicht fehlen.

Die wichtigsten Befehle:

-   `openstack port create`
    Erstellt einen neuen Port
-   `openstack port delete`
    Löscht einen bestehenden Port
-   `openstack port show`
    Zeigt alle verfügbaren Informationen zu einem Port an

Volume
------

Volumes sind persistente Speicherorte, die über die Existenz von
einzelnen Instanzen hinaus erhalten bleiben. Wichtige Befehle sind:

-   `openstack volume create`
    Erstellt ein neues Volume
-   `openstack volume delete`
    Löscht ein bestehendes Volume
-   `openstack volume show`
    Zeigt alle verfügbaren Informationen zu einem Volume an

Abschluss
---------

In diesem Schritt wurden die wichtigsten Befehle einmal aufgelistet und
auch mit einer kleinen Erklärung versehen. 

Die genannten Befehle werden in den nächsten Schritten benötigti und
bilden somit die Grundlage für die Guided Tour.

In Schritt 6 wird das Thema ein selbst erstelltes SSH Key Pair
sein.
