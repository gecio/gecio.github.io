---
title: Einen Cluster anlegen
lang: de
permalink: /gks/clusterlifecycle/creatingacluster/
nav_order: 4100
parent: Cluster Lebenszyklus
---
<!-- LTeX:  language=de-DE -->

# Einen Cluster anlegen

Um einen Cluster in GKS anzulegen, benötigen Sie nur ein paar Klicks.

Dazu benötigen Sie ein Projekt.

Falls Sie noch kein Projekt erstellt haben, erstellen Sie zuerst ein [Projekt](/gks/managingprojects/creatingaproject).

Um einen Cluster anzulegen, klicken Sie im gewünschten Projekt oben rechts auf `Create Cluster`.
![Create Cluster](../../gettingstarted/images/GS05_CreaClus.png)

Jetzt öffnet sich die erste Seite für den Prozess, einen Cluster anzulegen.
Wählen Sie den Provider `openstack`.

![Choose Provider](../../gettingstarted/images/GS06a1_CreaClus.png)

Wählen Sie anschließend eine der drei Verfügbarkeitszonen aus, in diesem Beispiel
nehmen Sie `IX2`:
![Choose Datacenter](../../gettingstarted/images/GS06a2_CreaClus.png)

Im nächsten Schritt konfigurieren Sie die Cluster-Details. In dem Beispiel nennen Sie den Cluster `first-system` und
wählen die gewünschte Kubernetes-Version aus.
![Add Cluster Details](../../gettingstarted/images/GS07_CreaClus.png)


Für den gelegentlichen SSH-Zugriff auf Worker-Nodes können Sie optional einen öffentlichen SSH-Schlüssel hinterlegen.
Zum Hinzufügen eines SSH-Keys klicken Sie auf `Add SSH Key`.
![Add SSH Key](../../gettingstarted/images/GS08_CreaClusSSH.png)

In dem sich öffnenden Dialog können Sie Ihren SSH Public Key eintragen
und ihm einen passenden Namen geben.
![Add SSH Key](../../gettingstarted/images/GS09_CreaClusSSH.png)

Damit GKS in der OpenStack-Infrastruktur die notwendigen Ressourcen erzeugen kann,
geben Sie im nächsten Schritt Ihre Zugangsdaten ein. Danach wird der Inhalt im Feld `Project`
automatisch aktualisiert und Sie können in der Dropdown-Liste Ihr gewünschtes OpenStack Projekt
auswählen.
![Add Cluster user credentials](../../gettingstarted/images/GS10_CreaClusUserCred.png)
![Add Cluster user credentials](../../gettingstarted/images/GS11_CreaClusUserCred.png)

Mit dem Hinzufügen der Credentials und dem Auswählen des OpenStack-Projekts haben Sie alle
notwendigen Eingaben getätigt und Sie können mit dem nächsten Schritt fortfahren. Wenn Sie das tun,
wird automatisch ein eigenes Netzwerk, Subnetz sowie eine Security Gruppe für das neue Cluster erstellt.

Sie können jedoch auch ein existierendes Netzwerk verwenden, um den Cluster zu erstellen.
Dazu müssen Sie das Netzwerk und das Subnetz auswählen. Diese müssen allerdings mit einem Router verbunden sein.
In unserer [OpenStack Dokumentation](/optimist/guided_tour/step10/) ist beschrieben, wie Sie einen Router erstellen
und mit einem Netzwerk verbinden können.

![Add Cluster Network](../../gettingstarted/images/GS12_CreaClusUserCred.png)

Im nächsten Schritt definieren Sie, wie viele und welche virtuellen Maschinen als Worker-Nodes im Cluster verfügbar
sein sollen.

Zuerst geben Sie dem sogenannten `Machine Deployment` einen Namen. Für Ihren Testcluster nutzen Sie dazu den Namensgenerator.
![Add Cluster Name Generator](../../gettingstarted/images/GS13_CreaClus.png)

Danach spezifizieren Sie die `Replicas` (Anzahl der Worker-Nodes im Kubernetes-Cluster) und den `Flavor` (den Maschinentyp), welcher
im Wesentlichen die Anzahl der verfügbaren CPU-Kerne und des RAMs bestimmt.
![Add Cluster replica and flavor](../../gettingstarted/images/GS13a_CreaClus.png)

Wählen Sie Flatcar als Betriebssystem für die Worker-Nodes.
![Add Cluster Flatcar](../../gettingstarted/images/GS14_CreaClus.png)

Über einen Klick auf `Next` gelangen Sie zum letzten Schritt, wo Sie noch einmal alle Einstellungen verifizieren und mit `Create Cluster`
die Cluster-Erstellung starten können.
![Create cluster](../../gettingstarted/images/GS15_CreaClus.png)

Nun wird das Cluster erstellt. Um auf die Informationen zugreifen zu können, müssen
Sie wieder zur Cluster-Übersicht des Projektes gehen und dort Ihren Cluster auswählen.
![Add Cluster select project](../../gettingstarted/images/GS16_CreaClus.png)

Nach der Auswahl Ihres Clusters kommen Sie auf die Seite mit allen Cluster-Details.
![Cluster Details](../../gettingstarted/images/GS17_CreaClus.png)

## Zusammenfassung

Herzlichen Glückwunsch! Sie haben folgende Schritte erfolgreich durchgeführt und gelernt:

* Was ist ein GKS Cluster?
* Wie erstellt man einen GKS Cluster?

In den folgenden Abschnitten finden Sie Beispiele für die Verwendung von Clustern.
