---
title: S3 Compatible Object Storage
lang: en
permalink: /optimist/storage/s3_documentation/
nav_order: 3100
parent: Storage
has_children: true
---

S3 Compatible Object Storage Introduction
=================================================

- [Create and use S3 credentials](./CreateAndUseS3CredentialsEN.md)
- [Create and delete a bucket](./CreateAndDeleteBucketEN.md)
- [Enable and disable versioning and also delete a versioned object](./VersioningEN.md)
- [Upload and delete an object](./UploadAndDeleteObjectEN.md)

What exactly is Object Storage?
-----

Object storage is an alternative to classic block storage. The individual data is not assigned to individual blocks on a device, but is stored as binary objects within a storage cluster. The necessary metadata is stored in a separate database.
The storage cluster consists of several individual servers (nodes), which in turn have several storage devices installed. With the storage devices, we have a mix of classic HDDs, SSDs, and modern NVMe solutions.
The CRUSH algorithm, which is implemented on the server-side of the Object Storage, decides which device an object ultimately lands on.

How can I access it?
-----

Access to this type of storage is accomplished exclusively via HTTPS. For this, we provide a highly available endpoint, where the individual operations can be executed.
We support two independent protocols:

- S3
- Swift

S3 is a protocol created by Amazon in order to work with this type of data. Swift is the protocol provided by the OpenStack service of the same name.
Regardless of which protocol you use, you always have access to all your data. You can, therefore, use both protocols in combination.
There are tools for all common platforms to work with the data in the object storage:

- Windows: s3cmd, Cyberduck
- MacOS: s3cmd, Cyberduck
- Linux: s3cmd

In addition, there are integrations in all popular programming languages.

How secure is my data?
-----

The Object Storage is based on our Openstack Cloud Platform with the distributed Ceph Storage Cluster. The objects are distributed and replicated on the server-side across several storage devices.
Ceph ensures the replication and integrity of the data sets. If a server or hard disk fails, the affected data records are replicated to available servers and the desired replication level is automatically restored.

In addition, the data is mirrored to another data centre on another dedicated storage cluster and can be used from there in the event of a disaster.

Advantages at a glance
-----

- Deployment via API: The HTTPS interface is compatible with both the Amazon S3 API and the OpenStack Swift API.
- Supports all common operating systems and programming languages.
- Full scalability - Storage can be used dynamically.
- Maximum reliability thanks to integrated replication and mirroring via 2 independent data centres.
- Access is possible from almost any internet-enabled device - thus a good alternative to NFS and Co.
- PAYG billing according to used monthly average.
- Transparent billing and therefore good predictability - no extra traffic costs or costs for access to the data.
- Ability to define s3 lifecycle policies to manage the objects inside the buckets.
