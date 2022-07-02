---
title: Images
lang: de
permalink: /optimist/specs/images/
parent: Spezifikationen
nav_order: 9500
---

# Images

Es gibt vier Arten von Images in OpenStack:

- **Public Images:** Diese Images werden von uns gepflegt, sind für alle Benutzer verfügbar, werden regelmäßig aktualisiert und zur Verwendung empfohlen.
- **Community Images:** Ehemals öffentliche Images, die durch neuere Versionen ersetzt wurden. Wir behalten diese Images, bis sie nicht mehr verwendet werden, um die  Deployments der Benutzer nicht zu gefährden.
- **Private Images:** Von den Benutzern hochgeladene Images, die nur für ihre Projekte verfügbar sind.
- **Shared Images:** Private Images, die entweder durch die Benutzer oder mit ihnen in mehreren Projekten gemeinsam genutzt werden.

Nur die ersten beiden Typen werden von uns verwaltet.

## Public und community images

Um den Aufwand für Sie so gering wie möglich zu halten, stellen wir Ihnen eine Reihe ausgewählter Images zur Verfügung.

Aktuell enthält diese Liste:

- Ubuntu 22.04 LTS (Jammy Jellyfish)
- Ubuntu 20.04 LTS (Focal Fossa)
- Ubuntu 18.04 LTS (Bionic Beaver)
- Debian 11 (Bullseye)
- Debian 10 (Buster)
- CentOS 8
- CentOS 7
- Flatcar Linux
- Windows Server 2019 (GUI/Core)

Diese Images werden täglich auf neue Versionen überprüft. Die neueste verfügbare Version ist immer ein "public image" und endet auf `Latest`. Alle vorherigen Versionen eines Images werden durch unseren Automatismus in "community images" umgewandelt, umbenannt (`latest` wird durch das Datum des ersten Uploads ersetzt), und bei ausbleibender Verwendung (keinerlei Nutzung) schlussendlich gelöscht.

OpenStack und viele Deployment-Tools unterstützen die Verwendung dieser Images entweder über den Namen oder über ihre UUID. Durch die Verwendung eines Namens, z.B. `Ubuntu 22.04 Jammy Jellyfish - Latest`, erhalten Sie jeweils die aktuellste Version des jeweiligen Images, indem Sie ihre Instanzen neu bereitstellen oder neu aufbauen, selbst wenn wir das Image zwischendurch ersetzen. Sie können dieses Verhalten vermeiden, indem Sie stattdessen die UUID verwenden. Dies kann für Cluster-Einsätze nützlich sein, bei denen Sie sicherstellen wollen, dass auf allen Instanzen die gleiche Version des Images läuft.

## Linux Images

Alle von uns zur Verfügung gestellten Linux-Images sind unmodifiziert und kommen direkt von ihren offiziellen Maintainern. Wir testen sie während des Upload-Prozesses auf Kompatibilität.

## Windows Images

### Was ist darin enthalten?

Leider gibt es keine vorgefertigten Images für Windows-Deployments, deshalb haben wir eigene gebaut. Unsere Anpassungen sind minimal, gerade genug, um eine einfache Nutzung innerhalb unserer Instanzen zu ermöglichen.

Unsere Images basieren auf einer regulären Installation von Windows Server 2019 Standard Edition, Version 1809 (LTSC). Unsere Image Builds enthalten die aktuellsten Treiber für unsere Virtualisierungsinfrastruktur, für die Netzwerkkarte und Festplatten.

Außerdem haben wir die neueste OpenSSH-Version für Windows und die neueste Version der PowerShell installiert. Beide sind für die folgenden Schritte erforderlich und ermöglichen den Benutzern die erste Verbindung mit ihrer Instanz.

Des weiteren ist der RDP-Dienst aktiviert, der für eine Remote-Desktop-Verbindung erforderlich ist. Vergessen Sie nicht, die dafür erforderlichen Sicherheitsgruppen hinzuzufügen und achten Sie darauf, den Zugriff so weit wie möglich einzuschränken. Außerdem haben wir aus Sicherheitsgründen AutoLogon deaktiviert.

Unsere Images sind außerdem mit aktivierten Spectre- und Meltdown-Mitigations ausgestattet. Außerdem mussten wir die Nutzung von zufälligen MAC-Adressen deaktivieren, da unsere virtuellen Netzwerke feste MAC-Adressen voraussetzen.

Für einen schnellen Start und Ihre Sicherheit stellen wir diese Windows-Images mit den neuesten kumulativen Updates für Windows und das .NET Framework bereit. Nach dem Hochfahren einer Instanz müssen Sie wahrscheinlich nur die Windows-Defender-Definitionen aktualisieren.

Schließlich haben wir die verfügbaren DotNetAssemblies optimiert, Firewall-Regeln hinzugefügt, um ICMP-Echoantworten zuzulassen, und cloud-init installiert. Letzteres ist für das Hinzufügen Ihrer ssh-Schlüssel zu den neuen Instanzen verantwortlich.

### Wie funktioniert das?

Fast genauso einfach wie mit unseren Linux-Instanzen. Importieren Sie Ihren SSH-Schlüssel in OpenStack (CLI oder Dashboard) und starten Sie Ihre Instanzen. Danach können Sie sich mit folgendem Befehl anmelden:

```bash
ssh -i ~/.ssh/id_rsa $instanceIP -l Administrator
```

Einmal eingeloggt, können Sie nun ein neues Administrator-Passwort vergeben, mit dem Sie sich am Remote-Desktop einloggen können:

```bash
net user Administrator $password
```

Wir raten dringend davon ab, veraltete Verfahren wie z.B. ein `admin_pass` über die Instanz-Metadaten zu setzen. Hierbei wird nichts verschlüsselt oder anderweitig gesichert, und wird außerdem nicht funktionieren, sollte Ihr Passwort nicht den nötigen Sicherheitsrichtlinien entsprechen.

**Achtung:** Unsere Windows-Images enthalten weder Produkt-Schlüssel, noch Lizenzen. Sie werden daher Ihre eigenen verwenden müssen.

## Upload von eigenen Images

Sie können jederzeit Ihre eigenen Images hochladen, anstatt die von uns bereit gestellten zu nutzen. Am einfachsten funktioniert das über die OpenStack-CLI.

```bash
openstack image create \
  --property hw_disk_bus=scsi \
  --property hw_qemu_guest_agent=True \
  --property hw_scsi_model=virtio-scsi \
  --property os_require_quiesce=True \
  --private \
  --disk-format qcow2 \
  --container-format bare \
  --file ~/my-image.qcow2 \
  my-image
```

Dabei müssen mindestens Sie folgende Parameter spezifizieren:

- `--disk-format`: Das Format Ihres Quell-Images, z.B. `qcow2`
- `--file`: Das Quell-Image auf Ihrem System
- Name des Abbilds: `my-image` als Beispiel.

Das gleiche funktioniert auch über das Dashboard. Achten Sie hier darauf, alle der obigen Parameter anzugeben.
