---
title: Aktualisierung des Betriebssystems auf Worker-Nodes
lang: de
permalink: /imke/machinedeployments/updatingnodeos/
nav_order: 5500
parent: Machine Deployments
---

Die iMKE-Plattform erlaubt es Ihnen, das Betriebssystem der Worker-Nodes ihrer Kubernetes-Cluster über das `Machine Deployment` selbst zu wählen.

* Für Flatcar kümmert sich die iMKE-Plattform (sofern nicht explizit deaktiviert) sogar um regelmäßige Betriebssystem-Updates, wie das Einspielen von Sicherheitsupdates.
* Für die Ubuntu-basierten Worker-Nodes sind manuelle Aktionen der Cluster-Besitzer erforderlich, um Updates zu erhalten.

## Flatcar Worker-Nodes automatisch aktualisieren

iMKE bietet die Funktionalität, um das Betriebssystem von Flatcar-basierten Worker-Knoten auf dem neuesten Stand zu halten.
Diese Funktion installiert automatisch alle Updates auf den Worker-Nodes, die vom Upstream-Anbieter (Kinvolk) für Flatcar veröffentlicht werden.

Die Auto-Update-Funktion verwendet [FLUO](https://github.com/kinvolk/flatcar-linux-update-operator), den Flatcar Linux Update Operator im Hintergrund.
Wenn nach dem Aktualisieren des Systems ein Neustart erforderlich ist, wird der Knoten vor dem Neustart evakuiert. Der Operator koordiniert den Neustart
mehreren Knoten im Cluster, und stellt sicher, dass immer nur ein Knoten gleichzeitig neu gestartet wird.

Die Verwendung der Auto-Update-Funktion ist standardmäßig aktiviert. Der folgende Screenshot zeigt die Erstellung eines Machine-Deployments mit aktiviertem Auto-Updater:

![Machine Deployment mit autoupdater erstellen](autoupdate_flatcar.png)

Wenn Sie sich selbst um Betriebssystemaktualisierungen (und Neustarts) kümmern möchten, können Sie die automatischen Aktualisierungen der Worker-Nodes deaktivieren, indem Sie das Kontrollkästchen `Disable auto-update` aktivieren:

![Machine Deployment ohne autoupdater erstellen](autoupdate_flatcar_disable.png)

> Wir empfehlen unseren Benutzern dringend, die Auto-Updater-Funktion zu verwenden, um die Sicherheit Ihrer Infrastruktur zu gewährleisten.

### Prüfen der Auto-Updater-Einstellungen eines Clusters

Um zu überprüfen, ob Ihre Knoten automatische Betriebssystemaktualisierungen erhalten, klicken Sie auf das Machine-Deployment:

![Machine Deployment öffnen](autoupdate_open_md.png)

und prüfen Sie, ob vor dem Kästchen `Disable auto-update` ein grünes Häkchen angezeigt wird (die automatische Aktualisierung ist deaktiviert):

![Autoupdater an](autoupdate_enabled.png)

oder ob es ausgegraut ist (die automatische Aktualisierung ist an):

![Autoupdater aus](autoupdate_disabled.png)

### Aktivieren / Deaktivieren des automatischen Updaters für ein vorhandenes Machine-Deployment

Um den Status des automatischen Updaters zu ändern, klicken Sie auf die Schaltfläche `Edit Machine Deployment` der Machine-Deployment:

![Edit Machine Deployment](autoupdate_edit_md.png)

und (de) -aktivieren Sie das Kontrollkästchen entsprechend:

![MD Autoupdater ändern](autoupdate_flatcar_modify.png)

Nachdem Sie auf `Save Changes` geklickt haben, führen alle Worker-Knoten ein rollierendes Update durch und starten neu.

## Zusammenfassung

In dieser Anleitung haben wir Folgendes gelernt:

* Was ist die Auto-Update-Funktion
* Wie man die Auto-Update-Funktion für ein Machine-Deployment aktivieren oder deaktivieren kann

**Weiterführende Themen**

* [Auto-Updating Flatcar Container Linux](https://kinvolk.io/docs/lokomotive/git-main/how-to-guides/auto-update-flatcar/)
* [FLUO on Github](https://github.com/kinvolk/flatcar-linux-update-operator)
* [The Flatcar partitioning scheme](https://kinvolk.io/docs/flatcar-container-linux/latest/reference/developer-guides/sdk-disk-partitions/)
