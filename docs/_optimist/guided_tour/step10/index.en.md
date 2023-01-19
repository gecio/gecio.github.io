---
title: "10: Get access to the Internet; Create a network"
lang: "en"
permalink: /optimist/guided_tour/step10/
nav_order: 1100
parent: Guided Tour
---

# Step 10: Get access to the Internet: Create a network

## Start

So far, you have created a VM and a security group.

The next step is to create a network.

## The network

You will start with the network. As with previous commands, you have
additional options you can list with `--help`.

To create your network use the following command:

```bash
$ openstack network create BeispielNetzwerk
+---------------------------+--------------------------------------+
| Field                     | Value                                |
+---------------------------+--------------------------------------+
| admin_state_up            | UP                                   |
| availability_zone_hints   |                                      |
| availability_zones        |                                      |
| created_at                | 2017-12-08T12:06:38Z                 |
| description               |                                      |
| dns_domain                | None                                 |
| id                        | ff6d8654-66d6-4881-9528-2686bddcb6dc |
| ipv4_address_scope        | None                                 |
| ipv6_address_scope        | None                                 |
| is_default                | False                                |
| mtu                       | 1500                                 |
| name                      | BeispielNetzwerk                     |
| port_security_enabled     | True                                 |
| project_id                | b15cde70d85749689e08106f973bb002     |
| provider:network_type     | None                                 |
| provider:physical_network | None                                 |
| provider:segmentation_id  | None                                 |
| qos_policy_id             | None                                 |
| revision_number           | 2                                    |
| router:external           | Internal                             |
| segments                  | None                                 |
| shared                    | False                                |
| status                    | ACTIVE                               |
| subnets                   |                                      |
| updated_at                | 2017-12-08T12:06:38Z                 |
+---------------------------+--------------------------------------+
```

## Subnet

Now that you have a network, you need to create a subnet for it.

The subnet creation command also has a few options. In our example, we use:

- `--network`: Specifies the network where the subnet will be created
- `--subnet-range`: [The CIDR](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing) range
    for the subnet. In our example it is `192.168.2.0/24`

To create a subnet in your existing network, run the following command:

```bash
$ openstack subnet create BeispielSubnet --network BeispielNetzwerk --subnet-range 192.168.2.0/24
+-------------------------+--------------------------------------+
| Field                   | Value                                |
+-------------------------+--------------------------------------+
| allocation_pools        | 192.168.2.2-192.0.2.254                |
| cidr                    | 192.168.2.0/24                         |
| created_at              | 2017-12-08T12:09:07Z                 |
| description             |                                      |
| dns_nameservers         |                                      |
| enable_dhcp             | True                                 |
| gateway_ip              | 192.168.2.1                            |
| host_routes             |                                      |
| id                      | 984b24bf-db60-46a9-83c3-d68f6f1062e4 |
| ip_version              | 4                                    |
| ipv6_address_mode       | None                                 |
| ipv6_ra_mode            | None                                 |
| name                    | BeispielSubnet                       |
| network_id              | ff6d8654-66d6-4881-9528-2686bddcb6dc |
| project_id              | b15cde70d85749689e08106f973bb002     |
| revision_number         | 0                                    |
| segment_id              | None                                 |
| service_types           |                                      |
| subnetpool_id           | None                                 |
| updated_at              | 2017-12-08T12:09:07Z                 |
| use_default_subnet_pool | None                                 |
+-------------------------+--------------------------------------+
```

## Router

For your virtual network to be able to reach the internet, you need to
create a router:

```bash
$ openstack router create BeispielRouter
+-------------------------+--------------------------------------+
| Field                   | Value                                |
+-------------------------+--------------------------------------+
| admin_state_up          | UP                                   |
| availability_zone_hints |                                      |
| availability_zones      |                                      |
| created_at              | 2017-12-08T12:09:49Z                 |
| description             |                                      |
| distributed             | False                                |
| external_gateway_info   | None                                 |
| flavor_id               | None                                 |
| ha                      | False                                |
| id                      | bfb91c7f-acca-450a-aae0-c519ab563d38 |
| name                    | BeispielRouter                       |
| project_id              | b15cde70d85749689e08106f973bb002     |
| revision_number         | None                                 |
| routes                  |                                      |
| status                  | ACTIVE                               |
| updated_at              | 2017-12-08T12:09:49Z                 |
+-------------------------+--------------------------------------+
```

To be able to access the internet, you need to define the external gateway:

```bash
openstack router set BeispielRouter --external-gateway provider
```

Next, add the subnet to the router:

```bash
openstack router add subnet BeispielRouter BeispielSubnet
```

## Port

Now that you have your subnet and router, you need to create a port for the
network.

You can link the port using the `--network` option:

```bash
$ openstack port create BeispielPort --network BeispielNetzwerk
+-----------------------+----------------------------------------------------------------------------+
| Field                 | Value                                                                      |
+-----------------------+----------------------------------------------------------------------------+
| admin_state_up        | UP                                                                         |
| allowed_address_pairs |                                                                            |
| binding_host_id       | None                                                                       |
| binding_profile       | None                                                                       |
| binding_vif_details   | None                                                                       |
| binding_vif_type      | None                                                                       |
| binding_vnic_type     | normal                                                                     |
| created_at            | 2017-12-08T12:12:13Z                                                       |
| description           |                                                                            |
| device_id             |                                                                            |
| device_owner          |                                                                            |
| dns_assignment        | None                                                                       |
| dns_name              | None                                                                       |
| extra_dhcp_opts       |                                                                            |
| fixed_ips             | ip_address='192.168.2.8', subnet_id='984b24bf-db60-46a9-83c3-d68f6f1062e4' |
| id                    | 31777c0a-a952-43ca-bb7f-11ad33926dae                                       |
| ip_address            | None                                                                       |
| mac_address           | fa:16:3e:09:88:c8                                                          |
| name                  | BeispielPort                                                               |
| network_id            | ff6d8654-66d6-4881-9528-2686bddcb6dc                                       |
| option_name           | None                                                                       |
| option_value          | None                                                                       |
| port_security_enabled | True                                                                       |
| project_id            | b15cde70d85749689e08106f973bb002                                           |
| qos_policy_id         | None                                                                       |
| revision_number       | 3                                                                          |
| security_group_ids    | 3d3e3074-3087-4965-9a64-34a6d56193b9                                       |
| status                | DOWN                                                                       |
| subnet_id             | None                                                                       |
| updated_at            | 2017-12-08T12:12:13Z                                                       |
+-----------------------+----------------------------------------------------------------------------+
```

## Conclusion

After the router, subnet, and port have been created and linked together,
the setup of the sample network is complete. In the next step, you will add IPv6 access.
