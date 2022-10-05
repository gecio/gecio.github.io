---
title: Swift - Serving a Static Website
lang: de
permalink: /optimist/storage/s3_documentation/swiftservestaticwebsite/
nav_order: 3160
parent: S3 Kompatiblen Objekt Storage
grand_parent: Storage
---

# Swift - Serving a Static Website

## Einführung

Mit Hilfe der Swift-CLI können die Daten in Containern als statische Website ausgeliefert werden. Die folgende Anleitung enthält die wichtigsten Schritte und ein Beispiel.

## Erste Schritte

### Erstellen eines Containers

Zunächst wird ein Container mit dem Namen `example-webpage` erstellt, der als Basis für diese Anleitung verwendet wird:

```bash
swift post example-webpage
```

### Den Container öffentlich lesbar machen

Als nächstes wird sichergestellt, dass der Container öffentlich lesbar ist. Weitere Informationen zum Sichern von Containern und zum Festlegen von Bucket-Richtlinien finden Sie [hier](/optimist/storage/s3_documentation/security/):

```bash
swift post -r '.r:*' example-webpage
```

### Setzen der Indexdatei der Seite

Nun wird die Indexdatei gesetzt. In diesem Fall ist `index.html` die Standarddatei und wird angezeigt, wenn die Seite erscheint:

```bash
swift post -m 'web-index:index.html' example-webpage
```

### Aktivieren der Dateiliste

Optional kann auch die Dateiliste aktiviert werden. Wenn mehrere Downloads bereitgestellt werden müssen, ist es sinnvoll, die Verzeichnisliste zu aktivieren:

```bash
swift post -m 'web-listings: true' example-webpage
```

### Aktivieren von CSS für Dateilisten

Mit dem folgenden Befehl wird ein benutzerdefiniertes Listing-Stylesheet aktiviert:

```bash
swift post -m 'web-listings-css:style.css' example-webpage
```

### Einrichten von Fehlerseiten

Mit dem folgenden Befehl wird eine benutzerdefinierte Fehlerseite eingebunden:

```bash
swift post -m 'web-error:404error.html' example-webpage
```

## Beispiel-Webseite

Hier ist nochmal eine Zusammenfassung der Schritte, wie statische Webseiten aktiviert werden können:

```bash
swift post example-webpage
swift post -r '.r:*' example-webpage
swift post -m 'web-index:index.html' example-webpage
swift post -m 'web-listings: true' example-webpage
swift post -m 'web-listings-css:style.css' example-webpage
swift post -m 'web-error:404error.html' example-webpage
```

Nachdem die obigen Schritte abgeschlossen sind, kann die statische Webseite angepasst werden. Nachfolgend wird gezeigt, wie man schnell eine Seite mit dem Container `example-webpage` einrichten kann.

### Anpassen der Seiten index.html, page.html und 404error.html

Diese Seite dient als Startseite und erstellt einen Link zu einer sekundären Seite.

```html
<!-- index.html -->
<html>
<h1>
See the web page <a href="mywebsite/page.html">here</a>.
</h1>
</html>
```

Die nächste Seite (page.html) zeigt ein Bild namens `sample.png` an:

```html
<!-- page.html -->
<html>
<img src="sample.png">
</html>
```

Es  können auch benutzerdefinierte Fehlerseiten hinzugefügt werden. Derzeit werden nur die Fehler 401 (Nicht autorisiert) und 404 (Nicht gefunden) unterstützt. Das folgende Beispiel zeigt, wie eine 404-Fehlerseite erstellt wird:

```html
<!-- 404error.html -->
<html>
<h1>
404 Not Found - We cannot find the page you are looking for!
</h1>
</html>
```

### Hochladen der Dateien index.html und page.html

Nachdem die Inhalte der Dateien erstellt wurden, können die Dateien mit den folgenden Befehlen hochgeladen werden:

```bash
swift upload beispiel-webseite index.html
swift upload beispiel-webseite meinewebseite/seite.html
swift upload beispiel-webseite-meine-website/beispiel.png
swift upload beispiel-webseite 404error.html
```

### Betrachten der Website

Wenn alle oben genannten Schritte abgeschlossen sind, kann man die neu erstellte Website anschauen. Der Link zur Website befindet sich im *Optimist Dashboard* → *Object Store* → *Containers* über den abgebildeten Link.

Durch Klicken auf den Link wird die neu erstellte Website angezeigt:

![](attachments/Webpage01.png)

Durch Klicken auf *here*, navigiert man zu der Seite, auf der das Beispielbild hochgeladen wurde:

![](attachments/Webpage02.png)

Für den Fall, dass man zu einer Seite navigiert, die nicht existiert, wird die benutzerdefinierte 404-Seite angezeigt:

![](attachments/Webpage03.png)
