---
title: S3 Kennung erstellen und einlesen
lang: de
permalink: /optimist/storage/s3_documentation/createanduses3credentials/
nav_order: 3110
parent: S3 Kompatiblen Objekt Storage
grand_parent: Storage
---

# S3 Kennung erstellen und einlesen

## Inhalt:

- [Erstellen der Credentials](#credentials-erstellen)
  - [S3cmd](#s3cmd)
  - [S3 Browser](#s3browser)
  - [Cyberduck](#cyberduck)
  - [Boto3](#boto3)

- [Anzeigen der Credentials](#credentials-anzeigen)
- [Löschen der Credentials](#credentials-löschen)

## Erstellen der Credentials

Damit Sie auf den Object Storage zugreifen können, benötigen Sie zunächst die Login Daten (Credentials).
Um diese Daten mit OpenStackAPI erzeugen zu können, benötigen Sie den OpenStackClient. Führen Sie dort den folgenden Befehl aus:

`$ openstack ec2 credentials create`

Wenn die Daten korrekt erstellt worden sind, sieht die Ausgabe in etwa so aus:

```bash
$ openstack ec2 credentials create
+------------+-----------------------------------------------------------------+
| Field      | Value                                                           |
+------------+-----------------------------------------------------------------+
| access     | aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa                                 |
| links      | {u'self': u'https://identity.optimist.innovo.cloud/v3/users/bbb |
|            | bbbbbbbbbbbbbbbbbbbbbbbbbbbbb/credentials/OS-                   |
|            | EC2/aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa'}                           |
| project_id | cccccccccccccccccccccccccccccccc                                |
| secret     | dddddddddddddddddddddddddddddddd                                |
| trust_id   | None                                                            |
| user_id    | bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb                                |
+------------+-----------------------------------------------------------------+
```

Nachdem die Zugangsdaten (Credentials) vorliegen, brauchen Sie eine Möglichkeit, auf den S3 kompatiblen ObjectStorage zugreifen zu können.
Hierfür gibt es verschiedene Optionen. Hier sind vier aufgelistet:

- [S3cmd](https://s3tools.org/s3cmd) für Linux/Mac
- [S3Browser](https://s3browser.com/) für  Windows
- [Cyberduck](https://cyberduck.io/)
- [Boto3](https://boto3.amazonaws.com/v1/documentation/api/latest/index.html)

## Benutzerdaten in die Konfigurationsdatei eintragen

### S3cmd

Um s3cmd zu installieren, brauchen Sie einen Paketmanager, zum Beispiel *pip*. Die Installation und Nutzung wird in [Schritt 4](/optimist/guided_tour/step04/) der Guided Tour erklärt.
Der Befehl für die Installation lautet:

```bash
pip install s3cmd
```

Nach der erfolgreichen Installation von s3cmd, müssen Sie die vorher erstellten Zugangsdaten (Credentials) in die Konfigurationsdatei von s3cmd eintragen.
Die dafür zuständige Datei `.s3cfg` befindet sich standardmäßig im Homeverzeichnis. Sollte diese noch nicht existieren, müssen Sie diese vorher erstellen.

Tragen Sie die folgenden Daten in `.s3cfg` ein und speichern diese:

```bash
access_key = aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
check_ssl_certificate = True
check_ssl_hostname = True
host_base = s3.es1.fra.optimist.innovo.cloud
host_bucket = s3.es1.fra.optimist.innovo.cloud
secret_key = dddddddddddddddddddddddddddddddd
use_https = True
```

### S3 Browser

Für den S3 Browser genügt es, diesen [herunterzuladen](https://s3browser.com/) und zu installieren.
Nach der erfolgreichen Installation müssen Sie die entsprechenden Daten hinterlegen.
Öffnen Sie hierfür den S3 Browser. Beim ersten Starten öffnet sich automatisch das folgende Fenster:

![](attachments/CreateAndUseS3Credentials_S3Browser.png)

Tragen Sie die folgenden Werte ein:

```text
* Account Name: Frei wählbarer Name für den Account
* Account Type: S3 Compatible Storage
* REST Endpoint: s3.es1.fra.optimist.innovo.cloud
* Signature Version: Signature V2
* Access Key ID: Den entsprechenden Access Key (im Beispiel: aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa)
* Secret Access Key: Das entsprechende Secret (im Beispiel: dddddddddddddddddddddddddddddddd)
```

Klicken Sie auf *Add new account*.

### Cyberduck

Um Cyberduck nutzen zu können, müssen Sie diesen installieren. Sie können ihn [hier](https://cyberduck.io/) herunterladen.
Nach der Installation und dem ersten Öffnen, klicken Sie auf *Neue Verbindung*  (1).
Danach öffnet sich ein neues Fenster. Wählen Sie im Dropdown Menü *Amazon S3* (2) aus und geben Sie die folgenden Daten ein:

- Server(3): s3.es1.fra.optimist.innovo.cloud
- Access Key ID(4): Den entsprechenden Access Key (im Beispiel: aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa)
- Secret Access Key(5): Das entsprechende Secret (im Beispiel: dddddddddddddddddddddddddddddddd)

![](attachments/CreateAndUseS3Crendentials_Cyberduck.png)

Um eine Verbindung herzustellen, klicken Sie im letzten Schritt auf *Verbinden*.

### Boto3

Um Boto3 nutzen zu können, benötigen Sie einen Paketmanager, zum Beispiel *pip*. Die Installation und Nutzung wird in [Schritt 4](/optimist/guided_tour/step04/) der Guided Tour erklärt.
Der Befehl für die Installation lautet:

```bash
pip install boto3
```

Nach der erfolgreichen Installation von boto3 können Sie das Tool nun nutzen.
Wichtig ist, dass bei boto3 ein Script erstellt wird, welches am Ende ausgeführt wird.
Daher ist der Konfigurationsteil, der im Anschluss gezeigt wird, später immer Teil der weiterführenden Scripte.
Hierfür erstellen Sie eine Python-Datei, z.B. "Beispiel.py" und fügen dort den folgenden Inhalt ein:

- endpoint_url: s3.es1.fra.optimist.innovo.cloud
- aws_access_key_id: Den entsprechenden Access Key (Im Beispiel: aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa)
- aws_secret_access_key: Das entsprechende Secret (Im Beispiel: dddddddddddddddddddddddddddddddd)

```python
#!/usr/bin/env/python

import boto3
from botocore.client import Config

s3 = boto3.resource('s3',
                        endpoint_url='https://s3.es1.fra.optimist.innovo.cloud',
                        aws_access_key_id='aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa',
                        aws_secret_access_key='dddddddddddddddddddddddddddddddd',
                    )
```

Dies dient als Startpunkt und wird in den folgenden Skripten referenziert und verwendet.

## Anzeigen der Credentials

Um erstellte Object Storage EC2-Credentials anzuzeigen, benötigen Sie den OpenstackClient. Führen Sie dort den folgenden Befehl aus:

`$ openstack ec2 credentials list`

Der Befehl erstellt eine Liste mit allen EC2 Credentials, die für den aktuellen Nutzer sichtbar sind.

```bash
$ openstack ec2 credentials list
+----------------------------------+----------------------------------+----------------------------------+----------------------------------+
| Access                           | Secret                           | Project ID                       | User ID                          |
+----------------------------------+----------------------------------+----------------------------------+----------------------------------+
| aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa | xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx | 12341234123412341234123412341234 | 32132132132132132132132132132132 |
| bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb | yyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyy | 56756756756756756756756756756756 | 65465465465465465465465465465465 |
| cccccccccccccccccccccccccccccccc | zzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzz | 89089089089089089089089089089089 | 09809809809809809809809809809809 |
+----------------------------------+----------------------------------+----------------------------------+----------------------------------+
```

## Löschen der Credentials

Um vorhandene Object Storage EC2-Credentials zu löschen, benötigen Sie den OpenstackClient. Führen Sie dort den folgenden Befehl aus:

`$ openstack ec2 credentials delete <access-key>`
