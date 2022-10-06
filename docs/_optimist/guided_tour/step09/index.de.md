---
title: "09: Die erste Security-Group"
lang: "de"
permalink: /optimist/guided_tour/step09/
nav_order: 1090
parent: Guided Tour
---

# Schritt 9: Die erste Security-Group

## Einführung

Standardmäßig ist jeglicher Zugriff auf eine Instanz von außerhalb
verboten. Um Zugriff auf eine Instanz zu erlauben, muss mindestens
eine Security Group definiert und der Instanz zugewiesen werden.

Es ist möglich, alle Zugriffsregeln in einer Security Group
zusammenzufassen, doch für komplexe Stacks ist es sinnvoll, die Regeln
nach Aufgabe einzelner Instanzen in eigenen Security Groups zu
hinterlegen.

## Vorgehensweise

Der Grundbefehl für das Erstellen einer Security Group lautet:

`openstack security group create allow-ssh-from-anywhere`

Beispiel:

```bash
$ openstack security group create allow-ssh-from-anywhere --description Beispiel
+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+
| Field           | Value                                                                                                                                               |
+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+
| created_at      | 2017-12-08T12:01:42Z                                                                                                                                |
| description     | Beispiel                                                                                                                                            |
| id              | 1cab4a62-0fda-40d9-bac8-fd73275b472d                                                                                                                |
| name            | allow-ssh-from-anywhere                                                                                                                             |
| project_id      | b15cde70d85749689e08106f973bb002                                                                                                                    |
| revision_number | 2                                                                                                                                                   |
| rules           | created_at='2017-12-08T12:01:42Z', direction='egress', ethertype='IPv6', id='5a852e4b-1d79-4fe9-b359-64ca54c98501',                                 |
|                 | updated_at='2017-12-08T12:01:42Z'                                                                                                                   |
|                 | created_at='2017-12-08T12:01:42Z', direction='egress', ethertype='IPv4', id='fa90a1ee-d3b9-40d4-9bb5-89fdd5005c02',                                 |
|                 | updated_at='2017-12-08T12:01:42Z'                                                                                                                   |
| updated_at      | 2017-12-08T12:01:42Z                                                                                                                                |
+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+
```

Damit eine Security Group nicht nur eine leere Hülle ist, können Sie den
Befehl durch weitere Zusätze sinnvoll erweitern.

Hier ist eine kurze Übersicht der wichtigsten Optionen:

- `--protocol` = Definition des genutzten Protokolls (mögliche
    Optionen: icmp, tcp, udp)
- `--dst-port` = Gibt den Port oder die Range der Ports an. (22:22 ist
    Port 22, 1:[65535 würde alle Ports
    definieren)]{style="color: rgb(34,34,34);"}
- `--remote-ip` = Kann eine IP oder IP-Range definieren. (Default um
    den Zugang über alle IPs zu gewähren ist 0.0.0.0/0
- `--ingress` bzw. `--egress` = ingress definiert den eingehenden
    Verkehr, egress den ausgehenden

Da die wichtigsten Optionen nun bekannt sind, können Sie jetzt eine Security Group erstellen, mit der Sie theoretisch von überall Zugriff mit SSH erhalten.

Der Befehl lautet:
`openstack security group rule create allow-ssh-from-anywhere --protocol tcp --dst-port 22:22 --remote-ip 0.0.0.0/0`

```bash
$ openstack security group rule create allow-ssh-from-anywhere --protocol tcp --dst-port 22:22 --remote-ip 0.0.0.0/0
+-------------------+--------------------------------------+
| Field             | Value                                |
+-------------------+--------------------------------------+
| created_at        | 2017-12-08T12:02:15Z                 |
| description       |                                      |
| direction         | ingress                              |
| ether_type        | IPv4                                 |
| id                | 694a0573-b4c3-423c-847d-550f79e83f2b |
| name              | None                                 |
| port_range_max    | 22                                   |
| port_range_min    | 22                                   |
| project_id        | b15cde70d85749689e08106f973bb002     |
| protocol          | tcp                                  |
| remote_group_id   | None                                 |
| remote_ip_prefix  | 0.0.0.0/0                            |
| revision_number   | 0                                    |
| security_group_id | 1cab4a62-0fda-40d9-bac8-fd73275b472d |
| updated_at        | 2017-12-08T12:02:15Z                 |
+-------------------+--------------------------------------+
```

Um zu prüfen, ob die Security Group korrekt angelegt wurde und um eine
Übersicht über alle Gruppen zu erhalten, können Sie folgenden Befehl verwenden:

 `openstack security group show allow-ssh-from-anywhere`

```bash
$ openstack security group show allow-ssh-from-anywhere
+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+
| Field           | Value                                                                                                                                               |
+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+
| created_at      | 2017-12-08T12:01:42Z                                                                                                                                |
| description     | Beispiel                                                                                                                                            |
| id              | 1cab4a62-0fda-40d9-bac8-fd73275b472d                                                                                                                |
| name            | allow-ssh-from-anywhere                                                                                                                             |
| project_id      | b15cde70d85749689e08106f973bb002                                                                                                                    |
| revision_number | 3                                                                                                                                                   |
| rules           | created_at='2017-12-08T12:01:42Z', direction='egress', ethertype='IPv6', id='5a852e4b-1d79-4fe9-b359-64ca54c98501',                                 |
|                 | updated_at='2017-12-08T12:01:42Z'                                                                                                                   |
|                 | created_at='2017-12-08T12:02:15Z', direction='ingress', ethertype='IPv4', id='694a0573-b4c3-423c-847d-550f79e83f2b', port_range_max='22',           |
|                 | port_range_min='22', protocol='tcp', remote_ip_prefix='0.0.0.0/0', updated_at='2017-12-08T12:02:15Z'                                                |
|                 | created_at='2017-12-08T12:01:42Z', direction='egress', ethertype='IPv4', id='fa90a1ee-d3b9-40d4-9bb5-89fdd5005c02',                                 |
|                 | updated_at='2017-12-08T12:01:42Z'                                                                                                                   |
| updated_at      | 2017-12-08T12:02:15Z                                                                                                                                |
+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+
```

## Zusammenfassung

Sie haben erfolgreich eine Security-Group erstellt. Im nächsten Schritt lernen Sie, wie man ein Netzwerk hinzufügt.
