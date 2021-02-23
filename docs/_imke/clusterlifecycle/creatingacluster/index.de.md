---
title: Einen Cluster anlegen
lang: de
permalink: /imke/clusterlifecycle/creatingacluster/
nav_order: 4100
parent: Cluster Lebenszyklus
---

Einen Cluster in iMKE anzulegen benötigt nur ein paar Klicks.
Doch bevor wir dies tun können, benötigen wir ein Projekt.

Wenn Sie noch kein Projekt erstellt haben, [erstellen Sie bitte zuerst ein Projekt](/imke/managingprojects/creatingaproject).

Um einen Cluster anzulegen, klicken wir oben rechts auf `Add Cluster`.
![Add Cluster](projectview_addcluster.png)

Jetzt öffnet sich die erste Seite für den Prozess, einen Cluster anzulegen.
Im Beispiel nennen wir unseren Cluster `first-system` und wählen die Kubernetes
Version 1.18.6 aus:
![Add Cluster Step 1](add_step1.png)

Wir klicken dann auf `Next` und im nächsten Schritt wählen wir den Provider
`openstack` und eine der drei Verfügbarkeitszonen aus, in diesem Beispiel
nehmen wir `IX1`:
![Add Cluster Step 2.1](add_step2_1.png) ![Add Cluster Step 2.2](add_step2_2.png)

Damit iMKe in der OpenStack Plattform die notwendigen Ressourcen erzeugen kann,
geben wir hier erneut unsere Zugangsdaten ein. Danach wird der Inhalt in `Project`
automatisch aktualisiert und wir können unser gewünschtes OpenStack Projekt
auswählen:
![Add Cluster Step 3.1](add_step3.png)
![Add Cluster Step 3.2](add_step3_2.png)

Man kann ein existierendes Netzwerk verwenden, um den Cluster zu erstellen. Dazu müssen wir das Netzwerk und das Subnetz auswählen.
Diese müssen mit einem Router verbunden sein.
Also man soll einen Router erstellen, was auf dem Optimist Dashboard oder aus der OpenStack Kommandozeile durchgeführt werden kann.
Hierfür kann man die OpenStack Dokumentation nutzen, um den Router zu erstellen und das Netzwerk zu verbinden. (<http://docs.innovo.cloud/guided_tour/en/step10/>)
![Add Cluster Network](create-cluster-network-exist.png)

In den Node Settings konfigurieren wir, wie viele Server später im Kubernetes Cluster
als Nodes verwendet werden können. Diese Gruppe von Servern braucht einen Namen und
eine Größe. Für den ersten Cluster sind genaue Namen nicht so wichtig, deswegen benutzen
wir den Namensgenerator. Die Anzahl und Größe der Server belassen wir auf den
Standardwerten.
![Add Cluster Step 4](add_step4.png)

Als Image entscheiden wir uns für Flatcar Linux. Flatcar Linux ist
speziell für den Betrieb von Containern gedacht und wird durch iMKE
automatisch aktualisiert.
![Add Cluster Step 5](add_step5.png)

Für den SSH Zugriff müssen wir unseren öffentlichen SSH-Schlüssel hinterlegen sowie "Floating IP" aktivieren. Zum Hinzufügen eines SSH-Keys klicken wir auf `Add SSH Key` und tragen in dem Dialog der sich öffnet unseren SSH Public Key ein
und geben ihm einen passenden Namen:
![Add Cluster Step 6](add_step6.png)
![Add Cluster Step 6.2](add_step6_2.png)

Zum Schluss klicken wir wieder auf `Next` und nachdem wir alle Informationen
noch einmal geprüft haben auf `Create`.
![Add Cluster Step 7](add_step7.png)

Nun wird das Cluster erstellt. Um auf die Informationen zugreifen zu können müssen
wir nun wieder auf die Cluster-Übersicht des Projektes und dort auf unser Cluster
schicken.
![Add Cluster Step 8](add_step8.png)
![Add Cluster Step 8.2](add_step8_2.png)

## Zusammenfassung

Folgende Schritte wurden erfolgreich durchgeführt und gelernt:

* Was ist ein Projekt in iMKE
* Wie legt man ein Projekt in iMKE an
* Was ist ein iMKE Cluster
* Wie konfiguriert man ein iMKE Cluster
* Wie startet man ein iMKE Cluster

Herzlichen Glückwunsch! Dies sind alle notwendigen Steps um ein Kubernetes Cluster
in iMKE anzulegen. Wie dieses verwendet werden kann steht auf den nächsten Seiten.
