---
title: "11: Zugriff aus dem Internet vorbereiten; Wir ergänzen IPv6"
lang: "de"
permalink: /optimist/guided_tour/step11/
nav_order: 1110
parent: Guided Tour
---

# Schritt 11: Zugriff aus dem Internet vorbereiten: Wir ergänzen IPv6

## Einführung

In [Schritt 10](/optimist/guided_tour/step10/) haben wir bereits ein Netzwerk angelegt und in diesem Schritt erweitern wir dieses um IPv6.

Dabei nutzen wir unter anderem die bereits bestehenden Router. Wichtig ist, dass
die IPv4-Adresse auf dem ersten Interface läuft.

Die Cloud Images sind so konzipiert, dass das primäre Interface mit
[DHCP](https://de.wikipedia.org/wiki/Dynamic_Host_Configuration_Protocol)
vorkonfiguriert ist. Erst wenn das erfolgt ist, wird auf den Metadata
Service zugegriffen, um IPv6 hochzufahren.

## Subnet

Für die IPv6 Netze gibt es bereits einen Pool, aus dem Sie sich einfach
ein eigenes Subnetz generieren lassen können.

Welche Pools es gibt, können Sie sich mit diesem Befehl anzeigen lassen:

```bash
$ openstack subnet pool list
+--------------------------------------+---------------+---------------------+
| ID                                   | Name          | Prefixes            |
+--------------------------------------+---------------+---------------------+
| f541f3b6-af22-435a-9cbb-b233d12e74f4 | customer-ipv6 | 2a00:c320:1000::/48 |
+--------------------------------------+---------------+---------------------+
```

Aus diesem Pool können Sie sich nun eigene Subnetze generieren lassen. Die Prefixlänge von 64 Bit ist dabei pro generiertem Subnet fest
vorgegeben.

Bei der Erstellung der Pools können Sie die Subnets direkt mit angeben.
oder Sie überlassen es OpenStack.

Nutzen Sie dazu im folgenden Befehl den Zusatz `--use-default-subnet-pool`:

```bash
$ openstack subnet create --network BeispielNetzwerk --ip-version 6 --use-default-subnet-pool --ipv6-address-mode dhcpv6-stateful --ipv6-ra-mode dhcpv6-stateful BeispielSubnetIPv6
+-------------------------+----------------------------------------------------------+
| Field                   | Value                                                    |
+-------------------------+----------------------------------------------------------+
| allocation_pools        | 2a00:c320:1000:2::2-2a00:c320:1000:2:ffff:ffff:ffff:ffff |
| cidr                    | 2a00:c320:1000:2::/64                                    |
| created_at              | 2017-12-08T12:41:42Z                                     |
| description             |                                                          |
| dns_nameservers         |                                                          |
| enable_dhcp             | True                                                     |
| gateway_ip              | 2a00:c320:1000:2::1                                      |
| host_routes             |                                                          |
| id                      | 0046c29b-a9b0-47c3-b5dd-704aa801704d                     |
| ip_version              | 6                                                        |
| ipv6_address_mode       | dhcpv6-stateful                                          |
| ipv6_ra_mode            | dhcpv6-stateful                                          |
| name                    | BeispielSubnetIPv6                                       |
| network_id              | ff6d8654-66d6-4881-9528-2686bddcb6dc                     |
| project_id              | b15cde70d85749689e08106f973bb002                         |
| revision_number         | 0                                                        |
| segment_id              | None                                                     |
| service_types           |                                                          |
| subnetpool_id           | f541f3b6-af22-435a-9cbb-b233d12e74f4                     |
| updated_at              | 2017-12-08T12:41:42Z                                     |
| use_default_subnet_pool | True                                                     |
+-------------------------+----------------------------------------------------------+
```

## Router

Da nun auch das IPv6 Netz erstellt ist, werden wir in diesem Schritt das
neue Netz mit dem in Schritt 10 erstellten Router verbinden.

Nutzen Sie dazu den folgenden Befehl:

```bash
openstack router add subnet BeispielRouter BeispielSubnetIPv6
```

## Security Group

Die Regeln, die wir zuvor in Schritt 9 angelegt haben, beziehen
sich nur auf IPv4.

Damit auch IPv6 genutzt werden kann, legen wir noch zwei weitere Regeln in den schon bestehenden SecurityGroups an.

Um auch über IPv6 Zugriff per SSH auf die VM zu erlangen, nutzen wir den Befehl:

```bash
$ openstack security group rule create --remote-ip "::/0" --protocol tcp --dst-port 22:22 --ethertype IPv6 --ingress allow-ssh-from-anywhere
+-------------------+--------------------------------------+
| Field             | Value                                |
+-------------------+--------------------------------------+
| created_at        | 2017-12-08T12:44:04Z                 |
| description       |                                      |
| direction         | ingress                              |
| ether_type        | IPv6                                 |
| id                | 7d871e85-05fa-4620-b558-c6fc64076cde |
| name              | None                                 |
| port_range_max    | 22                                   |
| port_range_min    | 22                                   |
| project_id        | b15cde70d85749689e08106f973bb002     |
| protocol          | tcp                                  |
| remote_group_id   | None                                 |
| remote_ip_prefix  | ::/0                                 |
| revision_number   | 0                                    |
| security_group_id | 1cab4a62-0fda-40d9-bac8-fd73275b472d |
| updated_at        | 2017-12-08T12:44:04Z                 |
+-------------------+--------------------------------------+
```

Nun fehlt noch der Zugriff per ICMP, damit wir die VM auch über IPv6 mit Ping erreichen können.

Dies geht mit diesem Befehl:

```bash
$ openstack security group rule create --remote-ip "::/0" --protocol ipv6-icmp --ingress allow-ssh-from-anywhere
+-------------------+--------------------------------------+
| Field             | Value                                |
+-------------------+--------------------------------------+
| created_at        | 2017-12-08T12:44:44Z                 |
| description       |                                      |
| direction         | ingress                              |
| ether_type        | IPv6                                 |
| id                | f63e4787-9965-4732-b9d2-20ce0fedc974 |
| name              | None                                 |
| port_range_max    | None                                 |
| port_range_min    | None                                 |
| project_id        | b15cde70d85749689e08106f973bb002     |
| protocol          | ipv6-icmp                            |
| remote_group_id   | None                                 |
| remote_ip_prefix  | ::/0                                 |
| revision_number   | 0                                    |
| security_group_id | 1cab4a62-0fda-40d9-bac8-fd73275b472d |
| updated_at        | 2017-12-08T12:44:44Z                 |
+-------------------+--------------------------------------+
```

## Anpassungen am Betriebssystem

Startet man nun eine VM innerhalb des angelegten Netzes, bekommt diese sowohl eine IPv4 als auch eine IPv6 Adresse.

Die Standard-Images der Hersteller sind aber leider noch nicht für IPv6 vorkonfiguriert, weshalb tatsächlich nur die IPv4 Adresse in der VM ankommt.

Nutzt man die bereitgestellten Heat Templates, sind die notwendigen
Anpassungen bereits im Template enthalten.

Um dies auch bei bestehenden Instanzen nachträglich zu ermöglichen,
gibt es hier für verschiedene Linux-Distributionen eine Anleitung.

### Ubuntu 16.04

Um IPv6 korrekt nutzen zu können, müssen folgende Dateien mit dem
angegeben Inhalt erstellt werden.

- `/etc/dhcp/dhclient6.conf`

    ```
    timeout 30;
    ```

- `/etc/cloud/cloud.cfg.d/99-disable-network-config.cfg`

    ```
    network: {config: disabled}
    ```

- `/etc/network/interfaces.d/lo.cfg`

    ```
    auto lo
    iface lo inet loopback
    ```

- `/etc/network/interfaces.d/ens3.cfg`

    ```
    iface ens3 inet6 auto
        up sleep 5
        up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.ens3.leases -v ens3 || true
    ```

Im Anschluss wird das entsprechende Interface neu gestartet:

```bash
sudo ifdown ens3 && sudo ifup ens3
```

Die VM hat jetzt eine weitere IPv6 Adresse auf dem Interface, auf dem
vorher nur die IPv4 Adresse existierte und kann somit auch über IPv6
korrekt erreicht werden.

Damit Sie die beschriebenen Punkte nicht jedes mal manuell abarbeiten
müssen, können Sie die folgende `cloud-init` Konfiguration verwenden (was
`cloud-init` genau ist, erklären wir in [Schritt 19](/optimist/guided_tour/step19)):

```yaml
#cloud-config
write_files:
        - path: /etc/dhcp/dhclient6.conf
          content: "timeout 30;"
        - path: /etc/cloud/cloud.cfg.d/99-disable-network-config.cfg
          content: "network: {config: disabled}"
        - path: /etc/network/interfaces.d/lo.cfg
          content: |
              auto lo
              iface lo inet loopback
        - path: /etc/network/interfaces.d/ens3.cfg
          content: |
              iface ens3 inet6 auto
                  up sleep 5
                  up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.ens3.leases -v ens3 || true

runcmd:
        - [ ifdown, ens3]
        - [ ifup, ens3]
```

### CentOS 7

Die genannten Parameter müssen den angegebenen Dateien neu hinzugefügt
oder falls diese bereits vorhanden sind, ergänzt werden:

- `/etc/sysconfig/network`

    ```
    NETWORKING_IPV6=yes
    ```

- `/etc/sysconfig/network-scripts/ifcfg-eth0`

    ```
    IPV6INIT=yes
    DHCPV6C=yes
    ```

Anschließend wird das entsprechende Interface neu gestartet:

```bash
sudo ifdown eth0 && sudo ifup eth0
```

Die VM hat jetzt eine weitere IPv6 Adresse auf dem Interface, auf dem vorher nur die IPv4 Adresse existierte und kann somit auch über IPv6 korrekt erreicht werden.

Damit Sie die beschriebenen Punkte nicht jedes mal manuell abarbeiten
müssen, können Sie die folgende `cloud-init` Konfiguration verwenden (was
`cloud-init` genau ist, erklären wir für Ubuntu 16.04 in [Schritt 19](/optimist/guided_tour/step19/)):

```yaml
#cloud-config
write_files:
        - path: /etc/sysconfig/network
          owner: root:root
          permissions: '0644'
          content: |
              NETWORKING=yes
              NOZEROCONF=yes
              NETWORKING_IPV6=yes
        - path: /etc/sysconfig/network-scripts/ifcfg-eth0
          owner: root:root
          permissions: '0644'
          content: |
              DEVICE="eth0"
              BOOTPROTO="dhcp"
              ONBOOT="yes"
              TYPE="Ethernet"
              USERCTL="yes"
              PEERDNS="yes"
              PERSISTENT_DHCLIENT="1"
              IPV6INIT=yes
              DHCPV6C=yes
runcmd:
        - [ ifdown, eth0]
        - [ ifup, eth0]
```

## Externer Zugriff

**Wichtig**: Diese VM ist ab sofort weltweit über ihre
IPv6 Adresse zu erreichen. Das funktioniert natürlich nur auf den Ports, die wir auch in
den Security Groups aktiviert haben.

Wir benötigen also keine weitere Floating IP um externen Zugriff auf
diese VM zu ermöglichen.

Das ist wichtig zu erwähnen, da wir hier ein anderes Verhalten
haben, als mit den IPv4 Adressen.

Möchte man über IPv4 aus dem Internet auf diese VM zugreifen, muss man
weiterhin auf die Floating IPs zurückgreifen.

Hat man selbst lokal kein IPv6, möchte aber testen ob die VM
prinzipiell erreichbar ist, kann man auf Online Tools zurückgreifen, zum Beispiel <https://www.subnetonline.com/pages/ipv6-network-tools/online-ipv6-ping.php>.

## Zusammenfassung

Nachdem im letzten Schritt bereits eine Verbindung per IPv4 erfolgte,
wurde nun auch noch der Zugriff per IPv6 hinzugefügt.

Im nächsten Schritt nutzen wir die Instanz aus [Schritt 7](/optimist/guided_tour/step07/) als Vorlage und machen sie von außen zugänglich.
