---
title: S3 Security
lang: de
permalink: /optimist/storage/s3_documentation/security/
nav_order: 3150
parent: S3 Compatible Object Storage
grand_parent: Storage
---

# S3 Security

## Einführung

Diese Seite behandelt die folgenden Themen im Zusammenhang mit S3-Buckets/Swift:

* Container Access Control Lists (ACLs)
* Bucket-Policies

Operationen auf Container-ACLs müssen auf OpenStack-Ebene mit Swift-Befehlen ausgeführt werden, während Bucket-Policies für jeden Bucket innerhalb eines Projekts mit Hilfe der s3cmd-Befehlszeile festgelegt werden müssen. Nachfolgend sind einige Beispiele für sämtliche Operationen aufgeführt.

## Container Access Control Lists (ACLs)

Standardmäßig haben nur Projektbesitzer die Berechtigung, Container und Objekte zu erstellen, zu lesen und zu ändern. Ein Eigentümer (Owner) kann jedoch anderen Benutzern mit Hilfe einer Access Control List (ACL) Zugriff gewähren. Die ACL kann für jeden Container festgelegt werden und gilt nur für diesen Container und die Objekte in diesem Container.

Einige der Hauptelemente, mit denen eine ACL für einen Container festgelegt werden kann, sind nachfolgend aufgeführt:

| **Element**        | **Beschreibung** |
| :----------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `.r: *.`           | Jeder Benutzer hat Zugriff auf Objekte. In der Anforderung ist kein Token erforderlich. |
| `.r: <Referrer>`   | Der Referrer erhält Zugriff auf Objekte. Der Referrer wird durch den Referrer-Anforderungsheader in der Anforderung identifiziert. Es ist kein Token erforderlich. |
| `.r: - <Referrer>` | Diese Syntax (mit "-" vor dem Referrer) wird unterstützt. Der Zugriff wird jedoch nicht verweigert, wenn ein anderes Element (z. B. `.r:*`) den Zugriff gewährt. |
| `.rlistings`       | Jeder Benutzer kann HEAD- oder GET-Operationen für den Container ausführen, wenn der Benutzer auch Lesezugriff auf Objekte hat (z. B. auch `.r: *` oder `.r: <Referrer>`. Es ist kein Token erforderlich. |

Als Beispiel setzen wir die Policy `.r:*.` für einen Container mit dem Namen `"<example-container>"`. Diese Policy ermöglicht jedem externen Benutzer den Zugriff auf die Objekte im Container.

```bash
swift post example-container --read-acl ".r:*"
```

Umgekehrt können wir Benutzern auch erlauben, die Liste der Objekte in einem Container aufzulisten, aber nicht darauf zuzugreifen. Das erreichen wir, indem wir die Policy ".rlistings" für unseren "example-container" festlegen:

```bash
swift post example-container --read-acl ".rlistings"
```

Mit dem folgenden Befehl können die Lese-Policy entfernt und der Container auf den Standardstatus *privat* gesetzt werden:

```bash
swift post -r "" example-container
```

Mit dem folgenden Befehl kann überprüft werden, welche ACL für einen Container festgelegt ist.

```bash
swift stat example-container
```

Hier werden die Statistiken und die aktuelle ACL-Regel für einen Container angezeigt.

### Verhindern des Auflisten auf Container, wenn die Policy `.r: * .` verwendet wird:

In der aktuellen Version von OpenStack empfehlen wir, ein leeres index.html-Objekt im Container zu erstellen. Damit wird verhindert, dass der Inhalt aufgelistet wird, während die Policy `.r: * .` für einen Container verwendet wird. Auf diese Weise können Benutzer Objekte herunterladen, ohne dass der Inhalt der Buckets aufgelistet wird. Dazu sind folgende Schritte nötig:

Zunächst wird die leere Datei `index.html` zu dem `example-container` hinzugefügt:

```bash
swift post -m 'Webindex: index.html' example-container
```

Anschließend wird die Datei index.html als Objekt im Container erstellt:

```bash
touch index.html && openstack object create example-container index.html
```

Dadurch können externe Benutzer auf bestimmte Dateien zugreifen, ohne dass der Inhalt des Containers aufgelistet wird.

## Bucket-Policy

Bucket-Policies werden verwendet, um den Zugriff auf jeden Bucket in einem Projekt zu steuern. Wir empfehlen, bei der Erstellung eine Policy für alle Buckets festzulegen.

Der erste Schritt besteht darin, eine Policy folgendermaßen zu erstellen. Für die folgende Vorlage muss nur der Bucket-Name für die nachfolgenden Policies geändert werden.

Im diesem Beispiel wird eine Policy für den Bucket `example-bucket` erstellt:

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

Erkärung der Elemente des obigen Policy-Beispiels:

* `Version`: Gibt die Sprachsyntaxregeln an, die zum Verarbeiten der Policy verwendet werden sollen. Wir empfehlen, folgende Syntax zu verwenden: "2012-10-17", da dies die aktuelle Version der Policy-Sprache ist.
* `Statement`: Das Hauptelement der Policy. Die anderen Elemente befinden sich in dieser Anweisung.
* `SID`: Die Statement-ID. Dies ist eine optionale Kennung, mit der die Richtlinienanweisung beschrieben werden kann. Sie wird empfohlen, damit der Zweck jeder Policy klar ist.
* `Effect`: Hier muss entweder *Allow* oder *Deny* eingestellt werden.
* `Principal`: Gibt den Principal an, dem der Zugriff auf eine Ressource gestattet oder verweigert wird. Hier wird der Platzhalter "*" verwendet, um die Regel auf alle anzuwenden.
* `Action`: Beschreibt die spezifischen Aktionen, die zugelassen oder abgelehnt werden.

Weitere Informationen zu den verfügbaren Policy-Optionen und zur Anpassung an Ihre spezifischen Anforderungen finden Sie in der [offiziellen AWS-Dokumentation](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html).

Als Nächstes wird die neu erstellte Policy auf `example-bucket` angewandt:

```bash
s3cmd setpolicy examplepolicy s3://example-bucket
```

Anschließend wird der folgende Befehl ausgeführt, um sicherzustellen, dass die Policy vorhanden ist:

```bash
s3cmd info s3://example-bucket
```

Sobald die Policy angewendet wurde, kann im Dashboard erneut folgendes festgelegt werden: `Public Access: Disabled`.

Sobald die oben genannten Schritte ausgeführt wurden, erhält man die folgenden Ergebnisse:

* Der Container ist privat und Dateien werden nicht über XML aufgelistet oder angezeigt.
* Die Policy ermöglicht jetzt den Zugriff auf bestimmte Dateien mit einem direkten Link.
