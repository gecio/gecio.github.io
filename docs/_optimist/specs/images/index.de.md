---
title: OpenStack Images
lang: de
permalink: /optimist/specs/images/
parent: Spezifikationen
nav_order: 9400
---

# OpenStack Images

## Einführung

Die Optimist-Plattform stellt eine Liste öffentlicher OpenStack-Images bereit, die regelmäßig aktualisiert wird. Täglich werden automatisierte Prüfungen auf neue Images durchgeführt. Wenn neue Anbieterimages gefunden werden, beschaffen wir die neuen Images, testen und veröffentlichen sie innerhalb von 24 Stunden nach der Herstellerfreigabe.
Ältere Images werden erst dann herausgedreht, wenn alle Benutzer diese Version nicht mehr verwenden.

## Image-liste anzeigen

Eine Liste unserer öffentlichen Images kann über die Openstack-CLI mit dem folgenden Befehl angezeigt werden:

`$ openstack image list`

Alternativ können Sie die Image-liste aus dem [dashboard]([https://dashboard.optimist.innovo.cloud/project/images](https://dashboard.optimist.innovo.cloud/project/images)), unter Projekt > Images und Filtern nach Visbility: Public.

## Übersicht der verfügbaren Images

Hier ist eine Liste unserer derzeit verfügbaren öffentlichen Images:

- CentOS 7 - Latest
- CentOS 8.1 - Latest
- Debian Buster - Latest
- Flatcar_Production - Latest
- Ubuntu 16.04 Xenial Xerus - Latest
- Ubuntu 18.04 Bionic Beaver - Latest
- Ubuntu 20.04 Focal Fossa - Latest
- Windows Server 2019 Standard Core - Latest
- Windows Server 2019 Standard GUI - Latest
