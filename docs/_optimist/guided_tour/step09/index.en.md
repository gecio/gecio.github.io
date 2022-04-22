---
title: "09: A Security Group"
lang: en
permalink: /optimist/guided_tour/step09/
nav_order: 1090
parent: Guided Tour
---

Step 9: A Security Group
========================

Start
-----

For security reasons, any incoming traffic to a VM is denied.

To be able to access a VM, we need to assign it at least one security
group.

While it is possible to add all access rules into a single security
group, it's advisable to use a separate security group per service.

How-to
------

The base command for creating security groups is `openstack security group create`, for example:

```bash
openstack security group create allow-ssh-from-anywhere --description Beispiel
+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+
| Field           | Value                                                                                                                                               |
+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+
| created_at      | 2017-12-08T12:01:42Z                                                                                                                                |
| description     | Beispiel                                                                                                                                            |
| id              | 1cab4a62-0fda-40d9-bac8-fd73275b472d                                                                                                                |
| name            | allow-ssh-from-anywhere                                                                                                                             |
| project_id      | b15cde70d85749689e08106f973bb002                                                                                                                    |
| revision_number | 2                                                                                                                                                   |
| rules           | created_at='2017-12-08T12:01:42Z', direction='egress', ethertype='IPv6', id='5a852e4b-1d79-4fe9-b359-64ca54c98501',                                 |
|                 | updated_at='2017-12-08T12:01:42Z'                                                                                                                   |
|                 | created_at='2017-12-08T12:01:42Z', direction='egress', ethertype='IPv4', id='fa90a1ee-d3b9-40d4-9bb5-89fdd5005c02',                                 |
|                 | updated_at='2017-12-08T12:01:42Z'                                                                                                                   |
| updated_at      | 2017-12-08T12:01:42Z                                                                                                                                |
+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+
```

Now that we've created an empty security group, we're going to add some
rules.

Some commonly used options are:

- `--protocol`: The protocol that this rule matches. (Example
    arguments: tcp, udp, icmp)
- `--dst-port`: Destination port range to give access to. (Example
    arguments: 22:22 for port 22 100:200 for ports 100 through 200).
- `--remote-ip`: Remote IP to allow access from. (Example arguments:
    0.0.0.0/0 for all IP addresses, 1.2.3.0/24 for all IP addresses
    starting with 1.2.3.).
- `--ingress` or `--egress:` ingress is incoming traffic and egress is
    outgoing traffic (No arguments possible)

We're going to use these options to create a rule for our new seurity group to
actually allow SSH from anywhere:

```bash
$ openstack security group rule create allow-ssh-from-anywhere --protocol tcp --dst-port 22:22 --remote-ip 0.0.0.0/0
+-------------------+--------------------------------------+
| Field             | Value                                |
+-------------------+--------------------------------------+
| created_at        | 2017-12-08T12:02:15Z                 |
| description       |                                      |
| direction         | ingress                              |
| ether_type        | IPv4                                 |
| id                | 694a0573-b4c3-423c-847d-550f79e83f2b |
| name              | None                                 |
| port_range_max    | 22                                   |
| port_range_min    | 22                                   |
| project_id        | b15cde70d85749689e08106f973bb002     |
| protocol          | tcp                                  |
| remote_group_id   | None                                 |
| remote_ip_prefix  | 0.0.0.0/0                            |
| revision_number   | 0                                    |
| security_group_id | 1cab4a62-0fda-40d9-bac8-fd73275b472d |
| updated_at        | 2017-12-08T12:02:15Z                 |
+-------------------+--------------------------------------+
```

Next, we verify if our security group was created correctly:

```bash
$ openstack security group show allow-ssh-from-anywhere
+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+
| Field           | Value                                                                                                                                               |
+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+
| created_at      | 2017-12-08T12:01:42Z                                                                                                                                |
| description     | Beispiel                                                                                                                                            |
| id              | 1cab4a62-0fda-40d9-bac8-fd73275b472d                                                                                                                |
| name            | allow-ssh-from-anywhere                                                                                                                             |
| project_id      | b15cde70d85749689e08106f973bb002                                                                                                                    |
| revision_number | 3                                                                                                                                                   |
| rules           | created_at='2017-12-08T12:01:42Z', direction='egress', ethertype='IPv6', id='5a852e4b-1d79-4fe9-b359-64ca54c98501',                                 |
|                 | updated_at='2017-12-08T12:01:42Z'                                                                                                                   |
|                 | created_at='2017-12-08T12:02:15Z', direction='ingress', ethertype='IPv4', id='694a0573-b4c3-423c-847d-550f79e83f2b', port_range_max='22',           |
|                 | port_range_min='22', protocol='tcp', remote_ip_prefix='0.0.0.0/0', updated_at='2017-12-08T12:02:15Z'                                                |
|                 | created_at='2017-12-08T12:01:42Z', direction='egress', ethertype='IPv4', id='fa90a1ee-d3b9-40d4-9bb5-89fdd5005c02',                                 |
|                 | updated_at='2017-12-08T12:01:42Z'                                                                                                                   |
| updated_at      | 2017-12-08T12:02:15Z                                                                                                                                |
+-----------------+-----------------------------------------------------------------------------------------------------------------------------------------------------+
```

Conclusion
----------

After successfully creating the security group, the next step is to add a network.

We will learn how to do this in Step 10: Getting access to the Internet: Creating a network
