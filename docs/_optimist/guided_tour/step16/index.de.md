---
title: "16: Wir lernen Heat besser kennen"
lang: "de"
permalink: /optimist/guided_tour/step16/
nav_order: 1160
parent: Guided Tour
---

# Schritt 16: Wir lernen Heat besser kennen

## Einführung

Bisher konnte bei Ihnen der Eindruck erweckt werden, dass das Erstellen einer VM mit *Heat* und das manuelle
Erstellen mit der Kommandozeile gleich viel Zeit in Anspruch nimmt. Das stimmt auch beim einmaligen Erstellen der VM.
Der Vorteil von *Heat* besteht jedoch darin, dass das Template für weitere VMs wiederverwendet und weiterentwickelt werden kann.

Wie das geht zeigen wir Ihnen in diesem Schritt. Dazu fügen wir zusätzliche Parameter in das Template ein.

## Parameter

Zunächst ist es sinnvoll, bekannte oder individuelle  Parameter zu definieren.

In unserem Beispiel ersetzen wir den vorgegebenen SSH-Key und definieren ihn anstelle eines festen Werts als individuellen Parameter, der beim Start angegeben werden kann. Das hat den Vorteil, dass für eine VM andere Schlüssel verwendet werden können, ohne dass die Vorlage geändert werden muss.

Wie Sie bereits wissen, beginnt das Template mit der Version und wird mit `parameters` fortgeführt.

Danach wird ein individueller Name vergeben und der Typ angegeben. In unserem Fall verwenden wir als Typ `string`:

```yaml
heat_template_version: 2014-10-16
 
parameters:
  key_name:
    type: string
```

Nachdem wir den Parameter festgelegt haben, nutzen wir das vorige Template als Vorlage und ergänzen es.

Das Template sieht dann so aus:

```yaml
heat_template_version: 2014-10-16

parameters:
  key_name:
    type: string


resources:
  Instanz:
    type: OS::Nova::Server
    properties:
      key_name: BeispielKey
      image: Ubuntu 16.04 Xenial Xerus - Latest
      flavor: m1.small
```

Um das Template zu vervollständigen, ersetzen wir `key_name` durch den vorher definierten Parameter.

Der Befehl dafür lautet `get_param`.

Dieser Befehl sagt aus, dass ein definierter Parameter genutzt werden soll. Damit das Template weiß, welchen Parameter es nutzen soll, ergänzen wir
den Befehl `get_param` um `key_name`:

```yaml
heat_template_version: 2014-10-16

parameters:
  key_name:
    type: string

resources:
  Instanz:
    type: OS::Nova::Server
    properties:
      key_name: { get_param: key_name }
      image: Ubuntu 16.04 Xenial Xerus - Latest
      flavor: m1.small
```

## Zusammenfassung

Wir haben das Template mit einem frei definierbaren Parameter erweitert. Im nächsten Schritt fügen wir das Netzwerk hinzu.
