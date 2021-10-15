---
title: Aktualisierung des Betriebssystems auf Worker-Nodes
lang: de
permalink: /imke/machinedeployments/updatingnodeos/
nav_order: 5500
parent: Machine Deployments
---
<!-- LTeX:  language=de-DE -->

## Flatcar

### Flatcar Worker-Nodes automatisch aktualisieren

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

#### Prüfen der Auto-Updater-Einstellungen eines Clusters

Um zu prüfen, ob Ihre Knoten automatische Betriebssystemaktualisierungen erhalten, klicken Sie auf das Machine-Deployment:

![Machine Deployment öffnen](autoupdate_open_md.png)

und kontrollieren Sie, ob vor dem Kästchen `Disable auto-update` ein grünes Häkchen angezeigt wird (die automatische Aktualisierung ist deaktiviert):

![Autoupdater an](autoupdate_enabled.png)

oder ob es ausgegraut ist (die automatische Aktualisierung ist an):

![Autoupdater aus](autoupdate_disabled.png)

#### Aktivieren / Deaktivieren des automatischen Updaters für ein vorhandenes Machine-Deployment

Um den Status des automatischen Updaters zu ändern, klicken Sie auf die Schaltfläche `Edit Machine Deployment` der Machine-Deployment:

![Edit Machine Deployment](autoupdate_edit_md.png)

und (de) -aktivieren Sie das Kontrollkästchen entsprechend:

![MD Autoupdater ändern](autoupdate_flatcar_modify.png)

Nachdem Sie auf `Save Changes` geklickt haben, führen alle Worker-Knoten ein rollierendes Update durch und starten neu.

### Flatcar Worker-Nodes manuell aktualisieren

Um einen Flatcar Worker-Node manuell aktualisieren zu können, wird [SSH-Zugriff](/imke/machinedeployments/add_ssh_key/) benötigt.

Die aktuelle OS-Version findet man unter `/etc/os-release`:

```bash
$ grep VERSION_ID /etc/os-release
VERSION_ID=2765.2.2
```

Im nächsten Schritt müssen wir den Dienst `update-engine` demaskieren und starten:

```bash
$ sudo systemctl unmask update-engine.service
Removed /etc/systemd/system/update-engine.service.
$ sudo systemctl start update-engine.service
```

Nun können wir nach verfügbaren Updates suchen und sie installieren lassen:

```bash
$ sudo update_engine_client -check_for_update
$ sudo update_engine_client -status
```

Der Update-Engine-Client lädt jetzt die letzte verfügbare Version von Flatcar herunter und passt
automatisch die Boot-Reihenfolge so an, dass beim nächsten Boot schon die neue Version gebootet wird.

![Update Engine](fc_update_engine.gif)

Wenn der Status sich von `UPDATE_STATUS_UPDATE_AVAILABLE` in `UPDATE_STATUS_DOWNLOADING`,
und dann in `UPDATE_STATUS_UPDATED_NEED_REBOOT` geändert hat, können wir den Worker Node rebooten
und den Vorgang für alle Worker Nodes durchführen.

````bash
$ sudo systemctl reboot
````

> Wir empfehlen unseren Benutzern dringend, die Auto-Updater-Funktion zu verwenden, um die Sicherheit Ihrer Infrastruktur zu gewährleisten.

## Ubuntu

Ubuntu wurde im Juli 2021 aus dem Support für iMKE genommen, bitte aktualisieren Sie Ihre bestehenden Machine Deployments.

### Auf Flatcar aktualisieren

Um auf Flatcar zu aktualisieren, klicken Sie bitte den `Machine Deployment Edit` Button.

![Edit Machine Deployment](update_to_flatcar_edit.png)

Dann auf das Flatcar Logo klicken.

![Change to Flatcar](update_to_flatcar.png)

Es wurde das Image gewechselt und die Autoupdate Option angezeigt.

![Save Flatcar](update_to_flatcar_save.png)

Die Nodes werden nun neu gebaut und der Cluster ist wieder aktuell.

## Zusammenfassung

In dieser Anleitung haben wir Folgendes gelernt:

* Was ist die Auto-Update-Funktion
* Wie man die Auto-Update-Funktion für ein Flatcar Machine-Deployment aktivieren oder deaktivieren kann
* Wie man einen Flatcar Worker-Node manuell aktualisieren kann
* Wie man die automatische Reboots der Ubuntu Worker-Nodes aktiviert

**Weiterführende Themen**

* [Auto-Updating Flatcar Container Linux](https://kinvolk.io/docs/lokomotive/git-main/how-to-guides/auto-update-flatcar/)
* [FLUO on Github](https://github.com/kinvolk/flatcar-linux-update-operator)
* [The Flatcar partitioning scheme](https://kinvolk.io/docs/flatcar-container-linux/latest/reference/developer-guides/sdk-disk-partitions/)
