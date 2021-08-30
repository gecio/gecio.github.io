---
title: OpenStack Images
lang: en
permalink: /optimist/specs/images/
parent: Specifications
nav_order: 9400
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
- Flatcar_Production - Latest
- Ubuntu 16.04 Xenial Xerus - Latest
- Ubuntu 18.04 Bionic Beaver - Latest
- Ubuntu 20.04 Focal Fossa - Latest
- Windows Server 2019 Standard Core - Latest
- Windows Server 2019 Standard GUI - Latest
