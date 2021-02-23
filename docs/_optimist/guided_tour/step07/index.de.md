---
title: Schritt 7 - Die erste eigene Instanz
lang: de
permalink: /optimist/guided_tour/step07
nav_order: 2700
parent: Guided Tour
---

Schritt 7: Die erste eigene Instanz
===================================

Vorwort
-------

Wir wissen jetzt alles, was nötig ist, um die erste eigene Instanz
anzulegen und zu starten.

Es ist am sinnvollsten, das gleich in einem Stack zu organisieren und
diesen mit einem Template zu beschreiben, anstatt alle notwendigen
Arbeitsschritte von Hand durchzuführen.

Trotzdem erzeugen wir im allerersten Schritt erst mal eine Instanz von
Hand.

Installation
------------

Der Grundbefehl für das Erstellen einer Instanz in der Kommandozeile
lautet:

```bash
$ openstack server create test
```

Wenn der Befehl ohne weitere Zusätze ausgeführt wird, erscheint direkt
eine Fehlermeldung:

```bash
usage: openstack server create [-h] [-f {json,shell,table,value,yaml}]
                               [-c COLUMN] [--max-width <integer>]
                               [--print-empty] [--noindent] [--prefix PREFIX]
                               (--image <image> | --volume <volume>) --flavor
                               <flavor>
                               [--security-group <security-group-name>]
                               [--key-name <key-name>]
                               [--property <key=value>]
                               [--file <dest-filename=source-filename>]
                               [--user-data <user-data>]
                               [--availability-zone <zone-name>]
                               [--block-device-mapping <dev-name=mapping>]
                               [--nic <net-id=net-uuid,v4-fixed-ip=ip-addr,v6-fixed-ip=ip-addr,port-id=port-uuid,auto,none>]
                               [--hint <key=value>]
                               [--config-drive <config-drive-volume>|True]
                               [--min <count>] [--max <count>] [--wait]
                               <server-name>
openstack server create: error: argument --flavor is required
```

Der Fehler besagt, dass kein Flavor angegeben ist.

Damit nun eine Instanz gestartet werden kann, wird dem Befehl noch der
entsprechende Flavor hinzugefügt.

Um zu sehen welche Flavors bereit stehen, führen wir folgenden Befehl
aus:

```bash
$ openstack flavor list
+--------------------------------------+------------+-------+------+-----------+-------+-----------+
| ID                                   | Name       |   RAM | Disk | Ephemeral | VCPUs | Is Public |
+--------------------------------------+------------+-------+------+-----------+-------+-----------+
| 090bcc91-6207-465d-aff0-bfcc10a9e063 | m1.medium  |  8192 |   20 |         0 |     4 | True      |
| 4ade7a50-f829-4bf6-af15-266798ea8d6f | win.large  | 32768 |   80 |         0 |     8 | True      |
| 5dd72380-088e-48cd-9a18-112cb5a9cab5 | win.small  |  8192 |   80 |         0 |     2 | True      |
| 884d5b93-1467-4bc1-a445-ff7c74271cbd | m1.micro   |  1024 |   20 |         0 |     1 | True      |
| b7c4fa0b-7960-4311-a86b-507dbf58e8ac | m1.small   |  4096 |   20 |         0 |     2 | True      |
| d45e3029-8364-4e4c-beab-242e8b4622a3 | win.medium | 16384 |   80 |         0 |     4 | True      |
| dfead62e-96a8-46e9-bdae-342ecce32d41 | win.micro  |  2048 |   80 |         0 |     1 | True      |
| ed18c320-324a-487f-88e1-3e9eb9244509 | m1.large   | 16384 |   20 |         0 |     8 | True      |
+--------------------------------------+------------+-------+------+-----------+-------+-----------+
```

Sollte man nun den Befehl `openstack stack create Beispiel` mit
`--flavor m1.micro` starten, würde erneut eine Fehlermeldung angezeigt
werden, da weitere Parameter fehlen.

Um eine Instanz über diesen Weg zu starten, wird neben dem
Flavor(`--flavor`) auch noch der SSH-Key (`--key-name`), das
Image (`--image`), das verfügbare Netz (`--network`, in alten Versionen
des Clients muss `--nic net_id=` verwendet werden) und eine
SecurityGroup (`--security-group`) benötigt.

Der SSH-Key wurde im letzten Schritt bereits erstellt und braucht so
nicht erneut angelegt werden.

Damit fehlen noch das Image (`--image`) und das Netz.

Starten wir zunächst mit dem Image, wie bereits bei dem Flavor, kann
auch hier eine Übersicht der möglichen Images mit folgendem Befehl
angezeigt werden:

```bash
$ openstack image list
+--------------------------------------+---------------------------------------+--------+
| ID                                   | Name                                  | Status |
+--------------------------------------+---------------------------------------+--------+
| fd8ad5aa-6b33-4198-a05d-8be42fc0f20e | CentOS 7 - Latest                     | active |
| 82242d21-d990-4fc2-92a5-c7bd7820e790 | Ubuntu 16.04 Xenial Xerus - Latest    | active |
| 8e82fd42-3d6f-44a7-9f20-92f5661823cf | Windows Server 2012 R2 Std - Latest   | active |
| 536c086c-d2a4-43dd-80ea-a9d05ee2b97f | Windows Server 2016 Std - Latest      | active |
| c94ced87-a03e-4eec-89f7-48f2c0ec6cd2 | debian-9.1.5-20170910-openstack-amd64 | active |
| b1195ddf-9336-42a7-a134-4f2e7ea57710 | iNNOVO-OPNsense-17.7.8                | active |
| 9134b6ed-8c5a-4a9a-907e-733dc2b5f0ef | iNNOVO_pfSense 2.3.4                  | active |
+--------------------------------------+---------------------------------------+--------+
```

Nun fehlt noch ein Netzwerk.

An dieser Stelle gibt es 2 Möglichkeiten für das Netzwerk, zum einen
kann man ein sehr simples Netzwerk anlegen und so die Instanz starten,
dafür nutzt man folgenden Befehl:

```bash
$ openstack network create BeispielNetzwerk
+---------------------------+--------------------------------------+
| Field                     | Value                                |
+---------------------------+--------------------------------------+
| admin_state_up            | UP                                   |
| availability_zone_hints   |                                      |
| availability_zones        |                                      |
| created_at                | 2017-12-08T08:32:44Z                 |
| description               |                                      |
| dns_domain                | None                                 |
| id                        | a783d691-7efe-4f67-9226-99a014fa8926 |
| ipv4_address_scope        | None                                 |
| ipv6_address_scope        | None                                 |
| is_default                | False                                |
| mtu                       | 1500                                 |
| name                      | BeispielNetzwerk                     |
| port_security_enabled     | True                                 |
| project_id                | b15cde70d85749689e08106f973bb002     |
| provider:network_type     | None                                 |
| provider:physical_network | None                                 |
| provider:segmentation_id  | None                                 |
| qos_policy_id             | None                                 |
| revision_number           | 2                                    |
| router:external           | Internal                             |
| segments                  | None                                 |
| shared                    | False                                |
| status                    | ACTIVE                               |
| subnets                   |                                      |
| updated_at                | 2017-12-08T08:32:44Z                 |
+---------------------------+--------------------------------------+
```

Der Nachteil an diesem Netzwerk ist, dass man die Instanz nicht
erreichen kann. Soll die Instanz nutzbar sein, wird ein funktionierendes
Netz benötigt, welches in [Schritt 10](schritt10.md) komplett angelegt
wird. 

Nachdem alle Bestandteile jetzt bekannst sind, kann die erste Instanz
erstellt werden.

Dafür wird das `--flavor m1.small`, der SSH-Key aus Schritt 6, das Netzwerk
von weiter oben, das `--image "Ubuntu 16.04 Xenial Xerus - Latest"` und die
`--security-group default`:

```bash
$ openstack server create BeispielServer --flavor m1.small --key-name Beispiel --image 82242d21-d990-4fc2-92a5-c7bd7820e790 --network=BeispielNetzwerk --security-group default
+-----------------------------+--------------------------------------------------------+
| Field                       | Value                                                  |
+-----------------------------+--------------------------------------------------------+
| OS-DCF:diskConfig           | MANUAL                                                 |
| OS-EXT-AZ:availability_zone | es1                                                    |
| OS-EXT-STS:power_state      | NOSTATE                                                |
| OS-EXT-STS:task_state       | scheduling                                             |
| OS-EXT-STS:vm_state         | building                                               |
| OS-SRV-USG:launched_at      | None                                                   |
| OS-SRV-USG:terminated_at    | None                                                   |
| accessIPv4                  |                                                        |
| accessIPv6                  |                                                        |
| addresses                   |                                                        |
| adminPass                   | 6MotuEMy4c3t                                           |
| config_drive                |                                                        |
| created                     | 2017-12-06T14:15:02Z                                   |
| flavor                      | m1.small (676d2587-b5aa-49eb-998d-d91c1bd6c056)        |
| hostId                      |                                                        |
| id                          | 44ff2688-4ce5-417d-962b-3a80199bf1bc                   |
| image                       | cirros-tempest1 (2fbe66ef-adc8-44d0-b2e2-03d95dc36936) |
| key_name                    | cg                                                     |
| name                        | BeispielServer                                         |
| progress                    | 0                                                      |
| project_id                  | 1e775e2cc71a461991be42d4fad8a5cb                       |
| properties                  |                                                        |
| security_groups             | name='3265503b-ac24-4f60-a8d0-466b7c812916'            |
| status                      | BUILD                                                  |
| updated                     | 2017-12-06T14:15:02Z                                   |
| user_id                     | b54fda3f4d1a484797b3ad4de9b3f4f9                       |
| volumes_attached            |                                                        |
+-----------------------------+--------------------------------------------------------+
```

Weitere mögliche Parameter für die Erstellung einer Instanz können
mit `--help` abgefragt werden:

```bash
$ openstack server create --help
usage: openstack server create [-h] [-f {json,shell,table,value,yaml}]
                               [-c COLUMN] [--max-width <integer>]
                               [--print-empty] [--noindent] [--prefix PREFIX]
                               (--image <image> | --volume <volume>) --flavor
                               <flavor>
                               [--security-group <security-group-name>]
                               [--key-name <key-name>]
                               [--property <key=value>]
                               [--file <dest-filename=source-filename>]
                               [--user-data <user-data>]
                               [--availability-zone <zone-name>]
                               [--block-device-mapping <dev-name=mapping>]
                               [--nic <net-id=net-uuid,v4-fixed-ip=ip-addr,v6-fixed-ip=ip-addr,port-id=port-uuid,auto,none>]
                               [--hint <key=value>]
                               [--config-drive <config-drive-volume>|True]
                               [--min <count>] [--max <count>] [--wait]
                               <server-name>

Create a new server

positional arguments:
  <server-name>         New server name

optional arguments:
  -h, --help            show this help message and exit
  --image <image>       Create server boot disk from this image (name or ID)
  --volume <volume>     Create server using this volume as the boot disk (name
                        or ID)
  --flavor <flavor>     Create server with this flavor (name or ID)
  --security-group <security-group-name>
                        Security group to assign to this server (name or ID)
                        (repeat option to set multiple groups)
  --key-name <key-name>
                        Keypair to inject into this server (optional
                        extension)
  --property <key=value>
                        Set a property on this server (repeat option to set
                        multiple values)
  --file <dest-filename=source-filename>
                        File to inject into image before boot (repeat option
                        to set multiple files)
  --user-data <user-data>
                        User data file to serve from the metadata server
  --availability-zone <zone-name>
                        Select an availability zone for the server
  --block-device-mapping <dev-name=mapping>
                        Map block devices; map is
                        <id>:<type>:<size(GB)>:<delete_on_terminate> (optional
                        extension)
  --nic <net-id=net-uuid,v4-fixed-ip=ip-addr,v6-fixed-ip=ip-addr,port-id=port-uuid,auto,none>
                        Create a NIC on the server. Specify option multiple
                        times to create multiple NICs. Either net-id or port-
                        id must be provided, but not both. net-id: attach NIC
                        to network with this UUID, port-id: attach NIC to port
                        with this UUID, v4-fixed-ip: IPv4 fixed address for
                        NIC (optional), v6-fixed-ip: IPv6 fixed address for
                        NIC (optional), none: (v2.37+) no network is attached,
                        auto: (v2.37+) the compute service will automatically
                        allocate a network. Specifying a --nic of auto or none
                        cannot be used with any other --nic value.
  --hint <key=value>    Hints for the scheduler (optional extension)
  --config-drive <config-drive-volume>|True
                        Use specified volume as the config drive, or 'True' to
                        use an ephemeral drive
  --min <count>         Minimum number of servers to launch (default=1)
  --max <count>         Maximum number of servers to launch (default=1)
  --wait                Wait for build to complete

output formatters:
  output formatter options

  -f {json,shell,table,value,yaml}, --format {json,shell,table,value,yaml}
                        the output format, defaults to table
  -c COLUMN, --column COLUMN
                        specify the column(s) to include, can be repeated

table formatter:
  --max-width <integer>
                        Maximum display width, <1 to disable. You can also use
                        the CLIFF_MAX_TERM_WIDTH environment variable, but the
                        parameter takes precedence.
  --print-empty         Print empty table if there is no data to show.

json formatter:
  --noindent            whether to disable indenting the JSON

shell formatter:
  a format a UNIX shell can parse (variable="value")

  --prefix PREFIX       add a prefix to all variable names
```

Abschluss
---------

Nachdem wir in diesem Schritt nicht nur eine neue Instanz erstellt ,
sondern auch noch einige Basis Befehle für OpenStack angewedet haben.
Werden wir im [nächsten Schritt](schritt08.md) diese VM wieder löschen.
