---
title: S3 Security
lang: "de"
permalink: /optimist/storage/s3_documentation/security/
nav_order: 3150
parent: S3 Compatible Object Storage
grand_parent: Storage
---

# S3 Security

## Einführung

Diese Seite bietet einen Überblick über die folgenden Themen im Zusammenhang mit S3-Buckets / Swift:

* Container Access Control Lists (ACLs)
* Bucket-Policies

Operationen auf Container-ACLs müssen auf OpenStack-Ebene mit Swift-Befehlen ausgeführt werden, während Bucket-Policies für jeden Bucket innerhalb eines Projekts mit Hilfe der s3cmd-Befehlszeile festgelegt werden müssen. In diesem Dokument werden einige Beispiele für jede Art von Operation beschrieben.

## Container Access Control Lists (ACLs)

Standardmäßig haben nur Projektbesitzer die Berechtigung, Container und Objekte zu erstellen, zu lesen und zu ändern. Ein Eigentümer kann jedoch anderen Benutzern mit Hilfe einer Access Control List (ACL) Zugriff gewähren. Die ACL kann für jeden Container festgelegt werden und gilt nur für diesen Container und die Objekte in diesem Container.

Einige der Hauptelemente, mit denen eine ACL für einen Container festgelegt werden kann, sind nachfolgend aufgeführt:

| **Element**        | **Beschreibung** |
| :----------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `.r: *.`           | Jeder Benutzer hat Zugriff auf Objekte. In der Anforderung ist kein Token erforderlich. |
| `.r: <Referrer>`   | Der Referrer erhält Zugriff auf Objekte. Der Referrer wird durch den Referer-Anforderungsheader in der Anforderung identifiziert. Es ist kein Token erforderlich. |
| `.r: - <Referrer>` | Diese Syntax (mit "-" vor dem Referrer) wird unterstützt. Der Zugriff wird jedoch nicht verweigert, wenn ein anderes Element (z. B. `.r:*`) den Zugriff gewährt. |
| `.rlistings`       | Jeder Benutzer kann HEAD- oder GET-Operationen für den Container ausführen, wenn der Benutzer auch Lesezugriff auf Objekte hat (z. B. auch `.r: *` oder `.r: <Referrer>`. Es ist kein Token erforderlich. |

Als Beispiel setzen wir die Policy `.r:*.` für einen Container mit dem Namen `"<example-container>"`. Diese Policy ermöglicht jedem externen Benutzer den Zugriff auf die Objekte im Container.

```bash
swift post example-container --read-acl ".r:*"
```

Umgekehrt können wir Benutzern auch erlauben, die Liste der Objekte in einem Container aufzulisten, aber nicht darauf zuzugreifen, indem wir die Policy ".rlistings" für unseren "example-container" festlegen:

```bash
swift post example-container --read-acl ".rlistings"
```

Der folgende Befehl kann verwendet werden, um die Lese-Policy zu entfernen und den Container auf den Standardstatus "privat" zu setzen:

```bash
swift post -r "" example-container
```

Verwenden Sie den folgenden Befehl, um zu überprüfen, welche ACL für einen Container festgelegt ist.

```bash
swift stat example-container
```

Dies gibt einen Überblick über die Statistiken für den Container und zeigt die aktuelle ACL-Regel für einen Container an.

### Verhindern Sie das Auflisten auf Containern, wenn Sie die Policy `.r: * .` verwenden:

In der aktuellen Version von OpenStack empfehlen wir, ein leeres index.html-Objekt im Container zu erstellen, um zu verhindern, dass der Inhalt aufgelistet wird, während die Policy `.r: * .` für einen Container verwendet wird. Auf diese Weise können Benutzer Objekte herunterladen, ohne den Inhalt der Buckets aufzulisten zu koennen. Dies kann mit den folgenden Schritten erreicht werden:

Fügen Sie zunächst die leere Datei `index.html` zu unserem `example-container` hinzu:

```bash
swift post -m 'Webindex: index.html' example-container
```

Erstellen Sie dann die Datei index.html als Objekt im Container:

```bash
touch index.html && openstack object create example-container index.html
```

Dadurch können externe Benutzer auf bestimmte Dateien zugreifen, ohne den Inhalt des Containers aufzulisten.

## Bucket-Policy

Bucket-Policies werden verwendet, um den Zugriff auf jeden Bucket in einem Projekt zu steuern. Es wird empfohlen, bei der Erstellung eine Policy für alle Buckets festzulegen.

Der erste Schritt besteht darin, eine Policy wie folgt zu erstellen. Für die folgende Vorlage muss nur der Bucket-Name für nachfolgende Policy geändert werden. Im folgenden Beispiel wird eine Policy für den Bucket `example-bucket` erstellt:

```bash
cat > examplepolicy
{
    "Version": "2008-10-17",
    "Statement": [
        {
        "Sid": "AddPerm",
        "Effect": "Allow",
        "Principal": "*",
        "Action": "s3:GetObject",
        "Resource": "arn:aws:s3:::example-bucket/*"
        }
    ]
}
```

Aufschlüsselung jedes Elements innerhalb des obigen Policy-Beispiels:

* `Version`: Gibt die Sprachsyntaxregeln an, die zum Verarbeiten der Policy verwendet werden sollen. Es wird empfohlen, immer Folgendes zu verwenden: "2012-10-17", da dies die aktuelle Version der Policy-Sprache ist.
* `Statement`: Das Hauptelement der Policy, die anderen Elemente befinden sich in dieser Anweisung.
* `SID`: Die Statement-ID. Dies ist eine optionale Kennung, mit der die Richtlinienanweisung beschrieben werden kann. Empfohlen, damit der Zweck jeder Policy klar ist.
* `Effect`: Stellen Sie entweder "Allow" oder "Deny" ein.
* `Principal`: Gibt den Principal an, dem der Zugriff auf eine Ressource gestattet oder verweigert wird. Hier wird der Platzhalter "*" verwendet, um die Regel auf alle anzuwenden.
* `Action`: Beschreibt die spezifischen Aktionen, die zugelassen oder abgelehnt werden.

(Weitere Informationen zu den verfügbaren Policy-Optionen und zur Anpassung an Ihre spezifischen Anforderungen finden Sie in der [offiziellen AWS-Dokumentation](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html)).

Wenden Sie als Nächstes die neu erstellte Policy auf `example-bucket` an:

```bash
s3cmd setpolicy examplepolicy s3://example-bucket
```

Anschließend können Sie den folgenden Befehl ausführen, um sicherzustellen, dass die Policy vorhanden ist:

```bash
s3cmd info s3://example-bucket
```

Sobald die Policy angewendet wurde, können Sie im Dashboard erneut festlegen: `Public Access: Disabled`.

Sobald die oben genannten Schritte ausgeführt wurden, erhalten wir die folgenden Ergebnisse:

* Der Container ist privat und Dateien werden nicht über XML aufgelistet oder angezeigt.
* Die Policy ermöglicht jetzt den Zugriff auf bestimmte Dateien mit einem direkten Link.
