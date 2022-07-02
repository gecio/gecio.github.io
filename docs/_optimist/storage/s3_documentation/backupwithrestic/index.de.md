---
title: Sichere Datensicherung mit Restic und Rclone
lang: de
permalink: /optimist/storage/s3_documentation/backupwithrestic/
nav_order: 3170
parent: S3 Kompatiblen Objekt Storage
grand_parent: Storage
---
# Sichere Datensicherung mit Restic und Rclone

Restic ist eine sehr einfache und leistungsstarke File-basierte Backup-Lösung, die schnell an Popularität gewinnt. Es kann in Kombination mit S3 verwendet werden, und ist somit ein großartiges Werkzeug für Optimist.

## Problemstellung

Beim Durchführen von Sicherungen (Backups) auf Dateiebene eines Nodes in S3 benötigt die Sicherungssoftware Schreibberechtigungen für den S3-Bucket.

Verschafft sich ein Angreifer jedoch Zugriff auf die Maschine, kann er auch die Backups im Bucket zerstören, da die S3-Anmeldeinformationen auf dem kompromittierten System vorhanden sind.

Eine einfache Lösung könnte sein, die Zugriffsebene der Backup-Software auf den Bucket einzuschränken. Leider ist das bei S3 nicht trivial.

## Hintergrund

[S3-Zugriffskontrolllisten (ACLs)](/optimist/storage/s3_documentation/security) ermöglichen die Verwaltung des Zugriffs auf Buckets und Objekte, sie sind jedoch sehr eingeschränkt und unterscheiden im Wesentlichen READ- und WRITE-Berechtigungen:

* READ – Ermöglicht dem Benutzer, die Objekte im Bucket aufzulisten
* WRITE - Ermöglicht dem Benutzer, jedes Objekt im Bucket zu erstellen, zu überschreiben und zu löschen

Die Einschränkungen von ACLs wurden durch die Zugriffsrichtlinienberechtigungen (ACP) behoben. An den Bucket könnte auch eine No-Delete-Richtlinie angehängt werden, zum Beispiel:

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

Leider wurde bei der Entwicklung des S3-Protokolls nicht das Konzept von WORM-Backups (Write Once Read Many) berücksichtigt. Zugriffsrichtlinienberechtigungen unterscheiden nicht zwischen dem Ändern eines vorhandenen Objekts (was ein effektives Löschen ermöglichen würde) und dem Erstellen eines neuen Objekts.

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

### Unsere Umgebung (Environment)

* `appsrv`: das zu sichernde System, mit Zugriff auf die Sicherungen
* `rclonesrv`: das System, auf dem der rclone-Proxy ausgeführt wird (und nichts anderes, um die Angriffsfläche zu minimieren)

### Einrichtung des rclone-Proxy

1. Installieren von rclone auf `rclonesrv`:

    ```bash
    sudo apt install rclone
    ```

1. Anlegen des Users für rclone:

    ```bash
    sudo useradd -m rcloneproxy
    sudo su - rcloneproxy
    ```

1. Erstellen der Backend-Konfiguration für rclone backend:

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

1. Testen des Zugriffs auf das Repository:

    ```bash
    rclone lsd s3-resticrepo:databucket
            0 2021-11-21 20:02:10        -1 data
            0 2021-11-21 20:02:10        -1 index
            0 2021-11-21 20:02:10        -1 keys
            0 2021-11-21 20:02:10        -1 snapshots
    ```

### Konfiguration des Appservers

1. Generieren eines SSH-Schlüsselpaars auf `appsrv` mit dem Benutzer, mit dem das Backup durchgeführt wird:

    ```bash
    ssh-keygen -o -a 256 -t ed25519 -C "$(hostname)-$(date +'%d-%m-%Y')"
    ```

1. Setzen der Umgebungsvariablen für restic:

    ```bash
    export RESTIC_PASSWORD="MyV3ryS3cUr3r3571cP4ssW0rd"
    export RESTIC_REPOSITORY=rclone:s3-resticrepo:databucket
    ```

### Beschränkung des SSH-Schlüssels auf restic-only Befehle

Als letzter Schritt wird nun die SSH-Datei `authorized_keys` auf dem `rclonesrv` bearbeitet, um den neu generierten SSH-Schlüssel auf einen einzigen Befehl zu beschränken. Auf diese Weise kann ein Angreifer das SSH-Schlüsselpaar nicht verwenden, um beliebige Befehle auf dem rclone-Proxy auszuführen und die Backups zu kompromittieren.

```bash
vi ~/.ssh/authorized_keys
# Fügen Sie einen Eintrag mit dem öffentlichen Schlüssel des restic-Benutzers hinzu, der in dem obigen Schritt generiert wurde:
command="rclone serve restic --stdio --append-only s3-resticrepo:databucket" ssh-ed25519 AAAAC3fdsC1lZddsDNTE5ADsaDgfTwNtWmwiocdT9q4hxcss6tGDfgGTdiNN0z7zN appsrv-18-11-2021
```

## Verwendung von restic mit dem rclone Proxy

Wenn die Umgebungsvariablen gesetzt sind, sollte restic jetzt von dem `appsrv` aus funktionieren.

Beispiel: Sicherung von `/srv/myapp`:

```bash
restic -o rclone.program="ssh rcloneproxy@rclonesrv.mydomain.com" backup /srv/myapp
```

Auflisten der Snapshots:

```bash
restic -o rclone.program="ssh rcloneproxy@rclonesrv.mydomain.com" snapshots
```

Löschen der Snapshots:

```bash
restic -o rclone.program="ssh rcloneproxy@rclonesrv.mydomain.com" forget 2738e969
repository b71c391e opened successfully, password is correct
Remove(<snapshot/2738e9693b>) returned error, retrying after 446.577749ms: blob not removed, server response: 403 Forbidden (403)
```

Das ergibt einen Fehler, was auch unser Ziel war.

## Zusammenfassung

* Der rclone-Proxy auf dem `rclonerv` läuft nicht einmal als Dienst. Er wird nur bei Bedarf für die Dauer der Restic-Operation gestartet. Die Kommunikation erfolgt über HTTP2 über `stdin/stdout` in einem verschlüsselten SSH-Tunnel.
* Da rclone mit `--append-only` läuft, ist es nicht möglich, Snapshots im S3-Bucket zu löschen (oder zu überschreiben).
* Alle Daten (außer Zugangsdaten) werden lokal verschlüsselt/entschlüsselt und dann über `rclonesrv` an/von S3 gesendet/empfangen.
* Alle Zugangsdaten werden **nur** auf `rclonesrv` gespeichert, um mit S3 zu kommunizieren.

Da der Befehl in der SSH-Konfiguration für den SSH-Schlüssel des Benutzers fest eingetragen ist, kann der Schlüssel nicht verwendet werden, um Zugriff auf den rclone-Proxy zu erhalten.

## Noch ein paar Gedanken

Die Vorteile der Lösung liegen auf der Hand. Hier noch ein paar weitere Gedanken dazu:

* Die Verwaltung von Snapshots (sowohl manuell als auch mit einer Aufbewahrungsrichtlinie) ist nur noch auf dem rclone-Proxy möglich.
* Eine einzelne rclone-Proxy-VM (oder sogar ein Docker-Container auf einer isolierten VM) kann mehrere Backup-Clients bedienen.
* Es wird empfohlen, für jeden Server, der Daten sichert, einen eigenen Schlüssel zu verwenden.
* Wenn mehr als ein Repository aus einem Node verwendet werden soll, werden dafür neue SSH-Schlüssel benötigt. Mit `-i ~/.ssh/id_ed25519_another_repo` kann in den `rclone.program`-Argumenten genauso wie bei SSH angegeben werden, welcher Schlüssel verwendet werden soll.
