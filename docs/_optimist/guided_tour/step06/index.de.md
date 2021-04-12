---
title: "06: Einen eigenen SSH-Key per Konsole erstellen und nutzen"
lang: de
permalink: /optimist/guided_tour/step06
nav_order: 1060
parent: Guided Tour
---

Schritt 6: Einen eigenen SSH-Key per Konsole erstellen und nutzen
=================================================================

Vorwort
-------

Um später Zugriff auf den ersten deployten Stack per SSH zu erhalten, ist es
notwendig, ein Key Pair zu erzeugen und dieses im Gegensatz zu [Schritt
2](schritt02.md) auch zu nutzen.

Sollte bereits ein Keypair vorhanden sein, ist es nicht notwendig einen neuen
zu erstellen. Eine Ausnahme besteht hier für ED25519 SSH Keys, diese können
aktuell nicht genutzt werden.

Installation
------------

Wie in Schritt 2 erwähnt, gibt es mehrere Optionen um einen Key zu
erstellen.

Da wir bereits per Horizon(Dashboard) einen Key erzeugt haben, wird in
diesem Schritt er direkt über einen Befehl in der Kommandozeile
erstellt.

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

Mit dem oben genutzten Befehl (`ssh-keygen -t rsa -f Beispiel.key`)
werden zwei Dateien erzeugt, also das vorher genannte Keypair.

Zum einen die `Beispiel.key` Datei und die `Beispiel.key.pub`, dabei ist
`Beispiel.key` der private Teil, der nur uns bekannt sein soll und
`Beispiel.key.pub` wird als öffentlicher Teil genutzt.

Einsatzort
----------

Um den gerade erstellten Key zu nutzen, muss dieser eingebunden und für
später erstellte Instanzen/Stacks bereit gestellt werden.

Dies geht direkt mit dem vorher installierten OpenStackClient.

In der Dokumentation gehen wir davon aus, dass der erzeugte Key in
`~/.ssh/ liegt`, sollte sich dieser an einer anderen Stelle befinden,
muss das Keypair dorthin kopiert werden oder der Befehl entsprechend
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

Da im weiteren Verlauf der SSH-Key genutzt wird, sollte der Name, der
statt `Beispiel` vergeben wird, leicht merkbar sein.

Um zu überprüfen, ob der Key korrekt abgelegt wurde oder um sich den
Namen erneut anzeigen zu lassen, nutzt man folgenden
Befehl:

```bash
$ openstack keypair list
+----------+-------------------------------------------------+
| Name     | Fingerprint                                     |
+----------+-------------------------------------------------+
| Beispiel | ec:a6:75:f9:33:4b:e0:ba:e7:bb:b6:8a:a1:5d:48:ff |
+----------+-------------------------------------------------+
```

Abschluss
---------

Da der SSH Key jetzt genutzt werden kann, wird es Zeit weiter vorzugehen und
eine eigene Instanz zu erstellen.

Wie das genau funktioniert, erklären wir in [Schritt 7: Die erste eigene Instanz](schritt07.md).
