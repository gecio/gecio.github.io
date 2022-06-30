---
title: "03: Einen Stack starten"
lang: "de"
permalink: /optimist/guided_tour/step03/
nav_order: 1030
parent: Guided Tour
---

# Schritt 3: Einen Stack starten

## Einführung

In diesem Schritt lernen Sie, im Horizon Dashboard
einen Stack zu starten.

Wichtige Voraussetzung ist an dieser Stelle ein SSH-Key, den Sie in
[Schritt 2](/optimist/guided_tour/step02/) erzeugt haben.

## Start

Um einen Stack zu starten, melden Sie sich zunächst im Horizon Dashboard
mit den in [Schritt 1](/optimist/guided_tour/step01/) geänderten Zugangsdaten ein.

Navigieren Sie über *Orchestration* zu *Stacks* und klicken Sie anschließend auf *Launch Stack*.

Um den Stack zu starten, benötigen Sie zunächst ein Template,
das in dem Stack eine Instanz startet.

Nutzen Sie dazu die [SingleServer.yaml](https://github.com/gecio/openstack_examples/blob/master/heat/templates/SingleServer/SingleServer.yaml) Datei aus dem [GECio Github Repository](https://github.com/gecio/).

![](attachments/13536111.png)

In dem sich nun öffnenden Fenster, wählen Sie bei *Template Source*
**File** aus und verwenden Sie bei *Template File*, die zuvor heruntergeladene `SingleServer.yaml` Datei.

Belassen Sie den Rest unverändert und klicken Sie auf *Next*.

![](attachments/13536112.png)

Machen Sie nun folgende Eingaben und klicken Sie anschließend auf *Launch*:

- Stack Name: BeispielServer
- Creation Timeout: 60
- Password for User: Tragen Sie hier Ihr eigenes Passwort ein
- availability\_zone: ix1
- flavor\_name: m1.micro
- key\_name: BeispielKey
- machine\_name: singleserver
- public\_network\_id: provider

![](attachments/13536113.png)

Nun wird der Stack direkt gestartet und das Horizon Dashboard
sieht dann so aus:

![](attachments/13536114.png)

Um zu überprüfen, ob die Instanz korrekt gestartet wurde, wechseln Sie im Navigationsmenü zu *Compute* → *Instances*.

Die Übersicht sieht  wie folgt aus:

![](attachments/13536115.png)

Nachdem nun der Stack und die darin enthaltene Instanz
gestartet wurden, löschen Sie jetzt wieder den Stack inklusive der Instanz.

Sie könnten auch die Instanz alleine löschen, das kann aber im Nachgang
zu Problemen beim Löschen des Stacks führen.

Um den Stack zu löschen, wechseln Sie im Navigationsmenü wieder zu
*Orchestration* → *Stacks*.

Klicken Sie im Fenster rechts unter *Actions* auf den Pfeil nach unten und wählen dort *Delete Stack*.

![](attachments/13536116.png)

## Zusammenfassung

Sie haben Ihren ersten Stack erstellt und anschließend wieder gelöscht.
