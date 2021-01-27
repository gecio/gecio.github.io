---
layout: post
title:  "Flavor Specifications"
permalink: /en/specs/
ref: specs
lang: en
order: 10
---
Flavor Specifications
===============

In the OpenStack context the term "flavor" refers to a hardware profile that can be used for a virtual machine.
In Optimist we have set up various standard hardware profiles (flavors).
These have different limits, which are listed below for all available flavors.

Standard Flavors
---------

| Name               | Virtual Cores   | RAM in GB      | Disk in GB    | IOPS Limits (read/write)   | IO throughput rate per second (read/write)   | Network Bandwidth (M/Bit)  |
|:------------------:|:---------------:|:--------------:|:-------------:|:--------------------------:|:--------------------------------------------:|:--------------------------:|
| m1.micro           |          1      |        1       |      20       |       1000/1000            |          200MByte / 200MByte                 |       1Gbit / sec          |
| m1.small           |          2      |        4       |      20       |       1000/1000            |          200MByte / 200MByte                 |       2Gbit / sec          |
| m1.medium          |          4      |        8       |      20       |       1000/1000            |          200MByte / 200MByte                 |       3Gbit / sec          |
| m1.large           |          8      |        16      |      20       |       1000/1000            |          200MByte / 200MByte                 |       4Gbit / sec          |
| m1.xlarge          |          16     |        32      |      20       |       1000/1000            |          200MByte / 200MByte                 |       4Gbit / sec          |
| m1.xxlarge         |          30     |        64      |      20       |       1000/1000            |          200MByte / 200MByte                 |       4Gbit / sec          |

HPC Flavors
---------

| Name                | Virtual Cores   | RAM in GB      | Disk in GB    | IOPS Limits (read/write)   | IO throughput rate per second (read/write)   | Network Bandwidth (M/Bit)  |
|:-------------------:|:---------------:|:--------------:|:-------------:|:--------------------------:|:--------------------------------------------:|:--------------------------:|
| hpc.micro           |         8       |       4        |      20       |      1000/1000             |         200MByte / 200MByte                  |      1Gbit / sec           |
| hpc.small           |         16      |       8        |      20       |      1000/1000             |         200MByte / 200MByte                  |      2Gbit / sec           |
| hpc.medium          |         30      |       16       |      20       |      1000/1000             |         200MByte / 200MByte                  |      3Gbit / sec           |

Memory Flavors
---------

| Name                | Virtual Cores   | RAM in GB      | Disk in GB    | IOPS Limits (read/write)   | IO throughput rate per second (read/write)   | Network Bandwidth (M/Bit)  |
|:-------------------:|:---------------:|:--------------:|:-------------:|:--------------------------:|:--------------------------------------------:|:--------------------------:|
| mem.micro           |         4       |       16       |      20       |      1000/1000             |         200MByte / 200MByte                  |      1Gbit / sec           |
| mem.small           |         8       |       32       |      20       |      1000/1000             |         200MByte / 200MByte                  |      2Gbit / sec           |
| mem.medium          |         8       |       64       |      20       |      1000/1000             |         200MByte / 200MByte                  |      3Gbit / sec           |

Windows Flavors
---------

| Name               | Virtual Cores   | RAM in GB      | Disk in GB    | IOPS Limits (read/write)   | IO throughput rate per second (read/write)   | Network Bandwidth (M/Bit)  |
|:------------------:|:---------------:|:--------------:|:-------------:|:--------------------------:|:--------------------------------------------:|:--------------------------:|
| win.micro          |          1      |        2       |      80       |       1000/1000            |          200MByte / 200MByte                 |       1Gbit / sec          |
| win.small          |          2      |        8       |      80       |       1000/1000            |          200MByte / 200MByte                 |       2Gbit / sec          |
| win.medium         |          4      |        16      |      80       |       1000/1000            |          200MByte / 200MByte                 |       3Gbit / sec          |
| win.large          |          8      |        32      |      80       |       1000/1000            |          200MByte / 200MByte                 |       4Gbit / sec          |
| win.xlarge         |          16     |        64      |      80       |       1000/1000            |          200MByte / 200MByte                 |       4Gbit / sec          |

e1-Family (e = equal)
---------

| Name               | Virtual Cores   | RAM in GB      | Disk in GB    | IOPS Limits (read/write)   | IO throughput rate per second (read/write)   | Network Bandwidth (M/Bit)  |
|:------------------:|:---------------:|:--------------:|:-------------:|:--------------------------:|:--------------------------------------------:|:--------------------------:|
| e1.small           |          2      |        2       |      20       |       1000/1000            |          200MByte / 200MByte                 |           1Gbit / sec      |
| e1.medium          |          4      |        4       |      20       |       1000/1000            |          200MByte / 200MByte                 |           2Gbit / sec      |
| e1.large           |          8      |        8       |      20       |       1000/1000            |          200MByte / 200MByte                 |           3Gbit / sec      |
| e1.xlarge          |          16     |        16      |      20       |       1000/1000            |          200MByte / 200MByte                 |           4Gbit / sec      |
| e1.xxlarge         |          30     |        32      |      20       |       1000/1000            |          200MByte / 200MByte                 |           4Gbit / sec      |

r1-Family (r = ram)
---------

| Name               | Virtual Cores   | RAM in GB      | Disk in GB    | IOPS Limits (read/write)   | IO throughput rate per second (read/write)   | Network Bandwidth (M/Bit)  |
|:------------------:|:---------------:|:--------------:|:-------------:|:--------------------------:|:--------------------------------------------:|:--------------------------:|
| r1.small           |          2      |        6       |      20       |       1000/1000            |          200MByte / 200MByte                 |           1Gbit / sec      |
| r1.medium          |          4      |        12      |      20       |       1000/1000            |          200MByte / 200MByte                 |           2Gbit / sec      |
| r1.large           |          8      |        32      |      20       |       1000/1000            |          200MByte / 200MByte                 |           3Gbit / sec      |
| r1.xlarge          |          16     |        48      |      20       |       1000/1000            |          200MByte / 200MByte                 |           4Gbit / sec      |





