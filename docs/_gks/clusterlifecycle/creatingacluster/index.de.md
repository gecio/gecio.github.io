---
title: Einen Cluster anlegen
lang: de
permalink: /gks/clusterlifecycle/creatingacluster/
nav_order: 4100
parent: Cluster Lebenszyklus
---
<!-- LTeX:  language=de-DE -->

# Cluster Lebenszyklus

Um einen Cluster in GKS anzulegen, benötigen Sie nur ein paar Klicks.

Dazu benötigen Sie ein Projekt.

Falls Sie noch kein Projekt erstellt haben, erstellen Sie zuerst ein [Projekt](/gks/managingprojects/creatingaproject).

Um einen Cluster anzulegen, klicken Sie im gewünschten Projekt oben rechts auf `Create Cluster`.
![Create Cluster](projectview_addcluster.png)

Jetzt öffnet sich die erste Seite für den Prozess, einen Cluster anzulegen.
Wählen Sie den Provider `openstack` und eine der drei Verfügbarkeitszonen aus, in diesem Beispiel
nehmen Sie `IX2`:
![Add Cluster Step 1](add_step1.png)

Im nächsten Schritt konfigurieren Sie die Cluster-Details. In dem Beispiel nennen Sie den Cluster `first-system` und
wählen die gewünschte Kubernetes-Version aus.
![Add Cluster Step 2](add_step2.png)

Für den gelegentlichen SSH-Zugriff auf Worker-Nodes können Sie optional einen öffentlichen SSH-Schlüssel hinterlegen.
Zum Hinzufügen eines SSH-Keys klicken Sie auf `Add SSH Key`.
![Add Cluster Step 2.2](add_step2_2.png)

In dem sich öffnenden Dialog können Sie Ihren SSH Public Key eintragen
und ihm einen passenden Namen geben.
![Add Cluster Step 2.3](add_step2_3.png)

Damit GKS in der OpenStack-Infrastruktur die notwendigen Ressourcen erzeugen kann,
geben Sie im nächsten Schritt Ihre Zugangsdaten ein. Danach wird der Inhalt im Feld `Project`
automatisch aktualisiert und Sie können in der Dropdown-Liste Ihr gewünschtes OpenStack Projekt
auswählen.
![Add Cluster Step 3.1](add_step3.png)
![Add Cluster Step 3.2](add_step3_2.png)

Mit dem Hinzufügen der Credentials und dem Auswählen des OpenStack-Projekts haben Sie alle
notwendigen Eingaben getätigt und Sie können mit dem nächsten Schritt fortfahren. Wenn Sie das tun,
wird automatisch ein eigenes Netzwerk, Subnetz sowie eine Security Gruppe für das neue Cluster erstellt.

Sie können jedoch auch ein existierendes Netzwerk verwenden, um den Cluster zu erstellen.
Dazu müssen Sie das Netzwerk und das Subnetz auswählen. Diese müssen allerdings mit einem Router verbunden sein.
In unserer [OpenStack Dokumentation](/optimist/guided_tour/step10/) ist beschrieben, wie Sie einen Router erstellen
und mit einem Netzwerk verbinden können.

![Add Cluster Network](create-cluster-network-exist.png)

Im nächsten Schritt definieren Sie, wie viele und welche virtuellen Maschinen als Worker-Nodes im Cluster verfügbar
sein sollen.

Zuerst geben Sie dem sogenannten `Machine Deployment` einen Namen. Für Ihren Testcluster nutzen Sie dazu den Namensgenerator.
![Add Cluster Step 4](add_step4.png)

Danach spezifizieren Sie die `Replicas` (Anzahl der Worker-Nodes im Kubernetes-Cluster) und den `Flavor` (den Maschinentyp), welcher
im Wesentlichen die Anzahl der verfügbaren CPU-Kerne und des RAMs bestimmt.
![Add Cluster Step 4.2](add_step4_2.png)

Über einen Klick auf `Next` gelangen Sie zum letzten Schritt, wo Sie noch einmal alle Einstellungen verifizieren und mit `Create Cluster`
die Cluster-Erstellung starten können.
![Add Cluster Step 5](add_step5.png)

Nun wird das Cluster erstellt. Um auf die Informationen zugreifen zu können, müssen
Sie wieder zur Cluster-Übersicht des Projektes gehen und dort Ihren Cluster auswählen.
![Add Cluster Step 6](add_step6.png)

Nach der Auswahl Ihres Clusters kommen Sie auf die Seite mit allen Cluster-Details.
![Add Cluster Step 6.2](add_step6_2.png)

## Zusammenfassung

Herzlichen Glückwunsch! Sie haben folgende Schritte erfolgreich durchgeführt und gelernt:

* Was ist ein GKS Cluster?
* Wie erstellt man einen GKS Cluster?

In den folgenden Abschnitten finden Sie Beispiele für die Verwendung von Clustern.
