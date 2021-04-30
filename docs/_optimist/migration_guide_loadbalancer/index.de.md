---
title: "Migration: Loadbalancer"
lang: de
permalink: /optimist/migration_loadbalancer/
nav_order: 7000
last_modified_date: 2021-04-30
---

# Migration: Neutron LBaas (Deprecated) zu Octavia Loadbalancer

Mit dem Release von OpenStack Train (erwartet in Q4/2021) entfällt die Unterstützung für die veralteten Neutron LBaas-Loadbalancer. Als Vorbereitung zu diesem Update betrachten wir auch bei uns Neutron Loadbalancer als "Deprecated". Zukünftig unterstützen wir ausschließlich die Loadbalancer der Octavia-Komponente. Daher ist es wichtig, bestehende Neutron Balancer zu Octavia Balancern zu migrieren, bevor die Unterstützung weg fällt.

Um den Prozess möglichst einfach zu gestalten, haben wir hier einen kurzen Guide zusammen gestellt, um Sie bei der Migration zu unterstützen.

Die folgenden Schritte sind nötig:
- Neutron Loadbalancer auflisten
- Details der Loadbalancer ausgeben
- Einen neuen Octavia-Balancer mit den gleichen Einstellungen anlegen
- Abhängen der Floating IP des LBaas
- Anhängen der Floating IP an den neuen Loadbalancer

Sobald alle Schritte durchgeführt wurden, ist die Migration abgeschlossen. Nachfolgend beschreiben wir die Prozedur im Detail.

Alle Schritte finden auf der Kommandozeile statt.

## Wichtige Hinweise

Die Migration ist nur für Loadbalancer nötig, die **nicht** durch IMKE angelegt wurden.

Außerdem ist für den Zeitraum zwischen Abhängen und Wieder-Anhängen der Floating IP Ihr dahinter betriebener Service **offline**. Um das zu vermeiden, können Sie auch alternativ dem neuen Load-Balancer eine neue Floating IP zuweisen und Ihre verwendeten Nameserver entsprechend umkonfigurieren. Dabei sind dann für einen gewissen Zeitraum beide Loadbalancer aktiv.


## Neutron Loadbalancer auflisten

Für die ersten Schritte ist der Neutron-Client notwendig. Diesen installieren Sie mit `pip install python-neutronclient`.

Mit folgendem Befehl listen Sie alle Neutron Balancer in Ihrem Projekt auf:

```bash
$ neutron lbaas-loadbalancer-list
neutron CLI is deprecated and will be removed in the future. Use openstack CLI instead.
+--------------------------------------+--------------------+-------------+---------------------+----------+
| id                                   | name               | vip_address | provisioning_status | provider |
+--------------------------------------+--------------------+-------------+---------------------+----------+
| 2550d3ea-fa73-4923-a6df-1b0c828a8ff1 | Load Balancer Test | 10.0.0.19   | ACTIVE              | haproxy  |
| f731f07d-4612-4f94-81a3-ccb65a1ae1f0 | Apr27NeutronLB     | 192.168.5.7 | ACTIVE              | haproxy  |
+--------------------------------------+--------------------+-------------+---------------------+----------+
```

## Details der Loadbalancer ausgeben

Details zu den einzelnen Balancern:

```bash
$ neutron lbaas-loadbalancer-show Apr27NeutronLB
neutron CLI is deprecated and will be removed in the future. Use openstack CLI instead.
+---------------------+--------------------------------------+
| Field               | Value                                |
+---------------------+--------------------------------------+
| admin_state_up      | True                                 |
| description         |                                      |
| id                  | f731f07d-4612-4f94-81a3-ccb65a1ae1f0 |
| listeners           |                                      |
| name                | Apr27NeutronLB                       |
| operating_status    | ONLINE                               |
| pools               |                                      |
| provider            | haproxy                              |
| provisioning_status | ACTIVE                               |
| tenant_id           | c906ac5a32b34b54a0398d28ad448301     |
| vip_address         | 192.168.5.7                          |
| vip_port_id         | 2cc1b3f9-0457-410b-93dd-a96338de3b12 |
| vip_subnet_id       | c2bff369-0e62-44b3-89af-59ba6950094f |
+---------------------+--------------------------------------+
```

Alle Ressourcen innerhalb des Balancers finden Sie mit folgenden Befehlen:

```bash
$ neutron lbaas-healthmonitor-list
$ neutron lbaas-member-list
$ neutron lbaas-pool-list
$ neutron lbaas-listener-list
```

## Die Floating IP Ihres Balancers

In der vorherigen Ausgabe (`lbass-loadbalancer-show`) sehen Sie eine Zeile `vip_port_id`. Mit dieser ID finden Sie mit nachfolgendem Befehl Ihre Floating IP (und deren UUID). In unserem Beispiel wäre das für den Port (ID: `2cc1b3f9-0457-410b-93dd-a96338de3b12`) die IP `45.94.110.184` (mit ID `cd17e8b1-3d4b-417a-9a80-a21a9879a8dc`). Diese Information brauchen wir später.

```bash
$ openstack floating ip list
neutron CLI is deprecated and will be removed in the future. Use openstack CLI instead.
+--------------------------------------+------------------+---------------------+--------------------------------------+
| id                                   | fixed_ip_address | floating_ip_address | port_id                              |
+--------------------------------------+------------------+---------------------+--------------------------------------+
| 1856d701-a0c6-4d6c-a1c7-1f35072d3a34 |                  | 45.94.108.155       |                                      |
| 419208fc-e6e0-450c-a503-516b1337d44e | 192.168.2.6      | 185.116.245.202     | ab0df913-52fb-42e6-be59-3f9c2fa0102f |
| 90ad56e5-a3c8-4f9c-a12f-f534f11d7ed9 | 10.0.0.12        | 185.116.247.205     | 8a19cf54-e350-4f93-b572-db23dc650b30 |
| c6788cd1-c03f-496e-9962-678c0c8b9216 | 10.0.0.19        | 185.116.245.226     | 35f4fb69-e86d-4859-93f3-aa0c91fab123 |
| cd17e8b1-3d4b-417a-9a80-a21a9879a8dc | 192.168.5.7      | 45.94.110.184       | 2cc1b3f9-0457-410b-93dd-a96338de3b12 |
| e9b6e51a-0946-43f0-95fb-4679adc2638d | 10.0.0.13        | 185.116.245.13      | 9b8c1d2a-843f-44c5-a109-07f2a7e7be5d |
+--------------------------------------+------------------+---------------------+--------------------------------------+
```

## Einen neuen Octavia-Balancer mit den gleichen Einstellungen anlegen

Sobald der zu migrierende Neutron Loadbalancer identifiziert wurde, muss ein neuer Octavia-Balancer mit den gleichen Einstellungen angelegt werden (siehe auch [Schritt 24 in unserem Tutorial](/optimist/guided_tour/step24/)). 

Nachdem der neue Balancer eingerichtet wurde, notieren Sie sich bitte die ID des VIP ports, diese Information wird im letzten Schritt benötigt:

```bash
$ openstack loadbalancer show Apr27OctLB -f value -c vip_port_id
bde93be2-c0c1-4e23-af02-e0c4300ff8c0
```

## Abhängen der Floating IP des LBaaS

Jetzt kommt der finale Schritt: Das Abhängen der Floating IP vom Neutron Balancer und das Wieder-Anhängen an den neuen Octavia-Balancer. 

Zuerst detachen wir die IP vom Neutron Port. Die UUID der Floating IP (`45.94.110.184`) ist in unserem Beispiel `cd17e8b1-3d4b-417a-9a80-a21a9879a8dc`.

```bash
$ openstack floating ip unset --port 2cc1b3f9-0457-410b-93dd-a96338de3b12 cd17e8b1-3d4b-417a-9a80-a21a9879a8dc
neutron CLI is deprecated and will be removed in the future. Use openstack CLI instead.
Disassociated floating IP cd17e8b1-3d4b-417a-9a80-a21a9879a8dc
```

## Anhängen der Floating IP an den neuen Loadbalancer

Direkt darauffolgend attachen wir die gleiche Floating IP, die wir gerade vom Neutron LB Port abgehängt haben, an den Octavia Port:

```bash
LB VIP Port: bde93be2-c0c1-4e23-af02-e0c4300ff8c0
Floating IP: cd17e8b1-3d4b-417a-9a80-a21a9879a8dc

$ openstack floating ip set cd17e8b1-3d4b-417a-9a80-a21a9879a8dc --port bde93be2-c0c1-4e23-af02-e0c4300ff8c0
```

Wenn alles funktioniert hat, ist jetzt der neue Octavia-Balancer aktiv.