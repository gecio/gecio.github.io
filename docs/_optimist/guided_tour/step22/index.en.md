---
title: "22: Creating a DNS record in Designate"
lang: en
permalink: /optimist/guided_tour/step22
nav_order: 1220
parent: Guided Tour
---

Step 22: Creating a DNS record in Designate
===========================================

Start
-------

The Openstack Optimist platform includes a technology called DNS-as-a-Service (DNSaaS), also known as Designate.
DNSaaS includes a REST API for domain and records management, is multi-tenant and integrates the OpenStack Identity Service (Keystone) for authentication.

In this step, we will create a fictitious zone (domain) with MX and A records and deposit the appropriate IP/CNAME.

-----

In order to start, we first read the access data as in ["Step 4: Our way to the console"](/optimist/guided_tour/step04/) and ensure that the `python-designateclient` is installed (pip install python-openstackclient python-designateclient)
Then we serve the Openstack client and create a zone for our project first.

```bash
$ openstack zone create --email webmaster@foobar.cloud foobar.cloud.
+----------------+--------------------------------------+
| Field          | Value                                |
+----------------+--------------------------------------+
| action         | CREATE                               |
| attributes     |                                      |
| created_at     | 2018-08-15T06:45:24.000000           |
| description    | None                                 |
| email          | webmaster@foobar.cloud               |
| id             | 036ae6e6-6318-47e1-920f-be518d845fb5 |
| masters        |                                      |
| name           | foobar.cloud.                        |
| pool_id        | bb031d0d-b8ca-455a-8963-50ec70fe57cf |
| project_id     | 2b62bc8ff48445f394d0318dbd058967     |
| serial         | 1534315524                           |
| status         | PENDING                              |
| transferred_at | None                                 |
| ttl            | 3600                                 |
| type           | PRIMARY                              |
| updated_at     | None                                 |
| version        | 1                                    |
+----------------+--------------------------------------+
```

Note the final "." at the zone/domain to be created.
The result so far:

```bash
$ openstack zone list
+--------------------------------------+-----------------------+---------+------------+--------+--------+
| id                                   | name                  | type    |     serial | status | action |
+--------------------------------------+-----------------------+---------+------------+--------+--------+
| 036ae6e6-6318-47e1-920f-be518d845fb5 | foobar.cloud.         | PRIMARY | 1534315524 | ACTIVE | NONE   |
+--------------------------------------+-----------------------+---------+------------+--------+--------+

$ openstack zone show foobar.cloud.
+----------------+--------------------------------------+
| Field          | Value                                |
+----------------+--------------------------------------+
| action         | NONE                                 |
| attributes     |                                      |
| created_at     | 2018-08-15T06:45:24.000000           |
| description    | None                                 |
| email          | webmaster@foobar.cloud               |
| id             | 036ae6e6-6318-47e1-920f-be518d845fb5 |
| masters        |                                      |
| name           | foobar.cloud.                        |
| pool_id        | bb031d0d-b8ca-455a-8963-50ec70fe57cf |
| project_id     | 2b62bc8ff48445f394d0318dbd058967     |
| serial         | 1534315524                           |
| status         | ACTIVE                               |
| transferred_at | None                                 |
| ttl            | 3600                                 |
| type           | PRIMARY                              |
| updated_at     | 2018-08-15T06:45:30.000000           |
| version        | 2                                    |
+----------------+--------------------------------------+
```

Now the domain "foobar.cloud" is registered for our project and ready to use (status: ACTIVE).
In the next step, we want to create MX records (records for mail servers in this zone) for this domain.

But first let's see what content (recordsets) our new zone already owns.

```bash
$ openstack recordset list foobar.cloud.
+--------------------------------------+---------------+------+--------------------------------------------------------------------------------+--------+--------+
| id                                   | name          | type | records                                                                        | status | action |
+--------------------------------------+---------------+------+--------------------------------------------------------------------------------+--------+--------+
| 4b666317-6170-4d71-8962-94f1cbe94dda | foobar.cloud. | NS   | dns1.ddns.innovo.cloud.                                                        | ACTIVE | NONE   |
|                                      |               |      | dns2.ddns.innovo.cloud.                                                        |        |        |
| 7915c9c1-de30-4144-b82b-36295687b0fe | foobar.cloud. | SOA  | dns2.ddns.innovo.cloud. webmaster.foobar.cloud. 1534315524 3507 600 86400 3600 | ACTIVE | NONE   |
+--------------------------------------+---------------+------+--------------------------------------------------------------------------------+--------+--------+
```

Here we see an "empty shell" of a domain with automatically create NS and SOA entries that are ready for immediate query.
```bash
$ dig +short @dns1.ddns.innovo.cloud foobar.cloud NS
dns1.ddns.innovo.cloud.
dns2.ddns.innovo.cloud.

$ dig +short @dns1.ddns.innovo.cloud foobar.cloud SOA
dns2.ddns.innovo.cloud. webmaster.foobar.cloud. 1534315524 3507 600 86400 3600
```

Creating an MX record:
Now we can add, modify or delete records within this zone (openstack recordset --help).
For MX records we also set up the typical mail server priorities (10,20), where the lower value is always selected first
and the second entry serves as backup.

```bash
$ openstack recordset create --record '10 mx1.foobar.cloud.' --record '20 mx2.foobar.cloud.' --type MX foobar.cloud. foobar.cloud.
+-------------+--------------------------------------+
| Field       | Value                                |
+-------------+--------------------------------------+
| action      | CREATE                               |
| created_at  | 2018-08-15T07:15:32.000000           |
| description | None                                 |
| id          | 4197e380-dc61-4e1d-bf92-0dcf5344eafa |
| name        | foobar.cloud.                        |
| project_id  | 2b62bc8ff48445f394d0318dbd058967     |
| records     | 10 mx1.foobar.cloud.                 |
|             | 20 mx2.foobar.cloud.                 |
| status      | PENDING                              |
| ttl         | None                                 |
| type        | MX                                   |
| updated_at  | None                                 |
| version     | 1                                    |
| zone_id     | 036ae6e6-6318-47e1-920f-be518d845fb5 |
| zone_name   | foobar.cloud.                        |
+-------------+--------------------------------------+

$ openstack recordset list foobar.cloud.
+--------------------------------------+---------------+------+--------------------------------------------------------------------------------+---------+--------+
| id                                   | name          | type | records                                                                        | status  | action |
+--------------------------------------+---------------+------+--------------------------------------------------------------------------------+---------+--------+
| 4b666317-6170-4d71-8962-94f1cbe94dda | foobar.cloud. | NS   | dns1.ddns.innovo.cloud.                                                        | ACTIVE  | NONE   |
|                                      |               |      | dns2.ddns.innovo.cloud.                                                        |         |        |
| 7915c9c1-de30-4144-b82b-36295687b0fe | foobar.cloud. | SOA  | dns2.ddns.innovo.cloud. webmaster.foobar.cloud. 1534317332 3507 600 86400 3600 | PENDING | UPDATE |
| 4197e380-dc61-4e1d-bf92-0dcf5344eafa | foobar.cloud. | MX   | 20 mx2.foobar.cloud.                                                           | PENDING | CREATE |
|                                      |               |      | 10 mx1.foobar.cloud.                                                           |         |        |
+--------------------------------------+---------------+------+--------------------------------------------------------------------------------+---------+--------+

```

```bash
$ openstack recordset create --type A --record 1.2.3.4 foobar.cloud. www
+-------------+--------------------------------------+
| Field       | Value                                |
+-------------+--------------------------------------+
| action      | CREATE                               |
| created_at  | 2018-08-15T07:28:15.000000           |
| description | None                                 |
| id          | d932688f-21d5-44b1-aa27-030c342788e7 |
| name        | www.foobar.cloud.                    |
| project_id  | 2b62bc8ff48445f394d0318dbd058967     |
| records     | 1.2.3.4                              |
| status      | PENDING                              |
| ttl         | None                                 |
| type        | A                                    |
| updated_at  | None                                 |
| version     | 1                                    |
| zone_id     | 036ae6e6-6318-47e1-920f-be518d845fb5 |
| zone_name   | foobar.cloud.                        |
+-------------+--------------------------------------+
```

Result:
```bash
$ openstack recordset list foobar.cloud.
+--------------------------------------+-------------------+------+--------------------------------------------------------------------------------+--------+--------+
| id                                   | name              | type | records                                                                        | status | action |
+--------------------------------------+-------------------+------+--------------------------------------------------------------------------------+--------+--------+
| 4b666317-6170-4d71-8962-94f1cbe94dda | foobar.cloud.     | NS   | dns1.ddns.innovo.cloud.                                                        | ACTIVE | NONE   |
|                                      |                   |      | dns2.ddns.innovo.cloud.                                                        |        |        |
| 7915c9c1-de30-4144-b82b-36295687b0fe | foobar.cloud.     | SOA  | dns2.ddns.innovo.cloud. webmaster.foobar.cloud. 1534318095 3507 600 86400 3600 | ACTIVE | NONE   |
| 4197e380-dc61-4e1d-bf92-0dcf5344eafa | foobar.cloud.     | MX   | 20 mx2.foobar.cloud.                                                           | ACTIVE | NONE   |
|                                      |                   |      | 10 mx1.foobar.cloud.                                                           |        |        |
| d932688f-21d5-44b1-aa27-030c342788e7 | www.foobar.cloud. | A    | 1.2.3.4                                                                        | ACTIVE | NONE   |
+--------------------------------------+-------------------+------+--------------------------------------------------------------------------------+--------+--------+
```

If the recordsets are active we can use the designated DNS servers
* dns1.ddns.innovo.cloud
* dns2.ddns.innovo.cloud

Query for these records:

```bash
$ dig +short @dns1.ddns.innovo.cloud foobar.cloud NS
dns1.ddns.innovo.cloud.
dns2.ddns.innovo.cloud.

$ dig +short @dns1.ddns.innovo.cloud foobar.cloud MX
10 mx1.foobar.cloud.
20 mx2.foobar.cloud.

$ dig +short @dns1.ddns.innovo.cloud foobar.cloud www.foobar.cloud
1.2.3.4
```


>        ATTENTION! At this time, this domain (foobar.cloud) is not yet resolvable worldwide.

For this construct to be used worldwide, each domain managed by the Designate must have the delegation to the name servers `dns1.ddns.innovo.cloud` and `dns2.ddns.innovo.cloud` established by the respective registrar.

>        Details about our authoritative DNS servers:
>          * dns1.ddns.innovo.cloud: '185.116.244.45' / '2a00:c320:0:1::d'
>          * dns2.ddns.innovo.cloud: '185.116.244.46' / '2a00:c320:0:1::e'

In order to complete the mail records, it is still possible to deposit corresponding A-records for the mail servers
```bash
$ openstack recordset create --type A --record 2.3.4.5 foobar.cloud. mx1
+-------------+--------------------------------------+
| Field       | Value                                |
+-------------+--------------------------------------+
| action      | CREATE                               |
| created_at  | 2018-08-15T07:42:31.000000           |
| description | None                                 |
| id          | 630d5103-7c02-4a58-83a5-97f802cf141c |
| name        | mx1.foobar.cloud.                    |
| project_id  | 2b62bc8ff48445f394d0318dbd058967     |
| records     | 2.3.4.5                              |
| status      | PENDING                              |
| ttl         | None                                 |
| type        | A                                    |
| updated_at  | None                                 |
| version     | 1                                    |
| zone_id     | 036ae6e6-6318-47e1-920f-be518d845fb5 |
| zone_name   | foobar.cloud.                        |
+-------------+--------------------------------------+
and
$ openstack recordset create --type A --record 3.4.5.6 foobar.cloud. mx2
+-------------+--------------------------------------+
| Field       | Value                                |
+-------------+--------------------------------------+
| action      | CREATE                               |
| created_at  | 2018-08-15T07:42:56.000000           |
| description | None                                 |
| id          | 2b47dbe4-70b9-4edb-ac2f-25cb8398bacb |
| name        | mx2.foobar.cloud.                    |
| project_id  | 2b62bc8ff48445f394d0318dbd058967     |
| records     | 3.4.5.6                              |
| status      | PENDING                              |
| ttl         | None                                 |
| type        | A                                    |
| updated_at  | None                                 |
| version     | 1                                    |
| zone_id     | 036ae6e6-6318-47e1-920f-be518d845fb5 |
| zone_name   | foobar.cloud.                        |
+-------------+--------------------------------------+
```

The result after a few seconds:
```bash
$ openstack recordset list foobar.cloud.
+--------------------------------------+-------------------+------+--------------------------------------------------------------------------------+--------+--------+
| id                                   | name              | type | records                                                                        | status | action |
+--------------------------------------+-------------------+------+--------------------------------------------------------------------------------+--------+--------+
| 4b666317-6170-4d71-8962-94f1cbe94dda | foobar.cloud.     | NS   | dns1.ddns.innovo.cloud.                                                        | ACTIVE | NONE   |
|                                      |                   |      | dns2.ddns.innovo.cloud.                                                        |        |        |
| 7915c9c1-de30-4144-b82b-36295687b0fe | foobar.cloud.     | SOA  | dns2.ddns.innovo.cloud. webmaster.foobar.cloud. 1534318976 3507 600 86400 3600 | ACTIVE | NONE   |
| 4197e380-dc61-4e1d-bf92-0dcf5344eafa | foobar.cloud.     | MX   | 20 mx2.foobar.cloud.                                                           | ACTIVE | NONE   |
|                                      |                   |      | 10 mx1.foobar.cloud.                                                           |        |        |
| d932688f-21d5-44b1-aa27-030c342788e7 | www.foobar.cloud. | A    | 1.2.3.4                                                                        | ACTIVE | NONE   |
| 630d5103-7c02-4a58-83a5-97f802cf141c | mx1.foobar.cloud. | A    | 2.3.4.5                                                                        | ACTIVE | NONE   |
| 2b47dbe4-70b9-4edb-ac2f-25cb8398bacb | mx2.foobar.cloud. | A    | 3.4.5.6                                                                        | ACTIVE | NONE   |
+--------------------------------------+-------------------+------+--------------------------------------------------------------------------------+--------+--------+
```

Conclusion
----------

In this step, we learned how to create a zone, configure a record set, and query it.
