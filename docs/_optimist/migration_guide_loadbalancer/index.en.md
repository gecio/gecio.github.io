---
title: "Migration: Loadbalancer"
lang: en
permalink: /optimist/migration_loadbalancer/
nav_order: 7000
last_modified_date: 2021-04-30
---

# Migration: Neutron LBaas (Deprecated) to Octavia Loadbalancer

With the Train release of OpenStack (which we expect to complete in Q4/2021), support for the deprecated LBaaS Neutron Loadbalancers will be removed. In anticipation of this update, we also therefore consider Neutron Loadbalancers deprecated on our own platform. Supported loadbalancers going forward will therefore solely be Octavia Loadbalancers. It is therefore important that customers migrate from our legacy Neutron Loadbalancers to Octavia Loadbalancers at their earliest available opportunity.

In order to streamline the migration process for our customers, we have prepared the following guide to assist you with migrating from Neutron to Octavia Loadbalancers.

The steps needed to successfully migrate a Neutron LB: 
- Display a list of Neutron Loadbalancers within a project
- Display the listeners / pools and members of that Loadbalancer
- Create a new Octavia Loadbalancer with the same settings
- Detach the floating IP from the Neutron Loadbalancer
- Attach the same floating IP to the Octavia Loadbalancer

Once all steps have been completed, the migration from Neutron to Octavia will be successfully completed. The procedure is described in detail below. 

All steps are completed via the command line.

## Important information

The migration is only necessary for load balancers that were **not** created by IMKE.

In addition, the service operated behind it is **offline** for the period between hanging and reattaching the floating IP. To avoid this, you can alternatively assign a new floating IP to the new load balancer and reconfigure your name servers accordingly. Both load balancers are then active for a certain period of time.

## Identify Neutron Loadbalancers

For the first steps the Neutron client is necessary. You can install this with `pip install python-neutronclient`.

Once installed, run the following command to see a list of all Neutron Loadbalancers within your project:

```bash
$ neutron lbaas-loadbalancer-list
neutron CLI is deprecated and will be removed in the future. Use openstack CLI instead.
+--------------------------------------+--------------------+-------------+---------------------+----------+
| id                                   | name               | vip_address | provisioning_status | provider |
+--------------------------------------+--------------------+-------------+---------------------+----------+
| 2550d3ea-fa73-4923-a6df-1b0c828a8ff1 | Load Balancer Test | 10.0.0.19   | ACTIVE              | haproxy  |
| f731f07d-4612-4f94-81a3-ccb65a1ae1f0 | ExampleNeutronLB   | 192.168.5.7 | ACTIVE              | haproxy  |
+--------------------------------------+--------------------+-------------+---------------------+----------+
```

## Output loadbalancer details

For more detail on each load balancer within the project, use the show command:

```bash
$ neutron lbaas-loadbalancer-show ExampleNeutronLB
neutron CLI is deprecated and will be removed in the future. Use openstack CLI instead.
+---------------------+--------------------------------------+
| Field               | Value                                |
+---------------------+--------------------------------------+
| admin_state_up      | True                                 |
| description         |                                      |
| id                  | f731f07d-4612-4f94-81a3-ccb65a1ae1f0 |
| listeners           |                                      |
| name                | ExampleNeutronLB                     |
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

## Identify the floating IP associated with a load balancer

When we used the `lbaas-loadbalancer-show` command, we saw a row called `vip_port_id`, we need to cross-reference this row with our project's floating IP list to confirm what the associated port is. As can be seen below, the port `2cc1b3f9-0457-410b-93dd-a96338de3b12` from our Neutron Loadbalancer has an associated floating IP address of `45.94.111.185` (with ID `cd17e8b1-3e4c-417b-9c80-a21a9879a8dc`). We will need this information later.

```bash
$ openstack floating ip list --port 2cc1b3f9-0457-410b-93dd-a96338de3b12
+--------------------------------------+------------------+---------------------+--------------------------------------+
| id                                   | fixed_ip_address | floating_ip_address | port_id                              |
+--------------------------------------+------------------+---------------------+--------------------------------------+
| cd17e8b1-3e4c-417b-9c80-a21a9879a8dc | 192.168.5.7      | 45.94.111.185       | 2cc1b3f9-0457-410b-93dd-a96338de3b12 |
+--------------------------------------+------------------+---------------------+--------------------------------------+
```

## Create an Octavia Loadbalancer

Once we have identified the Neutron Loadbalancers we need to migrate, the next step is to mirror the setup in Octavia. We can create a new Octavia Loadbalancer (Refer to [Step 24](/optimist/guided_tour/step24/) of our guide for instructions on setting up an Octavia Loadbalancer).

Once the new load balancer has been created, please ensure that you take the opportunity to check what the VIP port ID of the loadbalancer is with the following command, we will need it in the final step:

```bash
$ openstack loadbalancer show ExampleOctLB -f value -c vip_port_id
bde93be2-c0c1-5b32-02a3-e0c4300ff8c0
```

## Detach Floating IP from Neutron and reattach it to the Octavia Loadbalancer

Once the Octavia load balancer has been created, we are ready for the final step of the migration. In this step we will detach the floating IP from the Neutron port and reattach the Floating IP to the new port of the Octavia load balancer.

Firstly detach the Floating IP from the Neutron LB. The UUID of floating IP 45.94.111.185 in this case is `cd17e8b1-3e4c-417b-9c80-a21a9879a8dc`

```bash
$ openstack floating ip unset --port 2cc1b3f9-0457-410b-93dd-a96338de3b12 cd17e8b1-3e4c-417b-9c80-a21a9879a8dc
Disassociated floating IP cd17e8b1-3e4c-417b-9c80-a21a9879a8dc
```

Immediately after this, attach the same Floating IP we just removed from the Neutron LB port, to the Octavia LB port:

LB VIP Port: `bde93be2-c0c1-5b32-02a3-e0c4300ff8c0`

Floating IP: `cd17e8b1-3e4c-417b-9c80-a21a9879a8dc`

```bash
$ openstack floating ip set cd17e8b1-3e4c-417b-9c80-a21a9879a8dc --port bde93be2-c0c1-5b32-02a3-e0c4300ff8c0
```

If everything worked correctly, the new Octavia balancer is now active.
