---
title: Flavor Spezifikationen
lang: de
permalink: /optimist/specs/flavor_specification/
parent: Spezifikationen
nav_order: 9100
last_modified_date: 2022-03-29
---

# Flavor Spezifikationen

Der Begriff "Flavor" bezeichnet im OpenStack-Kontext ein Hardware-Profil, das eine virtuelle Maschine nutzt bzw. nutzen kann. Im Optimist
sind diverse Standard-Hardwareprofile (Flavors) eingerichtet. Diese haben unterschiedliche Limits und Begrenzungen, welche hier für alle
verfügbaren Flavors aufgelistet sind.

## Migration zwischen Flavor-Typen

Um die Flavors bestehender Instanzen zu ändern, kann die OpenStack-Option „Resize Instance“ entweder über das Dashboard oder die CLI verwendet werden. Dies führt zu einem Neustart der Instanz, aber der Inhalt der Instanz bleibt erhalten.

{: .alert .alert-error }

**WARNUNG**  
Dies gilt nicht für l1 (localstorage) Flavors!  
Bitte lesen Sie für weitere Informationen [Storage → Localstorage](/optimist/storage/localstorage/#openstack-features)

## Deprecated Flavor-Typen

Die folgenden Flavor-Typen gelten derzeit als veraltet und die Entfernung dieser Flavor-familien ist für die nahe Zukunft geplant. Wir werden regelmäßig überprüfen, ob diese Flavors noch verwendet werden, wenn nicht, werden wir sie auf privat setzen, um zu vermeiden, dass neue Instanzen damit erstellt werden:

- m1-Familie (deprecated)
- e1-Familie (e = equal) (deprecated)
- r1-Familie (r = ram) (deprecated)
- Memory-Flavors (deprecated)
- Windows-Flavors (deprecated)

## Flavor-Typen

### Standard-Flavors

| Bezeichnung | Kerne |   RAM |  Disk | IOPS Limits (read/write) | IO throughput rate (read/write) | Network Bandwidth |
| :---------- | ----: | ----: | ----: | -----------------------: | ------------------------------: | ----------------: |
| s1.micro    |     1 |  2 GB | 20 GB |              1000 / 1000 |             200 MB/s / 200 MB/s |          1 Gbit/s |
| s1.small    |     2 |  4 GB | 20 GB |              1000 / 1000 |             200 MB/s / 200 MB/s |          2 Gbit/s |
| s1.medium   |     4 |  8 GB | 20 GB |              1000 / 1000 |             200 MB/s / 200 MB/s |          3 Gbit/s |
| s1.large    |     8 | 16 GB | 20 GB |              1000 / 1000 |             200 MB/s / 200 MB/s |          4 Gbit/s |
| s1.xlarge   |    16 | 32 GB | 20 GB |              1000 / 1000 |             200 MB/s / 200 MB/s |          4 Gbit/s |
| s1.2xlarge  |    30 | 64 GB | 20 GB |              1000 / 1000 |             200 MB/s / 200 MB/s |          4 Gbit/s |

### Dedicated CPU Flavors

| Bezeichnung | Kerne |   RAM  |  Disk | IOPS Limits (read/write) | IO throughput rate (read/write) | Network Bandwidth |
| :---------- | -----:| -----: | ----: | -----------------------: | ------------------------------: | ----------------: |
| d1.micro    |    1  |  8 GB  | 20 GB |              1000 / 1000 |             200 MB/s / 200 MB/s |          1 Gbit/s |
| d1.small    |    2  | 16 GB  | 20 GB |              1000 / 1000 |             200 MB/s / 200 MB/s |          2 Gbit/s |
| d1.medium   |    4  | 32 GB  | 20 GB |              1000 / 1000 |             200 MB/s / 200 MB/s |          3 Gbit/s |
| d1.large    |    8  | 64 GB  | 20 GB |              1000 / 1000 |             200 MB/s / 200 MB/s |          4 Gbit/s |
| d1.xlarge   |   16  | 128 GB | 20 GB |              1000 / 1000 |             200 MB/s / 200 MB/s |          4 Gbit/s |
| d1.2xlarge  |   30  | 256 GB | 20 GB |              1000 / 1000 |             200 MB/s / 200 MB/s |          4 Gbit/s |

### Localstorage Flavors

| Bezeichnung | Kerne  |   RAM  |  Disk    | IOPS Limits (read/write) | IO throughput rate (read/write) | Network Bandwidth |
| :---------- | -----: | -----: | -------: | -----------------------: | ------------------------------: | ----------------: |
| l1.micro    |    1   |   8 GB |   300 GB |            25000 / 10000 |             125 MB/s / 60 MB/s  |          1 Gbit/s |
| l1.small    |    2   |  16 GB |   600 GB |            50000 / 25000 |             250 MB/s / 125 MB/s |          1 Gbit/s |
| l1.medium   |    4   |  32 GB |  1200 GB |           100000 / 50000 |             500 MB/s / 250 MB/s |          1 Gbit/s |
| l1.large    |    8   |  64 GB |  2500 GB |          100000 / 100000 |            1000 MB/s / 500 MB/s |          1 Gbit/s |
| l1.xlarge   |   16   | 128 GB |  5000 GB |          100000 / 100000 |           2000 MB/s / 1125 MB/s |          1 Gbit/s |
| l1.2xlarge  |   30   | 256 GB | 10000 GB |          100000 / 100000 |           2000 MB/s / 2000 MB/s |          1 Gbit/s |

### m1-Familie (deprecated)

| Bezeichnung | Kerne |   RAM |  Disk | IOPS Limits (read/write) | IO throughput rate (read/write) | Network Bandwidth |
| :---------- | ----: | ----: | ----: | -----------------------: | ------------------------------: | ----------------: |
| m1.micro    |     1 |  1 GB | 20 GB |              1000 / 1000 |             200 MB/s / 200 MB/s |          1 Gbit/s |
| m1.small    |     2 |  4 GB | 20 GB |              1000 / 1000 |             200 MB/s / 200 MB/s |          2 Gbit/s |
| m1.medium   |     4 |  8 GB | 20 GB |              1000 / 1000 |             200 MB/s / 200 MB/s |          3 Gbit/s |
| m1.large    |     8 | 16 GB | 20 GB |              1000 / 1000 |             200 MB/s / 200 MB/s |          4 Gbit/s |
| m1.xlarge   |    16 | 32 GB | 20 GB |              1000 / 1000 |             200 MB/s / 200 MB/s |          4 Gbit/s |
| m1.xxlarge  |    30 | 64 GB | 20 GB |              1000 / 1000 |             200 MB/s / 200 MB/s |          4 Gbit/s |

### e1-Familie (e = equal) (deprecated)

| Bezeichnung | Kerne |   RAM |  Disk | IOPS Limits (read/write) | IO throughput rate (read/write) | Network Bandwidth |
| :---------- | ----: | ----: | ----: | -----------------------: | ------------------------------: | ----------------: |
| e1.small    |     2 |  2 GB | 20 GB |              1000 / 1000 |             200 MB/s / 200 MB/s |          1 Gbit/s |
| e1.medium   |     4 |  4 GB | 20 GB |              1000 / 1000 |             200 MB/s / 200 MB/s |          2 Gbit/s |
| e1.large    |     8 |  8 GB | 20 GB |              1000 / 1000 |             200 MB/s / 200 MB/s |          3 Gbit/s |
| e1.xlarge   |    16 | 16 GB | 20 GB |              1000 / 1000 |             200 MB/s / 200 MB/s |          4 Gbit/s |
| e1.xxlarge  |    30 | 32 GB | 20 GB |              1000 / 1000 |             200 MB/s / 200 MB/s |          4 Gbit/s |

### r1-Familie (r = ram) (deprecated)

| Bezeichnung | Kerne |   RAM |  Disk | IOPS Limits (read/write) | IO throughput rate (read/write) | Network Bandwidth |
| :---------- | ----: | ----: | ----: | -----------------------: | ------------------------------: | ----------------: |
| r1.small    |     2 |  6 GB | 20 GB |              1000 / 1000 |             200 MB/s / 200 MB/s |          1 Gbit/s |
| r1.medium   |     4 | 12 GB | 20 GB |              1000 / 1000 |             200 MB/s / 200 MB/s |          2 Gbit/s |
| r1.large    |     8 | 32 GB | 20 GB |              1000 / 1000 |             200 MB/s / 200 MB/s |          3 Gbit/s |
| r1.xlarge   |    16 | 48 GB | 20 GB |              1000 / 1000 |             200 MB/s / 200 MB/s |          4 Gbit/s |

### Memory-flavors (deprecated)

| Bezeichnung | Kerne |   RAM |  Disk | IOPS Limits (read/write) | IO throughput rate (read/write) | Network Bandwidth |
| :---------- | ----: | ----: | ----: | -----------------------: | ------------------------------: | ----------------: |
| mem.micro   |     4 | 16 GB | 20 GB |              1000 / 1000 |             200 MB/s / 200 MB/s |          1 Gbit/s |
| mem.small   |     8 | 32 GB | 20 GB |              1000 / 1000 |             200 MB/s / 200 MB/s |          2 Gbit/s |
| mem.medium  |     8 | 64 GB | 20 GB |              1000 / 1000 |             200 MB/s / 200 MB/s |          3 Gbit/s |

### Windows-flavors (deprecated)

| Bezeichnung | Kerne |   RAM |  Disk | IOPS Limits (read/write) | IO throughput rate (read/write) | Network Bandwidth |
| :---------- | ----: | ----: | ----: | -----------------------: | ------------------------------: | ----------------: |
| win.micro   |     1 |  2 GB | 80 GB |              1000 / 1000 |             200 MB/s / 200 MB/s |          1 Gbit/s |
| win.small   |     2 |  8 GB | 80 GB |              1000 / 1000 |             200 MB/s / 200 MB/s |          2 Gbit/s |
| win.medium  |     4 | 16 GB | 80 GB |              1000 / 1000 |             200 MB/s / 200 MB/s |          3 Gbit/s |
| win.large   |     8 | 32 GB | 80 GB |              1000 / 1000 |             200 MB/s / 200 MB/s |          4 Gbit/s |
| win.xlarge  |    16 | 64 GB | 80 GB |              1000 / 1000 |             200 MB/s / 200 MB/s |          4 Gbit/s |
