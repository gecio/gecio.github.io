---
title: Sichere Datensicherung mit Restic und Rclone
lang: "de"
permalink: /optimist/storage/s3_documentation/backupwithrestic/
nav_order: 3170
parent: S3 Kompatiblen Objekt Storage
grand_parent: Storage
---

Restic ist eine sehr einfache und leistungsstarke file-basierte Backup-Lösung, die schnell an Popularität gewinnt. Es kann in Kombination mit S3 verwendet werden, was es zu einem großartigen Werkzeug für Optimist macht.

## Problemstellung

Beim Durchführen von Sicherungen auf Dateiebene eines Nodes in S3 benötigt die Sicherungssoftware Schreibberechtigungen für den S3-Bucket.

Verschafft sich ein Angreifer jedoch Zugriff auf die Maschine, kann er auch die Backups im Bucket zerstören, da die S3-Anmeldeinformationen auf dem kompromittierten System vorhanden sind.

Die Lösung kann so einfach sein, wie die Zugriffsebene der Backup-Software auf den Bucket einzuschränken. Leider ist das bei S3 nicht trivial.

## Hintergrund

[S3-Zugriffskontrolllisten (ACLs)](/optimist/storage/s3_documentation/security) ermöglichen Ihnen die Verwaltung des Zugriffs auf Buckets und Objekte, sind jedoch sehr eingeschränkt. Sie unterscheiden im Wesentlichen READ- und WRITE-Berechtigungen:

* READ – Ermöglicht dem Benutzer, die Objekte im Bucket aufzulisten
* WRITE - Ermöglicht dem Benutzer, jedes Objekt im Bucket zu erstellen, zu überschreiben und zu löschen

Die Einschränkungen von ACLs wurden durch die Zugriffsrichtlinienberechtigungen (ACP) behoben. Wir könnten dem Bucket eine No-Delete-Richtlinie anhängen, z.B.

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "nodelete1",
            "Effect": "Deny",
            "Action": [
                "s3:DeleteBucket",
                "s3:DeleteBucketPolicy",
                "s3:DeleteBucketWebsite",
                "s3:DeleteObject",
                "s3:DeleteObjectVersion"
            ],
            "Resource": [
                "arn:aws:s3:::*"
            ]
        }
    ]
}
```

Leider wurde das S3-Protokoll selbst nicht mit dem Konzept von WORM-Backups (Write Once Read Many) im Hinterkopf entwickelt. Zugriffsrichtlinienberechtigungen unterscheiden nicht zwischen dem Ändern eines vorhandenen Objekts (was ein effektives Löschen ermöglichen würde) und dem Erstellen eines neuen Objekts.

Das Anhängen der obigen Richtlinie an einen Bucket verhindert nicht das Überschreiben der darin enthaltenen Objekte.

```bash
$ s3cmd put testfile s3://appendonly-bucket/testfile
upload: 'testfile' -> 's3://appendonly-bucket/testfile' [1 of 1]
 1054 of 1054 100% in 0s 5.46 KB/s done     # die Richtlinie erlaubt Schreiben
$ s3cmd rm s3://appendonly-bucket/testfile
ERROR: S3 error: 403 (AccessDenied)         # die Richtlinie verweigert das Löschen
$
$ s3cmd put testfile s3://appendonly-bucket/testfile
upload: 'testfile' -> 's3://appendonly-bucket/testfile' [1 of 1]
 1054 of 1054 100% in 0s 5.50 KB/s done     # :(
```

## Vorgeschlagene Lösung

Da ein Angreifer auf einem kompromittierten System immer Zugriff auf die S3-Anmeldeinformationen und jeden auf dem System laufenden Dienst hat – einschließlich Restic selbst, Proxys usw. – benötigen wir eine zweite, abgesicherte VM, die Löschvorgänge einschränken kann. Restic lässt sich perfekt in rclone integrieren, daher verwenden wir es in diesem Beispiel.

### Our environment

* `appsrv`: das zu sichernde System, mit Zugriff auf die Sicherungen
* `rclonesrv`: das System, auf dem der rclone-Proxy ausgeführt wird (und nichts anderes, um die Angriffsfläche zu minimieren)

### Den rclone-Proxy einrichten

1. rclone auf dem `rclonesrv` installieren:

    ```bash
    sudo apt install rclone
    ```

1. User für rclone anlegen

    ```bash
    sudo useradd -m rcloneproxy
    sudo su - rcloneproxy
    ```

1. Die Backend-Konfiguration für rclone backend erstellen

    ```bash
    mkdir -p .config/rclone
    cat << EOF > .config/rclone/rclone.conf
    [s3-resticrepo]
    type = s3
    provider = Other
    env_auth = false
    access_key_id = 111122223333444455556666
    secret_access_key = aaaabbbbccccddddeeeeffffgggghhhh
    region = eu-central-1
    endpoint = s3.es1.fra.optimist.innovo.cloud
    acl = private
    bucket_acl = private
    upload_concurrency = 8
    EOF
    ```

1. Den Zugriff auf das Repository testen:

    ```bash
    rclone lsd s3-resticrepo:databucket
            0 2021-11-21 20:02:10        -1 data
            0 2021-11-21 20:02:10        -1 index
            0 2021-11-21 20:02:10        -1 keys
            0 2021-11-21 20:02:10        -1 snapshots
    ```

### Den Appserver konfigurieren

1. Ein SSH-Schlüsselpaar auf `appsrv` mit dem Benutzer generieren, mit dem Sie das Backup durchführen:

    ```bash
    ssh-keygen -o -a 256 -t ed25519 -C "$(hostname)-$(date +'%d-%m-%Y')"
    ```

1. Die Umgebungsvariablen für restic setzen:

    ```bash
    export RESTIC_PASSWORD="MyV3ryS3cUr3r3571cP4ssW0rd"
    export RESTIC_REPOSITORY=rclone:s3-resticrepo:databucket
    ```

### Den SSH-Schlüssel auf restic-only Befehle beschränken

Der letzte Schritt ist nun die SSH-Datei `authorized_keys` auf dem `rclonesrv` zu bearbeiten, um den neu generierten SSH-Schlüssel auf einen einzigen Befehl zu beschränken. Auf diese Weise kann ein Angreifer das SSH-Schlüsselpaar nicht verwenden, um beliebige Befehle auf dem rclone-Proxy auszuführen und die Backups zu kompromittieren.

```bash
vi ~/.ssh/authorized_keys
# Fügen Sie inen Eintrag mit dem öffentlichen Schlüssel des restic-Benutzers hinzu, der in dem obigen Schritt generiert wurde:
command="rclone serve restic --stdio --append-only s3-resticrepo:databucket" ssh-ed25519 AAAAC3fdsC1lZddsDNTE5ADsaDgfTwNtWmwiocdT9q4hxcss6tGDfgGTdiNN0z7zN appsrv-18-11-2021
```

## restic mit dem rclone Proxy verwenden

Wenn die Umgebungsvariablen gesetzt sind, sollte restic jetzt von dem `appsrv` aus funktionieren.

Beispiel: Sicherung von `/srv/myapp`:

```bash
restic -o rclone.program="ssh rcloneproxy@rclonesrv.mydomain.com" backup /srv/myapp
```

Snapshots auflisten:

```bash
restic -o rclone.program="ssh rcloneproxy@rclonesrv.mydomain.com" snapshots
```

Snapshots löschen:

```bash
restic -o rclone.program="ssh rcloneproxy@rclonesrv.mydomain.com" forget 2738e969
repository b71c391e opened successfully, password is correct
Remove(<snapshot/2738e9693b>) returned error, retrying after 446.577749ms: blob not removed, server response: 403 Forbidden (403)
```

Ah, das geht nicht. Das war ja unser Ziel!

## Zusammenfassung

Auf dieser Art und Weise,

* der rclone-Proxy auf dem `rclonerv` läuft nicht einmal als Dienst. Es wird nur bei Bedarf für die Dauer der Restic-Operation gestartet. Die Kommunikation erfolgt über HTTP2 über stdin/stdout in einem verschlüsselten SSH-Tunnel.
* Da rclone mit `--append-only` läuft, ist es nicht möglich, Snapshots im S3-Bucket zu löschen (oder zu überschreiben).
* Alle Daten (außer Zugangsdaten) werden lokal verschlüsselt/entschlüsselt und dann über `rclonesrv` an/von S3 gesendet/empfangen.
* Alle Zugangsdaten werden **nur** auf `rclonesrv` gespeichert, um mit S3 zu kommunizieren.

Da der Befehl in der SSH-Konfiguration für den SSH-Schlüssel des Benutzers fest eingetragen ist, gibt es keine Möglichkeit, den Schlüssel zu verwenden, um Zugriff auf den rclone-Proxy zu erhalten.

## Noch ein paar Gedanken

Die Vorteile der Lösung sind wahrscheinlich schon klar. Im Weiteren,

* die Verwaltung von Snapshots (sowohl manuell als auch mit einer Aufbewahrungsrichtlinie) ist nur noch auf dem rclone-Proxy möglich.
* Eine einzelne rclone-Proxy-VM (oder sogar ein Docker-Container auf einer isolierten VM) kann mehrere Backup-Clients bedienen.
* Es ist sehr empfohlen, für jeden Server, der Daten sichert, einen eigenen Schlüssel zu verwenden.
* Wenn Sie mehr als ein Repository aus einem Node verwenden möchten, benötigen Sie dafür neue SSH-Schlüssel. Sie können dann mit `-i ~/.ssh/id_ed25519_another_repo` in den `rclone.program`-Argumenten genau wie bei SSH angeben, welcher Schlüssel verwendet werden soll.
