---
title: "04: Der Weg vom Horizon Dashboard zur Kommandozeile"
lang: "de"
permalink: /optimist/guided_tour/step04/
nav_order: 1040
parent: Guided Tour
---

# Schritt 4: Der Weg vom Horizon Dashboard zur Kommandozeile

## Einführung

Auf den ersten Blick mag es komfortabel erscheinen, die OpenStack
Umgebung mit dem Horizon Dashboard zu verwalten.

Für einfache, nicht wiederkehrende Aufgaben, ist das Horizon Dashboard mit seinen grafischen Ansichten sehr hilfreich.

Für sich regelmäßig wiederholende Aufgaben oder für die Verwaltung eines komplexeren Stacks, ist es jedoch sinnvoller, den OpenStack Client und auch Heat (welches in einem späteren Schritt erklärt wird) zu verwenden.

Anfangs mag die Handhabung mit OpenStack ungewohnt sein. Mit ein wenig Übung kann die Arbeit an den eigenen Stacks jedoch schnell und effizient erledigt werden.

Der OpenStack Client ist sehr hilfreich bei der Administration der
OpenStack Umgebung, da dort bereits Komponenten wie Nova, Glance, Heat,
Cinder, und Neutron enthalten sind.

Da wir auch im weiteren Verlauf des Tutorials den Client nutzen,
installieren wir ihn in diesem Kapitel.

## Installation

Um den OpenStackClient zu installieren, benötigen Sie mindestens [Python
2.7](https://www.python.org/downloads/release/python-2713/) und die [Python
Setuptools](https://pypi.python.org/pypi/setuptools) (diese
sind bei macOS bereits vorinstalliert).

Es gibt verschiedene Optionen, die Installation durchzuführen; `pip` hat
sich hierbei als eine gute Lösung erwiesen und wird als Grundlage
in dieser Dokumentation verwendet.

`pip` ist einfach zu bedienen und stellt sicher, dass die aktuellste
Version der Pakete genutzt wird. Sie können damit auch im Nachhinein Updates einspielen.

Sie können den Client als root/admin installieren, was aber zu weiteren Problem führen kann. Daher nutzen wir für den Client eine virtuelle
Umgebung.

### macOS

Damit der [OpenStack
Client](https://docs.openstack.org/python-openstackclient/latest/) installiert werden kann, installieren Sie zunächst `pip`.

Öffnen Sie dazu die Konsole (zum Beispiel über *Launchpad → Konsole*) und führen Sie den folgenden Befehl aus:

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

Sobald die Installation von `pip` abgeschlossen ist, legen Sie die
virtuelle Umgebung an:

```bash
$ pip install virtualenv
Collecting virtualenv
  Downloading virtualenv-15.1.0-py2.py3-none-any.whl (1.8MB)
    100% |????????????????????????????????| 1.8MB 619kB/s
Installing collected packages: virtualenv
Successfully installed virtualenv-15.1.0
```

Nachdem Sie nun mit `virtualenv` eine virtuelle Umgebung nutzen können,
erstellen Sie direkt eine solche Umgebung:

```bash
$ virtualenv ~/.virtualenvs/openstack
New python executable in /Users/GEC/.virtualenvs/openstack/bin/python
Installing setuptools, pip, wheel...done.
```

Wechseln Sie jetzt in die erzeugte virtuelle Umgebung:

```bash
$ source ~/.virtualenvs/openstack/bin/activate
(openstack) $
```

Anschließend können Sie in der Umgebung direkt den OpenStackClient installieren:

```bash
(openstack) $ pip install python-openstackclient
```

Da im Verlauf des Tutorials auch andere Services benutzt werden, installieren wir die entsprechenden Klienten gleich mit:

```bash
(openstack) $ pip install python-heatclient python-designateclient python-octaviaclient
```

Verlassen Sie nun die Umgebung wieder:

```bash
(openstack) $ deactivate
```

Damit Sie  den OpenStackClient nutzen können, müssen Sie diesen in die Path Variable aufnehmen:

```bash
export PATH="$HOME/.virtualenvs/openstack/bin/:$PATH"
```

Um zu sehen, ob alles korrekt funktioniert hat, testen Sie die Ausgabe:

```bash
$ type -a openstack
openstack is /home/GEC/.virtualenvs/openstack/bin/openstack
```

### Windows

Um `pip` nutzen zu können, müssen Sie zuerst in den Installationsordner von  Python wechseln (Standard-Ordner: `C:\Python27\Scripts`).

Um `pip` zu installieren, benutzen Sie den Befehl `easy_install pip`:

```text
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

Nach der erfolgreichen Installation von `pip`, können Sie mit `pip
install python-openstackclient` den OpenStackClient installieren:

```text
C:\Python27\Scripts>pip install python-openstackclient
Collecting python-openstackclient
  Downloading python_openstackclient-3.12.0-py2.py3-none-any.whl (772kB)
    100% |################################| 778kB 1.1MB/s
```

### Linux (in diesem Beispiel Ubuntu)

Zunächst wird auch hier `pip` benötigt. Wir nutzen dafür `apt-get`:

```bash
$ sudo apt-get install python3-pip
Reading package lists... Done
Building dependency tree
Reading state information... Done
```

Sobald die Installation von `pip` abgeschlossen ist, legen Sie die
Virtuelle Umgebung an:

```bash
$ sudo apt-get install python3-virtualenv
Reading package lists... Done
Building dependency tree
Reading state information... Done
```

Nachdem Sie nun mit `virtualenv` eine virtuelle Umgebung nutzen können,
erstellen Sie direkt eine solche Umgebung:

```bash
$ virtualenv ~/.virtualenvs/openstack
New python executable in /Users/GEC/.virtualenvs/openstack/bin/python
Installing setuptools, pip, wheel...done.
```

Wechseln Sie jetzt in die erzeugte virtuelle Umgebung:

```bash
$ source ~/.virtualenvs/openstack/bin/activate
(openstack) $
```

Anschließend können Sie in der Umgebung direkt den OpenStackClient installieren:

```bash
(openstack) $ pip install python-openstackclient
```

Da wir im Verlauf der Dokumentation auch Heat benutzen, installieren wir
den entsprechenden Heat Client gleich mit:

```bash
(openstack) $ pip install python-heatclient
```

Verlassen Sie die Umgebung wieder:

```bash
(openstack) $ deactivate
```

Damit Sie den OpenStackClient nutzen können, müssen Sie diesen in die Path Variable aufnehmen:

```bash
export PATH="$HOME/.virtualenvs/openstack/bin/:$PATH"
```

Um zu sehen ob alles korrekt funktioniert hat, testen Sie die Ausgabe:

```bash
$ type -a openstack
openstack is /home/GEC/.virtualenvs/openstack/bin/openstack
```

## Zugangsdaten

Nachdem der OpenStackClient installiert ist, benötigen Sie noch die
Zugangsdaten für Openstack.

Sie können diese direkt im [Horizon
Dashboard](https://dashboard.optimist.innovo.cloud/identity/) herunterladenn. Loggen Sie sich dazu ein und klicken Sie rechts oben auf die E-Mail-Adresse. Klicken Sie anschließend auf *Download OpenStack RC File v3*.
Die heruntergeladene Datei trägt den Projektnamen (Projektname.sh). In unserem Beispiel nennen wir sie `Beispiel.sh`.

### macOS | Linux

Um die Zugangsdaten in den OpenStackClienten einzulesen, führen Sie
den folgenden Befehl aus:

```bash
source Beispiel.sh
```

### Windows

Um in Windows die Zugangsdaten einzulesen, müssen Sie entweder
*PowerShell*, *Git for Windows* oder [*Linux on Windows*](https://docs.microsoft.com/en-us/windows/wsl/install-win10) nutzen.

Bei *Linux on Windows* und *Git for Windows* mithilfe von *Git Bash*, wird der gleiche Befehl wie im Beispiel für macOS | Linux genutzt:

```bash
source Beispiel.sh
```

Bei der Nutzung von *PowerShell* müssen die Variablen einzeln gesetzt werden.
Sie finden alle notwendigen Variablen in der Datei `Beispiel.sh`. Sie können die Datei mit einem Editor öffnen.
Um die Variablen zu setzen, können Sie den folgenden Befehl benutzen:

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

## Zusammenfassung

Die Installation des OpenStackClienten ist nun abgeschlossen und Sie können die ersten Befehle damit testen.

Sie erhalten eine Übersicht über sämtliche Befehle mit dem folgendem Kommando:

```bash
openstack --help
```
