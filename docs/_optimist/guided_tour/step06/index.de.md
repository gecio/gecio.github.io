---
title: "06: Einen eigenen SSH-Key mit der Kommando-Konsole erstellen und nutzen"
lang: "de"
permalink: /optimist/guided_tour/step06/
nav_order: 1060
parent: Guided Tour
---

# Schritt 6: Einen eigenen SSH-Key mit der Kommando-Konsole erstellen und nutzen

## Einführung

Um später Zugriff auf den ersten deployten Stack über SSH zu erhalten, muss ein Key Pair erzeugt und im Gegensatz zu [Schritt 2](/optimist/guided_tour/step02/) auch verwendet werden.

Falls bereits ein Keypair vorhanden ist, ist es nicht notwendig, einen neuen Key zu erstellen.

## Installation

Wie in Schritt 2 erwähnt, gibt es mehrere Möglichkeiten, einen Key zu
erstellen.

Sie haben bereits mit dem Horizon Dashboard einen Key erzeugt. In  diesem Schritt lernen Sie, den Key mit dem folgenden Befehl in der Kommandozeile zu erstellen.

```bash
$ ssh-keygen -t rsa -f Beispiel.key
Generating public/private rsa key pair.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in Beispiel.key.
Your public key has been saved in Beispiel.key.pub.
The key fingerprint is:
SHA256:UKSodmr6MFCO1fSqNYAoyM7uX8n/O5a43cPEV5vJXW8
The key's randomart image is:
+---[RSA 2048]----+
|    .  .o        |
|+. o o o         |
|=.+ o +          |
|+= o . .       ..|
|oo+ =   S .   o B|
|o. =...    o . =E|
|o.+  +  . + .  . |
|.=  . ...+.o     |
|.oo.   o++o..    |
+----[SHA256]-----+
```

Mit dem oben genannten Befehl (`ssh-keygen -t rsa -f Beispiel.key`)
werden zwei Dateien (das Keypair) erzeugt, die `Beispiel.key` Datei und die `Beispiel.key.pub` Datei. Dabei ist
`Beispiel.key` der private Teil, der nur Ihnen bekannt und an einem sicheren Ort gespeichert sein soll und
`Beispiel.key.pub`, der als öffentlicher Teil genutzt wird.

## Einsatzort

Um den gerade erstellten Key zu nutzen, muss dieser in die OpenStack Umgebung eingebunden und für
später erstellte Instanzen/Stacks bereit gestellt werden.

Dies geht direkt mit dem vorher installierten OpenStack Client.

In dieser Dokumentation gehen wir davon aus, dass der erzeugte Key in
`~/.ssh/` liegt. Falls er sich an einer anderen Stelle befindet,
muss das Keypair dorthin kopiert oder der Befehl entsprechend
angepasst werden:

```bash
$ openstack keypair create --public-key ~/.ssh/Beispiel.key.pub Beispiel
+-------------+-------------------------------------------------+
| Field       | Value                                           |
+-------------+-------------------------------------------------+
| fingerprint | ec:a6:75:f9:33:4b:e0:ba:e7:bb:b6:8a:a1:5d:48:ff |
| name        | Beispiel                                        |
| user_id     | 9bf501f4c3d14b7eb0f1443efe80f656                |
+-------------+-------------------------------------------------+
```

Da im weiteren Verlauf der SSH-Key genutzt wird, sollten Sie einen Namen
anstelle von `Beispiel` vergeben, den Sie sich leicht merken können.

Um zu überprüfen, ob der Key korrekt abgelegt wurde oder um sich den
Namen erneut anzeigen zu lassen, können Sie folgenden
Befehl verwenden:

```bash
$ openstack keypair list
+----------+-------------------------------------------------+
| Name     | Fingerprint                                     |
+----------+-------------------------------------------------+
| Beispiel | ec:a6:75:f9:33:4b:e0:ba:e7:bb:b6:8a:a1:5d:48:ff |
+----------+-------------------------------------------------+
```

## Zusammenfassung

Sie haben ein neues SSH Keypair erzeugt und den öffentlichen Key hochgeladen. Sie können den SSH Key nun nutzen und eine eigene Instanz erstellen.

Wie das genau funktioniert, erfahren Sie in [Schritt 7](/optimist/guided_tour/step07/).
