---
title: "Migration: Loadbalancer"
lang: en
permalink: /optimist/migration_loadbalancer
nav_order: 7000
last_modified_date: 2021-04-30
---

# Migration: Neutron LBaas (Deprecated) to Octavia Loadbalancer

With the Train release of OpenStack (which we expect to complete in Q4/2021), support for the deprecated LBaaS Neutron Loadbalancers will be removed. In anticipation of this update, we also therefore consider Neutron Loadbalancers deprecated on our own platform. Supported loadbalancers going forward will therefore solely be Octavia Loadbalancers. It is therefore important that customers migrate from our legacy Neutron Loadbalancers to Octavia Loadbalancers at their earliest available opportunity.

In order to streamline the migration process for our customers, we have prepared the following guide to assist you with migrating from Neutron to Octavia Loadbalancers.

The steps needed to successfully migrate a Neutron LB: 
- Display a list of Neutron Loadbalancers within a project
- Display the listeners / pools and members of that Loadbalancer 
- Detach the floating IP from the Neutron Loadbalancer
- Attach the same floating IP to the Octavia Loadbalancer

Once all steps have been completed, the migration from Neutron to Octavia will be successfully completed. The procedure is described in detail below. All steps are completed via the command line.

## Important instructions

The migration is only necessary for load balancers that were **not** created by IMKE.

In addition, the service operated behind it is **offline** for the period between hanging and reattaching the floating IP. To avoid this, you can alternatively assign a new floating IP to the new load balancer and reconfigure your name servers accordingly. Both load balancers are then active for a certain period of time.

# Identify Neutron Loadbalancers

In your project, run the following command to see a list of all Neutron Loadbalancers within your project:

```bash
$ neutron lbaas-loadbalancer-list
neutron CLI is deprecated and will be removed in the future. Use openstack CLI instead.
+--------------------------------------+--------------------+-------------+---------------------+----------+
| id                                   | name               | vip_address | provisioning_status | provider |
+--------------------------------------+--------------------+-------------+---------------------+----------+
| 2550d3ea-fa73-4923-a6df-1b0c828a8ff1 | Load Balancer Test | 10.0.0.19   | ACTIVE              | haproxy  |
| f731f07d-4612-4f94-81a3-ccb65a1ae1f0 | Apr27NeutronLB     | 192.168.5.7 | ACTIVE              | haproxy  |
+--------------------------------------+--------------------+-------------+---------------------+----------+
```

For more detail on each load balancer within the project, use the show command:

```bash
$ neutron lbaas-loadbalancer-show Apr27NeutronLB
neutron CLI is deprecated and will be removed in the future. Use openstack CLI instead.
+---------------------+--------------------------------------+
| Field               | Value                                |
+---------------------+--------------------------------------+
| admin_state_up      | True                                 |
| description         |                                      |
| id                  | f731f07d-4612-4f94-81a3-ccb65a1ae1f0 |
| listeners           |                                      |
| name                | Apr27NeutronLB                       |
| operating_status    | ONLINE                               |
| pools               |                                      |
| provider            | haproxy                              |
| provisioning_status | ACTIVE                               |
| tenant_id           | c906ac5a32b34b54a0398d28ad448301     |
| vip_address         | 192.168.5.7                          |
| vip_port_id         | 2cc1b3f9-0457-410b-93dd-a96338de3b12 |
| vip_subnet_id       | c2bff369-0e62-44b3-89af-59ba6950094f |
+---------------------+--------------------------------------+
```

To view all resources within the load balancer, run the following commands:
```bash
$ neutron lbaas-healthmonitor-list
$ neutron lbaas-member-list
$ neutron lbaas-pool-list
$ neutron lbaas-listener-list
```

# Identify the floating IP associated with a load balancer

When we used the `lbaas-loadbalancer-show` command, we saw a row called `vip_port_id`, we need to cross-reference this row with our project's floating IP list to confirm what the associated port is. As can be seen below, the port `2cc1b3f9-0457-410b-93dd-a96338de3b12` from our Neutron Loadbalancer has an associated floating IP address of `45.94.110.184`:

```bash
$ openstack floating ip list
+--------------------------------------+------------------+---------------------+--------------------------------------+
| id                                   | fixed_ip_address | floating_ip_address | port_id                              |
+--------------------------------------+------------------+---------------------+--------------------------------------+
| 1856d701-a0c6-4d6c-a1c7-1f35072d3a34 |                  | 45.94.108.155       |                                      |
| 419208fc-e6e0-450c-a503-516b1337d44e | 192.168.2.6      | 185.116.245.202     | ab0df913-52fb-42e6-be59-3f9c2fa0102f |
| 90ad56e5-a3c8-4f9c-a12f-f534f11d7ed9 | 10.0.0.12        | 185.116.247.205     | 8a19cf54-e350-4f93-b572-db23dc650b30 |
| c6788cd1-c03f-496e-9962-678c0c8b9216 | 10.0.0.19        | 185.116.245.226     | 35f4fb69-e86d-4859-93f3-aa0c91fab123 |
| cd17e8b1-3d4b-417a-9a80-a21a9879a8dc | 192.168.5.7      | 45.94.110.184       | 2cc1b3f9-0457-410b-93dd-a96338de3b12 |
| e9b6e51a-0946-43f0-95fb-4679adc2638d | 10.0.0.13        | 185.116.245.13      | 9b8c1d2a-843f-44c5-a109-07f2a7e7be5d |
+--------------------------------------+------------------+---------------------+--------------------------------------+
```

# Create an Octavia Loadbalancer

Once we have identified the Neutron Loadbalancers we need to migrate, the next step is to mirror the setup in Octavia. We can create a new Octavia Loadbalancer (Refer to [Step 24](/optimist/guided_tour/step24) of our guide for instructions on setting up an Octavia Loadbalancer).

Once the new load balancer has been created, please ensure that you take the opportunity to check what the VIP port ID of the loadbalancer is with the following command, we will need it in the final step:

```bash
$ openstack loadbalancer show Apr27OctLB -f value -c vip_port_id
bde93be2-c0c1-4e23-af02-e0c4300ff8c0
```

# Detach Floating IP from Neutron and reattach it to the Octavia Loadbalancer

Once the Octavia load balancer has been created, we are ready for the final step of the migration. In this step we will detach the floating IP from the Neutron port and reattach the Floating IP to the new port of the Octavia load balancer.

Firstly detach the Floating IP from the Neutron LB. The UUID of floating IP 45.94.110.184 in this case is `cd17e8b1-3d4b-417a-9a80-a21a9879a8dc`

```bash
$ openstack floating ip unset --port cd17e8b1-3d4b-417a-9a80-a21a9879a8dc
neutron CLI is deprecated and will be removed in the future. Use openstack CLI instead.
Disassociated floating IP cd17e8b1-3d4b-417a-9a80-a21a9879a8dc
```

Immediately after this, attach the same Floating IP we just removed from the Neutron LB port, to the Octavia LB port:

LB VIP Port: `bde93be2-c0c1-4e23-af02-e0c4300ff8c0`
Floating IP: `cd17e8b1-3d4b-417a-9a80-a21a9879a8dc`

```bash
$ openstack floating ip set cd17e8b1-3d4b-417a-9a80-a21a9879a8dc --port bde93be2-c0c1-4e23-af02-e0c4300ff8c0
```
