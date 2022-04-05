---
title: OpenStack Images
lang: de
permalink: /optimist/specs/images/images_overview/
parent: Images
nav_order: 9410
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
- Debian 11 Bullseye - Latest
- Flatcar_Production - Latest
- Ubuntu 16.04 Xenial Xerus - Latest
- Ubuntu 18.04 Bionic Beaver - Latest
- Ubuntu 20.04 Focal Fossa - Latest
- Windows Server 2019 Standard Core - Latest
- Windows Server 2019 Standard GUI - Latest

## Community Images

Neben unseren öffentlichen Images bieten wir auch Community-Images an. Hier finden Sie ältere Versionen der Images in der öffentlichen Liste.

Eine Liste der verfügbaren Community-Images kann über die OpenStack-CLI mit dem folgenden Befehl angezeigt werden:

`$ openstack image list --community`

Die Community-Images werden erst herausgedreht, wenn keine Projekte die Images verwenden. Bitte beachten Sie, dass wir für Community-Images nicht denselben Support bieten können wie für unsere von Anbietern bereitgestellten öffentlichen Images.

Obwohl wir die Verfügbarkeit bestimmter Distributionsversionen nicht garantieren können, können Sie, falls bekannt, den Quell-Hash unserer Community-Images mit dem folgenden Befehl vergleichen:

$ openstack image show ee689d40-c980-40b2-ac6d-e74b948e558b -f json | grep 'source_hash.*'
"source_hash": "525e89ea678d4cf27d2255cb6a39c8f38727799694e89a3fb3ca7e08c17566f5cde81e2d6ceaa08a9e26db502d472e32dfd0e9478906ad22055901c5548c92b2",
