---
title: OpenStack Images
lang: en
permalink: /optimist/specs/images/images_overview/
parent: Images
nav_order: 9410
---

# OpenStack Images

## Introduction

The Optimist platform provides a list of public OpenStack images which are updated regularly. Automated checks for new images are performed each day. If new vendor images are located, we obtain the new images, test, and publish them within 24 hours of the vendor release.
Older images will only be rotated out only once all users have ceased using that version.

## View Image List

A list of our public images can be viewed from the OpenStack CLI with the following command:

`$ openstack image list`

Alternatively you can view the image list from the [dashboard]([https://dashboard.optimist.innovo.cloud/project/images](https://dashboard.optimist.innovo.cloud/project/images)), under Project > Images, and filtering by Visibility: Public.

## Overview of Available Images

Here is a list of our currently available public images:

- CentOS 7 - Latest
- CentOS 8.1 - Latest
- Debian Buster - Latest
- Debian 11 Bullseye - Latest
- Flatcar_Production - Latest
- Ubuntu 16.04 Xenial Xerus - Latest
- Ubuntu 18.04 Bionic Beaver - Latest
- Ubuntu 20.04 Focal Fossa - Latest
- Windows Server 2019 Standard Core - Latest
- Windows Server 2019 Standard GUI - Latest

## Community Images

In addition to our public images, we also offer community images. Here you will find older versions of the images in public list.

A list of available community images can be viewed from the OpenStack CLI with the following command:

`$ openstack image list --community`

The community images will only be rotated out once zero projects are using the images. Please be aware that we cannot offer the same level of support for community images as for our provider-supplied public images.

While we cannot guarantee availability of specific distro versions, if known, you can compare the source hash of our community images with the following command:

$ openstack image show ee689d40-c980-40b2-ac6d-e74b948e558b -f json | grep 'source_hash.*'
"source_hash": "525e89ea678d4cf27d2255cb6a39c8f38727799694e89a3fb3ca7e08c17566f5cde81e2d6ceaa08a9e26db502d472e32dfd0e9478906ad22055901c5548c92b2",
