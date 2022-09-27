---
title: Volume Specifications
lang: en
permalink: /optimist/specs/volume_specification/
parent: Specifications
nav_order: 9400
last_modified_date: 2022-02-02
---

# Volume Specifications

In OpenStack, “volumes” are persistent storage that you can attach to your running OpenStack Compute instances to. In Optimist we have set up three classes of service for volumes. These have different limits, which are detailed below.

## Volume Types

We have three main volume types:

* high-iops
* default
* low-iops

## Volume Type List

An overview of the three volume types below:

| Name          | Read Bytes Sec | Read IOPS Sec  | Write Bytes Sec | Write IOPS Sec |
| :------------ | -------------: | -------------: | --------------: | -------------: |
| high-iops     | 524288000      | 10000          | 524288000       | 10000          |
| default       | 209715200      | 2500           | 209715200       | 2500           |
| low-iops      | 52428800       | 300            | 52428800        | 300            |

## Choosing a Volume Type

You can select one of the three volume types upon creation of a volume with the following command (Unless otherwise specified, the type "default" is always used):
`$ openstack volume create <volume-name> --size 10 --type high-iops`
