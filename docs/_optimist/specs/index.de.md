---
title: Flavor Spezifikationen
lang: de
permalink: /optimist/specs/
nav_order: 6000
last_modified_date: 2021-03-02
---

# Flavor Spezifikationen

Der Begriff "Flavor" bezeichnet im OpenStack-Kontext ein Hardware-Profil, das eine virtuelle Maschine nutzt bzw. nutzen kann. Im Optimist
sind diverse Standard-Hardwareprofile (Flavors) eingerichtet. Diese haben unterschiedliche Limits und Begrenzungen, welche hier für alle
verfügbaren Flavors aufgelistet sind.

## Standard-Flavors

| Bezeichnung | Virtuelle Kerne |          RAM |         Disk | IOPS Limits (read/write) |   IO throughput rate (read/write) | Network Bandwidth |
| :---------- | --------------: | -----------: | -----------: | -----------------------: | --------------------------------: | ----------------: |
| m1.micro    |               1 |  1&thinsp;GB | 20&thinsp;GB |              1000 / 1000 | 200&thinsp;MB/s / 200&thinsp;MB/s |   1&thinsp;Gbit/s |
| m1.small    |               2 |  4&thinsp;GB | 20&thinsp;GB |              1000 / 1000 | 200&thinsp;MB/s / 200&thinsp;MB/s |   2&thinsp;Gbit/s |
| m1.medium   |               4 |  8&thinsp;GB | 20&thinsp;GB |              1000 / 1000 | 200&thinsp;MB/s / 200&thinsp;MB/s |   3&thinsp;Gbit/s |
| m1.large    |               8 | 16&thinsp;GB | 20&thinsp;GB |              1000 / 1000 | 200&thinsp;MB/s / 200&thinsp;MB/s |   4&thinsp;Gbit/s |
| m1.xlarge   |              16 | 32&thinsp;GB | 20&thinsp;GB |              1000 / 1000 | 200&thinsp;MB/s / 200&thinsp;MB/s |   4&thinsp;Gbit/s |
| m1.xxlarge  |              30 | 64&thinsp;GB | 20&thinsp;GB |              1000 / 1000 | 200&thinsp;MB/s / 200&thinsp;MB/s |   4&thinsp;Gbit/s |

## HPC-flavors

| Bezeichnung | Virtuelle Kerne |          RAM |         Disk | IOPS Limits (read/write) |   IO throughput rate (read/write) | Network Bandwidth |
| :---------- | --------------: | -----------: | -----------: | -----------------------: | --------------------------------: | ----------------: |
| hpc.micro   |               8 |  4&thinsp;GB | 20&thinsp;GB |              1000 / 1000 | 200&thinsp;MB/s / 200&thinsp;MB/s |   1&thinsp;Gbit/s |
| hpc.small   |              16 |  8&thinsp;GB | 20&thinsp;GB |              1000 / 1000 | 200&thinsp;MB/s / 200&thinsp;MB/s |   2&thinsp;Gbit/s |
| hpc.medium  |              30 | 16&thinsp;GB | 20&thinsp;GB |              1000 / 1000 | 200&thinsp;MB/s / 200&thinsp;MB/s |   3&thinsp;Gbit/s |

## Memory-flavors

| Bezeichnung | Virtuelle Kerne |          RAM |         Disk | IOPS Limits (read/write) |   IO throughput rate (read/write) | Network Bandwidth |
| :---------- | --------------: | -----------: | -----------: | -----------------------: | --------------------------------: | ----------------: |
| mem.micro   |               4 | 16&thinsp;GB | 20&thinsp;GB |              1000 / 1000 | 200&thinsp;MB/s / 200&thinsp;MB/s |   1&thinsp;Gbit/s |
| mem.small   |               8 | 32&thinsp;GB | 20&thinsp;GB |              1000 / 1000 | 200&thinsp;MB/s / 200&thinsp;MB/s |   2&thinsp;Gbit/s |
| mem.medium  |               8 | 64&thinsp;GB | 20&thinsp;GB |              1000 / 1000 | 200&thinsp;MB/s / 200&thinsp;MB/s |   3&thinsp;Gbit/s |

## Windows-flavors

| Bezeichnung | Virtuelle Kerne |          RAM |         Disk | IOPS Limits (read/write) |   IO throughput rate (read/write) | Network Bandwidth |
| :---------- | --------------: | -----------: | -----------: | -----------------------: | --------------------------------: | ----------------: |
| win.micro   |               1 |  2&thinsp;GB | 80&thinsp;GB |              1000 / 1000 | 200&thinsp;MB/s / 200&thinsp;MB/s |   1&thinsp;Gbit/s |
| win.small   |               2 |  8&thinsp;GB | 80&thinsp;GB |              1000 / 1000 | 200&thinsp;MB/s / 200&thinsp;MB/s |   2&thinsp;Gbit/s |
| win.medium  |               4 | 16&thinsp;GB | 80&thinsp;GB |              1000 / 1000 | 200&thinsp;MB/s / 200&thinsp;MB/s |   3&thinsp;Gbit/s |
| win.large   |               8 | 32&thinsp;GB | 80&thinsp;GB |              1000 / 1000 | 200&thinsp;MB/s / 200&thinsp;MB/s |   4&thinsp;Gbit/s |
| win.xlarge  |              16 | 64&thinsp;GB | 80&thinsp;GB |              1000 / 1000 | 200&thinsp;MB/s / 200&thinsp;MB/s |   4&thinsp;Gbit/s |

## e1-Familie (e = equal)

| Bezeichnung | Virtuelle Kerne |          RAM |         Disk | IOPS Limits (read/write) |   IO throughput rate (read/write) | Network Bandwidth |
| :---------- | --------------: | -----------: | -----------: | -----------------------: | --------------------------------: | ----------------: |
| e1.small    |               2 |  2&thinsp;GB | 20&thinsp;GB |              1000 / 1000 | 200&thinsp;MB/s / 200&thinsp;MB/s |   1&thinsp;Gbit/s |
| e1.medium   |               4 |  4&thinsp;GB | 20&thinsp;GB |              1000 / 1000 | 200&thinsp;MB/s / 200&thinsp;MB/s |   2&thinsp;Gbit/s |
| e1.large    |               8 |  8&thinsp;GB | 20&thinsp;GB |              1000 / 1000 | 200&thinsp;MB/s / 200&thinsp;MB/s |   3&thinsp;Gbit/s |
| e1.xlarge   |              16 | 16&thinsp;GB | 20&thinsp;GB |              1000 / 1000 | 200&thinsp;MB/s / 200&thinsp;MB/s |   4&thinsp;Gbit/s |
| e1.xxlarge  |              30 | 32&thinsp;GB | 20&thinsp;GB |              1000 / 1000 | 200&thinsp;MB/s / 200&thinsp;MB/s |   4&thinsp;Gbit/s |

## r1-Familie (r = ram)

| Bezeichnung | Virtuelle Kerne |          RAM |         Disk | IOPS Limits (read/write) |   IO throughput rate (read/write) | Network Bandwidth |
| :---------- | --------------: | -----------: | -----------: | -----------------------: | --------------------------------: | ----------------: |
| r1.small    |               2 |  6&thinsp;GB | 20&thinsp;GB |              1000 / 1000 | 200&thinsp;MB/s / 200&thinsp;MB/s |   1&thinsp;Gbit/s |
| r1.medium   |               4 | 12&thinsp;GB | 20&thinsp;GB |              1000 / 1000 | 200&thinsp;MB/s / 200&thinsp;MB/s |   2&thinsp;Gbit/s |
| r1.large    |               8 | 32&thinsp;GB | 20&thinsp;GB |              1000 / 1000 | 200&thinsp;MB/s / 200&thinsp;MB/s |   3&thinsp;Gbit/s |
| r1.xlarge   |              16 | 48&thinsp;GB | 20&thinsp;GB |              1000 / 1000 | 200&thinsp;MB/s / 200&thinsp;MB/s |   4&thinsp;Gbit/s |
