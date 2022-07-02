---
title: Application Credentials
lang: de
permalink: /optimist/specs/application_credentials/
parent: Spezifikationen
nav_order: 9300
---

# Einführung

Benutzer können Application Credentials erstellen, damit sich ihre Anwendungen bei der OpenStack-Komponente Keystone authentifizieren können, ohne die eigenen Anmeldeinformationen des Benutzers verwenden zu müssen.

Mit Application Credentials können sich Anwendungen mit der Application Credential-ID und einer geheimen Zeichenfolge authentifizieren, die nicht das Kennwort des Benutzers ist. Auf diese Weise wird das Passwort des Benutzers nicht in die Konfiguration der Anwendung eingebettet.

Benutzer können eine Teilmenge ihrer Rollenzuweisungen für ein Projekt an Application Credentials delegieren und der Anwendung dieselben oder eingeschränkte Berechtigungen innerhalb eines Projekts erteilen.

## Anforderungen für Application Credentials

### Name / Secrets

Die Benutzer können die Application Credentials für ihr Projekt über die Befehlszeile oder über das Dashboard generieren. Diese werden dem Projekt zugeordnet, in dem sie erstellt werden.

Der einzige erforderliche Parameter zum Erstellen der Anmeldeinformationen ist ein Name, jedoch kann mit dem Parameter `—-secret` ein bestimmtes Secret festgelegt werden. Ohne Parameter wird automatisch ein Secret in der Ausgabe generiert.

Es ist in jedem Fall wichtig, das Secret zu notieren, da dieses vor dem Speichern gehasht wird und nach dem Festlegen nicht wiederhergestellt werden kann. Wenn das Secret verloren geht, müssen neue Application Credentials für die Anwendung erstellt werden.

### Roles

Wir empfehlen außerdem, die Rollen (Roles) festzulegen, die die Application Credentials der Anwendung im Projekt haben sollen, da standardmäßig ein neu erstellter Satz von Anmeldeinformationen alle verfügbaren Rollen erbt.

Im Folgenden sind die verfügbaren Rollen aufgeführt, die einem Satz von Application Credentials zugewiesen werden können. Wenn diese Rollen mithilfe des Parameters `--role` auf einen Satz von Application Credentials angewendet werden, ist es wichtig, bei allen Rollen-Namen die Groß-/Kleinschreibung zu beachten.

- `Member`: Die Rolle "Member" hat nur administrativen Zugriff auf das zugewiesene Projekt.
- `heat_stack_owner`: Die Rolle "heat_stack_owner" kann vorhandene HEAT-Templates verwenden und ausführen.
- `load-balancer_member`: Die Rolle "load-balancer_member" kann die Octavia LoadBalancer-Ressourcen nutzen.

### Expiration

Standardmäßig laufen erstellte Application Credentials nicht ab, jedoch können feste Ablaufdaten/-zeiten für Application Credentials bei der Erstellung festgelegt werden. Dazu wird der Parameter `--expires` im Befehl verwendet (zum Beispiel: `--expires '2021-07-15T21: 00:00'`).

## Erstellen von Application Credentials über die CLI

Ein Set von Application Credentials kann im gewünschten Projekt über die CLI erstellt werden. Das folgende Beispiel zeigt, wie ein Set von Credentials mit den folgenden Parametern erstellt wird:

- Name: test-credentials
- Secret: ZYQZm2k6pk
- Roles: Member, heat_stack_owner, load-balancer_member
- Expiration Date/Time: 2021-07-12 at 21:00:00

Die neuen Zugangsdaten sollten wie folgt aussehen:

```bash
$ openstack application credential create test-credentials --secret ZYQZm2k6pk --role Member --role heat_stack_owner --role load-balancer_member --expires '2021-07-15T21:00:00'
+--------------+----------------------------------------------+
| Field        | Value                                        |
+--------------+----------------------------------------------+
| description  | None                                         |
| expires_at   | 2021-07-15T21:00:00.000000                   |
| id           | 707d14e835124b4f957938bb5a57d1be             |
| name         | test-credentials                             |
| project_id   | c704ac5a32b84b54a0407d28ad448399             |
| roles        | Member heat_stack_owner load-balancer_member |
| secret       | ZYQZm2k6pk                                   |
| system       | None                                         |
| unrestricted | False                                        |
| user_id      | 1d9f1ecb5de3607e8982695f72036fa5             |
+--------------+----------------------------------------------+
```

Hinweis: Das Secret (ob vom Benutzer festgelegt oder automatisch generiert) wird nur beim Erstellen der Application Credentials angezeigt. Es ist wichtig, das Secret zu diesem Zeitpunkt zu notieren.

## Anzeigen von Application Credentials über die CLI

Die Liste der zu einem Projekt gehörenden Application-Credentials kann mit dem folgenden Befehl aufgelistet werden.

```bash
$ openstack application credential list
+----------------------------------+-------------------+----------------------------------+-------------+------------+
| ID                               | Name              | Project ID                       | Description | Expires At |
+----------------------------------+-------------------+----------------------------------+-------------+------------+
| 707d14e835124b4f957938bb5a57d1be | test-credentials  | c704ac5a32b84b54a0407d28ad448399 | None        | None       |
+----------------------------------+-------------------+----------------------------------+-------------+------------+
```

Einzelne Credentials können mit dem `$ openstack application credential show <name>` Befehl angezeigt werden.

## Löschen von Application Credentials über die CLI

Application Credentials können über die CLI mit dem folgenden Befehl von Anmeldeinformationen gelöscht werden. Dazu wird im Befehl der Name oder die ID des spezifischen Satzes verwendet.

```bash
openstack application credential delete test-credentials
```

## Erstellen und Löschen von Application Credentials für Anwendungen über das Optimist Dashboard

Alternativ können Application Credentials auch über das Optimist Dashboard unter *Identität* → *Application Credentials* generiert werden:

![](attachments/createappcredentials.png)

Hinweis: Hier können die Benutzer mehrere Rollen auswählen, indem sie die Umschalttaste gedrückt halten und durch die Optionen navigieren.

Nach der Erstellung wird ein Dialogfeld angezeigt mit der Aufforderung, die ID und das Secret zu notieren. Danach klickt man auf "Close".

![](attachments/secretappcredentials.png)

Die Zugangsdaten hier können jederzeit gelöscht werden, indem die Benutzer den zu löschenden Zugangsdatensatz markieren und auf "DELETE APPLICATION CREDENTIAL" klicken.

![](attachments/deleteappcredentials.png)

## Application Credentials testen

Sobald über die CLI oder das Dashboard ein Satz von Application Credentials erstellt wurde, können Benutzer mit dem folgenden curl-Befehl überprüfen, ob sie funktionieren.

Dazu müssen die Benutzer ihr `<name>` und `<secret>` im curl-Befehl verwenden:

```bash
curl -i -H "Content-Type: application/json" -d ' { "auth": { "identity": { "methods": ["application_credential"],  "application_credential": {  "id": “<id>", "secret": “<secret>"}}}}' https://identity.optimist.innovo.cloud/v3/auth/tokens
```

Ein erfolgreicher curl-Versuch gibt ein `x-subject-token` aus, erfolglose Versuche mit falschen Anmeldeinformationen führen zu einem 401-Fehler.
