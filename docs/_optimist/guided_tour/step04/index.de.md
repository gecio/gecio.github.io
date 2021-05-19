---
title: "04: Der Weg vom Horizon auf die Kommandozeile"
lang: de
permalink: /optimist/guided_tour/step04/
nav_order: 1040
parent: Guided Tour
---

Schritt 4: Der Weg vom Horizon auf die Kommandozeile
====================================================

Vorwort
-------

Auf den ersten Blick kann es komfortabel erscheinen, seine OpenStack
Umgebung mit dem Horizon Dashboard zu verwalten.

Für einfache, nicht wiederkehrende Aufgaben, kann das Horizon Dashboard
mit seinen grafische Ansichten wirklich hilfreich sein.

Sobald Aufgaben regelmäßig wiederholt werden oder ein komplexerer Stack
verwaltet werden soll, ist es sinnvoller, den OpenStack Client und auch
Heat(welches in den späteren Schritten mit erklärt wird) zu verwenden.

Anfangs mag die Handhabung ungewohnt sein, mit ein wenig Übung kann die
Arbeit an den eigenen Stacks schnell und effizient erledigt werden.

Der OpenStack Client ist sehr hilfreich bei der Administration der
OpenStack Umgebung, da dort bereits Komponenten wie Nova, Glance, Heat,
Cinder, Neutron enthalten sind.

Da wir auch im weiteren Verlauf der Dokumentation den Client nutzen,
installieren wir ihn in diesem Schritt.

Installation
------------

Um den OpenStackClient installieren zu können, wird mindestens [Python
2.7](https://www.python.org/downloads/release/python-2713/) noch die [Python
Setuptools](https://pypi.python.org/pypi/setuptools) (diese
sind bei macOS bereits vorinstalliert).

Es gibt verschiedene Optionen die Installation durchzuführen, `pip` hat
sich hierbei als eine gute Lösung herausgestellt und wird als Grundlage
in der Dokumentation verwendet.

Selbiges ist einfach zu bedienen, stellt sicher das die aktuellste
Version der Pakete genutzt wird und kann im Nachhinein Updates
einspielen.

Man kann den Client ohne weiteres in root/admin installieren, das kann
aber zu weiteren Problem führen, daher nutzen wir eine virtuelle
Umgebung für den Clienten.

### macOS

Damit nun der [OpenStack
Client](https://docs.openstack.org/python-openstackclient/latest/) installieren werden
kann, wird zunächst `pip` benötigt.

Um `pip` zu installieren, wird zunächst die Konsole geöffnet (diese kann
zum Beispiel über das Launchpad → Konsole geöffnet werden)  und dann
folgender Befehl ausgeführt:

```bash
$ easy_install pip
Searching for pip
Best match: pip 9.0.1
Adding pip 9.0.1 to easy-install.pth file
Installing pip script to /usr/local/bin
Installing pip2.7 script to /usr/local/bin
Installing pip2 script to /usr/local/bin

Using /usr/local/lib/python2.7/site-packages
Processing dependencies for pip
Finished processing dependencies for pip
```

Sobald die Installation von `pip` abgeschlossen ist, wird nun die
Virtuelle Umgebung angelegt:

```bash
$ pip install virtualenv
Collecting virtualenv
  Downloading virtualenv-15.1.0-py2.py3-none-any.whl (1.8MB)
    100% |????????????????????????????????| 1.8MB 619kB/s
Installing collected packages: virtualenv
Successfully installed virtualenv-15.1.0
```

Nachdem wir nun mit `virtualenv` eine virtuelle Umgebung nutzen können,
erstellen wir direkt eine:

```bash
$ virtualenv ~/.virtualenvs/openstack
New python executable in /Users/iNNOVO/.virtualenvs/openstack/bin/python
Installing setuptools, pip, wheel...done.
```

Jetzt wechseln wir in die erzeugte virtuelle Umgebung:

```bash
$ source ~/.virtualenvs/openstack/bin/activate
(openstack) $
```

Nachdem dieser Wechsel funktioniert hat, können wir in der geschaffenen
Umgebung auch direkt den OpenStackClient installieren:

```bash
(openstack) $ pip install python-openstackclient
```

Da wir im Verlauf der Dokumentation auch andere Services benutzen, installieren wir
die entsprechenden Clienten gleich mit:

```bash
(openstack) $ pip install python-heatclient python-designateclient python-octaviaclient
```

Nun verlassen wir die Umgebung auch direkt wieder:

```bash
(openstack) $ deactivate
```

Damit wir den OpenStackClient auch nutzen können, ist es nun notwendig
dies in die Path Variablen aufzunehmen.

```bash
$ export PATH="$HOME/.virtualenvs/openstack/bin/:$PATH"
```

Um zu sehen ob alles korrekt funktioniert hat, testen wir die Ausgabe:

```bash
$ type -a openstack
openstack is /home/iNNOVO/.virtualenvs/openstack/bin/openstack
```

### Windows

Um pip zu nutzen, ist zuerst der Wechsel in den Ordner der Python
Installation notwendig (Speicherort der Standart Installation:
`C:\Python27\Scripts`).

`pip` wird dann mit dem Befehl `easy_install pip` installiert:

```
C:\Python27\Scripts>easy_install pip
Searching for pip
Best match: pip 9.0.1
Adding pip 9.0.1 to easy-install.pth file
Installing pip-script.py script to c:\python27\scripts
Installing pip.exe script to c:\python27\scripts
Installing pip.exe.manifest cript to c:\python27\scripts
Installing pip3.5-script.py script to c:\python27\scripts
Installing pip3.5.exe script to c:\python27\scripts
Installing pip3.5.exe.manifest script to c:\python27\scripts
Installing pip3-script.py script to c:\python27\scripts
Installing pip3.exe script to c:\python27\scripts
Installing pip3.exe.manifest script to c:\python27\scripts


Using c:\python27\lib\site-packages
Processing dependencies for pip
Finished processing dependencies for pip
```

Nach der erfolgreichen Installation von `pip`, kann direkt mit pip
install `python-openstackclient` der OpenStackClient auch installiert
werden:

```
C:\Python27\Scripts>pip install python-openstackclient
Collecting python-openstackclient
  Downloading python_openstackclient-3.12.0-py2.py3-none-any.whl (772kB)
    100% |################################| 778kB 1.1MB/s
```

### Linux (in diesem Beispiel Ubuntu)

Zunächst wird auch hier `pip` benötigt, dafür wird `apt-get` genutzt:

```bash
$ sudo apt-get install python-pip
Reading package lists... Done
Building dependency tree
Reading state information... Done
```

Sobald die Installation von `pip` abgeschlossen ist, wird nun die
Virtuelle Umgebung angelegt:

```bash
$ sudo apt install python-virtualenv
Reading package lists... Done
Building dependency tree
Reading state information... Done
```

Nachdem wir nun mit `virtualenv` eine virtuelle Umgebung nutzen können,
erstellen wir direkt eine:

```bash
$ virtualenv ~/.virtualenvs/openstack
New python executable in /Users/iNNOVO/.virtualenvs/openstack/bin/python
Installing setuptools, pip, wheel...done.
```

Jetzt wechseln wir in die erzeugte virtuelle Umgebung:

```bash
$ source ~/.virtualenvs/openstack/bin/activate
(openstack) $
```

Nachdem dieser Wechsel funktioniert hat, können wir in der geschaffenen
Umgebung auch direkt den OpenStackClient installieren:

```bash
(openstack) $ pip install python-openstackclient
```

Da wir im Verlauf der Dokumentation auch Heat benutzen, installieren wir
den entsprechenden Heat Clienten gleich mit:

```bash
(openstack) $ pip install python-heatclient
```

Nun verlassen wir die Umgebung auch direkt wieder:

```bash
(openstack) $ deactivate
```

Damit wir den OpenStackClient auch nutzen können, ist es nun notwendig
dies in die Path Variablen aufzunehmen.

```bash
$ export PATH="$HOME/.virtualenvs/openstack/bin/:$PATH"
```

Um zu sehen ob alles korrekt funktioniert hat, testen wir die Ausgabe:

```bash
$ type -a openstack
openstack is /home/iNNOVO/.virtualenvs/openstack/bin/openstack
```

Zugangsdaten
------------

Nachdem der OpenStackClient nun installiert ist, werden noch die
Zugangsdaten für Openstack  benötigt. 

Diese können direkt im [Horizon
Dashboard](https://dashboard.optimist.innovo.cloud/identity/) heruntergeladen
werden.  Dafür loggen wir uns ein und klicken dann rechts oben in der Ecke auf
die E-Mail-Adresse und dann auf *Download OpenStack RC File v3*.
Die heruntergeladene Datei trägt den Projektnamen (Projektname.sh), 
in unserem Beispiel nennen wir sie Beispiel.sh

### macOS | Linux

Um die Zugangsdaten in den OpenStackClienten einzulesen, führen wir
nun folgenden Befehl aus:

```bash
$ source Beispiel.sh
```

### Windows

Um unter Windows die Zugangsdaten einzulesen, ist es notwendig entweder 
*PowerShell*, *Git for Windows* oder [*Linux on Windows*](https://docs.microsoft.com/en-us/windows/wsl/install-win10) zu nutzen. 

Bei *Linux on Windows* und *Git for Windows* via *Git Bash*, wird der gleiche Befehl wie im Beispiel für
macOS | Linux genutzt:

```
source Beispiel.sh
```


Bei der Nutzung von *PowerShell* müssen die Variablen einzeln gesetzt werden. 
Alle notwendigen Variablen befinden sich in der Datei Beispiel.sh und diese kann 
mit einem Editor geöffnet werden. 
Um die Variablen zu setzen, kann folgender Befehl genutzt werden:

```bash
set-item env:OS_AUTH_URL -value "https://identity.optimist.innovo.cloud/v3"
set-item env:OS_PROJECT_ID -value "Projekt ID eintragen"
set-item env:OS_PROJECT_NAME -value "Namen eintrage"
set-item env:OS_USER_DOMAIN_NAME -value "Default"
set-item env:OS_USERNAME -value "Usernamen eintragen"
set-item env:OS_PASSWORD -value "Passwort eingeben"
set-item env:OS_USER_DOMAIN_NAME -value "Default"
set-item env:OS_REGION_NAME -value "fra"
set-item env:OS_INTERFACE -value "public"
set-item env:OS_IDENTITY_API_VERSION -value "3"
```

Ziel
----

Die Installation des OpenStackClienten ist abgeschlossen und die ersten
Befehle können damit getestet werden. 

Eine Übersicht über alle Befehle, kann mit folgendem Kommando abgerufen
werden:

```bash
$ openstack --help
```
