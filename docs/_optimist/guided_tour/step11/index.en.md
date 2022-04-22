---
title: "11: Prepare access to the internet; Add IPv6 to our network"
lang: en
permalink: /optimist/guided_tour/step11/
nav_order: 1110
parent: Guided Tour
---

# Step 11: Prepare access to the internet: Add IPv6 to our network

## Start

Now that we have our working network, we will enable IPv6 to our setup.

We don't have to create a new router, as we'll be using our existing
one.

The cloud images we supply have a predefined primary network interface with
with [DHCP](https://en.wikipedia.org/wiki/Dynamic_Host_Configuration_Protocol)
enabled and once we finished this step, IPv6 will work as well.

## Subnet

We already have an IPv6 pool defined, we will use this to create a new
subnet.

Let's list all existing pools:

```bash
$ openstack subnet pool list
+--------------------------------------+---------------+---------------------+
| ID                                   | Name          | Prefixes            |
+--------------------------------------+---------------+---------------------+
| f541f3b6-af22-435a-9cbb-b233d12e74f4 | customer-ipv6 | 2a00:c320:1000::/48 |
+--------------------------------------+---------------+---------------------+
```

We can now use the pool to generate a subnet, it will automatically
force us to use a prefix length of 64 bits.

You can use the subnet in the creation process, or you can accept the
default from OpenStack.

Let's create our subnet now:

```bash
$ openstack subnet create --network BeispielNetzwerk --ip-version 6 --use-default-subnet-pool --ipv6-address-mode dhcpv6-stateful --ipv6-ra-mode dhcpv6-stateful BeispielSubnetIPv6
+-------------------------+----------------------------------------------------------+
| Field                   | Value                                                    |
+-------------------------+----------------------------------------------------------+
| allocation_pools        | 2a00:c320:1000:2::2-2a00:c320:1000:2:ffff:ffff:ffff:ffff |
| cidr                    | 2a00:c320:1000:2::/64                                    |
| created_at              | 2017-12-08T12:41:42Z                                     |
| description             |                                                          |
| dns_nameservers         |                                                          |
| enable_dhcp             | True                                                     |
| gateway_ip              | 2a00:c320:1000:2::1                                      |
| host_routes             |                                                          |
| id                      | 0046c29b-a9b0-47c3-b5dd-704aa801704d                     |
| ip_version              | 6                                                        |
| ipv6_address_mode       | dhcpv6-stateful                                          |
| ipv6_ra_mode            | dhcpv6-stateful                                          |
| name                    | BeispielSubnetIPv6                                       |
| network_id              | ff6d8654-66d6-4881-9528-2686bddcb6dc                     |
| project_id              | b15cde70d85749689e08106f973bb002                         |
| revision_number         | 0                                                        |
| segment_id              | None                                                     |
| service_types           |                                                          |
| subnetpool_id           | f541f3b6-af22-435a-9cbb-b233d12e74f4                     |
| updated_at              | 2017-12-08T12:41:42Z                                     |
| use_default_subnet_pool | True                                                     |
+-------------------------+----------------------------------------------------------+
```

## Router

Now that the subnet has been created, we'll add it to the router.

We'll do so by executing this command:

```bash
openstack router add subnet BeispielRouter BeispielSubnetIPv6
```

## Security Group

The security group rules that we've created in step 9 were IPv4 rules, so now
we'll have to add two more rules for IPv6.

First, we'll allow SSH access via IPv6 (::/0 is the equivalent of 0.0.0.0/0
but for IPv6):

```bash
$ openstack security group rule create --remote-ip "::/0" --protocol tcp --dst-port 22:22 --ethertype IPv6 --ingress allow-ssh-from-anywhere
+-------------------+--------------------------------------+
| Field             | Value                                |
+-------------------+--------------------------------------+
| created_at        | 2017-12-08T12:44:04Z                 |
| description       |                                      |
| direction         | ingress                              |
| ether_type        | IPv6                                 |
| id                | 7d871e85-05fa-4620-b558-c6fc64076cde |
| name              | None                                 |
| port_range_max    | 22                                   |
| port_range_min    | 22                                   |
| project_id        | b15cde70d85749689e08106f973bb002     |
| protocol          | tcp                                  |
| remote_group_id   | None                                 |
| remote_ip_prefix  | ::/0                                 |
| revision_number   | 0                                    |
| security_group_id | 1cab4a62-0fda-40d9-bac8-fd73275b472d |
| updated_at        | 2017-12-08T12:44:04Z                 |
+-------------------+--------------------------------------+
```

And, just for completion's sake, we'll allow ICMP access so that we
can ping our VM via IPv6:

```bash
$ openstack security group rule create --remote-ip "::/0" --protocol ipv6-icmp --ingress allow-ssh-from-anywhere
+-------------------+--------------------------------------+
| Field             | Value                                |
+-------------------+--------------------------------------+
| created_at        | 2017-12-08T12:44:44Z                 |
| description       |                                      |
| direction         | ingress                              |
| ether_type        | IPv6                                 |
| id                | f63e4787-9965-4732-b9d2-20ce0fedc974 |
| name              | None                                 |
| port_range_max    | None                                 |
| port_range_min    | None                                 |
| project_id        | b15cde70d85749689e08106f973bb002     |
| protocol          | ipv6-icmp                            |
| remote_group_id   | None                                 |
| remote_ip_prefix  | ::/0                                 |
| revision_number   | 0                                    |
| security_group_id | 1cab4a62-0fda-40d9-bac8-fd73275b472d |
| updated_at        | 2017-12-08T12:44:44Z                 |
+-------------------+--------------------------------------+
```

## Adjustments to the operating system

Any new VM based on our images will now have both IPv4 and IPv6 configured, and
our provided heat templates will also enable IPv6.

Many standard vendor images will not have IPv6 configured, this means that
they'll only have IPv4 enabled by default.

If we want to enable IPv6 on a VM where it's not enabled, we can follow the
instructions below.

### Ubuntu 16.04

To properly enable IPv6, we'll have to create the following files with
the specified content.

- `/etc/dhcp/dhclient6.conf`

    ```text
    timeout 30;
    ```

- `/etc/cloud/cloud.cfg.d/99-disable-network-config.cfg`

    ```text
    network: {config: disabled}
    ```

- `/etc/network/interfaces.d/lo.cfg`

    ```text
    auto lo
    iface lo inet loopback
    ```

- `/etc/network/interfaces.d/ens3.cfg`

    ```text
    iface ens3 inet6 auto
        up sleep 5
        up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.ens3.leases -v ens3 || true
    ```

Now that we've created the files, we'll reenable the interface:

```bash
sudo ifdown ens3 && sudo ifup ens3
```

Once this is completed, we'll have working IPv4 and IPv6 addresses.

If we want to automate the actions above, we can add this to the *cloud-init*
part of our heat template (We'll go over cloud-init in [Step 19](/optimist/guided_tour/step19/):

```yaml
#cloud-config
write_files:
        - path: /etc/dhcp/dhclient6.conf
          content: "timeout 30;"
        - path: /etc/cloud/cloud.cfg.d/99-disable-network-config.cfg
          content: "network: {config: disabled}"
        - path: /etc/network/interfaces.d/lo.cfg
          content: |
              auto lo
              iface lo inet loopback
        - path: /etc/network/interfaces.d/ens3.cfg
          content: |
              iface ens3 inet6 auto
                  up sleep 5
                  up dhclient -1 -6 -cf /etc/dhcp/dhclient6.conf -lf /var/lib/dhcp/dhclient6.ens3.leases -v ens3 || true

runcmd:
        - [ ifdown, ens3]
        - [ ifup, ens3]
```

### CentOS 7

To properly enable IPv6, we'll have to create the following files with the
specified content.

- `/etc/sysconfig/network`

    ```text
    NETWORKING_IPV6=yes
    ```

- `/etc/sysconfig/network-scripts/ifcfg-eth0`

    ```text
    IPV6INIT=yes
    DHCPV6C=yes
    ```

Now that we've created the files, we'll reenable the interface:

```bash
sudo ifdown eth0 && sudo ifup eth0
```

Once this is completed, we'll have working IPv4 and IPv6 addresses.

If we want to automate the actions above, we can add this to the *cloud-init*
part of our heat template (We'll go over cloud-init in step 19):

```yaml
#cloud-config
write_files:
        - path: /etc/sysconfig/network
          owner: root:root
          permissions: '0644'
          content: |
              NETWORKING=yes
              NOZEROCONF=yes
              NETWORKING_IPV6=yes
        - path: /etc/sysconfig/network-scripts/ifcfg-eth0
          owner: root:root
          permissions: '0644'
          content: |
              DEVICE="eth0"
              BOOTPROTO="dhcp"
              ONBOOT="yes"
              TYPE="Ethernet"
              USERCTL="yes"
              PEERDNS="yes"
              PERSISTENT_DHCLIENT="1"
              IPV6INIT=yes
              DHCPV6C=yes
runcmd:
        - [ ifdown, eth0]
        - [ ifup, eth0]
```

## External access

**Important:** Now that we've enabled IPv6 on the VM, it's reachable from the
rest of the world on its IPv6 address on the ports that we've allowed in the
security group.

Unlike IPv4, we don't need to assign a floating IP address to be able to reach
the VM.

If we want to be able to reach the VM via IPv4, we'll have to assign a
floating IP address.

If you want to test the IPv6 reachability but don't have access to a
machine with IPv6, you can use some web based tools like
<https://www.subnetonline.com/pages/ipv6-network-tools/online-ipv6-ping.php>

## Conclusion

A connection via IPv4 was already established in the last step, access via IPv6 has now also been added.

In the next step, the instance from [Step 7](/optimist/guided_tour/step07/) will be used as a template and made accessible from outside.
