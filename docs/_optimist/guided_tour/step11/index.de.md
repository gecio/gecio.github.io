---
title: "11: Zugriff aus dem Internet vorbereiten; Wir ergänzen IPv6"
lang: de
permalink: /optimist/guided_tour/step11
nav_order: 1110
parent: Guided Tour
---

Schritt 11: Zugriff aus dem Internet vorbereiten: Wir ergänzen IPv6
===================================================================

Vorwort
-------

In Schritt 10 wurde von uns bereits ein Netzwerk angelegt und
in diesem Schritt erweitern wir selbiges um IPv6.

Dabei nutzen wir die bereits bestehenden Router etc. Wichtig ist, dass
die IPv4-Adresse auf dem ersten Interface läuft.

Die Cloud Images sind so konzipiert, dass das primäre Interface mit
[DHCP](https://de.wikipedia.org/wiki/Dynamic_Host_Configuration_Protocol)
vorkonfiguriert ist. Erst wenn das erfolgt ist, wird auf den Metadata
Service zugegriffen um IPv6 überhaupt hoch zu fahren.

Subnet
------

Für die IPv6 Netze gibt es bereits einen Pool, aus dem man sich einfach
ein eigenes Subnetz generieren lassen kann.

Welche Pools es gibt, findet man mit diesem Befehl heraus:

```bash
$ openstack subnet pool list
+--------------------------------------+---------------+---------------------+
| ID                                   | Name          | Prefixes            |
+--------------------------------------+---------------+---------------------+
| f541f3b6-af22-435a-9cbb-b233d12e74f4 | customer-ipv6 | 2a00:c320:1000::/48 |
+--------------------------------------+---------------+---------------------+
```

Aus diesem Pool kann man sich nun eigene Subnetze generieren lassen und
die Prefixlänge von 64Bit ist dabei pro generiertem Subnet fest
vorgegeben.

Bei der Erstellung der Pools kann man die Subnets direkt mit angeben
oder man überlässt es OpenStack.

Dafür wird im Befehl
`openstack subnet create --network BeispielNetzwerk --ip-version 6 --use-default-subnet-pool --ipv6-address-mode dhcpv6-stateful --ipv6-ra-mode dhcpv6-stateful BeispielSubnetIPv6`
einfach der Zusatz `--use-default-subnet-pool`
genutzt.

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

Router
------

Da nun das IPv6 Netz auch erstellt ist, werden wir in diesem Schritt das
neue Netz mit dem in Schritt 10 erstellten Router verbinden. 

Dafür nutzen wir den Befehl:

```bash
$ openstack router add subnet BeispielRouter BeispielSubnetIPv6
```

Security Group
--------------

Die Regeln die wir zuvor in Schritt 9 angelegt haben, beziehen
sich nur auf IPv4.

Damit auch IPv6 genutzt werden kann, legen wir noch zwei weitere Regeln
in den schon bestehenden SecurityGroups an.

Um auch per IPv6 Zugriff per SSH auf die VM zu erlangen nutzen wir den
Befehl:

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

Nun fehlt noch der Zugriff per ICMP, damit wir die VM auch über IPv6 per
Ping erreichen können.

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

Anpassungen am Betriebssystem
-----------------------------

Startet man nun eine VM innerhalb des angelegten Netzes, bekommt diese
eine IPv4, als auch eine IPv6 Adresse.

Die Standardimages der Hersteller sind aber leider noch nicht für IPv6
vorkonfiguriert, weshalb tatsächlich nur die IPv4 Adresse in der VM
ankommt.

Nutzt man unsere bereitgestellten Heat Templates, sind die notwendigen
Anpassungen bereits im Template enthalten. 

Um dies auch bei bestehenden Instanzen nachträglich auch zu ermöglichen,
gibt es hier für verschiedene Distributionen einen Anleitung.

### Ubuntu 16.04

Um IPv6 korrekt nutzen zu können, müssen folgende Dateien, mit dem
angegeben Inhalt erstellt werden.

-   `/etc/dhcp/dhclient6.conf`

    ```
    timeout 30;
    ```

-   `/etc/cloud/cloud.cfg.d/99-disable-network-config.cfg`

    ```
    network: {config: disabled}
    ```

-   `/etc/network/interfaces.d/lo.cfg`

    ```
    auto lo
    iface lo inet loopback
    ```

-   `/etc/network/interfaces.d/ens3.cfg`

    ```
    iface ens3 inet6 auto
        up sleep 5
        up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.ens3.leases -v ens3 || true
    ```

Im Anschluss wird das entsprechende Interface neugestartet:

```bash
$ sudo ifdown ens3 && sudo ifup ens3
```

Die VM hat jetzt eine weitere IPv6 Adresse auf dem Interface, auf dem
vorher nur die IPv4 Adresse existierte und kann somit auch per IPv6
korrekt erreicht werden.

Damit man die beschriebenen Punkte nicht jedes mal manuell abarbeiten
muss, kann man folgende `cloud-init` Konfiguration verwenden (Was
`cloud-init` genau ist, erklären wir in [Schritt 19: Unsere Instanz lernt
IPv6](schritt19.md)):

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
oder falls diese bereits vorhanden sind ergänzt werden:

-   `/etc/sysconfig/network`

    ```
    NETWORKING_IPV6=yes
    ```

-   `/etc/sysconfig/network-scripts/ifcfg-eth0`

    ```
    IPV6INIT=yes
    DHCPV6C=yes
    ```

Anschließend wird das entsprechende Interface neugestartet:

```bash
$ sudo ifdown eth0 && sudo ifup eth0
```

Die VM hat jetzt eine weitere IPv6 Adresse auf dem Interface, auf dem
vorher nur die IPv4 Adresse existierte und kann somit auch per IPv6
korrekt erreicht werden.

Damit man die beschriebenen Punkte nicht jedes mal manuell abarbeiten
muss, kann man folgende `cloud-init` Konfiguration verwenden(Was
`cloud-init` genau ist, erklären wir für Ubuntu 16.04 in [Schritt 19: Unsere Instanz lernt IPv6](/de/optimist/guided_tour/step19/):

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

Externer Zugriff
----------------

**Wichtig**: Diese VM ist ab sofort von überall auf der Welt über ihre
IPv6 Adresse zu erreichen. Natürlich nur auf den Ports, die wir auch in
den Security Groups aktiviert haben.

Wir benötigen also keine weitere Floating IP um externen Zugriff auf
diese VM zu ermöglichen.

Es ist deshalb so wichtig zu erwähnen, da wir hier ein anderes Verhalten
haben, als mit den IPv4 Adressen.

Möchte man per IPv4 aus dem Internet auf diese VM zugreifen, muss man
weiterhin auf die Floating IPs zurückgreifen.

Hat man selbst lokal kein IPv6, möchte aber testen ob seine VM
prinzipiell erreichbar ist, kann man auf Online Tools zurückgreifen, wie
z.B. <https://www.subnetonline.com/pages/ipv6-network-tools/online-ipv6-ping.php>

Abschluss
---------

Nachdem im letzten Schritt bereits eine Verbindung per IPv4 erfolgte,
wurde nun auch noch der Zugriff per IPv6 hinzugefügt. 

Im nächsten Schritt wird dann die Instanz aus [Schritt 7](de/optimist/guided_tour/step07/) als Vorlage genutzt und erreichbar von außen.
