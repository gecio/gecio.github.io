---
title: Openstack Application Credentials benutzen
lang: de
permalink: /gks/clusterlifecycle/applicationcredentials/
nav_order: 4300
parent: Cluster Lebenszyklus
---
<!-- LTeX:  language=de-DE -->

# Openstack Application Credentials benutzen

Während der Erstellung eines GKS-Clusters ist es notwendig sich bei der
Openstack API anzumelden. Dies wurde in der bisherigen Dokumentation mit
dem Benutzenamen und dem Passwort des Benutzers des Openstack Tenants
beschrieben. Da jedoch sowohl der *openstack-cloud-controller* als auch
der *machine-controller* beide Zugriff auf die Openstack-Ressourcen
benötigen (Provisionieren von Compute-Instanzen für neue Kubernetes-worker
Knoten oder Provisionierung von Netzwerk-Loadbalancern wenn Kubernetes
Services des Typ *Loadbalancer* eingerichtet werden sollen) ist es
notwendig die bei der Erstellung des Clusters verwendeten Benutzernamen und
Passwort im GKS-cluster selbst als Kubernetes-Secret zu hinterlegen.
Dies resultiert darin, dass alle Benutzer mit der Rolle *ClusterAdmin*
Rechte haben dieses Secret zu lesen. Bei erhöhtem Sicherheitsbedarf ist
dieser Zustand nicht wünschenswert.

Es gibt eine Openstack-Funktionalität welche dieses verhindert:
[Openstack Application Credentials](https://docs.gec.io/de/optimist/specs/application_credentials/).

Openstack erlaubt das Erzeugen weiterer Applikationsuser, welches ihr
eigenes Passwort haben und dessen Rechte eingeschränkt werden können.
Eine weitere Eigenschaft dieser Applikationsuser ist, dass sie nicht
Rechte auf den gesamten Openstack Tenant haben können, sondern ihre
Reichweite auf einzelne Projekte innerhalb des Openstack Tenants
eingeschränkt sind. Dies ermöglicht die kontrollierte Separierung von
Umgebungen. Es wäre zum Beispiel möglich innerhalb eines Openstack
Tenants drei Projekte dev, test und prod zu erstellen und für jedes
Projekt einen Applikationsuser anzulegen und nur diesen bei der
Erstellung der jeweiligen GKS-Cluster zu benutzen. Dies würde dem
Benutzer das Managen der Openstack-Ressourcen aller Umgebungen
(Projekte) ermöglichen und trotzdem die größtmögliche Isolation
zwischen den Umgebungen gewährleisten.


## Openstack Applikations-User anlegen

Eine detaillierte Beschreibung dazu was Openstack Applikations-User
genau sind und wie sie angelegt werden findet man in unserer
[Openstack-Dokumentation hier](https://docs.gec.io/de/optimist/specs/application_credentials/).


## Erstellen von GKS-Clustern mit Openstack Applikations-Usern

Beim Erstellen eines GKS-Clusters wird in Schritt 3 (Settings) nach
Openstack Authentifizierungsdaten gefragt. Standardmäßig ist hierbei
die HTML-Form *Basic Credentials* ausgewählt in der Benutzernamen,
Passwort, Projektnamen und ProjektID für Openstack eingegeben werden
soll. Es gibt jedoch einen weiteren Tab mit dem Namen *Application
Credentials* wo nur noch in zwei Feldern die *ID* und das *Secret*
desselben erfragt wird. Projektname und -ID entfallen da *Application
Credentials* immer einem Projekt eindeutig zugeordnet sind und sich
dies auch nicht im Nachhinein ändern lässt.

![Eingabe Application Credentials](../images/AppCreds01.png)

Nach der Eingabe der *Application Credentials* beginnt im Hintergrund
die Erstellung des Kubernetes Clusters wie bereits zuvor in der
Dokumentation beschrieben. Zuvor gibt es nochmal die Möglichkeit
die Eingabe auf der Zusammenfassungsseite in Schritt 5 zu überprüfen.
Hier findet sich nun nur noch die _applicationID_ anstelle von
Domäne, Benutzer- und Projektname des Openstack Projekts.


## Austauschen von Openstack Applikations-Usern in bestehenden GKS-Clustern

Eine weitere Besonderheit von Openstack Applikationsusern ist, dass
man Sie mit einem Ablaufdatum versehen kann. Dies erhöht die Sicherheit
des Setups und garantiert dass bei unentdeckter Enthüllung des Passworts
dieses trotzdem nur eine begrenzte Zeit unerlaubt genutzt werden kann.

In diesem Fall sollte der Openstack-Tenant Besitzer rechtzeitig einen neuen
Applikationsuser anlegen und den verfallenden damit ersetzen. Hierzu wird
die Clusteransicht des betreffenden Clusters im GKS-Dashboard geöffnet.

![Clusteransicht](../images/OSCred01.png)

Um den Applikationsuser im laufenden Betrieb reibungslos auszutauschen
drückt der GKS-Benutzer auf den Knopf mit den drei vertikalen Punkten
in der oberen rechten Ecke der Clusteransicht und wählt den Menüpunkt
*Edit Provider* aus.

![Edit Provider](../images/OSCred03.png)

Im nächsten Schritt wird ein Dialog zum austauschen der Openstack
Authentifizierungsdaten angezeigt. Wie zuvor gibt es auch hier
zwei Tabs, wobei standardmäßig die *User Credentials* ausgewählt sind.

![Dialog Authentifizierungsdaten ändern](../images/AppCreds02.png)

Auch hier wechselt der Benutzer auf den *Application Credentials* Tab
und bekommt nun die Möglichkeit eine neue ApplikationsID und -Passwort
einzugeben.

![Auswahl Application Credentials Tab](../images/AppCreds03.png)

Nach der Eingabe der korrekten Werte für ID und Passwort müssen die
Eingaben durch den Druck auf das *Save Settings* Feld bestätigt werden.
Danach startet im Hintergrund der automatische Austauschprozess für
die neuen Zugangsdaten.

![Neue Application Credentials speichern](../images/AppCreds04.png)
